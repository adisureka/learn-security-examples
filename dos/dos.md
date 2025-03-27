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
- Lack of Rate Limiting- The server does not restrict how many requests a client can send. This makes it vulnerable to request flooding, where an attacker sends a high number of requests to exhaust system resources (CPU, memory, DB connections).
- Unbounded or Expensive DB Queries - The user input (req.query.id or req.query.username) is directly passed into database queries.If a malicious user sends a payload like an object with $regex or complex filters, it could trigger expensive queries and overburden the database.
- No Input Validation - Without strict input sanitization or schema validation, malformed or complex inputs can be sent repeatedly, stressing the application and potentially causing memory bloat or crashes. 


2. Briefly explain how a malicious attacker can exploit them.
- Request Flooding - The attacker can send thousands of requests per second to /userinfo, consuming CPU and memory, eventually causing the server to become unresponsive.
- Regex DoS (ReDoS): If the backend does not escape regex characters, then payloads can exploit catastrophic backtracking in MongoDB’s regex engine, freezing the process.
- Resource Starvation - Running many User.findOne() operations without limits can exhaust database connections and cause system slowdown or crash.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
- Rate Limiting - Limits clients to 1 request every 5 seconds and prevents brute-force attempts, bot flooding, and basic DoS attacks
- Input Validation and Sanitization - Blocks malformed or complex patterns like injection or regex payloads along with keeping queries light and safe
- Graceful Error Handling - The server avoids crashing by handling invalid inputs and query errors + sending clean, non-verbose messages that don’t help attackers probe vulnerabilities.
