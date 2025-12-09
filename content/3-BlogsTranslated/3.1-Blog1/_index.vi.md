---
title: "Blog 1"
date: "2025-10-04"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Bài Blog mã nguồn mở AWS
## AWS tham gia dự án DocumentDB để xây dựng công nghệ cơ sở dữ liệu tài liệu mã nguồn mở có khả năng tương tác
**Bởi Rashim Gupta | Ngày đăng: 24 tháng 8 năm 2025 | trong Amazon DocumentDB, Thông báo, Cơ sở dữ liệu, Mã nguồn mở | Liên kết cố định**

Tại AWS, chúng tôi thiết kế các dịch vụ đám mây mang lại cho khách hàng sự tự do lựa chọn công nghệ phù hợp nhất với nhu cầu của họ. Cam kết của chúng tôi về khả năng tương tác với các tiêu chuẩn mở và công nghệ mã nguồn mở là một trong những lý do khách hàng chọn AWS. Đây cũng chính là lý do tại sao chúng tôi ra mắt Amazon DocumentDB (tương thích với MongoDB) vào năm 2019.

Amazon DocumentDB là một dịch vụ cơ sở dữ liệu tài liệu hoàn toàn được quản lý, không cần máy chủ (serverless), và tương thích với API MongoDB. Hiện nay, Amazon DocumentDB phục vụ hàng chục nghìn khách hàng trên toàn cầu, trong nhiều ngành nghề khác nhau. Ví dụ: United Airlines sử dụng Amazon DocumentDB để hiện đại hóa quy trình đặt vé. Capital One sử dụng Amazon DocumentDB cho ứng dụng ra quyết định tín dụng. FINRA tận dụng Amazon DocumentDB để cải tiến cách thu thập các hồ sơ báo cáo quy định nhằm hỗ trợ sứ mệnh phục vụ khách hàng.

Hôm nay, AWS chính thức tham gia dự án DocumentDB mã nguồn mở, được điều hành bởi Linux Foundation, thể hiện cam kết của chúng tôi đối với sự lựa chọn và khả năng tương tác cho khách hàng. Microsoft đã khởi xướng dự án DocumentDB vào tháng 1 năm 2025 và sau đó chuyển dự án này cho Linux Foundation, nơi nó sẽ được lãnh đạo và quản trị độc lập. Mục tiêu của dự án là cung cấp cho cộng đồng nhà phát triển một cơ sở dữ liệu tài liệu dựa trên PostgreSQL, với toàn quyền minh bạch về kiến trúc và cách triển khai. Vì dự án được phát hành mã nguồn mở theo giấy phép MIT (rất tự do), các nhà phát triển và tổ chức có thể di chuyển các ứng dụng hiện có với ít thậm chí không cần thay đổi hoặc xây dựng các ứng dụng mới một cách linh hoạt. AWS đã gia nhập ủy ban kỹ thuật (Technical Steering Committee) của dự án và sẽ đóng góp để thúc đẩy sự phát triển của công nghệ quan trọng này.

Trong bài viết này, chúng tôi sẽ trình bày ba lý do AWS tham gia dự án DocumentDB mã nguồn mở.

**1. Độ tương thích với API MongoDB** 
Mục tiêu của DocumentDB là đạt được 100% khả năng tương thích với API MongoDB — API cơ sở dữ liệu tài liệu phổ biến nhất hiện nay. Bằng việc tham gia dự án này, AWS giúp khách hàng có thể truy cập cùng một khả năng tương thích, hiệu suất và tính năng, dù họ chạy ứng dụng ở đâu — trên AWS, đám mây khác, on-premises (tại chỗ) hay máy tính cá nhân.

**2. Đổi mới tính năng**
Mã nguồn mở thúc đẩy đổi mới nhanh hơn. Khi có nhiều nhà cung cấp cơ sở dữ liệu và đám mây — như Microsoft và Yugabyte — cùng tham gia dự án, cộng đồng sẽ không chỉ hướng đến việc tương thích hoàn toàn với MongoDB API, mà còn cải thiện hiệu năng, tính năng và trải nghiệm cho nhà phát triển. Mỗi tổ chức mang đến chuyên môn riêng, và với nhiều công ty cùng đóng góp, khách hàng sẽ hưởng lợi từ những cải tiến liên tục trong dự án, mà vẫn đảm bảo khả năng tương thích với MongoDB API.

**3. Xây dựng trên nền PostgreSQL**
DocumentDB được xây dựng dựa trên PostgreSQL — cơ sở dữ liệu quan hệ được nhiều doanh nghiệp và startup tin dùng nhờ độ tin cậy, tính năng phong phú và khả năng mở rộng, với gần 35 năm phát triển liên tục. AWS được công nhận là một nhà đóng góp lớn cho cộng đồng mã nguồn mở PostgreSQL, cung cấp các đóng góp cho phần mềm cơ sở dữ liệu cốt lõi, tiện ích mở rộng, trình điều khiển và quản trị dự án. AWS đã đóng góp vào các tính năng của PostgreSQL về tính khả dụng cao, các bản nâng cấp lớn, cải thiện hiệu suất cho các truy vấn và hoạt động bảo trì, trình điều khiển JDBC, các tiện ích mở rộng như pgvector và các hoạt động cộng đồng. Ví dụ: Trusted Language Extensions (pg_tle) để xây dựng các tiện ích mở rộng trên các hệ thống tệp bị hạn chế, và pgactive để tăng cường khả năng phục hồi và tính linh hoạt khi di chuyển dữ liệu giữa hai hoặc nhiều cơ sở dữ liệu đang hoạt động. Ngay bây giờ, khi tham gia dự án DocumentDB, chúng tôi sẽ kết hợp chuyên môn về Amazon DocumentDB và PostgreSQL để cải thiện dự án mã nguồn mở, mang lại lợi ích cho người dùng.

## Nhìn về tương lai
Từ khi thành lập, AWS luôn là nơi tốt nhất để khách hàng xây dựng và vận hành phần mềm mã nguồn mở trên đám mây. AWS tự hào hỗ trợ các dự án, quỹ và đối tác mã nguồn mở. Chúng tôi cam kết mang lại giá trị của mã nguồn mở cho khách hàng, đồng thời mang sự xuất sắc trong vận hành của AWS đến cộng đồng.

Là một phần trong cam kết nguồn mở của chúng tôi, AWS có bề dày lịch sử đóng góp cho các dự án nguồn mở. Ví dụ, chúng tôi có rất nhiều đóng góp cho Lucene, Valkey, containerd, PostgreSQL, Rust và OpenSearch. Chúng tôi đã học được rất nhiều từ những kinh nghiệm đó, bao gồm tầm quan trọng của việc tham gia và đóng góp trực tiếp vào các dự án nguồn mở mà chúng tôi xây dựng và khách hàng của chúng tôi tin tưởng. AWS vẫn cam kết lâu dài với DocumentDB vì chúng tôi tin tưởng mạnh mẽ vào sức mạnh của nguồn mở trong việc thúc đẩy đổi mới. Tương tự như Valkey và OpenSearch, chúng tôi sẽ tiếp tục đưa những cải tiến đã được xây dựng trong Amazon DocumentDB vào dự án nguồn mở DocumentDB để khách hàng có thể tiếp tục tận dụng các khả năng đó, bất kể họ chọn chạy phần mềm ở đâu.

Mặc dù dự án DocumentDB, do Linux Foundation quản lý, có tên tương tự như Amazon DocumentDB, nhưng chúng sử dụng phần mềm khác nhau bên trong. Amazon DocumentDB là cơ sở dữ liệu tài liệu tương thích với API MongoDB do AWS xây dựng. Mặt khác, dự án do Linux Foundation quản lý, mặc dù cũng tương thích với MongoDB, nhưng lại sử dụng một công cụ nguồn mở được xây dựng dưới dạng tiện ích mở rộng trên PostgreSQL. Đây là một công cụ khác với công cụ được sử dụng trong Amazon DocumentDB. AWS sẽ tiếp tục đầu tư vào cả Amazon DocumentDB và DocumentDB nguồn mở tương tự như cách chúng tôi đầu tư vào Amazon OpenSearch Service và OpenSearch. Trong tương lai, chúng tôi sẽ bắt đầu đóng góp các cải tiến của Amazon DocumentDB cho dự án nguồn mở và áp dụng các tính năng và khả năng từ công cụ DocumentDB nguồn mở vào dịch vụ Amazon DocumentDB do chúng tôi quản lý theo thời gian. Chúng tôi sẽ thông báo những thay đổi này trong các bài đăng Có gì mới trong những tháng tới.

> “Thật tuyệt vời khi Microsoft, AWS và các công ty khác cùng hợp tác phát triển DocumentDB, một triển khai mã nguồn mở của API tương thích với MongoDB trên PostgreSQL. Microsoft và AWS đã hợp tác để cải tiến PostgreSQL, vì vậy việc họ sử dụng mã nguồn Postgres chất lượng cao và tận dụng khả năng mở rộng của nó để đáp ứng nhu cầu về một cơ sở dữ liệu tài liệu mã nguồn mở là điều hợp lý”, Bruce Momjian cho biết, thành viên sáng lập nhóm phát triển cốt lõi của PostgreSQL. “Ý tưởng sử dụng Postgres theo cách này đã có từ lâu, vì vậy tôi rất vui vì giờ đây nó đang nhận được sự quan tâm đáng kể. DocumentDB sẽ là một lựa chọn thay thế thú vị cho những người dùng muốn triển khai mã nguồn mở, và cho những người dùng cơ sở dữ liệu chỉ muốn có một giao diện đơn giản hơn với PostgreSQL.”

> “DocumentDB lấp đầy một khoảng trống quan trọng trong hệ sinh thái cơ sở dữ liệu tài liệu, thu hút những người đóng góp, người dùng và những người tiên phong. Điều thú vị hơn nữa là nó cung cấp một tiêu chuẩn mở cho các ứng dụng dựa trên tài liệu, giống như những gì SQL đã làm cho cơ sở dữ liệu quan hệ”, Jim Zemlin, giám đốc điều hành của Linux Foundation, cho biết. “Bằng cách tham gia Linux Foundation, DocumentDB đang đảm bảo tương lai nguồn mở của mình và góp phần vạch ra một hướng đi mới cho các tiêu chuẩn cơ sở dữ liệu NoSQL và đổi mới sáng tạo do cộng đồng thúc đẩy.”

Chúng tôi rất hào hứng được đóng góp cho dự án DocumentDB với tư cách là một trong nhiều bên liên quan. Hãy tham gia cùng chúng tôi trên GitHub để tiếp tục phát triển nguồn mở trên DocumentDB. Bạn cũng có thể truy cập trang web của chúng tôi tại đây để tìm hiểu thêm và tham gia.

**Rashim Gupta**  
Rashim Gupta là Quản lý Cấp cao, Quản lý Sản phẩm tại AWS. Trong bảy năm qua, Rashim đã lãnh đạo sản phẩm cho nhiều dịch vụ cơ sở dữ liệu tại AWS. Hiện tại, ông đang lãnh đạo các nhóm Quản lý Sản phẩm (PM) cho Amazon DocumentDB, Amazon Timestream và Amazon Neptune. Trước khi đảm nhiệm vai trò hiện tại, ông đã lãnh đạo việc ra mắt Valkey trên Amazon ElastiCache và Amazon MemoryDB. Ông cũng đã làm việc về các dịch vụ lưu trữ và điện toán trên AWS, phân tích trên Microsoft Azure và trải nghiệm phát triển tại Meta.
