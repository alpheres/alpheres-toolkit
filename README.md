# alpheres-toolkit

Coleção de plugins para Claude Code.

## Plugins incluídos

| Plugin | Descrição |
|--------|-----------|
| **rfc-writer** | Escrita e revisão de RFCs / design docs seguindo template fixo |
| **secure-code** | Aplica princípios de segurança (OWASP, auth, injection prevention) em todo código gerado — ativo automaticamente via hook de sessão |

## Instalar (CLI ou VS Code)

Dentro de uma sessão do Claude Code:

```
/plugin install alpheres@alpheres-toolkit
```

## Uso

- **RFC**: peça para escrever ou revisar um RFC — a skill `rfc-writer` ativa automaticamente.
- **Segurança**: as regras de segurança são injetadas em toda sessão. Use `/secure-code` para revisar código específico contra os princípios OWASP.
