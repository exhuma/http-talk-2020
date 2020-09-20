# Common Mistakes

---

## Breaking Intended Use: URLs

* Identify *Resources*
* Put metadata headers instead of body
* Don't put verbs in URLs

Note:

* URLs should *only* identify resources
* Payloads should *only* contain data *decribing* the resource
* Headers should *only* contain *meta-data* about the client/server dialog.
* Ideally, no verbs should be present in a URL. Consider using a resource
  representing a "job" or "action" instead.


---

## Breaking Intended Use: Methods

* Use `GET` to fetch a resource
* Use `POST` to create a new resource
* Use `PUT` to create/update a resource

Note:

# `POST` vs `PUT`

A `POST` request means: *"Create a new resource with the data enclosed in the
body and give me back the URL of this newly created instance"*. Use a
"container" URL  as target for a `POST` request. For example: `/users`. Don't
use an exact URL like `/users/1` for a POST request.

A `PUT` request means: *"Take the body of this request and store it at exactly
the location of the URL of the request"*. Use a `PUT` request if you want to
create a new resource and you already know the target URL. You can also use a
`PUT` request to "replace" an existing resource (thereby updating it).

*Tip:* Instead of putting verbs into URLs and accessing them via `GET`, use a
resource of type "job" or "action" and "create" that job using normal methods
on the server. This has additional useful side-effects. Jobs can have an ID,
They can be made asynchronous, ...

---

## Breaking Intended Use: Status-Codes

* Status Codes have *well-defined* meanings
* Incorrect status-codes can have unintended side-effects
* Most client-side errors will be `400 Bad Request`
* Don't use status 200 for sending errors!

Note:

Status codes are well-defined by the IETF and well documented in RFCs. Only the
status-codes in [RFC-7231 Section 6][status-codes] are guaranteed to be
implemnted. Any other status codes should be well documented on the API.

Don't invent new status codes (subject of the IETF).

Layers between client and server (web-server implementations,
programming-frameworks, proxies, caches, load-balancers, API-managers,
browsers) may misbehave if used incorrectly. This is more important than you
may think. For example, a caching proxy might decide to cache responses with
2xx status codes. If this contains an error-body, the error will be cached.

Misusing status-codes is unintuitive for API consumers and is likely to cause
bugs because of it.


[status-codes]: https://tools.ietf.org/html/rfc7231#section-6

---

## Missing Out: custom media-types

* Don't use `application/json` for everything
* Use custom media-types
* Same media type makes contracts brittle

Note:

Using the same media-type for everything makes it difficult to evolve APIs. If
you want (or need) to make backwards incompatible changes, you break clients.
Custom media-types provide a solution for this.

Using custom media-types makes the message exchange more developer friendly.

*Tip:* Use your own media-types (using the "prs" prefix) including a format
suffix and version. For example: `prs.major/minor+json; v=1.0`

---

## Missing Out: Not time-zones

* A timestamp without a time-zone is worthless
* UTC? Localtime? Different time-zone? DST? No DST?

Note:

Even *if* a timestamp is sent in UTC, *tell* the consumer by appending `+0000`
or `Z` to the timestamp. Be explicit!

Use either Unix-timestamps or [ISO-8601][iso-8601] in a simple format.  F.ex.:
`2020-10-23T23:59:10.985+0200`

Note that ISO-8601 is notoriously flexible in how timestamps can be formatted.
If you use ISO-8601 also add the exact format you use in the documentation.
This will make it a lot easier for API consumers. And be consistent!

[iso-8601]: https://www.iso.org/iso-8601-date-and-time-format.html
