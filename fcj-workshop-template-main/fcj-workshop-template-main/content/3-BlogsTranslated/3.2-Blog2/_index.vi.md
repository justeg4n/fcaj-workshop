---
title: "Blog 2"
date: 2026-03-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Công bố Phát hành Rộng rãi (GA) AWS DevOps Agent
*Tác giả: Madhu Balaji vào ngày 31 tháng 03 năm 2026*

Hôm nay, chúng tôi xin thông báo chính thức phát hành rộng rãi (General Availability) AWS DevOps Agent. AWS DevOps Agent là người đồng đội vận hành luôn luôn sẵn sàng của bạn. Nó giải quyết và chủ động ngăn ngừa các sự cố, tối ưu hóa độ tin cậy và hiệu suất của ứng dụng, đồng thời xử lý các tác vụ SRE (Kỹ sư Độ tin cậy Hệ thống) theo yêu cầu trên AWS, môi trường đa đám mây (multicloud), và hệ thống tại chỗ (on-premises).

Các đội ngũ vận hành thường tiêu tốn vô số giờ đồng hồ để điều tra sự cố, liên kết dữ liệu trên nhiều công cụ khác nhau và phân loại cảnh báo một cách thủ công. Sự vất vả trong vận hành này đã cướp đi thời gian dành cho sự đổi mới và công việc mang tính chiến lược của các kỹ sư. AWS DevOps Agent xóa bỏ gánh nặng này bằng cách điều tra các sự cố hệt như một kỹ sư DevOps giàu kinh nghiệm. Nó tìm hiểu các ứng dụng của bạn và các mối quan hệ của chúng, làm việc với các công cụ khả năng quan sát (observability), runbooks, kho lưu trữ mã nguồn, và pipeline CI/CD của bạn, rồi liên kết dữ liệu viễn trắc, mã nguồn, và dữ liệu triển khai xuyên suốt tất cả các nền tảng đó. Trong giai đoạn xem trước (preview), các khách hàng và đối tác sử dụng AWS DevOps Agent đã báo cáo MTTR (Thời gian trung bình để khắc phục) giảm tới 75%, điều tra nhanh hơn 80%, và độ chính xác của nguyên nhân gốc rễ (root cause) đạt 94%, qua đó cho phép giải quyết sự cố nhanh hơn từ 3-5 lần.

## AWS DevOps Agent hoạt động như thế nào
AWS DevOps Agent đại diện cho một lớp tác nhân tiên phong mới (frontier agents) — các hệ thống tự trị hoạt động độc lập để đạt được mục tiêu, mở rộng quy mô khổng lồ để giải quyết các tác vụ đồng thời, và chạy liên tục mà không cần sự giám sát thường xuyên của con người. AWS DevOps Agent làm việc song hành với đội ngũ vận hành của bạn trong toàn bộ vòng đời của sự cố — từ lúc phát hiện, điều tra, khôi phục, cho đến ngăn ngừa.

**Phản hồi sự cố tự động:** AWS DevOps Agent bắt đầu điều tra ngay khoảnh khắc một cảnh báo được gửi đến, bất kể là lúc 2 giờ sáng hay trong giờ cao điểm. Điều này giúp giảm thiểu MTTR và nhanh chóng khôi phục ứng dụng của bạn về hiệu suất tối ưu.

**Chủ động ngăn ngừa sự cố:** AWS DevOps Agent chuyển đổi các nhóm từ việc "chữa cháy" thụ động sang cải tiến vận hành chủ động. Nó phân tích các mẫu hình xuyên suốt các sự cố lịch sử để đưa ra các đề xuất nhắm trúng mục tiêu. Các đề xuất này ngăn ngừa sự cố trong tương lai và củng cố độ bền bỉ của quy trình cũng như hệ thống.

**Xử lý các tác vụ SRE theo yêu cầu:** AWS DevOps Agent tận dụng sự thấu hiểu sâu sắc về môi trường của bạn. Điều này cho phép bạn đào sâu vào môi trường ứng dụng của mình không chỉ dừng lại ở việc đặt câu hỏi. Bạn có thể tạo, lưu trữ, và chia sẻ các biểu đồ cũng như báo cáo tùy chỉnh.

## Có gì mới trong Bản Phát hành Rộng rãi (GA)?
Bản phát hành GA mở rộng các khả năng của AWS DevOps Agent dựa trên phản hồi của khách hàng. Nó giúp việc phản hồi sự cố trở nên mở rộng hơn, linh hoạt hơn, và thông minh hơn.

### Mở rộng Các Trường hợp Sử dụng
* **Hỗ trợ Azure:** AWS DevOps Agent nay đã vươn ra ngoài các môi trường AWS để điều tra sự cố trong các workload của Azure. Tác nhân này liên kết dữ liệu trên các triển khai đa đám mây, cung cấp phản hồi sự cố thống nhất bất kể ứng dụng của bạn chạy trên AWS, Azure, hay cả hai.
* **Hỗ trợ On-Premises:** Thông qua Giao thức Ngữ cảnh Mô hình (Model Context Protocol - MCP), tác nhân có thể điều tra hệ thống tại chỗ.
* **Đại lý Triage (Phân loại):** Tác nhân Triage tự động đánh giá mức độ nghiêm trọng của sự cố và xác định các ticket bị trùng lặp.

### Trí thông minh được tăng cường
* **Kỹ năng Học được (Learned Skills):** AWS DevOps Agent học hỏi từ các mô hình điều tra, việc sử dụng công cụ, và cấu trúc liên kết của tổ chức bạn.
* **Kỹ năng Tùy chỉnh (Custom Skills):** Thêm các quy trình điều tra, các thực hành tốt nhất (best practices), và kiến thức tổ chức đặc thù cho hệ thống của bạn.
* **Lập chỉ mục mã nguồn (Code Indexing):** Tác nhân hiện tại lập chỉ mục các kho lưu trữ mã nguồn ứng dụng của bạn. Điều này cho phép nó hiểu cấu trúc mã, xác định các bug tiềm ẩn và đề xuất các bản vá cấp độ code như một phần của kế hoạch khắc phục.

### Tích hợp Mới
Được xây dựng trên nền tảng tích hợp hiện có với Datadog, Dynatrace, New Relic, Splunk, GitHub Actions, GitLab CI/CD, và ServiceNow, chúng tôi bổ sung thêm: PagerDuty, Grafana, Azure DevOps, Amazon EventBridge và các API mới.

## Kết luận
Để tìm hiểu thêm, hãy truy cập AWS DevOps Agent và khám phá Hướng dẫn Người dùng. Nếu có câu hỏi hoặc muốn thảo luận về cách AWS DevOps Agent có thể giúp ích cho tổ chức của bạn, hãy liên hệ với đội ngũ tài khoản AWS của bạn. Đăng ký ngay hôm nay.