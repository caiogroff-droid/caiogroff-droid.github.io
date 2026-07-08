---
title: "Issues cregit"
date: 2026-07-08 00:00:00 -0300
categories: [MAC0470, Open Source]
tags: [open-source, git, github, documentation, cregit]
---

Além das contribuições para o kernel Linux, a disciplina também envolve contribuir para outros projetos open source. Uma dessas oportunidades foi o repositório `cregit-codev`, um fork do projeto original `cregit`.

## Issue 1 — Clarificar a relação com o upstream

A primeira issue pedia para deixar claro no README a relação entre o fork `cregit-codev` e o repositório upstream original em `https://github.com/cregit/cregit`.

A solução foi simples: adicionar uma seção `## About` logo após o logo, antes de "Preliminaries", explicando em duas frases que se trata de um fork e onde fica o upstream. Assim quem abre o repositório já entende o contexto antes de começar a ler qualquer documentação.

No `CONTRIBUTING.md` foi feita uma mudança semelhante, adicionando uma observação logo após o cabeçalho do arquivo.

O princípio aqui foi não escrever além do necessário — a issue pedia apenas clareza sobre a relação com o upstream, então duas ou três frases já resolvem o problema.

## Issue 2 — Documentar como rodar o `run_pipeline_process.sh`

A segunda issue pedia para adicionar ao README instruções de como executar o script `run_pipeline_process.sh`. O README já mencionava que o script existia e que aceitava `--help`, mas não explicava como usá-lo.

Ao analisar o script, percebi que ele não implementa `--help` de fato — ele simplesmente lê o primeiro argumento como número do passo:

```sh
FROM_STEP=${1:-1}
```

Então ao invés de documentar uma funcionalidade que não existe, a PR documenta o uso real:

```sh
# Executar o pipeline completo
./run_pipeline_process.sh

# Retomar a partir de um passo específico
./run_pipeline_process.sh 5
```

E menciona na descrição da PR que a referência ao `--help` foi removida porque o script não implementa essa opção. Isso evita confundir futuros contribuidores com documentação incorreta.
