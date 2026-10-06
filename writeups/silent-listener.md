# The Silent Listener (Network — 150)

**Flag:** `flag{http_exfiltration_detected}`

## Challenge
A simulated internal network where an exposed service was rumored to have
leaked a secret.

## Approach (overview)
1. Loaded the pcap; most traffic (11 hosts, 192.168.1.10–20) was decoy noise
   — routine DNS lookups and generic HTTP requests to the gateway.
2. Spotted one host (`192.168.1.100`) that appeared only once, at the very
   end of the capture, sending a `POST /upload` with
   `Content-Type: application/zip` to the gateway.
3. Extracted the raw HTTP body and confirmed it was a valid ZIP archive
   containing a password-protected file.
4. Cracked the ZIP password with a wordlist attack (`letmein`).
5. Decrypted the archive entry and read the flag.

## Key takeaway
In noisy captures, look for the host or packet that breaks the pattern
(appears only once, at an unusual time, or with an unusual content type) —
that's usually the signal among the decoys.
