# src/image-tampering — Hollowing, Stomping and Mapping PoCs

📖 Research notes: [docs/image-tampering.md](../../docs/image-tampering.md)

## Planned experiments

| # | Experiment | Technique | Note |
|---|---|---|---|
| I1 | Process hollowing | Unmap + rewrite of the image in a suspended process | Host: a copy of `notepad.exe` in the lab |
| I2 | Module stomping | `.text` overwrite of a legitimate lab DLL | Compare: image-backed memory |
| I3 | Mapping injection | `NtCreateSection` + local/remote map | Alternative without `WriteProcessMemory` |

## Conventions

- Language: C (MSVC, x64)
- I1 is the most fragile PoC in the repository — validate section alignment and relocations of the injected image
- Payload: project-standard MessageBox (minimal PE image for I1)
