---
title: "Bản đề xuất"
date: 2026-01-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SmartHire AI — Nền tảng Tuyển dụng Kỹ thuật được Hỗ trợ bởi AI
## Hệ thống Đối sánh Hai chiều Serverless trên AWS cho Quy trình Tuyển dụng Hiện đại

### 1. Tóm tắt điều hành

SmartHire AI là nền tảng tuyển dụng kỹ thuật end-to-end được hỗ trợ bởi AI, xây dựng hoàn toàn trên hạ tầng serverless của AWS. Hệ thống tự động hóa các bước tốn thời gian nhất trong quy trình tuyển dụng — sàng lọc CV, đối sánh ứng viên-việc làm và tạo câu hỏi phỏng vấn — thông qua pipeline AI đa tầng.

Nền tảng triển khai **đối sánh hai chiều**: khi ứng viên tải CV lên, hệ thống tự động gợi ý những công việc phù hợp nhất. Khi nhà tuyển dụng đăng mô tả công việc, hệ thống ngay lập tức xếp hạng các ứng viên phù hợp nhất từ pool nhân tài hiện có. Cả hai luồng đều được điều khiển bởi engine chấm điểm AI lai kết hợp Cohere Embed v3 (tìm kiếm vector ngữ nghĩa) và Cross-Encoder reranker (ms-marco-MiniLM-L-6-v2), với Amazon Bedrock Claude 3.5 Sonnet tạo ra các giải thích, hướng dẫn phỏng vấn và báo cáo tổng hợp dễ đọc.

Nhóm gồm 5 thành viên — 2 Backend (.NET 8 / AWS Lambda), 2 Frontend (ReactJS / WebSocket), và 1 AI Engineer (Python / Bedrock / pgvector) — vận hành trên stack AWS serverless hoàn toàn với chi phí máy chủ cố định bằng không.

---

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại là gì?

Tuyển dụng kỹ thuật tốn kém và thiếu nhất quán. Nhà tuyển dụng tại các công ty vừa và nhỏ phải mất 3–5 giờ mỗi ứng viên chỉ để sàng lọc thủ công trước khi tiến hành phỏng vấn. Ứng viên không nhận được phản hồi có cấu trúc. Các nền tảng hiện có (HackerRank, Greenhouse, LinkedIn Recruiter) hoặc quá đắt cho các nhóm nhỏ, hoặc không cung cấp đánh giá AI thực sự — chúng chỉ kiểm tra code có biên dịch được không, chứ không đánh giá ứng viên có thực sự hiểu những gì họ xây dựng hay không.

Ba khoảng trống cụ thể tồn tại trong thị trường hiện tại:

1. **Từ vựng bất đối xứng**: Ngôn ngữ CV ("thiết kế các cluster có tính sẵn sàng cao") và ngôn ngữ JD ("yêu cầu kinh nghiệm AWS EC2") có cùng nghĩa nhưng các hệ thống đơn giản lại nhúng chúng vào các vùng vector khác nhau. Cosine similarity thuần túy thất bại ở đây.
2. **Thiếu tính hai chiều**: Hầu hết các nền tảng chỉ đối sánh một CV với một JD theo yêu cầu. Không có hệ thống nào chủ động gợi ý "bạn nên xem ứng viên này" với nhà tuyển dụng ngay khi JD mới được đăng.
3. **Rủi ro thiên kiến**: Các hệ thống AI xử lý văn bản CV thô có thể vô tình ưu tiên tên gọi, trường đại học và quốc tịch. Không có công cụ tuyển dụng chủ lưu nào giải quyết điều này bằng cách ẩn PII tự động.

#### Giải pháp

SmartHire AI giải quyết cả ba khoảng trống:

- **Engine chấm điểm lai** (Bi-Encoder + Cross-Encoder) thu hẹp khoảng cách từ vựng với trọng số 65% cho Cross-Encoder, vốn xử lý CV và JD đồng thời qua attention của transformer thay vì so sánh các embedding độc lập.
- **Pipeline hai chiều** đảm bảo cả CV mới tải lên và JD mới đăng đều kích hoạt các lần chạy đối sánh tự động theo chiều ngược lại, lưu ngay vào DynamoDB và đẩy đến người dùng liên quan qua WebSocket AppSync.
- **Module sàng lọc mù** (spaCy NER) chạy trước khi bất kỳ mô hình AI nào nhìn thấy văn bản CV, thay thế các thực thể PERSON, ORG, GPE và NORP bằng các token trung lập, đảm bảo mọi điểm số đều dựa thuần túy trên năng lực kỹ thuật.

#### Lợi ích và hoàn vốn đầu tư

| Chỉ số | Tình trạng hiện tại | Với SmartHire AI |
|---|---|---|
| Thời gian sàng lọc của nhà tuyển dụng/ứng viên | 3–5 giờ | ~15 phút (đọc tóm tắt AI) |
| Tính nhất quán đánh giá | Tùy thuộc nhà tuyển dụng | Điểm STAR chuẩn hóa |
| Phản hồi cho ứng viên | Không có | Bảng điểm có phân tích kỹ năng còn thiếu |
| Chi phí vận hành nền tảng | Phí cố định | $0 khi không có lưu lượng (100% serverless) |

Chi phí vận hành ước tính khoảng **$0.08 mỗi phiên xử lý CV** (gọi Bedrock Claude + Cohere embeddings + Textract). Không có chi phí máy chủ cố định. Nền tảng được xây dựng hoàn toàn với các dịch vụ AWS đủ điều kiện Free Tier cho môi trường phát triển và chi phí tăng tuyến tính chỉ theo mức sử dụng thực tế trong production.

---

### 3. Kiến trúc giải pháp

Nền tảng được chia thành hai luồng chính chia sẻ một Lambda xử lý AI chung:

**Luồng Ứng viên**: Trình duyệt → Route 53 → CloudFront → S3 Frontend | Ứng viên tải CV (presigned URL) → S3 → SQS → IngestionTrigger Lambda → Step Functions → `cv_jd_processor` Lambda → Routing Choice → `JobSuggestionEngine` Lambda → DynamoDB + AppSync push

**Luồng Nhà tuyển dụng**: Trình duyệt → Route 53 → WAF → API Gateway → Cognito → Job Lambda (.NET) → Step Functions → `cv_jd_processor` Lambda → Routing Choice → `CandidateRankingEngine` Lambda → DynamoDB + AppSync push

Cả hai luồng hội tụ trong Step Functions và phân kỳ tại Choice State dựa trên `profile_id`.

![Kiến trúc SmartHire AI — Luồng Ứng viên và Nhà tuyển dụng](/images/2-Proposal/architecture_overview.png)

![SmartHire AI — Chi tiết Pipeline AI (cv_jd_processor → Engines)](/images/2-Proposal/ai_pipeline_detail.png)

#### Dịch vụ AWS sử dụng

| Dịch vụ | Vai trò |
|---|---|
| **Amazon Route 53** | DNS cho cả hai subdomain `app.` và `api.` |
| **Amazon CloudFront + ACM** | CDN cho React SPA, chứng chỉ HTTPS |
| **AWS WAF** | Gắn vào API Gateway — lọc SQLi, XSS, giới hạn tốc độ |
| **Amazon API Gateway** | Endpoint REST + WebSocket |
| **Amazon Cognito** | Xác thực JWT cho mọi lệnh gọi API |
| **AWS Step Functions** | Điều phối pipeline xử lý CV/JD |
| **AWS Lambda (.NET 8)** | Auth Service, Job Service, Stream Master |
| **AWS Lambda (Python)** | IngestionTrigger, cv_jd_processor, JobSuggestionEngine, CandidateRankingEngine |
| **Amazon SQS** | `cv-upload-queue` — tách rời S3 events khỏi Lambda |
| **Amazon S3** | CV Bucket (presigned uploads), lưu trữ Frontend, lưu trữ Reports |
| **Amazon RDS PostgreSQL** | Nguồn dữ liệu chính — Users, Jobs, Applications, pgvector embeddings |
| **Amazon RDS Proxy** | Connection pooling cho Lambda → RDS |
| **Amazon ElastiCache Redis** | Cache hot-read, trạng thái phiên |
| **Amazon DynamoDB** | Hot store — JOB_SUGGESTIONS, CANDIDATE_RANKING, đọc thời gian thực |
| **Amazon Bedrock (Claude 3.5 Sonnet)** | Agent 1: trích xuất kỹ năng · Agent 2: hướng dẫn phỏng vấn · Agent 3: giải thích |
| **Amazon Bedrock (Cohere Embed v3)** | Embedding 1024-dim — `search_document` để lưu trữ, `search_query` cho ANN |
| **Amazon Textract** | Trích xuất văn bản PDF không đồng bộ từ file CV |
| **Amazon AppSync** | GraphQL + WebSocket push đến cả hai người dùng |
| **Amazon SNS + SES** | Thông báo email khi kết quả sẵn sàng |
| **AWS NAT Gateway** | Lambda trong private-subnet gọi ra ngoài đến Bedrock, Textract, SNS, SES |
| **AWS VPC Endpoint** | Truy cập DynamoDB — lưu lượng ở lại trong backbone AWS |
| **AWS Secrets Manager** | Mọi API key, thông tin đăng nhập RDS — không có secret được hardcode |
| **AWS KMS** | Mã hóa cho S3 recordings và các trường DynamoDB nhạy cảm |
| **Amazon CloudWatch + X-Ray** | Tracing end-to-end, metrics Lambda, dashboard tùy chỉnh |
| **AWS IAM** | Role tối thiểu quyền cho mỗi Lambda và Step Functions state |

#### Thiết kế thành phần Pipeline AI

Chuỗi xử lý AI cốt lõi bên trong `cv_jd_processor` chạy các bước này theo thứ tự, tất cả qua NAT Gateway đến Bedrock:

1. **Amazon Textract** — trích xuất văn bản PDF không đồng bộ
2. **spaCy NER (local, bên trong Lambda)** — ẩn PII trước khi bất kỳ mô hình AI nào nhìn thấy văn bản
3. **Claude 3.5 Sonnet — Agent 1** — trích xuất kỹ năng có cấu trúc (JSON), `temperature=0.0`
4. **Claude 3.5 Sonnet — Agent 2** — tạo hướng dẫn phỏng vấn từ các kỹ năng còn thiếu, `temperature=0.3`
5. **Cohere Embed v3** — embedding `search_document` → lưu vào RDS pgvector
6. **Cross-Encoder ms-marco-MiniLM-L-6-v2 (local, baked vào container)** — xếp hạng lại top-N ứng viên/việc làm
7. **Claude 3.5 Sonnet — Agent 3** — giải thích đối sánh hoặc snapshot ứng viên, `temperature=0.4`

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

Dự án theo 4 giai đoạn trong 13 tuần:

**Giai đoạn 1 — Nền tảng (Tuần 1–2):**
Thiết lập tài khoản AWS, IAM roles, Cognito, CI/CD pipeline (GitHub Actions → SAM deploy), scaffold React + Tailwind, CloudFront + S3 hosting, kiểm tra WebSocket API Gateway.

**Giai đoạn 2 — Pipeline cốt lõi (Tuần 3–6):**
Định nghĩa Step Functions workflow, Lambda `cv_jd_processor` với Textract + Claude Agent 1, Cohere embed + lưu RDS pgvector, kết nối SQS → IngestionTrigger → Step Functions, luồng upload presigned URL.

**Giai đoạn 3 — AI Engines + Đối sánh hai chiều (Tuần 7–10):**
`JobSuggestionEngine` (ANN + Cross-Encoder + Claude Agent 3 cho giải thích đối sánh), `CandidateRankingEngine` (tương tự cho phía nhà tuyển dụng), AppSync mutations, ghi DynamoDB hot store, thông báo SNS/SES.

**Giai đoạn 4 — Hiệu chỉnh điểm số, Kiểm thử, Demo (Tuần 11–13):**
Hiệu chỉnh điểm số 5 bước (sửa L2→cosine, calibrated sigmoid, unified aggregation, chunk lớn hơn, tinh chỉnh hybrid weights), đồng bộ giải thích nhận biết điểm số, kiểm tra replay với dữ liệu production, lập kịch bản demo (hai persona ứng viên: mạnh vs yếu), kiểm thử tích hợp cuối.

#### Tuần 12 — Deliverables của AI Engineer (Tuần hiện tại)

- **Bước 1:** Sửa công thức bi-score — thay đổi toán tử SQL từ `<->` (L2) sang `<=>` (cosine distance) trong cả `JobSuggestionEngine` và `CandidateRankingEngine`. Cập nhật chuyển đổi điểm số từ ánh xạ L2 tuyến tính sang `(1 - cosine_distance) * 100`. Kết quả mong đợi: Technical Lead biScore tăng từ ~44 lên ~68–75.
- **Bước 2:** Thay `_sigmoid` thô bằng `_calibrated_sigmoid(shift=-1.0, scale=1.5)` trong cả ba Lambda. Kết quả mong đợi: crossScore cho các đối sánh liên quan tăng từ ~38 lên ~62–72.
- **Bước 3:** Thống nhất chiến lược tổng hợp — thay thế max pooling trong `JobSuggestionEngine` bằng `_aggregate_chunk_scores([0.55, 0.30, 0.15])` để nhất quán với hai Lambda kia.
- **Phase 1A:** Cập nhật `generate_match_explanation` và `generate_candidate_snapshot` để nhận `final_score` và đưa vào Claude prompt một chỉ thị về tông giọng (ánh xạ theo bậc mạnh/trung bình/yếu).

#### Tuần 13 — Deliverables của AI Engineer (Tuần cuối)

- **Bước 4:** Tăng kích thước input cho cross-encoder qua env vars: `CE_JD_QUERY_CHARS=2500`, `CE_CHUNK_CHARS=1200`, `CE_OVERLAP_CHARS=200`, `CE_MAX_CHUNKS=6`.
- **Bước 5:** Tinh chỉnh hybrid weights dựa trên kết quả quan sát từ Bước 1–4. Điều chỉnh `SUGGESTION_WEIGHT_BI` và `SUGGESTION_WEIGHT_CROSS` trong biến môi trường Lambda mà không cần deploy code.
- **Phase 1C:** Triển khai logging có cấu trúc CloudWatch: ghi lại `biScore`, `crossScore`, `finalScore`, `threshold_pass`, `scorer_mode` và `explanation_mode` cho mỗi lần xử lý.
- **Chuẩn bị Demo:** Lập kịch bản hai persona ứng viên cho Demo Day — Persona A (mạnh, điểm ≥80) và Persona B (yếu, điểm ≤42) — và xác minh tông giọng giải thích được hiệu chỉnh chính xác cho từng trường hợp.
- **Tài liệu:** Cập nhật `architecture-smart-matching.md` với số thứ tự luồng cuối cùng, chuỗi gọi mô hình AI và bảng tham chiếu biến môi trường.

#### Yêu cầu kỹ thuật

**Stack AI Engineer:**
- Python 3.11, `sentence-transformers`, `spacy`, `pg8000`, `boto3`
- Triển khai container image (Lambda) — cross-encoder được baked tại thời điểm build vào `/opt/ml/cross_encoder`
- AWS Bedrock API (Claude 3.5 Sonnet APAC endpoint, Cohere Embed v3)
- pgvector với index `vector_cosine_ops` trên `candidate_embeddings` và `job_embeddings`
- Step Functions Express Workflows, SQS DLQ, EventBridge routing

---

### 5. Lộ trình & Mốc triển khai

| Thời gian | Mốc |
|---|---|
| Tuần 1–2 | Nền tảng: tài khoản AWS, CI/CD, scaffold frontend, WebSocket API Gateway |
| Tuần 3–4 | CV ingestion: S3 → SQS → IngestionTrigger → Step Functions → Textract |
| Tuần 5–6 | Claude Agent 1 + Cohere embed + pgvector storage |
| Tuần 7–8 | JobSuggestionEngine + CandidateRankingEngine (ANN + Cross-Encoder) |
| Tuần 9–10 | Claude Agent 3 + AppSync push + DynamoDB hot store |
| Tuần 11 | Kiểm thử tích hợp, đo baseline điểm số |
| **Tuần 12** | **Hiệu chỉnh điểm số Bước 1–3 + Phase 1A (giải thích nhận biết điểm số)** |
| **Tuần 13** | **Bước 4–5 + Phase 1C telemetry + chuẩn bị demo + tài liệu** |

---

### 6. Ước tính ngân sách

Tất cả chi phí dưới đây được ước tính cho môi trường phát triển/demo (lưu lượng thấp). Chi phí production tăng tuyến tính theo mức sử dụng — hệ thống tốn $0 khi không có lưu lượng.

| Dịch vụ | Chi phí/Tháng (Dev) | Ghi chú |
|---|---|---|
| AWS Lambda | $0.00 | Free tier đủ cho mọi lần gọi dev |
| Amazon S3 | $0.05 | CV bucket + lưu trữ reports |
| Amazon DynamoDB | $0.00 | On-demand, free tier |
| Amazon RDS PostgreSQL (Multi-AZ) | $0.00 | Instance dev — t3.micro free tier |
| Amazon Bedrock (Claude 3.5 Sonnet) | ~$0.30 | ~50 phiên xử lý CV/tháng |
| Amazon Bedrock (Cohere Embed v3) | ~$0.05 | ~200 lần gọi embedding/tháng |
| Amazon Textract | ~$0.08 | ~50 file CV/tháng |
| AWS Step Functions | $0.00 | Free tier — 4.000 state transitions |
| Amazon SQS | $0.00 | Free tier |
| Amazon API Gateway | $0.01 | ~2.000 requests |
| Amazon CloudFront | $0.00 | Free tier |
| Amazon AppSync | $0.01 | ~2.000 kết nối |
| Amazon SNS + SES | $0.00 | Free tier |
| NAT Gateway | ~$0.05 | Dev — lưu lượng data thấp |

**Tổng: ~$0.55/tháng (phát triển) · ~$0.08/phiên CV (chi phí xử lý AI)**

Phần cứng: Không cần phần cứng bổ sung. Mọi xử lý đều là cloud-native.

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

| Rủi ro | Ảnh hưởng | Xác suất | Biện pháp giảm thiểu |
|---|---|---|---|
| Bedrock throttling dưới tải | Cao | Trung bình | Adaptive retry config (tối đa 10 lần), SQS làm buffer |
| Cross-encoder cold start (Lambda) | Trung bình | Cao | Bake model vào container image lúc build — loại bỏ download lúc cold start |
| Không khớp loại index pgvector (L2 vs cosine) | Cao | Thấp | Xác minh index bằng query `pg_indexes` trước khi deploy Bước 1 |
| Giải thích điểm thấp nhưng text tích cực | Trung bình | Cao | Phase 1A: đưa `final_score` + chỉ thị tông giọng vào mỗi prompt Claude Agent 3 |
| Vượt chi phí Bedrock | Thấp | Thấp | AWS Budget alert ở $5/tháng; feature flag `ENABLE_CROSS_ENCODER=false` |
| RDS connection exhaustion | Cao | Trung bình | RDS Proxy xử lý pooling; read replica cho các query báo cáo |

#### Kế hoạch dự phòng

- Nếu Bedrock không khả dụng: fallback về bi-encoder only (đường dẫn `cross_score=None` đã được triển khai).
- Nếu ANN pgvector không trả về kết quả: hiển thị thông báo "Chưa có kết quả phù hợp — hãy kiểm tra lại sau khi thêm ứng viên".
- Sử dụng AWS CloudFormation/SAM để khôi phục mọi cấu hình Lambda từ IaC trong dưới 10 phút.

---

### 8. Kết quả kỳ vọng

#### Deliverables kỹ thuật

- Pipeline đối sánh serverless được triển khai đầy đủ với hiệu chỉnh điểm số có thể đo lường: **Technical Lead finalScore ≥ 70%, Sustainability Intern finalScore ≤ 35%, chênh lệch ≥ 30 điểm** giữa đối sánh tốt nhất và kém nhất.
- Đối sánh hai chiều thời gian thực với AppSync WebSocket push — kết quả được giao trong vòng 90 giây sau khi tải CV.
- Sàng lọc mù (spaCy NER) áp dụng trước khi bất kỳ mô hình AI nào nhìn thấy văn bản CV.
- Hướng dẫn phỏng vấn có cấu trúc (3 câu hỏi mỗi ứng viên) được tạo tự động và lưu trong DynamoDB.
- Khả năng quan sát đầy đủ: X-Ray traces, CloudWatch custom dashboards, structured JSON logs mỗi lần xử lý.

#### Giá trị dài hạn

Kiến trúc có thể tái sử dụng cho bất kỳ trường hợp đối sánh tài liệu nào (xem xét hợp đồng pháp lý, đối sánh bài báo học thuật, căn chỉnh thông số sản phẩm). Pool nhân tài pgvector và engine hai chiều có thể đóng vai trò nền tảng cho một sản phẩm SaaS tuyển dụng cấp production. Phương pháp hiệu chỉnh điểm số (kế hoạch 5 bước: sửa cosine → calibrated sigmoid → unified aggregation → chunk lớn hơn → tinh chỉnh weights) được ghi chép và có thể chuyển giao cho bất kỳ hệ thống hybrid retrieval trong tương lai nào xây dựng trên Bedrock + pgvector.
