# 🖥️ Asset Lifecycle Dashboard API (ServiceNow + OAuth2)

## Overview
The **Asset Lifecycle Dashboard API** is a **Scripted REST API** built in ServiceNow that provides a **360° view of IT assets** — from procurement to retirement.  

It consolidates data across multiple ServiceNow tables (`alm_asset`, `incident`, `sc_request`, etc.) and returns it in a clean JSON format.  

This project also demonstrates securing the API using **OAuth 2.0** in ServiceNow.  

---

## Features
- **Asset Details** → Model, assigned user, status, warranty expiry  
- **Open Incidents** → Linked to the asset’s CI  
- **Open Requests** → Active service requests for the asset  
- **Disposal Info** → Retirement details (if applicable)  
- **OAuth 2.0 Secured Endpoint** → Token-based access control  

---

## Technical Design
- **Platform:** ServiceNow Scoped App  
- **API Endpoint:**  

```bash
GET /api/x\_asset\_lifecycle\_api/asset/{asset\_tag}
```

- **Tables Queried:**  
- `alm_asset` → Asset details  
- `incident` → Open incidents linked to asset  
- `sc_request` → Open requests linked to asset  
- **Auth:** OAuth 2.0 (Client Credentials Grant)  

## OAuth 2.0 Setup in ServiceNow

### 1. Create OAuth Provider
1. Navigate to **System OAuth > Application Registry**  
2. Click **New** → **Create an OAuth API endpoint for external clients**  
3. Fill in details:  
 - Name: `Asset Lifecycle API Client`  
 - Client ID: *(auto-generated)*  
 - Client Secret: *(save securely)*  
 - Grant type: **Client Credentials**  

---

### 2. Test Authentication
Request an access token with **client credentials**:

```bash
POST https://<instance>.service-now.com/oauth_token.do
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=<CLIENT_ID>&client_secret=<CLIENT_SECRET>
````

**Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "token_type": "Bearer",
  "expires_in": 1799,
  "scope": "useraccount"
}
```

---

### 3. Call the API with Bearer Token

```bash
GET https://<instance>.service-now.com/api/x_asset_lifecycle_api/asset/C12345
Authorization: Bearer <ACCESS_TOKEN>
```

**Sample Response:**

```json
{
  "asset": {
    "tag": "C12345",
    "name": "Dell Latitude 7420",
    "model": "Dell Laptop",
    "assigned_to": "Jane Doe",
    "status": "In Use",
    "warranty_expiry": "2025-12-31"
  },
  "incidents": [
    {
      "number": "INC0010023",
      "short_description": "Laptop not booting",
      "state": "In Progress"
    }
  ],
  "requests": [
    {
      "number": "REQ0010056",
      "requested_for": "Jane Doe",
      "stage": "Fulfillment"
    }
  ],
  "disposal": {}
}
```

---

## Project Structure

```plaintext
/AssetLifecycleAPI
 ├── README.md
 ├── docs/
 │    ├── api-design.md
 │    ├── oauth2-setup.md
 └── screenshots/
      ├── oauth-client-registry.png
      ├── api-test-postman.png
```

---

## Testing

* **Postman / Curl** → For quick API testing with OAuth2 tokens
* **Automated Tests** → Could be extended with ServiceNow ATF or external test runners

---

## Learning Outcomes

* Building **Scripted REST APIs** in ServiceNow
* Querying multiple tables with **GlideRecord**
* Implementing **OAuth 2.0 Client Credentials** in ServiceNow
* Securing APIs with **Bearer tokens**

---

## 🏆 Resume Highlight

> *Built a secured Scripted REST API in ServiceNow that consolidates IT asset lifecycle details from multiple modules, with OAuth 2.0 authentication for enterprise-grade security.*

```



