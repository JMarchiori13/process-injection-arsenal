# T1055 — Thread Hijacking

Redirecionar o fluxo de execução de uma thread existente do processo alvo, em vez de criar thread nova.

## Fluxo conceitual

```
OpenThread(thread do alvo, THREAD_SUSPEND_RESUME | THREAD_GET_CONTEXT | THREAD_SET_CONTEXT)
  └─ SuspendThread
       └─ GetThreadContext          (captura RIP/RSP e registradores)
            ├─ VirtualAllocEx + WriteProcessMemory (shellcode no alvo)
            └─ SetThreadContext      (RIP = shellcode)
                 └─ ResumeThread     (thread retoma no shellcode)
```

## Características

| Aspecto | Detalhe |
|---|---|
| Criação de thread | Não — reutiliza thread existente |
| Escolha da thread | Threads do próprio alvo; preferir threads em wait estável |
| Estabilidade | Fragiliza o alvo se o contexto for restaurado incorretamente; PoC deve salvar/restaurar registradores |
| Variantes | Hijack com retorno ao fluxo original (stub que restaura contexto) vs. hijack destrutivo |

## Pré-requisitos

- Handle de thread com os direitos acima (mesmo contexto ou `SeDebugPrivilege`)
- Cuidado com arquitetura (Wow64 muda a estrutura de CONTEXT)

## Artefatos observáveis

- Sysmon: Event ID 10 (ProcessAccess/ThreadAccess) — sem Event ID 8
- Thread com call stack anômala (instrução atual fora de módulo)
- Suspend/resume de thread por processo externo

## Notas de lab

- Alvo: thread secundária do processo dummy em `WaitForSingleObject`
- Validar com x64dbg anexado ao alvo: stepping do hijack instrução a instrução
