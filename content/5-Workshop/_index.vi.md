---
title: "Workshop"
date: 2026-07-10
weight: 5
chapter: false
pre: " <b> 5. </b> "
---




# Xây dựng Pipeline xử lý CV tự động — HireFlow AI


![](/images/2-Proposal/Proposal.png)


#### Tổng quan


Workshop này hướng dẫn xây dựng toàn bộ hệ thống HireFlow AI trên AWS Serverless — từ khi ứng viên upload CV qua Candidate App, file được kiểm tra bảo mật (Lambda FileValidator), trích xuất văn bản bằng **Amazon Textract** (Lambda Extract), chấm điểm bằng **Extend AI** (Lambda Score), cho đến khi kết quả được lưu vào **DynamoDB** (bảng Candidate, JD) và gửi thông báo qua email (Lambda Save & Notify).

**Kiến trúc:** Queue-based serverless pipeline — mỗi giai đoạn xử lý (Extract → Score → Save&Notify) tách rời nhau bằng **SQS**, đảm bảo scalability và fault-tolerance: một Lambda lỗi không làm sập cả pipeline, message tự động retry. Không có server persistent — toàn bộ chạy trên Lambda, auto-scale theo số lượng CV nộp vào. Quản lý quyền truy cập bằng **IAM**, mã hóa dữ liệu bằng **KMS**, giám sát vận hành bằng **CloudWatch**.



#### Nội dung

1. [Tổng quan về workshop](5.1-Introduction/_index.vi.md)
2. [Chuẩn bị](5.2-Prerequiste/_index.vi.md)
3. [Truy cập đến S3 từ VPC](5.3-Implementation%20Steps/_index.vi.md)
4. [Truy cập đến S3 từ TTDL On-premises](5.4-Validation%20&%20Analysis/_index.vi.md)
5. [VPC Endpoint Policies (làm thêm)](5.5-Cleanup/_index.vi.md)
6. [Dọn dẹp tài nguyên](5.6-Challenges%20&%20Future%20Direction/_index.vi.md)
7. [Thực Nghiệm Sản Phẩm](5.7-Product%20Demonstration/_index.vi.md)