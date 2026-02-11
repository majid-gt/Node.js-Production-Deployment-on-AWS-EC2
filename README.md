<div align="center">

# 🚀 Node.js Application Deployment on AWS EC2

<img src="https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws&logoColor=white" />
<img src="https://img.shields.io/badge/Node.js-18-green?logo=node.js&logoColor=white" />
<img src="https://img.shields.io/badge/NGINX-ReverseProxy-brightgreen?logo=nginx&logoColor=white" />
<img src="https://img.shields.io/badge/PM2-ProcessManager-blue?logo=pm2&logoColor=white" />
<img src="https://img.shields.io/badge/SSL-LetsEncrypt-red?logo=letsencrypt&logoColor=white" />

Production-ready deployment using **NGINX**, **PM2**, and **Let’s Encrypt SSL**

</div>

---

## 📌 Project Overview

This project demonstrates deploying a **Node.js application** on an AWS EC2 Ubuntu server with:

- PM2 as a process manager  
- NGINX as a reverse proxy  
- Let’s Encrypt for SSL  
- Elastic IP for stable public access  
- Cloudflare DNS for domain routing  

> 🎯 Objective: Deploy a production-ready Node.js application with HTTPS and process persistence.

---

# 🏗 Architecture Diagram

            🌐 User
               │
               ▼
    ☁ Cloudflare DNS (A Record)
               │
               ▼
    🖥 AWS EC2 (Elastic IP)
               │
               ▼
    🌍 NGINX (Reverse Proxy :80 / :443)
               │
               ▼
    ⚙ PM2 (Process Manager)
               │
               ▼
    🟢 Node.js Application (Port 8001)


    
---

# 🛠 Technologies Used

- ☁ **AWS EC2 (Ubuntu)**
- 🌐 **Elastic IP**
- 🟢 **Node.js v18**
- ⚙ **PM2**
- 🌍 **NGINX**
- 🔐 **Let’s Encrypt (Certbot)**
- 🛡 **Cloudflare DNS**
- 🧑‍💻 **Git & GitHub**

---

# ☁ Step 1 — Launch EC2 Instance

### Create Instance

- Ubuntu Server
- t2.medium
- Attach Elastic IP
- Allow ports **22, 80, 443**

### SSH into Server

```bash
ssh ubuntu@your-ec2-public-ip
```
# 🟢 Step 2 — Install Node.js (v18)

## Add NodeSource Repository

```bash
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

## Install Node.js

```bash
sudo apt install nodejs
```

## Verify Installation

```bash
node --version
```

---

# 📥 Step 3 — Clone Application

## Clone Repository

```bash
git clone https://github.com/piyushgargdev-01/short-url-nodejs
```

## Navigate into Project Directory

```bash
cd short-url-nodejs
```

---

# 📦 Step 4 — Install PM2 & Start Application

## Install PM2 Globally

```bash
sudo npm install -g pm2
```

## Start Application

```bash
pm2 start index
```

---

## ⚙ PM2 Commands

### Check Application Status

```bash
pm2 status
```

### Restart Application

```bash
pm2 restart app
```

### Stop Application

```bash
pm2 stop app
```

### View Logs

```bash
pm2 logs
```

### Clear Logs

```bash
pm2 flush
```

### Enable Auto Start on Reboot

```bash
pm2 startup ubuntu
```

---

# 🌐 Step 5 — Install & Configure NGINX

## Install NGINX

```bash
sudo apt install nginx
```

## Open Default Configuration File

```bash
sudo nano /etc/nginx/sites-available/default
```

## Add This Inside the `server` Block

```nginx
server_name kcmkcmkcmkcmkcmkcmkcm.dpdns.org;

location / {
    proxy_pass http://localhost:8001;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

## Test NGINX Configuration

```bash
sudo nginx -t
```

## Reload NGINX

```bash
sudo nginx -s reload
```

---

# 🔐 Step 6 — Enable HTTPS with Let’s Encrypt

## Install Certbot Repository

```bash
sudo add-apt-repository ppa:certbot/certbot
```

## Update Package List

```bash
sudo apt-get update
```

## Install Certbot for NGINX

```bash
sudo apt-get install python3-certbot-nginx
```

## Generate SSL Certificate

```bash
sudo certbot --nginx -d kcmkcmkcmkcmkcmkcmkcm.dpdns.org
```

## Test SSL Renewal

```bash
certbot renew --dry-run
```

---

# 🌍 DNS Configuration

## Cloudflare Setup

- Configure **A Record**
- Point domain to **EC2 Elastic IP**
- Set Proxy Mode to **DNS Only**

---

# 🚧 Challenge Faced

## Issue

Cloudflare proxy masked the EC2 public IP, preventing proper SSL validation.

## Solution

Switched Cloudflare to **DNS Only Mode** to allow certificate validation and direct routing.

---

# 🚀 Production Readiness Features

- Persistent Node.js process via PM2  
- Automatic restart on server reboot  
- HTTPS enabled with Let’s Encrypt  
- Reverse proxy architecture using NGINX  
- Elastic IP for stable endpoint  

---

# 📈 Key Learnings

- Production-grade Node.js deployment  
- Reverse proxy configuration  
- SSL certificate lifecycle management  
- DNS troubleshooting  
- Infrastructure layering (DNS → EC2 → NGINX → PM2 → Application)  

---

# 👤 Author

## Md Majid  
### DevOps & SRE Enthusiast  

AWS | Node.js | Linux | NGINX  

---

# 📜 License

This project uses the original application:

https://github.com/piyushgargdev-01/short-url-nodejs

Refer to the original repository for licensing details.
