## Workshop 1
### Introduction to WebGoat & Web Developer Tools
**WebGoat Lessons:** General → Developer Tools

---

#### What I Did

This workshop focused on getting familiar with the WebGoat environment and using browser developer tools to interact with a running web application.

After setting up the Kali Linux VM and launching WebGoat 2023.8, I navigated to the **General → Developer Tools** lesson. The lesson had two main tasks: using the browser console to call a JavaScript function, and using the Network tab to intercept a randomly generated number from an HTTP request.

**Task 1 – Using the Console**

I opened the browser developer tools (`Ctrl + Shift + I`) and navigated to the Console tab. From there, I called the function:

```
webgoat.customjs.phoneHome()
```

This returned a response in the console containing a randomly generated number, which I then submitted into the lesson's input field to complete the task.

**Task 2 – Using the Network Tab**

For this task, I clicked the "Go!" button in the lesson to generate an HTTP request, then switched to the Network tab in developer tools. I located the request to `start.mvc` and clicked on the **Request** tab to find the `networkNum` field embedded in the request parameters. I copied the number and submitted it to complete the lesson.

---

#### What I Learned

Developer tools aren't just for web developers — they're one of the first tools a security tester reaches for. Being able to read HTTP request details and interact with JavaScript functions directly in the browser is a foundational skill for web application testing. This lesson made it clear how much information is exposed through the browser if you know where to look.

---

#### OWASP Reference

- General security awareness / tooling fundamentals
