#Sử dụng lệnh cd ~/myapp để vào thư mục myapp
#Kiểm tra các container đang chạy trong docker, nếu có cái nào bị restart cần tìm lỗi rồi edit lại docker-compose.yml
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/7be16672-dbc1-484a-9bc2-71f6ce351662" />
#Kiểm tra kiểm thử các service đang chạy độc lập thông qua ip và port của nó: ví dụ mở trình duyệt ip_ubuntu:1880 để check nodered đã chạy chưa
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/c09917f6-aa1a-4cde-bdbf-9aae3528ba89" />
#Sử dụng nodered: kéo nodered http_in , http_response, function : để tạo api get đơn giản (dùng cho /api proxy_pass của nginx)
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/098c3740-7420-40b8-9206-0e69487d2eab" />
#Sửa lại file html
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/707eabdf-e20c-4d33-8d00-79239c7e2a75" />
code html
```<!DOCTYPE html>
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
                            <p class="mb-0"><i class="fas fa-envelope text-muted me-2"></i>Email: <b>k225480106049@tnut.edu.vn</b></p>
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
#kết quả
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/014a945b-4466-4884-b690-6db785e92201" />
