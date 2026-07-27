# T1055 — APC Injection

Injection via Asynchronous Procedure Calls: queueing code for execution in the context of an existing thread of the target process.

## Concept

Every Windows thread has an APC queue. A queued APC executes when the thread enters an **alertable state** (e.g., `SleepEx`, `WaitForSingleObjectEx` with `bAlertable=TRUE`). Injection abuses this to run code without creating a new thread.

## Documented variants

| Variant | Target | Notes |
|---|---|---|
| `QueueUserAPC` | Existing thread in an alertable state | Requires an alertable thread in the target; less reliable |
| Early Bird APC | Thread of a newly created (suspended) process | Creates a suspended process, injects, queues the APC before the thread starts — guaranteed execution on resume |
| `NtQueueApcThread` | Native API | Same operation via ntdll, bypasses WinAPI hooks |

## Early bird flow

```
CreateProcess(target, CREATE_SUSPENDED)
  └─ VirtualAllocEx + WriteProcessMemory (shellcode)
       └─ QueueUserAPC(shellcode, main thread)
            └─ ResumeThread → APC executes at thread start
```

## Prerequisites

- For APC into an existing process: an alertable thread (rare in GUI processes; common in processes with waits)
- For early bird: no special requirement — you control process creation

## Observable artifacts

- Absence of Event ID 8 (no CreateRemoteThread)
- RWX/RX non-module memory region in the target process
- Early bird: process created suspended + remote write before the first resume

## Lab notes

- Standard payload (MessageBox) — for APC into an existing thread, use a dummy process thread with alertable `SleepEx`
