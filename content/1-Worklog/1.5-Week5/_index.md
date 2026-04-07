---
title: "Week 5 Worklog"
date: 2026-02-02
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Week 5 Objectives:
* Implement in-memory caching with ElastiCache (Redis).
* Master advanced networking concepts via AWS Networking Workshop.
* Deliver content globally using Amazon CloudFront.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Deploy an ElastiCache Redis cluster.<br>- Integrate Redis cache into a simple Python app to reduce DB load. | 02/02/2026 | 02/02/2026 | https://000061.awsstudygroup.com/ |
| 3 | - Start Networking Workshop: VPC Peering setups.<br>- Route traffic between two distinct VPCs. | 02/03/2026 | 02/03/2026 | https://000092.awsstudygroup.com/ |
| 4 | - Networking Workshop: Transit Gateway overview.<br>- Configure NAT Gateways for private subnet internet access. | 02/04/2026 | 02/04/2026 | https://000092.awsstudygroup.com/ |
| 5 | - Setup CloudFront distribution pointing to an S3 origin.<br>- Configure Cache Behaviors and TTL policies. | 02/05/2026 | 02/05/2026 | https://000094.awsstudygroup.com/ |
| 6 | - Use Origin Access Control (OAC) to restrict S3 access strictly to CloudFront.<br>- Invalidate CloudFront cache after site updates. | 02/06/2026 | 02/06/2026 | https://000094.awsstudygroup.com/ |

### Week 5 Achievements:
* **Pros/Cons:** CloudFront vastly speeds up image/asset delivery. OAC is highly secure but tricky to configure. Redis integration makes data retrieval instantaneous.
* **Challenges & Solutions:** Could not access the internet from my private subnet EC2. Found out my Route Table was missing the 0.0.0.0/0 route pointing to the NAT Gateway. 
* **Next Steps:** Active Directory, Windows workloads, and Edge Computing.
