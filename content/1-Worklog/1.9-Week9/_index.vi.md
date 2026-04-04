---
title: "Nhật ký công việc Tuần 9"
date: 2026-03-02
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo.
{{% /notice %}}

### Mục tiêu Tuần 9:
* Chuyển đổi SmartHireAI sang hạ tầng không máy chủ (Serverless).
* Xây dựng luồng xử lý CV bằng S3, SQS và Lambda.

### Các nhiệm vụ thực hiện trong tuần này:
| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 57 | - Tạo S3 Bucket `smarthire-cv-storage` để lưu trữ CV ứng viên. | 03/02/2026 | 03/02/2026 | AWS Console |
| 58 | - Tạo SQS Queues để tách biệt xử lý giữa Backend và AI Lambdas. | 03/03/2026 | 03/03/2026 | AWS SQS Docs |
| 59 | - Thiết kế IAM policies cho phép Lambda truy cập Textract và Bedrock. | 03/04/2026 | 03/04/2026 | IAM Policy |
| 60 | - Tạo Lambda Layer chứa Boto3 phiên bản mới nhất cho Bedrock. | 03/05/2026 | 03/05/2026 | Lambda Docs |
| 61 | - Lập trình `cv_parser.py`: Trích xuất text PDF bằng Textract. | 03/06/2026 | 03/06/2026 | Boto3 Textract |
| 62 | - Kiểm thử tích hợp Bedrock (Claude 3.5 Sonnet) để phân tích kỹ năng. | 03/07/2026 | 03/07/2026 | Bedrock API |
| 63 | - Đánh giá độ trễ và giới hạn (throttling) của Bedrock. | 03/08/2026 | 03/08/2026 | CloudWatch |

### Thành quả Tuần 9:
* Triển khai thành công luồng sự kiện: S3 -> SQS -> Lambda xử lý CV.
* Trích xuất thông tin kỹ năng từ CV dưới dạng JSON có cấu trúc bằng Claude 3.5.