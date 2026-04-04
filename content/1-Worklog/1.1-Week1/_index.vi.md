---
title: "Nhật ký công việc Tuần 1"
date: 2026-01-05
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo. Vui lòng **không sao chép nguyên văn** vào báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}

### Mục tiêu Tuần 1:
* Làm quen với chương trình FCJ và thiết lập môi trường làm việc trên AWS.
* Nắm vững các thành phần hạ tầng cốt lõi: Bảo mật IAM, mạng VPC và quản lý chi phí.

### Các nhiệm vụ thực hiện trong tuần này:
| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Định hướng chương trình & giới thiệu FCJ. <br> - Tạo tài khoản AWS Free Tier & thiết lập AWS Budgets. | 01/05/2026 | 01/05/2026 | FCJ Portal |
| 2 | - Cách nhận hỗ trợ từ AWS Support. <br> - Quản lý truy cập với AWS IAM (Người dùng, Nhóm, Chính sách). | 01/06/2026 | 01/06/2026 | AWS Skill Builder |
| 3 | - Cơ bản về Mạng: Hiểu về Subnets, Route Tables và Internet Gateways. | 01/07/2026 | 01/07/2026 | AWS Documentation |
| 4 | - **Thực hành:** Tự xây dựng một VPC tùy chỉnh với Public và Private subnets từ đầu. | 01/08/2026 | 01/08/2026 | FCJ Labs |
| 5 | - Cơ bản về Điện toán: Các loại instance Amazon EC2 và AMIs. <br> - Các phương thức kết nối SSH (MobaXterm/PuTTY). | 01/09/2026 | 01/09/2026 | AWS EC2 Guide |
| 6 | - Instance Profiling với IAM Roles cho EC2. <br> - **Thực hành:** Gắn IAM roles vào EC2 để truy cập S3 một cách bảo mật. | 01/10/2026 | 01/10/2026 | FCJ Labs |
| 7 | - Ôn tập tài liệu tuần 1 và tổng hợp ghi chú về Trạng thái Bảo mật Đám mây. | 01/11/2026 | 01/11/2026 | Tự học |

### Thành quả Tuần 1:
* Triển khai thành công kiến trúc VPC tùy chỉnh với các quy tắc định tuyến và nhóm bảo mật (Security Groups) phù hợp.
* Cấu hình AWS Budgets để ngăn chặn các chi phí phát sinh ngoài ý muốn.
* Hiểu rõ Nguyên tắc Đặc quyền Tối thiểu (Least Privilege) bằng cách cấu hình IAM Roles thay vì sử dụng thông tin xác thực cố định.