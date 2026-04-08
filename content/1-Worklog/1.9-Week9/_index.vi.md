---
title: "Worklog Tuần 9"
date: 2026-03-02
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu Tuần 9:
* Thiết lập Terraform Remote State cho làm việc nhóm.
* Thực hành Serverless: S3 kích hoạt Lambda.
* Thiết kế schema DynamoDB (Lab Bookstore) và áp dụng ý tưởng cho SmartHireAI.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Cấu hình S3 lưu Terraform State.<br>- Dùng DynamoDB để Lock State tránh xung đột team. | 03/02/2026 | 03/02/2026 |  |
| 3 | - Code hàm Python Lambda (`ingestion_trigger.py`).<br>- Cài đặt trigger tự chạy khi có file mới upload lên S3. | 03/03/2026 | 03/03/2026 |  |
| 4 | - Làm lab Serverless Bookstore.<br>- Kết nối API Gateway tới Lambda. | 03/04/2026 | 03/04/2026 |  |
| 5 | - Thiết kế DynamoDB Single-Table.<br>- Xác định Partition Key và Sort Key linh hoạt. | 03/05/2026 | 03/05/2026 |  |
| 6 | - Ứng dụng lab Bookstore vào SmartHireAI: dùng Lambda để nhận PDF CV upload và đẩy vào SQS. | 03/06/2026 | 03/06/2026 |  |

### Thành tựu Tuần 9:
* **Ưu/Khuyết điểm:** Tư duy DynamoDB khác hoàn toàn SQL, rất khó thiết kế ban đầu nhưng truy vấn siêu tốc. S3-Lambda trigger tạo ra luồng async hoàn hảo.
* **Khó khăn & Khắc phục:** Up file lên S3 mà Lambda không chạy. Kiểm tra Terraform thấy thiếu block `aws_lambda_permission` cấp quyền cho S3 gọi Lambda. Sửa lại và apply thành công.
* **Kế hoạch:** Debug Serverless và AWS Well-Architected Framework.
