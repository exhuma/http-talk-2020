# URIs & URLs

---

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

---

## Examples (from [RFC-3986][rfc3986]):

    ftp://ftp.is.co.za/rfc/rfc1808.txt
    http://www.ietf.org/rfc/rfc2396.txt
    ldap://[2001:db8::7]/c=GB?objectClass?one
    mailto:John.Doe@example.com
    news:comp.infosystems.www.servers.unix
    tel:+1-816-555-1212
    telnet://192.0.2.16:80/
    urn:oasis:names:specification:docbook:dtd:xml:4.1.2

---

## URL Elements & Their Uses

Example URL: http://www.example.com:1234/my;a=10/resource?foo=bar#hello

* `http://`
    * The "scheme" to use. Other examples can be `https`, `ftp`, `ssh`, ...

* `www.example.com`
    * The "authority" of the accessed resource (who manages/controls that
      resource)

* `:1234`
    * The port for the TCP connection

* `/my;a=10/resource`
    * The "path". This is an information for the server *where* to find the
      resource
    * Path segments are "opaque" to the protocol. But
      [reserved characters in the "sub-delims" group][uri-reserved]
      like `;` or `,` can be used by application-level code. See
      [URI-Path][uri-path]

* `?foo=bar`
    * The "query" string. This contains additional values which *should not be*
      used* to uniquely identify a resource. It should already have been
      defined by the previous URL parts.

* `#hello`
    * The "fragment" can be used to only request a section of a resource.

---

## Rules of Thumb

* Segments *should* represent a hierarchical relation of resources
* Use query-arguments only on URLs that return collections of things to
  *filter* that result
* More than one URL can target the same resouce, f. ex.:

    https://example.com/device/{device-id}/ports/{port-id}
    https://example.com/ports/{port-id}
