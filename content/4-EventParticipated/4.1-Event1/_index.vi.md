---
title: "AWS re:Invent 2025 re:Cap in HCMC"
date: 2026-01-29
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

<<<<<<< HEAD
# Bài thu hoạch: AWS re:Invent 2025 re:Cap in HCMC
=======
# Bài thu hoạch “GenAI-powered App-DB Modernization workshop”
>>>>>>> 5404d32071b538ecf9145bac7ee52b413b84184c

### Mục Đích Của Sự Kiện

Sự kiện này được tổ chức nhằm mang những thông báo quan trọng nhất và các xu hướng kiến trúc mới nhất từ AWS re:Invent đến gần hơn với cộng đồng công nghệ tại TP. Hồ Chí Minh. Với vai trò là người tham dự, em mong muốn cập nhật thêm kiến thức về các dịch vụ AWS mới, quan sát cách AWS giải quyết các bài toán hạ tầng thực tế, và hiểu rõ hơn cách kiến trúc cloud hiện đại đang phát triển theo hướng hiệu quả hơn, tiết kiệm hơn và an toàn hơn.

Điều làm cho sự kiện này đặc biệt hữu ích là nội dung không dừng ở mức giới thiệu sản phẩm, mà tập trung nhiều vào các chủ đề rất thực tế như tối ưu lưu trữ, tối ưu năng lực tính toán, quản lý chi phí cơ sở dữ liệu, tăng cường bảo mật tại edge, và cải thiện kết nối cho người dùng tại Việt Nam.

### Nội Dung Nổi Bật

#### App-DB Modernization Workshop
Phần workshop giúp em nhìn rõ hơn cách hiện đại hóa ứng dụng và cơ sở dữ liệu theo hướng thiết kế lại tổng thể thay vì chỉ vá lỗi cục bộ. Các diễn giả nhấn mạnh rằng thay vì giữ toàn bộ hệ thống trong một khối monolithic cồng kềnh, đội ngũ kỹ thuật nên tư duy theo hướng chia nhỏ thành các dịch vụ độc lập, dễ quản lý, dễ mở rộng và dễ triển khai hơn.

Điều em thấy hữu ích nhất là tư duy kết hợp giữa phân rã hệ thống, thiết kế hướng sự kiện (event-driven) và mô hình serverless. Em cũng học được cách các công cụ hỗ trợ lập trình bằng AI như Amazon Q Developer có thể giúp kỹ sư viết mã nhanh hơn, phát hiện lỗi sớm hơn và giảm bớt các công việc lặp đi lặp lại trong quá trình triển khai.

#### Storage và Data
Một trong những phần gây ấn tượng nhất với em là nội dung về lưu trữ dữ liệu. Em nhận ra AWS đang cải tiến rất nhiều ở cách dữ liệu được lưu trữ, truy xuất và tối ưu. Các tính năng như intelligent tiering hay tìm kiếm vector cho thấy storage ngày nay không chỉ là nơi để “để dữ liệu”, mà còn gắn chặt với hiệu năng, phân tích dữ liệu và các workload AI.

Em cũng chú ý đến phần quản lý truy cập theo tag. Việc dùng tag để quản lý quyền trên S3 là một cải tiến rất thực tế, đặc biệt khi nhiều team cùng làm việc trên cùng một nền tảng dữ liệu, vì nó giúp governance rõ ràng hơn và giảm bớt thao tác thủ công.

#### Servers và Running Applications
Phần compute cho thấy AWS vẫn đang liên tục nâng cấp hiệu năng và đơn giản hóa vận hành. Sự xuất hiện của các dòng Graviton mới cho thấy cloud platform ngày càng tối ưu hơn về giá trị hiệu năng trên chi phí. Em cũng thấy rất hữu ích khi AWS Backup đã hỗ trợ EKS, vì điều này giúp giảm nhu cầu viết các script backup thủ công phức tạp.

Một điểm em nhớ rõ là hệ thống cloud phải đảm bảo tính ổn định ngay cả khi có lỗi phát sinh. Những tính năng được giới thiệu trong sự kiện thể hiện rất rõ triết lý đó.

#### Networking và Security
Phần networking và security cũng rất thực tế. Em học được rằng bảo vệ tại edge đang ngày càng quan trọng, nhất là với các ứng dụng hiện đại có người dùng phân tán và nhiều workload nhạy cảm. Các cải tiến về bảo mật CloudFront, định hướng bảo vệ trước các mối đe dọa trong tương lai, và các gói giá cố định đều cho thấy cloud đang tiến đến mô hình vận hành ổn định, dễ dự đoán và an toàn hơn.

#### Databases
Phần cơ sở dữ liệu giúp em hiểu rằng tối ưu chi phí và tối ưu tốc độ truy xuất phải đi cùng nhau. AWS giới thiệu khả năng tiết kiệm chi phí cho RDS và DynamoDB nếu workload sử dụng đều đặn, đồng thời OpenSearch cũng trở nên thông minh hơn nhờ hỗ trợ GPU và khả năng hiểu truy vấn ngôn ngữ tự nhiên tốt hơn.

### Những Gì Học Được

Sau sự kiện này, em có cái nhìn rộng hơn về cách AWS định hình tương lai của hạ tầng cloud. Em học được rằng:

- hiện đại hóa không chỉ là thay hệ thống cũ bằng hệ thống mới, mà là thiết kế lại để linh hoạt hơn;
- lựa chọn cách lưu trữ phải gắn với chi phí, pattern truy cập và nhu cầu AI;
- nền tảng tính toán cần được đánh giá không chỉ bằng tốc độ mà còn bằng sự đơn giản trong vận hành và khả năng khôi phục;
- bảo mật ngày càng gắn chặt với kiến trúc edge và các thiết lập mặc định của nền tảng;
- database và search đang trở nên thông minh hơn, phù hợp hơn với từng loại workload.

### Bài học rút ra

Bài học lớn nhất mà em rút ra là cloud architecture ngày nay không còn đơn thuần là “đưa server lên mạng” nữa. Thay vào đó, đó là quá trình xây dựng các hệ thống có khả năng chịu lỗi, tiết kiệm chi phí, an toàn và thích ứng nhanh với thay đổi. Em cũng nhận ra rằng một kỹ sư giỏi không chỉ là người nhớ tên dịch vụ, mà quan trọng hơn là người hiểu mỗi dịch vụ đóng góp gì vào chiến lược tổng thể của hệ thống.

### Một số hình ảnh khi tham gia sự kiện

- [Chèn ảnh sự kiện tại đây]
- [Chèn ảnh giao lưu, networking tại đây]
- [Chèn ảnh workshop/slides tại đây]

> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp tôi thay đổi cách tư duy về thiết kế ứng dụng, hiện đại hóa hệ thống và phối hợp hiệu quả hơn giữa các team.

### Kết Luận

Việc tham gia AWS re:Invent 2025 re:Cap in HCMC giúp em có cái nhìn rất thực tế về định hướng phát triển của AWS. Sự kiện giúp em kết nối các xu hướng cloud mới với các quyết định vận hành cụ thể như kiểm soát chi phí, chiến lược scale, quản lý dữ liệu và lập kế hoạch bảo mật. Sau sự kiện, em cảm thấy mình hiểu rõ hơn về cách xây dựng kiến trúc cloud hiện đại và cũng nhận ra rằng việc học hỏi liên tục là điều bắt buộc trong lĩnh vực này.
