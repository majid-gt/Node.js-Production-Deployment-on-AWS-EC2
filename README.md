🚀 Node.js Application Deployment on AWS EC2
📌 Project Overview

This project demonstrates the deployment of a Node.js application on an AWS EC2 Ubuntu server using:

PM2 as a process manager

NGINX as a reverse proxy

Let’s Encrypt for SSL

Elastic IP for static public access

Cloudflare (DNS Provider) for domain management

The objective was to deploy a production-ready Node.js application with HTTPS and process management.

🏗 Architecture Overview
User → Cloudflare DNS → EC2 (Elastic IP)
                         ↓
                      NGINX (Reverse Proxy)
                         ↓
                      PM2 (Process Manager)
                         ↓
                     Node.js App

🛠 Technologies Used

AWS EC2 (Ubuntu)

Elastic IP

Node.js (v18)

PM2

NGINX

Let’s Encrypt (Certbot)

Cloudflare DNS

Git & GitHub

☁️ Step 1: Create AWS EC2 Instance

Created a t2.medium Ubuntu instance

Configured key pair

Attached Elastic IP

Connected via SSH:

ssh ubuntu@your-ec2-public-ip

🟢 Step 2: Install Node.js & NPM
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs
node --version

📥 Step 3: Clone Application

Repository used:

👉 https://github.com/piyushgargdev-01/short-url-nodejs

git clone https://github.com/piyushgargdev-01/short-url-nodejs
cd short-url-nodejs

📦 Step 4: Install Dependencies & Start App

Installed PM2 globally:

sudo npm install -g pm2


Start application:

pm2 start index

Useful PM2 Commands
pm2 show app
pm2 status
pm2 restart app
pm2 stop app
pm2 logs
pm2 flush


Enable auto-start on reboot:

pm2 startup ubuntu

🌐 Step 5: Configure NGINX Reverse Proxy

Install NGINX:

sudo apt install nginx


Edit config:

sudo nano /etc/nginx/sites-available/default


Update server block:

server_name kcmkcmkcmkcmkcmkcmkcm.dpdns.org;

location / {
    proxy_pass http://localhost:8001;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}


Test & restart:

sudo nginx -t
sudo nginx -s reload

🔐 Step 6: Enable HTTPS with Let’s Encrypt

Install Certbot:

sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python3-certbot-nginx


Generate SSL:

sudo certbot --nginx -d kcmkcmkcmkcmkcmkcmkcm.dpdns.org


Test renewal:

certbot renew --dry-run

🌍 DNS Configuration

Domain managed via Cloudflare

Pointed A record to EC2 Elastic IP

🚧 Challenge Faced

Cloudflare proxy was enabled, which masked the EC2 public IP.

Solution:
Disabled proxy (set to DNS only mode) to allow SSL validation and proper routing.


📈 Key Learnings

Setting up a production-ready Node.js deployment

Reverse proxy configuration with NGINX

Managing Node processes using PM2

Handling SSL certificates using Certbot

Debugging DNS issues with Cloudflare

Understanding interaction between Elastic IP, DNS & reverse proxy

🚀 Production Readiness Features

Persistent Node process via PM2

Auto restart on server reboot

HTTPS enabled

Reverse proxy architecture

Elastic IP for stable endpoint


👤 Author

Md Majid
DevOps & SRE Enthusiast
AWS | Node.js | Linux | NGINX

📜 License

This project uses the original application from:
https://github.com/piyushgargdev-01/short-url-nodejs

Refer to the original repository for licensing details.
