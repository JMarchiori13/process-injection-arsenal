# Setup do Laboratório

Ambiente de referência para todos os experimentos deste repositório. **Nenhum experimento deve rodar fora deste isolamento.**

## VM alvo (Windows 10/11)

| Item | Configuração |
|---|---|
| Rede | Host-only ou NIC desabilitada |
| Snapshots | `clean-base` (pós-install) + um por estágio de hardening |
| Processos alvo | `notepad.exe`, `explorer.exe`, processo dummy próprio do lab (alvo preferido — evita crashar processos do sistema) |
| Ferramentas | MSVC (Build Tools), x64dbg, Process Hacker/System Informer, Process Monitor, Sysmon |

## Processo alvo do lab

Para a maioria das PoCs, o alvo é um **processo dummy do próprio lab**: um executável mínimo que roda em loop. Injetar em processo próprio do lab evita instabilidade e simplifica a análise com debugger.

## Matriz de observação

| Estágio | Configuração | O que observar |
|---|---|---|
| 0 | Baseline | Funcionamento da técnica, telemetria Sysmon (Event IDs 1, 8, 10) |
| 1 | Defender ATP / EDR de lab | Quais etapas disparam alerta (alloc remoto, escrita, criação de thread) |
| 2 | Processo alvo protegido (PPL) | Falhas documentadas em `docs/` |

## Procedimento por experimento

1. Restaurar snapshot apropriado
2. Compilar a PoC fora da VM, copiar para o lab
3. Executar com o processo alvo dummy ativo, observando no debugger/System Informer
4. Registrar resultado + telemetria no doc da técnica
5. Restaurar snapshot
