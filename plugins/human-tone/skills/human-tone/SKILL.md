---
name: human-tone
description: "Define o tom de tudo que vai pro histórico do repositório: mensagem de commit, título e descrição de PR, comentário de review e comentário no código. Curto, informal, sem emoji, sem cara de texto gerado por AI. Use sempre que for escrever ou revisar commit, PR, changelog ou comentário de código."
---

# Human Tone

Vale para tudo que fica registrado no repositório: mensagem de commit, título e
descrição de PR, comentário de review e comentário dentro do código.
O objetivo é simples: tem que parecer que um dev escreveu com pressa, não que
uma ferramenta gerou.

## Sempre

- Sem emoji. Em lugar nenhum.
- Sem footer de AI ("Generated with Claude Code", "Co-Authored-By: Claude", etc).
- Voz ativa e direta. "sobe o timeout do client" e não "This commit increases...".
- Tom de dev falando com dev. Informal, sem cerimônia, sem tom de marketing.
- Curto. Subject de commit até 72 chars. Descrição de PR em 3 a 6 linhas.
- Escreva o que o diff não mostra: o porquê. O "o quê" já está no diff.
- Siga o idioma que o repositório já usa no histórico (`git log --oneline -20`).

## Nunca

- Lista gigante repetindo cada arquivo alterado.
- Seções tipo "## Summary / ## Changes / ## Test Plan" em PR pequeno.
- Adjetivo de release note: "robusto", "elegante", "comprehensive",
  "significantly improves", "melhora substancialmente".
- Abertura formal: "This PR introduces...", "Neste commit foi realizado...".
- Repetir o título na primeira linha da descrição.
- Comentar o óbvio no código (`// incrementa o contador`).

## Commits

Se o repositório já usa conventional commits, mantenha o prefixo. Senão,
imperativo em minúsculo e pronto.

Bom:

```
fix: timeout do client de pagamento tava em 2s, subi pra 10s
```

```
refactor: tira o wrapper de cache, lru_cache já resolve
```

Ruim:

```
feat: Implement comprehensive timeout configuration for payment client

## Summary
- Updated the timeout value from 2s to 10s
- Improved reliability of the payment integration
```

## PR

Título é o commit principal, sem enfeite. O corpo responde três coisas, em
prosa curta:

1. o que estava quebrado ou faltando
2. o que você fez
3. o que quem revisa precisa saber (risco, migração, o que não foi feito)

Se não tem nada além do título pra dizer, deixa só o título. PR curto com
descrição longa é sinal de texto inflado.

## Comentário de review

Vai direto ao ponto e sugere a correção. Uma ou duas linhas.
"esse path aceita `../`, dá pra sair do diretório" é melhor que um parágrafo
explicando path traversal.

## Comentário no código

Só quando o código não conta a história sozinho: decisão não óbvia, gambiarra
com motivo, limite conhecido. Nada de docstring cerimonial em função de três
linhas.

## Antes de mandar

Relê e pergunta: um dev escreveria isso? Se parece release note, corta metade.
