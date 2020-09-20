# Timestamps & Timezones

---

## What Problem Does it Solve?

* Determine exact points in time for certain events

---

## Important Notes

See also [RFC-5322 Section 3.3][rfc-5322-3.3]

* *Only values in headers are defined by the HTTP RFCs!*
* Timesamps in headers *must* include a Timezone offset from UTC
  * Use `+0000` if it is a timestamp in UTC and the system time is also in UTC
  * Use `-0000` if it is a timestamp in UTC and the system time is *not* in UTC
  * Use any other offset for localised timestamps
* Timestamps in message payloads are application dependent
* *Always* include time-zone information in application time-stamps

Note:

Hello World

# Title 1

## Title 2


[rfc-5322-3.3]: https://tools.ietf.org/html/rfc5322#section-3.3
