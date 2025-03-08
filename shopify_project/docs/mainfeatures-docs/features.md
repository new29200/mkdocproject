# 📂 Features

This section outlines the main features of the Shopify Order Management Application, including secure webhook handling, order processing, and logging mechanisms.

## ✅ Reception of Shopify Webhooks

The system listens for order events from Shopify via webhooks. When a customer places an order, Shopify sends a webhook request containing all relevant order details.

Webhook requests are processed through Flask, and the payload is extracted for validation and further processing.

## ✅ HMAC Validation and Security

To ensure authenticity and security, the system verifies Shopify webhooks using HMAC-SHA256 encryption.

## 🔒 Verification Process:

Shopify includes an HMAC signature in the webhook headers.

The application retrieves the X-Shopify-Hmac-SHA256 header.

The received signature is compared to a locally computed HMAC using the Shopify API Secret Key.

If the validation passes, the webhook data is processed.

# Example Python code for HMAC verification:

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

## ✅ Order Processing and Supplier Integration

Once the webhook is validated, the order is categorized based on the supplier responsible for fulfillment.

Identify supplier: Match products to predefined supplier categories (e.g., AGRIEST or FOURNIAL).

Generate XML order files: Convert order data into XML format for FTP transmission.

Send orders via FTP: Automatically send XML files to supplier FTP servers.

Confirm order reception: Wait for acknowledgment from suppliers.

# Example structure of generated XML order:

        <Order>
            <Customer>
                <Name>John Doe</Name>
                <Email>john@example.com</Email>
            </Customer>
            <Products>
                <Product>
                    <SKU>12345</SKU>
                    <Quantity>2</Quantity>
                </Product>
            </Products>
        </Order>

## ✅ Error Handling and Logging

To enhance reliability, the application includes detailed logging and automated error management:

Log all incoming webhooks in /opt/shopify-app/logs/app.log.

Track API call failures and retries.

Identify webhook validation errors (e.g., incorrect HMAC signatures).

Handle failed FTP transmissions and retry sending.

# Example Python logging setup:

        import logging

        logging.basicConfig(
            filename="/opt/shopify-app/logs/app.log",
            level=logging.INFO,
            format="%(asctime)s - %(levelname)s - %(message)s"
        )

        def log_event(event):
            logging.info(f"Event received: {event}")