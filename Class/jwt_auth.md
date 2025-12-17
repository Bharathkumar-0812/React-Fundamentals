# JWT Authentication & SSO (Keycloak) – Basics

This document explains **JWT authentication** and **Single Sign-On (SSO) using Keycloak**, focusing on **core concepts**, **flow**, and **how they fit into a React app**. No magic, no buzzwords, just mechanics.

---

## 1. Why Authentication Exists

Authentication answers one question:

> **Who are you?**

Authorization answers another:

> **What are you allowed to do?**

JWT and SSO mostly help with **authentication**, then enable authorization.

---

## 2. What is JWT (JSON Web Token)

A **JWT** is:

* A compact string
* Digitally signed
* Used to prove identity between client and server

It looks like nonsense but is very structured.

Example:

```
xxxxx.yyyyy.zzzzz
```

---

## 3. JWT Structure

A JWT has **three parts**:

### 1️⃣ Header

* Algorithm (`HS256`, `RS256`)
* Token type

### 2️⃣ Payload

* User data (claims)
* Example:

```json
{
  "sub": "123",
  "email": "user@test.com",
  "role": "admin",
  "exp": 1710000000
}
```

### 3️⃣ Signature

* Verifies token integrity
* Prevents tampering

Important:

* JWT is **not encrypted**
* Anyone can read the payload
* Never store secrets in it

---

## 4. How JWT Authentication Works

Typical flow:

1. User logs in with credentials
2. Server verifies credentials
3. Server issues JWT
4. Client stores JWT
5. Client sends JWT on every request
6. Server validates JWT

Header example:

```http
Authorization: Bearer <token>
```

---

## 5. Where to Store JWT (Frontend)

Common options:

### ❌ LocalStorage

* Easy
* Vulnerable to XSS

### ❌ SessionStorage

* Slightly better
* Still XSS-prone

### ✅ HttpOnly Cookies (Best)

* Not accessible via JS
* Safer against XSS

Security always wins over convenience.

---

## 6. Token Expiry & Refresh

JWTs are short-lived.

* **Access Token** → short life
* **Refresh Token** → longer life

Flow:

1. Access token expires
2. Client sends refresh token
3. Server issues new access token

This prevents long-term abuse.

---

## 7. What is SSO (Single Sign-On)

SSO allows:

* Login once
* Access multiple applications

Without SSO:

* Login everywhere

With SSO:

* Login once
* Trust shared identity

SSO relies on a **central identity provider**.

---

## 8. What is Keycloak

**Keycloak** is:

* Open-source Identity Provider (IdP)
* Manages users, roles, tokens
* Implements OAuth2 & OpenID Connect

Keycloak does:

* Login pages
* Token issuance
* User management

Your app does not handle passwords.

---

## 9. Keycloak Core Concepts

* **Realm** – isolated auth space
* **Client** – your application
* **User** – authenticated entity
* **Role** – permission grouping
* **Token** – JWT issued by Keycloak

One realm can serve many apps.

---

## 10. Keycloak Login Flow (High-Level)

1. User tries to access app
2. App redirects to Keycloak
3. User logs in at Keycloak
4. Keycloak redirects back
5. App receives JWT
6. App uses token for API calls

Your app never sees the password.

---

## 11. Using JWT in React (Concept)

Frontend responsibilities:

* Store token securely
* Attach token to API requests
* Handle logout
* Handle token expiration

Auth logic lives **outside UI components**.

---

## 12. Logout Flow

1. Client clears tokens
2. Client notifies Keycloak
3. Keycloak invalidates session

True logout requires IdP involvement.

---

## 13. Common Mistakes

Avoid:

* Storing sensitive data in JWT
* Long-lived access tokens
* Handling passwords in frontend
* Rolling your own auth

Auth bugs are expensive.

---

## 14. Mental Model

* JWT = proof of identity
* Keycloak = trusted authority
* React = consumer, not owner, of auth

Frontend trusts tokens. Backend verifies them.

---

## 15. Key Takeaways

* JWTs are signed, not encrypted
* SSO centralizes authentication
* Keycloak handles identity securely
* Frontend should stay dumb about auth

---

**Next Logical Topics:**

* OAuth2 vs OpenID Connect
* Role-based access control (RBAC)
* Protecting routes in React
* Secure API communication


