# T1055 — Thread Hijacking

Redirecting the execution flow of an existing thread in the target process instead of creating a new thread.

## Conceptual flow

```
OpenThread(target thread, THREAD_SUSPEND_RESUME | THREAD_GET_CONTEXT | THREAD_SET_CONTEXT)
  └─ SuspendThread
       └─ GetThreadContext          (capture RIP/RSP and registers)
            ├─ VirtualAllocEx + WriteProcessMemory (shellcode in the target)
            └─ SetThreadContext      (RIP = shellcode)
                 └─ ResumeThread     (thread resumes into the shellcode)
```

## Characteristics

| Aspect | Detail |
|---|---|
| Thread creation | None — reuses an existing thread |
| Thread selection | Threads of the target itself; prefer threads in a stable wait |
| Stability | Destabilizes the target if the context is restored incorrectly; the PoC must save/restore registers |
| Variants | Hijack with return to the original flow (context-restoring stub) vs. destructive hijack |

## Prerequisites

- A thread handle with the rights above (same context or `SeDebugPrivilege`)
- Watch the architecture (Wow64 changes the CONTEXT structure)

## Observable artifacts

- Sysmon: Event ID 10 (ProcessAccess/ThreadAccess) — no Event ID 8
- Thread with an anomalous call stack (current instruction outside a module)
- Thread suspend/resume by an external process

## Lab notes

- Target: a secondary dummy-process thread in `WaitForSingleObject`
- Validate with x64dbg attached to the target: step through the hijack instruction by instruction
