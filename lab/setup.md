# Lab Setup

Reference environment for every experiment in this repository. **No experiment should run outside this isolation.**

## Target VM (Windows 10/11)

| Item | Configuration |
|---|---|
| Network | Host-only or NIC disabled |
| Snapshots | `clean-base` (post-install) + one per hardening stage |
| Target processes | `notepad.exe`, `explorer.exe`, the lab's own dummy process (preferred target — avoids crashing system processes) |
| Tooling | MSVC (Build Tools), x64dbg, Process Hacker/System Informer, Process Monitor, Sysmon |

## Lab target process

For most PoCs, the target is a **lab-owned dummy process**: a minimal executable running in a loop. Injecting into a lab-owned process avoids instability and simplifies debugger analysis.

## Observation matrix

| Stage | Configuration | What to observe |
|---|---|---|
| 0 | Baseline | Technique functionality, Sysmon telemetry (Event IDs 1, 8, 10) |
| 1 | Defender ATP / lab EDR | Which steps trigger alerts (remote alloc, write, thread creation) |
| 2 | Protected target process (PPL) | Failures documented in `docs/` |

## Per-experiment procedure

1. Restore the appropriate snapshot
2. Build the PoC outside the VM, copy it into the lab
3. Run it with the dummy target process active, observing in the debugger/System Informer
4. Record the result + telemetry in the technique's doc
5. Restore the snapshot
