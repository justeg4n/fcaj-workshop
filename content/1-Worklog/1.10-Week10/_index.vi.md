---
title: "Worklog Tuần 10"
date: 2026-03-09
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Mục tiêu Tuần 10:
* Thành thạo kỹ năng debug hệ thống Serverless phân tán.
* Nghiên cứu AWS Well-Architected Framework.
* Cấu hình an ninh mạng VPC nghiêm ngặt bằng Terraform.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Debugging: Dùng AWS X-Ray trace luồng dữ liệu từ API Gateway -> Lambda -> DynamoDB. | 03/09/2026 | 03/09/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |
| 3 | - Đọc CloudWatch Logs tìm lỗi Lambda timeout.<br>- Đưa code khởi tạo Boto3 ra ngoài handler để tối ưu Cold Start. | 03/10/2026 | 03/10/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |
| 4 | - Học 6 trụ cột của AWS Well-Architected.<br>- Áp dụng trụ cột Security vào kiến trúc SmartHireAI. | 03/11/2026 | 03/11/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |
| 5 | - Chạy Terraform cấp phát mạng: Public (ALB), Private (ECS), Isolated (RDS). | 03/12/2026 | 03/12/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |
| 6 | - Thiết lập VPC Endpoints cho S3 và DynamoDB để giao tiếp mạng nội bộ AWS. | 03/13/2026 | 03/13/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.6-week6/ |

### Thành tựu Tuần 10:
* **Ưu/Khuyết điểm:** X-Ray vẽ sơ đồ lỗi quá trực quan. Tối ưu code đưa Boto3 ra ngoài handler giúp Lambda chạy nhanh hơn hẳn.
* **Khó khăn & Khắc phục:** Khi cho Lambda vào Private Subnet, nó không truy cập được DynamoDB nữa. Giải pháp là cắm thêm VPC Endpoint (Gateway type) cho DynamoDB, vừa bảo mật vừa không tốn tiền NAT.
* **Kế hoạch:** Phân tích Microservices và chốt database cho hệ thống.