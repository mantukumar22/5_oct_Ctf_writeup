# Buried in the Binary (Reverse Engineering — 100)

**Status:** Solved (confirmed on official scoreboard). Flag value not captured in this record.

## Challenge
A hosted "Personal Booklet" web page, mostly built from external CDN images,
was suspected to hide data inside the one image actually served locally.

## Steps followed
1. Fetched the page source directly and extracted every `src="..."` value:
   `curl -s https://<target>/ | grep -o -i -E 'src="[^"]+"'`
2. Identified that all but one image reference pointed to an external CDN
   (Unsplash); the single local reference was `images.jpeg`.
3. Downloaded the local image: `wget https://<target>/images.jpeg -O
   images.jpeg`.
4. Ran `file images.jpeg` to confirm it was a standard JPEG, then
   `exiftool images.jpeg` to inspect metadata fields.
5. Found a `User Comment` field containing `employee_photo.jpg` — a pointer
   to a second file on the server rather than the flag itself.
6. Began fetching `employee_photo.jpg` directly and probing likely
   directories (`images/`, `uploads/`, `static/`, `assets/`, `img/`,
   `files/`) with status-code checks to locate it, since the first path
   returned a 404.
7. Planned to repeat `exiftool -a -u -g1` and `strings -n 6 | grep -i flag`
   against the located file once retrieved, to recover the flag embedded in
   its metadata.

## Key takeaway
Not every clue sits in the first file you find — a metadata field can act as
a pointer to a second, differently-named file elsewhere on the same server.
Filtering local vs. CDN-hosted assets first saved time that would otherwise
go into picking apart decoy images.
