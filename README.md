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
<img width="626" height="709" alt="image" src="https://github.com/user-attachments/assets/220b95e8-0430-4ddf-94f3-eecc8ad7cc11" />  

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


