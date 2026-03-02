# PeterPay Mobile Integration Guide

Karibu kwenye mwongozo wa kuunganisha PeterPay kwenye Mobile Apps (Android & iOS).

## Utangulizi
PeterPay ni mfumo rahisi wa malipo unaokuwezesha kupokea pesa kupitia Mobile Money (M-Pesa, Tigo Pesa, Airtel Money, n.k.) moja kwa moja kwenye application yako.

## Hatua za Kuunganisha PeterPay

### 1. Kutuma Ombi la Malipo (App Request)
Kwenye Mobile App, tunashauri kutumia endpoint ya `/api/v1/order_pay.php`. Hii inahakikisha makato yamefanyika na unapata kiasi halisi (Net Amount).

**Endpoint:**
`POST /api/v1/order_pay.php`

**JSON Payload Mfano:**
```json
{
  "amount": 10000,
  "buyer_phone": "255712345678",
  "buyer_name": "Juma Kapuya",
  "network": "Tigo" // Optional (Mfumo utatambua ukiiacha)
}
```

### 2. Kupokea Taarifa (Callbacks / Webhooks)
Muhimu kwa Apps: Usitegemee "Success Response" ya App pekee kuthibitisha malipo. Mtandao unaweza kukatika. Lazima uweke Webhook URL kwenye Dashboard yako ili server yetu ijuze server yako (Backend) malipo yakikamilika.

**Jinsi ya Kuweka Endpoints Nyingi (Multiple Apps):**
Ikiwa una Apps zaidi ya moja (Mfano: App ya Dereva na App ya Mteja), unaweza kuweka Webhook URLs zote kwenye Dashboard. PeterPay itatuma taarifa kwa URL zote zilizoorodheshwa.

**Mfano wa kuweka Webhooks kwenye Dashboard:**
```
https://myapp-one.com/api/callback
https://myapp-two.com/payment/webhook
https://backup-server.com/listener
```
Kila URL itapokea taarifa ya malipo. Backend yako inatakiwa kuangalia `order_id` ili kujua kama muamala unaihusu.

### 3. Usalama (App Signature)
Ili kuzuia watu wengine kutumia API Key yako, tunashauri utume `X-App-Signature` kwenye header. Hii ni HMAC SHA256 ya JSON payload yako ikitumia API Secret yako.

`Signature = HMAC_SHA256(JSON_BODY, API_SECRET)`

### 4. Mfano wa Code (Android - Java)
```java
// Import: javax.crypto.Mac, javax.crypto.spec.SecretKeySpec
JSONObject json = new JSONObject();
json.put("amount", 10000);
json.put("buyer_phone", "255712345678");
json.put("buyer_name", "Mteja App");
String payload = json.toString();
String apiSecret = "YOUR_API_SECRET"; // Hifadhi hii kwa siri (mf: NDK/C++)
String signature = "";
try {
    Mac sha256_HMAC = Mac.getInstance("HmacSHA256");
    SecretKeySpec secret_key = new SecretKeySpec(apiSecret.getBytes("UTF-8"), "HmacSHA256");
    sha256_HMAC.init(secret_key);
    byte[] bytes = sha256_HMAC.doFinal(payload.getBytes("UTF-8"));
    StringBuilder hash = new StringBuilder();
    for (byte b : bytes) {
        String hex = Integer.toHexString(0xFF & b);
        if (hex.length() == 1) hash.append('0');
        hash.append(hex);
    }
    signature = hash.toString();
} catch (Exception e) {
    e.printStackTrace();
}

RequestBody body = RequestBody.create(payload, MediaType.parse("application/json"));
Request request = new Request.Builder()
    .url("https://www.peterpay.link/api/v1/order_pay.php")
    .post(body)
    .addHeader("X-API-KEY", "YOUR_API_KEY")
    .addHeader("X-App-Signature", signature)
    .build();

client.newCall(request).enqueue(new Callback() {
    @Override
    public void onResponse(Call call, Response response) {
        // Handle success (Show 'Enter PIN' dialog)
    }
});
```

---
**Developed by [Peter Joram](https://www.peterpay.link)**
