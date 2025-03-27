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
- Missing Authentication & Logging - leads to message spamming, impersonation (since the user field is client-provided) and potential misuse without accountability

2. Briefly explain why the vulnerability is addressed in __secure.ts__.
- Middleware Logging - Records every incoming request with a timestamp and IP address which helps trace malicious actions and/or abuse.
- Authentication Enforcement - introduces checks or middleware to validate the user identity before allowing access which prevents unauthorized message posting and fake or spoofed usernames in messages

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
- Middleware Pattern which are functions which are layered units of logic that process HTTP requests before they reach route handlers.
- Logging Middleware - Captures request details before they are handled.
- Authentication Middleware (implied) - Checks user session/token before allowing message actions.
- This pattern promotes reusability and a seperation of concerns

