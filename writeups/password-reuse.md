# Password Reuse (Passwords — 100)

**Status:** Solved (confirmed on official scoreboard). Flag value not captured in this record.

## Challenge
An internal-backup scenario: an encrypted PDF export locked with "the
requesting employee's own directory credentials," alongside a set of
legacy account records and a current credential dump.

## Steps followed
1. Reviewed `old_accounts.txt`, listing 5 disabled legacy accounts
   (`kshah`, `rkapoor`, `amehta`, `sverma`, `dnair`).
2. Reviewed `credential_dump.txt`, containing MD5 hashes for all 7 current
   employees, and `users.txt` with the matching name/department records.
3. Cross-referenced the two account lists: the 5 usernames present in both
   `old_accounts.txt` and `credential_dump.txt` were the password-reuse
   candidates, since the other 2 (`pdesai`, `riyer`) had no legacy account
   to reuse a password from.
4. Extracted just the relevant hash lines into `hashes.txt` with
   `grep -E "^[a-z]+:" credential_dump.txt > hashes.txt`.
5. Cracked all MD5 hashes in one pass:
   `john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt
   hashes.txt`, then `john --show --format=raw-md5 hashes.txt` to list the
   recovered plaintexts.
6. Took the cracked passwords for the 5 reuse-candidate accounts and tested
   each against the locked backup PDF with
   `qpdf --password='<candidate>' --decrypt backup_archive.pdf output.pdf`
   (alternatively, `pdfcrack -f backup_archive.pdf -w candidates.txt` to
   automate the attempt across all candidates).
7. On a successful decrypt, read the recovered PDF (`pdftotext output.pdf -`)
   to retrieve the flag.

## Key takeaway
The scenario's real vulnerability was password reuse across a decommissioned
and a live system — cracking the hash dump wasn't the end goal, it was the
means to test which legacy password still unlocked a current resource.
