---
title: "Week 8 Worklog"
date: 2026-02-23
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Week 8 Objectives:
* Official Kickoff: **SmartHireAI Project** (Feb 24).
* Learn Infrastructure as Code (IaC) with Terraform.
* Research container orchestration (ECS Fargate vs EKS) for the .NET backend.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Install Terraform CLI.<br>- Write `main.tf` to provision basic VPC and Security Groups. | 02/23/2026 | 02/23/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.3-week3/ |
| 3 | - SmartHireAI Kickoff: System architecture design brainstorming.<br>- Determine split-brain approach (.NET backend + Python AI engines). | 02/24/2026 | 02/24/2026 | |
| 4 | - Modularize Terraform code (`vpc.tf`, `variables.tf`, `outputs.tf`).<br>- Use `terraform plan` and `terraform apply`. | 02/25/2026 | 02/25/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.3-week3/ |
| 5 | - Research Orchestration: ECS Fargate vs EKS.<br>- Decide on Fargate for the SmartHireAI .NET API to minimize node management. | 02/26/2026 | 02/26/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.3-week3/ |
| 6 | - Setup Auto Scaling policies via Terraform.<br>- Version control Terraform code in GitHub. | 02/27/2026 | 02/27/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.3-week3/ |

### Week 8 Achievements:
* **Pros/Cons:** Terraform makes infrastructure highly reproducible, but state conflicts happen in teams. Fargate is perfectly suited for our .NET API without the overhead of Kubernetes (EKS).
* **Challenges & Solutions:** Hardcoding AWS credentials in Terraform is a bad practice. I learned to use AWS Profiles locally and rely on IAM Roles in CI/CD pipelines.
* **Next Steps:** Terraform Remote State and Lambda triggers for SmartHireAI ingestion.