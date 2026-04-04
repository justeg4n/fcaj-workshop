---
title: "Blog 1"
date: 2026-03-28
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Giới thiệu hỗ trợ OpenTelemetry & PromQL trong Amazon CloudWatch
*Tác giả: Rodrigue Koffi vào ngày 03 tháng 04 năm 2026*

Nếu bạn đang vận hành các khối lượng công việc (workloads) Kubernetes hoặc microservices trên AWS, các số liệu (metrics) của bạn rất có thể mang theo hàng tá nhãn (labels): không gian tên (namespace), pod, container, node, deployment, replica set, và các chiều dữ liệu nghiệp vụ tùy chỉnh. Để có được một bức tranh toàn cảnh về môi trường của mình, bạn có thể đang phải chia tách pipeline số liệu: Amazon CloudWatch cho các số liệu AWS, và một backend tương thích với Prometheus riêng biệt cho các số liệu ứng dụng và container có độ đa dạng dữ liệu cao (high-cardinality - nhiều tổ hợp nhãn độc nhất). Một số đội ngũ còn tiến xa hơn. Họ sử dụng các Prometheus CloudWatch exporters để kéo số liệu tài nguyên AWS vào backend Prometheus của họ thông qua các lệnh gọi API `GetMetricData`. Điều này làm tăng gánh nặng vận hành và chi phí, nhưng lại cho phép họ truy vấn mọi thứ tại một nơi duy nhất.

Amazon CloudWatch hiện tại đã hỗ trợ tiếp nhận trực tiếp các [số liệu OpenTelemetry](https://aws.amazon.com/otel) và cho phép truy vấn chúng bằng Ngôn ngữ Truy vấn Prometheus (PromQL). Tính năng đang trong giai đoạn xem trước (preview) này giới thiệu một kho lưu trữ số liệu high-cardinality hỗ trợ lên tới 150 nhãn cho mỗi số liệu, nhờ đó bạn có thể gửi các số liệu phong phú, dày đặc nhãn trực tiếp đến CloudWatch mà không cần chuyển đổi hay cắt xén. Kết hợp với tính năng tự động làm giàu (enrichment) số liệu do AWS cung cấp, CloudWatch trở thành một điểm đến duy nhất cho các số liệu hạ tầng, container, và ứng dụng, tất cả đều có thể truy vấn bằng PromQL.

Trong bài viết này, bạn sẽ học cách:
* Kích hoạt việc tiếp nhận số liệu OpenTelemetry và tự động làm giàu tài nguyên AWS trong tài khoản của bạn.
* Triển khai Amazon CloudWatch Container Insights trên một cụm Amazon Elastic Kubernetes Service (Amazon EKS).
* Truy vấn các số liệu hạ tầng và tài nguyên AWS bằng PromQL trong Amazon CloudWatch và Amazon Managed Grafana.
* Tạo các số liệu ứng dụng tùy chỉnh trong Amazon CloudWatch bằng OpenTelemetry SDK và xem chúng tự động được làm giàu với bối cảnh của AWS.

## Việc hỗ trợ OpenTelemetry có ý nghĩa gì đối với Amazon CloudWatch
Giao thức OpenTelemetry (OTLP) là giao thức truyền tải tiêu chuẩn cho dự án OpenTelemetry (OTel). Nó định nghĩa cách dữ liệu viễn trắc (telemetry), bao gồm số liệu, dấu vết (traces), và nhật ký (logs), được mã hóa và vận chuyển giữa các thành phần. Với bản xem trước này, CloudWatch mở ra một endpoint OTLP theo khu vực (regional endpoint) mà các bộ thu thập (collectors) hoặc SDK tương thích với OpenTelemetry có thể gửi số liệu tới.

CloudWatch tiếp nhận số liệu và lưu trữ chúng trong một kho lưu trữ số liệu high-cardinality mới, giữ lại nguyên vẹn các loại số liệu OpenTelemetry, bao gồm counters (bộ đếm), histograms (biểu đồ tần suất), gauges (thước đo), và up-down counters, mà không cần bất kỳ sự chuyển đổi nào. Với đợt ra mắt này, CloudWatch hoàn thiện việc hỗ trợ OpenTelemetry trên cả ba trụ cột của khả năng quan sát (observability). CloudWatch vốn đã chấp nhận traces và logs thông qua các endpoint OTLP của nó, việc bổ sung khả năng tiếp nhận số liệu OTLP gốc có nghĩa là giờ đây bạn có thể gửi toàn bộ dữ liệu viễn trắc của mình đến CloudWatch bằng các tiêu chuẩn mở, thông qua một giao thức duy nhất. Có ba khả năng làm cho điều này trở nên quan trọng:

1. **Hỗ trợ nhãn và cardinality mở rộng.** Các số liệu được tiếp nhận qua OTLP hỗ trợ lên tới 150 nhãn, so với giới hạn 30 chiều của các số liệu tùy chỉnh (custom metrics) trong CloudWatch. Điều này xóa bỏ một ràng buộc then chốt đối với Kubernetes, microservice, và các workload OpenTelemetry vốn phụ thuộc vào các nhãn high-cardinality để lọc và tổng hợp.
2. **Hỗ trợ truy vấn PromQL.** Bạn có thể truy vấn các số liệu được tiếp nhận thông qua OTLP bằng PromQL. Nếu bạn đã quen sử dụng Prometheus, bạn có thể dùng chính ngôn ngữ truy vấn đó trực tiếp trong CloudWatch và Amazon Managed Grafana, không cần học cú pháp mới.
3. **Tự động làm giàu tài nguyên AWS.** Khả năng này thay đổi căn bản cách bạn truy vấn và lọc số liệu xuyên suốt hạ tầng AWS của mình. CloudWatch làm giàu mọi số liệu được tiếp nhận bằng bối cảnh tài nguyên AWS: ID tài khoản, Khu vực (Region), Amazon Resource Name (ARN) của cụm, và các thẻ (tags) tài nguyên từ AWS Resource Explorer. Việc làm giàu này diễn ra tự động, không cần bất kỳ sự can thiệp cài đặt công cụ (instrumentation) nào thêm. Bạn có thể lọc và nhóm số liệu theo tài khoản AWS, Region, thẻ môi trường, hoặc tên ứng dụng, bất kể chúng đến từ Container Insights, một ứng dụng tùy chỉnh, hay một dịch vụ AWS. Không cần exporters, không cần nhãn tùy chỉnh, không cần thêm API calls.

*(Hình 1: Kiến trúc tiếp nhận và làm giàu số liệu OpenTelemetry trong Amazon CloudWatch)*

## Kích hoạt tiếp nhận OTLP và làm giàu tài nguyên AWS
Trước khi bạn có thể tiếp nhận và truy vấn số liệu OTLP, hãy kích hoạt hai thiết lập cấp tài khoản. Thiết lập đầu tiên cho phép lan truyền các thẻ (tags) tài nguyên từ các tài nguyên AWS sang dữ liệu viễn trắc của bạn, chính là các thẻ bạn thấy trong AWS Resource Explorer. Thiết lập thứ hai kích hoạt tính năng tiếp nhận OTLP cho CloudWatch.

Bạn có thể kích hoạt cả hai thiết lập làm giàu này từ AWS Management Console hoặc sử dụng AWS CLI.

**Sử dụng AWS CLI**
Chạy các lệnh sau:
```bash
# Bật các thẻ tài nguyên trên telemetry
aws observabilityadmin start-telemetry-enrichment

# Bật OTel enrichment cho CloudWatch
aws cloudwatch start-o-tel-enrichment