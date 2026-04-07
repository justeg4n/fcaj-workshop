---
title: "Week 7 Worklog"
date: 2026-02-16
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Week 7 Objectives:
* Consolidate networking/compute into a Highly Available (HA) architecture.
* Introduction to AWS AI/ML services and Amazon SageMaker.
* Run a SageMaker Notebook for data exploration.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Architect a multi-AZ web app: ALB -> ASG (Private) -> Multi-AZ RDS.<br>- Test failover mechanics. | 02/16/2026 | 02/16/2026 | https://000101.awsstudygroup.com/ |
| 3 | - Overview of AI/ML stack: Rekognition, Comprehend, Transcribe.<br>- Setup SageMaker Studio domain. | 02/17/2026 | 02/17/2026 | https://cloudjourney.awsstudygroup.com/7-aimlservice/ |
| 4 | - Launch a Jupyter Notebook instance in SageMaker.<br>- Load dataset from S3 into Pandas dataframe. | 02/18/2026 | 02/18/2026 | https://000200.awsstudygroup.com/ |
| 5 | - SageMaker Immersion Day: Train an XGBoost model.<br>- Deploy model to a real-time endpoint. | 02/19/2026 | 02/19/2026 | https://000200.awsstudygroup.com/ |
| 6 | - Invoke endpoint using Boto3.<br>- Clean up endpoints to avoid heavy GPU billing! | 02/20/2026 | 02/20/2026 | https://000200.awsstudygroup.com/ |

### Week 7 Achievements:
* **Pros/Cons:** SageMaker abstracts away infrastructure, letting me focus on data science (Python/R). However, endpoints can be expensive if left running.
* **Challenges & Solutions:** Encountered IAM permission issues when SageMaker tried to read my S3 bucket. Attached the `AmazonS3ReadOnlyAccess` policy to the SageMaker execution role.
* **Next Steps:** Kick off the internal project (SmartHireAI) and start using Terraform.
