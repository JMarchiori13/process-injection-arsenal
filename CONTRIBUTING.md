# Contribuindo

Contribuições são bem-vindas **dentro do escopo do projeto**: pesquisa de process injection documentada, PoCs de laboratório e correções nas notas técnicas.

## Regras

1. **Somente material de laboratório.** Payload padrão do projeto é a MessageBox inofensiva — não substitua por payloads reais.
2. **Toda PoC precisa de doc.** Cada implementação em `src/` deve vir acompanhada de nota em `docs/` explicando a técnica, pré-requisitos e comportamento esperado.
3. **Sem ofuscação/evasão.** As PoCs são didáticas: logging verboso, sem anti-analysis. Evasão de EDR está fora do escopo deste repositório.
4. **Não commite binários.** O `.gitignore` já cobre `.exe`, `.dll` e shellcode compilado — respeite-o.

## Processo

1. Abra uma issue descrevendo a técnica/módulo
2. Fork → branch `feat/<modulo>-<tecnica>`
3. PR com referência à issue e resultado do experimento de lab

## Padrões

- Linguagem das PoCs: C (MSVC) — logging verboso em cada etapa
- Notas em português ou inglês (consistência dentro do arquivo)
- Técnicas mapeadas para MITRE ATT&CK (técnica + sub-técnica)
- Tabelas para comparação de métodos
