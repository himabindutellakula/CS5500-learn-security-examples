# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.

In **insecure.ts**, there are several potential vulnerabilities that can lead to a Denial of Service (DoS) attack:
-  If the `id` parameter provided in the query is **not a valid MongoDB ObjectId**, it will cause the application to crash because MongoDB will not recognize the input as a valid ID. This could lead to unhandled exceptions or server crashes.
- The **lack of try-catch blocks** around the database queries means that any database error or invalid input can cause the server to crash, which can lead to a DoS.
- The **absence of rate limiting** allows attackers to flood the server with requests, leading to performance degradation or even a DoS attack if the server resources become exhausted.
- The application **doesn't sanitize the user input** (`id`), which increases the risk of NoSQL injection attacks and can result in unexpected behavior or resource exhaustion.

2. Briefly explain how a malicious attacker can exploit them.

A malicious attacker can exploit the vulnerabilities in **insecure.ts** in several ways:
- By sending an invalid `id` value that is not a valid MongoDB ObjectId, MongoDB may fail to process the query, potentially causing the server to crash or hang. Repeated requests with invalid IDs can cause a DoS.
- By sending a large number of requests in a short amount of time, overwhelming the server and causing it to become slow or unresponsive, leading to a DoS.
- By sending invalid requests or causing database errors, an attacker can trigger unhandled exceptions and crash the server due to the lack of proper error handling mechanisms (try-catch blocks).

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

In **secure.ts**, several defensive techniques are used to mitigate the DoS vulnerability:
- **Rate Limiting**: The `express-rate-limit` is used to limit the number of requests from a single IP address within a defined time window (5 seconds). This prevents an attacker from flooding the server with requests and helps protect against DoS attacks.
- **Error Handling**: The use of a try-catch block around the database query ensures that any errors are caught and handled gracefully. If an error occurs, the server responds with a generic error message, preventing the server from crashing and ensuring the service remains available.
- **Input Sanitization**: While **insecure.ts** does not explicitly sanitize the user input, in **secure.ts**, sanitizing the `id` parameter before performing database queries would be an important next step to avoid potential injection attacks and unexpected database behavior.
