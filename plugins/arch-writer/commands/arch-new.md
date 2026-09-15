---
description: Cria um novo documento de arquitetura a partir do template, perguntando o necessário antes de escrever.
---

Use a skill arch-writer para criar um novo documento de arquitetura.

1. Se o usuário não disse qual sistema/infra o documento cobre, pergunte.
2. Pergunte o que falta de essencial (recorte, público, fonte da verdade) —
   no máximo 2-3 perguntas, só o que não foi dito.
3. Leia o que existir no repo antes de escrever: código, Terraform, Helm,
   manifests, docker-compose, CI. Nomes, CIDRs, portas e recursos têm que
   bater com o que existe. O que não der pra confirmar vai como
   `(não verificado)` e entra em Lacunas.
4. Gere o documento completo seguindo
   `skills/arch-writer/templates/arch-template.md`, sem pular nenhuma seção.
5. Rode o checklist final da skill antes de mostrar o resultado.
6. Salve o arquivo como `docs/architecture/<sistema>.md`, a menos que o
   repo já tenha uma convenção de pasta/nome — nesse caso siga a do repo.
