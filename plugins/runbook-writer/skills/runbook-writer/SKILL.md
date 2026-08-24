---
name: runbook-writer
description: "Use sempre que o usuário pedir para escrever, criar, estruturar, preencher ou revisar um runbook, playbook operacional, procedimento de incidente, guia de deploy/rollback, ou passo a passo de operação. Também ativa se o usuário mencionar 'runbook', 'playbook', 'procedimento operacional', 'SOP' ou pedir para documentar como executar/recuperar/reiniciar algo em produção. Garante que o documento segue o template e o padrão de qualidade da empresa."
---

# Runbook Writer

Você está ajudando a escrever ou revisar um runbook. Siga este processo.

Um runbook é diferente de um RFC: ninguém lê pra decidir, lê pra executar —
geralmente sob pressão, de madrugada, sem contexto. Cada frase que exige
interpretação é um risco. O texto tem que ser executável literalmente.

## 1. Antes de escrever

Se faltar informação essencial, pergunte objetivamente (no máximo 2-3
perguntas) antes de gerar o documento. Informação essencial = qual situação
dispara o runbook, qual sistema é afetado, quem executa (on-call, time
específico, qualquer dev), e se a operação tem volta (rollback) ou não.
Não pergunte o que já foi dito na conversa.

Se o repo tiver código, scripts, manifests, Makefile ou pipeline relacionados
à operação, leia antes de escrever. Os comandos do runbook precisam ser os
que existem, com os nomes reais de serviço, namespace, job, bucket, etc.
Runbook com comando inventado é pior que runbook nenhum.

## 2. Estrutura obrigatória

Todo runbook segue exatamente as seções de `templates/runbook-template.md`,
nesta ordem: Informações Básicas → Quando Usar → Pré-requisitos →
Diagnóstico → Procedimento → Verificação → Rollback → Escalonamento →
Pós-ação e Histórico. Não pule seções, não invente seções novas no nível
`##`, não troque a ordem. Subseções (`###`/`####`) dentro de cada bloco são
livres.

Se uma seção `##` não se aplica, escreva "Não aplicável" e explique por quê
em uma linha — nunca delete a seção. Rollback "não aplicável" precisa de
justificativa forte (ex.: operação idempotente e somente leitura); se a
operação altera estado e não tem volta, diga isso em destaque e explique o
que fazer no lugar.

### Convenção de IDs

Itens enumeráveis usam prefixo de letra + número, estável dentro do
documento (para poder ser referenciado em chat de incidente ou postmortem):

- `G1, G2, ...` — gatilhos (sintomas/alertas que indicam usar este runbook)
- `P1, P2, ...` — pré-requisitos (acesso, ferramenta, permissão)
- Passos do procedimento usam número simples (`Passo 1`, `Passo 2`) e
  sub-passos `1.1`, `1.2` — assim dá pra escrever "se falhar, volte ao
  Passo 3"
- `V1, V2, ...` — verificações de sucesso
- `E1, E2, ...` — níveis de escalonamento
- `K1, K2, ...` — problemas conhecidos (known issues) e como contornar

### Anatomia de um passo

Todo passo do Procedimento tem, obrigatoriamente:

1. **O que fazer** — uma ação só. Se tem "e", são dois passos.
2. **Comando** — em bloco de código, copiável, com placeholders em
   `<MAIUSCULAS>` e uma linha dizendo de onde vem cada placeholder.
3. **Resultado esperado** — o que o operador deve ver pra saber que deu
   certo (saída, status, valor). "Deve funcionar" não é resultado esperado.
4. **Se falhar** — o que fazer: tentar de novo, pular para o Passo N, ir
   para Rollback, ou escalar para E#. Nunca deixe o operador decidir sozinho.
5. **Tempo estimado** — quando o passo demora mais que alguns segundos.

Comandos destrutivos ou irreversíveis (delete, drop, scale to 0, force push,
rotação de credencial) vêm precedidos de `> **ATENÇÃO:**` explicando o que
se perde e, quando existir, o `--dry-run` / plano / backup que deve rodar
antes.

### Diagramas

Se o Diagnóstico ou o Procedimento tem ramificação ("se X, vá para A; senão
B"), inclua um `flowchart` Mermaid com a árvore de decisão. Fluxo linear não
precisa de diagrama.

## 3. Regras de redação

- Imperativo, uma ação por frase. "Reinicie o pod" e não "o pod deve ser
  reiniciado" ou "pode-se reiniciar o pod".
- Nada de "verifique se está tudo ok", "confira os logs", "analise o
  serviço". Diga o comando exato, o que procurar na saída, e qual valor é
  normal.
- Placeholders sempre em `<MAIUSCULAS>` e sempre explicados. Valores que não
  mudam (namespace, nome de serviço, região) vão hardcoded, não como
  placeholder — placeholder demais também obriga o operador a pensar.
- Secrets, tokens, senhas: nunca no runbook. Aponte onde buscar (vault,
  secret manager, 1Password, variável de ambiente) e qual nome/chave.
- Contatos de escalonamento com nome, canal e horário reais (ex.: "#sre-oncall,
  24x7", "@fulano, horário comercial"). "Acione o time responsável" não é
  escalonamento.
- Tabelas para pré-requisitos, escalonamento e problemas conhecidos.
- Sem contexto histórico, sem justificativa de arquitetura, sem "por que
  esse sistema é assim". Se for relevante, linke o RFC/postmortem. O
  runbook é pra executar, não pra entender.

## 4. Checklist final (rode antes de entregar)

- [ ] Todas as seções `##` do template estão presentes e na ordem certa
- [ ] Gatilhos G# são sintomas/alertas observáveis, e existe "quando NÃO usar"
- [ ] Todo pré-requisito P# diz como obter (acesso, instalação, permissão)
- [ ] Diagnóstico confirma que é este o problema antes de agir
- [ ] Todo passo tem comando copiável + resultado esperado + "se falhar"
- [ ] Nenhum passo tem duas ações; nenhum passo pede julgamento sem critério
- [ ] Comandos destrutivos têm `ATENÇÃO` e dry-run/backup quando existe
- [ ] Nenhum secret, token ou senha no texto
- [ ] Verificação V# é observável (comando + valor esperado), não "funcionou"
- [ ] Rollback existe, ou "não aplicável" com justificativa forte
- [ ] Escalonamento E# tem nome/canal/horário reais
- [ ] Comandos batem com o que existe no repo (nomes de serviço, namespace,
      script) quando o repo estava disponível
- [ ] Data do último teste preenchida (ou marcada como "nunca testado")

## 5. Modo revisão

Se o usuário colar um runbook já escrito (em vez de pedir para criar um
novo), não reescreva do zero. Rode o checklist acima contra o documento e
devolva uma lista do que falta ou está fraco, seção por seção, citando os
IDs e passos (ex.: "Passo 4 não tem 'se falhar'", "E1 não tem canal").
Priorize na resposta: comando ausente/errado > destrutivo sem aviso > secret
exposto > rollback ausente > o resto. Só reescreva o trecho se o usuário
pedir explicitamente.

## Referências

- Template completo: `templates/runbook-template.md`
