---
title: "Cloud Mastery #2"
date: 2026-04-04
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Event Report: Cloud Mastery #2

### Event Objectives

The **Cloud Mastery #2** workshop was designed to provide participants with a strong foundation in modern cloud engineering practices, focusing on three critical domains:

- Container orchestration with Kubernetes
- High-concurrency system design using Elixir
- Infrastructure automation using Infrastructure as Code (IaC)

The event aimed to bridge the gap between theoretical knowledge and practical implementation, helping participants understand how modern cloud systems are built, deployed, and operated efficiently at scale.

---

### Key Highlights

#### 1. Kubernetes – Architecting for the Cloud

The first session introduced **Kubernetes (K8s)** as a core platform for container orchestration in modern cloud-native systems.

Key topics included:

- **Container Orchestration Challenges**
  - Managing containers manually becomes infeasible at scale
  - Need for automation in deployment, scaling, and recovery

- **Kubernetes Architecture**
  - Control Plane: API Server, Scheduler, Controller Manager, etcd
  - Worker Nodes: kubelet, kube-proxy, container runtime
  - Cluster-based system design

- **Core Concepts**
  - **Pods**: Smallest deployable unit
  - **ReplicaSets**: Maintain desired number of Pods (self-healing)
  - **Deployments**: Manage application lifecycle (rolling updates, rollback)
  - **ConfigMap & Secret**: Manage configuration and sensitive data
  - **Jobs**: Handle batch or one-time workloads

- **Key Capabilities**
  - Self-healing systems
  - Automatic scaling
  - Rolling updates without downtime

- **EKS (Elastic Kubernetes Service)**
  - Simplifies Kubernetes deployment on AWS
  - Reduces operational overhead
  - Integrates seamlessly with AWS ecosystem

- **Supporting Tools**
  - Helm for package management
  - K9s for cluster monitoring
  - Minikube/K3s/K3d for local development

This session provided a strong foundation for understanding how large-scale distributed systems are orchestrated in production.

---

#### 2. Elixir in DevOps – High Concurrency & Fault Tolerance

The second session explored **Elixir**, a functional programming language built on the BEAM virtual machine, and its role in DevOps systems.

Key insights included:

- **Functional Programming Paradigm**
  - Immutable data
  - Function-based design
  - Pattern matching

- **BEAM Architecture**
  - Lightweight processes (millions possible)
  - No shared memory between processes
  - Efficient scheduling

- **Concurrency Model**
  - Massive concurrency with minimal overhead
  - Ideal for real-time systems and distributed workloads

- **Fault Tolerance (OTP Model)**
  - Supervisor–Worker pattern
  - “Let It Crash” philosophy
  - Automatic process recovery

- **Hot Code Upgrade**
  - Update systems without downtime
  - Critical for high-availability systems

- **Real-World Use Case**
  - Migration from serverless (AWS Lambda) to Elixir significantly reduced cost
  - Demonstrates efficiency in handling high-throughput workloads

- **DevOps Integration**
  - Unified toolchain for development, deployment, and monitoring
  - Suitable for building resilient backend systems

This session highlighted how system reliability and scalability can be achieved through language and runtime design—not just infrastructure.

![CloudFormation template session at Cloud Mastery #2](https://justeg4n.github.io/fcaj-workshop/images/4-EventParticipated/4.4-Event4/cloudformation-template.png)

---

#### 3. Infrastructure as Code (IaC) – Automating the Cloud

The final session focused on **Infrastructure as Code**, a fundamental practice in modern DevOps.

Key concepts included:

- **What is IaC**
  - Managing infrastructure using code instead of manual configuration
  - Enables automation, reproducibility, and scalability

- **Limitations of ClickOps**
  - Human error
  - Inconsistency across environments
  - Lack of collaboration

- **AWS CloudFormation**
  - Template-based infrastructure definition (YAML/JSON)
  - Stack-based deployment model
  - Drift detection for configuration consistency

- **AWS CDK**
  - Define infrastructure using programming languages
  - Multi-level constructs:
    - L1 (low-level)
    - L2 (best practices)
    - L3 (high-level patterns)

- **Terraform**
  - Multi-cloud support
  - State management (terraform.tfstate)
  - Structured workflow:
    - init → validate → plan → apply → destroy

- **Key Benefits**
  - Automation
  - Reproducibility
  - Version control for infrastructure
  - Improved collaboration between teams

This session provided practical knowledge for building scalable and maintainable cloud infrastructure.

![How AWS CloudFormation works](https://justeg4n.github.io/fcaj-workshop/images/4-EventParticipated/4.4-Event4/cloudformation-workflow.jpg)

---

### What I Learned

Through this event, I gained a comprehensive understanding of modern cloud engineering:

- How to design scalable systems using Kubernetes
- How programming models (Elixir/BEAM) influence system reliability
- How to automate infrastructure using IaC tools

---

#### Lessons Learned

- Modern systems must be **declarative, automated, and resilient**
- Infrastructure and application logic must be designed together
- Choosing the right tool (K8s, Elixir, Terraform) depends on system requirements
- Observability and maintainability are as important as performance


---

> Overall, the event not only provided technical knowledge but also reshaped my mindset about system design, cloud modernization, and effective collaboration in engineering teams.
