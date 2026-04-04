---
title: "Proposal"
date: 2026-01-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SmartHire AI — AI-Powered Technical Recruitment Platform
## A Serverless, Bidirectional Matching System on AWS for Modern Hiring

### 1. Executive Summary

SmartHire AI is an end-to-end AI-powered recruitment platform built for technical hiring teams. The system automates the most time-consuming parts of the recruitment funnel — CV screening, candidate-job matching, and interview question generation — using a multi-stage AI pipeline deployed entirely on AWS serverless infrastructure.

The platform implements **bidirectional matching**: when a candidate uploads a CV, the system automatically suggests the best-fit open jobs. When a recruiter posts a job description, the system instantly ranks the most suitable candidates from the existing talent pool. Both flows are driven by a hybrid AI scoring engine combining Cohere Embed v3 (semantic vector search) and a Cross-Encoder reranker (ms-marco-MiniLM-L-6-v2), with Amazon Bedrock Claude 3.5 Sonnet generating human-readable explanations, interview guides, and executive summaries.

The team consists of 5 members — 2 Backend (.NET 8 / AWS Lambda), 2 Frontend (ReactJS / WebSocket), and 1 AI Engineer (Python / Bedrock / pgvector) — operating on a fully serverless AWS stack with zero fixed server costs.

---

### 2. Problem Statement

#### What is the Problem?

Technical hiring is expensive and inconsistent. Recruiters at small-to-medium companies spend 3–5 hours per candidate on manual screening before a single interview occurs. Candidates receive no structured feedback. Existing platforms (HackerRank, Greenhouse, LinkedIn Recruiter) are either too expensive for small teams or provide no meaningful AI evaluation — they check whether code compiles, not whether a candidate genuinely understands what they built.

Three specific gaps exist in the market today:

1. **Asymmetric vocabulary**: CV language ("architected highly available clusters") and JD language ("requires AWS EC2 experience") mean the same thing but embed to different vector regions in naive systems. Simple cosine similarity fails here.
2. **No bidirectionality**: Most platforms match one CV to one JD on-demand. No system proactively suggests "you should look at this candidate" to a recruiter the moment a new JD is posted.
3. **Bias risk**: AI systems that process raw CV text can unconsciously weight names, universities, and nationalities. No mainstream recruiting tool addresses this with automated PII masking.

#### The Solution

SmartHire AI addresses all three gaps:

- A **hybrid scoring engine** (Bi-Encoder + Cross-Encoder) bridges vocabulary gaps with 65% weight on the Cross-Encoder, which processes the CV and JD simultaneously through transformer attention rather than comparing isolated embeddings.
- A **bidirectional pipeline** ensures that both a newly uploaded CV and a newly posted JD trigger automatic matching runs in opposite directions, stored immediately in DynamoDB and pushed to the relevant user via AppSync WebSocket.
- A **blind screening module** (spaCy NER) runs before any AI model sees the CV text, replacing PERSON, ORG, GPE, and NORP entities with neutral tokens, ensuring all scoring is based purely on technical merit.

#### Benefits and Return on Investment

| Metric | Current State | With SmartHire AI |
|---|---|---|
| Recruiter screening time per candidate | 3–5 hours | ~15 minutes (read AI summary) |
| CV-to-interview consistency | Varies by recruiter | Standardized STAR rubric score |
| Candidate feedback | None | Structured scorecard with skill gap analysis |
| Platform cost (operational) | $0 when idle | $0 when idle (100% serverless) |

The operational cost is approximately **$0.08 per CV processing session** (Bedrock Claude invocations + Cohere embeddings + Textract). There are no fixed server costs. The platform is built entirely with AWS Free Tier-eligible services for development and costs scale linearly only with actual usage in production.

---

### 3. Solution Architecture

The platform is split into two main flows sharing a unified AI processing Lambda:

**Candidate Flow**: Browser → Route 53 → CloudFront → S3 Frontend | Candidate uploads CV (presigned URL) → S3 → SQS → IngestionTrigger Lambda → Step Functions → `cv_jd_processor` Lambda → Routing Choice → `JobSuggestionEngine` Lambda → DynamoDB + AppSync push

**Recruiter Flow**: Browser → Route 53 → WAF → API Gateway → Cognito → Job Lambda (.NET) → Step Functions → `cv_jd_processor` Lambda → Routing Choice → `CandidateRankingEngine` Lambda → DynamoDB + AppSync push

Both flows converge in Step Functions and diverge at a Choice State based on `profile_id`.

![SmartHire AI Architecture — Candidate and Recruiter Flows](/images/2-Proposal/architecture_overview.png)

![SmartHire AI — AI Pipeline Detail (cv_jd_processor → Engines)](/images/2-Proposal/ai_pipeline_detail.png)

#### AWS Services Used

| Service | Role |
|---|---|
| **Amazon Route 53** | DNS for both `app.` and `api.` subdomains |
| **Amazon CloudFront + ACM** | CDN for React SPA, HTTPS certificate |
| **AWS WAF** | Attached to API Gateway — filters SQLi, XSS, rate-limits |
| **Amazon API Gateway** | REST + WebSocket endpoints |
| **Amazon Cognito** | JWT authentication for all API calls |
| **AWS Step Functions** | Orchestrates CV/JD processing pipeline |
| **AWS Lambda (.NET 8)** | Auth Service, Job Service, Stream Master |
| **AWS Lambda (Python)** | IngestionTrigger, cv_jd_processor, JobSuggestionEngine, CandidateRankingEngine |
| **Amazon SQS** | `cv-upload-queue` — decouples S3 events from Lambda |
| **Amazon S3** | CV Bucket (presigned uploads), Frontend hosting, Reports archive |
| **Amazon RDS PostgreSQL** | Source of truth — Users, Jobs, Applications, pgvector embeddings |
| **Amazon RDS Proxy** | Connection pooling for Lambda → RDS |
| **Amazon ElastiCache Redis** | Hot-read cache, session state |
| **Amazon DynamoDB** | Hot store — JOB_SUGGESTIONS, CANDIDATE_RANKING, real-time read |
| **Amazon Bedrock (Claude 3.5 Sonnet)** | Agent 1: skill extraction · Agent 2: interview guide · Agent 3: explanations |
| **Amazon Bedrock (Cohere Embed v3)** | 1024-dim embeddings — `search_document` for storage, `search_query` for ANN |
| **Amazon Textract** | Async PDF text extraction from CV files |
| **Amazon AppSync** | GraphQL + WebSocket push to both users |
| **Amazon SNS + SES** | Email notification when results are ready |
| **AWS NAT Gateway** | Private-subnet Lambda outbound to Bedrock, Textract, SNS, SES |
| **AWS VPC Endpoint** | DynamoDB access — traffic stays inside AWS backbone |
| **AWS Secrets Manager** | All API keys, RDS credentials — zero hardcoded secrets |
| **AWS KMS** | Encryption for S3 recordings and sensitive DynamoDB fields |
| **Amazon CloudWatch + X-Ray** | End-to-end tracing, Lambda metrics, custom dashboards |
| **AWS IAM** | Least-privilege roles for every Lambda and Step Functions state |

#### AI Pipeline Component Design

The core AI processing chain inside `cv_jd_processor` runs these steps in sequence, all via NAT Gateway to Bedrock:

1. **Amazon Textract** — async PDF text extraction
2. **spaCy NER (local, inside Lambda)** — PII masking before any AI model sees the text
3. **Claude 3.5 Sonnet — Agent 1** — structured skill extraction (JSON), `temperature=0.0`
4. **Claude 3.5 Sonnet — Agent 2** — interview guide generation from gaps, `temperature=0.3`
5. **Cohere Embed v3** — `search_document` embedding → stored in RDS pgvector
6. **Cross-Encoder ms-marco-MiniLM-L-6-v2 (local, baked into container)** — reranks top-N candidates/jobs
7. **Claude 3.5 Sonnet — Agent 3** — match explanation or candidate snapshot, `temperature=0.4`

---

### 4. Technical Implementation

#### Implementation Phases

The project follows 4 phases over 13 weeks:

**Phase 1 — Foundation (Weeks 1–2):**
AWS account setup, IAM roles, Cognito, CI/CD pipeline (GitHub Actions → SAM deploy), React + Tailwind scaffold, CloudFront + S3 hosting, API Gateway WebSocket test.

**Phase 2 — Core Pipeline (Weeks 3–6):**
Step Functions workflow definition, `cv_jd_processor` Lambda with Textract + Claude Agent 1, Cohere embed + RDS pgvector store, SQS → IngestionTrigger → Step Functions wiring, presigned URL upload flow.

**Phase 3 — AI Engines + Bidirectional Matching (Weeks 7–10):**
`JobSuggestionEngine` (ANN + Cross-Encoder + Claude Agent 3 for match explanations), `CandidateRankingEngine` (same for recruiter side), AppSync mutations, DynamoDB hot store writes, SNS/SES notifications.

**Phase 4 — Score Calibration, Testing, Demo (Weeks 11–13):**
5-step score calibration (L2→cosine fix, calibrated sigmoid, unified aggregation, larger cross-encoder input, hybrid weight tuning), score-aware explanation synchronization, replay tests with production data, demo scenario scripting (two candidate personas: strong vs weak), final integration test.

#### Week 12 — AI Engineer Deliverables (Current Week)

- **Step 1:** Fix bi-score formula — change SQL operator from `<->` (L2) to `<=>` (cosine distance) in both `JobSuggestionEngine` and `CandidateRankingEngine`. Update score conversion from linear L2 mapping to `(1 - cosine_distance) * 100`. Expected: Technical Lead biScore jumps from ~44 to ~68–75.
- **Step 2:** Replace raw `_sigmoid` with `_calibrated_sigmoid(shift=-1.0, scale=1.5)` in all three Lambdas. Expected: crossScore for relevant matches rises from ~38 to ~62–72.
- **Step 3:** Unify aggregation strategy — replace max pooling in `JobSuggestionEngine` with `_aggregate_chunk_scores([0.55, 0.30, 0.15])` matching the other two Lambdas.
- **Phase 1A:** Update `generate_match_explanation` and `generate_candidate_snapshot` to receive `final_score` and inject a tone directive into the Claude prompt (strong/moderate/weak tier mapping).

#### Week 13 — AI Engineer Deliverables (Final Week)

- **Step 4:** Increase cross-encoder input sizes via env vars: `CE_JD_QUERY_CHARS=2500`, `CE_CHUNK_CHARS=1200`, `CE_OVERLAP_CHARS=200`, `CE_MAX_CHUNKS=6`.
- **Step 5:** Tune hybrid weights based on observed Step 1–4 results. Adjust `SUGGESTION_WEIGHT_BI` and `SUGGESTION_WEIGHT_CROSS` in Lambda environment variables without code deploy.
- **Phase 1C:** Implement structured CloudWatch logging: log `biScore`, `crossScore`, `finalScore`, `threshold_pass`, `scorer_mode`, and `explanation_mode` per processing run.
- **Demo preparation:** Script two candidate personas for Demo Day — Persona A (strong, score ≥80) and Persona B (weak, score ≤42) — and verify the explanation tone is correctly calibrated for each.
- **Documentation:** Update `architecture-smart-matching.md` with final flow numbers, AI model call chains, and environment variable reference table.

#### Technical Requirements

**AI Engineer Stack:**
- Python 3.11, `sentence-transformers`, `spacy`, `pg8000`, `boto3`
- Container image deployment (Lambda) — cross-encoder baked at build time to `/opt/ml/cross_encoder`
- AWS Bedrock API (Claude 3.5 Sonnet APAC endpoint, Cohere Embed v3)
- pgvector with `vector_cosine_ops` index on `candidate_embeddings` and `job_embeddings`
- Step Functions Express Workflows, SQS DLQ, EventBridge routing

---

### 5. Timeline & Milestones

| Period | Milestone |
|---|---|
| Week 1–2 | Foundation: AWS account, CI/CD, frontend scaffold, API Gateway WebSocket |
| Week 3–4 | CV ingestion: S3 → SQS → IngestionTrigger → Step Functions → Textract |
| Week 5–6 | Claude Agent 1 + Cohere embed + pgvector storage |
| Week 7–8 | JobSuggestionEngine + CandidateRankingEngine (ANN + Cross-Encoder) |
| Week 9–10 | Claude Agent 3 + AppSync push + DynamoDB hot store |
| Week 11 | Integration testing, baseline score measurement |
| **Week 12** | **Score calibration Steps 1–3 + Phase 1A (score-aware explanations)** |
| **Week 13** | **Steps 4–5 + Phase 1C telemetry + demo preparation + documentation** |

---

### 6. Budget Estimation

All costs below are estimated for a development/demo environment (low traffic). Production costs scale linearly with usage — the system costs $0 when idle.

| Service | Cost/Month (Dev) | Notes |
|---|---|---|
| AWS Lambda | $0.00 | Free tier covers all dev invocations |
| Amazon S3 | $0.05 | CV bucket + reports archive |
| Amazon DynamoDB | $0.00 | On-demand, free tier |
| Amazon RDS PostgreSQL (Multi-AZ) | $0.00 | Dev instance — t3.micro free tier |
| Amazon Bedrock (Claude 3.5 Sonnet) | ~$0.30 | ~50 CV processing sessions/month |
| Amazon Bedrock (Cohere Embed v3) | ~$0.05 | ~200 embedding calls/month |
| Amazon Textract | ~$0.08 | ~50 CV files/month |
| AWS Step Functions | $0.00 | Free tier — 4,000 state transitions |
| Amazon SQS | $0.00 | Free tier |
| Amazon API Gateway | $0.01 | ~2,000 requests |
| Amazon CloudFront | $0.00 | Free tier |
| Amazon AppSync | $0.01 | ~2,000 connections |
| Amazon SNS + SES | $0.00 | Free tier |
| NAT Gateway | ~$0.05 | Dev — low data transfer |

**Total: ~$0.55/month (development) · ~$0.08/CV session (AI processing cost)**

Hardware: No additional hardware required. All processing is cloud-native.

---

### 7. Risk Assessment

#### Risk Matrix

| Risk | Impact | Probability | Mitigation |
|---|---|---|---|
| Bedrock throttling under load | High | Medium | Adaptive retry config (max 10 attempts), SQS as buffer |
| Cross-encoder cold start (Lambda) | Medium | High | Bake model into container image at build time — eliminates download on cold start |
| pgvector index type mismatch (L2 vs cosine) | High | Low | Verify index with `pg_indexes` query before Step 1 deploy; add migration SQL if needed |
| Score-explanation mismatch (low score, positive text) | Medium | High | Phase 1A: inject `final_score` + tone directive into every Claude Agent 3 prompt |
| Cost overrun from Bedrock | Low | Low | AWS Budget alerts at $5/month; feature flag `ENABLE_CROSS_ENCODER=false` to reduce calls |
| RDS connection exhaustion under Lambda concurrency | High | Medium | RDS Proxy handles pooling; read replicas for report queries |

#### Contingency Plans

- If Bedrock is unavailable: fall back to bi-encoder only (`cross_score=None` path already implemented — hybrid returns bi-score directly).
- If pgvector ANN returns no results: return empty suggestions with a user-facing "No matching results yet — check back after more candidates upload" message.
- Use AWS CloudFormation/SAM to restore any Lambda configuration from IaC in under 10 minutes.

---

### 8. Expected Outcomes

#### Technical Deliverables

- Fully deployed serverless matching pipeline with measurable score calibration: **Technical Lead finalScore ≥ 70%, Sustainability Intern finalScore ≤ 35%, spread ≥ 30 points** between best and worst match.
- Bidirectional real-time matching with AppSync WebSocket push — results delivered within 90 seconds of CV upload.
- Blind screening (spaCy NER) applied before any AI model sees CV text — zero PII in Bedrock prompts.
- Structured interview guide (3 questions per candidate) generated automatically and stored in DynamoDB.
- Full observability: X-Ray traces, CloudWatch custom dashboards, structured JSON logs per processing run.

#### Long-term Value

The architecture is reusable for any document-matching use case (legal contract review, academic paper matching, product specification alignment). The pgvector talent pool and bidirectional engine can serve as the foundation for a production-grade SaaS recruitment product. The scoring calibration methodology (5-step plan: cosine fix → calibrated sigmoid → unified aggregation → larger chunks → weight tuning) is documented and transferable to any future hybrid retrieval system built on Bedrock + pgvector.
