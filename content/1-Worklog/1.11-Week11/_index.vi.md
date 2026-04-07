---
title: "Worklog Tuần 11"
date: 2026-03-16
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Mục tiêu Tuần 11:
* Phân tích Monolith vs Microservices.
* Lựa chọn giữa SQL (RDS) và NoSQL (DynamoDB).
* Tìm hiểu AWS Labs Terraform MCP Server.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Lên kiến trúc: Chia tách hệ thống theo hướng Event-Driven Microservices với SQS và EventBridge. | 03/16/2026 | 03/16/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.7-week7/ |
| 3 | - Phân tích Database.<br>- Chốt phương án: Dùng RDS PostgreSQL (chứa pgvector) cho AI, DynamoDB lưu lịch sử hội thoại interview. | 03/17/2026 | 03/17/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.7-week7/ |
| 4 | - Viết lại code Terraform cấp phát RDS có kích hoạt sẵn extension pgvector. | 03/18/2026 | 03/18/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.7-week7/ |
| 5 | - Nghiên cứu tích hợp MCP Server hỗ trợ Terraform tự động. | 03/19/2026 | 03/19/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.7-week7/ |
| 6 | - Thiết kế AWS Step Functions điều phối quy trình bóc tách CV (Textract -> Bedrock -> VectorOps). | 03/20/2026 | 03/20/2026 | |

### Thành tựu Tuần 11:
* **Ưu/Khuyết điểm:** Microservices giúp chia nhỏ việc cho team nhưng bù lại rắc rối ở khâu deploy. Kết hợp đa DB (Polyglot persistence) tận dụng tối đa thế mạnh từng loại.
* **Khó khăn & Khắc phục:** Tranh cãi việc nên dùng DB nào. Quyết định chia cắt State (RDS) và Hot Data (DynamoDB) giúp hệ thống đáp ứng realtime không bị block.
* **Kế hoạch:** Chạy thực tế hệ thống AI (Claude/Cohere), AppSync websocket và xử lý Git merge.
