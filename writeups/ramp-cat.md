# Ramp Cat (Forensics — 100)

**Status:** Solved (confirmed on official scoreboard).
**Flag:** `flag{Koneko}`

## Technique summary
EXIF / geolocation forensics. The provided image carries GPS coordinates in
its EXIF metadata (`exiftool IMG_0743.png`). Converting the GPS
latitude/longitude to decimal degrees and looking up the location
identifies a specific point of interest at that address (rather than the
address itself) — the flag names that location.

*Note: the exact command sequence and output weren't captured first-hand in
this record — this entry documents the flag and general method, not a
personal step log.*
