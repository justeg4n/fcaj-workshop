---
title: "Blog 2"
date: 2026-03-31
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

# Announcing General Availability of AWS DevOps Agent
*by Madhu Balaji on 31 MAR 2026 in Announcements, DevOps, Launch, News*

Today, we’re announcing the general availability of **AWS DevOps Agent**. AWS DevOps Agent is your always-available operations teammate. It resolves and proactively prevents incidents, optimizes application reliability and performance, and handles on-demand SRE tasks across AWS, multicloud, and on-premises environments.

Operations teams spend countless hours investigating incidents, correlating data across multiple tools, and manually triaging alerts. This operational toil takes engineers away from innovation and strategic work. AWS DevOps Agent eliminates this burden by investigating incidents as an experienced DevOps engineer would. It learns your applications and their relationships, works with your observability tools, runbooks, code repositories, and CI/CD pipelines, and correlates telemetry, code, and deployment data across all of them. In preview, customers and partners using AWS DevOps Agent report up to 75% lower MTTR, 80% faster investigations, and 94% root cause accuracy, enabling 3–5x faster incident resolution.

Since the preview launch, organizations have connected it with Amazon CloudWatch, and partner tools like Datadog, Dynatrace, New Relic, Splunk, GitHub, GitLab, ServiceNow, and Slack. We’re excited to add built-in support for Azure, Azure DevOps, PagerDuty, Grafana, and additional integrations as part of this GA launch.

## How AWS DevOps Agent Works
AWS DevOps Agent represents a new class of frontier agents—autonomous systems that work independently to achieve goals, scale massively to tackle concurrent tasks, and run persistently without constant human oversight. 

* **Autonomous incident response**: It begins investigating the moment an alert comes in, whether at 2AM or during peak hours. This reduces mean time to resolution (MTTR) and quickly restores your application to optimal performance.
* **Proactive incident prevention**: It moves teams from reactive firefighting to proactive operational improvement. It analyzes patterns across historical incidents to deliver targeted recommendations.
* **On-demand SRE task handling**: It leverages its deep understanding of your environment. You can create, save, and share custom charts and reports using a conversational AI assistant.

## What’s New in General Availability?
The GA release expands capabilities based on customer feedback.

### Expanded Use Cases
* **Azure Support**: Investigates incidents in Azure workloads, correlating data across multicloud deployments.
* **On-Premises Support**: Extends investigation using the Model Context Protocol (MCP) to discover resources and build topologies across AWS, Azure, and on-premises.
* **On-Demand SRE Tasks**: Use the conversational AI assistant to query application architectures and analyze system health.
* **Triage Agent**: Automatically assesses incident severity and links duplicate tickets to reduce noise.

### Enhanced Intelligence
* **Learned Skills**: Learns from investigation patterns, tool use, and topology.
* **Custom Skills**: Add organization-specific investigation procedures. Workflows can target specific agent types (Triage, RCA, Mitigation) to reduce context consumption.
* **Code Indexing**: Indexes application code repositories to suggest code-level fixes as part of mitigation plans.

### New Integrations
Building on existing integrations, we are adding:
* **PagerDuty**: Native integration for automatic incident response.
* **Grafana**: Built-in Grafana MCP server connects to Prometheus, Loki, OpenSearch, etc.
* **Azure DevOps**: Integration with Azure Pipelines.
* **Amazon EventBridge**: Custom automation workflows.

### Enterprise-Ready Capabilities
* **Regional Expansion**: Global reach across six AWS Regions.
* **Private Connections**: Securely connect the Agent Space to services in your VPC without exposing them to the public internet.
* **Security**: Supports customer managed keys and direct identity provider (IdP) integration with Okta and Microsoft Entra ID.
* **Localization**: Responds to browser locale settings, translating agent responses globally.

## Getting Started
AWS DevOps Agent is available today. Start by creating an Agent Space in the AWS Management Console, connect your observability tools, and run an investigation. With AWS DevOps Agent, you pay for the time the agent spends on operational tasks, billed per second. There are no upfront commitments.