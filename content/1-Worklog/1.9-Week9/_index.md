---
title: "Week 9 Worklog"
date: 2026-03-02
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Week 9 Objectives:
* Configure Terraform Remote State for team collaboration.
* Implement Serverless S3-Lambda triggers.
* Design DynamoDB schemas for the Serverless Bookstore lab and adapt concepts for SmartHireAI.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Setup S3 Backend for Terraform State.<br>- Add DynamoDB table for state locking to prevent concurrent apply issues. | 03/02/2026 | 03/02/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.4-week4/ |
| 3 | - Write Python Lambda function (`ingestion_trigger.py`) to process S3 `ObjectCreated` events. | 03/03/2026 | 03/03/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.5-week5/ |
| 4 | - Complete the Serverless Bookstore architecture lab.<br>- Use API Gateway to trigger Lambda. | 03/04/2026 | 03/04/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.5-week5/ |
| 5 | - Design DynamoDB Schema (Single-table design).<br>- Create `Partition Key` (PK) and `Sort Key` (SK) for flexible queries. | 03/05/2026 | 03/05/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.5-week5/ |
| 6 | - Adapt the Bookstore S3 processing concept to SmartHireAI (parsing CV PDFs asynchronously). | 03/06/2026 | 03/06/2026 | |

### Week 9 Achievements:
* **Pros/Cons:** Terraform S3 backend is essential for team collaboration. DynamoDB Single-Table design requires heavy upfront planning but scales infinitely.
* **Challenges & Solutions:** The Lambda function wasn't triggering upon S3 upload. Found out I needed to explicitly add the `aws_lambda_permission` in Terraform to allow S3 to invoke the Lambda function.
* **Next Steps:** Debugging serverless flows and AWS Well-Architected Framework review.
