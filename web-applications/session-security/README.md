# Session Security

A **user session** is a sequence of requests from the same client, along with the corresponding responses, during a specific time frame. Maintaining user sessions is crucial for modern web applications to track user information and status, enabling features like access rights and localization settings during user interactions.

Since **HTTP** is a stateless protocol, each request must contain all necessary information for the server to process it. Consequently, session state is primarily managed on the client side, utilizing methods such as cookies, URL parameters, and body arguments for session tracking.

***

### 🔒 Session Identifier Security

A **session identifier (Session ID)** is essential for distinguishing user sessions. If an attacker gains access to a session identifier, they can perform **session hijacking**, impersonating the victim.

#### 🚨 Methods of Session ID Theft

* **Passive Traffic Sniffing**: Capturing data packets.
* **Log Identification**: Finding session IDs in logs.
* **Prediction**: Guessing session IDs.
* **Brute Force**: Systematically trying combinations.

#### 🔑 Security Characteristics of Session Identifiers

1. **Validity Scope**: Should be valid for one session only.
2. **Randomness**: Must be generated using a robust algorithm to prevent prediction.
3. **Validity Time**: Should expire after a set duration.

Established programming technologies like **PHP** and **JSP** typically generate secure session identifiers. Custom implementations should be approached with caution and thoroughly tested.

#### 📍 Storage Locations and Risks

* **URL**: Can leak through the HTTP Referer header and browser history.
* **HTML**: May be stored in browser cache and intermediate proxies.
* **sessionStorage**: Data persists as long as the tab is open but clears when the session ends.
* **localStorage**: Data remains until explicitly deleted, except in private browsing sessions.

Session identifiers lacking server management or not adhering to security characteristics are considered weak.

***

### ⚔️ Session Attacks

#### 1. **Session Hijacking**

Attackers exploit insecure session identifiers to impersonate victims by obtaining their session IDs.

#### 2. **Session Fixation**

An attacker fixes a valid session identifier and tricks the victim into logging in with it, allowing for subsequent session hijacking.

#### 3. **Cross-Site Scripting (XSS)**

XSS attacks can compromise user sessions by injecting malicious scripts into web pages.

#### 4. **Cross-Site Request Forgery (CSRF)**

CSRF forces authenticated users to perform unintended actions on a web application by leveraging their identity through crafted requests.

#### 5. **Open Redirects**

An attacker can redirect victims to malicious sites by exploiting legitimate redirection functionalities without proper validation.

\
