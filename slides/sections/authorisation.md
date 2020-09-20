# Authorisation

---

## What Problem Does it Solve?

* Determine if a client is allowed to perform an action

---

## How Does it Work?

* A user sends a request including an `Authorization` header.
* Processing and granting/refusing access *is up to the application*
* If access is refused respond with a `403 Forbidden` response.

Note:

HTTP only defines how credentials are exchanged between client and server.
What the server does with these credentials to allow or refuse access is not
defined.
