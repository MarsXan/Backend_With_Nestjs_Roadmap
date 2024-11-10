# Cookie-Based Authentication in NestJS

Cookie-based authentication is a classic method for maintaining user sessions in web applications. Here’s how it works and how you can implement it in a **NestJS** application:

## What is Cookie-Based Authentication?

In cookie-based authentication:
1. **User Logs In**: When a user logs in, the server authenticates the user.
2. **Session Creation**: The server creates a session, storing relevant user data.
3. **Session ID as a Cookie**: A unique session ID is generated and sent to the client as a cookie.
4. **Client Sends Cookie with Each Request**: The client includes this cookie with each subsequent request to allow the server to identify and authenticate the user.

### Pros and Cons

**Pros:**
- **Stateful**: Maintains session state on the server, which works well for traditional web apps.
- **Simple**: Supported by browsers out-of-the-box.

**Cons:**
- **CSRF Vulnerability**: Cookies are automatically sent with requests, making them susceptible to Cross-Site Request Forgery (CSRF).
- **Cross-Origin Issues**: Cookies require additional configurations for cross-origin requests (e.g., `SameSite` and `CORS` settings).
- **Not Ideal for SPAs/Mobile Apps**: Other approaches like JWT-based authentication may be better for single-page or mobile applications.
