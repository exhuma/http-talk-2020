# Terminology <!--{{{-->

Resource
: Any "thing" that is processed by the back-end.
: A business object

URI
: Uniform Resource Identifier
: A textual string to identify a resource

URL
: Uniform Resource *Locator*
: A special form of a *URI* including a location of a resource.
: A textual string to identify *and find* a resource.

<!--}}}-->
# Common Mistakes<!--{{{-->

* Using `application/json` for everything

  * Makes it difficult to evolve APIs
  * Makes client/server dialog less expressive
  * Brittle
  * Consider using your own media-types including a format suffix and version:
    `major/minor+json; v=1.0`

* Not using status-codes correctly (or abusing them)

  * Each status-code has a *very well defined* meaning. They should be
    respected
  * Layers between client and server (web-server implementations,
    programming-frameworks, proxies, caches, load-balancers, API-managers,
    browsers) may misbehave if used incorrectly. This is more important than
    you may think.
  * Causes additional code to handle exceptions

* Not using time-zones in time-stamps

  * A timestamp without a time-zone is worthless
  * Even *if* it's UTC, *tell* the consumer by appending `+0000` or `Z` to the
    timestamp. Be explicit!
  * Use either Unix-timestamps or [ISO-8601][iso-8601] in a simple format.
    F.ex.: `2020-10-23T23:59:10.985+0200`


<!--}}}-->
# REST<!--{{{-->

Not every HTTP API is truly RESTful.

## Stateless<!--{{{-->

* Client State is OK
* Server-SIde State is not OK (session-ids, many auth-tokens)
  * *Question:* Does the server need some information from a previous request to
    complete a request? F. ex: Session IDs. See also [JWT][jwt]
* Some application logic for modifying resources pushed to client}}}

<!--}}}-->
# Anatomy of HTTP<!--{{{-->

Everything starts with the "Internet Messaging Format"
[RFC-5322][rfc-5322] (successor of RFC-822). This
RFC defines e-mail transport formatting, including many important rules like
Header syntax and Date-Time formatting.

This starting point for HTTP (which is based on RFC-5322) is
[RFC-7231](https://tools.ietf.org/html/rfc7231)

MDN has [a very good article][mdn-http-msgs] on HTTP messages.

## Message Parts

* Request Line/Status-Line
* Headers
* Empty Line
* Payload

## Sample Request

    GET /comments?postId=1 HTTP/2
    Host: jsonplaceholder.typicode.com
    User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

## Sample Response

    HTTP/2 200 
    date: Tue, 15 Sep 2020 05:38:11 GMT
    content-type: application/json; charset=utf-8
    etag: W/"5e6-4bSPS5tq8F8ZDeFJULWh6upjp7U"

    [{"postId": 1, ...]

<!--}}}-->
# Media-Types (MIME-Types)<!--{{{-->

## What Problem Does it Solve?

* Identification of the nature of the payload (format and encoding)

## How Does it Work?

Both the `Accept` and `Content-Type` header use "Media-Types" (often also
called MIME-types). [The official list or registered MIME-Types][mime-types] is
managed by IANA.

Media Types are extensible using the "personal" (prefix: `prs.`) and "vendor"
(prefix: `vnd.`) tree. The vendor tree *should* be registered with IANA, the
personal tree is "free-text".

Well-Known mime-types are:

* ``application/json``
* ``text/plain``
* ``text/html``

Example custom media-type:

    prs.lu.post.bb.object/generic-device+json; v=1.3

While the `+json` (or more general `+format`) suffix are not anchored in the
RFCs, it has become a de-facto standard.

## Media-Type Arguments

Media types can be followed by any number of arguments. The delimiter is a
semicolor `;`. **The argument `q` should be avoided as it has a special
meaning**. Example:

    prs.my/type; arg1=value1; arg2="another with a ; semicolon"; arg3=42

A media type should be considered identical if:

* The major and minor type are identical (case-insenitive)
* It has the same arguments with the same values (not depending on the
  orderging)


<!--}}}-->
# Content Negotiation (Accept & Content-Tpe)<!--{{{-->

## What Problem Does it Solve?

* API Versioning & Backwards Compatibility
* Future Proofing
* Client Flexibility
* Removes ambiguity when everything is `application/json`

## How Does it Work?

*All these steps are optional but highly recommended*

* The client sends a request *including an `Accept` header*.
* The server generates a response based on the `Accept` header.
* The client processes the response according to the `Content-Type` header of
  the response.

## Content-Type Header

The content-type header is simple. It contains exactly one media-type. It tells
the receiver of a HTTP message how it should handle the payload.

## Accept Header

The `Accept` header is a bit special. Like the `Content-Type` header it is
using media-types with a key difference: It can contain more than one. Multiple
media-types are simply separated by commas::

    text/html, text/plain, application/json

This header is only valid for requests.

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


<!--}}}-->
# Headers<!--{{{-->

## What Problem Does it Solve?

Headers are extensible and provide a storage area for any meta-data which does
not belong into the payload itself.

* Auditing & Access Control
* Caching
* Content Negotiation
* Monitoring, Tracing & Debugging
* ...

## Key Headers for APIs

* `Links` -- Offer cross-references to related documents
* `Accept` & `Content-Type` -- Used for content-negotiation
* `ETag`, `Expires`, `Cache-Control`, ... -- Used for various caching
  mechanisms
* `User-Agent` -- Identify the application that made the request
* `Authorization` -- Identify which user made the request
* `X-Forwarded-For` -- Identity a chain of intermediate servers processing the
  request (Proxies, API-Managers, ...)

<!--}}}-->
# URIs & URLs<!--{{{-->

## What Problem Does it Solve?

* Identification of things ("resources")
* Authority (who controls the resource)
* For URLs: localisation of resources (On which host the resource is available)

[URI-elements][uri-elements]
[uri-shemes](https://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml)

* HTTP uses URIs to identify resources
* An URL is a special form of an URI
* Every URL is a URI. Not every URI is an URL
* **ASCII encoded at protocol level** (See also [idna][idna],
  [punycode][punycode], [percent-encoding][pct-encoding])

## Examples (from [RFC-3986][rfc3986]):

    ftp://ftp.is.co.za/rfc/rfc1808.txt
    http://www.ietf.org/rfc/rfc2396.txt
    ldap://[2001:db8::7]/c=GB?objectClass?one
    mailto:John.Doe@example.com
    news:comp.infosystems.www.servers.unix
    tel:+1-816-555-1212
    telnet://192.0.2.16:80/
    urn:oasis:names:specification:docbook:dtd:xml:4.1.2

## URL Elements & Their Uses

Example URL: http://www.example.com:1234/my;a=10/resource?foo=bar#hello

`http://`
: The "scheme" to use. Other examples can be `https`, `ftp`, `ssh`, ...

`www.example.com`
: The "authority" of the accessed resource (who manages/controls that resource)

`:1234`
: The port for the TCP connection

`/my;a=10/resource`
: The "path". This is an information for the server *where* to find the resource
: Path segments are "opaque" to the protocol. But [reserved characters in the
  "sub-delims" group][uri-reserved] like `;` or `,` can be used by
  application-level code. See [URI-Path][uri-path]

`?foo=bar`
: The "query" string. This contains additional values which *should not be
  used* to uniquely identify a resource. It should already have been defined by
  the previous URL parts.

`#hello`
: The "fragment" can be used to only request a section of a resource.

## Rules of Thumb

* Segments *should* represent a hierarchical relation of resources
* Use query-arguments only on URLs that return collections of things to
  *filter* that result
* More than one URL can target the same resouce, f. ex.:

    https://example.com/device/{device-id}/ports/{port-id}
    https://example.com/ports/{port-id}

<!--}}}-->
# Multipart Messages <!--{{{-->

## What Problem Does it Solve?

* Sending more than one object (even of different types) in one message
* Removes the need to re-invent the wheel for new container types
* Each "part" benefits from all existing standards

## How Does it Work?

* The message uses the `multipart/alternative` or `multipart/mixed` media-type
* In the media-type caontains a "boundary" argument
* Each object is separated by the boundary string prefixed with `--`
* The final object is delimited with `--{boundary}--`

Example:

    Cache-Control: no-cache
    Content-Type: multipart/mixed; boundary=myboundary
    X-MyHeader: foobar

    --myboundary
    Content-Type: text/plain
    X-MyHeader: barbaz
    ETag: daaafdfea7f638d975713a01e68fc3ea

    Hello World
    --myboundary
    Content-Type: application/json
    ETag: 6e15b72f88e46fc0f28d175404030a66

    {"data": "another object"}
    --myboundary--


<!--}}}-->
# Authentication<!--{{{-->

## What Problem Does it Solve?

* Establishing that a user is who he/she says he/she is.

## How Does it Work?

* **Out of Scope of HTTP**
* Popular Mechanisms & Related Technologies:
  * [OAuth](https://oauth.net/)
  * [JWT](https://jwt.io)
  * [Basic Auth over SSL][rfc7617]

## JWT

* A user authenticates with the back-end (how, is not defined)
* The back-end generates a JWT token and signs it with a server-side secret
* The client uses this token in the `Authorization` header on subsequent
  requests.

### Advantages

* Tamper proof through cryptographic signature
* Payload is *transparent* and can be used to provide information to client
  applications (f.ex. Browser apps)
* Widely supported and easy to implement

<!--}}}-->
# Authorisation<!--{{{-->

## What Problem Does it Solve?

* Determines *who* is accessing the back-end

## How Does it Work?

* A user sends a request including an `Authorization` header.
* Processing and granting/refusing access is up to the application

<!--}}}-->
# Timestamps & Timezones<!--{{{-->

## What Problem Does it Solve?

* Determine exact points in time for certain events

## Important Notes

See also [RFC-5322 Section 3.3][rfc-5322-3.3]

* *Only values in headers are defined by the HTTP RFCs!*
* Timesamps in headers *must* include a Timezone offset from UTC
  * Use `+0000` if it is a timestamp in UTC and the system time is also in UTC
  * Use `-0000` if it is a timestamp in UTC and the system time is *not* in UTC
  * Use any other offset for localised timestamps
* Timestamps in message payloads are application dependent
* *Always* include time-zone information in application time-stamps

<!--}}}-->
# OpenAPI/Swagger<!--{{{-->

## What Problem Does it Solve?

* API Documentation
* Client Code Generation

## How Does it Work?

* The API provides a JSON file containing the OpenAPI specification.
* For easy publishing it can be provided directly by the API (f.ex.:
  `/swagger.json`
* Existing tools for humans:
  * [SwaggerUI][swaggerui]
  * [ReDoc][redoc]

<!--}}}-->
<!--{{{ links -->
[jwt]: https://jwt.io

[rfc3986]:      https://tools.ietf.org/html/rfc3986
[uris]:         https://tools.ietf.org/html/rfc3986
[uri-elements]: https://tools.ietf.org/html/rfc3986#section-3
[uri-path]:     https://tools.ietf.org/html/rfc3986#section-3.3
[pct-encoding]: https://tools.ietf.org/html/rfc3986#section-2.1
[uri-reserved]: https://tools.ietf.org/html/rfc3986#section-2.2

[idna]:    https://tools.ietf.org/html/rfc5890
[rfc5890]: https://tools.ietf.org/html/rfc5890

[punycode]: https://tools.ietf.org/html/rfc3492
[rfc3492]:  https://tools.ietf.org/html/rfc3492

[rfc7617]: https://tools.ietf.org/html/rfc7617

[rfc-5322]: https://tools.ietf.org/html/rfc5322
[rfc-5322-3.3]: https://tools.ietf.org/html/rfc5322#section-3.3

[multipart]: https://tools.ietf.org/html/rfc2046#section-5.1

[mdn-http-msgs]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages

[mime-types]: https://www.iana.org/assignments/media-types/media-types.xhtml

[iso-8601]: https://www.iso.org/iso-8601-date-and-time-format.html

[redoc]: https://github.com/Redocly/redoc

[swaggerui]: https://swagger.io/tools/swagger-ui/
<!--}}}-->

<!-- vim: set fdm=marker sw=4 ts=4 et foldlevel=0 : -->
