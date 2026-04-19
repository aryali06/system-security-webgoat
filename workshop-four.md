## Workshop 4
### Vulnerable Components & Cross-Site Scripting (XSS)
**WebGoat Lessons:** A6 – Vuln & Outdated Components, A3 – Cross-Site Scripting

---

#### What I Did

**Part 1 – Vulnerable Components (A6)**

This lesson focused on the risks of using outdated third-party libraries. The exercise used two versions of the `jquery-ui` library to demonstrate the difference a patch can make.

In the first example using `jquery-ui:1.10.4`, clicking "Go!" triggered a pop-up dialog that executed an XSS payload — demonstrating that the library had a known exploitable flaw. In the second example using `jquery-ui:1.12.0`, the same code no longer triggered the alert, because the vulnerability had been patched in the newer version.

> Note: On Kali Linux 2025, the pop-up may not render due to compatibility issues. The following can be entered in the browser console to replicate the behaviour manually:
> ```javascript
> webgoat.customjs.jqueryVuln = webgoat.customjs.jquery;
> webgoat.customjs.jquery('#dialog').html('XSS').dialog();
> ```

**Part 2 – Cross-Site Scripting (A3)**

**Task 2 – Cookie Theft via XSS**

I opened two tabs in the browser and used the browser console to run:

```javascript
alert("XSS-Test");
alert(document.cookie);
```

This revealed that the same session cookie was accessible from both tabs, showing how an XSS payload could steal session tokens and hijack an authenticated user's session.

**Task 7 – Reflected XSS**

In the shopping cart checkout page, I found that the credit card number field lacked input validation. I injected the following script into the field:

```html
alert("XSS")
```

Clicking "Purchase" caused the browser to execute the script — confirming a Reflected XSS vulnerability. In a real attack, an attacker could replace the benign alert with a script that exfiltrates cookies or redirects the user to a malicious site, then share the crafted URL with victims.

---

#### What I Learned

These two lessons connect closely. Vulnerable third-party components (A6) can introduce XSS vulnerabilities (A3) into an application even if the developer's own code is clean. The `jquery-ui` example was a good reminder that dependency management is a security concern, not just a maintenance one. For XSS, the key takeaway is that any user input that gets reflected back into a web page without sanitisation is a potential injection point — the fix is input validation on the server side and output encoding when rendering content.

---

#### OWASP Reference

- **A06:2021 – Vulnerable and Outdated Components**
- **A03:2021 – Injection** (XSS)
