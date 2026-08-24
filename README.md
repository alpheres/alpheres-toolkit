# alpheres-toolkit

Coleção de plugins para Claude Code.

## Plugins incluídos

| Plugin | Descrição |
|--------|-----------|
| **rfc-writer** | Escrita e revisão de RFCs / design docs seguindo template fixo |
| **runbook-writer** | Escrita e revisão de runbooks operacionais (procedimento passo a passo, verificação, rollback, escalonamento) seguindo template fixo |
| **secure-code** | Aplica princípios de segurança (OWASP, auth, injection prevention) em todo código gerado — ativo automaticamente via hook de sessão |
| **human-tone** | Commits, PRs e comentários com tom de dev — curto, informal, sem emoji, sem cara de AI. Ativo via hook de sessão |

## Instalar (CLI ou VS Code)

Dentro de uma sessão do Claude Code:

```
/plugin install alpheres@alpheres-toolkit
```

## Uso

- **RFC**: peça para escrever ou revisar um RFC — a skill `rfc-writer` ativa automaticamente.
- **Runbook**: peça para escrever ou revisar um runbook/playbook — a skill `runbook-writer` ativa automaticamente. Também `/runbook-new` e `/runbook-review`.
- **Segurança**: as regras de segurança são injetadas em toda sessão. Use `/secure-code` para revisar código específico contra os princípios OWASP.
- **Tom dos commits/PRs**: as regras de escrita entram em toda sessão. Peça um commit ou PR normalmente — sai sem emoji e sem texto inflado.
