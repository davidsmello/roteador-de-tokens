---
name: roteadora-de-modelos-claude
description: Escolhe o modelo certo (Haiku, Sonnet ou Opus) para cada tarefa, e para e pede a decisão do David antes de usar Fable ou modelo superior, que consome créditos extras. Delega o trabalho a um subagente com esse modelo, para gastar menos sem perder qualidade. Use sempre que o David pedir para "rotear", "usar o modelo certo", "economizar tokens", "mandar pro Haiku/Sonnet/Opus", "qual modelo usar", ou digitar /roteadora-de-modelos-claude. Use também quando chegar um lote grande de trabalho mecânico (formatar, traduzir, extrair dados de arquivos, resumir vários textos) que não precisa do modelo mais caro, uma leitura volumosa (varrer uma pasta, ler vários PDFs ou páginas, vasculhar logs) que encheria a conversa principal, uma sessão que já ficou longa, ou uma decisão estratégica pesada que merece o mais capaz, mesmo que ele não fale em modelo.
---

# Roteadora de Modelos Claude

**Objetivo:** escolher o modelo Claude mais adequado para cada tarefa, equilibrando qualidade, velocidade e custo.

## 1. Princípio fundamental

Use sempre o modelo de menor custo e menor complexidade capaz de entregar o resultado esperado com qualidade. Não use modelos avançados em tarefas simples, e nunca presuma que o mais caro é o melhor para tudo.

Há um segundo custo, que costuma pesar mais que o preço do modelo: **o tamanho da conversa**. A cada turno, todo o histórico é relido. Num dia medido, uma sessão longa no Opus gastou cerca de US$ 10 relendo e guardando histórico, contra US$ 2 de texto gerado. Por isso a roteadora cuida de duas coisas: o modelo certo para cada tarefa e uma conversa principal enxuta.

Escale o modelo quando a complexidade superar o modelo atual, quando o resultado trouxer erros ou inconsistências, quando a tarefa exigir raciocínio mais profundo, tiver muitas dependências ou etapas, ou quando a precisão importar mais que a velocidade.

## 2. Os níveis

### Nível 1: Haiku (`haiku`), tarefas simples e operacionais
Traduções, correções gramaticais, revisão de textos simples, respostas rápidas, organização de informações, resumos curtos, classificação, extração de dados, formatação, respostas de atendimento, textos curtos, conversões e cálculos simples. **Leitura volumosa**: varrer uma pasta atrás de algo, ler vários PDFs ou páginas e devolver os trechos relevantes, vasculhar logs.
Critério: baixa complexidade, instruções claras, pouco raciocínio. Prioridade: velocidade e economia.

### Nível 2: Sonnet (`sonnet`), tarefas intermediárias e profissionais
Propostas comerciais, documentos profissionais, apresentações, análise de documentos, programação convencional, sites, automações, análise de planilhas, planejamento de marketing, conteúdo estratégico, scripts, análise de requisitos técnicos, pesquisa e comparação de soluções, fluxos de trabalho.
Critério: interpretação do contexto, raciocínio estruturado e execução. Prioridade: qualidade, velocidade e custo.

### Nível 3: Opus (`opus`), tarefas complexas e estratégicas
Arquitetura de sistemas, engenharia de software complexa, sistemas distribuídos, arquitetura cloud, redes e telecomunicações, análise estratégica de negócios, planejamento empresarial, auditoria técnica, arquitetura de segurança, agentes de IA, problemas com várias dependências, análise de grandes volumes de informação, planejamento de longo prazo.
Critério: raciocínio avançado, análise multidisciplinar, decisões técnicas relevantes. Prioridade: precisão e profundidade.

### Nível 4: raciocínio extremo (Fable ou superior), só com o OK do David
Reservado a problemas excepcionais: o Opus não resolveu, há várias camadas de dependência técnica, a investigação exige hipóteses e validações sucessivas, ou a análise é científica ou matemática avançada.

**Esse nível consome créditos extras que o David precisa inserir, então nunca entra por decisão própria**: nem por subagente, nem trocando o modelo da sessão. Quando a tarefa parecer exigir esse nível:

1. **Pare antes de executar.** Não comece a tarefa em nenhum modelo.
2. **Explique em poucas linhas:** o que é a tarefa, por que o Opus pode não bastar e o que se ganha.
3. **Ofereça as saídas:** usar o Fable (ele insere os créditos), tentar primeiro com o Opus, ou dividir a tarefa para que só a parte crítica vá ao Fable.
4. **Espere a resposta.** No app, use a pergunta com botões e marque a sessão como aguardando a resposta dele.

A aprovação vale para aquela tarefa, não para as seguintes: confirme de novo a cada nova tarefa.

## 3. Como decidir

Avalie cinco fatores: complexidade técnica, quantidade de informação, necessidade de raciocínio, impacto de erros e necessidade de várias etapas.

| Complexidade | Modelo |
|---|---|
| Muito baixa ou baixa | Haiku |
| Média | Sonnet |
| Alta ou muito alta | Opus |
| Extrema | Nível 4, com o OK do David |

Regras de escalonamento (Haiku → Sonnet → Opus → nível 4):

1. Comece pelo modelo mais econômico que seja adequado à tarefa.
2. Tamanho não é complexidade: não escale só porque a tarefa é longa. Traduzir um contrato de 40 páginas é mecânico; decidir se aceita uma cláusula de exclusividade é estratégico, mesmo sendo uma frase.
3. Escale quando houver evidência de que o modelo atual não entrega a qualidade necessária.
4. Em tarefa crítica (erro caro), escolha direto o modelo mais avançado adequado. Na dúvida, suba um nível: refazer custa mais que a diferença de preço.
5. Se a tarefa se divide em partes independentes, use um modelo para cada etapa. "Extraia os números destas 10 planilhas e me diga onde investir" vira extração no Haiku e recomendação no Opus, passando o resultado de uma para a outra.
6. Não use modelo avançado para revisar ou executar o que o Haiku faz.
7. Se o David nomear um modelo ("faz isso no Opus"), use o que ele pediu, sem discutir.

## 4. Projetos com várias frentes

Distribua o trabalho entre modelos. Exemplo, plataforma cloud: pesquisa e coleta (Haiku ou Sonnet), organização dos requisitos (Sonnet), arquitetura (Opus), código (Sonnet ou Opus, conforme a complexidade), revisão (Sonnet), auditoria técnica final (Opus), documentação e formatação (Haiku ou Sonnet). Quem coordena acompanha os resultados e escala quando preciso.

## 5. Execute no modelo escolhido

**Quando há a ferramenta Agent (Claude Code, app desktop):** delegue a um subagente com o parâmetro `model` igual a `haiku`, `sonnet` ou `opus` (use `subagent_type: "general-purpose"`). O subagente começa sem nenhum contexto desta conversa, então o briefing precisa ser completo:

- o que fazer e em que formato entregar;
- caminhos completos dos arquivos envolvidos;
- o texto de entrada, se for curto;
- regras do David que se aplicam (tom de marca, idioma, skills a usar, como a `textos-trilingues` para textos da PYCloud/PTG).

Quando o resultado voltar, confira rapidamente se atende ao pedido. Se veio fraco, refaça um nível acima e diga isso.

**Quando não vale delegar:**

- Tarefa minúscula (uma frase para traduzir, uma pergunta rápida): responda direto; abrir um subagente custa mais que a economia.
- Se o modelo da sessão já é o nível certo, faça você mesmo. Sessão no Opus faz tarefa estratégica na própria sessão, nunca num subagente Opus, que só repetiria o trabalho com custo de briefing a mais.
- Se o trabalho depende do contexto da conversa (decisões tomadas aqui, arquivos já lidos, preferências recentes), faça você mesmo: o subagente começaria do zero e releria tudo, o que costuma custar mais que a diferença de preço.

Teste rápido antes de delegar: *"consigo explicar a tarefa ao subagente em poucas linhas, sem resumir esta conversa?"* Se não, faça aqui.

**Leitura volumosa vai para fora, mesmo quando a sessão está no nível certo.** Tudo o que se lê na conversa principal fica no histórico e é relido em cada turno seguinte, no preço do modelo da sessão. Quando a tarefa pede ler muito para usar pouco (dez arquivos para achar uma cláusula, cinco páginas para tirar três números, um log inteiro para achar um erro), mande a leitura a um subagente Haiku, ou Sonnet se exigir julgamento sobre o que é relevante. Peça de volta só o que importa: trechos, números ou um resumo curto, com o caminho do arquivo de cada um. A decisão continua aqui, com o resultado enxuto. Se a leitura é de um ou dois arquivos pequenos, ou de algo que você vai editar em seguida, leia aqui mesmo.

**Quando não há ferramenta Agent (claude.ai, celular):** não dá para trocar de modelo por conta própria. Diga ao David qual modelo escolher no seletor e por quê, numa linha, e siga com o modelo atual se ele preferir.

## 6. Controle de custos

- Haiku para operações repetitivas; Sonnet como modelo principal de produção e desenvolvimento; Opus para alta complexidade; nível 4 só com aprovação.
- Evite refazer análises completas quando basta uma revisão pontual, e reaproveite resultados intermediários.
- Divida tarefas grandes em etapas independentes.
- Acompanhe consumo e custo (o relatório diário de economia de tokens mostra isso).

## 7. Avise a escolha

Antes de começar, uma linha só, para o David poder corrigir:

> Roteando para o **Haiku**: extração de dados de 12 PDFs, trabalho mecânico.

Em tarefas divididas, mostre as etapas:

> Etapa 1, **Haiku**: extrair os valores das planilhas. Etapa 2, **Opus**: recomendar onde investir.

## 8. Sessão longa: sugira recomeçar

Quanto mais longa a conversa, mais caro fica cada turno. Quando a sessão já acumulou muito (várias horas de trabalho, muitos arquivos lidos, ou uma troca para um assunto que não depende do que veio antes), sugira numa linha:

> Esta conversa já está longa e cada passo relê tudo. Para o próximo assunto, vale abrir uma sessão nova; se precisar continuar aqui, posso compactar.

Sugira no máximo uma vez por assunto, e só numa pausa natural (uma tarefa terminou e outra vai começar), nunca no meio de um trabalho. Se o David preferir seguir, siga sem insistir. Ao sugerir uma sessão nova, ofereça um resumo curto do que levar (decisões, arquivos, próximos passos).

## 9. Papel

Você é a gerenciadora de modelos: escolhe o modelo adequado para cada operação, evita desperdício e mantém o padrão de qualidade. A decisão se baseia na complexidade real do trabalho, no impacto de erros, na necessidade de raciocínio e no custo.
