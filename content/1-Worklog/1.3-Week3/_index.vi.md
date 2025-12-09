---
title: "Nhật ký Tuần 3"
date: 2025-09-22
weight: 3
chapter: false
pre: " <b> 3.1. </b> "
---

## Mục tiêu Tuần 3 (22 – 28 Tháng 9, 2025)

- Khám phá **AWS Lambda** và các khái niệm serverless.  
- Học cách tạo Lambda function và tích hợp với các dịch vụ AWS khác.  
- Hiểu **API Gateway** và cách expose Lambda qua REST API.  
- Thực hành theo dõi hiệu suất và logs của Lambda bằng **CloudWatch**.

## Hoạt động & Nhiệm vụ

| Ngày | Hoạt động | Bắt đầu | Kết thúc | Tài liệu tham khảo |
| --- | --------- | ------- | -------- | ---------------- |
| Thứ Hai, 22/09 | Nghiên cứu kiến trúc serverless và các tình huống sử dụng AWS Lambda. | 22/09/2025 | 22/09/2025 | [AWS Lambda Docs](https://aws.amazon.com/lambda/) |
| Thứ Ba, 23/09 | Tạo Lambda function đầu tiên bằng Python, thử nghiệm chạy cơ bản, khám phá console và CLI. | 23/09/2025 | 23/09/2025 | [Lambda Getting Started](https://docs.aws.amazon.com/lambda/) |
| Thứ Tư, 24/09 | Tích hợp Lambda với sự kiện S3 để kích hoạt khi upload object. | 24/09/2025 | 24/09/2025 | [S3 Event Triggers](https://docs.aws.amazon.com/) |
| Thứ Năm, 25/09 | Khám phá API Gateway: tạo REST API, kết nối với Lambda, kiểm tra endpoints. | 25/09/2025 | 25/09/2025 | [API Gateway Docs](https://aws.amazon.com/api-gateway/) |
| Thứ Sáu, 26/09 | Thực hành theo dõi Lambda bằng CloudWatch logs, tạo cảnh báo lỗi và thời gian chạy. | 26/09/2025 | 26/09/2025 | [CloudWatch Docs](https://aws.amazon.com/cloudwatch/) |

## Thành tựu

- Hiểu được các khái niệm serverless và quy trình làm việc của Lambda.  
- Tạo và thử nghiệm Lambda function thành công, kích hoạt bởi sự kiện S3.  
- Expose Lambda qua REST API của API Gateway.  
- Thực hành theo dõi logs Lambda và đặt cảnh báo CloudWatch để giám sát chủ động.  

## Đánh giá

Tuần 3 mang lại kinh nghiệm thực tiễn về phát triển serverless. Kết hợp Lambda với S3 và API Gateway giúp xây dựng ứng dụng event-driven, có khả năng mở rộng mà không cần quản lý server. CloudWatch giúp đảm bảo hệ thống ổn định trong môi trường production.
