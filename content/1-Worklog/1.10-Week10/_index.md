---
title: "Week 10 Worklog"
date: 2026-03-09
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Week 10 Objectives:
* Master debugging in highly decoupled Serverless architectures.
* Research and present findings on the AWS Well-Architected Framework.
* Provision infrastructure networks securely using Terraform.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Debugging Serverless: Trace requests across API Gateway, Python Lambda, and DynamoDB using AWS X-Ray. | 03/09/2026 | 03/09/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |
| 3 | - Analyze CloudWatch Logs for Lambda timeouts.<br>- Optimize Boto3 initialization outside the Lambda handler. | 03/10/2026 | 03/10/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |
| 4 | - Research AWS Well-Architected Framework (6 pillars).<br>- Apply the Security and Reliability pillars to the SmartHireAI VPC design. | 03/11/2026 | 03/11/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |
| 5 | - Use Terraform to deploy strict VPC subnets (public ALB, private ECS, isolated RDS). | 03/12/2026 | 03/12/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |
| 6 | - Configure VPC Endpoints for S3 and DynamoDB to keep traffic strictly within the AWS network. | 03/13/2026 | 03/13/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |

### Week 10 Achievements:
* **Pros/Cons:** AWS X-Ray provides incredible visibility into distributed systems. VPC Endpoints enhance security but add to the hourly cost.
* **Challenges & Solutions:** My Lambda was timing out when reading from DynamoDB inside the VPC. Realized Lambda in a private subnet needs a NAT Gateway or VPC Endpoint to access AWS APIs. Implemented a DynamoDB Gateway Endpoint to fix it securely and freely.
* **Next Steps:** System Architecture (Monolith vs Microservices) and SQL vs NoSQL.
