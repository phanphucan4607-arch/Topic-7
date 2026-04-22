###  Mục tiêu bài học (Objectives)

+ Chuyển đổi môi trường quản trị (Migration): Hiểu cách di chuyển dữ liệu giữa các Control Panel khác nhau (từ aaPanel sang CyberPanel) mà vẫn đảm bảo tính nguyên vẹn của dữ liệu và cấu hình.

 + Tối ưu hóa Web Server với OpenLiteSpeed: Làm quen với cách cấu hình đặc thù của OpenLiteSpeed (thay vì Nginx hay Apache truyền thống).

+ Tích hợp đa dịch vụ (Microservices Architecture): Biết cách chạy song song một website truyền thống (WordPress) và một ứng dụng hiện đại (Python/Node.js) trên cùng một máy chủ.

+ Kỹ thuật Proxy ngược (Reverse Proxy): Làm chủ kỹ thuật điều hướng yêu cầu (request) từ người dùng đến các ứng dụng chạy trên các cổng (port) nội bộ khác nhau.

**A. Quản lý chứng chỉ số (SSL Management)**

    Nắm được cách Certbot lưu trữ chứng chỉ trong /etc/letsencrypt/.

    Hiểu rằng chứng chỉ SSL là các tệp văn bản mã hóa, có thể sao chép và tái sử dụng trên các máy chủ khác nhau miễn là tên miền không đổi.

**B. Môi trường chạy ứng dụng (Application Environment)**
+ cách khởi tạo một dich vụ chạy trên cổng nội bộ (localhost:5000)
+ sự khác biệt giữa truy cập qua cổng (ip:5000) và truy cập gián tiếp qua tên miền (Doamin/api)


### Cấu trúc hệ thống Hybrid Web Service

| Thành phần | Vai trò | Công nghệ |
| :--- | :--- | :--- |
| **Lớp ngoài (Mặt tiền)** | Tiếp nhận yêu cầu từ người dùng và hiển thị nội dung website chính. | WordPress / Laravel (Cổng 80/443) |
| **Lớp trong (Hậu trường)** | Xử lý các logic nghiệp vụ riêng biệt, tính toán dữ liệu hoặc cung cấp API. | Python Flask (Cổng nội bộ 5000) |
| **Cầu nối (Proxy)** | Điều hướng lưu lượng từ đường dẫn `/api` về ứng dụng chạy tại cổng 5000. | OpenLiteSpeed ProxyPass |


### Giai đoạn 1: Cài đặt CyberPanel và Thiết lập Website

Bước 1: Cài đặt CyberPanel lên VPS
```
sh <(curl https://cyberpanel.net/install.sh || wget -O - https://cyberpanel.net/install.sh)
```

+ Chọn số 1 (Install CyberPanel).
<img width="580" height="282" alt="image" src="https://github.com/user-attachments/assets/80d0b28e-546d-4035-970a-465677759a85" />

+ Chọn số 1 (Install CyberPanel with OpenLiteSpeed).
  <img width="580" height="282" alt="image" src="https://github.com/user-attachments/assets/daebacbf-abf3-4330-b718-e6a7d375791a" />

Visit: https://221.132.21.141:8090
                 
Panel username: admin                              

Panel password: geHbpcRSYtYqUtBJ  

<img width="689" height="90" alt="image" src="https://github.com/user-attachments/assets/2a5f8a4e-0b25-4be2-9c16-44488e1a6d5e" />
