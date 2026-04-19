## Workshop 5
### Broken Authentication & Broken Access Control
**WebGoat Lessons:** A7 – Authentication Bypasses, A1 – Broken Access Control

---

#### What I Did

**Part 1 – Authentication Bypass (A7) using Burp Suite**

This task demonstrated how authentication can be bypassed by manipulating HTTP request parameters using a web proxy.

I started Burp Suite and opened the built-in browser, then navigated to the WebGoat Authentication Bypasses lesson (A7). I turned on **Intercept** in Burp Suite and clicked Submit on the security questions page.

Burp Suite intercepted the POST request to `auth-bypass/verify-account`. The body of the request contained:

```
secQuestion0=&secQuestion1=&jsEnabled=1&verifyMethod=SEC_QUESTIONS&userId=<myid>
```

I modified the parameter names from `secQuestion0` and `secQuestion1` to `secQuestion2` and `secQuestion3` — parameters the server didn't expect and didn't validate against:

```
secQuestion2=&secQuestion3=&jsEnabled=1&verifyMethod=SEC_QUESTIONS&userId=<myid>
```

Forwarding the modified request caused the server to accept it as valid, bypassing the security questions entirely.

**Part 1 (Alternative) – Authentication Bypass using OWASP ZAP**

I also completed the same task using OWASP ZAP. After installing ZAP and launching the browser through **Manual Explore** with the HUD enabled, I navigated to the lesson and used ZAP's break button to intercept the `verify-account` request. I made the same parameter modifications as above and clicked Continue to forward it, achieving the same bypass.

**Part 2 – Missing Function Level Access Control (A1)**

This lesson showed how hidden UI elements can still expose real backend endpoints.

I opened developer tools and inspected the page source, finding two hidden menu items: `Users` and `Config`. These weren't visible in the UI but their links were present in the HTML.

I then navigated directly to:

```
http://127.0.0.1:8080/WebGoat/access-control/users
```

In Burp Suite, I added the header `Content-Type: application/json` to the request. The server returned a JSON list of users including their usernames and password hashes, along with an `admin` flag. I identified the entry where `"admin": true` and copied that user's hash into the lesson input field to complete the task.

---

#### What I Learned

Both vulnerabilities in this workshop stem from the server trusting that the client will behave as expected. In the authentication bypass, the server failed to validate that the submitted parameter names actually corresponded to the security questions it set up. Renaming parameters was enough to circumvent the check entirely.

In the access control lesson, the application relied on hiding links in the UI as a security measure — a pattern known as "security through obscurity." But any authenticated user who knows (or guesses) the endpoint URL can access it directly, bypassing the UI entirely. Proper access control needs to be enforced on the server side, not just hidden on the client side.

---

#### OWASP Reference

- **A07:2021 – Identification and Authentication Failures**
- **A01:2021 – Broken Access Control**
