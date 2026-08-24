---
description: Cria um novo runbook a partir do template, perguntando o necessário antes de escrever.
---

Use a skill runbook-writer para criar um novo runbook.

1. Se o usuário não disse qual operação/incidente o runbook cobre, pergunte.
2. Pergunte o que falta de essencial (gatilho, sistema afetado, quem executa,
   se existe rollback) — no máximo 2-3 perguntas, só o que não foi dito.
3. Se o repo tiver código, scripts, Makefile, manifests ou CI relacionados à
   operação, leia antes de escrever: os comandos do runbook têm que bater com
   o que existe, não com o que seria bonito.
4. Gere o documento completo seguindo
   `skills/runbook-writer/templates/runbook-template.md`, sem pular nenhuma
   seção.
5. Rode o checklist final da skill antes de mostrar o resultado.
6. Salve o arquivo como `runbooks/<sistema>-<acao>.md` (ex.:
   `runbooks/payment-api-restart.md`), a menos que o repo já tenha uma
   convenção de pasta/nome — nesse caso siga a do repo.
