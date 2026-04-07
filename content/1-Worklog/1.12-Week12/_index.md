---
title: "Week 12 Worklog"
date: 2026-03-23
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Week 12 Objectives:
* Finalize the integration of multi-agent Claude and AppSync architecture.
* Implement the L2 distance vector search (`job_suggestion_engine.py`).
* Resolve Git tracking issues and finalize production deployment.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Code `vector_ops.py` to call Amazon Bedrock (Cohere) for vector embeddings.<br>- Insert vectors into RDS pgvector. | 03/23/2026 | 03/23/2026 | |
| 3 | - Build `candidate_ranking_engine.py` (Cross-Encoder) using Claude 3 via Bedrock.<br>- Merge code on GitHub to `ai-core` branch. | 03/24/2026 | 03/24/2026 | |
| 4 | - Write GraphQL schema in AWS AppSync.<br>- Configure AppSync to notify React frontend via WebSockets upon AI task completion. | 03/25/2026 | 03/25/2026 | |
| 5 | - Resolve local Git tracking issues (`git add iac/lambda/* -f`).<br>- Push the updated `job_suggestion_engine.py` to origin. | 03/26/2026 | 03/26/2026 | |
| 6 | - End-to-end testing of the Interview Flow (API Gateway -> .NET Stream Master -> AI Orchestrator -> AppSync).<br>- Wrap up internship documentation. | 03/27/2026 | 03/27/2026 | |

### Week 12 Achievements:
* **Pros/Cons:** The Split-Brain architecture completely isolates heavy AI processing from the fast .NET CRUD API. AppSync WebSockets deliver an incredible real-time UI experience.
* **Challenges & Solutions:** Pushing empty lambda folders to Git caused 'nothing to commit' errors. Solved by cleaning `.gitignore`, copying the Python files correctly, and using `git switch` and force add to ensure tracking. 
* **Final Thoughts:** The 12 weeks transformed my theoretical AI knowledge from FPT University into practical Cloud Engineering skills. I can now confidently build, deploy, and monitor scalable AI-driven microservices on AWS.
