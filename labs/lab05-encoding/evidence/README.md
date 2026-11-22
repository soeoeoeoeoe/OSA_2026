## Lab 5 — Encoding forensics

Pair: Jazylbekova Inzhu + my laptop 
Machine: ASUS TUF GAMING A17 
Tools: Excel, LibreOffice, Git Bash 
Date: 16.11.26

## What I received
### **Excel:** Opened the file using a legacy single-byte encoding, which turned Russian and Kazakh letters into unreadable garbled characters (mojibake).

**LibreOffice:** Displayed an import dialog that defaulted to a Western European character set, resulting in broken symbols instead of correct Cyrillic and Kazakh text.

**Terminal:** Showed raw single-byte characters as replacement diamonds () because Git Bash expected UTF-8.

## Diagnosis (made before the fix)
###**`file` Output:** `Non-ISO extended-ASCII text, with CRLF line terminators` (`evidence/file-before.txt`)

**First 16 Bytes:** `4944 3bd4 e0ec e8eb e8ff 3bc8 ecff 3bce` (`evidence/hexdump-before.txt`)

**Line Ending:** `CRLF` — confirmed by the first line terminating with the bytes `0d 0a` at offset `0x24`.

**BOM:** None (`no`) — the file begins directly with `49 44` (`"ID"`) rather than `ef bb bf`. In the repaired file, I prepended the UTF-8 BOM (`ef bb bf`) so that Windows Excel automatically detects UTF-8 encoding when the CSV is opened via double-clicking.

**Encoding:** `KZ-1048` — each Cyrillic and Kazakh character is encoded as a single byte (for instance, `"А"` is `c0`, `"Ә"` is `a3`, and `"і"` is `b3` in `Әбдірахманова`, whereas Windows-1251 maps `a3` to `Ј`).

**Separator:** `;` (semicolon) — individual fields are delimited by the single byte `3b`. 

## Verification
`evidence/file-after.txt` outputs `Unicode text, UTF-8 (with BOM) text, with CRLF line terminators`, verifying both the presence of the UTF-8 BOM (`ef bb bf`) at the beginning of the file and multi-byte UTF-8 sequences (`d0 a4`, `d0 b0`, etc.) representing the Cyrillic characters.

The screenshot `evidence/opens-correctly.png` demonstrates `staff-fixed.csv` loading properly across Excel, LibreOffice Calc, and Git Bash, with all Russian and Kazakh names (`Әбдірахманова`, `Қасымов`, `Нұрлан`) and semicolon-delimited columns preserved intact.

## What did not work the first time
Executing `iconv -f KZ1048` initially failed with a `conversion from KZ1048 unsupported` error until checking `iconv -l | grep -i 1048` revealed the required hyphenated encoding name, `KZ-1048`.

Running `cat << 'EOF' > scripts/fix.sh` initially returned `No such file or directory` because the `scripts/` directory did not exist yet and needed to be created first using `mkdir -p scripts`.

