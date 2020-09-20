# Authentication

---

## What Problem Does it Solve?

* Establishing that a user is who he/she says he/she is.

---

## How Does it Work?

* *Out of Scope of HTTP*
* If the server cannot identiy the user, it response with `401 Unauthorized`

Note:

HTTP only defines how credentials are exchanged between client and server.
*How* the client received these credentials (for tokens) and *how* the server
validates those credentials is not defined in HTTP.

The `401` error can mean two things:

* The client did not *send* an `Authorization` header
* The client sent *incorrect* credentials (wrong password, ...)

---

## Popular Mechanisms & Related Technologies

* [OAuth](https://oauth.net/)
* [JWT](https://jwt.io)
* [Basic Auth over SSL](https://tools.ietf.org/html/rfc7617)

Note:

OAuth is a very popular mechanism to receive credentials.

JWT (JSON Web Token) is a very popular mechanism to exchange credentials
including additional data.

Basic Auth (over SSL) is the simplest form of authentication and is very useful
for debugging. For most authentication needs in production code I suggest using
JWT.

---

## JWT

* A user authenticates with the back-end (how, is not defined)
* The back-end generates a JWT token and signs it with a server-side secret
* The client uses this token in the `Authorization` header on subsequent
  requests.

---

### Advantages

* Tamper proof through cryptographic signature
* Payload is *transparent* and can be used to provide information to client
  applications (f.ex. Browser apps)
* Widely supported and easy to implement
