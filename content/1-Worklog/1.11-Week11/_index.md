---
title: "Week 11 Worklog"
date: 2026-03-16
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:
* Analyze Monolith vs Microservices for system architecture planning.
* Decide between SQL (RDS) vs NoSQL (DynamoDB) for SmartHireAI.
* Integrate AWS Labs Terraform MCP Server concepts.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - System planning: Monolithic vs Microservices breakdown.<br>- Decide on Event-Driven Microservices using EventBridge and SQS. | 03/16/2026 | 03/16/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.7-week7/ |
| 3 | - Database Analysis: RDS PostgreSQL vs DynamoDB.<br>- Concluded: RDS for user/job state, pgvector for AI embeddings, DynamoDB for real-time interview transcripts. | 03/17/2026 | 03/17/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.7-week7/ |
| 4 | - Refactor Terraform modules to support Amazon RDS with pgvector extension. | 03/18/2026 | 03/18/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.7-week7/ |
| 5 | - Study AWS Labs Terraform MCP Server integrations for automated IaC provisioning. | 03/19/2026 | 03/19/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.7-week7/ |
| 6 | - Draft AWS Step Functions state machine to orchestrate CV extraction pipeline. | 03/20/2026 | 03/20/2026 | |

### Week 11 Achievements:
* **Pros/Cons:** Using RDS with pgvector unifies our relational data and AI embeddings beautifully. Step functions orchestrate Lambda easily but JSON definitions are verbose.
* **Challenges & Solutions:** The debate over NoSQL vs SQL was tough. Realized we didn't have to choose one. Polyglot persistence allowed us to use RDS for profiles and DynamoDB for high-velocity streaming data (interview sessions).
* **Next Steps:** Finalize AI models (Claude, Cohere), AppSync websockets, and Git workflow resolution.
