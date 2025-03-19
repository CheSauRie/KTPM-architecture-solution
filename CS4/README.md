# CASE STUDY 4
Dưới đây là một chương trình đơn giản sử dụng `express.js`, để ghi các giá trị vào database theo cặp key-value. Chương trình cung cấp một trang web liên tục cập nhật giá trị của key để gửi về kết quả mới, có thể được ứng dụng để cập nhật giá vàng, thông số theo *thời gian thực*. Chương trình có thể chưa hoàn toàn được tối ưu.

## Hướng dẫn cài đặt
```sh
# Cài đặt các gói liên quan
$ npm install
# Tạo folder cho database
$ mkdir db
# Khởi chạy ứng dụng
$ npm start 
```

## Mô Tả
| Endpoint | Phương thức | Mục tiêu
|--|:--:|--|
| /add | POST | Thêm/chỉnh sửa giá trị trong database
| /get/:keyID | GET | Trả về giá trị của keyID
| /viewer/:keyID | GET | Trang web theo dõi giá trị của keyID


## Yêu cầu triển khai
| Mức độ | Mô tả |
|--|--|
| ![Static Badge](https://img.shields.io/badge/OPTIONAL-medium-yellow)  | Tối ưu chương trình trên |
| ![Static Badge](https://img.shields.io/badge/OPTIONAL-easy-green) | Bổ sung giao diện web hoàn chỉnh hơn |
| ![Static Badge](https://img.shields.io/badge/OPTIONAL-easy-green) | Thay thế cơ sở dữ liệu hiện tại |
| ![Static Badge](https://img.shields.io/badge/REQUIRED-easy-green) | Thay thế công nghệ sử dụng cho việc gọi request liên tục trong `viewer.html` (VD: socket.io, ...) |
| ![Static Badge](https://img.shields.io/badge/REQUIRED-medium-yellow) | Thêm lớp persistent bằng cách sử dụng ORM (Object-Relational Mapping) |
| ![Static Badge](https://img.shields.io/badge/REQUIRED-medium-yellow) | Triển khai theo kiến trúc Publisher-Subscriber và cài đặt message broker tuỳ chọn |
| ![Static Badge](https://img.shields.io/badge/REQUIRED-medium-yellow) | Nêu các vấn đề chương trình gốc đang gặp phải về các thuộc tính chất lượng và *đánh giá* hiệu năng sau khi nâng cấp |

Ngoài ra, các bạn có thể tuỳ chọn bổ sung thêm một số phần triển khai khác.

1. Đánh giá chương trình gốc
1.1. Kiến trúc và cách thức hoạt động
Mô hình hoạt động:
Chương trình sử dụng Express.js để xây dựng các endpoint phục vụ thao tác ghi/đọc dữ liệu theo cặp key-value. Cụ thể:

/add (POST): Thêm hoặc cập nhật giá trị cho key trong "database" (có thể là file lưu trong folder db).
/get/:keyID (GET): Lấy giá trị của key được yêu cầu.
/viewer/:keyID (GET): Cung cấp một giao diện web để theo dõi giá trị của key theo thời gian thực.
Ưu điểm:

Cài đặt đơn giản, dễ hiểu và triển khai nhanh.
Hỗ trợ cập nhật giá trị theo thời gian thực thông qua trang viewer (mặc dù cách cập nhật có thể dựa trên cơ chế polling).
1.2. Các hạn chế kỹ thuật và chất lượng hiện tại
Cơ sở dữ liệu (File-based):
Việc sử dụng file trong folder db không đảm bảo tính nhất quán, dễ gặp lỗi đồng thời (concurrency issues), hạn chế khả năng mở rộng và không hỗ trợ giao dịch (transactions).
Giao diện người dùng:
Giao diện của trang viewer có thể chưa được tối ưu về mặt trải nghiệm người dùng, thiếu các yếu tố giao diện hiện đại (responsive, thiết kế trực quan…).
Cơ chế cập nhật dữ liệu:
Nếu sử dụng polling (gửi request liên tục) để cập nhật trang viewer thì sẽ tạo tải mạng không cần thiết và có thể gây ra vấn đề về hiệu năng khi số lượng người dùng tăng.
Xử lý logic và bảo mật:
Có thể thiếu cơ chế kiểm tra dữ liệu (validation), xử lý lỗi (error handling) và bảo mật (như chống tấn công injection, xác thực request…).
2. Đề xuất cải tiến
2.1. Tối ưu chương trình
Refactoring mã nguồn:
Tách biệt các module (routes, controller, service, data access) để tăng tính tái sử dụng và dễ bảo trì.
Áp dụng các middleware để xử lý validate input và log lỗi.
Quản lý lỗi và logging:
Cải thiện cơ chế xử lý lỗi (error middleware) và triển khai logging (sử dụng thư viện như Winston hoặc Bunyan).
2.2. Bổ sung giao diện web hoàn chỉnh hơn
Cải tiến giao diện:
Sử dụng các framework CSS/JS hiện đại (ví dụ: Bootstrap, TailwindCSS) để xây dựng giao diện thân thiện, responsive.
Cung cấp thêm các tính năng hiển thị thông tin, biểu đồ (nếu cần theo dõi biến động dữ liệu theo thời gian) và thông báo trạng thái (real-time notifications).
2.3. Thay thế cơ sở dữ liệu hiện tại
Chuyển sang hệ quản trị CSDL chuyên nghiệp:
Sử dụng các CSDL như MongoDB, PostgreSQL hoặc MySQL thay vì lưu trữ dữ liệu trên file.
Điều này giúp đảm bảo tính nhất quán, mở rộng quy mô và hỗ trợ các tính năng giao dịch (transactions) khi cần.
2.4. Thay thế công nghệ gọi request liên tục trong viewer.html
Chuyển sang công nghệ push real-time:
Sử dụng WebSocket thông qua thư viện như socket.io để thay thế cơ chế polling hiện tại.
Ưu điểm: giảm tải mạng, cải thiện tốc độ phản hồi và tăng trải nghiệm người dùng.
2.5. Thêm lớp persistent bằng cách sử dụng ORM
Áp dụng ORM:
Sử dụng các ORM như Sequelize (với CSDL quan hệ như PostgreSQL/MySQL) hoặc Mongoose (với MongoDB) để quản lý dữ liệu.
Lợi ích: Trừu tượng hóa các thao tác CSDL, tăng tính bảo trì và dễ dàng mở rộng ứng dụng.
2.6. Triển khai kiến trúc Publisher-Subscriber và tích hợp message broker
Kiến trúc Pub/Sub:
Áp dụng mô hình Publisher-Subscriber để tách biệt giữa thành phần sản xuất dữ liệu (publisher) và thành phần tiêu thụ dữ liệu (subscriber).
Message Broker:
Tích hợp một message broker như Redis Pub/Sub, RabbitMQ hoặc Kafka để đảm bảo việc phân phối thông điệp một cách hiệu quả.
Ví dụ: Khi có dữ liệu mới được cập nhật qua endpoint /add, ứng dụng sẽ publish thông điệp đến broker, và các client đang kết nối qua socket.io sẽ subscribe và nhận thông báo cập nhật ngay lập tức.
3. Các vấn đề chương trình gốc gặp phải và đánh giá hiệu năng sau khi nâng cấp
3.1. Vấn đề của chương trình gốc
Khả năng mở rộng:
Cơ sở dữ liệu file-based không đảm bảo khả năng mở rộng khi số lượng request tăng cao.
Polling liên tục từ client dẫn đến tải không cần thiết cho server.
Độ tin cậy và bảo mật:
Thiếu các cơ chế xác thực, kiểm tra lỗi, dễ dẫn đến các lỗi không mong muốn hoặc tấn công từ bên ngoài.
Không đảm bảo tính nhất quán dữ liệu khi có nhiều yêu cầu ghi/đọc đồng thời.
Hiệu năng:
Sử dụng file I/O cho thao tác CSDL có thể gây ra độ trễ, đặc biệt trong môi trường có lượng truy cập lớn.
Polling gây tăng traffic không cần thiết, làm giảm khả năng đáp ứng của server.
3.2. Đánh giá hiệu năng sau khi nâng cấp
Cải thiện về hiệu năng:

Sử dụng CSDL chuyên nghiệp kết hợp với ORM: Giảm độ trễ truy xuất dữ liệu, hỗ trợ caching và tối ưu hóa truy vấn.
Ứng dụng Pub/Sub với message broker: Cho phép phân phối thông báo một cách nhanh chóng, giảm tải so với polling.
Sử dụng socket.io: Giảm bớt số lượng request HTTP không cần thiết, cải thiện khả năng phản hồi thời gian thực.
Tính mở rộng và bảo trì:

Kiến trúc module rõ ràng và tách biệt sẽ giúp dễ dàng mở rộng và bảo trì khi ứng dụng phát triển.
Việc sử dụng các công nghệ chuyên dụng (ORM, message broker, WebSocket) đảm bảo ứng dụng có thể phục vụ số lượng người dùng lớn hơn một cách hiệu quả.
Bảo mật và xử lý lỗi:

Các biện pháp bảo mật như validate dữ liệu đầu vào, xử lý lỗi tập trung và logging sẽ giúp ứng dụng trở nên ổn định và an toàn hơn trước các cuộc tấn công hoặc lỗi ngoài ý muốn.
