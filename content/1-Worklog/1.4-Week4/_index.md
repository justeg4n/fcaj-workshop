---
title: "Week 4 Worklog"
date: 2026-01-26
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Week 4 Objectives:
* Manage domains and routing policies using Route 53.
* Transition from GUI to AWS CLI for operational speed.
* Understand NoSQL concepts and deploy Amazon DynamoDB.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Register a dummy domain in Route 53.<br>- Configure Simple and Weighted routing policies.<br>- Point A-records to ALB. | 01/26/2026 | 01/26/2026 | https://000010.awsstudygroup.com/ |
| 3 | - Install and configure AWS CLI locally.<br>- Use CLI to create S3 buckets, upload files, and list EC2 instances. | 01/27/2026 | 01/27/2026 | https://000011.awsstudygroup.com/ |
| 4 | - Write bash scripts using AWS CLI to automate EC2 start/stop.<br>- Explore advanced JMESPath queries for CLI output filtering. | 01/28/2026 | 01/28/2026 | https://000011.awsstudygroup.com/ |
| 5 | - Introduction to NoSQL. Create a DynamoDB table with Partition and Sort keys.<br>- Insert and query data directly from Console. | 01/29/2026 | 01/29/2026 | https://000060.awsstudygroup.com/ |
| 6 | - Use Python (Boto3) to interact with DynamoDB.<br>- Perform CRUD operations (PutItem, GetItem, UpdateItem, Scan). | 01/30/2026 | 01/30/2026 | https://000060.awsstudygroup.com/ |

### Week 4 Achievements:
* **Pros/Cons:** DynamoDB is blazingly fast and serverless, perfect for my Python projects. Route 53 propagation takes time. AWS CLI is intimidating at first but vastly superior to the console for repetitive tasks.
* **Challenges & Solutions:** I accidentally used a `Scan` operation on DynamoDB instead of `Query`, which is highly inefficient. I learned to properly structure the Primary Key so I could use targeted `Query` operations via Boto3.
* **Next Steps:** ElastiCache, CloudFront, and deep diving into Networking.