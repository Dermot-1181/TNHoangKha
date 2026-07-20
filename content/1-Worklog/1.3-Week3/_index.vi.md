---

title: "Worklog Tuần 3"
date: 2026-05-04
weight: 1
chapter: false
pre: " **1.3.** "
---


### Mục tiêu tuần 3:

- Biết cách quản lý và kiểm soát chi phí sử dụng dịch vụ AWS.
- Nắm được cách giám sát tài nguyên, log và cấu hình cảnh báo với Amazon CloudWatch.

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                             |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 2   | Học Amazon VPC: CIDR, Subnet public/private, Route Table, Internet Gateway, NAT Gateway. Làm lab VPC + Site-to-Site VPN                                                 | 04/05/2026   | 04/05/2026      | [Amazon VPC and AWS Site-to-Site VPN Workshop :: Start with Amazon VPC and AWS VPN Site-to-Site.](https://000003.awsstudygroup.com/)                                                                                                       |
| 3   | Làm lab Deploy Application on Docker Container: tạo Dockerfile, build image, chạy container, đẩy lên ECR. Hiểu container vs serverless                                  | 05/05/2026   | 05/05/2026      | [Deploy Application on Docker :: DEPLOY APPLICATION ON DOCKER CONTAINER](https://000015.awsstudygroup.com/)[Amazon Elastic Container Registry Documentation](https://docs.aws.amazon.com/ecr/)                                            |
| 4   | Làm lab Deploy Applications on Amazon ECS: ECS Cluster, Task Definition, Service, ALB. Hiểu tại sao project HireFlow dùng Lambda thay vì ECS (cost + scale per-request) | 06/05/2026   | 06/05/2026      | [Deploy applications on Amazon Elastic Container Service :: DEPLOY APPLICATIONS ON AMAZON ELASTIC CONTAINER SERVICE](https://000016.awsstudygroup.com/)[Amazon Elastic Container Service Documentation](https://docs.aws.amazon.com/ecs/) |




### Kết quả đạt được tuần 3:

- Hiểu và triển khai được mô hình mạng VPC an toàn (public/private subnet) kết hợp Site-to-Site VPN
- Nắm được quy trình containerize ứng dụng: Dockerfile → image → ECR → chạy trên ECS (Cluster, Task Definition, Service, ALB)

