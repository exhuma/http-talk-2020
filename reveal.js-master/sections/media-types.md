# Media-Types

also known as MIME-Types

Note:

Media-types are an extremely powerful mechanism to ensure an API is both
future-proof and backwards-compatible. It also makes client/server
communication much more explicit and stable.

---

## What Problem Does it Solve?

* Identification of the nature of the payload (format and encoding)

Note:

Consider the document:

```
{"username": "jdoe", "email": "john.doe@example.com"}
```

and

```
{"hostname": "myhost.mydomain.tld", "ip": "192.0.2.1"}
```

Both are JSON. Custom media-types help to distinguish one from the other.




---

## How Does it Work?

* Plain-Text value to identify nature of payload
* major & minor type
* arguments

```
major/minor+json; arg1=value1; arg=value2
```

Note:

Both the `Accept` and `Content-Type` header use "Media-Types" (often also
called MIME-types). [The official list or registered MIME-Types][mime-types] is
managed by IANA.

[mime-types]: https://www.iana.org/assignments/media-types/media-types.xhtml

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
