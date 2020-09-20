# Multipart Messages

---

## What Problem Does it Solve?

* Exchange more than one resource
* Use it for "containers"
* Leverage existing standards

Note:

If a request returns a "list" of items, a multipart message can be helpful.
Each item can even be of a different type (heterogenuous lists). Slight
overhead due to the item separator.

Very interesting alternative to a custom schema for lists/containers. The main
benefit is that each item can have its own headers. This makes this return type
very explicit, expressive and flexible

---

## How Does it Work?

* Use `multipart/alternative` or `multipart/mixed`
* Add a "boundary" argument
* Separate items with `\n--{boundary}\n`
* End the document with `\n--{boundary}--\n`

Note:

You can either use `multipart/alternative` or `multipart/mixed`.

Use `multipart/alternative` if all items in the message contain the *same*
object but in different representations (f.ex.: plain-text, json and html in
the same HTTP message).

Use `multipart/mixed` if the HTTP message contains various distinct items.

Example:

```text [2,5,11,16]
Cache-Control: no-cache
Content-Type: multipart/mixed; boundary=myboundary
X-MyHeader: foobar
‌
--myboundary
Content-Type: text/plain
X-MyHeader: barbaz
ETag: daaafdfea7f638d975713a01e68fc3ea
‌
Hello World
--myboundary
Content-Type: application/json
ETag: 6e15b72f88e46fc0f28d175404030a66
‌
{"data": "another object"}
--myboundary--
```
