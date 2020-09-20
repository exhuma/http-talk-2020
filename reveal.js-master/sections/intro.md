# HTTP

HyperText Transfer Protocol

---

## What Problem Does it Solve?

Exchange documents between two parties.

---

## Background

* Started with "emails"
* Simple, extensible "body with envelope" structure.
* Widely supported and robust.

Note:

E-Mails and HTTP messages share the same structure. Core elements of HTTP
messages (like headers, mime-type, body, ...) still rely on these standards.

The main differences are in the headers. HTTP defines a large number of new
headers. Some mandatory e-mail headers (like `Subject` and `To`) are not
mandatory in HTTP as they don't make sense there.
