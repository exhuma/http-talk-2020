# Media-Types (MIME-Types)

---

## What Problem Does it Solve?

* Identification of the nature of the payload (format and encoding)

---

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