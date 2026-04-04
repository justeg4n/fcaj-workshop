---
title: "Week 8 Worklog"
date: 2026-02-23
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only.
{{% /notice %}}

### Week 8 Objectives:
* Officially kick off the **SmartHireAI** project.
* Transition from learning foundational AWS ML services to building serverless AI architecture.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 50 | - Review Machine Learning with Amazon SageMaker & SageMaker Immersion Day concepts. | 02/23/2026 | 02/23/2026 | AWS SageMaker |
| 51 | - **SmartHireAI Kickoff (Feb 24):** Define system architecture. <br> - Decide on Serverless approach (Lambda, API Gateway, SQS). | 02/24/2026 | 02/24/2026 | Project Board |
| 52 | - Write `simulate_interview.py` for local testing. <br> - Integrate Boto3 with Amazon Bedrock (Claude 3.5 Sonnet). | 02/25/2026 | 02/25/2026 | Boto3 Docs |
| 53 | - Draft Prompt Engineering rules (Anti-Jailbreak, Code Awareness, Persona constraints). | 02/26/2026 | 02/26/2026 | Anthropic Guide |
| 54 | - Debug local simulator issues: Fix duplicated inputs and CLI UI loops. | 02/27/2026 | 02/27/2026 | Python Docs |
| 55 | - Explore Amazon Polly for Neural TTS integration (converting Claude's text to speech). | 02/28/2026 | 02/28/2026 | AWS Polly |
| 56 | - Consolidate local test scripts into a dedicated GitHub folder structure to separate from production Lambdas. | 03/01/2026 | 03/01/2026 | Git Best Practices |

### Week 8 Achievements:

* Officially started the SmartHireAI capstone project.
* Successfully built a local CLI simulator to test Claude 3.5 Sonnet's conversational abilities.
* Designed strict Prompt Engineering guardrails to prevent candidate jailbreaks during interviews.