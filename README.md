# PeterPay Mobile Integration Guide

Karibu kwenye mwongozo wa kuunganisha PeterPay kwenye Mobile Apps (Android & iOS).

## Utangulizi
PeterPay ni mfumo rahisi wa malipo unaokuwezesha kupokea pesa kupitia Mobile Money (M-Pesa, Tigo Pesa, Airtel Money, n.k.) moja kwa moja kwenye application yako.

## Hatua za Kufuata

### 1. Pata API Key
Jisajili na upate API Key yako kutoka [www.peterpay.link](https://www.peterpay.link).

### 2. Integration (API Endpoints)
Tumia endpoints zifuatazo kuunganisha:

- **Kuanzisha Malipo (Create Order):**
  `POST https://www.peterpay.link/api/v1/create_order`
  
- **Kukagua Hali ya Malipo (Check Status):**
  `POST https://www.peterpay.link/api/v1/order_status`

### 3. Usalama (Security)
Hakikisha unatumia backend server yako kufanya communication na PeterPay API ili kulinda **API Key** yako isionekane na watumiaji wa mobile app.

---
**Developed by [Peter Joram](https://www.peterpay.link)**
