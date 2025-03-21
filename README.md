# 📦 Packaging a Simple Website with Nginx and Docker

## 📝 Mô tả
Hướng dẫn từng bước đóng gói và triển khai một trang web tĩnh đơn giản bằng **Nginx** và **Docker**.

---

## 1. Tạo thư mục và tệp cần thiết

Trước tiên, cần tạo một thư mục chứa trang web và các file cần thiết (ở đây chỉ cần 1 file text html)

```bash
mkdir my-web && cd my-web  # Tạo thư mục "my-web" và di chuyển vào đó
touch index.html           # Tạo file HTML của trang web
touch Dockerfile           # Tạo file cấu hình Docker
```
---

## 2. Viết trang web đơn giản (HTML)

Đây là trang web của bạn. Khi truy cập, trình duyệt sẽ hiển thị nội dung của file này.

File này cần đặt vào đúng thư mục mà Nginx có thể đọc.

Mở file index.html và thêm nội dung sau:
```
<h1>Hello I'm from lab cô Hà!</h1>
<p>This website is running in Docker Container with Nginx!</p>
```

---

## 3. Viết Dockerfile để đóng gói thành Docker Image.
Dockerfile là công thức để tạo Docker Image.

Chúng ta sẽ sử dụng Nginx để làm web server.

Chúng ta cần copy file index.html vào thư mục mặc định của Nginx để nó hiển thị trang web.

Mở file Dockerfile và thêm nội dung sau:
```dockerfile
# Sử dụng image Nginx mới nhất làm nền tảng
FROM nginx:latest  

# Copy file index.html vào thư mục gốc của Nginx trong container
COPY index.html /usr/share/nginx/html/index.html  

# Mở cổng 80 (cổng mặc định của Nginx)
EXPOSE 80  

# Chạy Nginx ở chế độ foreground (không chạy ngầm)
CMD ["nginx", "-g", "daemon off;"]
```

### 🔍 Giải thích từng dòng:

1️⃣ `FROM nginx:latest` → Lấy image Nginx mới nhất từ Docker Hub.  
2️⃣ `COPY index.html /usr/share/nginx/html/index.html` → Sao chép file web vào thư mục mặc định của Nginx.  
3️⃣ `EXPOSE 80` → Mở cổng 80 để nhận kết nối HTTP.  
4️⃣ `CMD ["nginx", "-g", "daemon off;"]` → Chạy Nginx ở chế độ foreground để container không bị dừng.  

---

## 4. Tạo Docker Image từ Dockerfile.
Giải thích:

Bây giờ chúng ta cần tạo Docker Image từ Dockerfile.

Docker sẽ đọc Dockerfile, lấy Nginx, copy index.html vào, và tạo một Image có thể chạy.

Chạy lệnh sau để tạo Image:
```bash
docker build -t my-nginx .
```

🔍 **Giải thích lệnh:**
- `docker build` → Dùng để tạo Docker Image.
- `-t my-nginx` → Đặt tên cho image là **my-nginx**.
- `.` → Chỉ định Dockerfile nằm trong thư mục hiện tại.

⏳ **Nếu thành công -> thông báo `FINISHED`!**

---

## 5. Chạy Container từ Image

Giải thích:

Chúng ta cần chạy Container từ Image vừa tạo.

Container sẽ dùng Nginx để phục vụ trang web.

Chúng ta sẽ ánh xạ cổng 8080 trên máy vào cổng 80 trong Container.

📌 Chạy lệnh sau để khởi động Container:
```bash
docker run -d -p 8080:80 my-nginx
```

Giải thích lệnh:

1️⃣ docker run → Chạy một Container mới.

2️⃣ -d → Chạy ở chế độ nền (detached mode).

3️⃣ -p 8080:80 → Chuyển tiếp cổng:

Cổng 8080 trên máy (host) → Cổng 80 trong Container (Nginx).

4️⃣ my-nginx → Chạy từ Image có tên my-nginx.

✅ Sau khi chạy xong, Container đang chạy nền và web đã hoạt động!

## 6. Truy cập vào trang web.

Mở trình duyệt, nhập địa chỉ:
```
http://localhost:8080  
```


