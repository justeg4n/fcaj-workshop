---
title: "Cloud Mastery 2026"
date: 2026-03-14
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Bài thu hoạch: Cloud Mastery 2026

### Mục Đích Của Sự Kiện

Sự kiện **Cloud Mastery 2026** được tổ chức nhằm trang bị cho người tham gia nền tảng vững chắc về các kỹ thuật Cloud hiện đại, tập trung vào ba chủ đề chính:

- Điều phối container với Kubernetes
- Xây dựng hệ thống chịu tải cao bằng Elixir
- Tự động hóa hạ tầng với Infrastructure as Code (IaC)

Mục tiêu của sự kiện là giúp người tham gia hiểu rõ cách các hệ thống cloud hiện đại được thiết kế, triển khai và vận hành một cách hiệu quả ở quy mô lớn.

---

### Nội Dung Nổi Bật

#### 1. Kubernetes – Nền tảng điều phối hệ thống hiện đại

Phần đầu tiên giới thiệu về **Kubernetes (K8s)** – nền tảng cốt lõi trong việc vận hành hệ thống cloud-native.

Các nội dung chính:

- **Thách thức khi quản lý container**
  - Không thể quản lý thủ công khi hệ thống lớn
  - Cần tự động hóa deployment, scaling và recovery

- **Kiến trúc Kubernetes**
  - Control Plane: API Server, Scheduler, Controller Manager, etcd
  - Worker Node: kubelet, kube-proxy
  - Hệ thống cluster

- **Các thành phần chính**
  - Pod: đơn vị nhỏ nhất
  - ReplicaSet: đảm bảo số lượng Pod
  - Deployment: quản lý vòng đời ứng dụng
  - ConfigMap & Secret: quản lý cấu hình
  - Job: xử lý tác vụ batch

- **Tính năng nổi bật**
  - Self-healing (tự phục hồi)
  - Auto-scaling (tự mở rộng)
  - Rolling update không downtime

- **EKS (AWS)**
  - Giúp triển khai Kubernetes dễ dàng hơn trên cloud
  - Giảm công sức vận hành

- **Công cụ hỗ trợ**
  - Helm, K9s
  - Minikube, K3s, K3d

---

#### 2. Elixir in DevOps – Hệ thống chịu lỗi và đồng thời cao

Phần thứ hai tập trung vào **Elixir** và BEAM VM.

Nội dung chính:

- **Lập trình hàm**
  - Immutable data
  - Pattern matching

- **BEAM Process**
  - Hàng triệu process nhẹ
  - Không share memory

- **Concurrency**
  - Xử lý đồng thời hiệu quả
  - Phù hợp hệ thống realtime

- **Fault Tolerance**
  - Supervisor–Worker model
  - “Let it crash”

- **Hot Code Upgrade**
  - Update không downtime

- **Case study thực tế**
  - Giảm chi phí từ serverless sang Elixir

- **Ứng dụng DevOps**
  - Unified toolchain
  - Phù hợp hệ thống backend lớn

---

#### 3. Infrastructure as Code – Tự động hóa hạ tầng

Phần cuối nói về **IaC**.

- **Khái niệm**
  - Quản lý hạ tầng bằng code

- **Nhược điểm ClickOps**
  - Lỗi con người
  - Không đồng nhất

- **CloudFormation**
  - Template YAML/JSON
  - Stack deployment
  - Drift detection

- **AWS CDK**
  - Viết infra bằng code
  - L1, L2, L3 constructs

- **Terraform**
  - Multi-cloud
  - State management
  - Workflow rõ ràng

- **Lợi ích**
  - Automation
  - Reproducibility
  - Collaboration

---

### Những Gì Học Được

- Hiểu rõ cách vận hành hệ thống lớn bằng Kubernetes  
- Nhận thức được vai trò của runtime (BEAM) trong độ ổn định hệ thống  
- Biết cách triển khai hạ tầng bằng code với IaC  

---

#### Bài học rút ra

- Hệ thống hiện đại cần **tự động hóa và resilient**
- Dev và Ops cần gắn kết chặt chẽ
- Lựa chọn công nghệ phụ thuộc bài toán

---


> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp tôi thay đổi cách tư duy về thiết kế hệ thống, hiện đại hóa ứng dụng và làm việc hiệu quả hơn trong môi trường Cloud.