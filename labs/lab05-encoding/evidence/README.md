## Lab 5 — Encoding forensics

Pair: Jazylbekova Inzhu + my laptop 
Machine: ASUS TUF GAMING A17 
Tools: Excel, LibreOffice, Git Bash 
Date: 16.11.26

## What I received
### **Excel:** Opened the file using a legacy single-byte encoding, which turned Russian and Kazakh letters into unreadable garbled characters (mojibake).

**LibreOffice:** Displayed an import dialog that defaulted to a Western European character set, resulting in broken symbols instead of correct Cyrillic and Kazakh text.

**Terminal:** Showed raw single-byte characters as replacement diamonds () because Git Bash expected UTF-8.
