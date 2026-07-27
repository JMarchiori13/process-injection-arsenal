# T1055 — Remote Thread Injection

The classic remote process injection technique. The comparison baseline for all others.

## Conceptual flow

```
OpenProcess(target, PROCESS_VM_OPERATION | PROCESS_VM_WRITE | PROCESS_CREATE_THREAD)
  └─ VirtualAllocEx(target, PAGE_EXECUTE_READWRITE)
       └─ WriteProcessMemory(target, shellcode)
            └─ CreateRemoteThread(target, entrypoint = shellcode)
```

## Documented variants

| Variant | Difference | Notes |
|---|---|---|
| `CreateRemoteThread` | Standard WinAPI | Most monitored; full chain visible in telemetry |
| `NtCreateThreadEx` | Native API | Same operation via `ntdll.dll`, bypasses user-mode hooks at the WinAPI layer |
| `RtlCreateUserThread` | Native, stable | Used internally by CSRSS; documented alternative |

## Prerequisites

- A handle with sufficient rights over the target process (same user and integrity level, or `SeDebugPrivilege` for targets in other contexts)
- Target and injector on the same architecture (x64 → x64)

## Observable artifacts

- Thread created with a start address outside any mapped module (image)
- RWX (or RW → RX) memory region not belonging to a module
- Sysmon: Event ID 8 (CreateRemoteThread), Event ID 10 (ProcessAccess)

## Lab notes

- Use the lab dummy process as the target
- Compare side by side: WinAPI vs. Native variant on the same snapshot
