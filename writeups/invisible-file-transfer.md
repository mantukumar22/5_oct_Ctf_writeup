# The Invisible File Transfer (Network — 150)

**Flag:** `flag{invisible_icmp_227673ac}`

## Challenge
A capture showed no obvious upload/download, but a confidential file was known
to have left the workstation.

## Approach (overview)
1. Loaded the pcap and separated normal ICMP ping padding from anomalous ICMP
   payloads — most of the capture was ordinary traffic.
2. Found a set of ICMP packets carrying heartbeat-style text (`hb,node=...,
   seq=...,chunk=...`). Reassembled the chunks in sequence order and decoded
   them from hex → a reference string (`REF-1EF0-25BD`).
3. Found two larger ICMP "diagnostic" packets carrying a base64 blob.
4. Base64-decoded the blob, then XORed it with the reference string (dashes
   stripped) as the repeating key.
5. The decoded output was the flag.

## Key takeaway
Data was tunneled over ICMP echo payloads disguised as heartbeat/diagnostic
traffic — a classic covert channel. Useful technique: when a derived key
doesn't work as-is, try stripping formatting characters (dashes, spaces)
before using it.
