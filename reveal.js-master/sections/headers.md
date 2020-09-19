# Headers

---

## What Problem Does it Solve?

Headers are extensible and provide a storage area for any meta-data which does
not belong into the payload itself.

* Auditing & Access Control
* Caching
* Content Negotiation
* Monitoring, Tracing & Debugging
* ...

---

## Key Headers for APIs

* `Links` -- Offer cross-references to related documents
* `Accept` & `Content-Type` -- Used for content-negotiation
* `ETag`, `Expires`, `Cache-Control`, ... -- Used for various caching
  mechanisms
* `User-Agent` -- Identify the application that made the request
* `Authorization` -- Identify which user made the request
* `X-Forwarded-For` -- Identity a chain of intermediate servers processing the
  request (Proxies, API-Managers, ...)
