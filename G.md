# Tạo tunnel trên cloudflare
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/b4f6a1e6-28fb-4007-b018-8ac2580acfec" />
# Khai báo nodered, ngix, flared
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/ffc0cf0b-9be3-405d-a5be-c1a18397f4e5" />
# Khởi động lại docker compose
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/16529abc-aef4-41fa-ab2f-cc786fea0021" />
# Public ứng dụng ra Internet
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/a5d2d369-05ce-40c4-94a5-fc8aef4fe68f" />

### Câu hỏi về bài làm?
1. Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?
Quản lý tập trung: Nginx đóng vai trò là "cổng bảo vệ", giúp điều hướng nhiều dịch vụ khác nhau (Website, API, Node-RED) chỉ qua một cổng duy nhất (80/443).
Bảo mật & Hiệu năng: Nginx xử lý các file tĩnh (HTML, CSS) nhanh hơn Node-RED và hỗ trợ tốt các tính năng chặn IP xấu, giới hạn băng thông trước khi yêu cầu chạm đến Node-RED.

2. Sự khác biệt giữa việc Mount file và Mount thư mục trong Docker là gì?
Mount thư mục: Docker sẽ đồng bộ toàn bộ nội dung trong thư mục đó giữa máy Ubuntu và Container. Thường dùng cho code web hoặc dữ liệu (như thư mục nodered).
Mount file: Chỉ đồng bộ duy nhất 1 file cụ thể (như file nginx.conf). Điều này giúp tránh việc Container vô tình tạo ra các file rác trong thư mục hệ thống của máy Ubuntu.

3. Nếu thay đổi file index.html ở máy Ubuntu, nội dung trên web có thay đổi ngay không? Tại sao?
Có, thay đổi ngay lập tức. * Tại sao: Vì chúng ta đã sử dụng tính năng Volume (Bind Mount). File trên máy Ubuntu và file trong Container thực chất là "hai là một". Khi Nam lưu file, Nginx trong container sẽ đọc trực tiếp nội dung mới nhất từ ổ đĩa đó mà không cần khởi động lại container.

4. restart: always hoặc restart: unless-stopped để làm gì?
Mục đích: Đảm bảo tính sẵn sàng cao của hệ thống.
always: Container sẽ tự động bật lại trong mọi trường hợp (lỗi crash, máy Ubuntu khởi động lại).
unless-stopped: Tương tự always, nhưng nếu Nam chủ động gõ lệnh docker stop thì nó sẽ không tự bật lại cho đến khi Nam ra lệnh Start.

5. Cách khai báo dùng chung 1 network? Lợi ích là gì?
Cách làm: Trong mỗi service, thêm đoạn:
```networks:
  - myapp_network```

Lợi ích: Các container có thể nói chuyện với nhau bằng Tên Service thay vì địa chỉ IP (IP Docker thường thay đổi sau mỗi lần restart). Điều này giúp cấu hình luôn ổn định.

6. Tại sao phải dùng file .env và .gitignore cho Cloudflare Token?
Bảo mật: Token là "chìa khóa" vào nhà Nam. Nếu push Token lên GitHub, ai cũng có thể điều khiển Tunnel của Nam.
Nguyên tắc: File .env lưu mật khẩu/token, và .gitignore dặn GitHub "đừng có upload file này lên". Đây là quy tắc bảo mật mã nguồn cơ bản trong ngành IT.

7. Tại sao nên thêm hậu tố :ro khi mount file cấu hình Nginx?
:ro (Read-Only): Nghĩa là "Chỉ cho phép đọc".
Lý do: Giúp bảo vệ file cấu hình gốc trên máy Ubuntu. Ngăn chặn trường hợp Container bị hacker tấn công rồi từ bên trong Container chúng quay ngược lại sửa đổi hoặc xóa file cấu hình trên máy thật của Nam.

8. Khi dùng Cloudflare Tunnel: có cần thiết phải mở cổng cho các service nữa không?
Không cần.
Lý do: Tunnel tạo ra một "đường hầm" hướng tâm (outbound) từ máy Nam ra Cloudflare. Dữ liệu đi qua đường hầm này nên không cần NAT Port trên Modem hay mở firewall cổng 80/443 nữa. Điều này giúp máy chủ của Nam hoàn toàn ẩn danh trên Internet.
