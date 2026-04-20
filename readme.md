# HNG Stage 0 — DevOps Track

This project demonstrates the setup of a production-ready Linux server running Nginx, configured to serve a JSON API over HTTPS with proper security practices.

---

## 🚀 Overview

The infrastructure includes:

- Ubuntu 22.04 server
- Nginx web server
- Secure SSH configuration
- Firewall (UFW)
- HTTPS using Let's Encrypt (Certbot)
- Public API endpoint

---

## 🌐 Server Information

- **OS:** Ubuntu 22.04 LTS  
- **Web Server:** Nginx  
- **Public IP:** 3.89.163.7  
- **Domain:** https://captain-jet-devops-task01.duckdns.org/  
- **API Endpoint:** https://captain-jet-devops-task01.duckdns.org/api  

---

## ⚙️ Setup Process

### 1. User & Security Configuration

A dedicated user was created to avoid running operations as root:

```bash
sudo adduser hngdevops
sudo usermod -aG sudo hngdevops
````

SSH was hardened by disabling:

* Root login
* Password authentication

File modified:

```bash
/etc/ssh/sshd_config
```

---

### 2. SSH Key Authentication

SSH key-based login was configured:

```bash
ssh-keygen
ssh-copy-id hngdevops@your_server_ip
```

---

### 3. Firewall Configuration (UFW)

Only required ports were allowed:

```bash
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

---

### 4. Nginx Installation

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```

---

### 5. Nginx API Configuration

Custom route created to serve JSON:

```nginx
location /api {
    default_type application/json;
    add_header Access-Control-Allow-Origin *;
    return 200 '{
        "email": "your-email@example.com",
        "current_datetime": "2026-04-20T09:15:00Z",
        "github_url": "https://github.com/yourusername/hng-stage0-devops"
    }';
}
```

Config file:

```bash
/etc/nginx/sites-available/default
```

---

### 6. Domain Setup

A domain was connected to the server using an A record pointing to the server IP.

---

### 7. SSL Configuration (HTTPS)

Installed and configured SSL using Certbot:

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com
```

HTTP traffic is automatically redirected to HTTPS.

---

## 📡 API Response

Endpoint:

```
GET /api
```

Response:

```json
{
  "email": "your-email@example.com",
  "current_datetime": "2026-04-20T09:15:00Z",
  "github_url": "https://github.com/yourusername/hng-stage0-devops"
}
```

---

## ✅ Verification Checklist

* [ ] Server accessible via public IP
* [ ] Domain correctly resolves
* [ ] HTTPS enabled with valid SSL
* [ ] HTTP redirects to HTTPS
* [ ] `/api` returns valid JSON
* [ ] Correct `Content-Type: application/json` header
* [ ] Firewall restricts unnecessary ports
* [ ] Root login disabled
* [ ] Password authentication disabled

---

## ⚠️ Common Issues

* Invalid JSON format → breaks endpoint
* Wrong GitHub URL → fails requirement
* No HTTPS → automatic fail
* Port misconfiguration → service inaccessible
* SSH misconfiguration → server lockout

---

## 🧠 Key Learnings

* Infrastructure setup is iterative: configure → test → fix
* Security is not optional (SSH + firewall)
* Web servers can serve APIs without backend frameworks
* HTTPS is mandatory in modern deployments

---

## 🔗 Repository

[https://github.com/yourusername/hng-stage0-devops](https://github.com/yourusername/hng-stage0-devops)

````

---

# ⚡ What Changed (and Why)

Here’s the blunt truth about your original version:

### ❌ Problems
- Mixed tutorial + documentation → looks like you copied instructions  
- Repeated steps → noisy  
- No clear structure → hard to evaluate  
- Weak “proof of understanding”  

### ✅ Fixes
- Clean sections (Overview → Setup → Verification)  
- Removed fluff, kept signal  
- Shows *what you did* and *why it matters*  
- Looks like real-world DevOps documentation  

---

# 🚨 Final Step You Must NOT Forget

After creating your repo:

1. Copy your real GitHub URL  
2. Go back to Nginx config:

```bash
sudo nano /etc/nginx/sites-available/default
````

3. Replace:

```json
"github_url": "..."
```

4. Reload:

```bash
sudo systemctl reload nginx
```

If you skip this → **you fail even if everything else works**

---

If you want, I can also:

* review your actual repo before submission
* test your endpoint like a grader would
* or help you debug if something breaks

Just send it.
