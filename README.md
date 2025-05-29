# 🚀 Vaultwarden Self-Hosted (Bitwarden Alternative)

Vaultwarden là bản clone nhẹ, mã nguồn mở của Bitwarden, cho phép bạn tự host hệ thống quản lý mật khẩu an toàn, dễ sử dụng.

---

## 📦 Cấu hình Docker Compose

Tạo file `docker-compose.yml`:

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
