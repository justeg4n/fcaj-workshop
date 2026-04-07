---
title: "Worklog Tuần 8"
date: 2026-02-23
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu Tuần 8:
* Bắt đầu dự án **SmartHireAI** (24/02).
* Học Infrastructure as Code (IaC) qua Terraform.
* Phân tích ECS Fargate vs EKS cho Backend .NET.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Cài Terraform CLI.<br>- Khởi tạo `main.tf` tạo VPC và Security Group. | 02/23/2026 | 02/23/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.3-week3/ |
| 3 | - Kickoff SmartHireAI: Lên ý tưởng kiến trúc "Split-Brain" (State bằng .NET, AI bằng Python Lambda). | 02/24/2026 | 02/24/2026 | |
| 4 | - Chia module Terraform (`vpc.tf`, `variables.tf`).<br>- Chạy `terraform apply` thử nghiệm. | 02/25/2026 | 02/25/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.3-week3/ |
| 5 | - Phân tích ECS và EKS.<br>- Quyết định dùng ECS Fargate cho API .NET để giảm thiểu quản trị node. | 02/26/2026 | 02/26/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.3-week3/ |
| 6 | - Viết code Terraform cho Auto Scaling.<br>- Đẩy source code Terraform lên GitHub team. | 02/27/2026 | 02/27/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.3-week3/ |

### Thành tựu Tuần 8:
* **Ưu/Khuyết điểm:** Terraform giúp hạ tầng minh bạch, dễ deploy lại. ECS Fargate phù hợp hơn EKS vì team không có kỹ sư chuyên cứng K8s.
* **Khó khăn & Khắc phục:** Vô tình gõ cứng Access Key vào file `main.tf`. Mentor nhắc nhở. Đã cấu hình `aws configure` local để Terraform tự đọc credentials an toàn.
* **Kế hoạch:** Quản lý State Terraform và làm S3-Lambda Trigger cho việc upload CV.