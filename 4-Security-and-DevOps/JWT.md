# JWTs
JSON Web Token (JWT) is an open standard  [RFC 7519](https://tools.ietf.org/html/rfc7519), that defines a compact and self-contained way for securely transmitting information between parties as a JSON object.

-   JWTs are used for authentication in (RESTful) microservices architecture.
-   REST is an acronym for REpresentational State Transfer. It is a software-architectural style, in which there is  **stateless**  communication between client and server.
-   **Stateless**  means that the server does not have to store the user cookies or other session data for any incoming request. Rather, the server authenticates a user based on a token (JWT).

## JWT Structure
An example of JSON Web Token is shown below. Can you notice the three parts (a header, payload, and signature) separated by a period (.)?

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJVZGFjaXR5X1Rlc3QiLCJleHAiOjE2MDgzNjYxNDJ9.0kjTgAOOJkNxM908qJE9p1503-mi-2tha9UVneI04EXvxte4KpKYl0pVMjXG-FLtDFzfmEuRRYyPm2JRGvlKFg
```

> The token above is a Base64 encoded form of the individual three parts.

These three parts together help to authenticate a user.

1.  **Header**  - In the example above, the  `eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9`  represents the header, such as:
    
    ```
    {
    "alg": "HS256",
    "typ": "JWT"
    }
    ```    
2.  **Payload**  - The  `eyJzdWIiOiJVZGFjaXR5X1Rlc3QiLCJleHAiOjE2MDgzNjYxNDJ9`  is the payload, also called a Claim. After Base64 decoding, you can get the actual user-data, such as:
    
    ```
    {
    "sub": "1234",
    "name": "Sareeta Panda",
    "admin": true
    }
    ```

**The user-data above is not necessarily secret because anyone who has access to the token can decode (Base64 decoding) the payload**. The payload tells us the username/ID (not password) of the user, i.e., who is making the request

3.  **Signature**  - The remaining third part is the signature, which helps the server to **verify the authenticity of the token** as well as the user. In fact, the signature is generated by using the header, payload, and a secret residing on the server only. If anyone (header, payload, and a secret) changes, then the signature will also change.

### How does it work?

![](resources/jwt-work-flow.png)

-   **Step 1 - Generate JWT**  - A user can attempt to log in from any client. The server returns a JSON Web Token (JWT) upon successful validation of the user credentials. This JWT is then stored locally in the client.
-   **Step 2 - Use JWT**  - Later, when the user requests to access any protected resource, a JWT is sent along. Then the server performs the JWT validation before granting access to the resource.

### What is statelessness?
JSON web tokens are intrinsically  _stateless_, meaning, the server does not store the user's session or cookies. When a JWT is sent back from a client to a server, the server only has to validate the token.

Statelessness also solves the problem of scalability. Let's say we have our API server that starts to have a tremendous amount of demand. In this case, we'll be spinning up multiple servers over the same service.

Now, a JWT could be hitting any one of these servers, and since it's stateless, each of those servers can be confident in the identity provided.

### Additional Resources

-   [JWT.io](https://jwt.io/introduction/)  a useful guide and list of popular JSON Web Token implementations.
-   [Base64 Encoding](https://en.wikipedia.org/wiki/Base64)
-   [HMAC](https://en.wikipedia.org/wiki/HMAC)  keyed-hash message authentication code