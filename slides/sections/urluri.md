# URIs & URLs

---

## What Problem Does it Solve?

* Identification of things ("resources")
* Authority (who controls the resource)
* For URLs: localisation of resources (On which host the resource is available)

Note:

**HTTP uses URIs to identify resources**

URLs are URIs. A URI is a "Uniform Resource Identfitier". See
[URI-elements][uri-elements].

Schemes like `http` and `ftp` are
[well-defined](https://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml)
by IANA.

URIs (and URLs) are ASCII encoded at protocol level. For non-ASCII chracater
an appropriate encoding is needed. Commonly [idna][idna] defines
[punycode][punycode] for domain-names. For many other cases
[percent-encoding][pct-encoding] is used.

More than one URL can target the same resouce, f. ex.:

    https://example.com/device/{device-id}/ports/{port-id}
    https://example.com/ports/{port-id}

[uri-elements]: https://tools.ietf.org/html/rfc3986#section-3
[idna]:    https://tools.ietf.org/html/rfc5890
[punycode]: https://tools.ietf.org/html/rfc3492
[pct-encoding]: https://tools.ietf.org/html/rfc3986#section-2.1

---

## Examples (from [RFC-3986](https://tools.ietf.org/html/rfc3986)):

```text
ftp://ftp.is.co.za/rfc/rfc1808.txt
http://www.ietf.org/rfc/rfc2396.txt
ldap://[2001:db8::7]/c=GB?objectClass?one
mailto:John.Doe@example.com
news:comp.infosystems.www.servers.unix
tel:+1-816-555-1212
telnet://192.0.2.16:80/
urn:oasis:names:specification:docbook:dtd:xml:4.1.2
```

---

## URL Elements & Their Uses

Example URL:

```text
http://www.example.com:1234/my;a=10/resource?foo=bar#hello
```

Note:

* `http://`  
   The "scheme" to use.

* `www.example.com`  
  The "authority" of the accessed resource (who manages/controls that resource)

* `:1234`  
  The port for the TCP connection

* `/my;a=10/resource`  
  The "path". This is an information for the server *where* to find the
  resource.

  Path segments are "opaque" to the protocol. But
  [reserved characters in the "sub-delims" group][uri-reserved]
  like `;` or `,` can be used by application-level code. See
  [URI-Path][uri-path]

* `?foo=bar`  
  The "query" string contains additional values which *should not be* used to
  uniquely identify a resource. It should already have been defined by the
  previous URL parts. It should be mainly used to filter query results (lists).

* `#hello`  
  The "fragment" can be used to only request a section of a resource.


[uri-reserved]: https://tools.ietf.org/html/rfc3986#section-2.2
[uri-path]:     https://tools.ietf.org/html/rfc3986#section-3.3

---

## Rules of Thumb

* Hierarchical relation
* Use query-arguments to *filter* collections/lists
* Be consistent with `-` and `_`
* Don't use "verbs" in URLs

Note:

Segments *should* represent a hierarchical relation of resources.

Use query-arguments only on URLs that return collections of things to *filter*
that result.

Prefer using hypens (`-`) in all parts of URLS. Domain-Names disallow
underscores so using hyphens everywhere improves consistency.

If a URL contains a verb (or active speech) it hints to the use of a "job" or
"action" on the server. These don't play well with the semantics of existing
HTTP verbs. Avoiding verbs in URLs future-proofs the API.
