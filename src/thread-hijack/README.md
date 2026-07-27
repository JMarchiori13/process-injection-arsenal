# src/thread-hijack — PoCs de Thread Hijacking

📖 Notas de pesquisa: [docs/thread-hijacking.md](../../docs/thread-hijacking.md)

## Experimentos planejados

| # | Experimento | Variante | Observação |
|---|---|---|---|
| T1 | Hijack destrutivo | `SuspendThread` → redirect RIP → `ResumeThread` | Baseline; thread não retorna |
| T2 | Hijack com restauração | Stub salva registradores, executa payload, restaura contexto | Thread sobrevive — comparar estabilidade |
| T3 | Seleção de thread | Enumerar threads do alvo, heurística de escolha (wait estável) | Documentar critérios |

## Convenções

- Linguagem: C (MSVC, x64)
- Alvo: thread secundária do dummy em wait estável
- Validar T2 com x64dbg: contexto antes/depois deve ser idêntico (exceto execução do payload)
- Payload: MessageBox padrão do projeto
