# 🚀 Vaultwarden Self-Hosted (Bitwarden Alternative)

**Vaultwarden** là phiên bản nhẹ, mã nguồn mở của Bitwarden, cho phép bạn tự triển khai hệ thống quản lý mật khẩu riêng tư, an toàn và dễ sử dụng.

---

## 🧰 Yêu cầu trước khi bắt đầu

- ✅ Máy chủ chạy **Ubuntu**, Debian hoặc Linux tương đương
- ✅ Đã cài **Docker** và **Docker Compose**

> Nếu chưa cài Docker:
> ```bash
> curl -fsSL https://get.docker.com | bash
> sudo apt install docker-compose -y
> ```

Kiểm tra:

```bash
docker -v
docker-compose -v
```

---

## 📦 Bước 1: Tạo thư mục project

### 👉 Di chuyển đến nơi bạn muốn lưu project (ví dụ `/var/www` hoặc `/home`):

```bash
cd /var/www
```

> 📌 Bạn có thể chọn bất kỳ thư mục nào phù hợp với hệ thống của bạn.

### 👉 Tạo thư mục `vaultwarden` và di chuyển vào:

```bash
mkdir -p vaultwarden
cd vaultwarden
```

---

## 📄 Bước 2: Tạo file `docker-compose.yml`

Tạo file:

```bash
nano docker-compose.yml
```

Dán nội dung sau:

```yaml
version: '3'

services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      DOMAIN: "https://vault.example.com"   # ⚠️ Thay bằng domain thực tế
      ADMIN_TOKEN: "123456789_supersecret_token"  # 🔐 Dùng để vào trang quản trị
    ports:
      - "8003:80"
    volumes:
      - ./data:/data
```

---

## ▶️ Bước 3: Khởi chạy Vaultwarden

```bash
docker-compose up -d
```

---

## ✅ Bước 4: Truy cập

- IP: `http://<SERVER_IP>:8003`
- Domain (nếu có): `https://vault.example.com`

Trang admin:

```
http://<SERVER_IP>:8003/admin
```

---

## 💾 Dữ liệu lưu ở đâu?

```
./data  →  <thư mục hiện tại>/data
```

---

## 🔒 Gợi ý bảo mật

- Dùng HTTPS (Nginx + Let's Encrypt)
- Không lộ `ADMIN_TOKEN`
- Giới hạn truy cập bằng firewall nếu cần

---