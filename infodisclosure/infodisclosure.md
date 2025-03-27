# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
- **NoSQL Injection Vulnerability**: The query in the route is constructed directly using user input without any proper validation or sanitization. This makes it vulnerable to NoSQL injection attacks. An attacker can manipulate the query to retrieve unauthorized information.

- **Lack of Access Control**: The application lacks access control for the `/userinfo` route, meaning any user can access user information if they provide a valid username.

- **Exposing Sensitive Information**: The application uses `console.log()` to print important information, such as the username. This could potentially expose sensitive information related to the system or users, which could be exploited by attackers.

1. Briefly explain how a malicious attacker can exploit them.

- **Exploiting NoSQL Injection Vulnerability**: An attacker can manipulate the `username` query parameter to inject malicious data into the MongoDB query. For example, the attacker can use a payload like `?username[$ne]=` to retrieve all user information from the database. This would expose the system to unauthorized data leakage.
- `/userinfo?username={"$ne":""}` This could disclose usernames and passwords, potentially leading to further attacks or unauthorized access.

- **Exploiting Lack of Access Control**: Since there is no access control in place for the `/userinfo` route, an attacker can craft requests with arbitrary usernames, potentially exposing sensitive user data. 

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
- **Input Validation and Sanitization**: 
  - First, it checks if the `username` is a valid string. If it’s not, the request is rejected with a 400 status and a message saying `Invalid username format`.
  - After validation, the `username` input is sanitized by removing any non-alphanumeric characters using a regular expression. This prevents injection of malicious characters or queries, mitigating the risk of NoSQL injection attacks.

    ```typescript
    const sanitizedUsername = username.replace(/[^\w\s]/gi, ''); // Removes non-alphanumeric characters
    ```