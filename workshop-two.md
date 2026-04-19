## Workshop 2
### Sensitive Data Exposure
**WebGoat Lessons:** A7 – Identity & Auth Failure → Insecure Login

---

#### What I Did

This workshop demonstrated how plaintext credentials can be intercepted when a web application transmits data over HTTP instead of HTTPS.

**Task 1 – Using Developer Tools**

I opened the developer tools and navigated to the **Network** tab. After clicking "Log in" on the WebGoat Insecure Login lesson, I located the POST request to `start.mvc` and clicked on the **Request** tab. The username and password of another user were visible in plain text in the request body — no decryption required.

**Task 2 – Using Wireshark**

I launched Wireshark (pre-installed on Kali Linux) and selected the **Loopback: lo** interface, since WebGoat runs locally on `127.0.0.1`. I started a capture, then clicked "Log in" on the lesson page.

To filter the results and find the relevant packet, I applied the display filter:

```
http.request.method == POST
```

This narrowed the captured traffic down to the login POST request. Expanding the **Hypertext Transfer Protocol** section in the packet details revealed the username and password transmitted in plaintext in the request body.

I entered the intercepted credentials into the lesson fields to complete it.

---

#### What I Learned

This exercise showed why HTTP (without TLS) is fundamentally insecure for transmitting sensitive data. Even on a local loopback interface, credentials were fully visible to any tool capable of capturing network traffic. The fix is straightforward in concept — enforce HTTPS — but the lesson reinforced why this matters in practice. It also gave me hands-on experience with Wireshark's filtering system, which is useful for isolating relevant traffic in a busy capture.

---

#### OWASP Reference

- **A02:2021 – Cryptographic Failures** (formerly Sensitive Data Exposure)
