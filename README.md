# 🚀 React + Docker Development Setup

This project provides a **Dockerized React (Vite) development environment** with **hot reload support**, ensuring consistent behavior across all machines.

---

## 📦 Tech Stack

* React (Vite)
* Docker
* Docker Compose
* Node.js 20 (Alpine)

---

## 🎯 Features

* 🔥 Hot Reload (works reliably inside Docker)
* 📁 Live Code Sync using volumes
* ⚡ Fast Refresh enabled
* 🐳 Same environment for all developers
* ❌ No need to install Node.js locally

---

## 📂 Project Structure

```
.
├── Dockerfile
├── docker-compose.yml
├── package.json
└── src/
```

---

## 🐳 Dockerfile Explained

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 5173

ENV CHOKIDAR_USEPOLLING=true
ENV WATCHPACK_POLLING=true
ENV FAST_REFRESH=true

CMD ["npm", "run", "dev", "--", "--host"]
```

### Key Points

* Uses **Node 20 Alpine** (lightweight image)
* Installs dependencies inside container
* Enables **hot reload fix** using polling
* Runs Vite dev server on `5173`

---

## ⚙️ Docker Compose Setup

```yaml
services:
  react-app:
    build: .
    ports:
      - "3000:5173"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - CHOKIDAR_USEPOLLING=true
      - WATCHPACK_POLLING=true
      - FAST_REFRESH=true
    stdin_open: true
    tty: true
    restart: unless-stopped
```

---

## 🔥 Important Concepts

### 1. Hot Reload Fix (Very Important)

Docker sometimes fails to detect file changes.

To fix this:

```
CHOKIDAR_USEPOLLING=true
WATCHPACK_POLLING=true
```

👉 This forces **polling mode**, ensuring file changes are always detected.

---

### 2. Volumes

```yaml
- .:/app
- /app/node_modules
```

* `. : /app` → Sync local code with container
* `/app/node_modules` → Prevents overwriting dependencies

👉 Without this, you may get `module not found` errors.

---

### 3. Fast Refresh

```
FAST_REFRESH=true
```

* Updates only changed components
* Preserves state
* Faster development

---

### 4. Interactive Mode

```yaml
stdin_open: true
tty: true
```

Equivalent to:

```
docker run -it
```

👉 Keeps container stable and interactive

---

## 🚀 Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
```

---

### 2. Run the project

```bash
docker-compose up --build
```

---

### 3. Open in browser

```
http://localhost:3000
```

---

## 🛠 Development Workflow

* Edit your code locally
* Changes reflect instantly inside Docker
* Browser auto reloads 🔥

---

## ⚠️ Notes

* This setup is **for development only**
* For production:

```bash
npm run build
```

and use an optimized production server

---

## 📌 Common Issues

### ❌ Hot reload not working

✔ Make sure:

```
CHOKIDAR_USEPOLLING=true
WATCHPACK_POLLING=true
```

---

### ❌ Module not found error

✔ Ensure this volume exists:

```yaml
- /app/node_modules
```

---

## 🤝 Contributing

Feel free to fork this repo and improve it!

---

## ⭐ Support

If you found this helpful, give it a ⭐ on GitHub!


* make it more **premium looking (like top GitHub repos)**
* or optimize it for **your course/project selling**
