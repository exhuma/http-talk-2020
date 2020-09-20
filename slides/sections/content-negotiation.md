# Content Negotiation

Accept & Content-Type Headers

---

## What Problem Does it Solve?

* API Versioning & Backwards Compatibility
* Future Proofing
* Client Flexibility
* Removes ambiguity when everything is `application/json`

---

## How Does it Work?

* Client uses the `Accept` header
* Server responds according to `Accept` (or `406`)
* Client reads according to `Content-Type`

Note:

*All these steps are optional but highly recommended*

The `Accept` header is the client saying: "Hey server, please give me
something in this format". This can contain more than one value.

If the server *can* respond in this format it returns it. If not, it will
generate a `406 Not Acceptable` error response.

If the client specified more than one `Accept` value it *must* look at the
`Content-Type` header of the response to identify which one it received.

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
