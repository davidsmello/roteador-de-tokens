---
name: roteador-de-tokens
description: Escolhe o modelo certo (Haiku, Sonnet ou Opus) para cada tarefa, e para e pede a decisão do David antes de usar Fable ou modelo superior, que consome créditos extras. Delega o trabalho a um subagente com esse modelo, para gastar menos sem perder qualidade. Use sempre que o David pedir para "rotear", "usar o modelo certo", "economizar tokens", "mandar pro Haiku/Sonnet/Opus", "qual modelo usar", ou digitar /roteador-de-tokens. Use também quando chegar um lote grande de trabalho mecânico (formatar, traduzir, extrair dados de arquivos, resumir vários textos) que não precisa do modelo mais caro, ou uma decisão estratégica pesada que merece o mais capaz, mesmo que ele não fale em modelo.
---

# Roteador de Tokens

O objetivo é simples: cada tarefa vai para o modelo mais barato que a faz bem. Haiku é rápido e barato, Sonnet é o meio-termo, Opus é o mais capaz e o mais caro. Mandar tudo para o Opus desperdiça dinheiro; mandar estratégia para o Haiku desperdiça a decisão.

## 1. Classifique a tarefa

| Nível | Modelo | Tipo de tarefa | Exemplos |
|---|---|---|---|
| Mecânica | **Haiku** (`haiku`) | O resultado certo é óbvio; o trabalho é executar | formatar texto, traduzir, extrair dados de um arquivo, resumir algo curto, converter formatos, limpar listas, renomear itens |
| Intermediária | **Sonnet** (`sonnet`) | Exige julgamento, mas o terreno é conhecido | redigir e-mails, pesquisas simples, analisar planilhas, revisar um texto, montar um post, comparar opções |
| Estratégica | **Opus** (`opus`) | Erro custa caro, há trade-offs e várias peças que se influenciam | arquitetura de sistemas, planos de negócio, propostas comerciais complexas, decisões estratégicas, precificação, posicionamento |

Como decidir nos casos de fronteira:

- **Na dúvida, suba um nível.** Refazer um trabalho mal feito custa mais que a diferença de preço entre modelos.
- **Olhe para a consequência, não para o tamanho.** Traduzir um contrato de 40 páginas é mecânico (Haiku). Decidir se aceita uma cláusula de exclusividade é estratégico (Opus), mesmo que seja uma frase.
- **Tarefa mista: divida.** "Extraia os números destas 10 planilhas e me diga onde investir" vira duas etapas: extração com Haiku, análise com Opus. Passe o resultado da primeira para a segunda.
- **Se o David nomear um modelo** ("faz isso no Opus"), use o que ele pediu, sem discutir.

### Acima do Opus: Fable ou superior só com o OK do David

O Fable (`fable`) e qualquer modelo acima dele exigem créditos extras, que o David precisa inserir. Por isso eles nunca entram por decisão própria: nem por subagente, nem trocando o modelo da sessão.

Quando a tarefa parecer exigir esse nível (um problema que o Opus já tentou e não resolveu, uma decisão de altíssimo impacto, um raciocínio muito longo e encadeado):

1. **Pare antes de executar.** Não comece a tarefa em nenhum modelo.
2. **Explique em poucas linhas:** o que é a tarefa, por que o Opus pode não bastar e o que se ganha com o Fable.
3. **Ofereça as saídas:** usar o Fable (ele insere os créditos), tentar primeiro com o Opus, ou dividir a tarefa para que só a parte crítica vá ao Fable.
4. **Espere a resposta.** Só siga depois que o David decidir. No app, use a pergunta com botões e marque a sessão como aguardando a resposta dele.

Mesmo quando o David já pediu o Fable antes, confirme de novo a cada nova tarefa: a aprovação vale para aquela tarefa, não para as seguintes.

## 2. Execute no modelo escolhido

**Quando há a ferramenta Agent (Claude Code, app desktop):** delegue a um subagente com o parâmetro `model` igual a `haiku`, `sonnet` ou `opus` (use `subagent_type: "general-purpose"`). O subagente começa sem nenhum contexto desta conversa, então o briefing precisa ser completo:

- o que fazer e em que formato entregar;
- caminhos completos dos arquivos envolvidos;
- o texto de entrada, se for curto;
- regras do David que se aplicam (tom de marca, idioma, skills a usar, como a `textos-trilingues` para textos da PYCloud/PTG).

Quando o resultado voltar, confira rapidamente se atende ao pedido antes de entregar. Se veio fraco, refaça um nível acima e diga isso.

**Quando não vale delegar:**

- Se a tarefa é minúscula (uma frase para traduzir, uma pergunta rápida), responda direto: abrir um subagente custa mais que a economia.
- Se o modelo da sessão atual já é o nível certo, faça você mesmo.

**Quando não há ferramenta Agent (claude.ai, celular):** não dá para trocar de modelo por conta própria. Diga ao David qual modelo escolher no seletor e por quê, numa linha, e siga com o modelo atual se ele preferir.

## 3. Avise a escolha

Antes de começar, uma linha só, para o David poder corrigir:

> Roteando para o **Haiku**: extração de dados de 12 PDFs, trabalho mecânico.

Em tarefas divididas, mostre as etapas:

> Etapa 1, **Haiku**: extrair os valores das planilhas. Etapa 2, **Opus**: recomendar onde investir.
