# 🌳 Giải thích cấu trúc thư mục & file

## 📂 project-root/

- **README.md** → Mô tả dự án, hướng dẫn cài đặt & chạy (tài liệu cho dev & giảng viên).
- **.gitignore** → Khai báo file/folder không đưa lên Git (_vd_: `node_modules/`, `__pycache__/`, `.env`).
- **.env.example** → Mẫu file config môi trường (`API_URL=...`, `OLLAMA_HOST=...`).
- **docker-compose.yml** → Chạy nhiều service (frontend + backend + DB) cùng lúc bằng 1 lệnh.

---

## 📂 frontend/ _(ReactJS + Vite + TypeScript + Tailwind)_

- **package.json** → Danh sách dependency frontend (_react, axios, tailwindcss..._).
- **tsconfig.json** → Cấu hình TypeScript.
- **vite.config.ts** → Cấu hình Vite (dev server, build).
- **index.html** → HTML gốc (Vite sẽ inject React vào đây).

### 📂 frontend/src/

- **main.tsx** → Entry point, render `<App />`.
- **App.tsx** → Component chính, chứa router / layout app.

#### 📂 components/ _(tái sử dụng)_
- **ChatBox.tsx** → Khung chat (input + danh sách tin nhắn).
- **MessageBubble.tsx** → Hiển thị tin nhắn (user vs bot).
- **UploadForm.tsx** → Form upload PDF.

#### 📂 pages/ _(trang chính)_
- **Home.tsx** → Giao diện trang chat.
- **Upload.tsx** → Giao diện upload PDF.

#### 📂 services/ _(API client)_
- **api.ts** → Config axios + hàm gọi API (`uploadPDF()`, `chat()`).

#### 📂 styles/
- Tailwind config + CSS custom nếu cần.

---

## 📂 backend/ _(FastAPI)_

- **main.py** → Entry point FastAPI, import router từ `app/api/`.
- **requirements.txt** → Dependency backend (_fastapi, uvicorn, chromadb, sentence-transformers..._).
- **Dockerfile** → Build backend thành image.

### 📂 backend/app/

#### 📂 api/ _(routes)_
- **health.py** → API `/health` (test server).
- **upload.py** → API `/upload` (nhận PDF, xử lý AI).
- **chat.py** → API `/chat` (query DB → gọi Ollama → trả lời).
- **reindex.py** → API `/reindex` (chạy lại embeddings).

#### 📂 core/ _(cấu hình & helper)_
- **config.py** → Load biến môi trường `.env`.
- **logger.py** → Logging (debug, info, error).

#### 📂 models/ _(schema & DB model)_
- **log.py** → Model SQLite log (lưu câu hỏi & câu trả lời).

#### 📂 services/ _(logic nghiệp vụ)_
- **pdf_loader.py** → Đọc PDF bằng pdfplumber, chunk text.
- **embedding.py** → Sinh embeddings bằng SentenceTransformers.
- **retriever.py** → Tìm `top_k` context từ ChromaDB.
- **ollama_client.py** → Gọi Ollama API sinh câu trả lời.
- **chat_service.py** → Logic tích hợp (retriever + prompt + Ollama).

#### 📂 db/ _(database)_
- **database.py** → Kết nối SQLite.
- **migrations/** → Lưu migration schema (nếu dùng Alembic).

## 📂 ai/ — AI & Data Pipeline (thử nghiệm)
- `notebooks/` → Notebook Jupyter để test code AI nhanh (ví dụ: test embeddings)
- `test_queries.json` → Danh sách câu hỏi để kiểm thử bot
- `eval.py` → Script chạy batch test, đo chất lượng trả lời

## 📂 infra/ — DevOps & Deploy
- `Dockerfile.frontend` → Build React app
- `Dockerfile.backend` → Build FastAPI app
- `nginx.conf` → Cấu hình reverse proxy (dùng khi deploy server thật)
- `scripts/start.sh` → Script khởi động backend/frontend
- `scripts/seed_db.py` → Script tạo DB mẫu hoặc seed dữ liệu

## 📂 docs/ — Tài liệu & báo cáo
- `architecture.png` → Sơ đồ kiến trúc hệ thống (frontend ↔ backend ↔ AI ↔ DB)
- `api-docs.md` → Mô tả các API (input/output JSON)
- `report.md` → Báo cáo ngắn, slide, tài liệu trình bày
