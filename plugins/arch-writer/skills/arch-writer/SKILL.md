---
name: arch-writer
description: "Use sempre que o usuário pedir para explicar, documentar, desenhar ou revisar a arquitetura de um sistema, de uma infraestrutura cloud (AWS, GCP, Azure, Kubernetes) ou de uma rede (VPC, subnets, DNS, load balancer, VPN, firewall). Também ativa se o usuário pedir 'como funciona o sistema X', 'documento de arquitetura', 'visão geral da arquitetura', 'diagrama de arquitetura/rede', 'onboarding técnico' ou 'explica essa infra'. Não use para propor mudança (isso é rfc-writer) nem para procedimento operacional (isso é runbook-writer)."
---

# Arch Writer

Você está ajudando a explicar ou documentar uma arquitetura. Siga este
processo.

Um documento de arquitetura é diferente de um RFC e de um runbook: o RFC é
lido pra decidir, o runbook pra executar, e este pra **entender**. O leitor
chega sem contexto (dev novo, outro squad, auditoria, alguém de plantão
tentando entender o que quebrou) e precisa sair com o modelo mental certo:
o que existe, como as peças conversam, onde estão as fronteiras e por que
foi feito assim.

## 0. Documento ou explicação rápida?

- **Pergunta pontual no chat** ("como funciona o NAT gateway aqui?",
  "qual a diferença entre peering e Transit Gateway?"): não use o template.
  Responda direto aplicando as regras de redação da seção 3: camadas,
  diagrama quando ajudar, números, trade-off.
- **Documento** (pedido explícito de doc, visão geral de um sistema inteiro,
  onboarding, material pra outro time): use o template completo.

## 1. Antes de escrever

Se faltar informação essencial, pergunte objetivamente (no máximo 2-3
perguntas) antes de gerar o documento. Informação essencial = qual sistema
ou recorte o documento cobre, quem vai ler (isso define a profundidade), e
onde está a fonte da verdade (repo, IaC, console, pessoa). Não pergunte o
que já foi dito na conversa.

Se o repo tiver código, Terraform, Helm, manifests Kubernetes,
docker-compose, pipeline de CI ou config de rede, leia antes de escrever.
Nomes de recurso, CIDRs, portas, regiões, filas, buckets e integrações
precisam ser os reais. Arquitetura descrita de memória costuma estar errada
em algum detalhe que importa.

O que não der pra confirmar nas fontes não vira afirmação: marque com
`(não verificado)` no texto e registre como lacuna `Q#`. Doc de arquitetura
confiante e errado é pior que doc com lacuna explícita.

## 2. Estrutura obrigatória

Todo documento segue exatamente as seções de
`templates/arch-template.md`, nesta ordem: Informações Básicas → Resumo →
Contexto → Componentes → Fluxos Principais → Infraestrutura e Rede → Dados →
Segurança → Operação e Resiliência → Decisões e Trade-offs → Glossário e
Lacunas. Não pule seções, não invente seções novas no nível `##`, não troque
a ordem. Subseções (`###`/`####`) dentro de cada bloco são livres.

Se uma seção `##` não se aplica, escreva "Não aplicável" e explique por quê
em uma linha — nunca delete a seção (ex.: sistema sem estado próprio →
Dados "Não aplicável: não persiste nada, só repassa para X").

A ordem é proposital: vai de fora pra dentro (zoom). Resumo e Contexto
tratam o sistema como caixa-preta; Componentes abre a caixa; Fluxos mostra
as peças em movimento; o resto aprofunda em camadas específicas.

### Convenção de IDs

Itens enumeráveis usam prefixo de letra + número, estável dentro do
documento (pra poder ser citado em RFC, PR, postmortem ou chat):

- `F1, F2, ...` — fluxos principais
- `T1, T2, ...` — fronteiras de confiança (onde o dado cruza de uma zona
  pra outra: internet → VPC, pod → banco, conta A → conta B)
- `L1, L2, ...` — limites conhecidos: SPOF, quota, gargalo, dívida técnica
  (cada um com `*Impacto: Alto/Médio/Baixo.*`)
- `D1, D2, ...` — decisões de arquitetura e seus trade-offs
- `Q1, Q2, ...` — lacunas: o que não foi possível confirmar, e com quem ou
  onde confirmar

### Diagramas

Mermaid, sempre. Um diagrama por nível de zoom, não um diagrama gigante:

| Onde | Tipo | Mostra |
|---|---|---|
| Contexto | `flowchart` | o sistema como uma caixa, atores e sistemas externos |
| Componentes | `flowchart` com `subgraph` | serviços, bancos, filas e quem chama quem |
| Fluxos Principais | `sequenceDiagram` | ordem das chamadas num fluxo F# |
| Infraestrutura e Rede | `flowchart` com `subgraph` aninhado | conta/projeto → região → VPC → subnet/AZ |

Regras:

- Toda seta tem rótulo: protocolo e porta, ou sync/async (`"HTTPS 443"`,
  `"gRPC"`, `"SQS async"`, `"TCP 5432"`). Seta sem rótulo não explica nada.
- Todo nó do diagrama aparece na tabela da seção, e vice-versa. Nome igual
  nos dois.
- No máximo ~15 nós por diagrama. Passou disso, quebre em mais de um
  diagrama ou suba o nível de abstração.
- Use `style` pra destacar: componentes com limite `L#` em vermelho
  (`fill:#fdd,stroke:#c00`) e sistemas externos/fora do escopo em cinza
  tracejado (`fill:#eee,stroke:#999,stroke-dasharray:4`).

## 3. Regras de redação

- Explique em camadas: primeiro a frase que resume, depois o detalhe.
  Quem parar no primeiro parágrafo de cada seção tem que ter entendido o
  essencial.
- Ajuste a profundidade ao público declarado em Informações Básicas. Doc
  pra gestão não lista porta de security group; doc pra SRE lista.
- Todo componente responde duas perguntas: o que faz e por que existe.
  "Redis — cache" não basta; "Redis — cache de sessão, evita ir no Postgres
  a cada request autenticado" basta.
- Termo técnico ou sigla explicado na primeira vez que aparece (e no
  Glossário). "Ingress", "NAT", "IRSA", "PrivateLink" não são óbvios pra
  todo mundo que vai ler.
- Quantifique: CIDR, porta, região, número de réplicas/instâncias, RPS,
  latência, volume de dados, retenção, custo em US$/mês. Cite a fonte e a
  data do número quando vier de ferramenta (ex.: "CloudWatch, média de
  agosto/2026").
- Tabelas pra inventário e comparação (componentes, portas, dados, modos de
  falha). Prosa pra explicar comportamento e porquê.
- Descreva o que existe, não o que deveria existir. Problema encontrado vira
  `L#` ou `Q#`; proposta de mudança vai pra um RFC (rfc-writer), com link.
- Secrets, tokens, senhas e chaves nunca no documento. Diga onde vivem
  (nome do secret no Secrets Manager/Vault, variável de ambiente) e quem tem
  acesso. Se o documento for sair da empresa, pergunte antes de incluir
  account IDs, IPs e hostnames internos.

## 4. Checklist final (rode antes de entregar)

- [ ] Todas as seções `##` do template estão presentes e na ordem certa
- [ ] Público está declarado e a profundidade bate com ele
- [ ] Resumo dá o modelo mental em 3-5 linhas, sem precisar ler o resto
- [ ] Diagramas de contexto, componentes e rede presentes (quando aplicável)
- [ ] Toda seta de diagrama tem rótulo; nó do diagrama = linha da tabela
- [ ] Todo componente diz o que faz e por que existe
- [ ] Pelo menos um fluxo F# com `sequenceDiagram` e o que acontece se um
      passo falha
- [ ] Toda fronteira de confiança T# diz qual controle é aplicado
- [ ] Limites L# têm impacto; decisões D# têm porquê e custo aceito
- [ ] Nomes, CIDRs, portas e recursos batem com o repo/IaC quando estava
      disponível; o resto está marcado `(não verificado)` e listado em Q#
- [ ] Nenhum secret no texto; siglas explicadas; sem seção vazia

## 5. Modo revisão

Se o usuário colar um documento de arquitetura já escrito (em vez de pedir
para criar um novo), não reescreva do zero. Rode o checklist acima contra o
documento e devolva uma lista do que falta ou está fraco, seção por seção,
citando os IDs (ex.: "T2 não diz qual controle protege a subnet de dados",
"diagrama de componentes tem `worker` que não está na tabela"). Se o repo
ou a IaC estiver disponível, confira se o documento ainda bate com o que
existe — divergência entre doc e infra é o achado mais importante.
Priorize: informação errada/desatualizada > segredo exposto > diagrama
inconsistente com a tabela > fronteira de confiança sem controle > o resto.
Só reescreva o trecho se o usuário pedir explicitamente.

## Referências

- Template completo: `templates/arch-template.md`
