---
<<<<<<< HEAD
title: "Worklog Tuần 9"
=======
title: "Nhật ký công việc Tuần 9"
>>>>>>> ed17fed27c55804e764aafb8a979a812b347a0b8
date: 2026-03-02
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
<<<<<<< HEAD
### Mục tiêu Tuần 9:
* Thiết lập Terraform Remote State cho làm việc nhóm.
* Thực hành Serverless: S3 kích hoạt Lambda.
* Thiết kế schema DynamoDB (Lab Bookstore) và áp dụng ý tưởng cho SmartHireAI.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Cấu hình S3 lưu Terraform State.<br>- Dùng DynamoDB để Lock State tránh xung đột team. | 03/02/2026 | 03/02/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.4-week4/ |
| 3 | - Code hàm Python Lambda (`ingestion_trigger.py`).<br>- Cài đặt trigger tự chạy khi có file mới upload lên S3. | 03/03/2026 | 03/03/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.5-week5/ |
| 4 | - Làm lab Serverless Bookstore.<br>- Kết nối API Gateway tới Lambda. | 03/04/2026 | 03/04/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.5-week5/ |
| 5 | - Thiết kế DynamoDB Single-Table.<br>- Xác định Partition Key và Sort Key linh hoạt. | 03/05/2026 | 03/05/2026 | https://aws-first-cloud-journey-six.vercel.app/1-worklog/1.5-week5/ |
| 6 | - Ứng dụng lab Bookstore vào SmartHireAI: dùng Lambda để nhận PDF CV upload và đẩy vào SQS. | 03/06/2026 | 03/06/2026 | |

### Thành tựu Tuần 9:
* **Ưu/Khuyết điểm:** Tư duy DynamoDB khác hoàn toàn SQL, rất khó thiết kế ban đầu nhưng truy vấn siêu tốc. S3-Lambda trigger tạo ra luồng async hoàn hảo.
* **Khó khăn & Khắc phục:** Up file lên S3 mà Lambda không chạy. Kiểm tra Terraform thấy thiếu block `aws_lambda_permission` cấp quyền cho S3 gọi Lambda. Sửa lại và apply thành công.
* **Kế hoạch:** Debug Serverless và AWS Well-Architected Framework.
=======
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
>>>>>>> ed17fed27c55804e764aafb8a979a812b347a0b8
