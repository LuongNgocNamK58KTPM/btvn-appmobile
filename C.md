## Tạo thư mục: ~/myapp
Chuyển vào trong thư mục ~/myapp

Tạo thư mục: ./myweb

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/07d4dc5e-509f-4ef5-b7c5-0fd102e21c3f" />

Tạo file ./myweb/index.html (với nội dung là thông tin cá nhân của em)

File html
<img width="1138" height="675" alt="image" src="https://github.com/user-attachments/assets/8e12ee0d-7015-48a5-bdda-72d57bd14646" />
code htlm
```
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Báo cáo Lab IoT - Lương Ngọc Nam</title>
    
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">

    <style>
        body { background-color: #f4f6f9; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        .main-card { border: none; border-radius: 15px; box-shadow: 0 10px 20px rgba(0,0,0,0.1); background: #fff; transition: transform 0.3s; }
        .main-card:hover { transform: translateY(-5px); }
        .api-box { background: #e8f0fe; border-left: 5px solid #1a73e8; border-radius: 8px; padding: 15px; margin-top: 20px; transition: all 0.5s ease; }
        .api-title { color: #1a73e8; font-weight: bold; font-size: 1.1rem; }
        #api-result { color: #3c4043; font-size: 1.2rem; margin-top: 10px; }
        .status-badge { font-size: 0.8rem; padding: 5px 10px; border-radius: 20px; }
    </style>
</head>
<body>

    <div class="container py-5">
        <div class="row justify-content-center">
            <div class="col-md-8 col-lg-6">
                
                <div class="card main-card p-4 text-center">
                    <div class="card-body">
                        
                        <div class="mb-4">
                            <i class="fas fa-network-wired fa-3x text-primary"></i>
                            <h2 class="card-title mt-3 font-weight-bold">BÁO CÁO THỰC HÀNH IOT</h2>
                            <p class="text-muted">Triển khai IoT Stack (Nginx + Node-RED + Tunnel)</p>
                        </div>
                        
                        <div class="info-group mb-4 text-start p-3 bg-light rounded-3">
                            <p class="mb-2"><i class="fas fa-user text-muted me-2"></i>Họ và tên: <b class="text-primary">Lương Ngọc Nam</b></p>
                            <p class="mb-2"><i class="fas fa-id-card text-muted me-2"></i>MSSV: <b class="text-primary">K225480106049</b></p>
                            <p class="mb-0"><i class="fas fa-envelope text-muted me-2"></i>Email: <b>k225480106049@sv.tnut.edu.vn</b></p>
                        </div>
                        
                        <hr>
                        
                        <div class="mt-4">
                            <div class="d-flex justify-content-between align-items-center">
                                <h5 class="text-muted"><i class="fas fa-exchange-alt me-2"></i>Kết nối API Node-RED</h5>
                                <span id="status-badge" class="badge bg-warning text-dark status-badge"><i class="fas fa-spinner fa-spin"></i> Đang gọi...</span>
                            </div>
                            
                            <div id="api-box" class="api-box text-start">
                                <p class="api-title mb-1">Tin nhắn phản hồi:</p>
                                <p id="api-result">Đang chờ dữ liệu...</p>
                            </div>
                        </div>

                    </div>
                </div>

                <p class="text-center text-muted mt-4 font-size-sm">© 2024 Lương Ngọc Nam - TNUT. All rights reserved.</p>
                
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
    
    <script>
        fetch('/api/hello')
            .then(response => {
                if (!response.ok) throw new Error('Kết nối API lỗi!');
                return response.json();
            })
            .then(data => {
                // Hiển thị dữ liệu
                document.getElementById('api-result').innerText = data.message;
                // Cập nhật trạng thái
                const badge = document.getElementById('status-badge');
                badge.classList.remove('bg-warning', 'text-dark');
                badge.classList.add('bg-success');
                badge.innerHTML = '<i class="fas fa-check-circle"></i> Đã kết nối';
            })
            .catch(err => {
                // Xử lý lỗi
                document.getElementById('api-result').innerHTML = "<b>Lỗi nghiêm trọng:</b> Không thể gọi API!";
                document.getElementById('api-box').style.borderColor = 'red';
                document.getElementById('api-box').style.background = '#ffebee';
                // Cập nhật trạng thái lỗi
                const badge = document.getElementById('status-badge');
                badge.classList.remove('bg-warning', 'text-dark');
                badge.classList.add('bg-danger');
                badge.innerHTML = '<i class="fas fa-times-circle"></i> Lỗi kết nối';
            });
    </script>
</body>
</html>
```

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/d36bdb4b-9940-4900-9c56-31a9703fe128" />

## Tạo file docker-compose.yml để nó sẽ có các dịch vụ sau:

Khai báo sử dụng nodered/node-red, cổng 1880, dữ liệu nằm tại thư mục ./nodered

Khai báo sử dụng nginx, cổng 80, cấu hình trong file ./nginx/nginx.conf

Mount thư mục ./myweb thành thư mục /myweb trong nginx

Mount file ./nginx/nginx.conf vào file /etc/nginx/nginx.conf trong nginx

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/3f1d2264-e6d3-4d81-a4b9-d4ae9e973ecc" />

code docker-compose.yml
```
services:
  nodered:
    image: nodered/node-red
    container_name: mynodered
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data
    restart: always

  mynginx:
    image: nginx
    container_name: mynginx
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    restart: always

  mycloudflared:
    image: cloudflare/cloudflared:latest
    container_name: mycloudflared
    command: tunnel --no-autoupdate run --token eyJhIjoiNjA2ZDY3YmNhNWEyNjIyMDNjYzk4ZTE5NGFiYWIwODEiLCJ0IjoiOWE1MTc4MWQtMTcyZS00ZTU3LTkwMzAtMWMyODRmMDE2ODY3IiwicyI6IlpqUTVNakZtT1RJdFlUUTVPQzAwTmpjM0xUZ3lOR0V0T1RJMk5EWmhZVFZtTVRndyJ9
    restart: always
```
## Edit file ./nginx/nginx.conf để:
Cấu hình web server cổng 80

server_name là sub-domain (sub-domain tuỳ ý của em)

location / trỏ tới root là thư mục /myweb

location /api dùng proxy_pass trỏ tới 1 (hoặc nhiều) node http_in của nodered

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/a42eca48-639c-44fd-b642-15714b21e020" />

code nginx.conf
```
events {}
http {
    server {
        listen 80;
        server_name web.luongnam2004.id.vn;

        location / {
            root /myweb;
            index index.html;
        }

        location /api {
            proxy_pass http://nodered:1880;
        }
    }
}
```
## Edit file ./nodered/settings.js để nodered bắt buộc đăng nhập
Chạy docker-compose lần đầu để Node-RED tự sinh file cấu hình trong thư mục ./nodered, sau đó mới tiến hành sửa settings.js và restart lại container

<img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/1f6b0a0b-fb9f-4789-a30e-054ec63aa5eb" />

code settings.js
```
module.exports = {
    flowFile: 'flows.json',
    userDir: '/data',
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "$2b$08$/phhOdJOsJ8tm6ovdf7QIufCOOOWYTisvr8wcUB3rU9YoJv83LBJS",
            permissions: "*"
        }]
    },
    functionGlobalContext: { },
        logging: { console: { level: "info", metrics: false, audit: false } }
}
```
Kết quả
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/fc654a5f-44b9-4603-86d4-e8ab1ba8aa23" />

