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
- No authentication or session management - code does not verify the identity of the user making the request. Anyone can send a request to /update-role.
- No authorization checks - server accepts userId and newRole directly from the client and applies the change without verifying if the requester has the required privileges (e.g., is an admin).
- Trusting client-side data - assumes that the userId and newRole values sent from the client are legitimate and allowed

2. Briefly explain how a malicious attacker can exploit them.
- An attacker (e.g., a regular user) can send a POST request like this: 
({
  "userId": 2,
  "newRole": "admin"
}) 
- Because there's no check on the role or identity of the requester, this request would succeed and elevate the user to an admin role and here users gain access to higher-level permissions without authorization (Classic privilege escalation attack).

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
- Session-Based Authentication - Tracks users using a secure session token, preventing spoofing and allows the server to know which user is making the request by checking req.session.userId
- Authorization Check Before Updating Role - Only users with an admin role can change user roles.
- Reject Unauthorized Access - Ensures only logged-in users can interact with sensitive routes like /update-role.
