---
title: "Week 10 Worklog"
date: 2026-03-09
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only.
{{% /notice %}}

### Week 10 Objectives:
* Resolve AWS Bedrock cross-region access issues.
* Implement Event-Driven Architecture integrating S3, SQS, Textract, and Bedrock for the Pre-Interview CV Parsing stage.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 64 | - Debug Bedrock errors (`ThrottlingException`). Implement `Config(mode='adaptive')` and exponential backoff in Boto3. | 03/09/2026 | 03/09/2026 | Boto3 Retry Config |
| 65 | - Debug Model Deprecation (`Legacy` error) and Marketplace subscription blocks. Work with Root User (BE1) to accept EULA for Claude 3.5 Sonnet v2. | 03/10/2026 | 03/10/2026 | AWS IAM / Bedrock |
| 66 | - Implement **Cross-Region Inference Profiles**. Update `MODEL_ID` to use APAC ARNs to route requests from Singapore to the US seamlessly. | 03/11/2026 | 03/11/2026 | AWS Cross-Region |
| 67 | - Develop `cv_parser.py` Lambda. Integrate Amazon Textract with pagination (`NextToken`) to handle multi-page CV PDFs. | 03/12/2026 | 03/12/2026 | AWS Textract |
| 68 | - Upgrade CV Parser to Unified AI Call: Evaluate CV against Job Description (JD) in a single Bedrock execution to save costs. | 03/13/2026 | 03/13/2026 | System Architecture |
| 69 | - Configure SQS payload processing. Design the JSON schema for the Backend to pass `profileId`, `fileKey`, and `jdText` to the AI Lambda. | 03/14/2026 | 03/14/2026 | AWS SQS |
| 70 | - End of Week 10 Review: Integrate Lambda with Backend .NET API using `urllib` to post parsed JSON results. | 03/15/2026 | 03/15/2026 | Serverless Integration |

### Week 10 Achievements:

* Conquered complex AWS Bedrock configuration issues, specifically resolving Marketplace EULA blocks and configuring Cross-Region ARNs for `ap-southeast-1`.
* Successfully built a production-ready Event-Driven pipeline: SQS -> Lambda -> Textract -> Bedrock -> .NET API.
* Optimized AI costs by combining CV parsing and JD matching into a single LLM prompt execution.