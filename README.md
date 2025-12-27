 
* **Spring Security Architecture**
* **Each component’s role (easy language)**
* **End-to-end request flow**
* **JWT + CSRF integration**
* **Interview-ready explanations**
* **Simple ASCII diagrams (safe for Markdown repos)**
 
---

## Visual Reference (for understanding)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/0%2AFRWqYhSfB_cQ4xtD.png)

![Image](https://docs.spring.io/spring-security/reference/_images/servlet/architecture/securityfilterchain.png)

![Image](https://bs-uploads.toptal.io/blackfish-uploads/uploaded_file/file/412345/image-1602672495860.085-952930c83f53503d7e84d1371bec3775.png)

---

## 📄 `README.md`

```md
# Spring Security Architecture (with CSRF & JWT) – Easy & Complete Guide

This document explains **Spring Security architecture** in a **simple, structured, and interview-ready way**, including:
- Authentication
- Authorization
- Security filter chain
- JWT-based security
- CSRF protection

---

## 1. What is Spring Security?

Spring Security is a **filter-based security framework** that protects applications by handling:

- Authentication (Who are you?)
- Authorization (What are you allowed to do?)
- Common security attacks (CSRF, session fixation, etc.)

Every HTTP request passes through Spring Security **before** reaching controllers.

---

## 2. High-Level Architecture

```

Client
↓
Security Filter Chain
↓
Authentication
↓
SecurityContext
↓
Authorization
↓
Controller
↓
Response

```

Spring Security works **outside your business logic** to ensure safety.

---

## 3. Security Filter Chain (Core of Spring Security)

### What is it?
A **chain of servlet filters** that intercept every request.

### Why filters?
Security must:
- Run before controllers
- Be applied consistently to all requests

### Example filters:
- Authentication filter
- JWT filter
- Authorization filter
- Exception handler filter

```

Request
→ Filter 1
→ Filter 2
→ Filter 3
→ Controller

```

If any filter fails → request is blocked.

---

## 4. Authentication (Who are you?)

### Authentication Object
Holds user details after successful login:
- Username
- Credentials
- Roles / authorities

```

Authentication
├── Principal (User)
├── Credentials
└── Authorities

```

---

## 5. AuthenticationManager

### Role:
Coordinates authentication.

```

AuthenticationManager
↓
AuthenticationProvider

```

It does not authenticate itself — it delegates.

---

## 6. AuthenticationProvider

### Role:
Performs **actual authentication logic**.

Examples:
- Username + password validation
- JWT token validation
- OAuth token validation

If authentication fails → exception thrown.

---

## 7. UserDetailsService

### Role:
Loads user information from storage.

```

loadUserByUsername(username)

```

Data source can be:
- Database
- LDAP
- External service

Separates **user lookup** from **security logic**.

---

## 8. PasswordEncoder

### Role:
Hashes and verifies passwords securely.

- Plain passwords are never stored
- BCrypt is industry standard

```

raw password → hashed → stored

```

---

## 9. SecurityContext & SecurityContextHolder

### SecurityContext:
Stores the authenticated user for the current request.

### SecurityContextHolder:
Stores SecurityContext using **ThreadLocal**.

```

Thread
└── SecurityContext
└── Authentication

````

Each request has its own isolated security data.

---

## 10. Authorization (What are you allowed to do?)

### Happens after authentication.

Based on:
- Roles (ROLE_ADMIN)
- Authorities (READ_USER)
- URL rules
- Method annotations

Examples:
```java
@PreAuthorize("hasRole('ADMIN')")
@Secured("ROLE_USER")
````

If unauthorized → 403 Forbidden.

---

## 11. Exception Handling in Security

Handled by security filters, not controllers.

| Scenario          | HTTP Code |
| ----------------- | --------- |
| Not authenticated | 401       |
| Not authorized    | 403       |

---

## 12. JWT-Based Security (Stateless)

### Why JWT?

* No server-side session
* Scales well
* Ideal for microservices

### JWT Flow

```
Login Request
 → Credentials validated
 → JWT generated
 → Token sent to client

Client Request
 → JWT sent in Authorization header
 → JWT validated
 → Authentication set in SecurityContext
```

Server does not store session state.

---

## 13. CSRF (Cross-Site Request Forgery)

### What is CSRF?

An attack where a logged-in user is tricked into sending an unwanted request.

### Why it happens?

* Browsers auto-send cookies
* Server trusts cookies
* Attacker abuses this trust

---

## 14. CSRF Protection Mechanism

### CSRF Token Rule:

Only requests with a valid token are allowed.

```
Server → CSRF token
Client → Sends token back
Server → Validates token
```

If token missing → request rejected.

---

## 15. CSRF in Spring Security

### Default:

* Enabled
* Required for POST / PUT / DELETE / PATCH

If token missing:

```
403 Forbidden
```

---

## 16. Why CSRF Is Disabled for JWT APIs

JWT-based APIs:

* Use Authorization headers
* Tokens are NOT auto-sent by browsers
* Attacker cannot forge headers

Therefore:

```
http.csrf().disable();
```

CSRF is unnecessary for stateless APIs.

---

## 17. Cookie vs Header Authentication

| Authentication   | CSRF Risk |
| ---------------- | --------- |
| Session + Cookie | High      |
| JWT Header       | None      |
| OAuth Header     | None      |

---

## 18. End-to-End Request Flow (JWT Example)

```
Client
 → JWT Filter
 → AuthenticationManager
 → SecurityContext
 → Authorization Filter
 → Controller
 → Response
```

---

## 19. Interview One-Minute Summary

Spring Security uses a filter-based architecture where every request passes through a security filter chain. Authentication is handled via AuthenticationManager and providers, user details are stored in the SecurityContext per request, and authorization is enforced before controllers. Modern applications use JWT for stateless authentication and disable CSRF because tokens are sent via headers instead of cookies.

---

## 20. Key Takeaways

* Spring Security is filter-based
* Authentication happens before authorization
* SecurityContext is request-scoped
* JWT eliminates server-side sessions
* CSRF exists only with cookie-based auth

---

 
```
