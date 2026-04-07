---
title: "Worklog Tuần 6"
date: 2026-02-09
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu Tuần 6:
* Sửa đổi HTTP request tại biên (Edge) bằng Lambda@Edge.
* Triển khai máy chủ Windows trên EC2.
* Cài đặt và quản trị AWS Managed Microsoft AD.

### Các công việc thực hiện trong tuần này:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Viết function Lambda@Edge (Node.js/Python).<br>- Gắn vào CloudFront Viewer Request để sửa header. | 02/09/2026 | 02/09/2026 | https://000130.awsstudygroup.com/ |
| 3 | - Khởi tạo Windows Server 2022 EC2.<br>- Lấy password Administrator và kết nối qua RDP. | 02/10/2026 | 02/10/2026 | https://000093.awsstudygroup.com/ |
| 4 | - Cài đặt IIS web server trên Windows.<br>- Đảm bảo Security Group mở port 3389 và 80. | 02/11/2026 | 02/11/2026 | https://000093.awsstudygroup.com/ |
| 5 | - Tạo thư mục AWS Managed Microsoft AD.<br>- Join domain cho máy Windows EC2. | 02/12/2026 | 02/12/2026 | https://000095.awsstudygroup.com/ |
| 6 | - Test cấu hình Group Policy (GPO) cơ bản.<br>- Xóa các AD directory (tốn phí khá cao). | 02/13/2026 | 02/13/2026 | https://000095.awsstudygroup.com/ |

### Thành tựu Tuần 6:
* **Ưu/Khuyết điểm:** Lambda@Edge cực kỳ lợi hại cho bảo mật và SEO. Tuy nhiên, Windows EC2 chậm và nặng hơn Linux nhiều.
* **Khó khăn & Khắc phục:** Lỗi không thể deploy Lambda@Edge. Đọc docs mới biết dịch vụ này bắt buộc phải tạo ở region us-east-1 (N. Virginia), sau đó nó tự replicate ra toàn cầu.
* **Kế hoạch:** Kiến trúc HA và bước vào mảng AI/ML với SageMaker.
