# Multipart Messages

---

## What Problem Does it Solve?

* Sending more than one object (even of different types) in one message
* Removes the need to re-invent the wheel for new container types
* Each "part" benefits from all existing standards

---

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
