---
title: "Week 9 Worklog"
date: 2026-03-02
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only.
{{% /notice %}}

### Week 9 Objectives:
* Transition SmartHireAI from local simulation to AWS Cloud infrastructure.
* Build the core Serverless components (S3, SQS, Lambda) for the CV parsing pipeline.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 57  | - Infrastructure Setup: Create `hirelo-cv-storage` S3 Bucket for candidate resumes. | 03/02/2026 | 03/02/2026 | AWS S3 Console |
| 58  | - Create SQS Queues to decouple the Backend .NET API from the Python AI Lambdas. | 03/03/2026 | 03/03/2026 | AWS SQS Docs |
| 59  | - IAM Configurations: Draft strict IAM policies allowing Lambda to access S3, SQS, Textract, and Bedrock. | 03/04/2026 | 03/04/2026 | AWS IAM Policy |
| 60  | - AWS Lambda setup: Create deployment packages and Lambda Layers for Boto3 updates. | 03/05/2026 | 03/05/2026 | AWS Lambda |
| 61  | - Code `cv_parser.py`: Integrate Amazon Textract to asynchronously read PDF text from S3. | 03/06/2026 | 03/06/2026 | Boto3 Textract |
| 62  | - Test Lambda integration with Bedrock (Claude 3.5 Sonnet) for JSON extraction of CV skills. | 03/07/2026 | 03/07/2026 | Anthropic Docs |
| 63  | - End of week review: Identify latency and throttling bottlenecks in the current architecture. | 03/08/2026 | 03/08/2026 | System Architecture |

### Week 9 Achievements:
* Successfully transitioned the AI logic from a local Python script into scalable AWS Lambda functions.
* Configured the necessary IAM roles and policies to ensure secure communication between SQS, Lambda, and Bedrock.
* Identified upcoming challenges with Bedrock throttling and Region availability.