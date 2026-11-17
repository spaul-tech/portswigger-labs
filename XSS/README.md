
# 💉 Cross-Site Scripting (XSS)

A practical collection of Cross-Site Scripting (XSS) concepts, vulnerability analysis, payloads, and hands-on lab solutions.

This repository documents my learning journey through XSS vulnerabilities using PortSwigger Web Security Academy and authorized testing environments.

## What is XSS?

Cross-Site Scripting (XSS) is a web security vulnerability that allows an attacker to inject malicious JavaScript into a web application. When another user visits the affected page, the injected script may execute in their browser.

Depending on the vulnerability, XSS can affect user sessions, modify page content, perform actions on behalf of users, or manipulate browser interactions.

## Types of XSS

### 1. 🪞 Reflected XSS
- User input is included in the server's response.
- The injected payload is not permanently stored.
- Usually delivered through a crafted URL or request.

### 2. 💾 Stored XSS
- Malicious input is stored on the server.
- The payload executes when another user views the affected content.
- Common injection points include comments, profiles, and messages.

### 3. 🌐 DOM-Based XSS
- Occurs when client-side JavaScript processes untrusted data in an unsafe way.
- Vulnerable sources may include `location.search` and `location.hash`.
- Vulnerable sinks may include `innerHTML` and `document.write()`.

## Important Concepts

| Concept | Description |
|---|---|
| HTML Injection | Injecting HTML elements into a page |
| JavaScript Context | Input placed inside JavaScript code |
| Attribute Injection | Breaking out of an HTML attribute |
| Event Handlers | JavaScript triggered by browser events |
| DOM Manipulation | Client-side modification of page content |
| Output Encoding | Encoding untrusted data before rendering |

## Tools & Platforms

- Burp Suite Community Edition
- Firefox Developer Tools
- PortSwigger Web Security Academy
- OWASP Juice Shop
- DVWA
- Kali Linux

## Practical Learning

My XSS practice focuses on:

- Identifying injection points.
- Inspecting HTTP requests and responses.
- Understanding HTML and JavaScript contexts.
- Testing reflected and stored XSS.
- Understanding DOM-based vulnerabilities.
- Using browser developer tools.
- Analyzing why payloads execute or fail.
- Learning XSS prevention techniques.

## Prevention

- Apply context-aware output encoding.
- Sanitize HTML when accepting user-generated HTML.
- Avoid unsafe DOM sinks such as `innerHTML`.
- Prefer `textContent` for inserting plain text.
- Implement Content Security Policy (CSP) as an additional security layer.
- Validate and handle untrusted input appropriately.

## Learning Resources

- [PortSwigger Web Security Academy - Cross-Site Scripting](https://portswigger.net/web-security/cross-site-scripting)
- [OWASP Cross-Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)

## Disclaimer

All testing and payload experimentation documented here are performed in authorized labs and intentionally vulnerable environments.

This repository is intended for educational and defensive cybersecurity learning.

---

**Focus:** Web Application Security | Application Security | XSS

