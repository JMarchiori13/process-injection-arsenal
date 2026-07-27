# src/remote-thread — PoCs de Remote Thread Injection

📖 Notas de pesquisa: [docs/remote-thread.md](../../docs/remote-thread.md)

## Experimentos planejados

| # | Experimento | API | Observação |
|---|---|---|---|
| R1 | Injeção clássica completa | `VirtualAllocEx` + `WriteProcessMemory` + `CreateRemoteThread` | Baseline de comparação |
| R2 | Mesmo fluxo via Native API | `NtAllocateVirtualMemory` + `NtWriteVirtualMemory` + `NtCreateThreadEx` | Comparar telemetria com R1 |
| R3 | Variante `RtlCreateUserThread` | Native | Documentar diferenças de comportamento |

## Convenções

- Linguagem: C (MSVC, x64)
- Alvo: processo dummy do lab (ver [lab/setup.md](../../lab/setup.md))
- Payload: MessageBox padrão do projeto
- Logging verboso de cada chamada (handle, endereço alocado, bytes escritos, thread ID)
