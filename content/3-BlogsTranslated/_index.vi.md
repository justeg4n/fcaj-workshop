---
title: "Các Blog Đã Dịch"
date: 2026-04-05
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Phần này sẽ liệt kê và giới thiệu các blog kỹ thuật về AWS Cloud Operations mà tôi đã nghiên cứu và dịch thuật trong quá trình thực tập. Các bài viết này bao trùm các chủ đề từ giám sát nâng cao, DevOps ứng dụng AI, cho đến tự động hóa hạ tầng lai (hybrid).

### [Blog 1 - Giới thiệu hỗ trợ OpenTelemetry & PromQL trong Amazon CloudWatch](3.1-Blog1/)
Blog này trình bày chi tiết về bản cập nhật hỗ trợ gốc cho giao thức OpenTelemetry (OTLP) và ngôn ngữ truy vấn Prometheus (PromQL) trong Amazon CloudWatch. Bài viết giải thích cách hợp nhất các pipeline thu thập số liệu cho Kubernetes và microservices, xử lý các nhãn có độ đa dạng dữ liệu cao (high-cardinality), và tự động làm giàu (enrich) số liệu với bối cảnh tài nguyên AWS mà không cần dùng đến các exporter bên ngoài.

### [Blog 2 - Chính thức ra mắt (GA) AWS DevOps Agent](3.2-Blog2/)
Bài viết này công bố việc phát hành rộng rãi (General Availability) của AWS DevOps Agent, một trợ lý vận hành tự trị được hỗ trợ bởi AI. Blog đi sâu vào cách agent này giúp giảm Thời gian trung bình để khắc phục (MTTR) bằng cách tự động điều tra sự cố, chủ động ngăn ngừa lỗi và xử lý các tác vụ Kỹ sư Độ tin cậy Hệ thống (SRE) theo yêu cầu trên các môi trường AWS, đa đám mây (Azure) và tại chỗ (on-premises).

### [Blog 3 - Tự động hóa kích hoạt AWS Systems Manager để đăng ký node quản lý lai (hybrid-managed)](3.3-Blog3/)
Quản lý hạ tầng lai đòi hỏi phải đăng ký các máy chủ on-premises thành các node được quản lý trong AWS Systems Manager. Blog này cung cấp một hướng dẫn kiến trúc toàn diện cùng các template CloudFormation để tự động hóa việc tạo và quản lý thông tin xác thực SSM Hybrid Activation thông qua Amazon API Gateway, AWS Lambda và DynamoDB, giúp giảm thiểu đáng kể gánh nặng vận hành thủ công.