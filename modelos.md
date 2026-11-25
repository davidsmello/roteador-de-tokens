# Registro de modelos (Model Registry)

Camada independente da política de roteamento. A `SKILL.md` fala em **aliases operacionais** (`haiku`, `sonnet`, `opus`, `fable`); este arquivo diz o que cada alias é hoje. Quando sair modelo novo ou o preço mudar, **só este arquivo é atualizado**, a política não muda.

Regras:
- O alias é o que vale no roteamento e no parâmetro `model` da ferramenta Agent. O identificador de API só vale em chamadas diretas à API.
- Nome comercial não é identificador de API. Só use o ID da coluna "ID da API" depois de confirmado aqui ou na Models API (`GET /v1/models`).
- Antes de depender de um preço ou capacidade, confira a coluna "Verificado em". Dado com mais de 30 dias ou marcado "não verificado" deve ser reconfirmado antes de embasar decisão de custo.

| Alias | ID da API | Versão | Entrada / saída (US$ por 1M tokens) | Cache (leitura) | Janela de contexto | Saída máxima | Capacidades | Na ferramenta Agent (`model`) | Verificado em |
|---|---|---|---|---|---|---|---|---|---|
| `haiku` | `claude-haiku-5-5` | 5.5 | 0,10 / 0,50 (prompts até 100K; acima disso 0,50 / 2,50) | não verificado | 1M | 128K | raciocínio adaptativo, esforço `low` a `max` (padrão `medium`), tarefas de baixa complexidade em volume | `haiku` | 2026-11-24 |
| `sonnet` | `claude-sonnet-5-5` | 5.5 | 2,00 / 10,00 | 0,10 | 1M | 128K | raciocínio adaptativo, esforço `low` a `max` (padrão `high`), código, agentes, trabalho profissional | `sonnet` | 2026-11-24 |
| `opus` | `claude-opus-5-5` | 5.5 | 4,00 / 20,00 | 0,20 | 1M | 128K | raciocínio adaptativo sempre ligado, esforço `low` a `max` (padrão `medium`), arquitetura e tarefas complexas | `opus` | 2026-11-24 |
| `fable` (nível 4) | `claude-fable-5-2` | 5.2 | 8,00 / 40,00 | 0,20 | 1M | 128K | raciocínio sempre ligado, tarefas de raciocínio extremo, só com OK do David | `fable` (exige autorização, seção 2) | 2026-11-24 |

Observações:
- Fonte dos preços, IDs, contexto e capacidades: tabela de modelos do skill `claude-api` (cache de 2026-11-03), consultada em 2026-11-24. O preço de cache do Haiku 5.5 não consta nessa fonte.
- A coluna "Na ferramenta Agent" vem do esquema da própria ferramenta nesta sessão, que aceita `haiku`, `sonnet`, `opus` e `fable`. Se a ferramenta deixar de aceitar um alias, aplique a política de fallback (seção 5).
- Em Bedrock, Vertex e Foundry os IDs e preços podem diferir. Este registro vale para a API da Anthropic.
- Fable 5.2 e o Mythos 5.2 exigem retenção de dados de 30 dias; contas sem isso recebem erro. Mais um motivo para o nível 4 ser decisão consciente.

## Limiares de saúde da sessão (estimativas)

Usados pela seção 8 da `SKILL.md`. São estimativas, não medições: calibre pelo relatório de economia de tokens.

| Alias | Turnos (aviso amarelo) | Horas de trabalho pesado (aviso amarelo) |
|---|---|---|
| `haiku` | ~150 | ~2 a 3 h |
| `sonnet` | ~100 | ~1,5 a 2 h |
| `opus` | ~70 | ~1 a 1,5 h |
