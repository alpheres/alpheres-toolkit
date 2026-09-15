---
description: Revisa um documento de arquitetura existente (arquivo ou colado no chat) contra o checklist da skill arch-writer.
---

Use a skill arch-writer em modo revisão.

1. Se o usuário não colou o conteúdo, pergunte o caminho do arquivo e leia-o.
2. Compare seção a seção com
   `skills/arch-writer/templates/arch-template.md`.
3. Se o repo tiver a IaC/código do sistema, confira se o documento ainda
   bate com o que existe (componentes, CIDRs, portas, regiões, integrações).
4. Rode o checklist final da skill. Dê atenção especial a: informação
   desatualizada, secret exposto, diagrama que não bate com a tabela, seta
   sem rótulo, fronteira de confiança sem controle, sigla sem explicação.
5. Devolva uma lista objetiva: o que está errado, o que está faltando, o que
   está fraco (com o motivo, citando seção/ID) e o que está bom. Não
   reescreva o documento, a menos que seja pedido.
