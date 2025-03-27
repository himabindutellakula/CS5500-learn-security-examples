# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
The spoofing vulnerability in `insecure.ts` arises due to improper cookie configurations. Specifically:

- **httpOnly is set to false** which allows JavaScript to access the session cookie, which is a security risk. If a malicious site runs a script on the user's browser, it could steal the session cookie and impersonate the user.
- **sameSite is not set** which makes it possible for cookies to be sent along with cross-site requests. This opens the door for **Cross-Site Request Forgery (CSRF)** attacks, where a user could unknowingly trigger an action on the server by visiting a malicious site while logged in.

2. Briefly explain different ways in which vulnerability can be exploited.
An attacker could exploit the vulnerabilities in the following ways:
- **Stealing Cookies**: If the user visits a malicious page (like `mal-steal-cookie.html`), JavaScript on the page can access the session cookie and steal it, since **httpOnly** is not set to `true`. The attacker can then use this stolen cookie to impersonate the user on the server.
- **Cross-Site Request Forgery (CSRF)**: If the user is logged in on `insecure.ts` and then visits a malicious site with a hidden form (like `mal-csrf.html`), the malicious site can automatically submit a request to the server. Since **sameSite** is not configured, the browser sends the user's session cookie with the malicious request, allowing the attacker to perform sensitive actions on behalf of the user without their knowledge or consent.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
In **secure.ts**, the spoofing vulnerability is mitigated by securing the session cookie with the following settings:
- **httpOnly: true**: This prevents JavaScript from accessing the session cookie, which stops attackers from stealing it through malicious scripts.
- **sameSite: true**: This setting ensures that the session cookie is only sent with same-origin requests. It blocks the cookies from being sent with cross-origin requests, preventing CSRF attacks.