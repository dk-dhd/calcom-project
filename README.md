# calcom-project
## 📝 7. Notes About Project Files

  ## this is my video Link:   
  
👉 Please Read Before Review:

🧾 This README provides only a simple overview. A full detailed documentation is available inside the ZIP file.

🖼 Due to time and technical constraints, I added only final steps screenshots, not for every individual step.

🎞 The ZIP file includes an explanation video, complete documentation, and code.

🛡 For security reasons, I have removed important credentials (e.g. secrets and passwords) from .env and YAML files shared publicly.

📁 You can view all code files (.env, docker-compose.yaml, etc.) inside the submitted ZIP.
















# 🚀 Cal.com Deployment on Azure

## 📁 Table of Contents
| S.No | Section                                | Page No. |
|------|----------------------------------------|----------|
| 1    | Introduction                           | 1        |
| 2    | Step-by-Step Deployment Guide          |          |
| 2.1  | Step 1: Create a Linux VM on Azure     | 1        |
| 2.2  | Step 2: Connect to VM and Clone Repo   | 2        |
| 2.3  | Step 3: Setup Environment Variables    | 3        |
| 2.4  | Step 4: Docker Compose Setup           | 4        |
| 2.5  | Step 5: Start the Cal.com App          | 5        |
| 3    | Networking & Domain Setup              |          |
| 3.1  | Step 6: Configure DNS (Azure DNS Zone) | 6        |
| 3.2  | Step 7: Configure HTTPS with Caddy     | 7        |
| 4    | Cal.com Application Features           | 8        |
| 5    | Scaling & Monitoring                   |          |
| 5.1  | Step 8: Set up Azure VM Scale Set      | 9        |
| 5.2  | Step 9: Creating Alerts for Failures   | 10       |
| 5.3  | Step 10: Monitoring (Logs, Cost Alerts)| 10       |
| 6    | Project Submission                     |          |
| 6.1  | Step 11: Push Project to GitHub        | 11       |
| 6.2  | Step 12: Create ZIP File for Submission| 12       |
| 6.3  | Step 13: Record and Submit Demo Video  | 13       |
| 7    | References & Useful Links              | 13       |

---

# 🌐 Cal.com on Azure - Deployment Guide

## 👩‍💻 Project by Karan Dubey

### 📄 Project Overview
> *Goal:* Deploy the open-source *Cal.com* scheduling app on *Azure* using *Docker Compose*, with domain, SSL, autoscaling, and monitoring.

### 🧰 Technologies Used
- ☁ Microsoft Azure
- 🐧 Ubuntu 22.04 LTS VM
- 🐳 Docker & Docker Compose
- 🐘 PostgreSQL
- 📅 Cal.com (open-source scheduling tool)
- 🔒 Caddy Server (for HTTPS & reverse proxy)
- 🌐 Azure DNS Zone
- 📈 Azure VM Scale Sets & Monitor

---

## ✅ Step-by-Step Deployment Instructions

### 📌 2.1 Step 1: Create a Linux VM on Azure
1. Go to [Azure Portal](https://portal.azure.com)
2. Navigate to: *Virtual Machines > Create*
3. Configure:
   - *Resource Group*: calcom-project 2
   - *VM Name*: Karan VM
   - *Image*: Ubuntu 22.04 LTS
   - *Size*: B1s or B1ms (lightweight)
   - *Auth*: SSH or password
   - *Ports*: Allow 22, 80, 443
4. Click *Review + Create* ➜ Wait for deployment

### 📌 2.2 Step 2: Connect & Clone Cal.com Docker Repo
bash
ssh azureuser@<your-vm-ip>
sudo apt update && sudo apt install git -y
mkdir calcom-docker && cd calcom-docker
git clone https://github.com/calcom/docker.git


### 📌 2.3 Step 3: Configure Environment Variables
bash
cp .env.example .env
vi .env  # Update DB passwords, secrets

Generate a secure secret:
bash
openssl rand -hex 32


### 📌 2.4 Step 4: Install Docker & Compose
bash
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker && sudo systemctl start docker
docker --version && docker-compose --version


### 📌 2.5 Step 5: Start Cal.com Application
bash
cd docker
docker-compose up -d
docker ps  # Verify containers

*Expected Services:*
- calcom-web
- calcom-db
- calcom-caddy

---

## 🌍 3. Networking & Domain Setup

### 🌐 3.1 Configure Azure DNS Zone
1. Navigate: *Azure > DNS Zones* ➜ Create New
2. Domain Name: calcom.local (or your domain)
3. Add *A Record*:
   - *Name*: @
   - *Type*: A
   - *IP Address*: Your VM's public IP

### 🔐 3.2 Setup HTTPS using Caddy
Create Caddyfile:
bash
nano Caddyfile

Paste:
caddy
KaranCalcom.EastUS.cloudapp.azure.com {
  reverse_proxy web:3000
}

Ensure *ports 80 & 443* are open in VM Networking.

---

## ✨ 4. Cal.com Application Features
- 📅 Interactive Booking UI
- ⚙ Admin Dashboard
- 🔁 Rescheduling & Email Confirmations
- 🌐 Public Event Pages
- 🔒 Secure & Open Source Scheduling

---

## 📈 5. Scaling & Monitoring

### 📊 5.1 Create Azure VM Scale Set (VMSS)
- Go to *Azure > Virtual Machine Scale Sets > Create*
- Use same region as VM
- Configure autoscaling:
  - *CPU > 70%* ➜ Add instance
  - *Min: 1, **Max*: 5

### 🚨 5.2 Setup Alerts via Azure Monitor
- Navigate to *Azure Monitor > Alerts*
- Example Alert:
  - *Condition*: CPU > 80% for 5 mins
  - *Action*: Send email / scale out

### 🧾 5.3 Monitor Logs & Budget
- Enable diagnostic settings on VM
- Set cost alerts to avoid budget overuse

---

## 📤 6. Project Submission

### 🧑‍💻 6.1 Push to GitHub
bash
git init
git remote add origin 
git add .
git commit -m "Initial commit with Cal.com setup"
git push -u origin main


### 📦 6.2 Create ZIP for Submission
bash
cd ~
zip -r calcom-azure-project.zip calcom-azure-project
scp azureuser@<vm-ip>:/home/azureuser/calcom-azure-project.zip .


### 🎥 6.3 Record and Upload Demo Video
- Must show:
  - 🔧 VM & Docker setup
  - 🌐 HTTPS & DNS setup
  - 📂 GitHub repo & VMSS
- Tools: OBS Studio / Zoom / Xbox Bar

📽 *Demo Video Link:* [Insert your video URL here]  

---

## 🔗 7. References & Useful Links
- [Azure Portal](https://portal.azure.com)
- [Cal.com GitHub](https://github.com/calcom)
- [Docker Docs](https://docs.docker.com)
- [Caddy Docs](https://caddyserver.com)
