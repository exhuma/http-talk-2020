# Anatomy of HTTP

---

Everything starts with the "Internet Messaging Format"
[RFC-5322][rfc-5322] (successor of RFC-822). This
RFC defines e-mail transport formatting, including many important rules like
Header syntax and Date-Time formatting.

This starting point for HTTP (which is based on RFC-5322) is
[RFC-7231](https://tools.ietf.org/html/rfc7231)

MDN has [a very good article][mdn-http-msgs] on HTTP messages.

---

## Message Parts

* Request Line/Status-Line
* Headers
* Empty Line
* Payload

---

## Sample Request

    GET /comments?postId=1 HTTP/2
    Host: jsonplaceholder.typicode.com
    User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

---

## Sample Response

    HTTP/2 200 
    date: Tue, 15 Sep 2020 05:38:11 GMT
    content-type: application/json; charset=utf-8
    etag: W/"5e6-4bSPS5tq8F8ZDeFJULWh6upjp7U"

    [{"postId": 1, ...]
