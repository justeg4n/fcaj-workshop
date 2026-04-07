---
title: "Worklog Tuần 7"
date: 2026-02-16
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu Tuần 7:
* Đúc kết kiến trúc Highly Available (Multi-AZ, ALB, RDS).
* Khám phá mảng AI/ML trên AWS và Amazon SageMaker.
* Chạy Jupyter Notebook huấn luyện mô hình.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Xây dựng HA: ALB -> ASG (Private) -> Multi-AZ RDS.<br>- Giả lập sập AZ để xem ALB chuyển traffic. | 02/16/2026 | 02/16/2026 | https://000101.awsstudygroup.com/ |
| 3 | - Tổng quan AI services (Rekognition, Comprehend).<br>- Thiết lập SageMaker Studio. | 02/17/2026 | 02/17/2026 | https://cloudjourney.awsstudygroup.com/7-aimlservice/ |
| 4 | - Tạo Jupyter Notebook trên SageMaker.<br>- Tải data từ S3 dùng Pandas (Python). | 02/18/2026 | 02/18/2026 | https://000200.awsstudygroup.com/ |
| 5 | - Huấn luyện mô hình XGBoost.<br>- Deploy mô hình thành API Endpoint. | 02/19/2026 | 02/19/2026 | https://000200.awsstudygroup.com/ |
| 6 | - Dùng Python (Boto3) gọi API inference dự đoán.<br>- Xóa endpoint ngay lập tức để tiết kiệm tiền. | 02/20/2026 | 02/20/2026 | https://000200.awsstudygroup.com/ |

### Thành tựu Tuần 7:
* **Ưu/Khuyết điểm:** SageMaker là thiên đường cho sinh viên AI, tự động hóa hạ tầng MLOps. Nhược điểm là chi phí GPU cao.
* **Khó khăn & Khắc phục:** SageMaker báo lỗi AccessDenied khi đọc S3. Tôi phải vào IAM, tìm Role của SageMaker và gắn thêm quyền S3.
* **Kế hoạch:** Tuần sau bắt đầu dự án SmartHireAI và thực hành Terraform.