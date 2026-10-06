# unk (Forensics — 100)

**Status:** Solved (confirmed on official scoreboard).
**Flag:** `flag{old_macdonald_or_mcdonalds_supplier}`

## Technique summary
File-format forensics. The file `unk` has no extension but is actually a
ZIP archive (a `.docx` file's native format) — `file unk` reveals this.
Extracting it (`unzip unk`, handling the malformed local-header prompt by
accepting "All") exposes the standard Word document structure
(`word/`, `docProps/`, `_rels/`). The flag is recoverable from the document
thumbnail (`docProps/thumbnail.jpeg`) or by grepping the extracted XML for
`flag{`.

*Note: the exact command sequence and output weren't captured first-hand in
this record — this entry documents the flag and general method, not a
personal step log.*
