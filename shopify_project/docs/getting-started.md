# Getting Started Guide

This guide provides step-by-step instructions for installing, configuring, and deploying the **Shopify Order Management Application** on a Virtual Private Server (VPS).

---

## ✅ Installation on VPS

**Step 1: Update your server**

# Update and upgrade the system
        sudo apt update && sudo apt upgrade -y

# Install required software
        sudo apt install nginx python3 python3-pip python3-venv sqlite3 certbot python3-certbot-nginx git -y

# Clone the application repository
        git clone https://github.com/yourusername/shopify-order-hub.git /opt/shopify-app

# Set up Python virtual environment and install dependencies
        cd /opt/shopify-app
        python3 -m venv venv
        source venv/bin/activate
        pip install -r requirements.txt
        
---

## ✅ Configuration of Environment Variables

# Create and edit .env file
        nano /opt/shopify-app/.env

# Paste and customize the following credentials:
        SHOPIFY_SHOP_URL=sermatdirect.myshopify.com
        SHOPIFY_ACCESS_TOKEN=your_access_token
        SHOPIFY_API_KEY=your_api_key
        SHOPIFY_API_SECRET=your_api_secret
        SHOPIFY_WEBHOOK_SECRET=your_webhook_secret

        FTP_AGRIEST_HOST=ftp.agriest.com
        FTP_AGRIEST_USER=ftp_user_agriest
        FTP_AGRIEST_PASS=ftp_password_agriest

        FTP_FOURNIAL_HOST=ftp.fournial.com
        FTP_FOURNIAL_USER=ftp_user_fournial
        FTP_FOURNIAL_PASS=ftp_password_fournial

---

## ✅ Deployment with Systemd

# Create a Systemd service file
        sudo nano /etc/systemd/system/shopify-app.service

# Paste the following configuration:

        [Unit]
        Description=Shopify Order Management Application
        After=network.target

        [Service]
        User=shopifyapp
        Group=www-data
        WorkingDirectory=/opt/shopify-app
        EnvironmentFile=/opt/shopify-app/.env
        ExecStart=/opt/shopify-app/venv/bin/gunicorn --workers 3 --bind unix:/opt/shopify-app/shopify-app.sock wsgi:app
        Restart=always
        RestartSec=5

        [Install]
        WantedBy=multi-user.target

# Enable and start the service
        sudo systemctl daemon-reload
        sudo systemctl enable shopify-app
        sudo systemctl start shopify-app
        sudo systemctl status shopify-app

---

## ✅ Configure Nginx as Reverse Proxy

# Create and edit Nginx configuration file
        sudo nano /etc/nginx/sites-available/shopify-app.conf

# Paste this configuration:

        server {
            listen 80;
            server_name sermatdirect.com;

            location / {
                proxy_pass http://unix:/opt/shopify-app/shopify-app.sock;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
            }
        }

# Enable the configuration and restart Nginx
        sudo ln -s /etc/nginx/sites-available/shopify-app.conf /etc/nginx/sites-enabled/
        sudo nginx -t
        sudo systemctl restart nginx

---

## 🔐 Secure your App with SSL (Let’s Encrypt)

# Install and configure SSL certificates
        sudo certbot --nginx -d sermatdirect.com

Certbot will automatically configure SSL and renew certificates.