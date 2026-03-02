# PeterPay Mobile Integration Guide

Welcome to the **PeterPay Mobile Integration Guide**. This repository provides information and implementation details for integrating PeterPay into your mobile applications (Android & iOS).

## Overview

PeterPay allows developers to easily process mobile money payments within their applications. This guide will walk you through the process of setting up and handling payments.

## Prerequisites

1. **PeterPay Account**: You'll need a merchant account on [PeterPay](https://www.peterpay.link).
2. **API Key**: Found in your PeterPay dashboard.
3. **Backend Support**: It's highly recommended to use a backend server to handle sensitive API keys.

## Implementation Steps

### 1. initiating a Payment
To initiate a payment, send a POST request to the PeterPay API.

**Endpoint:** `https://www.peterget.com/api/v1/create_order`

**Headers:**
- `X-API-KEY`: Your unique API Key
- `Content-Type`: `application/json`

**Body Example:**
```json
{
    "amount": 1000,
    "buyer_phone": "2557XXXXXXXX",
    "buyer_name": "John Doe",
    "buyer_email": "john@example.com"
}
```

### 2. verifying Payment Status
Once the payment is initiated, you can check its status using the `order_id` received in the response.

**Endpoint:** `https://www.peterpay.link/api/v1/order_status`

**Body Example:**
```json
{
    "order_id": "YOUR_ORDER_ID"
}
```

### 3. handling Webhooks
Configure a **Webhook URL** in your PeterPay dashboard to receive real-mtime updates on payment status. This is the most reliable way to confirm successful transactions.

## Best Practices
- **Security**: Never store your API keys directly in the mobile application's code. Use a backend service to communicate with PeterPay.
- **User Experience**: Show a clearly marked payment processing screen and confirmation once the transaction is completed.

## Support
If you have any questions or need further assistance, please reach out via:
- Website: [https://www.peterpay.link](https://www.peterpay.link)
- Email: support@peterpay.link

---
*Developed by [Peter Joram](https://www.peterpay.link)*
