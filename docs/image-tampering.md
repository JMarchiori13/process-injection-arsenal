# T1055.012 — Image Tampering: Hollowing, Stomping e Mapping

Família de técnicas que manipulam a **imagem** do processo — substituindo, corrompendo ou mapeando código no lugar de módulos legítimos.

## Process Hollowing

```
CreateProcess(legítimo, CREATE_SUSPENDED)
  └─ NtUnmapViewOfSection (desmapear a imagem original)
       └─ VirtualAllocEx (na base original)
            └─ WriteProcessMemory (headers + seções da nova imagem)
                 └─ SetThreadContext (entrypoint da nova imagem)
                      └─ ResumeThread
```

Resultado: processo com nome/caminho de binário legítimo executando imagem completamente diferente.

## Module Stomping (Module Overloading)

- Carregar (ou usar) uma DLL legítima no processo alvo
- Sobrescrever a seção `.text` do módulo com shellcode
- Executar via thread no endereço do módulo "pisoteado"
- Vantagem: código executa a partir de memória **backed por imagem em disco** — foge de heurísticas de "memória privada executável"

## Section Mapping Injection

```
NtCreateSection (PAGE_EXECUTE_READWRITE)
  ├─ NtMapViewOfSection (no processo local — escreve shellcode)
  └─ NtMapViewOfSection (no processo remoto — executável)
       └─ CreateRemoteThread / hijack / APC na view remota
```

Alternativa ao VirtualAllocEx + WriteProcessMemory: uma única seção compartilhada entre os dois processos.

## Comparativo

| Técnica | Imagem em disco? | Cria thread? | Complexidade |
|---|---|---|---|
| Hollowing | Sim (nome legítimo) | Não (thread do processo criado) | Alta |
| Module stomping | Sim (módulo legítimo) | Depende do disparo | Média |
| Mapping injection | Não (seção não-backed) | Depende do disparo | Média |

## Artefatos observáveis

- Hollowing: imagem do processo ≠ conteúdo em disco (comparação de hash), PEB vs. memória divergentes
- Stomping: módulo assinado com `.text` modificada (falha de verificação de assinatura em memória)
- Mapping: seções compartilhadas executáveis entre processos distintos

## Notas de lab

- Hollowing: usar binário legítimo próprio do lab como "hospedeiro" (ex.: cópia de `notepad.exe`)
- Stomping: DLL do próprio lab como vítima do overwrite
