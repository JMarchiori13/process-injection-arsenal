# T1055.012 — Image Tampering: Hollowing, Stomping and Mapping

A family of techniques that manipulate the process **image** — replacing, corrupting, or mapping code in place of legitimate modules.

## Process Hollowing

```
CreateProcess(legitimate, CREATE_SUSPENDED)
  └─ NtUnmapViewOfSection (unmap the original image)
       └─ VirtualAllocEx (at the original base)
            └─ WriteProcessMemory (headers + sections of the new image)
                 └─ SetThreadContext (new image entrypoint)
                      └─ ResumeThread
```

Result: a process with the name/path of a legitimate binary executing a completely different image.

## Module Stomping (Module Overloading)

- Load (or reuse) a legitimate DLL in the target process
- Overwrite the module's `.text` section with shellcode
- Execute via a thread at the "stomped" module address
- Advantage: code runs from memory **backed by an on-disk image** — evades "executable private memory" heuristics

## Section Mapping Injection

```
NtCreateSection (PAGE_EXECUTE_READWRITE)
  ├─ NtMapViewOfSection (into the local process — write shellcode)
  └─ NtMapViewOfSection (into the remote process — executable)
       └─ CreateRemoteThread / hijack / APC on the remote view
```

An alternative to VirtualAllocEx + WriteProcessMemory: a single section shared between both processes.

## Comparison

| Technique | On-disk image? | Creates thread? | Complexity |
|---|---|---|---|
| Hollowing | Yes (legitimate name) | No (thread of the created process) | High |
| Module stomping | Yes (legitimate module) | Depends on the trigger | Medium |
| Mapping injection | No (non-backed section) | Depends on the trigger | Medium |

## Observable artifacts

- Hollowing: process image ≠ on-disk content (hash comparison), PEB vs. memory divergence
- Stomping: signed module with modified `.text` (in-memory signature verification failure)
- Mapping: executable sections shared between distinct processes

## Lab notes

- Hollowing: use the lab's own legitimate binary as the "host" (e.g., a copy of `notepad.exe`)
- Stomping: a lab-owned DLL as the overwrite victim
