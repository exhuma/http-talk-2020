# REST

Not every HTTP API is truly RESTful.

Note:

A REST API must adhere to very strict rules. Avoid misunderstandings by
announcing an API as a "HTTP API" rather than a "REST API".

Every REST API is also a HTTP API. But not the other way around.

---

## Stateless

* Client State is OK
* Server-SIde State is not OK (session-ids, many auth-tokens)
* More code pushed to the client
* Resource Updates more difficult

Note:

Being stateless is the most difficult restriction to solve for a HTTP API and
pushes a lot of responsibility on the client.

Ask yourself: "Does the server need some information from a previous request to
complete a request? For example: Session IDs?"

For authorisation, [JWT][jwt] can solve some of this by storing contextual
information inside of the authorisation token.

[jwt]: https://jwt.io

---

## Transactions

* Series of operations as one
* Incompatible with DB-Transactions
* *Tip* Use a "job"

Note:

Some REST (and HTTP) restrictions like statelessness and idempotency make it
impossible to span a transaction across multiple HTTP requests.

A moderately simple solution is to create a "job" resource. This can represent
a series of actions. A client can then simply *create* a job using a POST or
PUT request.
