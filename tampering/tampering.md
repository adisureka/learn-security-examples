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
- Session Secret Injection via CLI Argument - The session secret is passed as a command-line argument, which can be accessed by other users on the same system using process inspection tools (ps, etc.).
- No User Authentication or Session Initialization - The session value req.session.user is never explicitly set through a secure login flow which means any client could potentially spoof or manipulate requests to set req.session.user = 'Admin'.
- Sensitive Data Stored in Session - If a user is identified as 'Admin', the server stores req.session.sensitive = 'supersecret' and if a session fixation or hijacking occurs, an attacker could access this data.

2. Briefly explain how a malicious attacker can exploit them.
- Command Line Session Secret Leakage - An attacker on the same machine could run: "ps aux | grep node" to discover the session secret and forge session cookies.
- Bypassing Authorization Checks - Without secure authentication, an attacker could craft a session manually with user = 'Admin' and/or gain access to privileged operations/data
- Session Fixation - Without regenerating the session on privilege changes (e.g., login), an attacker might trick a victim into using a pre-set session ID.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
- Proper Login Route - includes an authenticated login route that sets req.session.user only after validating credentials, preventing spoofing.
- Session Regeneration on Login - prevents session fixation, a secure implementation regenerates session IDs upon login.
- Access Controls Enforced in Routes - sensitive route checks both identity and role securely using session values validated through a secure flow.
