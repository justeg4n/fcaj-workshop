---
title: "Week 2 Worklog"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:
* Design and implement a custom Virtual Private Cloud (VPC).
* Setup cloud-based IDE using AWS Cloud9.
* Host a static website on Amazon S3.
* Provision and connect to a managed Relational Database Service (RDS).

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn VPC concepts (Subnets, IGW, Route Tables).<br>- Build a custom VPC with 1 public and 1 private subnet.<br>- Test routing rules. | 01/12/2026 | 01/12/2026 | https://000003.awsstudygroup.com/ |
| 3 | - Provision AWS Cloud9 environment.<br>- Clone repositories and set up Python/Node JS dependencies.<br>- Write basic test scripts in Cloud9. | 01/13/2026 | 01/13/2026 | https://000049.awsstudygroup.com/ |
| 4 | - Create an S3 Bucket and enable static website hosting.<br>- Write an `index.html` file.<br>- Update Bucket Policies to allow public read access. | 01/14/2026 | 01/14/2026 | https://000057.awsstudygroup.com/ |
| 5 | - Launch an Amazon RDS MySQL instance (Free Tier).<br>- Place RDS in the private subnet for security.<br>- Configure Security Group to allow port 3306. | 01/15/2026 | 01/15/2026 | https://000005.awsstudygroup.com/ |
| 6 | - Connect to RDS from Cloud9 (public subnet).<br>- Execute basic SQL queries to create tables.<br>- Clean up RDS snapshots and instances. | 01/16/2026 | 01/16/2026 | https://000005.awsstudygroup.com/ |

### Week 2 Achievements:
* **Pros/Cons:** VPC architecture is visually intuitive but complex under the hood. Pro: Cloud9 eliminates local environment issues. Con: RDS takes 5-10 minutes to provision.
* **Challenges & Solutions:** The S3 static site returned a 403 Forbidden error. I had to uncheck "Block all public access" and write a JSON bucket policy allowing `s3:GetObject`.
* **Next Steps:** Focus on alternative compute solutions like Lightsail and explore Auto Scaling.
