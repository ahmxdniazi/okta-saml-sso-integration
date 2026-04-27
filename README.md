[![Okta SAML SSO Integration Lab Banner](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/banner.svg)](https://github.com/ahmxdniazi/okta-saml-sso-integration)

&nbsp;

[![Okta](https://img.shields.io/badge/Okta-007DC1?style=for-the-badge&logo=okta&logoColor=white)](https://okta.com)
[![SAML](https://img.shields.io/badge/SAML_2.0-FF6600?style=for-the-badge&logo=xml&logoColor=white)](https://en.wikipedia.org/wiki/SAML_2.0)
[![Security](https://img.shields.io/badge/IAM-Zero_Trust-0A84FF?style=for-the-badge&logo=shield&logoColor=white)](https://en.wikipedia.org/wiki/Zero_trust_security_model)
[![Status](https://img.shields.io/badge/Status-Completed-22c55e?style=for-the-badge)](https://github.com/ahmxdniazi/okta-saml-sso-integration)

**A hands-on implementation of enterprise-grade Single Sign-On using Okta as Identity Provider with SAML 2.0**

[Overview](#-overview) · [Architecture](#-architecture) · [Implementation](#-implementation-steps) · [Screenshots](#-screenshots) · [Key Concepts](#-key-concepts)

---

## 🔍 Overview

This project demonstrates the **end-to-end configuration of SAML 2.0-based Single Sign-On** using **Okta** as the Identity Provider (IdP). The lab covers everything from creating a custom SAML application in Okta, configuring metadata and X.509 certificates, provisioning a test user, and verifying SSO login through the Okta user dashboard.

This is a real-world enterprise IAM (Identity and Access Management) scenario, the same pattern used by organizations globally to centralize authentication and eliminate password sprawl.

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          SAML 2.0 SSO Flow                              │
├─────────────┐     ┌──────────────────┐     ┌──────────────────────────┐│
│             │     │                  │     │                          ││
│    User     │────▶│  Service Provider│────▶│   Okta (Identity        ││
│  (Browser)  │     │  (SAML Test App) │     │    Provider / IdP)      ││
│             │     │                  │     │                          ││
│             │◀────│  Access Granted  │◀────│  SAML Assertion +       ││
│             │     │                  │     │  X.509 Signed Response  ││
└─────────────┘     └──────────────────┘     └──────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────┘
```

**Components:**

- **Identity Provider (IdP):** Okta (integrator-7188680 org)
- **Service Provider (SP):** Custom SAML Test App
- **Protocol:** SAML 2.0
- **Signing:** X.509 Certificate (RSA)
- **User Store:** Okta Universal Directory

---

## 🛠 Tech Stack & Tools

| Tool | Purpose |
|---|---|
| **Okta Developer Org** | Identity Provider (IdP) |
| **SAML 2.0 Protocol** | Federation Standard |
| **X.509 Certificate** | Assertion Signing & Encryption |
| **Okta Admin Console** | App configuration & user provisioning |
| **Okta End-User Dashboard** | SSO login verification |
| **SAML Metadata XML** | SP ↔ IdP handshake configuration |

---

## 📋 Implementation Steps

### Step 1 — Create SAML App Integration in Okta

Navigated to **Applications → Create App Integration** in the Okta Admin Console and selected **SAML 2.0** as the sign-on method. Configured the application with:

- App Name: `SAML Test App`
- Sign-on method: `SAML 2.0`

![SAML Test App created in Applications list](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/01-saml-app-created.png)

---

### Step 2 — Configure SAML Settings

Under the **Sign On** tab of the SAML Test App, retrieved critical IdP metadata required to link the SP:

| Field | Value |
|---|---|
| **Metadata URL** | `https://integrator-7188680.okta.com/app/exk12c2xst6TMHjEi698/sso/saml/metadata` |
| **SSO URL** | `https://integrator-7188680.okta.com/app/integrator-7188680_samltestapp_1/exk12c2xst6TMHjEi698/sso/saml` |
| **IdP Issuer** | `http://www.okta.com/exk12c2xst6TMHjEi698` |
| **Sign-on Method** | SAML 2.0 |

![Sign On tab showing Metadata URL and SAML 2.0 settings](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/03-saml-app-sign-on-settings.png)

---

### Step 3 — Extract X.509 Certificate

From the **"How to Configure SAML 2.0"** setup guide, extracted the **X.509 signing certificate** used by Okta to sign SAML assertions. This certificate must be installed on the Service Provider side to validate the IdP's signature.

```
-----BEGIN CERTIFICATE-----
MIIDtDCCApygAwIBAgIGAZ3Jw4AGMA0GCSqGSIb3DQEBCwUAMIGa...
[truncated for display]
-----END CERTIFICATE-----
```

![X.509 certificate for SAML assertion signing](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/08-xml-certificate.png)

---

### Step 4 — Verify App Assignments Panel

Confirmed the app is **Active** and inspected the **Assignments** tab to manage user/group access to the SAML application.

![Assignments tab — managing user access](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/04-saml-app-assignments.png)

---

### Step 5 — Provision Test User in Okta

Created a test user **Sarmad Farooq** (`sarmadfarooq@cyber.com`) in the Okta Universal Directory. Initial state: **Pending user action — password selection required.**

![Test user created, pending activation](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/05-user-pending-activation.png)

---

### Step 6 — Assign User to SAML App

Assigned **Sarmad Farooq** to the **SAML Test App** as an individual user. Assignment type: `Individual`, App username: `sarmadfarooq@cyber.com`.

![User assigned to SAML Test App](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/06-user-app-assigned.png)

---

### Step 7 — Verify SSO via Okta End-User Dashboard

Logged into the Okta End-User Dashboard as `sarmadfarooq@cyber.com`. The dashboard confirmed:

- Notification: **"You were assigned new apps"**
- App listed: **SAML Test App** ✅

This confirms the full SSO provisioning flow is working correctly.

![User dashboard showing SAML app notification](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/07-test-user-okta-dashboard.png)

---

## 📸 Screenshots

| Step | Screenshot | Description |
|---|---|---|
| 1 | ![](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/01-saml-app-created.png) | SAML Test App visible in Applications list |
| 2 | ![](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/03-saml-app-sign-on-settings.png) | Sign On tab — Metadata URL & SAML 2.0 settings |
| 3 | ![](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/04-saml-app-assignments.png) | Assignments tab — managing user access |
| 4 | ![](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/05-user-pending-activation.png) | Test user created, pending activation |
| 5 | ![](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/06-user-app-assigned.png) | User assigned to SAML Test App |
| 6 | ![](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/07-test-user-okta-dashboard.png) | User dashboard showing SAML app notification |
| 7 | ![](https://github.com/ahmxdniazi/okta-saml-sso-integration/raw/main/screenshots/08-xml-certificate.png) | X.509 certificate for SAML assertion signing |

---

## 🔑 Key Concepts

### What is SAML 2.0?

**Security Assertion Markup Language (SAML) 2.0** is an XML-based open standard for exchanging authentication and authorization data between parties — specifically, between an Identity Provider (IdP) and a Service Provider (SP). It enables **Single Sign-On (SSO)**, allowing users to authenticate once and gain access to multiple systems.

### SAML Flow (SP-Initiated)

1. User tries to access the Service Provider (SP)
2. SP redirects the user to the IdP (Okta) with a SAML AuthnRequest
3. Okta authenticates the user
4. Okta returns a **signed SAML Assertion** to the SP
5. SP validates the assertion using the **X.509 certificate**
6. User is granted access ✅

### Key SAML Metadata Fields

| Field | Purpose |
|---|---|
| **SSO URL** | Endpoint where the SP sends the AuthnRequest |
| **IdP Issuer** | Unique identifier for the IdP |
| **X.509 Certificate** | Used to verify the IdP's digital signature on assertions |
| **Metadata URL** | SP can auto-configure by fetching this XML |
| **NameID Format** | How the user's identity is communicated (e.g., email) |

---

## 🎯 What I Learned

- Configuring a **custom SAML 2.0 App Integration** in Okta from scratch
- Understanding the difference between **Identity Provider (IdP)** and **Service Provider (SP)**
- Extracting and applying **X.509 certificates** for assertion signing
- The complete **SAML assertion flow** and what each metadata field means
- **User lifecycle management** in Okta — creation, activation, and app assignment
- How **enterprise SSO** eliminates password fatigue and centralizes identity

---

## 🔐 Security Considerations

- SAML assertions are **digitally signed** using the X.509 certificate to prevent tampering
- Okta acts as the **single source of truth** for identity — no passwords stored at the SP
- **App assignment** controls who has access — principle of least privilege enforced at the IdP level
- Metadata URLs expose only public configuration — no secrets are shared

---

## 📂 Project Structure

```
okta-saml-sso-project/
│
├── README.md                              # This file
├── banner.svg                             # Project banner
├── saml-flow-diagram.svg                  # SAML flow architecture diagram
│
└── screenshots/
    ├── 01-saml-app-created.png            # App list showing SAML Test App
    ├── 03-saml-app-sign-on-settings.png   # Sign On configuration
    ├── 04-saml-app-assignments.png        # Assignments tab
    ├── 05-user-pending-activation.png     # New user state
    ├── 06-user-app-assigned.png           # User-to-app assignment
    ├── 07-test-user-okta-dashboard.png    # End-user SSO dashboard
    └── 08-xml-certificate.png             # X.509 certificate
```

---

## 👤 Author

**Muhammad Ahmad**

- Email: [m.ahmad.cybersec@gmail.com](mailto:m.ahmad.cybersec@gmail.com)
- Domain: Cybersecurity | IAM | Identity Federation

---

*Built as part of a hands-on Identity & Access Management (IAM) lab.*

⭐ If you found this useful, consider starring the repo!
