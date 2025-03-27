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
Allow an attacker to pass mongose queries instead of sanitizing them
- NoSQL Injection Vulnerability
- Sensitive Data Exposure
- Verbose Error Messages

- If running on github then the attacker can run a query that is not supposed to be run
- no sanitized usernames


2. Briefly explain how a malicious attacker can exploit them.
- Inject NoSQL queries that can exploit your system
- Credential Theft via Data Exposure
- Give the details of any user (Logging or sending raw database errors in production can provide attackers with insight into database structure, names, or misconfigurations)


3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
- Input Validation and Type Checking (ensuring the username is a string)
- Input Sanitization to Prevent NoSQL Injection (no alphanumeric characters- prevents injection of MongoDB operators like $ne, $gt, etc., which are often used in NoSQL injection attacks.)
- Safe Query Execution- eliminates the risk of unintended behavior due to query manipulation
- Controlled Error Responses 

