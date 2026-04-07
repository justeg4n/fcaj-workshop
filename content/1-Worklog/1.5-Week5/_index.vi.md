---
title: "Worklog Tuần 4"
date: 2026-02-02
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu Tuần 5:
* Cấu hình caching bộ nhớ trong bằng ElastiCache (Redis).
* Nâng cao kiến thức mạng với AWS Networking Workshop.
* Phân phối nội dung toàn cầu qua Amazon CloudFront.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Khởi tạo ElastiCache Redis.<br>- Gắn Redis vào app Python để giảm tải database. | 02/02/2026 | 02/02/2026 | https://000061.awsstudygroup.com/ |
| 3 | - Networking Workshop: Cấu hình VPC Peering.<br>- Thông mạng giữa 2 VPC khác nhau. | 02/03/2026 | 02/03/2026 | https://000092.awsstudygroup.com/ |
| 4 | - Tìm hiểu Transit Gateway.<br>- Thiết lập NAT Gateway cho phép private subnet ra Internet. | 02/04/2026 | 02/04/2026 | https://000092.awsstudygroup.com/ |
| 5 | - Tạo CloudFront phân phối nội dung từ S3.<br>- Setup Cache Behaviors và TTL. | 02/05/2026 | 02/05/2026 | https://000094.awsstudygroup.com/ |
| 6 | - Thiết lập OAC chặn truy cập trực tiếp S3.<br>- Thực hành Invalidate Cache khi update web. | 02/06/2026 | 02/06/2026 | https://000094.awsstudygroup.com/ |

### Thành tựu Tuần 5:
* **Ưu/Khuyết điểm:** OAC bảo mật cực tốt nhưng rườm rà. Redis giảm độ trễ truy xuất dữ liệu đáng kể.
* **Khó khăn & Khắc phục:** EC2 ở private subnet không ping được Google. Kiểm tra Route Table phát hiện thiếu rule trỏ 0.0.0.0/0 ra NAT Gateway.
* **Kế hoạch:** Học Windows trên AWS và Lambda@Edge tuần tới.