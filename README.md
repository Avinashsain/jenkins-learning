
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

## Architecture Flow

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

# Connecting the Application to MongoDB Atlas

This guide explains how to set up MongoDB Atlas and connect it to your application and MongoDB Compass.

---

## Steps to Set Up MongoDB on Atlas

### 1. Log in to MongoDB Atlas
- Visit: https://cloud.mongodb.com/
- Log in to your MongoDB Atlas account.

---

### 2. Create Organization and Project
- From the dashboard, create:
  - An **Organization**
  - A **Project** inside that organization

---

### 3. Create a Cluster
- Click **Create Cluster**.
- Name the cluster: `herocluster1`
- Select the **M0 Free Plan**.
- Click **Create Deployment** to deploy the cluster.

---

### 4. Create a Database User
- Go to **Database Access**.
- Click **Add New Database User**.
- Set a **username** and **password**.
- Click **Create DB User**.

---

### 5. Configure Network Access
- Go to **Network Access** from the left panel.
- Click **Add IP Address**.
- Enter:
  ```
  0.0.0.0/0
  ```
  to allow access from all IP addresses.
- Confirm the changes.

---

### 6. Connect MongoDB to Compass
- Go to the **Database** section.
- Click **Connect**.
- Select **Compass** as the connection method.
- Choose **I already have MongoDB Compass** if it's installed.

---

### 7. Copy the Connection String
- Copy the connection string provided by MongoDB Atlas.

Example format:
```
mongodb+srv://<username>:<password>@cluster0.mongodb.net/?retryWrites=true&w=majority
```

---

### 8. Connect via MongoDB Compass
- Open **MongoDB Compass**.
- Paste the connection string into the **New Connection** field.
- Click **Connect**.

---

## You're Connected!
Your MongoDB Atlas database is now successfully connected and ready to use with your application and Compass.

---

### Security Tip
For production environments, avoid using `0.0.0.0/0`. Instead, whitelist only trusted IP addresses.

---
