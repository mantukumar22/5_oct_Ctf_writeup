# The Forgotten USB (Forensics — 150)

**Status:** password reconstructed; flag retrieval pending final confirmation.

## Challenge
A near-empty USB disk image (`lost_flash_drive.img`, volume label `UNTITLED`)
was believed to hold an encrypted 7z backup.

## Approach (overview)
1. Examined the disk image and located a 7zAES-encrypted archive
   (`archive.7z`) — file names were readable, only contents were locked.
2. Extracted the password hash with `7z2john` for use with John the Ripper.
3. Recovered deleted files from the image by inode number (`icat`), including
   a personal reminder note and an "ops handbook" excerpt describing the
   company's archive-naming convention.
4. Combined the clues:
   - Project codename from the reminder note ("Helios migration backup").
   - Asset tag from the same note ("replacement laptop, asset IT-2048").
   - Fiscal quarter from context ("FY25 Q3").
   - Naming pattern from the ops handbook:
     `<ProjectCodename>_<OwnerAssetTag (no dash)>_<FiscalQuarter>`.
5. Built the candidate password `Helios_IT2048_Q3` and tested it with John
   against the extracted hash.
6. Planned to extract the archive with the recovered password and read
   `flag.txt`.

## Key takeaway
Forensic clues are often split across multiple deleted artifacts (a note +
a policy document); the password itself is rarely written down directly —
it has to be reconstructed from a documented naming convention.
