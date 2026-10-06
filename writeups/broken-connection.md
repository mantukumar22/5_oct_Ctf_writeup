# The Broken Connection (Network — 150)

**Flag:** `flag{follow_the_evidence_2f629bed}`

## Challenge
Multiple evidence sources were provided: host inventory (`hosts.txt`),
`firewall.log`, `dns.log`, and a short pcap — no single file told the full
story.

## Approach (overview)
1. Cross-referenced `dns.log`, `firewall.log`, and `hosts.txt` to identify
   the workstation involved: `10.10.10.15` (workstation-07), the only host
   resolving a suspicious domain (`telemetry-sync.webupdate-cdn.net`).
2. Confirmed via `firewall.log` that the same host made two allowed
   connections to the resolved IP shortly after the DNS lookup.
3. Matched those two firewall entries to two TCP streams in the pcap (by
   source port and timing).
4. Followed the HTTP request chain: a `/api/update/check` response returned
   a ticket ID, which was then used in a second request
   (`/api/update/payload?rid=<ticket>`).
5. The second response contained a base64 blob; decoding it gave
   `<ticket>:flag{...}`, confirming the chain and revealing the flag.

## Key takeaway
When evidence is spread across logs and a pcap, timeline correlation
(timestamps, ports, hostnames, IDs) is the fastest way to connect them.
