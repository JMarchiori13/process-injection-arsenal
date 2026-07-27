# T1055 — APC Injection

Injeção via Asynchronous Procedure Calls: enfileirar código para execução no contexto de uma thread existente do processo alvo.

## Conceito

Toda thread Windows possui uma fila de APCs. Uma APC enfileirada executa quando a thread entra em **estado alertável** (ex.: `SleepEx`, `WaitForSingleObjectEx` com `bAlertable=TRUE`). A injeção explora isso para executar código sem criar thread nova.

## Variantes documentadas

| Variante | Alvo | Observações |
|---|---|---|
| `QueueUserAPC` | Thread existente em estado alertável | Requer thread alertável no alvo; menos confiável |
| Early Bird APC | Thread de processo recém-criado (suspenso) | Cria processo suspenso, injeta, enfileira APC antes da thread iniciar — execução garantida no resume |
| `NtQueueApcThread` | Native API | Mesma operação via ntdll, contorna hooks WinAPI |

## Fluxo early bird

```
CreateProcess(alvo, CREATE_SUSPENDED)
  └─ VirtualAllocEx + WriteProcessMemory (shellcode)
       └─ QueueUserAPC(shellcode, thread principal)
            └─ ResumeThread → APC executa no início da thread
```

## Pré-requisitos

- Para APC em processo existente: thread alertável (raridade em processos GUI; comum em processos com waits)
- Para early bird: nenhum requisito especial — você controla a criação do processo

## Artefatos observáveis

- Ausência de Event ID 8 (nenhum CreateRemoteThread)
- Região de memória RWX/RX não-módulo no processo alvo
- Early bird: processo criado suspenso + escrita remota antes do primeiro resume

## Notas de lab

- Payload padrão (MessageBox) — em APC de thread existente, usar thread do processo dummy com `SleepEx` alertável
