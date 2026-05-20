# 🐚 HảiSản.vn — Chợ Hải Sản Online

> **Dự án môn học** · Full-stack Web Application  
> Nền tảng mua bán hải sản tươi & khô trực tiếp từ ngư dân đến người tiêu dùng.

---

## 📋 Mục lục

- [Giới thiệu](#-giới-thiệu)
- [Tính năng chính](#-tính-năng-chính)
- [Công nghệ sử dụng](#-công-nghệ-sử-dụng)
- [Cấu trúc thư mục](#-cấu-trúc-thư-mục)
- [Cài đặt & Chạy thử](#-cài-đặt--chạy-thử)
- [Biến môi trường](#-biến-môi-trường)
- [Cơ sở dữ liệu](#-cơ-sở-dữ-liệu)
- [API Reference](#-api-reference)
- [Phân công nhóm](#-phân-công-nhóm)

---

## 🌊 Giới thiệu

**HảiSản.vn** là ứng dụng web kết nối ngư dân và người mua hải sản theo mô hình C2C (Customer-to-Customer). Người bán đăng bài trực tiếp từ tàu hoặc kho, người mua tìm kiếm theo vị trí GPS hoặc duyệt danh mục hải sản khô toàn quốc.

**Hai loại sản phẩm:**
- 🌊 **Hải sản tươi** — Hiển thị theo GPS trong bán kính 20km, tự động hết hạn sau 24 giờ
- 🔥 **Hải sản khô** — Không giới hạn địa lý, giao hàng toàn quốc

---

## ✨ Tính năng chính

### Dành cho người dùng thông thường
| Tính năng | Mô tả |
|-----------|-------|
| 🔐 Đăng ký / Đăng nhập | Xác thực bằng số điện thoại + mật khẩu, JWT 7 ngày |
| 🏠 Trang chủ | Duyệt sản phẩm theo tab Tươi / Khô, lọc thông minh, tìm kiếm |
| 📍 Tìm theo GPS | Tự động lấy vị trí, hiển thị hải sản tươi trong vòng 20km |
| 🗺️ Bản đồ | Xem vị trí các điểm bán trên bản đồ Leaflet |
| 📦 Chi tiết sản phẩm | Ảnh slider, thông tin người bán, thanh tồn kho, countdown hết hạn |
| 💬 Chat real-time | Nhắn tin trực tiếp với người bán qua Socket.IO |
| ⭐ Đánh giá | Viết review + upload ảnh sau khi giao dịch |
| 👥 Theo dõi | Follow người bán yêu thích, nhận thông báo bài mới |
| 🔔 Thông báo | Real-time: bài mới từ người bán đang follow, review mới |

### Dành cho người bán (Seller)
| Tính năng | Mô tả |
|-----------|-------|
| ➕ Đăng bài | Upload ảnh (Cloudinary), chọn loại, nhập giá/kg, khối lượng, GPS bắt buộc với hải sản tươi |
| 📊 Dashboard | Quản lý bài đăng, cập nhật khối lượng còn lại, xem tin nhắn |
| 🗑️ Xoá bài | Xoá bài đăng bất kỳ lúc nào |

### Dành cho Admin
| Tính năng | Mô tả |
|-----------|-------|
| ⚙️ Trang Admin | Thống kê tổng quan: tổng user, bài tươi, bài khô, tin nhắn |
| 👥 Quản lý người dùng | Xem danh sách, khoá / mở khoá tài khoản |
| 📋 Quản lý bài đăng | Xem tất cả bài, xoá bài vi phạm |

### Tự động (Cronjob)
- ⏰ Mỗi giờ, hệ thống tự chuyển hải sản tươi đã qua 24h sang trạng thái `Expired` và ẩn khỏi trang chủ

---

## 🛠 Công nghệ sử dụng

### Frontend
| Công nghệ | Phiên bản | Dùng để |
|-----------|-----------|---------|
| React | 19 | UI framework |
| Vite | 8 | Build tool, dev server |
| React-Leaflet / Leaflet | 5 / 1.9 | Bản đồ tương tác |
| Socket.IO Client | 4 | Chat & thông báo real-time |
| Axios | 1.x | Gọi API |

### Backend
| Công nghệ | Phiên bản | Dùng để |
|-----------|-----------|---------|
| Node.js + Express | LTS / 4.x | REST API server |
| TypeScript | 5 | Type safety |
| MySQL2 | 3 | ORM-less database driver |
| Socket.IO | 4 | WebSocket server |
| JSON Web Token | 9 | Xác thực stateless |
| Bcryptjs | 2 | Hash mật khẩu |
| Multer + Cloudinary | 1.4 / 2 | Upload & lưu trữ ảnh |
| node-cron | 3 | Tác vụ định kỳ |

### Database
- **MySQL** với schema utf8mb4 (hỗ trợ tiếng Việt đầy đủ)

---

## 📁 Cấu trúc thư mục

```
seafood/
├── backend/                    # Node.js + Express API
│   ├── src/
│   │   ├── app.ts              # Entry point, middleware, routes
│   │   ├── db.ts               # MySQL connection pool
│   │   ├── socket.ts           # Socket.IO: chat + thông báo
│   │   ├── cron.ts             # Cronjob tự expire hải sản tươi
│   │   ├── controllers/
│   │   │   ├── auth.controller.ts
│   │   │   ├── product.controller.ts
│   │   │   ├── image.controller.ts
│   │   │   ├── message.controller.ts
│   │   │   ├── review.controller.ts
│   │   │   ├── follow.controller.ts
│   │   │   ├── notification.controller.ts
│   │   │   └── admin.controller.ts
│   │   ├── routes/             # Định nghĩa endpoints
│   │   ├── middlewares/
│   │   │   ├── auth.ts         # Kiểm tra JWT
│   │   │   └── upload.ts       # Multer config
│   │   └── utils/
│   │       └── haversine.ts    # Tính khoảng cách GPS
│   ├── sql/
│   │   ├── schema.sql          # Tạo database & bảng
│   │   └── seed.sql            # Dữ liệu mẫu
│   ├── .env                    # Biến môi trường (không commit)
│   └── package.json
│
└── client/my-app/              # React + Vite SPA
    ├── public/
    │   └── hero-ocean.png
    ├── src/
    │   ├── App.jsx             # Router, state toàn cục
    │   ├── main.jsx            # React DOM render
    │   ├── index.css           # Global styles, animations
    │   ├── components/
    │   │   ├── ProductCard.jsx # Card sản phẩm + Skeleton
    │   │   ├── ImageSlider.jsx # Slider ảnh sản phẩm
    │   │   ├── MapExplore.jsx  # Bản đồ trang chủ
    │   │   ├── MapMini.jsx     # Bản đồ nhỏ trong chi tiết
    │   │   ├── ChatBox.jsx     # Hộp chat real-time
    │   │   ├── ChatPopover.jsx # Popover danh sách chat
    │   │   └── ReviewList.jsx  # Danh sách đánh giá
    │   ├── layout/
    │   │   └── Navbar.jsx      # Thanh điều hướng
    │   ├── pages/
    │   │   ├── HomePage.jsx
    │   │   ├── ProductDetailPage.jsx
    │   │   ├── AuthPage.jsx
    │   │   ├── PostListingPage.jsx
    │   │   ├── DashboardPage.jsx
    │   │   ├── SellerProfilePage.jsx
    │   │   └── AdminPage.jsx
    │   ├── services/
    │   │   ├── api.js          # Axios wrapper + JWT header
    │   │   └── socket.js       # Socket.IO client singleton
    │   ├── hooks/
    │   │   └── useCountdown.js # Countdown 24h hải sản tươi
    │   └── utils/
    │       ├── theme.js        # Bảng màu toàn cục
    │       └── format.jsx      # Định dạng tiền tệ, pill tags
    └── package.json
```

---

## 🚀 Cài đặt & Chạy thử

### Yêu cầu hệ thống

- Node.js ≥ 18
- MySQL ≥ 8.0
- npm hoặc yarn

### 1. Clone repository

```bash
git clone https://github.com/<your-org>/haisanvn.git
cd haisanvn
```

### 2. Cài đặt Backend

```bash
cd backend
npm install
```

Tạo file `.env` từ mẫu (xem phần [Biến môi trường](#-biến-môi-trường)):

```bash
cp .env.example .env
# Chỉnh sửa .env với thông tin của bạn
```

Khởi tạo database:

```bash
mysql -u root -p < sql/schema.sql
mysql -u root -p seafood_db < sql/seed.sql   # (tuỳ chọn) dữ liệu mẫu
```

Chạy server (development):

```bash
npm run dev
# Server chạy tại http://localhost:5000
```

### 3. Cài đặt Frontend

```bash
cd ../client/my-app
npm install
npm run dev
# App chạy tại http://localhost:3000
```

### 4. Build production

```bash
# Backend
cd backend
npm run build        # Compile TypeScript → dist/
npm start            # Chạy dist/app.js

# Frontend
cd client/my-app
npm run build        # Output → dist/
```

---

## 🔑 Biến môi trường

Tạo file `backend/.env` với nội dung sau:

```env
# ── Database ──────────────────────────────────
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASS=your_password
DB_NAME=seafood_db

# ── Auth ──────────────────────────────────────
JWT_SECRET=your_super_secret_key_here
JWT_EXPIRES_IN=7d

# ── Server ────────────────────────────────────
PORT=5000
CLIENT_URL=http://localhost:3000

# ── Cloudinary (upload ảnh) ───────────────────
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

> ⚠️ **Lưu ý:** Không bao giờ commit file `.env` lên GitHub. File này đã được thêm vào `.gitignore`.

Để lấy thông tin Cloudinary: đăng ký miễn phí tại [cloudinary.com](https://cloudinary.com), vào **Dashboard → API Keys**.

---

## 🗄 Cơ sở dữ liệu

### Sơ đồ quan hệ (ERD)

```
User ────────────────────────── Product
 │   (1 user → nhiều sản phẩm)     │
 │                                  │
 ├──── Follow ────────────────── User (seller)
 │     (follower ↔ seller)
 │
 ├──── Review ──────────────── Product
 │     (reviewer, rating, comment)
 │
 └──── Message ─────────────── Product
       (sender, receiver, content)
             │
          ProductImage
          (Cloudinary URL)

Notification (trigger từ Follow + Review)
```

### Các bảng chính

| Bảng | Mô tả |
|------|-------|
| `User` | Tài khoản người dùng (Role: User / Admin) |
| `Product` | Bài đăng hải sản (Type: Fresh / Dried) |
| `ProductImage` | Ảnh sản phẩm lưu trên Cloudinary |
| `Message` | Tin nhắn gắn với từng sản phẩm |
| `Review` | Đánh giá của người mua cho người bán |
| `Follow` | Quan hệ theo dõi giữa người mua và người bán |
| `Notification` | Thông báo (bài mới / review mới) |

---

## 📡 API Reference

Base URL: `http://localhost:5000/api`

### Auth

| Method | Endpoint | Auth | Mô tả |
|--------|----------|------|-------|
| POST | `/auth/register` | ❌ | Đăng ký tài khoản |
| POST | `/auth/login` | ❌ | Đăng nhập, trả về JWT |
| GET | `/auth/me` | ✅ | Lấy thông tin user hiện tại |

### Products

| Method | Endpoint | Auth | Mô tả |
|--------|----------|------|-------|
| GET | `/products` | ❌ | Danh sách sản phẩm (hỗ trợ lọc GPS, type, search) |
| GET | `/products/:id` | ❌ | Chi tiết sản phẩm |
| GET | `/products/my` | ✅ | Bài đăng của chính mình |
| POST | `/products` | ✅ | Tạo bài đăng mới |
| PUT | `/products/:id` | ✅ | Cập nhật (vd: khối lượng còn lại) |
| DELETE | `/products/:id` | ✅ | Xoá bài đăng |

**Query parameters cho `GET /products`:**

| Param | Ví dụ | Mô tả |
|-------|-------|-------|
| `type` | `Fresh` / `Dried` | Lọc theo loại |
| `search` | `cá thu` | Tìm kiếm tên sản phẩm |
| `lat` + `lng` | `20.98,105.78` | GPS để lọc trong 20km |
| `limit` | `50` | Số lượng kết quả |

### Images

| Method | Endpoint | Auth | Mô tả |
|--------|----------|------|-------|
| POST | `/products/:id/images` | ✅ | Upload ảnh (multipart, tối đa 5 file) |
| DELETE | `/images/:id` | ✅ | Xoá ảnh khỏi Cloudinary |

### Messages (Socket.IO)

Kết nối: `ws://localhost:5000` với `auth: { token }`.

| Event | Hướng | Mô tả |
|-------|-------|-------|
| `join_room` | Client → Server | Tham gia room của product |
| `send_message` | Client → Server | Gửi tin nhắn |
| `new_message` | Server → Client | Nhận tin nhắn mới |
| `notification` | Server → Client | Nhận thông báo real-time |

### Reviews

| Method | Endpoint | Auth | Mô tả |
|--------|----------|------|-------|
| GET | `/reviews/seller/:sellerId` | ❌ | Đánh giá của người bán |
| POST | `/reviews` | ✅ | Gửi đánh giá |

### Follow

| Method | Endpoint | Auth | Mô tả |
|--------|----------|------|-------|
| POST | `/follows/:sellerId` | ✅ | Follow người bán |
| DELETE | `/follows/:sellerId` | ✅ | Unfollow |
| GET | `/follows/my` | ✅ | Danh sách đang follow |

### Notifications

| Method | Endpoint | Auth | Mô tả |
|--------|----------|------|-------|
| GET | `/notifications` | ✅ | Danh sách thông báo |
| PUT | `/notifications/read` | ✅ | Đánh dấu tất cả đã đọc |

### Admin *(yêu cầu Role = Admin)*

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/admin/stats` | Thống kê tổng quan |
| GET | `/admin/users` | Danh sách người dùng |
| PATCH | `/admin/users/:id/toggle` | Khoá / mở khoá tài khoản |
| GET | `/admin/listings` | Tất cả bài đăng |
| DELETE | `/admin/listings/:id` | Xoá bài đăng |

---

## 👥 Phân công nhóm

| Thành viên | Vai trò | Phụ trách |
|-----------|---------|-----------|
| [Tên 1] | Frontend Lead | HomePage, ProductCard, Navbar, giao diện tổng thể |
| [Tên 2] | Backend Lead | REST API, MySQL schema, JWT auth, Cloudinary |
| [Tên 3] | Fullstack | Socket.IO (chat + notification), DashboardPage |
| [Tên 4] | Fullstack | Bản đồ Leaflet, GPS logic, AdminPage |
| [Tên 5] | Fullstack | Review/Follow/Notification, SellerProfilePage |

> Cập nhật tên thành viên và phân công thực tế của nhóm vào bảng trên.

---

## 📸 Giao diện

| Trang chủ | Chi tiết sản phẩm | Đăng bài |
|-----------|-------------------|----------|
| *(thêm screenshot)* | *(thêm screenshot)* | *(thêm screenshot)* |

---

## 📝 Ghi chú kỹ thuật

**Tại sao không dùng ORM?**  
Dự án dùng trực tiếp `mysql2` với raw SQL để dễ hiểu luồng dữ liệu, phù hợp mục tiêu học tập.

**Tại sao GPS chỉ dùng cho hải sản tươi?**  
Hải sản tươi cần mua ngay, chỉ có giá trị trong bán kính gần và 24 giờ. Hải sản khô có thể vận chuyển toàn quốc nên không cần giới hạn địa lý.

**Cronjob hoạt động thế nào?**  
`node-cron` chạy mỗi giờ một lần, tự động `UPDATE Product SET Status='Expired'` cho tất cả hải sản tươi đã qua 24 giờ kể từ `CatchTime`. Khi server khởi động cũng chạy một lần để đồng bộ.

**Haversine formula:**  
Hàm `haversine.ts` tính khoảng cách thực tế giữa hai toạ độ GPS trên bề mặt Trái Đất, dùng để lọc sản phẩm trong 20km.

---

*HảiSản.vn — Dự án môn học Phát triển Ứng dụng Web*
