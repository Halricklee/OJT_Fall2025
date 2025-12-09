---
title: "Blog 3"
date: "2025-10-04"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
## AWS cho SAP
## Tự động hóa SAP HANA DB HA Patch bằng SSM và nZDT

Bởi Guilherme Sesterheim và Mohit Biyani | Ngày 22 tháng 8 năm 2025 | trong Amazon EC2, SAP trên AWS, Hướng dẫn kỹ thuật | Liên kết cố định

---

### Giới thiệu

Việc cập nhật cơ sở dữ liệu SAP HANA của bạn với các bản vá mới nhất là rất quan trọng để duy trì bảo mật, hiệu suất và độ tin cậy. Tuy nhiên, việc vá lỗi cơ sở dữ liệu truyền thống thường đòi hỏi thời gian ngừng hoạt động đáng kể, ảnh hưởng đến hoạt động kinh doanh. Trong khi các hướng dẫn của AWS trước đây đề cập đến nhiều phương pháp tự động hóa khác nhau, bài viết này giới thiệu một giải pháp mới giúp giảm thiểu thời gian ngừng hoạt động gần như bằng không cho cơ sở dữ liệu SAP HANA có tính sẵn sàng cao sử dụng các dịch vụ AWS gốc.

Sử dụng AWS Systems Manager và AWS CloudFormation, chúng tôi sẽ hướng dẫn bạn cách tự động hóa việc vá lỗi cơ sở dữ liệu SAP HANA cho cả môi trường Red Hat Enterprise Linux (RHEL) và SUSE Linux Enterprise Server (SLES).

### Lợi ích của phương pháp này

Việc sử dụng công cụ gốc AWS này giúp cải thiện việc bảo trì cơ sở dữ liệu SAP HANA bằng cách tích hợp các tính năng vận hành và bảo mật quan trọng vào một giải pháp thống nhất. AWS Systems Manager đóng vai trò là trung tâm chỉ huy của bạn, cung cấp khả năng giám sát theo thời gian thực, ghi nhật ký chi tiết và xác thực tình trạng hoạt động tự động trong suốt quá trình cập nhật. Giải pháp này phối hợp thông minh các bản cập nhật giữa các nút chính và phụ, đồng thời duy trì khả năng khôi phục để đảm bảo hoạt động. Bằng cách loại bỏ nhu cầu sử dụng các công cụ của bên thứ ba, bạn không chỉ giảm chi phí cấp phép mà còn được hưởng lợi từ việc tích hợp dịch vụ AWS gốc, bao gồm giao tiếp được mã hóa và các bản ghi kiểm toán toàn diện. Phương pháp hợp nhất này, được quản lý thông qua một bảng điều khiển AWS Systems Manager duy nhất, mang đến khả năng bảo trì cơ sở dữ liệu quy mô doanh nghiệp với các biện pháp kiểm soát bảo mật và tuân thủ tích hợp sẵn.

### Điều kiện tiên quyết

Bạn phải đáp ứng các điều kiện tiên quyết sau đây trước khi sử dụng mã này để cập nhật Cơ sở dữ liệu HANA (HDB) trong thiết lập HA:

* Môi trường cơ sở dữ liệu SAP HANA 2.0+ được cấu hình sẵn, chạy ở chế độ sẵn sàng cao trên các phiên bản Amazon EC2 của bạn. Mặc dù chúng tôi sẽ không đề cập đến thiết lập ban đầu trong bài viết này, nhưng chúng tôi khuyến khích bạn sử dụng AWS Launch Wizard để tự động hóa việc triển khai và cấu hình các khối lượng công việc SAP của bạn như SAP HANA, hoặc tận dụng tài liệu hướng dẫn SAP on AWS nếu bạn cần hỗ trợ về cấu hình.
* Hãy xác minh rằng các phiên bản EC2 của cơ sở dữ liệu SAP HANA của bạn được quản lý bởi AWS Systems Manager. Điều này rất quan trọng vì giải pháp tự động hóa tận dụng các khả năng của Systems Manager để quản lý và vận hành liền mạch.
* Nếu bạn đang sử dụng tài khoản dịch vụ trung tâm hoặc chia sẻ để tự động hóa (thực hành tốt nhất được khuyến nghị của AWS), hãy đảm bảo bạn đã cấu hình các quyền liên tài khoản phù hợp trước khi tiếp tục, hãy tham khảo liên kết để biết thêm thông tin.
* Tính năng tự động hóa của AWS Systems Manager sẽ đồng bộ hóa các tệp phương tiện Amazon S3 với thư mục /tmp trên phiên bản EC2 của bạn. Trước khi chạy tự động hóa, hãy đảm bảo bạn có đủ dung lượng lưu trữ trong thư mục tạm thời này. Dung lượng cần thiết phụ thuộc vào kích thước tệp cho bản cập nhật bạn đang thực hiện.
* Thêm thẻ vào phiên bản EC2 của cơ sở dữ liệu SAP HANA với cặp khóa-giá trị "Hostname: <hostname>". Bạn sẽ cần giá trị hostname này khi chạy giải pháp ở bước sau.

### Sơ đồ Kiến trúc

Sơ đồ kiến ​​trúc (trong Hình 1) minh họa giải pháp sử dụng AWS Systems Manager Automation Documents, một thùng chứa Amazon S3 chứa phương tiện cài đặt bản vá SAP HANA và các tham số thiết yếu được lưu trữ trong AWS Secrets Manager. Các khối lượng công việc SAP HANA chạy trên các phiên bản Amazon EC2 trong tài khoản AWS của bạn.

> Hình 1 – Cụm HANA DB HA 

### Chuẩn bị thực thi

1.  Thiết lập một tài khoản người dùng trong SAP HANA SYSTEMDB của bạn với đủ đặc quyền để thực hiện cập nhật cơ sở dữ liệu. Người dùng này sẽ được tham chiếu trong quy trình tự động hóa, vì vậy điều cần thiết là phải xác minh tài khoản có tất cả các quyền cần thiết trước khi tiến hành nâng cấp. Chúng tôi đặc biệt khuyên bạn nên tuân theo nguyên tắc đặc quyền tối thiểu khi cấu hình quyền người dùng cơ sở dữ liệu SAP HANA. Để biết thêm chi tiết, hãy xem Tạo Người dùng Cơ sở dữ liệu Ít Đặc quyền hơn để Cập nhật.

2.  Hãy đảm bảo các phiên bản cơ sở dữ liệu SAP HANA của bạn có các quyền cần thiết để truy cập vào thùng Amazon S3 chứa phương tiện cài đặt bản vá SAP HANA. Để biết hướng dẫn chi tiết về cách tạo và gắn chính sách IAM vào các phiên bản EC2, hãy xem phần Làm việc với Vai trò IAM trong Hướng dẫn Sử dụng AWS IAM. Đoạn mã 1 là một chính sách mẫu minh họa cách cấp cho một vai trò IAM cụ thể trong tài khoản của bạn các quyền cần thiết để tải xuống tệp từ thùng S3. Hãy đảm bảo bạn thay đổi tên thùng và vai trò (được tô sáng màu vàng) theo môi trường của bạn.

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AddPerm",
                "Effect": "Allow",
                "Principal": {
                    "AWS": "arn:aws:iam::{account_id}:role/service-role/{ec2_role}"
                },
                "Action": [
                    "s3:GetObject",
                    "s3:GetObjectVersion",
                    "s3:ListBucket"
                ],
                "Resource": [
                    "arn:aws:s3:::{bucket_name}/*",
                    "arn:aws:s3:::{bucket_name}"
                ]
            }
        ]
    }
    ```

    > Đoạn trích 1 – Chính sách nhóm S3

3.  Quá trình tự động hóa dựa vào AWS Secrets Manager để truy xuất thông tin xác thực cơ sở dữ liệu SAP HANA. AWS Secrets Manager cho phép bạn chia sẻ bí mật trên cùng/khác tài khoản AWS. Chức năng này cho phép bạn tập trung quản lý bí mật tại một vị trí duy nhất. Để biết thêm chi tiết, hãy tham khảo bài viết Cách chia sẻ bí mật AWS Secrets Manager giữa các tài khoản AWS. Hãy đảm bảo bạn đã tạo các bí mật cần thiết (mật khẩu người dùng sapadm và mật khẩu người dùng SYSTEM) trong AWS Secrets Manager và cấu hình các quyền phù hợp cho tài khoản AWS mục tiêu của bạn để truy cập các bí mật này.

4.  Để cho phép truy cập thông tin nhạy cảm giữa các tài khoản an toàn, giải pháp sử dụng AWS Secrets Manager với mã hóa AWS KMS. Các bí mật được mã hóa được bảo vệ bằng khóa KMS mà tất cả các tài khoản AWS tham gia đều có thể truy cập. Để biết hướng dẫn chi tiết về cách cấu hình quyền truy cập bí mật giữa các tài khoản, vui lòng tham khảo tài liệu.

### Cách chạy mã

Hãy làm theo các bước dưới đây để sử dụng mã có trong kho lưu trữ GitHub này để cập nhật Cơ sở dữ liệu HANA của bạn.

1.  Tạo các bí mật cần thiết trên AWS Secrets Manager trên cùng tài khoản bạn đang quản lý cơ sở dữ liệu HANA. Để bạn tham khảo, chúng tôi đã bao gồm một bí mật mẫu về cách cấu hình trên hình ảnh 2. Ví dụ này minh họa định dạng dự kiến ​​và các cặp khóa-giá trị cần thiết để tự động hóa hoạt động chính xác. Vui lòng đảm bảo các bí mật của bạn tuân theo cấu trúc tương tự khi sử dụng thông tin đăng nhập thực tế của bạn.

    > Hình 2 – Ví dụ về bí mật cho thông tin đăng nhập DB 

2.  Thiết lập một thùng S3 làm nơi lưu trữ cho các tệp cập nhật của bạn.

3.  Sử dụng hướng dẫn tạo ngăn xếp CloudFormation để triển khai giải pháp. Trong kho lưu trữ, tại thư mục cloudformation, hãy tham khảo 2 tệp bên dưới:

    * hana_db_patch_ha_rhel – được sử dụng cho các cơ sở dữ liệu được cấu hình cho HA trong hệ thống RHEL. Điều này sẽ đạt được khái niệm nZDT được giải thích trong phần Giới thiệu.
    * hana_db_patch_ha_suse – được sử dụng cho các cơ sở dữ liệu được cấu hình cho HA trong hệ thống SUSE. Điều này sẽ đạt được khái niệm nZDT được giải thích trong phần Giới thiệu.

4.  Vào Trình quản lý Hệ thống > Tài liệu > chọn tab “Do tôi sở hữu” > tìm kiếm “patch” và mở tài liệu tương ứng của bạn:

    > Hình 3 – Tài liệu SSM có sẵn để sử dụng 

5.  Chọn “Thực hiện tự động hóa” ở góc trên bên phải:

    > Hình 4 – Các bước thực hiện SSM 

6.  Điền các thông số đầu vào cần thiết:

    > Hình 5 – Tham số đầu vào của tài liệu SSM 

7.  Cuộn xuống và chọn “Thực hiện”.

8.  Sau khi hoàn tất thành công, bạn sẽ thấy thông báo hoàn tất như Hình 6.

    > Hình 6 – Đầu ra tự động hóa SSM 

### Quy trình thực hiện

Dưới đây là sơ đồ quy trình với tất cả các bước và chi tiết của chúng được thực hiện bởi quy trình tự động hóa này.

> Biểu đồ luồng 1 – Trình tự thực hiện SSM 

Các bước được hiển thị trong "Lưu đồ 1" được thực hiện tuần tự. Nếu bất kỳ bước nào bị lỗi, toàn bộ quy trình sẽ dừng ngay lập tức.

Nếu bạn gặp lỗi, vui lòng tham khảo phần Xử lý sự cố của blog này để chẩn đoán và giải quyết sự cố trước khi tiếp tục.

### Xử lý sự cố

Để giám sát và hỗ trợ chẩn đoán các sự cố tự động hóa, AWS Systems Manager duy trì nhật ký thực thi chi tiết trong cả phiên bản EC2 và Nhật ký CloudWatch. Các nhật ký này ghi lại tiến trình từng bước của quá trình tự động hóa của bạn.

#### Giám sát Trạng thái Tự động hóa:

Để kiểm tra trạng thái của các tự động hóa Systems Manager của bạn:

1.  Mở bảng điều khiển AWS Systems Manager.
2.  Trong ngăn điều hướng bên trái, chọn Tự động hóa
3.  Chọn Cấu hình tùy chọn > Thực thi
4.  Xem trạng thái tự động hóa của bạn trong phần Thực thi tự động hóa

#### Xem lại Chi tiết Thực thi:

AWS Management Console cho phép bạn xem xét chi tiết từng lần thực thi tự động hóa. Bạn có thể:

* Điều hướng qua từng bước tự động hóa
* Xem lại kết quả của từng bước
* Xác định bất kỳ lỗi nào xảy ra trong quá trình tự động hóa

#### Khắc phục sự cố với Nhật ký:

Có hai cách để truy cập nhật ký tự động hóa:

* Nhật ký Phiên bản EC2
    * Đường dẫn: /var/lib/amazon/ssm/{instance-id}/document/orchestration/{automation_step_execution_id}/awsrunShellScript/0.awsrunShellScript – chứa nhật ký thực thi chi tiết từ phiên bản EC2
    * Đường dẫn: /tmp/hana/patch – chứa các tệp được sử dụng cho quy trình vá
    * Đường dẫn: /tmp/update – chứa thông tin đăng nhập để chạy lệnh cập nhật
* Tích hợp Nhật ký CloudWatch
    * Bạn có thể cấu hình Systems Manager để gửi đầu ra tự động hóa đến Nhật ký Amazon CloudWatch
    * Để biết hướng dẫn thiết lập, hãy xem [Cấu hình Nhật ký Amazon CloudWatch để Chạy Lệnh]

### Cân nhắc về chi phí

AWS Systems Manager Automation được sử dụng trong giải pháp vá lỗi HANA DB này áp dụng mô hình trả tiền theo mức sử dụng. Bạn sẽ được tính phí dựa trên số lượng và thời lượng các bước tự động hóa được thực hiện, với một gói miễn phí hào phóng bao gồm:

* 100.000 bước tự động hóa mỗi tháng,
* 5.000 giây thời gian thực hiện tự động hóa mỗi tháng.

Nếu bạn đang sử dụng AWS Organizations, mức sử dụng gói miễn phí này được chia sẻ cho tất cả các tài khoản trong nhóm Consolidated Billing của bạn. Nếu bạn đang chạy các khối lượng công việc khác trên cùng một tài khoản đã sử dụng toàn bộ gói miễn phí của mình, chi phí liên quan đến việc chạy công cụ này sẽ duy trì dưới 10 đô la Mỹ.

Để biết thông tin chi tiết về cách tính chi phí, vui lòng tham khảo AWS Systems Manager Pricing.

### Kết luận

Như đã trình bày trong bài đăng trên blog này, việc tự động hóa các bản cập nhật vá lỗi cho HANA DB có thể dễ dàng thực hiện bằng các công cụ gốc của AWS. Khi triển khai tính năng này trong môi trường của bạn, hãy đảm bảo chạy thử trên một tài khoản không hoạt động trước khi triển khai trên môi trường thực tế, vì một số phiên bản hệ điều hành nhỏ có thể khiến giải pháp hoạt động khác nhau giữa các môi trường.

Bằng cách sử dụng giải pháp này, bạn có thể chuẩn hóa quy trình cập nhật trên các nền tảng và môi trường SAP khác nhau và có một nguồn dữ liệu duy nhất cho quy trình.

Bạn có muốn tìm hiểu thêm hoặc muốn hiểu rõ hơn về cách mở rộng giải pháp này cho dự án của mình không?

Để biết thêm thông tin, vui lòng liên hệ với chúng tôi theo địa chỉ sap-on-aws@amazon.com.