# Roteadora de Modelos Claude

Skill para Claude Code que escolhe o modelo certo para cada tarefa e delega o trabalho a um subagente com esse modelo:

- **Haiku**: tarefas mecânicas (formatar, traduzir, extrair dados, resumir algo curto)
- **Sonnet**: tarefas intermediárias (e-mails, pesquisas simples, análise de planilhas)
- **Opus**: tarefas estratégicas (arquitetura, planos de negócio, propostas complexas)
- **Leitura volumosa** (varrer pastas, ler vários PDFs ou páginas, vasculhar logs): vai para um subagente que devolve só o que importa, para a conversa principal não inchar
- **Sessão longa**: sugere, numa pausa natural, abrir uma sessão nova ou compactar, porque cada turno relê o histórico inteiro
- **Fable ou superior**: nunca usado sem aprovação, porque consome créditos extras. A skill para, explica o motivo e espera a decisão.

A política fica em `SKILL.md`. O registro de modelos (aliases, IDs da API, preços, contexto, data da última verificação) fica em `modelos.md`, separado, e é o único arquivo a atualizar quando sair modelo novo ou mudar o preço.

## Instalação

```bash
git clone https://github.com/davidsmello/roteadora-de-modelos-claude.git ~/.claude/skills/roteadora-de-modelos-claude
```

Depois, chame com `/roteadora-de-modelos-claude` ou peça para "rotear", "usar o modelo certo" ou "economizar tokens".
