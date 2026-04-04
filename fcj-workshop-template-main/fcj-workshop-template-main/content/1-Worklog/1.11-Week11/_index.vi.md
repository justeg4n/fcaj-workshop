---
title: "Nhật ký công việc Tuần 11"
date: 2026-03-16
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo.
{{% /notice %}}

### Mục tiêu Tuần 11:
* Tích hợp toàn diện các Engines gợi ý việc làm và xếp hạng ứng viên.
* Tinh chỉnh thuật toán Hybrid Matching (Bi-Encoder + Cross-Encoder).

### Các nhiệm vụ thực hiện trong tuần này:
| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 71 | - Tích hợp `JobSuggestionEngine` và `CandidateRankingEngine` vào Step Functions. | 03/16/2026 | 03/16/2026 | |
| 72 | - Triển khai logic tính điểm Hybrid: 35% Bi-Encoder (Cohere) và 65% Cross-Encoder. | 03/17/2026 | 03/17/2026 | |
| 73 | - Cấu hình AppSync notifier để đẩy kết quả ranking thời gian thực tới Recruiter. | 03/18/2026 | 03/18/2026 | |
| 74 | - Thiết lập Step Functions với cấu trúc 3 bước: Processor -> Choice -> Engine. | 03/19/2026 | 03/19/2026 | |
| 75 | - Tối ưu hóa truy vấn pgvector sử dụng chỉ mục IVFFlat để tăng tốc độ tìm kiếm. | 03/20/2026 | 03/20/2026 | |
| 76 | - Xây dựng Lambda Layer cho Cross-Encoder model để giảm thời gian khởi động nguội. | 03/21/2026 | 03/21/2026 | |
| 77 | - Kiểm thử hệ thống cuối tuần: Đảm bảo luồng Bidirectional Matching hoạt động chính xác. | 03/22/2026 | 03/22/2026 | Test Case |

### Thành quả Tuần 11:
* Hoàn thiện kiến trúc pipeline thống nhất v3.0, giảm độ trễ xử lý bằng cách gộp các Lambda.
* Tích hợp thành công AppSync giúp giao diện hiển thị kết quả matching ngay lập tức sau khi xử lý.