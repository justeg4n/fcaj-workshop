---
title: "Week 6 Worklog"
date: 2026-02-09
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Week 6 Objectives:
* Intercept and modify web requests at the edge via Lambda@Edge.
* Provision Windows Server workloads on EC2.
* Setup AWS Managed Microsoft AD for directory services.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Create a Lambda@Edge function.<br>- Intercept Viewer Request to modify HTTP headers dynamically. | 02/09/2026 | 02/09/2026 | https://000130.awsstudygroup.com/ |
| 3 | - Deploy Windows Server 2022 on EC2.<br>- Connect via RDP instead of SSH. | 02/10/2026 | 02/10/2026 | https://000093.awsstudygroup.com/ |
| 4 | - Install IIS web server on Windows EC2.<br>- Configure Security Groups for RDP and HTTP. | 02/11/2026 | 02/11/2026 | https://000093.awsstudygroup.com/ |
| 5 | - Provision AWS Managed Microsoft AD.<br>- Join the Windows EC2 instance to the Active Directory domain. | 02/12/2026 | 02/12/2026 | https://000095.awsstudygroup.com/ |
| 6 | - Configure Group Policies (GPO) for the EC2 instance.<br>- Review AD metrics and clean up resources. | 02/13/2026 | 02/13/2026 | https://000095.awsstudygroup.com/ |

### Week 6 Achievements:
* **Pros/Cons:** Lambda@Edge is brilliant for SEO routing and security headers. Windows instances consume more resources and boot slower than Amazon Linux.
* **Challenges & Solutions:** Lambda@Edge requires specific IAM execution roles and must be deployed in us-east-1. Took me an hour to figure out why my deployment failed in ap-southeast-1.
* **Next Steps:** Highly Available architecture patterns and SageMaker (Machine Learning)!