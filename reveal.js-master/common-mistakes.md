# Common Mistakes

---

## Mixing Intended Uses of URLs, Headers & Payloads

* URLs should *only* identify resources
* Payloads should *only* contain data *decribing* the resource
* Headers should *only* contain *meta-data* about the client/server dialog.
* Ideally, no verbs should be present in a URL. Consider using a resource
  representing a "job" or "action" instead.

---

## Incorrect usages of HTTP Verbs

* `GET` should *only* be used to retrieve something from the API
* `POST` should *only* be used to create *new* instances of resources on the
  server
* `PUT` should *only* be used to either create *new* or *update/replace* existing
  resources on the server.
* `DELETE` should *only* be used to delete a resource from the server.

---

## Using `application/json` for everything

* Makes it difficult to evolve APIs
* Makes client/server dialog less expressive
* Brittle
* Consider using your own media-types including a format suffix and version:
  `major/minor+json; v=1.0`

---

## Not using status-codes correctly (or abusing them)

* Each status-code has a *very well defined* meaning. They should be
  respected
* Layers between client and server (web-server implementations,
  programming-frameworks, proxies, caches, load-balancers, API-managers,
  browsers) may misbehave if used incorrectly. This is more important than
  you may think.
* Causes additional code to handle exceptions

---

## Not using time-zones in time-stamps

* A timestamp without a time-zone is worthless
* Even *if* it's UTC, *tell* the consumer by appending `+0000` or `Z` to the
  timestamp. Be explicit!
* Use either Unix-timestamps or [ISO-8601][iso-8601] in a simple format.
  F.ex.: `2020-10-23T23:59:10.985+0200`
