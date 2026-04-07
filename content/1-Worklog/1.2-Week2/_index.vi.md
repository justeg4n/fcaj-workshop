---
title: "Worklog Tuần 2"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu Tuần 2:
* Thiết kế Virtual Private Cloud (VPC) tùy chỉnh.
* Sử dụng IDE trên đám mây AWS Cloud9.
* Triển khai website tĩnh bằng Amazon S3.
* Thiết lập và kết nối với cơ sở dữ liệu quan hệ (RDS).

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Học VPC (Subnets, IGW, Route Tables).<br>- Tạo VPC với 1 public và 1 private subnet.<br>- Kiểm tra luồng mạng. | 01/12/2026 | 01/12/2026 | https://000003.awsstudygroup.com/ |
| 3 | - Khởi tạo môi trường AWS Cloud9.<br>- Clone source code, cài Python/Node JS dependencies.<br>- Viết script test nhanh. | 01/13/2026 | 01/13/2026 | https://000049.awsstudygroup.com/ |
| 4 | - Tạo S3 Bucket, bật tính năng static website hosting.<br>- Upload file `index.html`.<br>- Viết Bucket Policy cho phép truy cập public. | 01/14/2026 | 01/14/2026 | https://000057.awsstudygroup.com/ |
| 5 | - Tạo Amazon RDS MySQL instance (Free Tier).<br>- Đặt RDS vào private subnet để bảo mật.<br>- Mở port 3306 ở Security Group. | 01/15/2026 | 01/15/2026 | https://000005.awsstudygroup.com/ |
| 6 | - Kết nối RDS từ Cloud9.<br>- Chạy lệnh SQL tạo bảng cơ bản.<br>- Xóa RDS và snapshot dọn dẹp. | 01/16/2026 | 01/16/2026 | https://000005.awsstudygroup.com/ |

### Thành tựu Tuần 2:
* **Ưu/Khuyết điểm:** Tư duy mạng VPC rất logic nhưng dễ làm sai. Ưu: Cloud9 code mượt không lo cấu hình máy. Khuyết: Đợi RDS chạy lên rất lâu (5-10 phút).
* **Khó khăn & Khắc phục:** Website S3 báo lỗi 403 Forbidden. Đọc kĩ docs, tôi phải tắt "Block all public access" và thêm policy `s3:GetObject` dạng JSON.
* **Kế hoạch:** Nghiên cứu Lightsail và Auto Scaling vào tuần sau.
