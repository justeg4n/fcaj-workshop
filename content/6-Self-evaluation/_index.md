---
title: "Self-Assessment"
date: 2026-04-05
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

During my internship at **Bootcamp — First Cloud AI Journey @ AWS Study Group** from **January 5, 2026** to **April 5, 2026**, I had the opportunity to apply and deepen my knowledge in machine learning, cloud infrastructure, and software engineering within a real collaborative project environment.

I participated in **SmartHire AI** — an AI-powered technical recruitment platform built on AWS serverless infrastructure — where I served as the AI Engineer responsible for the entire intelligence layer of the system: from document processing and semantic vector scoring to bidirectional candidate-job matching and real-time LLM-generated explanations.

Throughout the project, I improved my skills in **Python Lambda development, prompt engineering with Amazon Bedrock, semantic search with pgvector, cross-encoder reranking, NLP-based PII masking, score calibration methodology, structured CloudWatch observability, and asynchronous AWS pipeline design**.

In terms of work ethic, I consistently prioritized precision over speed — understanding that incorrect AI scoring directly affects real hiring decisions. I engaged actively with the Backend and Frontend members to align data contracts and ensure the AI pipeline outputs were immediately usable by the rest of the system.

To objectively reflect on my internship, I evaluate myself based on the following criteria:

| No. | Criteria | Description | Good | Fair | Average |
| --- | --- | --- | --- | --- | --- |
| 1 | **Professional knowledge & skills** | Understanding of NLP, semantic search, cross-encoders, Bedrock API, pgvector, Lambda deployment, and Python pipeline design | ✅ | ☐ | ☐ |
| 2 | **Ability to learn** | Learning and applying new AWS services (Bedrock, Textract, AppSync, Step Functions) and ML concepts (calibrated sigmoid, asymmetric embedding) that were not previously studied in school | ☐ | ✅ | ☐ |
| 3 | **Proactiveness** | Identifying the root cause of score-explanation mismatch independently, proposing a 5-step calibration plan, and executing each step with verification before proceeding | ✅ | ☐ | ☐ |
| 4 | **Sense of responsibility** | All AI pipeline changes were tested after each step with the same CV baseline before advancing — never releasing untested score changes to the shared DynamoDB store | ✅ | ☐ | ☐ |
| 5 | **Discipline** | Followed the agreed step-by-step deploy-and-test process without skipping ahead, even when results were visible early | ☐ | ✅ | ☐ |
| 6 | **Progressive mindset** | Actively incorporated feedback from Backend members about API contract mismatches and from Frontend members about how scores were displayed, adjusting output schemas accordingly | ✅ | ☐ | ☐ |
| 7 | **Communication** | Writing detailed in-code documentation for every function (why calibrated sigmoid, why asymmetric Cohere input types, why top-K weighted aggregation) made the codebase self-explaining for team review | ☐ | ✅ | ☐ |
| 8 | **Teamwork** | Coordinated the `jd_vector` caching strategy with the Backend team so cv_jd_processor could reuse JD embeddings instead of recomputing them per candidate — a shared optimization benefiting both teams | ✅ | ☐ | ☐ |
| 9 | **Professional conduct** | Maintained objectivity when reviewing AI output quality — recognizing when scores were wrong due to technical issues rather than defending initial implementation choices | ✅ | ☐ | ☐ |
| 10 | **Problem-solving skills** | Diagnosed the L2-vs-cosine distance mismatch that caused Technical Lead bi-score to show 44% when the correct cosine score was ~70%, traced it to the SQL operator `<->` vs `<=>`, and proposed the surgical fix | ✅ | ☐ | ☐ |
| 11 | **Contribution to project/team** | The 5-step calibration plan and Phase 1A score-aware explanation synchronization were initiated and executed entirely by the AI Engineer role, directly resolving the most visible quality issue in the product | ✅ | ☐ | ☐ |
| 12 | **Overall** | Delivered a production-ready AI scoring pipeline with measurable improvement: spread between best and worst match increased from ~5 points to ≥30 points across the three test jobs | ✅ | ☐ | ☐ |

### Needs Improvement

- **Mathematical depth in ML systems**: While I could identify that the calibrated sigmoid was needed, I relied on heuristic shift/scale values rather than deriving them from empirical logit distribution analysis. Building the habit of measuring first and tuning second — rather than applying reasonable defaults — would make future calibrations more principled.

- **Cross-team API communication speed**: The `jd_vector` key naming inconsistency (`jdVector` in DynamoDB vs `jd_vector` in Python code) caused silent bugs in `ingestion_trigger.py` for two sprint cycles before being caught. Establishing a shared data contract document at the start of each feature would prevent this class of error entirely.

- **End-to-end latency ownership**: The AI pipeline's wall-clock time (Textract ~4s + Claude calls ~4s + Cohere embed ~1s = ~9s total per CV) was measured late in the project. Profiling each step with X-Ray from Week 3 onwards — rather than Week 11 — would have allowed architectural decisions (e.g., parallel Claude invocations) to be incorporated earlier.
