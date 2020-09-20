# Timestamps & Timezones

---

## What Problem Does it Solve?

* Determine exact points in time for certain events

---

## Key Points

* A timestamp without timezone is worthless
* Only header values are well-defined
* Body values are up to the application


Note:

See also [RFC-5322 Section 3.3][rfc-5322-3.3]

* *Only values in headers are defined by the HTTP RFCs!*
* Timestamps in message payloads are application dependent
* Timesamps in headers *must* include a Timezone offset from UTC
  * Use `+0000` if it is a timestamp in UTC and the system time is also in UTC
  * Use `-0000` if it is a timestamp in UTC and the system time is *not* in UTC
  * Use any other offset for localised timestamps

## Always use time-zones in message bodies

Even *if* a timestamp is sent in UTC, *tell* the consumer by appending `+0000`
or `Z` to the timestamp. Be explicit!

Use either Unix-timestamps or [ISO-8601][iso-8601] in a simple format.  F.ex.:
`2020-10-23T23:59:10.985+0200`

Note that ISO-8601 is notoriously flexible in how timestamps can be formatted.
If you use ISO-8601 also add the exact format you use in the documentation.
This will make it a lot easier for API consumers. And be consistent!

[rfc-5322-3.3]: https://tools.ietf.org/html/rfc5322#section-3.3
[iso-8601]: https://www.iso.org/iso-8601-date-and-time-format.html
