# REST

Not every HTTP API is truly RESTful.

---

## Stateless

* Client State is OK
* Server-SIde State is not OK (session-ids, many auth-tokens)
  * *Question:* Does the server need some information from a previous request to
    complete a request? F. ex: Session IDs. See also [JWT][jwt]
* Some application logic for modifying resources pushed to client}}}
