# Content Negotiation (Accept & Content-Tpe)

---

## What Problem Does it Solve?

* API Versioning & Backwards Compatibility
* Future Proofing
* Client Flexibility
* Removes ambiguity when everything is `application/json`

---

## How Does it Work?

*All these steps are optional but highly recommended*

* The client sends a request *including an `Accept` header*.
* The server generates a response based on the `Accept` header.
* The client processes the response according to the `Content-Type` header of
  the response.

---

## Content-Type Header

The content-type header is simple. It contains exactly one media-type. It tells
the receiver of a HTTP message how it should handle the payload.

---

## Accept Header

The `Accept` header is a bit special. Like the `Content-Type` header it is
using media-types with a key difference: It can contain more than one. Multiple
media-types are simply separated by commas::

    text/html, text/plain, application/json

This header is only valid for requests.

---

### `Accept` Header Precedence

If a server can produce more than one media-type from the `Accept` header list
it needs to pick one. This can be influenced by the `quality` value. The
quality value looks like a normal media-type argument "q" but it **must come
last**! The value is a float between 0.0 (lowest priority) and 1.0 (highest
priority) The details of this are out of scope of this document.

Example:

    text/html; q=0.1, text/plain; q=0.5, application/json; 1.0

If a server receives a request with this `Accept` value and supports both
`text/html` and `text/plain` it should return `text/plain`
