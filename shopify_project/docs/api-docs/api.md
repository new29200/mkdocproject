# 📂 API Documentation

This section provides a detailed overview of the Shopify Order Management API, including available endpoints, request formats, and response examples.

## ✅ OpenAPI Specification (Swagger)

The API follows the OpenAPI (Swagger) standard to provide clear documentation and easy integration.

The full OpenAPI specification is available in openapi.yaml.

You can access the interactive Swagger UI at:

        http://yourdomain.com/docs

# Example openapi.yaml:

        openapi: 3.0.0
        info:
        title: Shopify Order Management API
        description: API for handling Shopify orders and supplier communication.
        version: 1.0.0
        paths:
        /webhook:
            post:
            summary: Receives orders from Shopify webhook
            description: Processes the incoming order from Shopify.
            responses:
                '200':
                description: Order successfully processed.
                '400':
                description: Invalid request.
        /orders:
            get:
            summary: Retrieve all processed orders
            responses:
                '200':
                description: Returns a list of orders.
        /order/{id}:
            get:
            summary: Retrieve a specific order
            parameters:
                - name: id
                in: path
                required: true
                schema:
                    type: integer
            responses:
                '200':
                description: Order details returned.
                '404':
                description: Order not found.

## ✅ Available API Endpoints

# POST /webhook

Receives order data from Shopify when a customer completes a purchase.

# Request Example (cURL):

        curl -X POST "http://yourdomain.com/webhook" \
            -H "Content-Type: application/json" \
            -H "X-Shopify-Hmac-SHA256: your_hmac_signature" \
            -d '{ "order_id": 12345, "customer": { "name": "John Doe" }, "items": [...] }'

# Request Example (Python):

        import requests

        headers = {
            "Content-Type": "application/json",
            "X-Shopify-Hmac-SHA256": "your_hmac_signature"
        }

        data = {
            "order_id": 12345,
            "customer": { "name": "John Doe" },
            "items": [...]
        }

        response = requests.post("http://yourdomain.com/webhook", json=data, headers=headers)
        print(response.status_code, response.json())

# GET /orders

Retrieves a list of all processed orders.

Request Example (cURL):

        curl -X GET "http://yourdomain.com/orders"

Request Example (Python):

        response = requests.get("http://yourdomain.com/orders")
        print(response.json())

# GET /order/{id}

Retrieves the details of a specific order.

Request Example (cURL):

        curl -X GET "http://yourdomain.com/order/12345"

Request Example (Python):

        order_id = 12345
        response = requests.get(f"http://yourdomain.com/order/{order_id}")
        print(response.json())