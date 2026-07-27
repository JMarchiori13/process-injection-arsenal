# src/apc — APC Injection PoCs

📖 Research notes: [docs/apc-injection.md](../../docs/apc-injection.md)

## Planned experiments

| # | Experiment | Target | Note |
|---|---|---|---|
| A1 | `QueueUserAPC` on an alertable thread | Dummy thread with `SleepEx(INFINITE, TRUE)` | Validate alertable-state dependency |
| A2 | Early bird APC | Process created suspended by the PoC itself | Guaranteed execution on resume |
| A3 | `NtQueueApcThread` | Same scenario as A1, via Native API | Compare with A1 |

## Conventions

- Language: C (MSVC, x64)
- Payload: project-standard MessageBox
- In A2, the "target process" is the lab dummy binary created with `CREATE_SUSPENDED`
