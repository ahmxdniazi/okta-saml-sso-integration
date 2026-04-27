# SAML 2.0 Concepts — Quick Reference

## What is SAML?

**Security Assertion Markup Language (SAML) 2.0** is an XML-based open standard that enables
**Single Sign-On (SSO)** by securely exchanging authentication data between:

- **Identity Provider (IdP)** — the system that verifies identity (e.g., Okta)
- **Service Provider (SP)** — the application the user wants to access

---

## Core Terminology

| Term | Definition |
|------|-----------|
| **IdP** | Identity Provider — authenticates the user (Okta in this lab) |
| **SP** | Service Provider — the app users want to access (SAML Test App) |
| **SAML Assertion** | XML document from IdP asserting who the user is |
| **AuthnRequest** | XML request from SP asking IdP to authenticate a user |
| **ACS URL** | Assertion Consumer Service URL — SP endpoint that receives SAML response |
| **X.509 Certificate** | Public key certificate used to sign and verify SAML assertions |
| **Metadata** | XML document describing IdP or SP configuration |
| **NameID** | Identifier for the authenticated user (usually email) |
| **Relay State** | Optional parameter to redirect after SSO completes |
| **SSO URL** | The IdP endpoint that receives the AuthnRequest |
| **Entity ID / Issuer** | Unique identifier for the IdP |

---

## SAML Assertion Structure (simplified)

```xml
<samlp:Response>
  <saml:Issuer>http://www.okta.com/exk12c2xst6TMHjEi698</saml:Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
  </samlp:Status>
  <saml:Assertion>
    <saml:Subject>
      <saml:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress">
        sarmadfarooq@cyber.com
      </saml:NameID>
    </saml:Subject>
    <saml:Conditions NotBefore="..." NotOnOrAfter="..."/>
    <saml:AuthnStatement AuthnInstant="..."/>
    <!-- Digital Signature (X.509) -->
    <ds:Signature>...</ds:Signature>
  </saml:Assertion>
</samlp:Response>
```

---

## This Lab's Okta Configuration

| Field | Value |
|-------|-------|
| Org | `integrator-7188680.okta.com` |
| App Name | `SAML Test App` |
| App ID | `exk12c2xst6TMHjEi698` |
| SSO URL | `https://integrator-7188680.okta.com/app/integrator-7188680_samltestapp_1/exk12c2xst6TMHjEi698/sso/saml` |
| IdP Issuer | `http://www.okta.com/exk12c2xst6TMHjEi698` |
| Metadata URL | `https://integrator-7188680.okta.com/app/exk12c2xst6TMHjEi698/sso/saml/metadata` |
| Protocol | SAML 2.0 |
| Certificate | X.509 RSA (see `screenshots/08-xml-certificate.png`) |

---

## SAML vs Other SSO Protocols

| Feature | SAML 2.0 | OAuth 2.0 | OpenID Connect |
|---------|----------|----------|---------------|
| **Data Format** | XML | JSON (tokens) | JSON (JWT) |
| **Use Case** | Enterprise SSO | Authorization | Authentication |
| **Year** | 2005 | 2012 | 2014 |
| **Complexity** | High | Medium | Low |
| **Enterprise Adoption** | Very High | High | High |
| **Okta Support** | ✅ Full | ✅ Full | ✅ Full |

---

## Security Best Practices Applied

- ✅ X.509 certificate signing to prevent assertion tampering
- ✅ Short assertion validity window (NotBefore / NotOnOrAfter)
- ✅ Individual user assignment (least privilege)
- ✅ App activation requires admin action
- ✅ Centralized user lifecycle management via Okta
- ✅ No passwords transmitted to or stored at the SP

---

## Common SAML Errors (for reference)

| Error | Cause | Fix |
|-------|-------|-----|
| `Signature validation failed` | Cert mismatch | Re-download X.509 cert from Okta |
| `InResponseTo mismatch` | Replay attack / stale request | Check clock sync (NTP) |
| `Audience Restriction failed` | Wrong SP Entity ID | Verify SP Entity ID matches Okta config |
| `User not assigned` | App assignment missing | Assign user in Okta Admin Console |
| `Expired assertion` | Time drift | Ensure server time is synced |
