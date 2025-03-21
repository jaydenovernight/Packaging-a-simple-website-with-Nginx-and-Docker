## Bước 1: Tạo Dockerfile cho frontend

```
# Bước 1: Sử dụng Node.js image làm base
FROM node:16

# Bước 2: Đặt thư mục làm việc trong container
WORKDIR /app

# Bước 3: Copy package.json và package-lock.json vào container
COPY package*.json ./

# Bước 4: Cài đặt dependencies (thư viện frontend)
RUN npm install

# Bước 5: Copy toàn bộ mã nguồn vào container
COPY . .

# Bước 6: Build ứng dụng frontend
RUN npm run build

# Bước 7: Mở port 3000 để truy cập ứng dụng frontend
EXPOSE 3000

# Bước 8: Chạy ứng dụng frontend
CMD ["npm", "start"]

```

---

## Bước 2: Xây dựng Docker image
```
docker build -t frontend-app .
```

---

## Bước 3: Chạy container cho frontend
```
docker run -d --name frontend-app -p 3000:3000 frontend-app
```
Sau khi container frontend được chạy, bạn có thể truy cập vào frontend qua http://localhost:3000
