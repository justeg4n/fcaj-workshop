---
title: "Worklog Tuần 4"
date: 2026-01-26
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu Tuần 4:
* Quản lý DNS và policy với Amazon Route 53.
* Chuyển đổi thói quen dùng Console sang AWS CLI.
* Làm quen NoSQL và Amazon DynamoDB.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Trải nghiệm Route 53 Hosted Zone.<br>- Thử nghiệm Simple và Weighted Routing.<br>- Trỏ record A về ALB. | 01/26/2026 | 01/26/2026 | https://000010.awsstudygroup.com/ |
| 3 | - Cài đặt AWS CLI trên máy cá nhân.<br>- Dùng lệnh tạo S3, list EC2. | 01/27/2026 | 01/27/2026 | https://000011.awsstudygroup.com/ |
| 4 | - Viết shell script tự động bật/tắt EC2 bằng CLI.<br>- Lọc output JSON bằng JMESPath. | 01/28/2026 | 01/28/2026 | https://000011.awsstudygroup.com/ |
| 5 | - Học kiến trúc NoSQL. Tạo bảng DynamoDB với Partition & Sort Key.<br>- Nhập data mẫu trên Console. | 01/29/2026 | 01/29/2026 | https://000060.awsstudygroup.com/ |
| 6 | - Viết code Python (Boto3) tương tác DynamoDB.<br>- Thực hiện CRUD (Put, Get, Update, Delete). | 01/30/2026 | 01/30/2026 | https://000060.awsstudygroup.com/ |

### Thành tựu Tuần 4:
* **Ưu/Khuyết điểm:** DynamoDB cực nhanh và hợp với Python. AWS CLI khó thuộc lệnh nhưng tiết kiệm thời gian click chuột.
* **Khó khăn & Khắc phục:** Dùng lệnh `Scan` trong Boto3 để tìm dữ liệu, bị mentor nhắc nhở vì rất tốn RCU. Phải thiết kế lại Key Schema để có thể dùng hàm `Query` tối ưu hơn.
* **Kế hoạch:** Tìm hiểu Caching và CDN tuần sau.
