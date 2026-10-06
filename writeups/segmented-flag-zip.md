# segmented_flag_v2 (data.zip)

**Status:** Password cracked and confirmed in terminal output. Not matched
to a named challenge on the official scoreboard screenshots — verify the
platform challenge name before including this in a formal submission.

## Challenge
A ZIP archive (`data.zip`) containing an encrypted entry,
`segmented_flag_v2/flag.txt`.

## Steps followed
1. Extracted a crackable hash from the encrypted entry with:
   `zip2john data.zip > hash.txt`
   — confirmed the target was `segmented_flag_v2/flag.txt`
   (PKZIP-encrypted, `cmplen=49, decmplen=37`).
2. Attempted a wordlist crack with `john hash.txt --wordlist=rockyou.txt`,
   which failed initially because `rockyou.txt` wasn't present/extracted on
   the system.
3. Located and decompressed the real wordlist path:
   `sudo gunzip -k /usr/share/wordlists/rockyou.txt.gz`, then re-ran the
   crack against the correct path:
   `john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt`.
4. John recovered the password `mickeymouse` for
   `data.zip/segmented_flag_v2/flag.txt` (confirmed via `john --show
   hash.txt`).
5. Planned extraction with `unzip -P mickeymouse data.zip -d extracted`,
   followed by `cat extracted/segmented_flag_v2/flag.txt` to read the flag.

## Key takeaway
A straightforward wordlist attack, but the real friction was environment
setup — `zip2john`'s usage/flag syntax and locating the correct (gzipped)
rockyou wordlist path on Kali before the crack would even run.
