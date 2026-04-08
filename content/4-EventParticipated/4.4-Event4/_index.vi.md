---
title: "Cloud Mastery Series"
date: 2026-04-04
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Bài thu hoạch: Cloud Mastery Series

### Mục Đích Của Sự Kiện

Cloud Mastery Series được xây dựng nhằm mang đến một trải nghiệm học tập sâu hơn và thực tế hơn về cả DevOps lẫn Generative AI. So với các buổi giới thiệu ngắn, chuỗi sự kiện này tập trung vào kiến thức nền tảng và các khái niệm có thể áp dụng trực tiếp trong quá trình thực tập cũng như trong các dự án kỹ thuật sau này.

Điều khiến series này đặc biệt hữu ích với em là sự đa dạng của các chủ đề. Chuỗi buổi học kết hợp giữa hạ tầng dưới dạng mã, mô phỏng cloud cục bộ, Kubernetes, một ngôn ngữ hàm phục vụ DevOps, thiết kế prompt cho AI, AIoT và các ứng dụng theo hướng agent. Sự kết hợp đó giúp em thấy rõ hệ sinh thái cloud hiện đại rộng lớn đến mức nào.

### Nội Dung Nổi Bật

#### Track DevOps

##### 1. Infrastructure as Code với Terraform trên AWS
Chủ đề đầu tiên và cũng rất quan trọng là Infrastructure as Code. Workshop giải thích vì sao việc cấu hình thủ công bằng click không phải là lựa chọn tối ưu cho các dự án thực tế, dù có thể hữu ích khi mới học. Em học được rằng IaC giúp tăng tính tự động hóa, khả năng tái tạo, khả năng mở rộng và hiệu quả cộng tác trong team.

Buổi học cũng so sánh giữa AWS CloudFormation, AWS CDK và Terraform. Nhờ đó, em hiểu hơn khi nào nên dùng từng công cụ và cách các đội ngũ kỹ thuật khác nhau quản lý hạ tầng. Phần giải thích về cấu trúc thư mục Terraform, các lệnh như init, validate, plan, apply, destroy và khái niệm state tracking đều rất thực tế.

##### 2. LocalStack cho kiểm thử cục bộ
Một chủ đề hữu ích khác là LocalStack. Nội dung này giúp em hiểu rằng quá trình phát triển không phải lúc nào cũng cần phụ thuộc vào cloud thật. Bằng cách mô phỏng các dịch vụ AWS ngay trên máy local, lập trình viên có thể kiểm thử nhanh hơn, giảm chi phí và tránh được việc phải triển khai lên cloud liên tục.

##### 3. Kubernetes Fundamentals
Buổi học về Kubernetes rất giá trị vì đã giới thiệu container orchestration theo một cách có hệ thống. Em học được sự khác nhau giữa việc chạy container thủ công và việc quản lý chúng ở quy mô lớn trên một cluster. Nội dung bao gồm control plane, worker node, Pod, ReplicaSet, Deployment, ConfigMap, Secret và Job.

Điều em thấy hữu ích nhất là cách giải thích vì sao Kubernetes quan trọng đối với self-healing, scaling, rolling update và tính nhất quán trong vận hành. Dù một số dự án có thể sử dụng kiến trúc serverless, việc hiểu Kubernetes vẫn rất cần thiết vì đây là một trong những mô hình triển khai phổ biến nhất trong môi trường cloud.

##### 4. Elixir trong DevOps
Buổi Elixir mở ra cho em một góc nhìn mới về độ tin cậy của backend và hạ tầng. Em học được rằng Elixir, chạy trên BEAM virtual machine, được thiết kế cho khả năng xử lý đồng thời và chịu lỗi rất tốt. Các khái niệm như lightweight processes, supervision tree và triết lý “let it crash” đặc biệt thú vị.

Buổi này cho em thấy rằng việc lựa chọn ngôn ngữ lập trình không chỉ ảnh hưởng đến tốc độ phát triển mà còn ảnh hưởng đến độ bền vững và hành vi vận hành của hệ thống.

#### Track Generative AI

##### 5. Prompt Engineering
Buổi về prompt engineering giúp em hiểu rằng chất lượng đầu ra của AI phụ thuộc rất nhiều vào cách prompt được cấu trúc. Em nhận ra prompt writing là một kỹ năng kỹ thuật thật sự, chứ không chỉ là cách nói chuyện bình thường với AI. Prompt có cấu trúc tốt sẽ giúp kết quả đầu ra chính xác và ổn định hơn.

##### 6. AIoT Projects
Phần AIoT cho em thấy cloud có thể kết hợp với phần cứng và tự động hóa như thế nào. Chủ đề này nhắc em rằng hệ thống cloud không chỉ dùng cho ứng dụng web, mà còn có thể phục vụ các thiết bị thông minh, hệ thống giám sát và các bài toán tự động hóa trong thế giới vật lý.

##### 7. Strands Agent và AI Agents
Buổi học về Strands Agent giúp em hiểu rõ hơn hướng phát triển của AI theo mô hình agent. Em học được rằng AI agent đang trở thành một khái niệm trung tâm trong thế hệ phần mềm tiếp theo vì nó có thể thực hiện hành động, phối hợp nhiệm vụ và tích hợp với các công cụ bên ngoài.

### Những Gì Học Được

Từ Cloud Mastery Series, em có được một nền tảng vững hơn về cloud và DevOps. Em học được rằng:

- hạ tầng nên được quản lý bằng code;
- công cụ kiểm thử local có thể giúp tăng tốc phát triển rất nhiều;
- Kubernetes vẫn là một nền tảng orchestration cực kỳ quan trọng;
- thiết kế ngôn ngữ có thể ảnh hưởng đến concurrency, fault tolerance và độ tin cậy;
- prompt engineering là một kỹ năng quan trọng trong việc xây dựng ứng dụng GenAI;
- AIoT và AI agent là hướng phát triển quan trọng của tự động hóa dựa trên cloud.

### Bài học rút ra

Bài học quan trọng nhất đối với em là kiến thức cloud không phải là một chủ đề đơn lẻ. Nó là cả một hệ sinh thái bao gồm hạ tầng, triển khai, observability, kiến trúc ứng dụng và tích hợp AI. Em cũng học được rằng nắm thật chắc nền tảng là điều rất quan trọng, vì những hệ thống nâng cao đều được xây dựng từ chính những khái niệm cơ bản đó.

### Một số hình ảnh khi tham gia sự kiện

- [Chèn ảnh buổi IaC/Terraform tại đây]
- [Chèn ảnh buổi Kubernetes tại đây]
- [Chèn ảnh buổi Elixir hoặc GenAI tại đây]

> Tổng thể, Cloud Mastery Series giúp tôi xây dựng nền tảng kỹ thuật vững hơn và cho tôi thấy cách DevOps và Generative AI có thể được học và áp dụng theo hướng rất thực tế nhưng vẫn bắt kịp xu hướng tương lai.

### Kết Luận

Cloud Mastery Series là một trải nghiệm học tập rất giá trị vì nó kết hợp nhiều chủ đề có liên quan trực tiếp đến cloud engineering hiện đại. Từ Terraform và Kubernetes đến Elixir và prompt engineering, mỗi buổi học đều góp phần tạo nên một bức tranh đầy đủ hơn về cách các hệ thống hiện đại được thiết kế, triển khai và mở rộng.
