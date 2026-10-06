# lost flash drive (Forensics — 100)

**Status:** Solved (confirmed green on scoreboard).
**Flag:** `flag{its_adventure_time_yee_boi!!!}`

## Technique summary
File-carving / disk-image forensics. The provided ZIP contained a raw disk
image rather than a plain file. Identifying the image (`file`), then
inspecting or carving it with tools such as `binwalk` or filesystem tools
(`fls`, `icat`), surfaces a recoverable file containing stored credentials
with the flag embedded.

*Note: the exact command sequence and output weren't captured first-hand in
this record — this entry documents the flag and general method, not a
personal step log.*
