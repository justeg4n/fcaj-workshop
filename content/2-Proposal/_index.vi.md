---
title: "Đề xuất Dự án"
weight: 1
chapter: false
---

# Đề xuất (Proposal)
Phần này tóm tắt dự án/workshop dự kiến cho **SmartHire-AI**, được điều chỉnh phù hợp với cơ sở hạ tầng đã được định nghĩa bằng Terraform trong repository này.

## SmartHire-AI
**Nền tảng Tuyển dụng Thông minh trên AWS (Serverless + IaC)**

### 1. Tóm tắt dự án (Executive Summary)
SmartHire-AI được thiết kế dành cho cả ứng viên và nhà tuyển dụng. Ứng viên tải CV (định dạng PDF) lên object storage; hệ thống trích xuất văn bản, làm giàu dữ liệu bằng AI (Amazon Bedrock, Amazon Textract, Amazon Comprehend), lưu trữ trạng thái phân tích và kết quả, sau đó tạo ra các đề xuất công việc hoặc xếp hạng ứng viên.

Đối với **luồng JD (Mô tả công việc)**, nội dung được đọc trực tiếp từ cơ sở dữ liệu quan hệ (Amazon RDS PostgreSQL) bởi backend **.NET 8**, backend này sẽ trực tiếp khởi chạy AWS Step Functions khi có sự kiện tạo mới/cập nhật JD. Điều này loại bỏ sự phụ thuộc vào việc phải tải file JD lên S3 cho luồng này.

Web client (SPA) được phân phối qua **Amazon CloudFront + Amazon S3**; việc xác thực được xử lý bởi **Amazon Cognito** (bao gồm cả đăng nhập bằng Google); và các bản cập nhật thời gian thực (near real-time) cho client được phân phối thông qua **AWS AppSync** (GraphQL subscriptions). Toàn bộ môi trường được mô tả dưới dạng mã nguồn bằng **Terraform** (VPC, RDS, Cognito, luồng xử lý, X-Ray tracing, CI/CD, v.v.).

### 2. Phát biểu bài toán (Problem Statement)
**Vấn đề là gì?**
Việc sàng lọc CV và đối chiếu JD thủ công không thể mở rộng khi số lượng hồ sơ và tin tuyển dụng tăng lên. Các team thường thiếu một luồng xử lý (pipeline) thống nhất để trích xuất, lưu trữ và đề xuất; người dùng cũng cần phản hồi gần như theo thời gian thực thay vì phải tải lại trang để cập nhật. Các giải pháp SaaS bên ngoài khó tùy chỉnh hơn và thường ít được tích hợp sâu với dữ liệu nội bộ dựa trên RDS.

**Giải pháp đề xuất**
Giải pháp sử dụng kiến trúc AWS đa tầng:
* Tiếp nhận CV qua **S3** (prefix `candidates/`) với cơ chế thông báo **S3 -> SQS** chỉ giới hạn cho các object `.pdf`.
* Lambda `ingestion_trigger` tiêu thụ tin nhắn từ queue và gọi `StartExecution` trên Step Functions.
* State machine điều phối `cv_jd_processor` (Python 3.12), sau đó rẽ nhánh sang `job_suggestion_engine` hoặc `candidate_ranking_engine` (Lambda chạy bằng container image từ **Amazon ECR**).
* Kết quả đầu ra được ghi vào **DynamoDB** (thiết kế single-table để tracking ứng viên) và được publish thông qua **AppSync**.
* Business API được cung cấp bởi **API Gateway + Lambda .NET 8** nằm trong VPC, truy cập RDS qua **AWS Secrets Manager** và xác thực JWT thông qua **Cognito**.
* Frontend được lưu trữ trên S3 đứng sau CloudFront, với tùy chọn bảo mật **AWS WAF** ở lớp CDN.
* Việc triển khai tự động được xử lý bởi **CodePipeline / CodeBuild** kết nối với GitHub thông qua **CodeStar Connections**.

Kiến trúc này được xây dựng hướng tới khả năng bảo trì, kiểm soát chi phí dự đoán được và triển khai lặp lại thông qua IaC.

**Lợi ích và ROI**
SmartHire-AI giảm bớt công việc thủ công cho nhà tuyển dụng và ứng viên, chuẩn hóa siêu dữ liệu (metadata) CV/JD và cung cấp nền tảng thời gian thực cho các dashboard. Chi phí vận hành được ước tính chính xác bằng **Infracost** thông qua file `infracost.yml`, mô phỏng và tính toán chi phí cho ba môi trường (DEV, STAGING, PROD) bằng cách thay đổi các biến hạ tầng (xem Phần 6).

ROI (Tỷ suất hoàn vốn) mong đợi chủ yếu là về mặt vận hành: thời gian xử lý ngắn hơn, độ trễ phản hồi UI thấp hơn và dễ dàng mở rộng (scale-out) với IaC thay vì phải thao tác thủ công từng dịch vụ.

### 3. Kiến trúc Giải pháp (Solution Architecture)
Nền tảng được triển khai tại `ap-southeast-1` theo mặc định (cấu hình Terraform), với một VPC riêng biệt (public/private subnets), RDS PostgreSQL nằm ở private subnet và tùy chọn sử dụng bastion/RDS Proxy. Luồng xử lý CV là S3 -> SQS -> Lambda -> Step Functions -> Xử lý AI/Vector (dựa trên thiết kế pipeline nội bộ thống nhất). Luồng xử lý JD được kích hoạt bởi API .NET, đọc `Jobs.Description` từ RDS và gọi Step Functions. Các cảnh báo vận hành (độ sâu của queue, lỗi Lambda, thời gian phản hồi p95) có thể được kích hoạt thông qua **Amazon CloudWatch** và **Amazon SNS**.

**Sơ đồ luồng xử lý CV/JD**
![CV/JD Flow Diagram](D:\FPTU\AWS\architecture.png)
*Luồng xử lý song song cho CV và JD.*

**Các Dịch vụ AWS Được Sử dụng**
* **Amazon VPC:** Mạng ảo, subnets, security groups; tùy chọn VPC interface endpoints (Secrets Manager, Cognito, Bedrock Runtime) và các chế độ ra internet (`endpoint_only`, `shared_bastion_nat`, `nat_gateway`).
* **Amazon RDS (PostgreSQL):** Datastore quan hệ chính; thông tin xác thực được quản lý qua **Secrets Manager + KMS**; tùy chọn **RDS Proxy** và bastion host.
* **Amazon Cognito:** User pool, app client, identity pool; các Lambda triggers `pre_sign_up` / `post_confirmation`; xác thực qua Google.
* **Amazon S3:** Bucket Frontend (OAC với CloudFront) và bucket lưu file thô (`candidates/`, `jobs/`) có tính năng versioning/lifecycle và mã hóa SSE; Thông báo S3 -> SQS khi có file PDF.
* **Amazon SQS + DLQ:** Queue xử lý CV và dead-letter queue.
* **AWS Lambda:** `ingestion_trigger`, `cv_jd_processor` (ZIP), `job_suggestion_engine` / `candidate_ranking_engine` (ECR images), và các hàm liên quan đến Cognito.
* **Amazon ECR:** Kho chứa container cho các engine đối chiếu.
* **AWS Step Functions:** State machine thống nhất điều phối luồng CV/JD.
* **Amazon DynamoDB:** Tracking thông tin (ứng dụng thiết kế single-table, tùy chọn PITR/TTL tùy cấu hình).
* **AWS AppSync:** GraphQL API, datasource là DynamoDB, các resolvers hỗ trợ subscription cho job suggestion và candidate ranking.
* **Amazon Textract, Amazon Bedrock, Amazon Comprehend:** Trích xuất văn bản, mô hình tạo sinh/embedding, và xử lý thực thể/PII (tuân thủ IAM policy).
* **Amazon API Gateway:** REST API, CORS, Cognito authorizer.
* **AWS X-Ray:** Tracing end-to-end từ đầu đến cuối xuyên suốt các lớp AppSync, Lambda, và RDS.
* **Amazon CloudFront + AWS WAF** (ARN tùy chọn): Phân phối frontend và bảo vệ biên (edge protection), hỗ trợ custom domain.
* **Amazon Route 53 + ACM:** Quản lý DNS và chứng chỉ TLS.
* **AWS CodePipeline, AWS CodeBuild, CodeStar Connections:** CI/CD, đóng gói S3 artifacts, build/push ECR.
* **Amazon CloudWatch + Amazon SNS:** Logging và thông báo cảnh báo.
* **Terraform:** Công cụ Infrastructure as Code chính cho toàn bộ môi trường.

**Thiết kế Component**
* **Client:** SPA (React + TypeScript + Vite), xác thực Cognito, gọi request tới API Gateway, nhận AppSync subscriptions.
* **Phân phối biên (Edge):** CloudFront -> S3 frontend; HTTPS và tùy chọn WAF tại CDN edge.
* **Lớp API:** API Gateway + Lambda .NET (có tích hợp X-Ray) nằm trong VPC, Cognito authorizer, kết nối với RDS và Step Functions.
* **Đầu vào (CV):** Upload PDF lên S3 `candidates/` -> SQS -> ingestion Lambda -> Step Functions.
* **Đầu vào (JD):** Backend ghi văn bản JD vào RDS; event business gọi `StartExecution` (không dùng S3 notification cho JD).
* **Xử lý:** Step Functions gọi `cv_jd_processor`, điều hướng tới engine đối chiếu, ghi trạng thái vào DynamoDB, và publish qua AppSync mutations.
* **Lưu trữ (Persistence):** RDS cho dữ liệu quan hệ, DynamoDB để tracking/realtime, S3 cho file thô.
* **Định danh (Identity):** Cognito pool + xác thực Google; Lambda triggers cho luồng đăng ký.
* **CI/CD:** Pipeline dựa trên branch GitHub, tự động build/push container và deploy.

### 4. Triển khai Kỹ thuật (Technical Implementation)
**Các giai đoạn triển khai (khuyến nghị)**
1. **Thiết kế & IaC:** Hoàn thiện sơ đồ kiến trúc và biến Terraform (`project_name`, `environment`, `aws_region`, `egress_mode`, `domain`).
2. **Nền tảng Mạng & Dữ liệu:** Chạy `terraform apply` cho các module networking/database; cấu hình Secrets Manager; tùy chọn bastion/RDS Proxy.
3. **Xác thực & Hosting frontend:** Triển khai module xác thực (Cognito + Lambda triggers) và stack frontend (S3/CloudFront/Route 53/ACM).
4. **Luồng xử lý (Processing pipeline):** Cấu hình storage, queue, S3->SQS notification, và module xử lý (Lambda, Step Functions, DynamoDB, AppSync, alarms).
5. **API & Logic:** Triển khai API Gateway và Lambda .NET qua Terraform; xác thực tích hợp Secrets Manager và quyền IAM cho database.
6. **CI/CD:** Kết nối GitHub qua CodeStar Connections; cấu hình build/push ECR và release pipeline.
7. **Kiểm thử End-to-end:** Upload CV, tạo job, quan sát Step Functions/AppSync subscriptions, và kiểm tra telemetry trên CloudWatch.

**Yêu cầu kỹ thuật**
* **Terraform:** Quản lý state bảo mật; các biến nhạy cảm dùng `TF_VAR_` / `terraform.tfvars` (không commit secret lên git).
* **Python 3.12:** Pipeline Lambda (cấu hình handler/timeout/memory qua biến `cv_parser_*`).
* **.NET 8:** Lambda API backend nằm trong VPC, dùng IAM database auth cho RDS nếu có cấu hình.
* **React + TypeScript + Vite:** Phát triển frontend SPA hiện đại, type-safe.
* **Docker:** Build image cho `job_suggestion_engine` và `candidate_ranking_engine`, push lên ECR.
* **AWS CLI:** Triển khai tài nguyên và khắc phục sự cố.
* **Kiến thức thực tiễn:** Cognito (OAuth/OIDC), AppSync GraphQL, Step Functions ASL, các pattern S3 notification + SQS policy, và AWS X-Ray tracing.

### 5. Tiến độ & Cột mốc (Timeline & Milestones)
* **Tháng 1 & 2 (Học tập & Nghiên cứu):** Tìm hiểu sâu các dịch vụ AWS, review kiến trúc, kiểm tra hạn mức (quotas) của Bedrock/Textract, và chuẩn bị tài khoản AWS/domain.
* **Tháng 3 (Triển khai):** Xây dựng nền tảng (VPC, RDS, Cognito, S3/CF), hoàn thiện luồng xử lý serverless (Lambda, Step Functions, DynamoDB, AppSync), tích hợp CI/CD, và golive.
* **Sau golive:** Tối ưu chi phí, vận hành DLQ/alarm, và phát triển thêm tính năng theo backlog.

### 6. Dự toán Ngân sách (Budget Estimate)
Dưới đây là ước tính ngân sách hoàn chỉnh cho SmartHire-AI, được trích xuất từ cấu hình **Infracost** (`infracost.yml`) mô phỏng ba môi trường riêng biệt (DEV, STAGING, PROD). Bảng dự toán bao gồm các dịch vụ mạng (WAF, CloudFront), hạ tầng cố định (NAT, RDS, Proxy) và các chi phí dựa trên mức sử dụng.

#### 6.1. Tóm tắt Ngân sách Hàng tháng
Bảng dưới đây tóm tắt các khoản chi phí cố định hàng tháng theo môi trường:

| Component (Thành phần) | DEV (Tiết kiệm tối đa) | STAGING (Cân bằng) | PROD (HA/Bảo mật tiêu chuẩn) |
| :--- | :--- | :--- | :--- |
| RDS/NAT/Compute Core | $36.20 | $65.10 | $256.66 |
| RDS Proxy | $0.00 | $21.90 | $21.90 |
| AWS WAF (Tường lửa) | $0.00 | $0.00 | $6.00 |
| CloudFront (Free Tier) | $0.00 | $0.00 | $0.00 |
| **TỔNG CHI PHÍ CỐ ĐỊNH** | **$36.20/tháng** | **$87.00/tháng** | **$284.56/tháng** |

#### 6.2. Chi tiết từng môi trường

**1. Môi trường DEV**
Cấu hình: 1 NAT Instance (t3.micro), 1 AZ, không dùng Proxy/WAF.
**Tổng chi phí cố định: $36.20/tháng**

| Dịch vụ / Tài nguyên | Chi phí hàng tháng | Chi tiết Cấu hình |
| :--- | :--- | :--- |
| Amazon RDS | $23.20 | (db.t3.micro, 20 GB gp3) |
| NAT Instance | $10.60 | (t3.micro, 8 GB gp2) |
| Khác (KMS, Alarms, Secret) | $2.40 | (1 key, 10 alarms, 1 secret) |
| **TỔNG** | **$36.20** | |

**2. Môi trường STAGING**
Cấu hình: 1 NAT Instance (t3.medium), 2 AZs, có RDS Proxy.
**Tổng chi phí cố định: $87.00/tháng**

| Dịch vụ / Tài nguyên | Chi phí hàng tháng | Chi tiết Cấu hình |
| :--- | :--- | :--- |
| NAT Instance | $39.50 | (t3.medium, 8 GB gp2) |
| Amazon RDS | $23.20 | (db.t3.micro, 20 GB gp3) |
| Amazon RDS Proxy | $21.90 | (2 vCPUs pooling) |
| Khác (KMS, Alarms, Secret) | $2.40 | (1 key, 10 alarms, 1 secret) |
| **TỔNG** | **$87.00** | |

**3. Môi trường PROD**
Cấu hình: 2 NAT Gateways, có interface endpoints, 2 AZs, RDS Proxy, WAF.
**Tổng chi phí cố định: $284.56/tháng**

| Dịch vụ / Tài nguyên | Chi phí hàng tháng | Chi tiết Cấu hình |
| :--- | :--- | :--- |
| VPC Interface Endpoints | $113.88 | (6 service endpoints trải trên 2 AZs) |
| NAT Gateway | $86.14 | (2 gateways - chi phí thuê cố định) |
| Amazon RDS | $43.64 | (db.t3.small, 20 GB gp3) |
| Amazon RDS Proxy | $21.90 | (2 vCPUs pooling) |
| Bastion Host | $10.60 | (t3.micro, 8 GB gp2) |
| AWS WAF | $6.00 | (1 Web ACL + các luật managed phổ biến) |
| Khác (KMS, Alarms, Secret) | $2.40 | (1 key, 10 alarms, 1 secret) |
| **TỔNG** | **$284.56** | |

#### 6.3. Chi phí dựa trên mức sử dụng (Ước tính cho 1,000 CV/tháng)
Bảng dưới đây ước tính chi phí biến đổi tăng thêm khi xử lý **1,000 CV** mỗi tháng sử dụng các mô hình AI tiên tiến (Claude 3.5 Sonnet và Cohere Embed v3):

| Dịch vụ | Đơn giá | Ước lượng cho 1,000 CV | Chi phí dự kiến |
| :--- | :--- | :--- | :--- |
| Amazon Bedrock (Sonnet) | In: $3.00/M, Out: $15.00/M | ~2M tokens (Claude 3.5 Sonnet) | $18.00 |
| Amazon Bedrock (Cohere) | $0.10/1M tokens | ~1M tokens (Cohere Embed v3) | $0.10 |
| Amazon Textract | $1.50/1,000 trang | 1,000 trang CV (Detect Text) | $1.50 |
| AWS Lambda | $0.20/1M lần gọi | 10,000 calls (processing + auth/API) | $0.10 |
| AWS AppSync | $4.00/1M operations | 5,000 Queries/Mutations | $0.02 |
| AWS Cognito | Free Tier (50k MAUs) | < 1,000 user hoạt động | $0.00 |
| Các dịch vụ khác (S3/DB/SQS)| Lưu trữ và nhắn tin | File and state management | $0.17 |
| **TỔNG CHI PHÍ BIẾN ĐỔI** | | **~$19.84 / 1,000 CVs** | |

**Ghi chú:**
* **Claude 3.5 Sonnet** có thể mang lại chất lượng đối chiếu cao hơn, nhưng chi phí suy luận cao hơn rõ rệt so với các giải pháp nhẹ hơn.
* **Cohere Embed v3** vẫn cực kỳ tiết kiệm chi phí cho các luồng công việc tìm kiếm vector.
* Chi phí biến đổi trung bình cho mỗi CV được xử lý là khoảng **$0.02**.
* Ước tính chi phí vận hành PROD trong tháng đầu tiên (bao gồm 1,000 CV): **~$304.40**
* Các giả định về giá được dựa trên mức giá công bố của AWS tại Singapore (ap-southeast-1) và Virginia (us-east-1) do mức giá mô hình có sự khác biệt.

### 7. Đánh giá rủi ro (Risk Assessment)
**Các rủi ro chính**
* Chạm trần hạn mức (quota limits) của dịch vụ AI (Bedrock, Textract) trong những thời điểm xử lý cao điểm.
* Các file PDF không thể đọc được hoặc Textract xuất kết quả chất lượng thấp.
* Số lượng tin nhắn SQS tồn đọng tăng cao và dồn ứ ở DLQ.
* Nguy cơ lộ lọt Secret (RDS, Cognito) do cấu hình quyền truy cập sai.
* Tăng vọt chi phí do NAT/data transfer và các lời gọi API mô hình AI quá thường xuyên.

**Phương án giảm thiểu**
* Cài đặt CloudWatch alarms cho độ sâu queue, lỗi/throttle Lambda, và p95 duration, kèm thông báo qua SNS.
* Sử dụng DLQ + chính sách retry và phân tích dead-letter trước khi replay (gửi lại).
* Mã hóa Secrets Manager + KMS, tuân thủ nguyên tắc least-privilege của IAM và thắt chặt security groups.
* Đặt budget alarms và định kỳ đánh giá lại chi phí/kiến trúc.
* Cho phép cấu hình fallback sang các mô hình AI chi phí thấp hơn khi cần thiết.

### 8. Kết quả mong đợi (Expected Outcomes)
**Cải tiến kỹ thuật**
* Một luồng xử lý CV/JD thống nhất được điều phối bởi Step Functions, giúp giảm bớt các bước Lambda thừa và độ trễ end-to-end.
* Giao diện người dùng được cập nhật thời gian thực thông qua AppSync subscriptions (`onJobSuggestions`, `onCandidateRanking`).
* Thiết kế single-table DynamoDB cho khả năng tracking ứng viên, có thể mở rộng dễ dàng với các query pattern mới (thông qua GSI).

**Giá trị dài hạn**
* Khả năng triển khai hạ tầng có thể tái tạo (repeatable) và dễ dàng kiểm toán thông qua Terraform.
* Dữ liệu tuyển dụng trong lịch sử được cấu trúc tốt hơn, phục vụ cho phân tích và cải thiện mô hình AI trong tương lai.
* Khả năng quan sát (observability) vận hành mạnh mẽ hơn thông qua các hệ thống tập trung về logs, metrics, và alarms.
