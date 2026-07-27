# src/apc — PoCs de APC Injection

📖 Notas de pesquisa: [docs/apc-injection.md](../../docs/apc-injection.md)

## Experimentos planejados

| # | Experimento | Alvo | Observação |
|---|---|---|---|
| A1 | `QueueUserAPC` em thread alertável | Thread do dummy com `SleepEx(INFINITE, TRUE)` | Validar dependência de estado alertável |
| A2 | Early bird APC | Processo criado suspenso pela própria PoC | Execução garantida no resume |
| A3 | `NtQueueApcThread` | Mesmo cenário de A1, via Native API | Comparar com A1 |

## Convenções

- Linguagem: C (MSVC, x64)
- Payload: MessageBox padrão do projeto
- Em A2, o "processo alvo" é o binário dummy do lab criado com `CREATE_SUSPENDED`
