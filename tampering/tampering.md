# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   
The `insecure.ts` server is vulnerable to **Cross-Site Scripting (XSS)** attacks due to the following reasons:
It **does not sanitize user input** before storing it in the session (`req.session.user = req.body.name.trim()`). User input is directly inserted into the HTML response using `res.send`, making it possible for an attacker to inject malicious JavaScript code. The server assumes that user input will always be alphanumeric but does not enforce any restrictions.

2. Briefly explain how a malicious attacker can exploit them.

**Malicious Redirects:** Injecting `<a href="http://malicious.site">Click here</a>` could lead users to harmful websites. 
**Cross-Site Scripting (XSS):** An attacker can inject JavaScript code into the `name` field, which will execute when user visits the page. For example, submitting `<script>alert('Hacked!')</script>` would trigger an alert box.   
**Phishing Attacks:** An attacker could insert a fake login form to trick users into entering credentials.  

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

The **secure.ts** server mitigates the vulnerabilities by properly sanitizing user input before storing or displaying it. 

**Input Sanitization with `escapeHTML()`**
- The `escapeHTML()` function replaces special characters (`<`, `>`, `"`, `'`, `&`) with their HTML entity equivalents (`&lt;`, `&gt;`, `&quot;`, `&amp;`, `&#39;`), ensuring that user input is treated as plain text.
- This sanitization **prevents XSS** attacks by rendering injected HTML elements or JavaScript code as harmless text, rather than executing them.
