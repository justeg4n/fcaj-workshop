---
title: "Tự đánh giá"
date: 2026-04-05
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

Trong suốt thời gian thực tập tại **Bootcamp — First Cloud AI Journey @ AWS Study Group** từ **05/01/2026** đến **05/04/2026**, tôi đã có cơ hội áp dụng và mở rộng kiến thức về machine learning, hạ tầng cloud và kỹ thuật phần mềm trong môi trường dự án thực tế.

Tôi đã tham gia **SmartHire AI** — nền tảng tuyển dụng kỹ thuật được hỗ trợ bởi AI, xây dựng trên hạ tầng serverless AWS — với vai trò AI Engineer chịu trách nhiệm toàn bộ tầng trí tuệ của hệ thống: từ xử lý tài liệu và chấm điểm vector ngữ nghĩa đến đối sánh hai chiều ứng viên-việc làm và tạo giải thích bằng LLM thời gian thực.

Trong suốt dự án, tôi đã cải thiện kỹ năng **phát triển Lambda bằng Python, prompt engineering với Amazon Bedrock, tìm kiếm ngữ nghĩa với pgvector, cross-encoder reranking, ẩn PII bằng NLP, phương pháp hiệu chỉnh điểm số, observability có cấu trúc với CloudWatch và thiết kế pipeline AWS bất đồng bộ**.

Về tác phong, tôi luôn ưu tiên độ chính xác hơn tốc độ — hiểu rằng điểm AI sai trực tiếp ảnh hưởng đến các quyết định tuyển dụng thực tế. Tôi tích cực phối hợp với các thành viên Backend và Frontend để căn chỉnh các data contract và đảm bảo đầu ra pipeline AI có thể được phần còn lại của hệ thống sử dụng ngay lập tức.

Để phản ánh khách quan quá trình thực tập, tôi xin tự đánh giá dựa trên các tiêu chí dưới đây:

| STT | Tiêu chí | Mô tả | Tốt | Khá | Trung bình |
| --- | --- | --- | --- | --- | --- |
| 1 | **Kiến thức và kỹ năng chuyên môn** | Hiểu biết về NLP, tìm kiếm ngữ nghĩa, cross-encoders, Bedrock API, pgvector, Lambda deployment và thiết kế pipeline Python | ✅ | ☐ | ☐ |
| 2 | **Khả năng học hỏi** | Tự học và áp dụng các dịch vụ AWS mới (Bedrock, Textract, AppSync, Step Functions) và các khái niệm ML (calibrated sigmoid, asymmetric embedding) chưa được dạy ở trường | ☐ | ✅ | ☐ |
| 3 | **Chủ động** | Tự xác định nguyên nhân gốc rễ của sự không khớp giữa điểm và giải thích, đề xuất kế hoạch hiệu chỉnh 5 bước và thực thi từng bước có xác minh trước khi tiến hành | ✅ | ☐ | ☐ |
| 4 | **Tinh thần trách nhiệm** | Mọi thay đổi pipeline AI đều được kiểm tra sau mỗi bước với cùng một CV baseline trước khi tiến hành — không bao giờ phát hành thay đổi điểm số chưa được kiểm tra lên DynamoDB dùng chung | ✅ | ☐ | ☐ |
| 5 | **Kỷ luật** | Tuân theo quy trình deploy-và-test theo từng bước đã thỏa thuận mà không bỏ qua, ngay cả khi kết quả có thể nhìn thấy sớm | ☐ | ✅ | ☐ |
| 6 | **Tính cầu tiến** | Chủ động tiếp thu phản hồi từ thành viên Backend về sự không khớp API contract và từ thành viên Frontend về cách hiển thị điểm số, điều chỉnh output schema theo đó | ✅ | ☐ | ☐ |
| 7 | **Giao tiếp** | Viết tài liệu chi tiết trong code cho mỗi hàm (lý do calibrated sigmoid, lý do asymmetric Cohere input types, lý do top-K weighted aggregation) giúp codebase tự giải thích cho việc review nhóm | ☐ | ✅ | ☐ |
| 8 | **Hợp tác nhóm** | Phối hợp với nhóm Backend về chiến lược caching `jd_vector` để `cv_jd_processor` có thể tái sử dụng JD embeddings thay vì tính lại cho mỗi ứng viên — tối ưu hóa chung có lợi cho cả hai nhóm | ✅ | ☐ | ☐ |
| 9 | **Ứng xử chuyên nghiệp** | Duy trì tính khách quan khi đánh giá chất lượng đầu ra AI — nhận ra khi điểm số sai do vấn đề kỹ thuật thay vì bảo vệ các lựa chọn triển khai ban đầu | ✅ | ☐ | ☐ |
| 10 | **Tư duy giải quyết vấn đề** | Chẩn đoán sự không khớp L2-vs-cosine distance khiến bi-score Technical Lead hiển thị 44% trong khi điểm cosine đúng là ~70%, truy nguyên đến toán tử SQL `<->` vs `<=>` và đề xuất bản sửa lỗi phẫu thuật | ✅ | ☐ | ☐ |
| 11 | **Đóng góp vào dự án/tổ chức** | Kế hoạch hiệu chỉnh 5 bước và Phase 1A đồng bộ giải thích nhận biết điểm số được khởi xướng và thực thi hoàn toàn bởi vai trò AI Engineer, trực tiếp giải quyết vấn đề chất lượng nổi bật nhất trong sản phẩm | ✅ | ☐ | ☐ |
| 12 | **Tổng thể** | Giao nộp pipeline chấm điểm AI sẵn sàng production với cải tiến có thể đo lường: chênh lệch giữa đối sánh tốt nhất và kém nhất tăng từ ~5 điểm lên ≥30 điểm trên ba công việc kiểm thử | ✅ | ☐ | ☐ |

### Cần cải thiện

- **Chiều sâu toán học trong hệ thống ML**: Mặc dù có thể xác định rằng calibrated sigmoid là cần thiết, tôi đã dựa vào các giá trị shift/scale theo kinh nghiệm thực hành thay vì suy ra từ phân tích phân phối logit thực nghiệm. Xây dựng thói quen đo lường trước và tinh chỉnh sau — thay vì áp dụng các giá trị mặc định hợp lý — sẽ làm cho các lần hiệu chỉnh trong tương lai có nguyên tắc hơn.

- **Tốc độ giao tiếp API giữa các nhóm**: Sự không nhất quán trong đặt tên key `jd_vector` (là `jdVector` trong DynamoDB so với `jd_vector` trong code Python) đã gây ra lỗi im lặng trong `ingestion_trigger.py` trong hai chu kỳ sprint trước khi được phát hiện. Thiết lập tài liệu data contract dùng chung từ đầu mỗi tính năng sẽ ngăn chặn hoàn toàn loại lỗi này.

- **Quyền sở hữu latency end-to-end**: Thời gian wall-clock của pipeline AI (Textract ~4s + Claude calls ~4s + Cohere embed ~1s = ~9s tổng mỗi CV) được đo muộn trong dự án. Profiling từng bước với X-Ray từ Tuần 3 trở đi — thay vì Tuần 11 — sẽ cho phép các quyết định kiến trúc (ví dụ: gọi Claude song song) được tích hợp sớm hơn.
