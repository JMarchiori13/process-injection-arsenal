# process-injection-arsenal

![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-T1055-red)
![Platform](https://img.shields.io/badge/platform-Windows-blue)
![Status](https://img.shields.io/badge/status-research%20scaffold-yellow)
![License](https://img.shields.io/badge/license-MIT-green)

> **⚠️ Disclaimer**
> Este repositório é um projeto de **pesquisa em segurança ofensiva** para uso exclusivo em **laboratórios isolados** e **operações autorizadas** (red team engagements com escopo contratual e Rules of Engagement assinados). O uso de qualquer técnica aqui documentada contra sistemas sem autorização explícita é crime (Lei nº 12.737/2012 — Brasil; CFAA — EUA; legislação equivalente em outras jurisdições). O autor não se responsabiliza por uso indevido.

## Objetivo

Estudo estruturado das técnicas de **Process Injection** ([MITRE ATT&CK T1055](https://attack.mitre.org/techniques/T1055/)) no Windows. Cada técnica é implementada como PoC de laboratório com a mesma payload inofensiva (MessageBox), permitindo comparação direta de comportamento, pré-requisitos e artefatos.

## Índice

- [Estrutura do projeto](#estrutura-do-projeto)
- [Módulos](#módulos)
- [Laboratório](#laboratório)
- [Roadmap](#roadmap)
- [Referências](#referências)

## Estrutura do projeto

```
process-injection-arsenal/
├── docs/                       # Notas de pesquisa por família de técnica
│   ├── remote-thread.md        # T1055 — CreateRemoteThread / NtCreateThreadEx
│   ├── apc-injection.md        # T1055 — QueueUserAPC, early bird, NtQueueApcThread
│   ├── thread-hijacking.md     # T1055 — Suspend/GetContext/SetContext/Resume
│   └── image-tampering.md      # T1055.012 — hollowing, module stomping, mapping
├── src/                        # PoCs de laboratório (ver README de cada módulo)
│   ├── remote-thread/
│   ├── apc/
│   ├── thread-hijack/
│   └── image-tampering/
├── lab/
│   └── setup.md                # Setup do ambiente de testes
├── CONTRIBUTING.md
└── LICENSE
```

## Módulos

| Módulo | ATT&CK | Técnicas | Status |
|---|---|---|---|
| [`remote-thread`](src/remote-thread/) | T1055 | `VirtualAllocEx` + `WriteProcessMemory` + `CreateRemoteThread`, variante `NtCreateThreadEx` | 📋 planejado |
| [`apc`](src/apc/) | T1055 | `QueueUserAPC`, early bird APC, `NtQueueApcThread` | 📋 planejado |
| [`thread-hijack`](src/thread-hijack/) | T1055 | Suspend → `GetThreadContext` → `SetThreadContext` → Resume | 📋 planejado |
| [`image-tampering`](src/image-tampering/) | T1055.012 | Process hollowing, module stomping, section mapping injection | 📋 planejado |

## Convenção de payload

Todas as PoCs executam a **mesma payload inofensiva**: um shellcode mínimo que exibe `MessageBoxA("process-injection-arsenal lab")`. Isso garante comparação justa entre técnicas e mantém o projeto claramente educacional.

## Laboratório

Veja **[lab/setup.md](lab/setup.md)** — VM Windows isolada, snapshots por estágio de hardening e procedimento padrão por experimento.

## Roadmap

- [x] Scaffold do repositório + disclaimers
- [x] Notas de pesquisa das 4 famílias de técnica
- [ ] PoC: remote thread (clássico + `NtCreateThreadEx`)
- [ ] PoC: APC injection (padrão + early bird)
- [ ] PoC: thread hijacking
- [ ] PoC: process hollowing
- [ ] PoC: module stomping / mapping injection
- [ ] Comparativo final: técnica × privilégio × estabilidade × artefatos

## Visualizações

<p align="center">
  <img src="docs/assets/attack-coverage.png" alt="Cobertura MITRE ATT&CK por módulo" width="70%">
</p>

<p align="center">
  <img src="docs/assets/technique-comparison.png" alt="Comparativo das técnicas de injeção" width="90%">
</p>

<p align="center">
  <img src="docs/assets/roadmap-status.png" alt="Status do roadmap" width="45%">
</p>

## Referências

- [MITRE ATT&CK — Process Injection (T1055)](https://attack.mitre.org/techniques/T1055/)
- [Red Team Notes — ired.team](https://www.ired.team/offensive-security/code-injection-process-injection)
- [Elastic Security — Injection research](https://www.elastic.co/security-labs)
- [MalDev Academy](https://maldevacademy.com/)
- MS Learn — Win32 process/thread APIs, NT native API (documentação não-oficial: [ntdoc.m417z.com](https://ntdoc.m417z.com/))

## License

MIT — veja [LICENSE](LICENSE). O disclaimer acima permanece válido independentemente da licença.
