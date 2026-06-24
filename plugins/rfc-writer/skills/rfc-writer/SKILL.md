---
name: rfc-writer
description: "Use sempre que o usuário pedir para escrever, criar, estruturar, preencher ou revisar um RFC, design doc, ou proposta técnica. Também ativa se o usuário mencionar 'RFC', 'design doc' ou pedir para documentar uma decisão técnica/arquitetural. Garante que o documento segue o template e o padrão de qualidade da empresa."
---

# RFC Writer

Você está ajudando a escrever ou revisar um RFC. Siga este processo.

## 1. Antes de escrever

Se faltar informação essencial, pergunte objetivamente (no máximo 2-3 perguntas)
antes de gerar o documento. Informação essencial = problema que o RFC resolve,
escopo, área/time responsável, e se há números reais disponíveis (custo,
volume, latência) — RFCs sem números concretos tendem a ser fracos.
Não pergunte o que já foi dito na conversa.

## 2. Estrutura obrigatória

Todo RFC segue exatamente as seções de `templates/rfc-template.md`, nesta
ordem: Informações Básicas → Contexto e Problema → Solução Proposta →
Escopo → Impacto e Riscos → Plano de Implementação → Métricas de Sucesso →
Discussão e Feedback. Não pule seções, não invente seções novas no nível
`##`, não troque a ordem. Subseções (`###`/`####`) dentro de cada bloco são
livres — adicione as que a proposta precisar (ex.: "Custos", "Migração",
"Storage") seguindo o exemplo em `examples/`.

Se uma seção `##` não se aplica, escreva "Não aplicável" e explique por quê
em uma linha — nunca delete a seção.

### Convenção de IDs

Itens enumeráveis usam prefixo de letra + número, sempre estável dentro do
documento (para poder ser referenciado em discussões/PRs depois):

- `O1, O2, ...` — objetivos incluídos no escopo
- `N1, N2, ...` — o que fica fora do escopo
- `R1, R2, ...` — riscos (cada um com `*Impacto: Alto/Médio/Baixo.*`)
- as Mitigações usam os mesmos números dos riscos (R1 mitiga R1)
- `CS1, CS2, ...` — métricas/critérios de sucesso
- `D1, D2, ...` — dependências externas ao plano

### Diagramas

Sempre que a RFC envolver sistemas/arquitetura, inclua diagramas Mermaid
(` ```mermaid `, tipo `flowchart`) para o estado atual e o proposto, e para
o sequenciamento de fases quando houver mais de 2 fases. Use `style` para
destacar pontos problemáticos (estado atual) ou novos componentes-chave
(proposta) — veja `examples/` para o padrão de cores usado.

## 3. Regras de redação

- Seja direto. Frases curtas. Sem enchimento ("é importante notar que...").
- Toda decisão precisa de um porquê. Justifique escolhas técnicas mesmo
  quando parecem óbvias.
- Alternativas/decisões consideradas precisam ser reais. Se há só uma opção
  viável, diga isso e explique por que as outras foram descartadas.
- Riscos nunca podem ser omitidos, e cada um precisa de mitigação. Um RFC
  sem riscos listados é sinal de que não foram pensados, não de que não
  existem.
- Quantifique sempre que possível (custo em US$/mês, latência, GB, número de
  instâncias/squads/dependências) em vez de adjetivos ("rápido", "caro").
  Cite a fonte do dado quando vier de uma ferramenta externa (ex.: "dados do
  AWS Cost Explorer, junho/2026").
- Tabelas para qualquer comparação (componentes, custos, antes/depois,
  cronograma) — não description corrida quando uma tabela é mais clara.

## 4. Checklist final (rode antes de entregar)

- [ ] Todas as seções `##` do template estão presentes e na ordem certa
- [ ] O problema está descrito antes da solução, com números reais quando
      existirem
- [ ] Escopo tem O# (incluído) e N# (excluído) explícitos
- [ ] Todo risco R# tem uma mitigação correspondente
- [ ] Existe pelo menos um critério de sucesso (CS#) verificável, não vago
- [ ] Diagramas Mermaid presentes se a RFC envolve arquitetura/sistemas
- [ ] Sem jargão não explicado, sem seção vazia, sem prosa enrolada

## 5. Modo revisão

Se o usuário colar um RFC já escrito (em vez de pedir para criar um novo),
não reescreva do zero. Rode o checklist acima contra o documento e devolva
uma lista do que falta ou está fraco, seção por seção, citando os IDs (ex.:
"R3 não tem mitigação"). Só reescreva o trecho se o usuário pedir
explicitamente.

## Referências

- Template completo: `templates/rfc-template.md`
- Exemplo real e completo (tom, nível de detalhe, uso de diagramas e
  tabelas): `examples/RFC-002-Estrategia-RDS-Governanca.md`
