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
    "access_token": "6rTENXT96VWYgbZfV5DnfV-9F2ojxEMxLVx1-M4Gh3uLx9QHRD_C9KP4eqGOIsiTqa1NI4nSweO97d3kXbp7XQ",
    "refresh_token": "afqRdI-D3glTAqEjO6ZG7q_7r7f4MfmBzFGcRpe__tSJwOACtcst1N3TbBaWWb66jY2mgOlVbZC_B2wPOsjVSA",
    "scope": "",
    "token_type": "Bearer",
    "expires_in": 1799
}
```

---

### 3. Call the API with Bearer Token

```bash
GET https://<instance>/api/1778869/asset_lifecycle_dashboard_api/asset/LAP12345
Authorization: Bearer <ACCESS_TOKEN>
```

**Sample Response:**

```json
{
    "result": {
        "asset": {
            "tag": "LAP12345",
            "name": "LAP12345 - Dell Inc. Alienware M14x",
            "model": null,
            "ci": "SN123456 - Alienware M14x",
            "assigned_to": "Abel Tuter",
            "status": "",
            "warranty_expiry": "N/A"
        },
        "incidents": [
            {
                "number": "INC0012243",
                "short_description": "Laptop not booting",
                "state": "New"
            },
            {
                "number": "INC0012244",
                "short_description": "screen flickering",
                "state": "New"
            }
        ],
        "requests": [
            {
                "number": "REQ0000001",
                "requested_for": "System Administrator",
                "stage": "requested"
            },
            {
                "number": "REQ0010002",
                "requested_for": "Adam Meyers",
                "stage": "requested"
            },
            {
                "number": "REQ0010001",
                "requested_for": "Aeron Goldtree",
                "stage": "requested"
            }
        ]
    }
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
---

## Author
- Anasuya Rampalli
---


