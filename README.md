# **🌐 Bài tập 3 - Phát triển ứng dụng trên nền web**
**Giảng viên:** Đỗ Duy Cốp  
**Lớp học phần:** 58KTP  
**Sinh viên thực hiện:** Nguyễn Tuấn Anh  
**MSSV:** K225480106095  
**Chủ đề:** Lập trình ứng dụng web thương mại điện tử (web chuyên bán pc) trên nền Linux (Docker + docker desktop)
## **🧩 1. GIỚI THIỆU CHUNG**
Trong thời đại công nghệ 4.0, thương mại điện tử (E-commerce) trở thành một phần quan trọng trong kinh doanh. Dự án này tập trung xây dựng một **ứng dụng web bán PC**, giúp khách hàng dễ dàng tìm kiếm, lựa chọn và mua các linh kiện, phụ kiện hoặc máy tính hoàn chỉnh. Hệ thống được triển khai trên nền Linux sử dụng **Docker Desktop** để quản lý các container, giúp việc triển khai, bảo trì và mở rộng hệ thống trở nên dễ dàng và linh hoạt.  

### **Mục tiêu chính của dự án:**  

**1. Giao diện thân thiện với người dùng:** duyệt sản phẩm, thêm vào giỏ hàng, thanh toán và quản lý đơn hàng.  
**2. API backend ổn định:** quản lý dữ liệu sản phẩm, giỏ hàng, đơn hàng và người dùng.  
**3. Triển khai dễ dàng:** Docker Desktop giúp đóng gói frontend, backend và cơ sở dữ liệu, chạy được trên nhiều hệ điều hành.  
**4. Mở rộng linh hoạt:** tích hợp thanh toán trực tuyến, quản lý kho hàng hoặc thêm tính năng mới trong tương lai.  

### **Công nghệ sử dụng:**
**- Frontend:** HTML, CSS, JavaScript, framework SPA (React hoặc Vue.js).  
**- Backend:** Node.js + Express (hoặc Python Flask/Django).  
**- Database:** MySQL/PostgreSQL.  
**- Containerization:** Docker Desktop để quản lý container backend, frontend, database.  
**- Environment:** Linux (Ubuntu) chạy trong Docker Desktop.  
## **⚙️ 2. CẤU TRÚC DỰ ÁN**
```ecommerce-pc/
│
├── backend/                
│   ├── app.js              # Entry point của server
│   ├── routes/             # Route API (users, products, orders)
│   ├── controllers/        # Logic xử lý cho từng route
│   ├── models/             # Schema database
│   └── utils/              # Hàm tiện ích (hash password, JWT...)
│
├── frontend/               
│   ├── public/             # HTML, favicon, assets tĩnh
│   ├── src/
│   │   ├── components/     # Component UI (Navbar, ProductCard…)
│   │   ├── pages/          # Các trang (Home, ProductDetail, Cart, Checkout)
│   │   ├── services/       # Gọi API backend
│   │   └── App.js
│   └── package.json
│
├── database/               
│   ├── init.sql            # Script khởi tạo bảng và dữ liệu mẫu
│   └── docker-compose.yml  # Cấu hình MySQL/PostgreSQL
│
├── docker/                 
│   ├── Dockerfile.backend
│   ├── Dockerfile.frontend
│   └── docker-compose.yml  # Kết hợp frontend, backend, database
│
├── docs/                   
└── README.md               
```

## **🧱 3. CÀI ĐẶT MÔI TRƯỜNG**  
### **Bước 1️⃣: Cài đặt Docker Desktop**
1. Tải về Docker Desktop  
- Truy cập trang chính thức của Docker (https://www.docker.com/products/docker-desktop/).  
- Nhấn vào nút “Download for Windows” và lưu file cài đặt .exe về máy.  
2. Cài đặt Docker Desktop  
- Mở file cài đặt vừa tải về (Docker Desktop Installer.exe).  
- Trong quá trình cài đặt, tick chọn:  
- Install required components for WSL 2 (nếu chưa cài).  
- Add shortcut to desktop (nếu muốn).  
- Nhấn OK và đợi quá trình cài đặt hoàn tất.  
3. Khởi động và cấu hình lần đầu  
- Sau khi cài xong, khởi động lại máy (nếu được yêu cầu).  
- Mở Docker Desktop từ Start Menu hoặc biểu tượng trên desktop.  
- Chấp nhận các điều khoản sử dụng (License Agreement).  
- Docker sẽ tự động khởi động nền tảng WSL 2 và cấu hình mặc định.  
4. Kiểm tra Docker đã hoạt động  
### **Bước 2️⃣:Cấu hình docker compose**
1. File docker compose:
```
services:
  mariadb:
    image: mariadb:latest
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: appdb
      MYSQL_USER: user
      MYSQL_PASSWORD: user123
    volumes:
      - ./data/mariadb:/var/lib/mysql
    ports:
      - "3306:3306"
  
  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    restart: always
    environment:
      PMA_HOST: mariadb
      PMA_USER: root
      PMA_PASSWORD: root
    ports:
      - "8080:80"
    depends_on:
      - mariadb

  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    restart: always
    ports:
      - "1880:1880"
    volumes:
      - ./data/nodered:/data

  influxdb:
    image: influxdb:latest
    container_name: influxdb
    restart: always
    ports:
      - "8086:8086"
    volumes:
      - ./data/influxdb:/var/lib/influxdb

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: always
    ports:
      - "3000:3000"
    depends_on:
      - influxdb
    volumes:
      - ./data/grafana:/var/lib/grafana

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./web:/usr/share/nginx/html:ro
    depends_on:
      - nodered
      - grafana
```
3. Mở PowerShell
4. Trỏ đến folder chứa project  
5. Chạy "docker compose up -d"
<img width="255" height="155" alt="image" src="https://github.com/user-attachments/assets/071e72fe-1f14-45d2-9d76-c071a66ec6f2" />
   
### **Bước 3️⃣: cấu hình file nginx**
```server {
    listen 80;
    server_name nguyentuananh095.com www.nguyentuananh095.com;

    root /var/www/nguyentuananh095.com/frontend;
    index index.html;

    # Gửi request API đến Node-RED (chạy cổng 1880)
    location /api/ {
        proxy_pass http://localhost:1880/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    # Tất cả request khác → index.html (SPA)
    location / {
        try_files $uri /index.html;
    }

    access_log /var/log/nginx/nguyentuananh095.access.log;
    error_log  /var/log/nginx/nguyentuananh095.error.log;
}
```

