---
title: "Nhật ký công việc Tuần 10"
date: 2026-03-09
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo.
{{% /notice %}}

### Mục tiêu Tuần 10:
* Giải quyết các vấn đề truy cập chéo vùng của Bedrock.
* Triển khai kiến trúc hướng sự kiện hoàn chỉnh cho giai đoạn phân tích CV.

### Các nhiệm vụ thực hiện trong tuần này:
| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 64 | - Debug lỗi `ThrottlingException` của Bedrock bằng cơ chế exponential backoff. | 03/09/2026 | 03/09/2026 | Boto3 Config |
| 65 | - Xử lý lỗi đăng ký Marketplace cho Claude 3.5 Sonnet v2 với tài khoản Root. | 03/10/2026 | 03/10/2026 | AWS Bedrock |
| 66 | - Cấu hình Cross-Region Inference để gọi mô hình US từ Region Singapore. | 03/11/2026 | 03/11/2026 | AWS Blog |
| 67 | - Xử lý phân trang trong Textract cho các CV nhiều trang bằng `NextToken`. | 03/12/2026 | 03/12/2026 | Textract API |
| 68 | - Tối ưu chi phí: Gộp việc phân tích CV và so khớp JD vào một lần gọi AI duy nhất. | 03/13/2026 | 03/13/2026 | Prompt Eng |
| 69 | - Thiết kế JSON schema trao đổi dữ liệu giữa Backend và Lambda qua SQS. | 03/14/2026 | 03/14/2026 | JSON Spec |
| 70 | - Ôn tập Tuần 10: Trình diễn pipeline SQS -> Lambda -> Bedrock hoạt động ổn định. | 03/15/2026 | 03/15/2026 | Demo |

### Thành quả Tuần 10:
* Giải quyết triệt để vấn đề giới hạn vùng của Bedrock bằng Cross-Region Inference.
* Xây dựng pipeline hướng sự kiện ổn định: SQS -> Lambda -> Textract -> Bedrock.