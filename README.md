# Travel Memory Application – AWS Production Deployment (MERN Stack)

## Overview
This document explains **end-to-end deployment** of the **Travel Memory** MERN-stack application on AWS with:
- EC2
- Nginx Reverse Proxy
- MongoDB Atlas
- Custom Domains
- HTTPS (Certbot + ACM)
- Single Application Load Balancer
- Separate Target Groups & Auto Scaling Groups for Frontend and Backend
- PM2 Process Management

This README is **production-ready** and suitable for GitHub.

---

## Architecture Diagram

```
User
 └── Cloudflare DNS
      └── AWS Application Load Balancer (HTTPS / ACM)
           ├── Frontend Target Group → Frontend ASG → EC2 → Nginx → React App
           └── Backend Target Group  → Backend ASG  → EC2 → Nginx → Node API
                                               └── MongoDB Atlas
```

📸 **Screenshot suggestion**: AWS Architecture diagram / ALB target groups

---

## Backend Deployment (EC2)

### 1. Launch EC2 (Ubuntu)
- Instance type: t2.micro
- Open ports: 22, 80, 443, 3001

📸 Screenshot: EC2 instance running

---

### 2. Install Node.js & npm

```bash
sudo apt update -y
sudo apt install nodejs npm -y
node -v
npm -v
```

---

### 3. Clone Repository

```bash
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
cd TravelMemory/backend
```

---

### 4. Create Environment File

```bash
nano .env
```

```env
PORT=3001
MONGO_URI=YOUR_MONGODB_ATLAS_URI
```

---

### 5. Install & Run Backend

```bash
npm install
node index.js
```

Backend URL:
```
http://EC2_PUBLIC_IP:3001
```

📸 Screenshot: Backend running in browser / Postman

---

## MongoDB Atlas Setup

1. Login: https://cloud.mongodb.com
2. Create Project & Cluster (M0)
3. Create Database User
4. Network Access: `0.0.0.0/0`
5. Copy Connection String

📸 Screenshot: MongoDB cluster + Compass connection

⚠️ **Security Tip**: Restrict IPs in production.

---

## Frontend Deployment (EC2)

### 1. Clone Frontend

```bash
cd ~/TravelMemory/frontend
```

---

### 2. Create Environment File

```bash
nano .env
```

```env
REACT_APP_BACKEND_URL=https://api.learningtech.store
```

---

### 3. Install & Run

```bash
npm install
npm start
```

Frontend URL:
```
http://EC2_PUBLIC_IP:3000
```

📸 Screenshot: React app UI

---

## Nginx Reverse Proxy

### Frontend Config

```nginx
server {
    listen 80;
    server_name learningtech.store www.learningtech.store;

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

### Backend Config

```nginx
server {
    listen 80;
    server_name api.learningtech.store;

    location / {
        proxy_pass http://localhost:3001;
    }
}
```

```bash
sudo nginx -t
sudo systemctl reload nginx
```

📸 Screenshot: Nginx config test success

---

## SSL with Certbot (EC2)

### Frontend

```bash
sudo certbot --nginx -d learningtech.store -d www.learningtech.store
```

### Backend

```bash
sudo certbot --nginx -d api.learningtech.store
```

📸 Screenshot: HTTPS lock icon

---

## Scaling with Launch Templates & ASG

### Launch Template
- Create from configured EC2
- Separate templates for frontend & backend

📸 Screenshot: Launch template page

---

### Auto Scaling Groups

Frontend ASG:
- Min: 1
- Max: 2

Backend ASG:
- Min: 1
- Max: 2

📸 Screenshot: ASG instances

---

## Single Application Load Balancer

### Target Groups
| Component | Target Group |
|---------|-------------|
| Frontend | travel-memory-frontend-tg |
| Backend  | travel-memory-backend-tg |

### Listener Rules
- `/` → Frontend TG
- `/api/*` → Backend TG

📸 Screenshot: ALB listener rules

---

## AWS Certificate Manager (ACM)

- Domain: `lb.learningtech.store`
- HTTPS termination at ALB
- Auto-renewal enabled

📸 Screenshot: ACM certificate issued

---

## PM2 Process Manager

### Install

```bash
sudo npm install -g pm2
```

### Backend

```bash
pm2 start index.js --name travel-backend
pm2 save
pm2 startup
```

### Frontend (Production Build)

```bash
npm run build
pm2 start serve --name travel-frontend -- -s build -l 3000
pm2 save
```

---

## Final URLs

- 🌐 Frontend: https://learningtech.store
- 🌐 Backend: https://api.learningtech.store
- 🌐 Load Balancer: https://lb.learningtech.store

---

## Deployment Status

✅ HTTPS Enabled  
✅ Load Balanced  
✅ Auto Scaling Enabled  
✅ Production Ready  

---

## Author

**Avinash Sain**  
GitHub: https://github.com/Avinashsain
