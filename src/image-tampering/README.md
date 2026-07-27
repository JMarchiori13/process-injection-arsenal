# src/image-tampering — PoCs de Hollowing, Stomping e Mapping

📖 Notas de pesquisa: [docs/image-tampering.md](../../docs/image-tampering.md)

## Experimentos planejados

| # | Experimento | Técnica | Observação |
|---|---|---|---|
| I1 | Process hollowing | Unmap + rewrite da imagem em processo suspenso | Hospedeiro: cópia de `notepad.exe` no lab |
| I2 | Module stomping | Overwrite de `.text` de DLL legítima do lab | Comparar: memória backed por imagem |
| I3 | Mapping injection | `NtCreateSection` + map local/remoto | Alternativa sem `WriteProcessMemory` |

## Convenções

- Linguagem: C (MSVC, x64)
- I1 é a PoC mais frágil do repositório — validar alinhamento de seções e relocations da imagem injetada
- Payload: MessageBox padrão do projeto (imagem PE mínima para I1)
