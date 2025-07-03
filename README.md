# 🚀 Vaultwarden Self-Hosted (Bitwarden Alternative)

**Vaultwarden** là phiên bản nhẹ, mã nguồn mở của Bitwarden, cho phép bạn tự triển khai hệ thống quản lý mật khẩu riêng tư, an toàn và dễ sử dụng.

---

## 🧰 Yêu cầu trước khi bắt đầu

- ✅ Hướng dẫn máy chủ chạy **Ubuntu**, Debian hoặc hệ điều hành Linux tương đương
- ✅ **Docker** và **Docker Compose** đã được cài đặt

> Nếu chưa cài Docker:
> ```bash
> curl -fsSL https://get.docker.com | bash
> sudo apt install docker-compose -y
> ```

Kiểm tra Docker đã cài:

```bash
docker -v
docker-compose -v
```

---

## 📦 Bước 1: Tạo file `docker-compose.yml`

Tạo thư mục chứa project:

```bash
mkdir -p ~/vaultwarden && cd ~/vaultwarden
```

Tạo file `docker-compose.yml` với nội dung:

```yaml
version: '3'

services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      DOMAIN: "https://vault.exemple.com"   # ⚠️ Thay bằng domain thực tế
      ADMIN_TOKEN: "123456789_supersecret_token"  # 🔐 Dùng để vào trang quản trị
    ports:
      - "8003:80"  # Truy cập tại http://<IP>:8003
    volumes:
      - ./data:/data  # Lưu trữ dữ liệu vĩnh viễn
```

---

## ▶️ Bước 2: Khởi chạy Vaultwarden

Từ thư mục chứa file `docker-compose.yml`, chạy:

```bash
docker-compose up -d
```

Lệnh này sẽ:
- Tải image Vaultwarden mới nhất
- Tạo container có tên `vaultwarden`
- Tự động restart nếu server reboot

---

## ✅ Bước 3: Truy cập Vaultwarden

- Nếu dùng IP: `http://<SERVER_IP>:8003`
- Nếu có domain và reverse proxy (Nginx): `https://vault.example.com`

Trang quản trị (admin interface):

```plaintext
http://<SERVER_IP>:8003/admin
```

→ Nhập đúng `ADMIN_TOKEN` để truy cập.

---

## 💾 Bước 4: Dữ liệu được lưu ở đâu?

Vaultwarden lưu trữ dữ liệu người dùng, file đính kèm, thiết lập… tại thư mục `./data` (tức là `~/vaultwarden/data` trên host).

---

## 🔒 Gợi ý bảo mật

- Luôn dùng Vaultwarden kèm **HTTPS**
- Thêm **Nginx reverse proxy + SSL (Let's Encrypt)** nếu dùng domain
- Không chia sẻ `ADMIN_TOKEN`
- Đặt firewall nếu chỉ dùng nội bộ

---
