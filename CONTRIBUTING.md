# Contributing

Contributions are welcome **within the project scope**: documented process injection research, lab PoCs, and fixes to the technical notes.

## Rules

1. **Lab material only.** The project's standard payload is the harmless MessageBox — do not replace it with real payloads.
2. **Every PoC needs documentation.** Each implementation in `src/` must ship with a note in `docs/` explaining the technique, prerequisites, and expected behavior.
3. **No obfuscation/evasion.** PoCs are educational: verbose logging, no anti-analysis. EDR evasion is out of scope for this repository.
4. **Do not commit binaries.** The `.gitignore` already covers `.exe`, `.dll`, and compiled shellcode — respect it.

## Process

1. Open an issue describing the technique/module
2. Fork → branch `feat/<module>-<technique>`
3. PR referencing the issue, including the lab experiment result

## Standards

- PoC language: C (MSVC) — verbose logging at every step
- Notes in English (keep it consistent within each file)
- Techniques mapped to MITRE ATT&CK (technique + sub-technique)
- Tables for method comparisons
