---

title: "Worklog Tuần 2"
date: 2026-04-27  
weight: 1  
chapter: false  
pre: " **1.2.** "
---


### Mục tiêu tuần 2:

- Làm quen với EC2 — dịch vụ compute nền tảng của AWS
- Hoàn thành architecture diagram và sequence diagram cho toàn bộ hệ thống HireFlow AI

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                               |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------------------------------------------------------------------------------------- |
| 2   | Làm lab EC2 đầu tiên: launch t2.micro Ubuntu 22.04, tạo key pair `.pem`, cấu hình Security Group chỉ mở port 22, SSH thành công, cài Nginx, đặt Budget alert 1 USD | 27/04/2026   | 27/04/2026      | [Giới thiệu về Amazon EC2 :: GIỚI THIỆU VỀ AMAZON EC2](https://000004.awsstudygroup.com/vi/) |
| 3   | - Triển khai ứng dụng với Auto Scaling: - Chuẩn bị EC2, tạo AMI & Launch Template - Tạo Target Group, cấu hình Load Balancer - Kết nối ứng dụng với RDS            | 28/04/2026   | 28/04/2026      | [https://000006.awsstudygroup.com/](https://000006.awsstudygroup.com/)                       |
| 4   | **-** Auto Scaling nâng cao: - Kiểm thử ứng dụng qua Load Balancer - Cấu hình Dynamic Scaling / Scheduled Scaling - Test tải và quan sát Auto Scaling hoạt động    | 29/04/2026   | 29/04/2026      | [https://000006.awsstudygroup.com/](https://000006.awsstudygroup.com/)                       |




### Kết quả đạt được tuần 2:

- Hiểu và triển khai được Amazon RDS: tạo DB instance, cấu hình Security Group, kết nối ứng dụng tới database.
- Biết cách đóng gói ứng dụng qua AMI/Launch Template và triển khai Load Balancer để phân phối traffic.
- Cấu hình được Auto Scaling Group, hiểu các chiến lược scaling (dynamic/scheduled) và kiểm thử khả năng tự mở rộng của hệ thống.
- Nắm được quy trình dọn dẹp tài nguyên để tránh phát sinh chi phí ngoài ý muốn.

