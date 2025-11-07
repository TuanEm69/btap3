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
- Cấu hình nginx để chạy được website qua url http://nguyentuananh095.com  (thay fullname bằng chuỗi ko dấu viết liền tên của bạn)
 - Cấu hình nginx để http://nguyentuananh095.com/nodered truy cập vào nodered qua cổng 80, (dù nodered đang chạy ở port 1880)
 - Cấu hình nginx để http://nguyentuananh095.com/grafana truy cập vào grafana qua cổng 80, (dù grafana đang chạy ở port 3000)
```events {}

http {
  server {
    listen 80;
    server_name nguyentuananh095.com;

    # Website chính (index.html)
    location / {
      root /usr/share/nginx/html;
      index index.html;
      try_files $uri $uri/ =404;
    }

    # --- Proxy tới Node-RED ---
    location /nodered/ {
      proxy_pass http://nodered:1880/;
      proxy_http_version 1.1;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_cache_bypass $http_upgrade;

      # Xử lý redirect nội bộ để không mất /nodered/
      proxy_redirect / /nodered/;
    }

    # --- Proxy tới Grafana ---
    location /grafana/ {
      proxy_pass http://grafana:3000/;
      proxy_http_version 1.1;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_cache_bypass $http_upgrade;

      # Giữ prefix /grafana/
      proxy_redirect / /grafana/;
    }
  }
}
```
## **4. Lập trình web frontend+backend**
 ### 4.1 Web thương mại điện tử
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3edaa756-1a56-41ab-8969-5081bc495da1" />

 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6e48a11d-a1d5-4741-a85f-d9f6eac29226" />

   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c74a25d4-6d6c-4c89-8018-063ca73a7e11" />

   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - Có tính năng liệt kê các sản phẩm bán chạy ra trang chủ
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/658457ff-8940-423d-9625-f151828398fb" />

 - Có tính năng liệt kê các nhóm sản phẩm
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/73f299bd-7964-4f7c-8d57-9c94215364f7" />

 - Có tính năng liệt kê sản phẩm theo nhóm
 - Có tính năng tìm kiếm sản phẩm
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5f648877-9179-4dd7-8b1d-18aa9788b8c8" />

 - Có tính năng chọn sản phẩm (đưa sản phẩm vào giỏ hàng, thay đổi số lượng sản phẩm trong giỏ, cập nhật tổng tiền)
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fc89349d-a408-4467-9e1b-c6a226a5573d" />

 - Có tính năng đặt hàng, nhập thông tin giao hàng => được 1 đơn hàng.
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/973221bc-9b30-4329-9bba-54c3d8505dcc" />

 - Có tính năng dành cho admin: Thống kê xem có bao nhiêu đơn hàng, call để xác nhận và cập nhật thông tin đơn hàng. chuyển cho bộ phận đóng gói, gửi bưu điện, cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...
 - Có tính năng dành cho admin: biểu đồ thống kê số lượng mặt hàng bán được trong từng ngày. (sử dụng grafana)
   
   Bài em chưa có tính năng này cho admin;
 - backend: sử dụng nodered xử lý request gửi lên từ javascript, phản hồi về json.
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9b36a913-df9e-4c09-9c03-a9d8d5a3f3ed" />
    <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d83d3334-6163-4179-9c89-65ca19239265" />
