# Authentication

---

## What Problem Does it Solve?

* Establishing that a user is who he/she says he/she is.

---

## How Does it Work?

* **Out of Scope of HTTP**
* Popular Mechanisms & Related Technologies:
  * [OAuth](https://oauth.net/)
  * [JWT](https://jwt.io)
  * [Basic Auth over SSL][rfc7617]

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
