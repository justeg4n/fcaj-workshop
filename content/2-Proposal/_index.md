---
title: "Project Proposal"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Proposal
This section summarizes the planned workshop/project for **SmartHire-AI**, aligned with the infrastructure already defined using Terraform in this repository.

## SmartHire-AI
**Intelligent Hiring Platform on AWS (Serverless + IaC)**

### 1. Executive Summary
SmartHire-AI is designed for both candidates and recruiters. Candidates upload CVs (PDF) to object storage; the system extracts text, enriches data with AI (Amazon Bedrock, Amazon Textract, Amazon Comprehend), stores parsing status and outputs, and then generates either job suggestions or candidate rankings.

For the **JD (Job Description) flow**, content is read directly from the relational database (Amazon RDS PostgreSQL) by the **.NET 8** backend, which starts AWS Step Functions directly during job create/update events. This removes the dependency on uploading JD files to S3 for this flow.

The web client (SPA) is served via **Amazon CloudFront + Amazon S3**; authentication is handled by **Amazon Cognito** (including Google sign-in); and near real-time client updates are delivered through **AWS AppSync** (GraphQL subscriptions). The full environment is described as code using **Terraform** (VPC, RDS, Cognito, processing pipeline, X-Ray tracing, CI/CD, etc.).

### 2. Problem Statement
**What is the problem?**
Manual CV screening and JD matching do not scale as the number of applications and job postings grows. Teams often lack a unified pipeline for extraction, storage, and recommendation; users also need near real-time feedback instead of page refresh-based updates. External SaaS solutions are harder to customize and are often less integrated with internal RDS-based data.

**Proposed solution**
The solution uses a layered AWS architecture:
* CV ingestion via **S3** (prefix `candidates/`) with **S3 -> SQS** notifications limited to `.pdf` objects.
* `ingestion_trigger` Lambda consumes queue messages and calls `StartExecution` on Step Functions.
* State machine orchestrates `cv_jd_processor` (Python 3.12), then branches to `job_suggestion_engine` or `candidate_ranking_engine` (Lambda container images from **Amazon ECR**).
* Output is written to **DynamoDB** (single-table application tracking) and published through **AppSync**.
* Business API is provided by **API Gateway + Lambda .NET 8** in VPC, with RDS access via **AWS Secrets Manager** and JWT authentication through **Cognito**.
* Frontend is hosted on S3 behind CloudFront, with optional **AWS WAF** at the distribution layer.
* Repeatable delivery is handled by **CodePipeline / CodeBuild** with **CodeStar Connections** to GitHub.

This architecture is built for maintainability, predictable cost control, and repeatable deployment through IaC.

**Benefits and ROI**
SmartHire-AI reduces manual work for recruiters and candidates, standardizes CV/JD metadata, and provides a real-time foundation for dashboards. Operating costs are accurately estimated using **Infracost** via the `infracost.yml` file, which simulates and calculates costs for three environments (DEV, STAGING, PROD) by switching infrastructure variables (see Section 6).

Expected ROI is mostly operational: shorter processing time, lower UI response latency, and easier scale-out with IaC instead of manual service-by-service operations.

### 3. Solution Architecture
The platform is deployed in `ap-southeast-1` by default (per Terraform configuration), with a dedicated VPC (public/private subnets), private RDS PostgreSQL, and optional bastion/RDS Proxy. CV flow is S3 -> SQS -> Lambda -> Step Functions -> AI/vector processing (based on the unified internal pipeline design). JD flow is triggered by the .NET API, which reads `Jobs.Description` from RDS and calls Step Functions. Operational alerts (queue depth, Lambda errors, p95 duration) can be enabled via **Amazon CloudWatch** and **Amazon SNS**.

**CV/JD Flow Diagram**
![CV/JD Flow Diagram](D:\FPTU\AWS\architecture.png)
*Parallel CV and JD processing flow.*

**AWS Services Used**
* **Amazon VPC:** Virtual network, subnets, security groups; optional VPC interface endpoints (Secrets Manager, Cognito, Bedrock Runtime) and egress modes (`endpoint_only`, `shared_bastion_nat`, `nat_gateway`).
* **Amazon RDS (PostgreSQL):** Main relational datastore; credentials managed via **Secrets Manager + KMS**; optional **RDS Proxy** and bastion host.
* **Amazon Cognito:** User pool, app client, identity pool; `pre_sign_up` / `post_confirmation` Lambda triggers; Google federation.
* **Amazon S3:** Frontend bucket (OAC with CloudFront) and raw assets bucket (`candidates/`, `jobs/`) with versioning/lifecycle and SSE; S3 -> SQS notifications for CV PDFs.
* **Amazon SQS + DLQ:** CV parsing queue and dead-letter queue.
* **AWS Lambda:** `ingestion_trigger`, `cv_jd_processor` (ZIP), `job_suggestion_engine` / `candidate_ranking_engine` (ECR images), and Cognito-related functions.
* **Amazon ECR:** Container repositories for matching engines.
* **AWS Step Functions:** Unified CV/JD orchestration state machine.
* **Amazon DynamoDB:** Single-table application tracking (optional PITR/TTL depending on config).
* **AWS AppSync:** GraphQL API, DynamoDB datasource, resolvers for job suggestion and candidate ranking subscriptions.
* **Amazon Textract, Amazon Bedrock, Amazon Comprehend:** Text extraction, generative/embedding models, and entity/PII processing (per IAM policy).
* **Amazon API Gateway:** REST API, CORS, Cognito authorizer.
* **AWS X-Ray:** End-to-end tracing and observability across AppSync, Lambda, and RDS layers.
* **Amazon CloudFront + AWS WAF** (optional ARN): Frontend distribution and edge protection, with custom domain support.
* **Amazon Route 53 + ACM:** DNS and TLS certificates.
* **AWS CodePipeline, AWS CodeBuild, CodeStar Connections:** CI/CD, S3 artifacts, ECR build/push.
* **Amazon CloudWatch + Amazon SNS:** Logging and alarm notifications.
* **Terraform:** Primary Infrastructure as Code tool for the entire environment.

**Component Design**
* **Client:** SPA (React + TypeScript + Vite), Cognito authentication, API Gateway requests, AppSync subscriptions.
* **Edge delivery:** CloudFront -> S3 frontend; HTTPS and optional WAF at CDN edge.
* **API layer:** API Gateway + Lambda .NET (X-Ray enabled) in VPC, Cognito authorizer, integrated with RDS and Step Functions.
* **Ingest (CV):** Upload PDF to S3 `candidates/` -> SQS -> ingestion Lambda -> Step Functions.
* **Ingest (JD):** Backend writes JD text into RDS; business events call `StartExecution` (no S3 notification path for JD).
* **Processing:** Step Functions calls `cv_jd_processor`, routes to the matching engine, writes status to DynamoDB, and publishes via AppSync mutations.
* **Persistence:** RDS for relational data, DynamoDB for tracking/realtime contract, S3 for raw files.
* **Identity:** Cognito pool + Google federation; Lambda triggers for registration workflow.
* **CI/CD:** GitHub branch-based pipeline with container build/push and deployment automation.

### 4. Technical Implementation
**Implementation phases (recommended)**
1. **Design & IaC:** Finalize architecture flow and Terraform variables (`project_name`, `environment`, `aws_region`, `egress_mode`, `domain`).
2. **Network & data foundation:** `terraform apply` for networking/database modules; configure Secrets Manager; optional bastion/RDS Proxy.
3. **Auth & frontend hosting:** Deploy auth module (Cognito + Lambda triggers) and frontend stack (S3/CloudFront/Route 53/ACM).
4. **Processing pipeline:** Configure storage, queueing, S3->SQS notifications, and processing module (Lambda, Step Functions, DynamoDB, AppSync, alarms).
5. **API & Logic Configuration:** Deploy API Gateway and Lambda .NET via Terraform; validate Secrets Manager integration and IAM database permissions.
6. **CI/CD:** Connect GitHub via CodeStar Connections; build/push ECR and release pipeline.
7. **End-to-end testing:** Upload CV, create job, observe Step Functions/AppSync subscriptions, and verify CloudWatch telemetry.

**Technical requirements**
* **Terraform:** Secure state management; sensitive variables via `TF_VAR_` / `terraform.tfvars` (secrets not committed).
* **Python 3.12:** Lambda pipeline (handler/timeout/memory via `cv_parser_*` variables).
* **.NET 8:** Lambda API backend in VPC, IAM database auth for RDS if configured.
* **React + TypeScript + Vite:** Modern frontend SPA development, type-safe.
* **Docker:** Build images for `job_suggestion_engine` and `candidate_ranking_engine`, push to ECR.
* **AWS CLI:** Resource deployment and troubleshooting.
* **Working knowledge:** Cognito (OAuth/OIDC), AppSync GraphQL, Step Functions ASL, S3 notification + SQS policy patterns, and AWS X-Ray tracing.

### 5. Timeline & Milestones
* **Month 1 & 2 (Learning & Research):** Deep dive into AWS services, review architecture, validate quotas (Bedrock/Textract), and prepare AWS account/domain.
* **Month 3 (Implementation):** Build foundation (VPC, RDS, Cognito, S3/CF), complete serverless processing (Lambda, Step Functions, DynamoDB, AppSync), integrate CI/CD, and go-live.
* **Post go-live:** Cost tuning, DLQ/alarm operations, and backlog-driven feature expansion.

### 6. Budget Estimate
Below is a complete budget estimate for SmartHire-AI, extracted from an **Infracost** configuration (`infracost.yml`) that simulates three separate environments (DEV, STAGING, PROD). The estimate includes network services (WAF, CloudFront), fixed infrastructure (NAT, RDS, Proxy), and usage-based costs.

#### 6.1. Monthly Budget Summary
The table below summarizes fixed monthly costs by environment:

| Component | DEV (Extreme Cost Saving) | STAGING (Balanced) | PROD (HA/Security Standard) |
| :--- | :--- | :--- | :--- |
| RDS/NAT/Compute Core | $36.20 | $65.10 | $256.66 |
| RDS Proxy | $0.00 | $21.90 | $21.90 |
| AWS WAF (Firewall) | $0.00 | $0.00 | $6.00 |
| CloudFront (Free Tier) | $0.00 | $0.00 | $0.00 |
| **TOTAL FIXED COST** | **$36.20/month** | **$87.00/month** | **$284.56/month** |

#### 6.2. Environment Breakdown

**1. DEV Environment**
Configuration: 1 NAT Instance (t3.micro), 1 AZ, no Proxy/WAF.
**Total fixed monthly cost: $36.20/month**

| Service / Resource | Monthly Cost | Configuration Details |
| :--- | :--- | :--- |
| Amazon RDS | $23.20 | (db.t3.micro, 20 GB gp3) |
| NAT Instance | $10.60 | (t3.micro, 8 GB gp2) |
| Other (KMS, Alarms, Secret) | $2.40 | (1 key, 10 alarms, 1 secret) |
| **TOTAL** | **$36.20** | |

**2. STAGING Environment**
Configuration: 1 NAT Instance (t3.medium), 2 AZs, with RDS Proxy.
**Total fixed monthly cost: $87.00/month**

| Service / Resource | Monthly Cost | Configuration Details |
| :--- | :--- | :--- |
| NAT Instance | $39.50 | (t3.medium, 8 GB gp2) |
| Amazon RDS | $23.20 | (db.t3.micro, 20 GB gp3) |
| Amazon RDS Proxy | $21.90 | (2 vCPUs pooling) |
| Other (KMS, Alarms, Secret) | $2.40 | (1 key, 10 alarms, 1 secret) |
| **TOTAL** | **$87.00** | |

**3. PROD Environment**
Configuration: 2 NAT Gateways, full interface endpoints, 2 AZs, RDS Proxy, WAF.
**Total fixed monthly cost: $284.56/month**

| Service / Resource | Monthly Cost | Configuration Details |
| :--- | :--- | :--- |
| VPC Interface Endpoints | $113.88 | (6 service endpoints across 2 AZs) |
| NAT Gateway | $86.14 | (2 gateways - fixed hourly rental) |
| Amazon RDS | $43.64 | (db.t3.small, 20 GB gp3) |
| Amazon RDS Proxy | $21.90 | (2 vCPUs pooling) |
| Bastion Host | $10.60 | (t3.micro, 8 GB gp2) |
| AWS WAF | $6.00 | (1 Web ACL + common managed rules) |
| Other (KMS, Alarms, Secret) | $2.40 | (1 key, 10 alarms, 1 secret) |
| **TOTAL** | **$284.56** | |

#### 6.3. Usage-Based Cost (Estimate for 1,000 CVs/month)
The table below estimates incremental variable cost for processing **1,000 CVs** per month using advanced AI models (Claude 3.5 Sonnet and Cohere Embed v3):

| Service | Unit Pricing | Estimate for 1,000 CVs | Approx. Cost |
| :--- | :--- | :--- | :--- |
| Amazon Bedrock (Sonnet) | In: $3.00/M, Out: $15.00/M | ~2M tokens (Claude 3.5 Sonnet) | $18.00 |
| Amazon Bedrock (Cohere) | $0.10/1M tokens | ~1M tokens (Cohere Embed v3) | $0.10 |
| Amazon Textract | $1.50/1,000 pages | 1,000 CV pages (Detect Text) | $1.50 |
| AWS Lambda | $0.20/1M invocations | 10,000 calls (processing + auth/API) | $0.10 |
| AWS AppSync | $4.00/1M operations | 5,000 Queries/Mutations | $0.02 |
| AWS Cognito | Free Tier (50k MAUs) | < 1,000 active users | $0.00 |
| Other services (S3/DB/SQS) | Storage and messaging | File and state management | $0.17 |
| **TOTAL VARIABLE COST** | | **~$19.84 / 1,000 CVs** | |

**Notes:**
* **Claude 3.5 Sonnet** can deliver higher matching quality, but at noticeably higher inference cost than lighter alternatives.
* **Cohere Embed v3** remains highly cost-efficient for vector search workloads.
* Average variable cost per processed CV is around **$0.02**.
* Estimated first-month PROD operating cost (including 1,000 CVs): **~$304.40**
* Pricing assumptions are based on published AWS pricing in Singapore (ap-southeast-1) and Virginia (us-east-1) where model pricing differs.

### 7. Risk Assessment
**Key risks**
* AI service quota limits (Bedrock, Textract) during peak runs.
* Unreadable PDFs or low-quality Textract extraction output.
* SQS backlog growth and DLQ accumulation.
* Secret exposure risk (RDS, Cognito) from misconfigured access.
* Cost drift from NAT/data transfer and frequent AI model calls.

**Mitigation**
* CloudWatch alarms for queue depth, Lambda errors/throttles, and p95 duration, with SNS notifications.
* DLQ + retry policy and dead-letter analysis before replay.
* Secrets Manager + KMS encryption with least-privilege IAM and tight security groups.
* Budget alarms and periodic architecture/cost review.
* Configurable fallback to lower-cost AI models when required.

### 8. Expected Outcomes
**Technical improvements**
* A unified CV/JD pipeline orchestrated by Step Functions, reducing redundant Lambda steps and end-to-end latency.
* Realtime UI updates through AppSync subscriptions (`onJobSuggestions`, `onCandidateRanking`).
* Single-table DynamoDB tracking that scales with additional query patterns (GSI-based expansion).

**Long-term value**
* Repeatable, auditable infrastructure delivery through Terraform.
* Better-structured historical hiring data for analytics and future model improvements.
* Stronger operational observability through centralized logs, metrics, and alarms.
