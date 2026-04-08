---
title: "Worklog Tuần 3"
date: 2026-01-19
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu Tuần 3:
* So sánh Lightsail với EC2 và triển khai app nhanh.
* Đóng gói ứng dụng (Container) và chạy trên Lightsail Containers.
* Thực hành Auto Scaling để đảm bảo High Availability.
* Theo dõi hệ thống qua Amazon CloudWatch.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Triển khai WordPress bằng Amazon Lightsail.<br>- Gắn IP tĩnh cho Lightsail. | 01/19/2026 | 01/19/2026 | https://000045.awsstudygroup.com/ |
| 3 | - Build image Docker đơn giản.<br>- Push và chạy container trên Lightsail Containers.<br>- Map port để truy cập. | 01/20/2026 | 01/20/2026 | https://000046.awsstudygroup.com/ |
| 4 | - Tạo EC2 Launch Template có User Data.<br>- Cấu hình Auto Scaling Group (ASG) trên 2 AZs.<br>- Gắn Application Load Balancer (ALB). | 01/21/2026 | 01/21/2026 | https://000006.awsstudygroup.com/ |
| 5 | - Thử tắt nóng EC2 để xem ASG tự tạo lại.<br>- Cấu hình CloudWatch dashboard. | 01/22/2026 | 01/22/2026 | https://000006.awsstudygroup.com/<br>https://000008.awsstudygroup.com/ |
| 6 | - Tạo CloudWatch Alarm khi CPU > 70%.<br>- Dùng lệnh stress ép CPU EC2 tăng cao để test báo động.<br>- Cấu hình gửi email báo động qua SNS. | 01/23/2026 | 01/23/2026 | https://000008.awsstudygroup.com/ |

### Thành tựu Tuần 3:
* **Ưu/Khuyết điểm:** Lightsail quá tiện lợi cho app nhỏ nhưng thiếu tùy chỉnh sâu như EC2. ASG rất mạnh nhưng cần hiểu rõ ALB.
* **Khó khăn & Khắc phục:** EC2 mới sinh ra từ ASG liên tục bị đánh rớt (Unhealthy). Nguyên nhân là script bash cài Apache chạy lỗi nên không có file index trả về cho ALB. Sửa lại script User Data.
* **Kế hoạch:** Chuyển sang Route 53 và DynamoDB.
