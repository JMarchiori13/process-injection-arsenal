# src/thread-hijack — Thread Hijacking PoCs

📖 Research notes: [docs/thread-hijacking.md](../../docs/thread-hijacking.md)

## Planned experiments

| # | Experiment | Variant | Note |
|---|---|---|---|
| T1 | Destructive hijack | `SuspendThread` → redirect RIP → `ResumeThread` | Baseline; thread does not return |
| T2 | Hijack with restoration | Stub saves registers, runs payload, restores context | Thread survives — compare stability |
| T3 | Thread selection | Enumerate target threads, selection heuristic (stable wait) | Document criteria |

## Conventions

- Language: C (MSVC, x64)
- Target: a secondary dummy thread in a stable wait
- Validate T2 with x64dbg: context before/after must be identical (except for payload execution)
- Payload: project-standard MessageBox
