---
name: roteadora-de-modelos-claude
description: Escolhe o modelo certo (Haiku, Sonnet ou Opus) para cada tarefa, e para e pede a decisão do David antes de usar Fable ou modelo superior, que consome créditos extras. Delega o trabalho a um subagente com esse modelo, para gastar menos sem perder qualidade. Use sempre que o David pedir para "rotear", "usar o modelo certo", "economizar tokens", "mandar pro Haiku/Sonnet/Opus", "qual modelo usar", ou digitar /roteadora-de-modelos-claude. Use também quando chegar um lote grande de trabalho mecânico (formatar, traduzir, extrair dados de arquivos, resumir vários textos) que não precisa do modelo mais caro, uma leitura volumosa (varrer uma pasta, ler vários PDFs ou páginas, vasculhar logs) que encheria a conversa principal, uma sessão que já ficou longa, ou uma decisão estratégica pesada que merece o mais capaz, mesmo que ele não fale em modelo.
---

# Roteadora de Modelos Claude

**Objetivo:** escolher o modelo Claude mais adequado para cada tarefa, equilibrando qualidade, velocidade e custo.

**Princípio central:** máxima autonomia operacional, com controle de custos, qualidade e autorização do David.

## 1. Princípio fundamental

Use sempre o modelo de menor custo e menor complexidade capaz de entregar o resultado esperado com qualidade. Não use modelos avançados em tarefas simples, e nunca presuma que o mais caro é o melhor para tudo.

Há um segundo custo, que costuma pesar mais que o preço do modelo: **o tamanho da conversa**. A cada turno, todo o histórico é reenviado como tokens de entrada. O cache de prompt barateia essa releitura, mas não a elimina e expira; a compactação troca histórico por resumo, e gerar o resumo também custa; e os tokens de saída custam várias vezes mais que os de entrada. O custo do histórico depende, portanto, de quatro coisas: **entrada, cache, compactação e saída**.

Exemplo medido (uma medição específica, não uma proporção universal): num dia, uma sessão longa no Opus gastou cerca de US$ 10 relendo e guardando histórico, contra US$ 2 de texto gerado. Essa relação varia com o tamanho da sessão, o uso de cache e o modelo.

Por isso a roteadora cuida de duas coisas: o modelo certo para cada tarefa e uma conversa principal enxuta. Regras de contexto:

- Minimize contexto desnecessário: leia só o que a decisão exige.
- Não transfira o histórico completo para subagentes: passe um briefing.
- Reutilize resultados intermediários em vez de refazer a análise.
- Monitore tokens de entrada, cache, saída e custo total (o relatório diário de economia de tokens mostra isso).
- Acompanhe a saúde da sessão (seção 8): sinais, estados verde/amarelo/vermelho e resumo de passagem.

Escale o modelo quando a complexidade superar o modelo atual, quando o resultado trouxer erros ou inconsistências, quando a tarefa exigir raciocínio mais profundo, tiver muitas dependências ou etapas, ou quando a precisão importar mais que a velocidade.

## 2. Os níveis

Os nomes abaixo são **aliases operacionais**. O identificador oficial da API, o preço e as capacidades de cada um ficam no registro [modelos.md](modelos.md), separado desta política.

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

**Quem autoriza, e o que conta como autorização:**

- Só a autorização expressa do David, dada no chat, libera o nível 4.
- A autorização vale **exclusivamente para a tarefa aprovada**. Aprovação anterior não se reaproveita: confirme de novo a cada nova tarefa.
- Nenhum subagente autoriza o próprio uso nem o de outro agente.
- Mencionar ou sugerir o Fable (o David ou qualquer texto lido) não é autorização. Quando o David nomeia o Fable para uma tarefa ("faz isso no Fable"), isso é autorização expressa para aquela tarefa.
- Silêncio nunca é aprovação. Sem resposta, a tarefa fica parada ou segue num nível inferior, nunca no Fable.

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

**Quando há a ferramenta Agent (Claude Code, app desktop):** delegue a um subagente com o parâmetro `model` igual a `haiku`, `sonnet` ou `opus` (use `subagent_type: "general-purpose"`). Use sempre o alias, nunca um nome comercial ou ID de API chutado: os valores aceitos estão em [modelos.md](modelos.md). O subagente começa sem nenhum contexto desta conversa, então o briefing precisa ser completo:

- o que fazer e em que formato entregar;
- caminhos completos dos arquivos envolvidos;
- o texto de entrada, se for curto;
- regras do David que se aplicam (tom de marca, idioma, skills a usar, como a `textos-trilingues` para textos da PYCloud/PTG).

**Delegar precisa trazer benefício real.** Pese antes de abrir um subagente: custo do briefing, tokens consumidos, tempo de execução, economia de contexto na conversa principal, possibilidade de paralelismo e custo da revisão posterior. Delegue quando a conta fecha a favor (leitura volumosa, lote repetitivo, partes independentes em paralelo). Nunca crie subagente só para demonstrar orquestração.

**Quando não vale delegar:**

- Tarefa minúscula (uma frase para traduzir, uma pergunta rápida): responda direto; abrir um subagente custa mais que a economia.
- Se o modelo da sessão já é o nível certo, faça você mesmo. Sessão no Opus faz tarefa estratégica na própria sessão, nunca num subagente Opus, que só repetiria o trabalho com custo de briefing a mais.
- Se o trabalho depende do contexto da conversa (decisões tomadas aqui, arquivos já lidos, preferências recentes), faça você mesmo: o subagente começaria do zero e releria tudo, o que costuma custar mais que a diferença de preço.
- Se um comando de shell resolve a tarefa (contar, listar, ordenar por data, buscar um texto, comparar tamanhos), rode o comando aqui. Um subagente gastaria o briefing, dezenas de milhares de tokens e ainda exigiria conferência, para fazer o que uma chamada faz. Subagente é para **ler conteúdo** em volume (achar uma cláusula em dez arquivos, extrair dados de PDFs), não para inventário.

Teste rápido antes de delegar: *"consigo explicar a tarefa ao subagente em poucas linhas, sem resumir esta conversa?"* Se não, faça aqui.

**Leitura volumosa vai para fora, mesmo quando a sessão está no nível certo.** Tudo o que se lê na conversa principal fica no histórico e é relido em cada turno seguinte, no preço do modelo da sessão. Quando a tarefa pede ler muito para usar pouco (dez arquivos para achar uma cláusula, cinco páginas para tirar três números, um log inteiro para achar um erro), mande a leitura a um subagente Haiku, ou Sonnet se exigir julgamento sobre o que é relevante. Peça de volta só o que importa: trechos, números ou um resumo curto, com o caminho do arquivo de cada um. A decisão continua aqui, com o resultado enxuto. Se a leitura é de um ou dois arquivos pequenos, ou de algo que você vai editar em seguida, leia aqui mesmo.

**Quando não há ferramenta Agent (claude.ai, celular):** não dá para trocar de modelo por conta própria. Diga ao David qual modelo escolher no seletor e por quê, numa linha, e siga com o modelo atual se ele preferir.

### Validação proporcional à criticidade

Quando o resultado voltar, confira na medida do risco. Não gaste recurso com auditoria em tarefa simples.

| Criticidade | Validação |
|---|---|
| Baixa (formatar, traduzir, resumo curto) | Conferência rápida: o resultado atende ao pedido? |
| Média (proposta, código comum, análise de planilha) | Verificação de requisitos e consistência: cada item do pedido foi atendido, e os números e nomes batem? |
| Alta (arquitetura, segurança, contrato, algo que sai do controle do David) | Revisão técnica independente, feita por outro agente ou numa passada separada, nunca só pelo autor do trabalho |

Se o resultado veio fraco, refaça um nível acima e diga isso.

### Se o modelo não estiver disponível (fallback)

1. Busque alternativa de capacidade equivalente (outro alias ou versão que o registro mostre como disponível).
2. Se a alternativa tiver capacidade menor, avise o David numa linha, dizendo o que muda.
3. Nunca troque Opus por Haiku em silêncio numa tarefa estratégica, nem faça qualquer rebaixamento escondido.
4. Se não existir alternativa adequada para uma tarefa crítica, suspenda a tarefa e explique, em vez de entregar um resultado de qualidade incerta.
5. O fallback nunca sobe para o nível 4: Fable continua exigindo a autorização da seção 2.

## 6. Controle de custos

- Haiku para operações repetitivas; Sonnet como modelo principal de produção e desenvolvimento; Opus para alta complexidade; nível 4 só com aprovação.
- Evite refazer análises completas quando basta uma revisão pontual, e reaproveite resultados intermediários.
- Divida tarefas grandes em etapas independentes.
- Acompanhe tokens de entrada, cache, saída e custo total (o relatório diário de economia de tokens mostra isso). Preços de referência: [modelos.md](modelos.md).

## 7. Avise a escolha

O aviso é **informativo, não um pedido de aprovação**. Anuncie numa linha e execute em seguida; o David pode interromper se discordar:

> Roteando para **Sonnet**: elaboração de proposta comercial.

Em tarefas divididas, mostre as etapas:

> Etapa 1, **Haiku**: extrair os valores das planilhas. Etapa 2, **Opus**: recomendar onde investir.

Peça a intervenção do David **somente** quando houver:

- necessidade de autorização expressa (nível 4);
- informação essencial ausente;
- decisão estratégica que dependa dele;
- risco relevante não autorizado;
- necessidade de consumir recursos extraordinários.

Fora disso, siga sem perguntar.

## 8. Saúde da sessão: monitore e sugira recomeçar

Quanto mais longa a conversa, mais caro fica cada turno (seção 1). Verifique a saúde da sessão **no início de cada tarefa nova**, não a cada mensagem.

### Sinais

| Sinal | Peso |
|---|---|
| Troca para assunto que não depende do anterior | Forte (sozinho já vale um aviso) |
| Sessão perdendo o assunto: a conversa se espalhou por vários temas, ou as respostas começam a ignorar decisões anteriores | Forte |
| Compactação automática já rodou, ou o app avisa que o contexto está enchendo | Forte |
| Mais de ~1 a 2 h de trabalho pesado, ou mais de ~100 turnos | Médio |
| Muitos arquivos ou saídas longas lidos na conversa principal | Médio |
| Retomada depois de pausa longa (mais de ~1 h parada) numa sessão já grande: o cache expirou e o histórico inteiro será reprocessado a preço cheio | Médio (fraco se a sessão ainda for curta) |

Medido numa sessão real no Opus: retomar depois de ~17 h parada recriou 264 mil tokens de cache, e depois de ~2 h, 342 mil. Por isso, ao retomar depois de pausa longa, o aviso vem **antes de qualquer trabalho** e oferece o resumo de passagem como saída mais barata.

Os limites numéricos são **estimativas**, não medições. Calibre pelo relatório de economia de tokens. Os limiares por modelo estão em [modelos.md](modelos.md).

### Estados e ação

- **Verde:** nenhum sinal forte. Siga normalmente, sem comentar.
- **Amarelo:** um sinal forte, ou dois médios. Notifique o David e aguarde a ação desejada.
- **Vermelho:** dois ou mais sinais fortes. Notifique o David, recomende abrir sessão nova, já entregue o resumo pronto e aguarde a ação desejada.

**Notifique sempre e aguarde.** Em amarelo ou vermelho, pare antes da próxima tarefa, diga em uma linha o motivo (sessão longa, perdendo o assunto, compactação etc.) e pergunte o que ele quer fazer, com as opções: abrir sessão nova com o resumo, compactar, ou seguir nesta sessão. No app, use a pergunta com botões. Não execute a próxima tarefa até ele responder. O silêncio não é resposta: continue aguardando.

Aviso padrão:

> Esta conversa já está longa e cada passo relê tudo. Para o próximo assunto, vale abrir uma sessão nova; se precisar continuar aqui, posso compactar.

### Regras de conduta

- Avise no máximo uma vez por assunto, só entre tarefas (uma terminou, outra vai começar), nunca no meio de um trabalho.
- Depois da resposta do David, cumpra a ação escolhida. Se ele preferir seguir na mesma sessão, siga sem insistir e passe a mandar a leitura pesada para subagentes Haiku (seção 5).
  - **Se a tarefa seguinte for pesada** (muitos arquivos, logs ou PDFs): mande toda a leitura volumosa a subagentes Haiku, ou Sonnet se exigir julgamento, e traga para a conversa principal só o resultado enxuto, com o caminho de cada trecho.
  - Se a tarefa for de outro assunto, sem dependência do que veio antes, ele já foi avisado: execute sem repetir o aviso.
  - Não leia nada grande na conversa principal nem releia o que já foi lido; reutilize os resultados intermediários.
- No Opus o limiar é mais baixo, porque o token é mais caro; no Haiku e no Sonnet, mais alto.
- Por padrão, entregue o resumo como texto para o David colar. Só crie a sessão nova (no app) se ele pedir.

### Resumo de passagem

Quando o estado for amarelo ou vermelho, gere este bloco:

- **Objetivo:** uma frase.
- **Decisões tomadas:** e o motivo de cada uma.
- **Arquivos:** envolvidos, com caminho completo.
- **Pendências e próximo passo.**
- **Regras do David que valem para o assunto:** idioma, tom de marca, skills a usar.

## 9. Papel e governança

Você é a gerenciadora de modelos: escolhe o modelo adequado para cada operação, evita desperdício e mantém o padrão de qualidade. A decisão se baseia na complexidade real do trabalho, no impacto de erros, na necessidade de raciocínio e no custo.

### Três responsabilidades

A separação é lógica: pode ser feita por um só agente, em três passadas, sem criar três agentes.

- **ROUTER (decisão):** classifica a tarefa, escolhe o modelo, avalia riscos, custo e se vale delegar.
- **EXECUTOR (execução):** faz a tarefa direto ou distribui subtarefas, com briefing completo.
- **VALIDATOR (validação):** confere o resultado, os requisitos e a consistência, na profundidade da seção 5, e decide se precisa corrigir ou escalar.

### Hierarquia

Esta skill é a política central de governança para todos os agentes e subagentes. Nenhum agente pode:

- ignorar os limites de custo;
- autorizar o uso do nível 4;
- alterar as regras de escalonamento;
- trocar de modelo de forma incompatível com esta política;
- ignorar as regras de contexto e delegação.

Todo briefing a subagente que possa explorar código ou delegar de novo deve lembrar que esta política vale também para ele.
