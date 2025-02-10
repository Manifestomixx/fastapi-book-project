# 📚 FastAPI Books API

This is a **FastAPI-based Books API** that allows users to create, update, delete, and retrieve book records. The project includes:
- 🚀 **FastAPI** for building the backend API
- 🛠️ **Render** for deployment
- 🔀 **GitHub Actions** for CI/CD
- ⚡ **Nginx (Optional)** for reverse proxying

---

## 🏗️ Project Structure

```
📂 api/
 ┣ 📂 db/
 ┃ ┣ 📜 schemas.py   # Database schemas
 ┣ 📂 routes/
 ┃ ┣ 📜 books.py     # Books API routes
 ┣ 📜 router.py      # Main API router
📂 .github/workflows/
 ┣ 📜 ci.yml         # CI/CD pipeline
📜 main.py           # FastAPI entry point
📜 requirements.txt  # Dependencies
📜 README.md         # Documentation
```

---

## 🔧 Setup and Installation

### 1️⃣ Clone the Repository
```sh
git clone <your-repo-url>
cd fastapi-books-api
```

### 2️⃣ Create a Virtual Environment
```sh
python -m venv venv
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows
```

### 3️⃣ Install Dependencies
```sh
pip install -r requirements.txt
```

### 4️⃣ Run FastAPI Locally
```sh
uvicorn main:app --reload
```
- The API will be available at: `http://127.0.0.1:8000`
- Interactive Docs: `http://127.0.0.1:8000/docs`

---

## 🚀 Deployment on Render

1️⃣ Push your project to **GitHub**.
```sh
git add .
git commit -m "Initial commit"
git push origin main
```
2️⃣ Go to [Render](https://render.com/) and create a **new Web Service**.
3️⃣ Connect your GitHub repo and select the branch to deploy.
4️⃣ Set the **start command** to:
```sh
uvicorn main:app --host 0.0.0.0 --port 10000
```
5️⃣ Deploy the service. Your API will be live at: `https://your-app-name.onrender.com`

---

## 🛠️ Continuous Integration (CI/CD) with GitHub Actions

We have a **CI/CD pipeline** using GitHub Actions to deploy changes automatically.

### ✅ **CI Workflow (`.github/workflows/ci.yml`)**

```yaml
name: CI Pipeline
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Setup Python
        uses: actions/setup-python@v3
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
      - name: Run Tests
        run: |
          pytest
```

This workflow runs **whenever you push code**, ensuring your project is tested before deployment.

---

## ⚡ Nginx Reverse Proxy (For VPS Deployment)

If deploying on a **VPS (instead of Render)**, configure Nginx to serve FastAPI.

### 1️⃣ Install Nginx
```sh
sudo apt update
sudo apt install nginx -y
```

### 2️⃣ Configure Nginx Reverse Proxy
```sh
sudo nano /etc/nginx/sites-available/fastapi
```
Paste this:
```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
Enable the config and restart Nginx:
```sh
sudo ln -s /etc/nginx/sites-available/fastapi /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

### 3️⃣ Secure with SSL (Optional)
```sh
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

Your API will now be accessible via **`https://yourdomain.com`**.

---

## 🔗 API Endpoints

| Method  | Endpoint         | Description |
|---------|-----------------|-------------|
| `POST`  | `/books/`        | Create a new book |
| `GET`   | `/books/`        | Get all books |
| `PUT`   | `/books/{id}`    | Update book by ID |
| `DELETE`| `/books/{id}`    | Delete book by ID |

---

## 👥 Contributing

1. Fork the repository
2. Create a new branch (`git checkout -b feature-branch`)
3. Commit your changes (`git commit -m "Added new feature"`)
4. Push to your branch (`git push origin feature-branch`)
5. Create a Pull Request

---

## 📜 License
This project is licensed under the MIT License.

