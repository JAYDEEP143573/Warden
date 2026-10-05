# Warden

this is my second rebo in github.

A browser-based file inspection tool that computes file hashes and runs heuristic red-flag checks — the same first-pass techniques real malware triage starts with, running entirely client-side.

**No backend. No uploads. Nothing leaves the browser.**

## What it does

Drop in any file and Warden will:

- **Hash it** — computes SHA-256 and SHA-1 using the browser's native Web Crypto API
- **Check it against a demo signature list** — matches the hash against a small known-hash list (includes the [EICAR test file](https://en.wikipedia.org/wiki/EICAR_test_file), the industry-standard harmless AV test signature)
- **Run heuristic checks** on the file's raw bytes and name:
  - Double file extensions (e.g. `invoice.pdf.exe`)
  - File-signature (magic bytes) vs. extension mismatches
  - Hidden executable headers embedded deeper in the file (polyglot/dropper indicator)
  - Office macro indicators (`vbaProject`, `macroEnabled`)
  - Byte-entropy analysis (flags likely packed/encrypted/compressed content)
  - Executable-class file extensions

Each check shows a pass / caution / flag result with a plain-language explanation, plus an overall risk badge.

## Why it's not a real antivirus — and says so

Warden is a learning and triage tool, not a replacement for real antivirus software. It's upfront about this in the UI itself:

- The hash list is a small demo set, not a real threat-intel database with millions of signatures
- Heuristics can only flag known *patterns* — they can't catch malware that doesn't match those patterns
- It has no way to detect behavior (what a file does when run), only static, at-rest characteristics

For real detection, pair this kind of static analysis with an actual antivirus engine or a threat-intel API such as [VirusTotal](https://www.virustotal.com/).

## Running it

No build step, no dependencies, no server required.

```bash
git clone <this-repo>
cd warden
open index.html   # or just double-click the file
```

Or serve it locally if you prefer:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Tech

- Vanilla HTML / CSS / JavaScript — single file, no framework, no build tooling
- [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) for hashing (`crypto.subtle.digest`)
- All file reading done via `FileReader` / `ArrayBuffer`, processed entirely in-memory

## Roadmap

- [ ] Optional VirusTotal API integration for real multi-engine verdicts (user-supplied API key)
- [ ] ZIP/Office container parsing for deeper macro and embedded-object inspection
- [ ] Expandable custom hash-list import (paste your own IOC list)

## Disclaimer

This project is for educational purposes — to demonstrate how file hashing and static heuristic analysis work. It is not a certified security product and should not be relied on as your only line of defense. Always keep a real, updated antivirus/EDR solution running, and when in doubt about a file, don't open it.

## License

MIT — see [LICENSE](LICENSE).
