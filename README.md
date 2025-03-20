# Packaging-a-simple-website-with-Nginx-and-Docker

# 1. Tạo thư mục và file cần thiết.
// Chúng ta cần một thư mục để chứa trang web.
// Cần một file index.html để hiển thị nội dung web.
// Cần một file Dockerfile để chỉ định cách đóng gói trang web này vào Docker.

mkdir my-web && cd my-web  # Tạo thư mục "my-web" và vào thư mục đó
touch index.html           # Tạo file index.html (file HTML của trang web)
touch Dockerfile           # Tạo file Dockerfile (file cấu hình Docker)


# 2. Viết trang web đơn giản (HTML).

Đây là trang web của bạn. Khi truy cập, trình duyệt sẽ hiển thị nội dung của file này.
File này cần đặt vào đúng thư mục mà Nginx có thể đọc.

Mở file index.html và thêm nội dung sau:
<h1>Hello I'm from lab cô Hà!</h1>
<p>Trang web này đang chạy trong Docker Container với Nginx!</p>


# 3. Viết Dockerfile để đóng gói thành Docker Image.
Dockerfile là công thức để tạo Docker Image.
Chúng ta sẽ sử dụng Nginx để làm web server.
Chúng ta cần copy file index.html vào thư mục mặc định của Nginx để nó hiển thị trang web.

Mở file Dockerfile và thêm nội dung sau:
# Sử dụng Nginx làm nền tảng
FROM nginx:latest

# Copy file index.html từ thư mục hiện tại vào thư mục gốc của Nginx trong container
COPY index.html /usr/share/nginx/html/index.html

# Mở cổng 80 (cổng web của Nginx)
EXPOSE 80

# Chạy Nginx ở chế độ foreground (chạy liên tục)
CMD ["nginx", "-g", "daemon off;"]

Giải thích từng dòng:
1️⃣ FROM nginx:latest → Lấy image Nginx mới nhất từ Docker Hub.
2️⃣ COPY index.html /usr/share/nginx/html/index.html → Chép file index.html vào thư mục Nginx.
3️⃣ EXPOSE 80 → Khai báo cổng 80, đây là cổng mà Nginx lắng nghe để nhận request.
4️⃣ CMD ["nginx", "-g", "daemon off;"] → Chạy Nginx và giữ nó chạy mãi (nếu không container sẽ tự tắt).

# 4. Tạo Docker Image từ Dockerfile.
Giải thích:

Bây giờ chúng ta cần tạo Docker Image từ Dockerfile.
Docker sẽ đọc Dockerfile, lấy Nginx, copy index.html vào, và tạo một Image có thể chạy.
Chạy lệnh sau để tạo Image:
docker build -t my-nginx .

Giải thích lệnh:
1️⃣ docker build → Lệnh tạo Docker Image.
2️⃣ -t my-nginx → Đặt tên cho Image là my-nginx.
3️⃣ . → Chỉ định Dockerfile nằm trong thư mục hiện tại.

⏳ Chờ một chút để Docker tạo image. Nếu thành công, bạn sẽ thấy dòng Successfully built


# 5. Chạy Container từ Image.

Giải thích:

Chúng ta cần chạy Container từ Image vừa tạo.
Container sẽ dùng Nginx để phục vụ trang web.
Chúng ta sẽ ánh xạ cổng 8080 trên máy vào cổng 80 trong Container.
📌 Chạy lệnh sau để khởi động Container:
docker run -d -p 8080:80 my-nginx

Giải thích lệnh:
1️⃣ docker run → Chạy một Container mới.
2️⃣ -d → Chạy ở chế độ nền (detached mode).
3️⃣ -p 8080:80 → Chuyển tiếp cổng:

Cổng 8080 trên máy (host) → Cổng 80 trong Container (Nginx).
4️⃣ my-nginx → Chạy từ Image có tên my-nginx.
✅ Sau khi chạy xong, Container đang chạy nền và web đã hoạt động!

# 6. Truy cập vào trang web.
Mở trình duyệt, nhập địa chỉ:

http://localhost:8080  



