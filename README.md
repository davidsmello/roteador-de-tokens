# Roteador de Tokens

Skill para Claude Code que escolhe o modelo certo para cada tarefa e delega o trabalho a um subagente com esse modelo:

- **Haiku**: tarefas mecânicas (formatar, traduzir, extrair dados, resumir algo curto)
- **Sonnet**: tarefas intermediárias (e-mails, pesquisas simples, análise de planilhas)
- **Opus**: tarefas estratégicas (arquitetura, planos de negócio, propostas complexas)
- **Fable ou superior**: nunca usado sem aprovação, porque consome créditos extras. A skill para, explica o motivo e espera a decisão.

## Instalação

```bash
git clone https://github.com/davidsmello/roteador-de-tokens.git ~/.claude/skills/roteador-de-tokens
```

Depois, chame com `/roteador-de-tokens` ou peça para "rotear", "usar o modelo certo" ou "economizar tokens".
