---
title: "Blog 1"
date: 2026-03-28
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

# Introducing OpenTelemetry & PromQL support in Amazon CloudWatch
*by Rodrigue Koffi on 03 APR 2026 in Amazon CloudWatch, Management Tools, Technical How-to*

If you run Kubernetes or microservices workloads on AWS, your metrics likely carry dozens of labels: namespace, pod, container, node, deployment, replica set, and custom business dimensions. To get a complete picture of your environment, you may be splitting your metrics pipeline: Amazon CloudWatch for AWS metrics, and a separate Prometheus-compatible backend for high-cardinality (many unique label combinations) container and application metrics. Some teams go further. They use Prometheus CloudWatch exporters to pull AWS resource metrics into their Prometheus backend through `GetMetricData` API calls. This adds operational overhead and cost but lets them query everything in one place.

Amazon CloudWatch now natively ingests **OpenTelemetry metrics** and supports querying them with **Prometheus Query Language (PromQL)**. This preview capability introduces a high-cardinality metrics store that supports up to 150 labels per metric, so you can send rich, label-dense metrics directly to CloudWatch without conversion or truncation. Combined with automatic AWS vended metric enrichment, CloudWatch becomes a single destination for infrastructure, container, and application metrics, all queryable with PromQL.

In this post, you will learn how to:
* Enable OpenTelemetry metrics ingestion and automatic AWS resource enrichment in your account.
* Deploy Amazon CloudWatch Container Insights on an Amazon Elastic Kubernetes Service (Amazon EKS) cluster.
* Query infrastructure and AWS resource metrics using PromQL in Amazon CloudWatch and Amazon Managed Grafana.
* Create custom application metrics in Amazon CloudWatch using the OpenTelemetry SDK and see them automatically enriched with AWS context.

## What OpenTelemetry support means for Amazon CloudWatch
The OpenTelemetry Protocol (OTLP) is the standard wire protocol for the OpenTelemetry (OTel) project. It defines how telemetry data, including metrics, traces, and logs, is encoded and transported between components. With this preview, CloudWatch exposes a regional OTLP endpoint that OpenTelemetry-compatible collectors or SDKs can send metrics to.

CloudWatch receives the metrics and stores them in a new high-cardinality metrics store, retaining OpenTelemetry metric types, including counters, histograms, gauges, and up-down counters, without conversion. With this launch, CloudWatch completes its support for OpenTelemetry across all three pillars of observability. CloudWatch already accepts traces and logs through its OTLP endpoints, adding native OTLP metrics ingestion means you can now send all your telemetry to CloudWatch using open standards, through a single protocol. Three capabilities make this significant:

1. **Extended label and cardinality support.** OTLP-ingested metrics support up to 150 labels, compared to the 30-dimension limit of CloudWatch custom metrics. This removes a key constraint for Kubernetes, microservice, and OpenTelemetry workloads that rely on high-cardinality labels for filtering and aggregation.
2. **PromQL query support.** You can query metrics ingested through OTLP using PromQL. If you already use Prometheus, you can use the same query language directly in CloudWatch and Amazon Managed Grafana, no new syntax to learn.
3. **Automatic AWS resource enrichment.** This capability fundamentally changes how you query and filter metrics across your AWS infrastructure. CloudWatch enriches every ingested metric with AWS resource context: account ID, Region, cluster ARN, and resource tags from AWS Resource Explorer. This enrichment happens automatically, without additional instrumentation. You can filter and group metrics by AWS account, Region, environment tag, or application name, whether they come from Container Insights, a custom application, or an AWS service. No exporters, no custom labels, no additional API calls.

## Enable OTLP ingestion and AWS resource enrichment
Before you can ingest and query OTLP metrics, enable two account-level settings. The first enables resource tags propagation from your AWS resources to your telemetry. The second enables OTLP ingestion for CloudWatch.

### Using the console
1. Open the Amazon CloudWatch console.
2. In the navigation pane, choose **Settings**.
3. Enable resource tags on telemetry.
4. Enable OTel enrichment for AWS metrics.

### Using the AWS CLI
Run the following commands:
```bash
# Enable resource tags on telemetry
aws observabilityadmin start-telemetry-enrichment

# Enable OTel enrichment for CloudWatch
aws cloudwatch start-o-tel-enrichment