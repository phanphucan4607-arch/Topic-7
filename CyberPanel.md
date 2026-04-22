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

#### Bước 1: Cài đặt CyberPanel lên VPS
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

#### Bước 2. Tạo Website & Cài đặt SSL
<img width="1920" height="1080" alt="Screenshot from 2026-04-22 10-02-07" src="https://github.com/user-attachments/assets/b3c07313-5b05-4507-9b1b-8be00239eb4b" />

+ cáu hình SSL nhấn Issue SSL
<img width="1143" height="728" alt="image" src="https://github.com/user-attachments/assets/240e496c-4b5c-4bc7-ba7f-f532f52bfd05" />
  
#### Bước 4. Upload dữ liệu và database

Dữ liệu: Vào File Manager của website mới tạo -> Vào thư mục public_html. Upload file nén .zip của web cũ lên và giải nén (Extract) tại đây.

<img width="1143" height="728" alt="image" src="https://github.com/user-attachments/assets/ec012eeb-3dfb-4533-aeb0-ab88f8634297" />

+ database: Vào Databases -> Create Database. Lưu lại tên DB, User, Password.

<img width="1152" height="956" alt="image" src="https://github.com/user-attachments/assets/5ccc2edc-7475-448e-9b38-bbc9d09f9ed0" />

Database Name:  wp_admin

user:  wp_admin  

pass: gOk2w3Lylremv!Nd


Database: * Vào Databases -> Create Database. Lưu lại tên DB, User, Password.

+ vào PHPMyAdmin vào cái data vừa mới tạo imporrt .sql cũ vào
<img width="1152" height="956" alt="image" src="https://github.com/user-attachments/assets/da833244-9eee-47df-bcc9-b4622b8195c5" />

nén và import file sql 
<img width="1152" height="956" alt="image" src="https://github.com/user-attachments/assets/5967b028-89ae-459a-bbbe-32620ccbc97e" />

Cấu hình: Mở file wp-config.php trong File Manager, sửa lại DB_NAME, DB_USER, DB_PASSWORD cho đúng với thông tin mới vừa tạo.
  <img width="1151" height="1007" alt="image" src="https://github.com/user-attachments/assets/fadc06ce-5c0e-4dda-a7eb-293b9389ac9a" />

+ Kiểm tra
https://wp.phucan.vietnix.tech/

<img width="1177" height="1001" alt="image" src="https://github.com/user-attachments/assets/c4ae9e3b-7210-40fe-b1f1-8127e63eea6a" />


#### Giai đoạn 2: Cài đặt ứng dụng Python (Port 5000)

tạo một ứng dụng Python Flask đơn giản để kiêm tra tính năng proxy
```
apt update && apt install python3-pip -y
pip3 install flask
```



#### Giai đoạn 3: Cấu hình ProxyPass trên OpenLiteSpeed
<img width="807" height="756" alt="image" src="https://github.com/user-attachments/assets/fed151f3-649b-479e-9e90-942b9a0b7ac0" />

**Bước 1. cấu hình proxy**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53bf939f-9df0-4603-9c31-b625ec6ad17a" />

Nhấn Add chọn web server
<img width="811" height="183" alt="image" src="https://github.com/user-attachments/assets/5e4fc412-8ca0-4137-82b5-500d8bf468d1" />

thông tin ta nhập

+ name: Python_Flask_API

+ Address: ta để ip localhost và cổng là 5000 (172.0.0.1: 5000)

+ Max Connections: 100

+ intial request timeout: 60

+ rerey Timeout: 0

==> save 
sau khi save thì nó sẽ hiện thông báo bắt mình restart lại nên ta click vào biểu tuợng sau
<img width="205" height="159" alt="image" src="https://github.com/user-attachments/assets/4d04176e-6f94-4711-9974-73a225534c07" />

quay lại tab Virtual Hosts vào doamin của mình 
<img width="873" height="415" alt="image" src="https://github.com/user-attachments/assets/fb4455ac-31cd-4d22-8e17-11220b2c9711" />

vào tab context cấu hình như sau 
<img width="853" height="531" alt="image" src="https://github.com/user-attachments/assets/a3fb60d5-3a0c-4126-822e-ca24e714c22f" />

==> save và restart lại 
<img width="851" height="169" alt="image" src="https://github.com/user-attachments/assets/c5604a7c-f273-42cc-b6b9-6837d5fbb34b" />
