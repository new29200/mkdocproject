# 📂 FAQ & Glossary

# ❓ Frequently Asked Questions

# How does the webhook verification work?

Shopify includes an X-Shopify-Hmac-SHA256 header with each webhook request. The server recalculates this hash and compares it to ensure the request's authenticity.

# What happens if an order fails to process?

Failed orders are logged, and a retry mechanism is in place. If the issue persists, an alert is triggered for manual review.

# How secure is the data transfer?

All API requests and webhook transmissions are encrypted using HTTPS (SSL/TLS). Sensitive credentials are stored in environment variables.

# Can I manually resend an order to the supplier?

Yes, orders that fail to send via FTP are stored in a queue and can be retried using a manual script.

## 📖 Glossary

Webhook: A method for Shopify to send real-time updates to the backend.

HMAC (Hash-based Message Authentication Code): A cryptographic method for verifying request authenticity.

Reverse Proxy: Nginx is used as a reverse proxy to route and secure requests.

FTP (File Transfer Protocol): Used for transmitting order XML files to suppliers.

Systemd: A service management system ensuring the continuous running of the application.