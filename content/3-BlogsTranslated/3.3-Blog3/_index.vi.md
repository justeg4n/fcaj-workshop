---
title: "Blog 3"
date: 2026-03-09
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Tự động hóa kích hoạt AWS Systems Manager để đăng ký node quản lý lai (hybrid-managed)
*Tác giả: Justin Thomas vào ngày 09 tháng 03 năm 2026*

AWS Systems Manager (trước đây được gọi là SSM) là một dịch vụ của AWS mà bạn có thể sử dụng để xem và kiểm soát các máy chủ của mình trên đám mây AWS và trên hạ tầng tại chỗ (on-premises). Systems Manager giúp việc quản lý một môi trường lai (hybrid environment) trở nên dễ dàng.

Để thiết lập các máy chủ và máy ảo (VM) trong môi trường lai của bạn thành các instances được Systems Manager quản lý, bạn cần tạo một quá trình kích hoạt phiên bản được quản lý (managed-instance activation). Việc tạo và quản lý thông tin xác thực (credentials) Systems Manager Hybrid Activations cho các máy chủ và máy ảo tại chỗ của bạn có thể là một nhiệm vụ thủ công và tẻ nhạt. Thông tin xác thực Hybrid Activations có thể đạt đến ngày hết hạn kích hoạt hoặc giá trị giới hạn đăng ký, sau thời điểm đó thông tin xác thực này không thể được sử dụng để đăng ký máy chủ nữa. Khi đó, thông tin xác thực sẽ cần phải được tạo lại một cách thủ công và các máy chủ mới phải được cấu hình để sử dụng thông tin xác thực mới này. Nhu cầu cốt lõi ở đây là tự động hóa việc tạo và quản lý thông tin xác thực System Manager Hybrid Activations, giúp giảm thiểu sự hỗ trợ vận hành cần thiết trong tác vụ này.

Trong bài viết này, tôi sẽ hướng dẫn các bạn giải pháp để tự động hóa việc tạo System Manager Hybrid Activations.

## Tổng quan giải pháp
Giải pháp này được kích hoạt bằng cách sử dụng AWS CloudFormation stack. CloudFormation stack sẽ tạo ra các tài nguyên AWS trên tài khoản của bạn cần thiết cho giải pháp. Các tài nguyên này như sau:
* **Amazon API Gateway:** REST API loại Riêng tư (Private), tích hợp với hàm AWS Lambda. Khi một web client từ máy chủ on-premises thực hiện yêu cầu GET tới API gateway, nó sẽ trả về tổ hợp Mã/ID của Hybrid Actions.
* **AWS Lambda:** Hàm Lambda cung cấp tổ hợp Mã/ID Hybrid Activations cho máy chủ on-premises thông qua API gateway. Nó sẽ tạo ra một mã kích hoạt mới nếu phát hiện mã kích hoạt hiện tại đã hết hạn hoặc đạt đến giới hạn đăng ký.
* **Amazon DynamoDB:** Dùng để lưu trữ trạng thái. Lambda cập nhật bảng thành trạng thái ‘Locked’ nếu nó đang phục vụ yêu cầu từ một client. Nó cập nhật bảng thành ‘Unlocked’ sau khi hoàn tất việc phục vụ yêu cầu.
* **Amazon VPC Endpoint:** VPC Endpoint cho API gateway dùng để truy cập riêng tư vào URL của API gateway từ mạng lưới on-premises.
* **AWS Systems Manager Parameter Store:** Dùng để lưu trữ ID/Mã Hybrid Activations.

## Hướng dẫn từng bước (Walkthrough)

### Điều kiện tiên quyết
Đối với hướng dẫn này, bạn cần có những điều kiện sau:
* Một tài khoản AWS.
* Một user/role của AWS Identity and Access Management (IAM) có khả năng: Tạo một API riêng tư, tạo một phương thức (method), và triển khai nó trên API Gateway; Tạo hàm Lambda, DynamoDB, Parameter Store Parameter, và AWS CloudWatch Log Group.
* VPC mà bạn định triển khai phải có cả hai thuộc tính VPC `enableDnsSupport` và `enableDnsHostnames` được đặt là `true`.

### Bước 1: Tạo VPC endpoint cho API Gateway
Trong bước đầu tiên, bạn tạo các VPC endpoint cho API Gateway trong VPC của bạn. Cùng với đó, bạn tạo một security group gắn vào endpoint để cho phép cổng TCP 443. 
*Tải CloudFormation Template > Tạo Stack > Lấy VPC endpoint ID.*

### Bước 2: Tạo Khóa KMS
Trong bước này, bạn sẽ tạo một khóa AWS Key Management Service (AWS KMS) để mã hóa Parameter Store (nơi được dùng để lưu trữ Mã Kích hoạt và ID Kích hoạt).

### Bước 3: Tạo API Gateway và Lambda
Trong bước cuối cùng, bạn sẽ tạo và triển khai một Private API và hàm Lambda thông qua CloudFormation Template.

Sau khi stack được tạo thành công, từ máy chủ on-premises cần được đăng ký, hãy truy cập vào URL đã sao chép bằng `curl`/`wget` hoặc bất kỳ web client nào khác. Tổ hợp ID/Mã Kích hoạt sẽ được trả về dưới định dạng JSON.

Ví dụ cho Linux script để lấy thông tin xác thực và cài đặt tự động:
```bash
#!/bin/bash
sudo yum erase amazon-ssm-agent --assumeyes
credentials=$(curl -s https://[YOUR_API_URL]/lambdastage)
activationcode=$(echo $credentials | jq -r '.ActivationCode')
activationid=$(echo $credentials| jq -r '.ActivationId') 
mkdir /tmp/ssm
curl [https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm](https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm) -o /tmp/ssm/amazon-ssm-agent.rpm
sudo dnf install -y /tmp/ssm/amazon-ssm-agent.rpm
sudo systemctl stop amazon-ssm-agent
sudo -E amazon-ssm-agent -register -code $activationcode -id $activationid -region us-east-1
sudo systemctl start amazon-ssm-agent