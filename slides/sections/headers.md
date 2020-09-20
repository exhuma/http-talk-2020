# Headers

---

## What Problem Does it Solve?

* Content Negotiation
* Authorisation
* Cache-Control
* ...

Note:

Headers are extensible and provide a storage area for any meta-data which does
not belong into the payload itself.

* Auditing & Access Control
* Monitoring, Tracing & Debugging
* ...

Headers are simple key/value pairs. Many keys are defined in
[RFC-7231][rfc-7231]. They are separated into [request headers][section-5] and
[response headers][section-7].

There is no restriction on adding personal custom headers. Historically custom
headers had to be prefixed by `X-`. This is no longer necessary [^1].

Headers have a max-length limit (998/78 characters). This can be circumvented by
[continuing the value on the next line][folding]. See [RFC-5322][line-lengths]
for details on these limits.


[rfc-7231]: https://tools.ietf.org/html/rfc7231
[section-5]: https://tools.ietf.org/html/rfc7231#section-5
[section-7]: https://tools.ietf.org/html/rfc7231#section-7
[folding]: https://tools.ietf.org/html/rfc5322#section-2.2.3
[line-lengths]: https://tools.ietf.org/html/rfc5322#section-2.1.1
[^1]: https://tools.ietf.org/html/rfc7231#section-8.3.1

---

## Key Headers for APIs

---

## `Accept` & `Content-Type`

Already covered in previous slides

---

## `Location`

Tells the client the URL of a newly created resource.

Note:

For `POST` requests, the response *should* contain a `Location` header. The
value should be the URL where the newly created instance is reachable.

---

## `Authorization`

* Identify *who* is making a request
* Different value types possible
* Use SSL/TLS when sending an `Authorization` header.

Note:

If the uses is logged in to a system, the log-in information is contained in
the `Authorization` header.

The simplest implementation is `Basic` which transfers the value in clear text.

Use [`JWT` (JSON Web Token)][jwt] when you want to embed additional information.

Example Headers:

```
Authorization: Basic am9obi5kb2VAZXhhbXBsZS5jb206NXVwM3I1M2NyMzcK
```

```
Authorization: JWT eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJlbWFpbCI6ImpvaG4uZG9lQGV4YW1wbGUuY29tIiwidXNlcl9pZCI6NDIsImRpc3BsYXlfbmFtZSI6IkpvaG4gRG9lIn0.EdyKAcSHYuT6HEhJfygosfBq9wxHcOM40r6CO8T1KfQ
```

[jwt]: https://jwt.io

---

## `User-Agent`

* Identify the application that made the request
* Values should be in the form `<name>/<version>`

Note:

Add the `User-Agent` header on each request you make to an API. Set it
yourself. If you don't set it yourself, you usually get the name of the
underlying HTTP library. This is not very helpful.

This helps in debugging. It allows you to filter logs by application.

Example custom value:

```
myapplication/1.0
```

You can also append the underlying HTTP library if you want to (separated by a
space):

```
myapplication/1.0 libwww/2.17b3
```

See [RFC-7231 Section 5.5.3](https://tools.ietf.org/html/rfc7231#section-5.5.3)

---

## Caching

Note:

See [RFC-7234 Section 5.2](https://tools.ietf.org/html/rfc7234#section-5.2)

---

## `ETag`

A textual value (often a hash) that changes if the resource changed

---

## `Expires`

A timestamp when the resource is no longer valid

---

## `Cache-Control`

Tells clients and caching proxies how to process a response

----

* `no-cache`: Don't use cached values for a response
* `no-store`: Never store something in the cache

See [RFC-7234 Section 5.2](https://tools.ietf.org/html/rfc7234#section-5.2)

---

## `Forwarded`

* Identify a chain of intermediate servers
* Each "hop" is appended to the value
* Exposes the "real" client IP on the server

See [RFC-7239](https://tools.ietf.org/html/rfc7239#section-4)

Note:

An alternative header for this is `X-Forwarded-For` which is still heavily used
today.

---

## Links

* Programatic cross-linking
* Provides links to related resources
* Relation type defined by [a "rel"][rels]

[rels]: https://www.iana.org/assignments/link-relations/link-relations.xhtml

Note:

Links take the following form:

```
Link: <http://example.com/resource>; rel="self"; title="This Resource"
```

For `rel` you have two options:

* A predefined `rel` by [IANA][rels]
* A URI for custom rels. *These URIs do not need to be reachable*.

[rels]: https://www.iana.org/assignments/link-relations/link-relations.xhtml
