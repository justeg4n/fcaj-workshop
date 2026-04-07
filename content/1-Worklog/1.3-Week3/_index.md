---
title: "Week 3 Worklog"
date: 2026-01-19
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
### Week 3 Objectives:
* Compare Lightsail with EC2 and deploy a simplified app.
* Containerize an application and deploy via Lightsail Containers.
* Implement High Availability using EC2 Auto Scaling.
* Set up system monitoring and alarms with Amazon CloudWatch.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Deploy a WordPress site using Amazon Lightsail.<br>- Assign static IP to the Lightsail instance. | 01/19/2026 | 01/19/2026 | https://000045.awsstudygroup.com/ |
| 3 | - Build a basic Docker image.<br>- Push and run the container using Lightsail Containers.<br>- Map ports for external access. | 01/20/2026 | 01/20/2026 | https://000046.awsstudygroup.com/ |
| 4 | - Create an EC2 Launch Template with User Data.<br>- Configure an Auto Scaling Group (ASG) across 2 Availability Zones.<br>- Attach an Application Load Balancer (ALB). | 01/21/2026 | 01/21/2026 | https://000006.awsstudygroup.com/ |
| 5 | - Terminate an instance manually to watch ASG automatically replace it.<br>- Configure CloudWatch dashboard. | 01/22/2026 | 01/22/2026 | https://000006.awsstudygroup.com/<br>https://000008.awsstudygroup.com/ |
| 6 | - Create a CloudWatch Alarm triggered by CPU Utilization > 70%.<br>- Stress test EC2 to trigger the alarm.<br>- Configure SNS to email me upon alarm state. | 01/23/2026 | 01/23/2026 | https://000008.awsstudygroup.com/ |

### Week 3 Achievements:
* **Pros/Cons:** Lightsail is incredibly fast for standard setups but lacks granular control compared to EC2. Auto Scaling is powerful but requires ALB for full effectiveness.
* **Challenges & Solutions:** The ASG instances kept failing health checks. I realized the ALB target group was pinging `/index.html` but my User Data script failed to start Apache. Fixed the bash script syntax.
* **Next Steps:** Dive into DNS management (Route 53) and NoSQL (DynamoDB).