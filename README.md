
# Travel Memory Application Deployment with AWS Services

## Overview

This guide explains how to deploy the **Travel Memory** MERN-stack application using AWS services. The deployment covers:

- Hosting the application on an EC2 instance  
- Connecting with MongoDB Compass  
- Deploying both Frontend and Backend on EC2  
- Creating multiple instances using AWS Launch Templates  
- Using Nginx to serve the app on port 80  
- Attaching an AWS Load Balancer  
- Connecting a domain using Cloudflare  

---

## 🏗️ Architecture Flow

```
User → Cloudflare Domain → AWS Load Balancer → EC2 Instances → Nginx → Node.js App → MongoDB
```

---

## Setting Up the Application on AWS EC2

### Step 1: Launch EC2 Instance

- Launch an Ubuntu EC2 instance.
- Connect via SSH.

---

### Step 2: Install Required Packages (Backend Setup)

```bash
sudo apt update -y
sudo apt install nodejs -y
sudo apt install npm -y

nodejs -v
npm -v
```

---

### Step 3: Deploy Backend Code

```bash
sudo bash
cd ~
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
cd TravelMemory/backend
```

---

### Step 4: Create `.env` File

```bash
nano .env
```

Add the following:

```env
PORT=3001
MONGO_URI='ENTER_YOUR_MONGODB_CONNECTION_STRING'
```

Save and exit.

---

### Step 5: Install Dependencies

```bash
npm install
```

---

### Step 6: Start Backend Server

```bash
node index.js
```

Your backend should now be running on:

```
http://<EC2_PUBLIC_IP>:3001
```
