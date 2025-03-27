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
- Weak Security Settings - "cookie: { httpOnly: false }" allows client-side JavaScript to read and manipulate the session cookie. 
- There's no user authentication or validation logic to securely assign or check "req.session.user" 
- An attacker could manually craft a request and spoof the session ("req.session.user = 'Admin';").

2. Briefly explain different ways in which vulnerability can be exploited.
- Manually Setting Session Cookie - An attacker could use tools like Postman or a browser extension to set a fake session cookie
- No Login Flow / Access Control - attacker doesn’t need to bypass a login mechanism; they just need to assign "req.session.user = 'Admin'" themselves in a request.
- Client-Side JavaScript Tampering

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
- "httpOnly: true" - As seen in the code of secure.ts 
"cookie: {
  httpOnly: true,
  sameSite: true
}"
- Secure Secret Passed via CLI - prevents exposing the secret in code — it must be passed securely at runtime (e.g., via env vars or CLI).
- Session-Based Identity Storage - only authenticated sessions will pass authorization checks.

