# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

The main issue with `insecure.ts` is that it has **Weak or Missing Authorization**. It simply checks if the user’s role is 'admin' based on the user’s input, which is risky. There’s no real authentication or session management to make sure the user is who they say they are and has the right permissions to update roles.

2. Briefly explain how a malicious attacker can exploit them.

**Spoofing** - An attacker could easily exploit this vulnerability by sending a request to update a user’s role. They could pretend to be an admin (by altering the request body) and change their own role or someone else’s, even if they don't have the necessary permissions. The lack of proper checks means anyone can spoof a role change.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

- **Validating User Roles** - Before updating a user's role, the app makes sure that the person logged in actually has admin privileges. This ensures that only authorized admins can make changes to user roles, preventing unauthorized privilege escalation.
- In `secure.ts`, The session cookie is also secured with `httpOnly` and `sameSite` attributes.
- But, In `secure.ts`, the session’s secret key is hardcoded which means **secret is not secure,** which is a big security risk. Ideally, it should be stored in an environment variable or a secret manager, so it's not exposed in the codebase and can’t be accessed by unauthorized users.
   