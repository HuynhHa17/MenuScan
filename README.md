# 🍽️ MenuScan — Giải Pháp Đọc Menu Thông Minh

> **Biến menu khó đọc thành trải nghiệm ẩm thực trực quan, thân thiện và đầy đủ thông tin.**

---

## 📌 Tổng Quan Dự Án

**MenuScan** là một hệ thống nhận diện và trình bày menu nhà hàng sử dụng công nghệ OCR kết hợp AI. Người dùng chỉ cần chụp ảnh menu, hệ thống sẽ tự động nhận diện, trích xuất và hiển thị thông tin món ăn dưới dạng card trực quan, dễ đọc.

### 🎯 Vấn Đề Giải Quyết

Menu nhà hàng thường:
- Chữ nhỏ, khó đọc, ánh sáng yếu
- Ngôn ngữ nước ngoài hoặc chữ viết tay
- Thiếu hình ảnh minh họa món ăn
- Không thân thiện với người dùng mới

### 💡 Giải Pháp

```
📷 Scan Menu
    │
    ▼
🤖 OCR Model (Image → Raw Text)
    │
    ▼
🧠 LLM Processing (Raw Text → Structured JSON)
    │
    ▼
🃏 Food Card UI (Hiển thị trực quan)
```

---

## 🏗️ Kiến Trúc Hệ Thống

```
┌─────────────────────────────────────────────────────────┐
│                     CLIENT (React)                      │
│  ┌──────────┐  ┌──────────┐  ┌─────────┐  ┌─────────┐  │
│  │ Upload   │  │ Display  │  │  Food   │  │ Detail  │  │
│  │ Image    │  │ Results  │  │  Card   │  │  View   │  │
│  └──────────┘  └──────────┘  └─────────┘  └─────────┘  │
└───────────────────────┬─────────────────────────────────┘
                        │ REST API / WebSocket
┌───────────────────────▼─────────────────────────────────┐
│                  BACKEND (FastAPI)                       │
│  ┌──────────┐  ┌──────────┐  ┌─────────────────────┐   │
│  │   Auth   │  │  Image   │  │   Menu & Card       │   │
│  │ Module   │  │  Upload  │  │   Management API    │   │
│  └──────────┘  └──────────┘  └─────────────────────┘   │
└──────┬──────────────┬──────────────────┬────────────────┘
       │              │                  │
┌──────▼──────┐ ┌─────▼──────┐ ┌────────▼────────┐
│  OCR Module │ │ Raw OCR    │ │   PostgreSQL    │
│             │ │ → JSON     │ │   Database      │
│ Image→Text  │ │ Structured │ │                 │
│             │ │ Output     │ │ ┌─────────────┐ │
└─────────────┘ └────────────┘ │ │  DB Vector  │ │
                               │ │  Storage    │ │
                               │ ├─────────────┤ │
                               │ │  S3 Object  │ │
                               │ │  Storage    │ │
                               │ └─────────────┘ │
                               └─────────────────┘
```

---

## 📦 Cấu Trúc Module

### 1. 🖥️ Frontend (`/frontend`) — React

| Chức năng | Mô tả |
|-----------|-------|
| **Upload Ảnh** | Giao diện tải ảnh menu lên hệ thống |
| **Hiển Thị Kết Quả** | Render danh sách món ăn sau xử lý |
| **Food Card** | Component hiển thị từng món dạng thẻ đẹp |
| **Xem Chi Tiết** | Modal/trang chi tiết từng món ăn |

### 2. ⚙️ Backend (`/backend`) — Python + FastAPI

| Chức năng | Mô tả |
|-----------|-------|
| **Auth** | Xác thực và phân quyền người dùng |
| **Upload Ảnh** | Nhận và lưu trữ ảnh menu từ client |
| **Quản Lý Menu & Card** | CRUD menu, món ăn, card dữ liệu |
| **REST API** | Cung cấp endpoint cho frontend |

### 3. 🔍 OCR Module (`/ocr`)

| Chức năng | Mô tả |
|-----------|-------|
| **Image → Text** | Nhận diện văn bản từ ảnh menu thô |

### 4. 🧠 Raw OCR Processor (`/ocr/processor`)

| Chức năng | Mô tả |
|-----------|-------|
| **Text → JSON** | Phân tích OCR text, trích xuất cấu trúc món ăn |
| **Output Chuẩn Hóa** | Trả về JSON với tên món, giá, mô tả, v.v. |

---

## 🛠️ Công Nghệ Sử Dụng

### Backend
| Công nghệ | Mục đích |
|-----------|----------|
| **Python 3.11+** | Ngôn ngữ chính |
| **FastAPI** | REST API framework |
| **SQLAlchemy** | ORM cho PostgreSQL |
| **Pydantic** | Validation & serialization |

### Frontend
| Công nghệ | Mục đích |
|-----------|----------|
| **React 18+** | UI framework |
| **Vite** | Build tool & dev server |
| **Axios** | HTTP client |

### AI / OCR
| Công nghệ | Mục đích |
|-----------|----------|
| **OCR Engine** | Nhận diện ký tự từ ảnh |
| **LLM (Simple Model)** | Trích xuất & cấu trúc hóa dữ liệu |

### Database & Storage
| Công nghệ | Mục đích |
|-----------|----------|
| **PostgreSQL** | Cơ sở dữ liệu quan hệ chính |
| **Vector DB** | Lưu trữ embedding (RAG support) |
| **AWS S3 / MinIO** | Object storage — lưu ảnh menu gốc |

---

## 🗂️ Cấu Trúc Thư Mục

```
MenuScan/
├── 📁 backend/
│   ├── app/
│   │   ├── api/           # Route handlers
│   │   ├── core/          # Config, security, auth
│   │   ├── models/        # SQLAlchemy models
│   │   ├── schemas/       # Pydantic schemas
│   │   ├── services/      # Business logic
│   │   └── main.py        # FastAPI entrypoint
│   ├── requirements.txt
│   └── Dockerfile
│
├── 📁 frontend/
│   ├── src/
│   │   ├── components/    # Food Card, Upload, Display
│   │   ├── pages/         # Main pages
│   │   ├── services/      # API calls
│   │   └── App.jsx
│   ├── package.json
│   └── Dockerfile
│
├── 📁 ocr/
│   ├── engine/            # OCR model integration
│   ├── processor/         # Raw text → JSON parser
│   └── requirements.txt
│
├── 📁 database/
│   ├── migrations/        # Alembic migrations
│   └── init.sql
│
├── docker-compose.yml
└── README.md
```

---

## 🚀 Hướng Dẫn Cài Đặt

### Yêu Cầu Hệ Thống

- Python `>= 3.11`
- Node.js `>= 18`
- Docker & Docker Compose
- PostgreSQL `>= 15`

### 1. Clone Repository

```bash
git clone https://github.com/HuynhHa17/MenuScan.git
cd MenuScan
```

### 2. Cấu Hình Môi Trường

```bash
cp .env.example .env
# Chỉnh sửa các biến môi trường trong .env
```

**Các biến cần thiết:**
```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/menuscan

# Storage
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret
S3_BUCKET_NAME=menuscan-media

# Auth
SECRET_KEY=your_secret_key
ALGORITHM=HS256

# OCR / AI
OCR_MODEL_API_KEY=your_api_key
```

### 3. Chạy với Docker Compose

```bash
docker-compose up --build
```

### 4. Chạy Thủ Công (Development)

**Backend:**
```bash
cd backend
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

**Frontend:**
```bash
cd frontend
npm install
npm run dev
```

**OCR Service:**
```bash
cd ocr
pip install -r requirements.txt
python engine/main.py
```

---

## 📡 API Endpoints

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| `POST` | `/api/v1/auth/register` | Đăng ký tài khoản |
| `POST` | `/api/v1/auth/login` | Đăng nhập |
| `POST` | `/api/v1/menu/upload` | Upload ảnh menu |
| `GET`  | `/api/v1/menu/{id}` | Lấy thông tin menu |
| `GET`  | `/api/v1/menu/{id}/cards` | Lấy danh sách food cards |
| `GET`  | `/api/v1/food/{id}` | Chi tiết một món ăn |
| `POST` | `/api/v1/ocr/process` | Xử lý OCR từ ảnh |

---

## 🗺️ Luồng Xử Lý Dữ Liệu

```
1. User upload ảnh menu
        ↓
2. Backend nhận ảnh → lưu S3
        ↓
3. OCR Module: Image → Raw Text
        ↓
4. Processor: Raw Text → JSON
   {
     "items": [
       {
         "name": "Phở Bò",
         "price": 55000,
         "description": "Phở truyền thống...",
         "category": "Món chính"
       }
     ]
   }
        ↓
5. Lưu vào PostgreSQL
        ↓
6. Frontend fetch → Render Food Cards
```

---

## 🤝 Đóng Góp

1. Fork repository
2. Tạo branch mới: `git checkout -b feature/ten-tinh-nang`
3. Commit thay đổi: `git commit -m "feat: thêm tính năng X"`
4. Push branch: `git push origin feature/ten-tinh-nang`
5. Tạo Pull Request

---

## 📄 License

Dự án được phân phối dưới giấy phép **MIT License**. Xem file [LICENSE](./LICENSE) để biết thêm chi tiết.

---

<div align="center">
  <p>Made with ❤️ by <strong>HuynhHa17</strong></p>
  <p>⭐ Star repo nếu dự án hữu ích với bạn!</p>
</div>
