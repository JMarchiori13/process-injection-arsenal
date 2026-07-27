# src/remote-thread — Remote Thread Injection PoCs

📖 Research notes: [docs/remote-thread.md](../../docs/remote-thread.md)

## Planned experiments

| # | Experiment | API | Note |
|---|---|---|---|
| R1 | Full classic injection | `VirtualAllocEx` + `WriteProcessMemory` + `CreateRemoteThread` | Comparison baseline |
| R2 | Same flow via Native API | `NtAllocateVirtualMemory` + `NtWriteVirtualMemory` + `NtCreateThreadEx` | Compare telemetry with R1 |
| R3 | `RtlCreateUserThread` variant | Native | Document behavior differences |

## Conventions

- Language: C (MSVC, x64)
- Target: lab dummy process (see [lab/setup.md](../../lab/setup.md))
- Payload: project-standard MessageBox
- Verbose logging of every call (handle, allocated address, bytes written, thread ID)
