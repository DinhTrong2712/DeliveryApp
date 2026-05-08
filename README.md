# Hệ thống Quản lý Giao hàng & Thu tiền

## Cài đặt nhanh

### Yêu cầu
- .NET 8 SDK
- Node.js 18+
- PostgreSQL 14+

### 1. Cấu hình database

Tạo database PostgreSQL và cập nhật connection string trong `DeliveryApp.API/appsettings.json`:
```json
"ConnectionStrings": {
  "DefaultConnection": "Host=localhost;Database=delivery_db;Username=postgres;Password=YOUR_PASSWORD"
}
```

### 2. Chạy backend

```bash
cd DeliveryApp.API
dotnet run
```

API sẽ chạy tại `http://localhost:5000`
Database migration + seed sẽ tự động chạy khi khởi động.

**Tài khoản admin mặc định:** `admin / Admin@123`

### 3. Chạy frontend

```bash
cd shipper-frontend
npm install
npm run dev
```

Frontend chạy tại `http://localhost:5173`

## Cấu hình production

Cập nhật `appsettings.json`:
- `Jwt:Secret`: chuỗi ngẫu nhiên >= 64 ký tự
- `R2:*`: thông tin Cloudflare R2 (lưu ảnh)
- `Tingee:Secret`: HMAC secret từ Tingee

## Kiến trúc

```
ASP.NET Core 8 API  ←→  React + Vite (TypeScript)
      ↕                        ↕
  PostgreSQL              SignalR (real-time)
      ↕
  Cloudflare R2 (ảnh)
      ↑
  Tingee Webhook
```

## Phân quyền

| Role | Trang |
|------|-------|
| Shipper | /shipper/orders |
| Accountant | /accountant/dashboard |
| Admin | /admin/users |
