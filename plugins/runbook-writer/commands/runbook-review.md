---
description: Revisa um runbook existente (arquivo ou colado no chat) contra o checklist da skill runbook-writer.
---

Use a skill runbook-writer em modo revisão.

1. Se o usuário não colou o conteúdo, pergunte o caminho do arquivo e leia-o.
2. Compare seção a seção com
   `skills/runbook-writer/templates/runbook-template.md`.
3. Rode o checklist final da skill. Dê atenção especial a: passo sem comando
   concreto, passo sem "se falhar", comando destrutivo sem aviso, secret
   hardcoded, rollback ausente, escalonamento sem nome/canal real.
4. Devolva uma lista objetiva: o que está faltando, o que está fraco (com o
   motivo, citando o passo/ID), e o que está bom. Não reescreva o documento,
   a menos que seja pedido.
