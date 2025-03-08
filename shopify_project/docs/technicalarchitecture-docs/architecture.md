# 📂 Technical Architecture

This section describes the architectural components of the Shopify Order Management Application, including backend services, database management, and security measures.

## ✅ System Architecture Overview

The application consists of multiple components that work together to process Shopify orders, manage data, and securely communicate with external services.

## 🏗 Main Components

- Flask (Python Backend): Handles webhook processing, API requests, and order data management.

- Nginx (Reverse Proxy): Acts as an intermediary between external requests and the Flask application, ensuring secure and efficient request handling.

- SQLite (Database): Stores order data, logs, and supplier information for quick retrieval and record-keeping.

- FTP Integration: Transmits order files in XML format to supplier servers for order fulfillment.

- Systemd Service: Ensures continuous application uptime and automatic restart upon failure.

## ✅ Architecture Diagram (Mermaid)

Below is a Mermaid diagram visualizing the flow of orders from Shopify to the suppliers.

        graph TD;
            A[Shopify Order] -->|Webhook| B[Flask Application]
            B -->|HMAC Verification| C{Valid Request?}
            C -- Yes --> D[Process Order]
            C -- No --> E[Reject Request]
            D -->|Store Order Data| F[SQLite Database]
            D -->|Generate XML| G[Supplier FTP Server]
            G -->|Order Sent| H[Supplier Confirmation]
            H -->|Log Success| I[Logging System]
            G -->|Error?| J{Retry Mechanism}
            J -- Yes --> G
            J -- No --> K[Log Failure]

This diagram represents the process from webhook reception to supplier order confirmation.

## ✅ Security Measures

Security is a major focus of this system, incorporating multiple layers of protection:

## 🔒 SSL/TLS Encryption (Let's Encrypt)

Ensures all data transmitted between Shopify, the server, and suppliers is encrypted.

Managed automatically via Certbot.

        sudo certbot --nginx -d yourdomain.com

## 🔒 Reverse Proxy with Nginx

Prevents direct access to the Flask application.

Routes traffic securely through port 443 (HTTPS).

        server {
            listen 443 ssl;
            server_name yourdomain.com;
            location / {
                proxy_pass http://unix:/opt/shopify-app/shopify-app.sock;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
# 🔒 HMAC Signature Verification

Ensures only Shopify can send valid webhooks.

Uses the Shopify API Secret Key to validate incoming requests.

        import hmac
        import hashlib
        import base64
        import os
        from flask import request

        SHOPIFY_SECRET = os.getenv("SHOPIFY_API_SECRET")

        def verify_hmac():
            received_hmac = request.headers.get("X-Shopify-Hmac-SHA256")
            data = request.get_data()
            computed_hmac = base64.b64encode(hmac.new(SHOPIFY_SECRET.encode(), data, hashlib.sha256).digest()).decode()
            return hmac.compare_digest(computed_hmac, received_hmac)