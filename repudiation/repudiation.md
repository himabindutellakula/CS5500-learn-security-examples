# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.

The vulnerability in the **insecure.ts** lies in the **lack of logging** for the request and response. There is no way to detect if things go wrong or trace actions performed by users. Without logging, it becomes difficult to audit actions, detect suspicious activities, or troubleshoot errors effectively. This can lead to issues such as inability to diagnose problems, track malicious actions, or detect errors promptly.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.

In the **secure.ts**, the vulnerability is addressed by implementing a middleware for logging every request. This middleware logs essential details such as the request method, URL, timestamp, and IP address. Additionally, it logs important actions like sending messages and retrieving messages. The log entries are written to a `server.log` file, which helps in detecting issues, auditing actions, and identifying potential security breaches. This ensures that the system has traceability and can handle incidents more effectively.

However, file I/O is a costly operation and can lead to performance issues, particularly under high traffic or when writing to a file synchronously. In this case, if the logging process takes too long or fails, it can result in Denial of Service (DoS) issues. To avoid this, it is recommended to either optimize file I/O operations or handle it asynchronously.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

The **Middleware Pattern** is used in the secure version to address the vulnerability. In Express.js, middleware is a function that executes during the lifecycle of a request to the server. The middleware in `secure.ts` intercepts every incoming request and logs important details about it (e.g., method, URL, timestamp, IP address). This design pattern allows the application to log and handle requests in a centralized manner without cluttering the business logic in individual route handlers. 

 