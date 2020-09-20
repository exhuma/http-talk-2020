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

The `Accept` header is the client saying: "*Hey server, please give me
something in this format*". This can contain more than one value, separated by
commas.

    text/html, text/plain, application/json

If the server *can* respond in this format it returns it. If not, it will
generate a `406 Not Acceptable` error response.

If the client specified more than one `Accept` value it *must* look at the
`Content-Type` header of the response to identify which one it received.

Looking at the `Content-Type` of a response is always a good idea. Even if you
only specified one media-type.

---

## `Accept` Header Precedence

* Set priorities on requested content-types using `q` ("quality")
* The `q` value ranges from 0.0 to 1.0
* Separate content types with commas

```
text/plain; q=0.5, application/json; q=1.0, */*; q=0.1
```

Note:


Breaking down the following example

```
text/plain; q=0.5, application/json; q=1.0, */*; q=0.1
```

Gives us the following precedence:

* First try: `application/json`
* Then try: `text/plain`
* Finally return anything possible

------

If a server can produce more than one media-type from the `Accept` header list
it needs to pick one. This can be influenced by the `quality` value. The
quality value looks like a normal media-type argument "q" but it **must come
last**! The value is a float between 0.0 (lowest priority) and 1.0 (highest
priority) The details of this are out of scope of this document.
