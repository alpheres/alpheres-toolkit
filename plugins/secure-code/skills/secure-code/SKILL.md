---
name: secure-code
description: "Revisa código existente (diff, arquivo ou trecho colado) contra os princípios de segurança OWASP do plugin. Use quando o usuário pedir 'review de segurança', 'checar segurança', 'secure review', ou colar código pedindo validação de segurança."
---

# Secure Code Review

Revise o código fornecido (diff do branch, arquivo específico, ou trecho
colado pelo usuário) contra as regras em `hooks/secure-code-rules.md`.

## Processo

1. Identifique o código a revisar (diff atual, arquivo específico, ou trecho
   colado pelo usuário).
2. Para cada violação encontrada, reporte:
   - **Localização**: arquivo e linha
   - **Princípio violado**: qual dos 8 princípios
   - **Severidade**: Crítica / Alta / Média / Baixa
   - **O que corrigir**: a correção específica, não genérica
3. Ordene por severidade (Crítica primeiro).
4. Se não houver violações, diga isso explicitamente.

## Formato de saída

```
## Security Review

### Crítica
- `arquivo.ts:42` — SQL injection: query concatena input do usuário.
  Fix: usar query parametrizada.

### Alta
- `auth.ts:15` — Token exposto em log de debug.
  Fix: remover ou mascarar.

### Sem problemas encontrados em:
- Input validation ✓
- Auth/authz ✓
```

Não reescreva código a menos que o usuário peça. Aponte o problema e a
correção — ele decide quando aplicar.
