# The Midnight Request (Network — 150)

**Flag:** `flag{midnight_request_9bb5d42e}`

## Challenge
A finance workstation generated unusual outbound traffic at 02:13 AM while
unattended.

## Approach (overview)
1. Loaded the pcap and filtered out routine traffic (DNS, OCSP, CDN, standard
   ICMP padding).
2. Identified one suspicious conversation with a host impersonating a
   monitoring service (`edge.metricpulse-sync.net`), using a fake-sounding
   user agent (`MetricPulse-Agent/3.4`).
3. Found a registration POST (device metadata) followed by a correlation
   reference returned by the server.
4. Found a follow-up GET to `/api/v2/manifest?ref=<reference>` using the same
   correlation ID, returning a base64 manifest.
5. Base64-decoded the manifest: `<reference>:flag{...}` — the reference
   prefix confirmed the manifest belonged to the same session, and the flag
   followed the colon.

## Key takeaway
Correlate requests/responses by shared identifiers (correlation IDs, request
IDs) to follow a multi-step exchange even when it's spread across several
HTTP calls.
