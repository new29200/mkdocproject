# Shopify Order Management Application

Welcome to the official documentation of the **Shopify Order Management Application**, an automated and secure system built to process Shopify orders, manage inventory, and streamline supplier interactions via FTP.

---

## 🚀 Project Overview

The **Shopify Order Handler** automates the lifecycle of customer orders from the Sermat Équipement Shopify store. It securely processes incoming Shopify webhooks, classifies products by supplier, generates XML files, and transfers orders to suppliers through secure FTP connections.

---

## 🎯 Project Goals

- Automate Shopify order processing and supplier interactions.
- Maintain real-time inventory accuracy.
- Ensure secure and reliable data handling.
- Simplify debugging with comprehensive logs.
- Support continuous, automated deployment using DevOps methodologies.

---

## 📌 Key Features

- **Real-time Webhook Processing**: Instantly handle new Shopify orders.
- **Secure Data Transactions**: Uses Shopify HMAC verification and HTTPS via Let's Encrypt.
- **Supplier Automation**: Automatic XML order creation and FTP delivery.
- **Logging & Monitoring**: Detailed event tracking and error logging.
- **Inventory Sync**: Real-time updates and accurate inventory tracking.
- **High Availability**: System managed by Systemd for automatic recovery.

---

## 🔧 Technical Stack

- **Backend**: Python (Flask)
- **Reverse Proxy**: Nginx
- **Database**: SQLite
- **Server Infrastructure**: VPS (Linux)
- **CI/CD**: Git, GitHub Actions
- **Documentation**: MkDocs, Mermaid, Swagger

---

## 🔒 Security Practices

- **HTTPS**: Secure SSL/TLS communication with Let's Encrypt certificates.
- **Webhook Validation**: HMAC signature verification from Shopify.
- **Credential Protection**: Environment variables for secure key storage.
- **Secure FTP Transfers**: Encrypted file transmissions to suppliers.

---

## 📁 Project Structure

```bash
/opt/shopify-app
├── app.py               # Main Flask API
├── wsgi.py              # WSGI entrypoint
├── data/                # SQLite Database
├── logs/                # Log files
├── shipment/            # Shipment handling
├── stock/               # Inventory management scripts
├── venv/                # Python virtual environment
├── .env                 # Environment variables
├── requirements.txt     # Python dependencies
└── stock-update.py      # Automated inventory updater
