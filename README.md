# Vaultwarden Self-Hosted

`Vaultwarden` là một phiên bản nhẹ của Bitwarden, phù hợp để tự host trình quản lý mật khẩu trên máy chủ riêng.

Repo này đã được cấu hình theo hướng:

- `docker-compose.yml` được commit lên GitHub
- `.env` chỉ dùng ở máy chạy thật, không commit
- `.env.example` được commit để làm file mẫu cấu hình

## Yêu cầu

- Docker
- Docker Compose plugin (`docker compose`)

Kiểm tra:

```bash
docker -v
docker compose version
```

## Cấu trúc file

```text
docker-compose.yml
.env.example
.env
data/
```

Ý nghĩa:

- `docker-compose.yml`: cấu hình chạy container
- `.env.example`: file mẫu để tham khảo và copy sang máy khác
- `.env`: file chứa giá trị thật như domain, token, SMTP
- `data/`: nơi lưu dữ liệu Vaultwarden

## Cách sử dụng

### 1. Clone repo

```bash
git clone <REPO_URL>
cd vaultwarden
```

### 2. Tạo file `.env` từ file mẫu

Linux:

```bash
cp .env.example .env
```

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

### 3. Sửa file `.env`

Cần cập nhật các giá trị trong `.env` cho đúng với môi trường thật:

```env
DOMAIN=https://vault.example.com
ADMIN_TOKEN=change_this_to_a_long_random_string
SIGNUPS_ALLOWED=false
INVITE_ONLY=true

SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURITY=starttls
SMTP_FROM=your-email@gmail.com
SMTP_FROM_NAME=Vaultwarden Security
SMTP_USERNAME=your-email@gmail.com
SMTP_PASSWORD=your_app_password
SMTP_AUTH_MECHANISM=Plain

VAULTWARDEN_PORT=8003
```

Giải thích nhanh:

- `DOMAIN`: domain dùng để truy cập Vaultwarden
- `ADMIN_TOKEN`: token để vào trang admin
- `SIGNUPS_ALLOWED=false`: không cho đăng ký tự do
- `INVITE_ONLY=true`: chỉ đăng ký qua lời mời
- `SMTP_*`: cấu hình gửi email
- `VAULTWARDEN_PORT`: cổng mở ra bên ngoài máy chủ

## Chạy dịch vụ

```bash
docker compose up -d
```

Kiểm tra log:

```bash
docker compose logs -f
```

Dừng dịch vụ:

```bash
docker compose down
```

## Truy cập

- Vaultwarden: `http://<SERVER_IP>:8003`
- Nếu dùng domain và reverse proxy: `https://vault.example.com`
- Trang admin: `http://<SERVER_IP>:8003/admin`

Nếu bạn đổi `VAULTWARDEN_PORT` trong `.env` thì thay `8003` bằng cổng mới.

## Dữ liệu lưu ở đâu

Dữ liệu được lưu trong thư mục:

```text
./data
```

Thư mục này cần được backup định kỳ.

## Làm việc với GitHub

Nên commit:

- `docker-compose.yml`
- `.env.example`
- `README.md`

Không nên commit:

- `.env`
- `data/`

Quy trình cho máy khác:

1. Pull hoặc clone repo về.
2. Copy `.env.example` thành `.env`.
3. Điền giá trị thật vào `.env`.
4. Chạy `docker compose up -d`.

## Gợi ý bảo mật

- Dùng `ADMIN_TOKEN` dài, khó đoán
- Không commit `.env` lên GitHub
- Nên dùng HTTPS thông qua Nginx, Traefik hoặc Cloudflare Tunnel
- Backup thư mục `data/` định kỳ
- Nếu dùng Gmail SMTP, hãy dùng app password thay vì mật khẩu tài khoản

