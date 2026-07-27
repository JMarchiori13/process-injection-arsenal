# T1055 — Remote Thread Injection

A técnica clássica de injeção em processo remoto. Base de comparação para todas as demais.

## Fluxo conceitual

```
OpenProcess(alvo, PROCESS_VM_OPERATION | PROCESS_VM_WRITE | PROCESS_CREATE_THREAD)
  └─ VirtualAllocEx(alvo, PAGE_EXECUTE_READWRITE)
       └─ WriteProcessMemory(alvo, shellcode)
            └─ CreateRemoteThread(alvo, entrypoint = shellcode)
```

## Variantes documentadas

| Variante | Diferença | Observações |
|---|---|---|
| `CreateRemoteThread` | WinAPI padrão | Mais monitorada; cadeia completa visível em telemetria |
| `NtCreateThreadEx` | Native API | Mesma operação via `ntdll.dll`, contorna hooks em user-mode da camada WinAPI |
| `RtlCreateUserThread` | Native, estável | Usada internamente pelo CSRSS; alternativa documentada |

## Pré-requisitos

- Handle com direitos suficientes sobre o processo alvo (mesmo usuário e integridade, ou `SeDebugPrivilege` para alvos de outros contextos)
- Alvo e injetor na mesma arquitetura (x64 → x64)

## Artefatos observáveis

- Thread criada com start address fora de módulo mapeado (imagem)
- Região de memória RWX (ou RW → RX) não pertencente a módulo
- Sysmon: Event ID 8 (CreateRemoteThread), Event ID 10 (ProcessAccess)

## Notas de lab

- Usar o processo dummy do lab como alvo
- Comparar lado a lado: variante WinAPI vs. Native no mesmo snapshot
