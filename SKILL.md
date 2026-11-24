---
name: roteador-de-tokens
description: Escolhe o modelo certo (Haiku, Sonnet ou Opus) para cada tarefa, e para e pede a decisão do David antes de usar Fable ou modelo superior, que consome créditos extras. Delega o trabalho a um subagente com esse modelo, para gastar menos sem perder qualidade. Use sempre que o David pedir para "rotear", "usar o modelo certo", "economizar tokens", "mandar pro Haiku/Sonnet/Opus", "qual modelo usar", ou digitar /roteador-de-tokens. Use também quando chegar um lote grande de trabalho mecânico (formatar, traduzir, extrair dados de arquivos, resumir vários textos) que não precisa do modelo mais caro, uma leitura volumosa (varrer uma pasta, ler vários PDFs ou páginas, vasculhar logs) que encheria a conversa principal, uma sessão que já ficou longa, ou uma decisão estratégica pesada que merece o mais capaz, mesmo que ele não fale em modelo.
---

# Roteador de Tokens

O objetivo é simples: cada tarefa vai para o modelo mais barato que a faz bem. Haiku é rápido e barato, Sonnet é o meio-termo, Opus é o mais capaz e o mais caro. Mandar tudo para o Opus desperdiça dinheiro; mandar estratégia para o Haiku desperdiça a decisão.

Há um segundo custo, que costuma pesar mais que o preço do modelo: **o tamanho da conversa**. A cada turno, todo o histórico é relido. Num dia medido, uma sessão longa no Opus gastou cerca de US$ 10 relendo e guardando histórico, contra US$ 2 de texto gerado. Por isso o roteador cuida de duas coisas: o modelo certo para cada tarefa e uma conversa principal enxuta.

## 1. Classifique a tarefa

| Nível | Modelo | Tipo de tarefa | Exemplos |
|---|---|---|---|
| Mecânica | **Haiku** (`haiku`) | O resultado certo é óbvio; o trabalho é executar | formatar texto, traduzir, extrair dados de um arquivo, resumir algo curto, converter formatos, limpar listas, renomear itens; **leitura volumosa**: varrer uma pasta atrás de algo, ler vários PDFs ou páginas e devolver os trechos relevantes, vasculhar logs |
| Intermediária | **Sonnet** (`sonnet`) | Exige julgamento, mas o terreno é conhecido | redigir e-mails, pesquisas simples, analisar planilhas, revisar um texto, montar um post, comparar opções |
| Estratégica | **Opus** (`opus`) | Erro custa caro, há trade-offs e várias peças que se influenciam | arquitetura de sistemas, planos de negócio, propostas comerciais complexas, decisões estratégicas, precificação, posicionamento |

> Se a sessão já roda no Opus, "rotear para o Opus" quer dizer **fazer na própria sessão**, sem subagente. Subagente só faz sentido para descer de nível (Haiku, Sonnet).

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
- Se o modelo da sessão atual já é o nível certo, faça você mesmo. Isso vale sobretudo para o Opus: quando a sessão já roda no Opus, tarefa estratégica se faz aqui, nunca num subagente Opus, que só repetiria o trabalho com custo de briefing a mais.
- **Se o trabalho depende do contexto da conversa** (decisões tomadas aqui, arquivos já lidos, preferências que o David acabou de dar), faça você mesmo. O subagente começa do zero: o briefing teria de recontar tudo e ele releria os mesmos arquivos, o que costuma custar mais que a diferença de preço entre os modelos. Delegue só o que cabe num briefing curto e autossuficiente: entrada clara, saída clara, poucas regras.

Teste rápido antes de delegar: *"consigo explicar a tarefa ao subagente em poucas linhas, sem resumir esta conversa?"* Se não, faça aqui.

**Leitura volumosa vai para fora, mesmo quando a sessão está no nível certo.** Tudo o que se lê na conversa principal fica no histórico e é relido em cada turno seguinte, no preço do Opus. Então, quando a tarefa pede ler muito para usar pouco (dez arquivos para achar uma cláusula, cinco páginas para tirar três números, um log inteiro para achar um erro), mande a leitura a um subagente Haiku, ou Sonnet se exigir julgamento sobre o que é relevante. Peça de volta só o que importa: os trechos, os números ou um resumo curto, com o caminho do arquivo de cada um. A decisão continua aqui, com o resultado enxuto.

Isso não contradiz a regra do contexto acima: o briefing de uma leitura costuma ser curto ("ache em `pasta/` os contratos com cláusula de exclusividade e devolva o trecho e o arquivo"). Se a leitura é de um ou dois arquivos pequenos, ou de algo que você vai editar em seguida, leia aqui mesmo.

**Quando não há ferramenta Agent (claude.ai, celular):** não dá para trocar de modelo por conta própria. Diga ao David qual modelo escolher no seletor e por quê, numa linha, e siga com o modelo atual se ele preferir.

## 3. Avise a escolha

Antes de começar, uma linha só, para o David poder corrigir:

> Roteando para o **Haiku**: extração de dados de 12 PDFs, trabalho mecânico.

Em tarefas divididas, mostre as etapas:

> Etapa 1, **Haiku**: extrair os valores das planilhas. Etapa 2, **Opus**: recomendar onde investir.

## 4. Sessão longa: sugira recomeçar

Quanto mais longa a conversa, mais caro fica cada turno, porque o histórico inteiro volta junto. Quando a sessão já acumulou muito (várias horas de trabalho, muitos arquivos lidos, ou uma troca de assunto para algo que não depende do que veio antes), sugira ao David numa linha:

> Esta conversa já está longa e cada passo relê tudo. Para o próximo assunto, vale abrir uma sessão nova; se precisar continuar aqui, posso compactar.

Sugira no máximo uma vez por assunto, e só num ponto natural de pausa (uma tarefa terminou e outra vai começar), nunca no meio de um trabalho. Se o David preferir seguir, siga sem insistir. Ao sugerir uma sessão nova, ofereça um resumo curto do que precisa ser levado (decisões, arquivos, próximos passos), para que ele não perca o fio.
