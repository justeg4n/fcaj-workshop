---
title: "Worklog Tuần 12"
date: 2026-03-23
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Mục tiêu Tuần 12:
* Hoàn thiện tích hợp multi-agent Claude và kiến trúc AppSync.
* Triển khai hàm tìm kiếm khoảng cách vector L2 (`job_suggestion_engine`).
* Khắc phục lỗi Git và hoàn thiện deploy Production.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Code `vector_ops.py` gọi Amazon Bedrock (Cohere) lấy embedding.<br>- Lưu vector vào RDS pgvector. | 03/23/2026 | 03/23/2026 | |
| 3 | - Xây dựng `candidate_ranking_engine.py` dùng Claude 3.<br>- Xử lý merge code branch `ai-core` trên GitHub. | 03/24/2026 | 03/24/2026 | |
| 4 | - Viết GraphQL schema cho AWS AppSync.<br>- Cấu hình WebSockets báo realtime về frontend khi AI xử lý xong. | 03/25/2026 | 03/25/2026 | |
| 5 | - Xử lý lỗi Git bỏ sót thư mục (`git add iac/lambda/* -f`).<br>- Push các lambda Python cuối cùng lên Git. | 03/26/2026 | 03/26/2026 | |
| 6 | - Test End-to-End luồng Phỏng vấn (Interview Flow).<br>- Tổng kết tài liệu thực tập. | 03/27/2026 | 03/27/2026 | |

### Thành tựu Tuần 12:
* **Ưu/Khuyết điểm:** Kiến trúc Split-Brain tách biệt hoàn toàn AI nặng nề khỏi API .NET tốc độ cao. AppSync đem lại trải nghiệm thời gian thực tuyệt vời.
* **Khó khăn & Khắc phục:** Git từ chối push thư mục rỗng. Phải clear `.gitignore` bị dính ký tự lạ, copy code Python vào đúng chỗ và dùng lệnh `git switch` ép thư mục tracking thành công.
* **Tổng kết:** 12 tuần thực tập đã chuyển hóa kiến thức AI lý thuyết tại ĐH FPT thành kỹ năng Cloud thực chiến. Tôi đã tự tin thiết kế, deploy và vận hành hệ thống Serverless AI quy mô lớn trên AWS.
