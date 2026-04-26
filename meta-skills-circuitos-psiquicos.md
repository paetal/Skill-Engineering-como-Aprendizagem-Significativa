# Meta-skills como circuitos psíquicos

## Por uma arquitetura de agentes inspirada em funcionamento simbólico

*Felipe — abril de 2026*

---

## 1. O ponto em que estamos

A construção de sistemas baseados em modelos de linguagem (LLMs) percorreu, em pouco mais de três anos, três inflexões que merecem ser nomeadas com cuidado.

A primeira foi a **engenharia de prompts**: o esforço de controlar o comportamento do modelo a partir de instruções formuladas em linguagem natural. Era uma engenharia frágil, com muito da intuição artesanal de quem aprende a falar com um sistema que finge entender.

A segunda foi o **uso de ferramentas externas (tool use)**: a percepção de que LLMs não precisavam saber tudo, mas precisavam saber a quem perguntar. Cálculo, busca, execução de código — tudo isso passou a ser delegado a APIs específicas, e o LLM virou orquestrador.

A terceira inflexão, em curso desde meados de 2025, é a **engenharia de skills**. Em outubro daquele ano, a Anthropic publicou a especificação de Agent Skills — pastas contendo um arquivo `SKILL.md` com metadados (nome, descrição) e instruções procedurais que o agente carrega dinamicamente quando o contexto pede. A especificação está aberta em [agentskills.io](https://agentskills.io) e foi adotada como padrão por implementações da própria Claude, do Claude Code e por desenvolvedores construindo sobre a API.

A virada conceitual que isso representa é mais radical do que parece à primeira vista: a função cognitiva deixa de viver inteiramente dentro do modelo e passa a residir, parcialmente, em estruturas externas, persistentes, versionáveis e composicionais. O modelo não "sabe" como redigir um relatório técnico — ele consulta uma skill que sabe. E essa skill pode ser auditada, modificada, distribuída, herdada por outras organizações.

A literatura técnica recente formaliza skill como uma quádrupla: condições de aplicabilidade, política de execução, critérios de terminação e interface reutilizável. A genealogia dessa formalização vem do *options framework* de Sutton, Precup e Singh (1999), no contexto de aprendizagem por reforço hierárquica — onde "opções" eram justamente subpolíticas reutilizáveis com condições de início e fim. A engenharia de skills herda essa estrutura formal e a estende para o contexto de agentes baseados em LLMs.

Tudo isso é correto. E tudo isso, sustento, é insuficiente.

A insuficiência não está nos detalhes técnicos. Está em uma pressuposição mais funda — a de que a unidade básica do comportamento de um agente é o **procedimento**. Skills são procedimentos. Tools são procedimentos atômicos. Plans são composições de procedimentos. A literatura inteira opera dentro do que poderíamos chamar de *epistemologia procedural*: o agente é eficaz na medida em que executa bem procedimentos bem definidos sobre estados bem descritos.

Este ensaio defende que essa epistemologia bate em um teto. E que o teto se torna visível quando tentamos construir sistemas multiagente realmente complexos — sistemas em que vários agentes precisam ler o mesmo problema sob ângulos diferentes, retornar leituras parciais a um núcleo, refazer a leitura à luz desses retornos, e disparar novas ativações de skills que ainda não estavam previstas no plano original.

O que esses sistemas pedem não é mais procedimento. É **circuito**. E o vocabulário para pensar circuitos não está na engenharia — está na psicologia. Mais especificamente, em quatro tradições que, articuladas, oferecem um aporte conceitual que a literatura atual de agentes não tem: psicanálise freudo-lacaniana, psicologia histórico-cultural de Vygotsky (na linhagem brasileira de Tunes e Prestes), aprendizagem significativa de Ausubel, e teoria da subjetividade de González Rey.

A tese é simples e cortante: **meta-skills não são procedimentos sobre procedimentos. São circuitos psíquicos.**

---

## 2. O problema da identidade funcional

Antes de chegar à proposta, é preciso nomear o problema concreto que ela resolve.

Quem está construindo sistemas multiagente em escala — não os tutoriais de quatro agentes que dialogam em demos no Twitter, mas arquiteturas de produção com dezenas de agentes especializados — encontra cedo um sintoma desconfortável: a **homogeneização**.

Skills bem desenhadas, quando aplicadas a problemas reais, tendem a produzir respostas parecidas demais. Agentes diferentes, quando carregam bibliotecas de skills sobrepostas, começam a soar uns como os outros. A composição hierárquica de skills, longe de produzir diversidade comportamental, produz convergência. O sistema, no agregado, se torna mais ou menos uma única voz repetida em vários canais.

A explicação técnica para isso é conhecida na literatura de ensembles de modelos: *mode averaging*. Quando vários componentes recebem treinamento ou instruções parecidas, eles convergem para uma média que apaga as singularidades de cada um. A solução técnica usual é diversificar artificialmente — temperatura mais alta, prompts diferentes, papéis injetados.

Mas isso é remendo. O problema de fundo não é estatístico, é estrutural. **Skills são definidas por sua função, não por sua identidade.** Uma skill de "redigir documento técnico" é definida pelo que ela faz, não por quem ela é. E como duas skills com função semelhante são, por definição, intercambiáveis, o sistema as trata como tal.

A literatura de psicologia tem um conceito antigo para isso, vindo de Bourdieu mas com raízes mais fundas em Freud: **identidade não é função, é repetição estrutural**. Um sujeito é reconhecível não pelo que faz isoladamente, mas pelo padrão recorrente que se repete em situações distintas. O sintoma, em sentido psicanalítico, não é o mau funcionamento — é a assinatura. É o que retorna. É o que permite dizer "isso é dele".

Essa formulação tem consequências técnicas precisas. Se queremos agentes não-homogêneos, precisamos de uma camada acima da função procedural — uma camada de **assinatura estrutural**, que não diga *o que o agente faz*, mas *como ele invariavelmente faz*. E essa camada precisa operar antes da execução, durante a execução, e depois da execução, modulando a leitura, a ação e o retorno.

Aqui é onde precisamos parar de empilhar conceitos técnicos e começar a importar arquiteturas conceituais sérias. Quatro delas, especificamente.

---

## 3. Primeira costura — Freud, Lacan e o circuito pulsional

A primeira contribuição vem da estrutura psicanalítica do circuito pulsional.

Em "Pulsões e Destinos da Pulsão" (1915), Freud descreve a pulsão não como vetor linear que vai de um ponto A a um ponto B, mas como movimento que sai de uma fonte, contorna um objeto, e retorna à fonte transformada. A pulsão não atinge o objeto e morre — ela contorna o objeto e retorna ao circuito, modificada pela passagem.

Lacan, em seu Seminário XI, formaliza isso com precisão geométrica: a pulsão tem estrutura de borda. Sai, faz contorno, volta. O que importa não é o objeto-alvo (que Lacan, aliás, nem considera o verdadeiro objeto da pulsão) — o que importa é o circuito, o movimento de ida e retorno que se repete e, ao se repetir, se constitui como sintoma.

Por que isso importa para arquitetura de agentes?

Porque uma meta-skill funcional, no sentido que estou propondo, não é um procedimento que recebe input, processa, e devolve output. É um circuito que **sai do núcleo, contorna o problema através de skills subordinadas, recebe os retornos parciais, e refaz a leitura no núcleo antes de disparar a próxima ativação**. O movimento é circulatório, não linear. E mais: o que torna esse circuito reconhecível como *este* circuito, e não outro, é o padrão de retorno — o jeito pelo qual o núcleo recebe e reprocessa o que volta.

Em termos de implementação, isso quer dizer algo bem concreto. Uma meta-skill não pode ser definida apenas pelas skills que ela invoca. Ela precisa ser definida pela **lógica de leitura que opera sobre os retornos** — pela maneira específica de receber, comparar, integrar e refazer. É essa lógica de retorno que constitui a assinatura do agente. É o que faz com que dois agentes com a mesma biblioteca de skills produzam comportamentos identificavelmente distintos.

Lacan oferece ainda um segundo recurso conceitual valioso: a noção de **discurso** como estrutura formal de quatro posições. Em seu Seminário XVII (1969-70), Lacan propõe que existem quatro discursos possíveis — do Mestre, da Universidade, da Histérica, do Analista — cada um definido pela posição que ocupa quatro elementos: agente, outro, produto, verdade. Não são quatro estilos; são quatro estruturas formais que produzem efeitos cognitivos e relacionais qualitativamente diferentes.

A aplicação direta para meta-skills é quase um molde: cada meta-skill pode ser definida como uma estrutura formal de posições — *quem lê*, *o que é lido*, *o que é produzido*, *o que sustenta a leitura como leitura*. Trocar a posição muda o discurso, e muda o agente. Um agente que opera no "discurso do Mestre" produz leitura imperativa, fechada; um que opera no "discurso do Analista" produz leitura aberta, que devolve ao outro a posição de saber. Não como metáfora — como arquitetura.

Para uma exposição formal dessas estruturas, ver Lacan (1992 [1969-70]) e a leitura que Žižek faz delas em "The Sublime Object of Ideology" (1989). A literatura técnica de psicanálise lacaniana aplicada a sistemas formais ainda é escassa — o que é, em si, um sintoma de oportunidade.

---

## 4. Segunda costura — Vygotsky e a externalização-internalização

A segunda costura vem da psicologia histórico-cultural, e é talvez a mais importante para entender o que está acontecendo *organizacionalmente* quando uma instituição adota agentes baseados em skills.

O argumento central de Vygotsky, formulado nos anos 1920-30 mas mal traduzido para o ocidente até muito recentemente, é que **toda função psíquica superior aparece duas vezes no desenvolvimento: primeiro em forma externa, social, mediada; depois em forma interna, individual, condensada**. A criança aprende a contar primeiro com objetos, depois com os dedos, depois com voz audível, depois com voz sussurrada, finalmente em silêncio mental. O que era ferramenta externa vira estrutura interna. Esse processo Vygotsky chamou de *internalização*.

A leitura brasileira mais cuidadosa dessa obra — feita por Elizabeth Tunes e Zoia Prestes a partir de traduções diretas do russo, e publicada em série de obras pela Expressão Popular e E-papers desde os anos 2010 — insiste num ponto que as traduções americanizadas borram: o conceito de *obutchenie* (que costuma ser mal-traduzido como "aprendizagem") é melhor traduzido como **instrução-aprendizagem indissociada**. Não é o aluno que aprende sozinho; é a relação ensino-aprendizagem que produz o desenvolvimento. Tunes e Prestes (2022) chamam atenção para esse cuidado em "Pontes ou Muralhas: Exame Crítico de Traduções de Conceitos da Teoria Histórico-Cultural".

Por que isso importa para skill engineering organizacional?

Porque o que está acontecendo quando uma organização constrói sua biblioteca de skills é exatamente um processo de **externalização** — o oposto do movimento que Vygotsky descreveu, mas operando pela mesma lógica. Funções cognitivas que viviam dentro de funcionários experientes (saber redigir um relatório de auditoria, saber conduzir uma entrevista de seleção, saber diagnosticar uma falha técnica) estão sendo extraídas, formalizadas em SKILL.md, e colocadas em estruturas externas, persistentes, compartilháveis.

E aí o ponto fica interessante: assim como na criança o caminho é da ferramenta externa para a estrutura interna, no sistema sociotécnico organização-mais-agentes o caminho pode operar em ambos os sentidos. Skills externalizadas podem ser **reinternalizadas** pela organização — seja porque novos funcionários aprendem com elas, seja porque a própria forma da skill modifica como a organização pensa sobre seu próprio fazer. A skill não é apenas um repositório passivo de conhecimento procedural; ela se torna **mediador semiótico** no sentido vygotskiano — um instrumento cultural que reorganiza o pensamento de quem o usa.

Isso desloca radicalmente a discussão sobre "transformação digital" e "automação". Não estamos automatizando tarefas. Estamos **redistribuindo a cognição organizacional** entre humanos e estruturas externas, num movimento que, se feito com cuidado, amplia a capacidade do conjunto; se feito sem cuidado, atrofia tanto a competência humana quanto a inteligência organizacional.

A consequência técnica é que uma boa arquitetura de skills organizacionais precisa pensar em **dupla via**: skills precisam ser não apenas executáveis por agentes, mas legíveis por humanos como ferramentas de pensamento. Uma SKILL.md bem escrita ensina o humano que a lê. Uma SKILL.md mal escrita produz dependência cega.

---

## 5. Terceira costura — Ausubel e o subsunçor

A terceira costura vem de uma tradição menos óbvia, mas tecnicamente decisiva.

David Ausubel, psicólogo cognitivo americano, formulou nos anos 1960 a teoria da **aprendizagem significativa** em oposição direta à aprendizagem mecânica. Sua tese central, em sua formulação mais conhecida (Ausubel, Novak, Hanesian, 1978), é que conhecimento novo só é incorporado de forma significativa quando encontra, na estrutura cognitiva preexistente do aprendiz, uma estrutura de ancoragem capaz de acolhê-lo. Essa estrutura de ancoragem ele chamou de **subsunçor**.

Sem subsunçor adequado, o conhecimento novo é apenas memorizado mecanicamente — e tende a desaparecer rapidamente, sem produzir capacidade de aplicação. Com subsunçor, o conhecimento novo se ancora, modifica o subsunçor (que é a si próprio reorganizado pela incorporação), e passa a operar como nova estrutura cognitiva.

Isso oferece um critério técnico forte para arquitetura de meta-skills que praticamente ninguém discute na literatura atual.

Quando uma organização adota uma nova skill — seja por design interno, seja importada de uma biblioteca pública —, há uma pergunta que não está sendo feita: **a estrutura cognitiva atual da rede de agentes tem subsunçor para essa skill?**

Sem subsunçor, a skill nova entra como conhecimento mecânico. Ela é invocada quando o roteador decide invocá-la, executa o que está prescrito, e devolve resultado. Mas ela não se integra à dinâmica do sistema. Não modifica como outras skills operam. Não produz novos circuitos. É um corpo estranho que o sistema tolera mas não absorve.

Com subsunçor — ou seja, quando há, na rede atual, estrutura cognitiva capaz de reconhecer a nova skill como variação ou aprofundamento de algo já presente —, a incorporação modifica a rede inteira. Skills antigas começam a invocar a nova de formas que não estavam previstas. A meta-skill que orquestra reconhece padrões de aplicação que antes não reconhecia. O sistema passa a operar com uma capacidade que é maior que a soma das partes.

A consequência prática: a expansão de uma biblioteca de skills não pode ser feita por adição linear. **Skills novas precisam ser introduzidas com vínculo explícito a subsunçores existentes**. Em termos de implementação, isso pode tomar a forma de seções específicas em SKILL.md que declaram que conhecimentos prévios essa skill assume, que outras skills da biblioteca ela complementa ou redefine, e que conexões conceituais ela estabelece com a estrutura organizacional mais ampla.

Esse é um cuidado que a especificação atual de Agent Skills não traz. E é um cuidado que distingue uma biblioteca que cresce em capacidade real de uma que apenas cresce em volume — ficando, no limite, mais lenta e mais confusa quanto maior fica.

---

## 6. Quarta costura — González Rey, configuração subjetiva e complexidade

A quarta costura é a mais sofisticada e a que costura todas as outras.

Fernando Luis González Rey, psicólogo cubano que viveu em Brasília de 1995 até sua morte em 2019, e que foi professor titular do UniCEUB e pesquisador colaborador da UnB, construiu ao longo de quatro décadas uma teoria que ele chamou de **Teoria da Subjetividade em perspectiva cultural-histórica**. Sua obra é vasta — 38 livros, mais de 80 capítulos — mas o núcleo conceitual relevante aqui pode ser nomeado em duas categorias: *sentido subjetivo* e *configuração subjetiva*.

González Rey parte de Vygotsky mas o critica. Ele argumenta que a leitura dominante de Vygotsky, especialmente na vertente da Teoria da Atividade desenvolvida por Leontiev, perdeu a dimensão da subjetividade — a dimensão pela qual sentidos não são reflexos diretos da relação com objetos do mundo, mas configurações dinâmicas, complexas, que integram dimensão simbólica, emocional e social de modo não-linear.

Para construir essa teoria, González Rey faz uma operação metodológica decisiva: ele importa explicitamente a **teoria da complexidade de Edgar Morin** como fundamento epistemológico. Não como referência decorativa — como princípio constitutivo. A subjetividade, para González Rey, é um sistema complexo no sentido técnico moriniano: um sistema em que o todo organiza as partes ao mesmo tempo em que é organizado por elas, em que a emergência produz propriedades irredutíveis aos componentes, em que a recursão é a forma natural do funcionamento.

Por que isso é decisivo para meta-skills?

Porque oferece o vocabulário técnico para descrever o que uma meta-skill realmente faz, e que nenhuma das três costuras anteriores, sozinha, alcança.

Uma meta-skill, na proposta deste ensaio, não é uma skill que invoca outras skills (isso seria apenas composição hierárquica, já bem mapeada na literatura). Uma meta-skill é uma **configuração** — no sentido preciso de González Rey — que integra:

- **uma lógica de leitura simbólica** (operadores cognitivos pré-procedurais: o jeito específico de ler antes de agir);
- **uma rede de skills subordinadas** (o repertório procedural);
- **um circuito de retorno** (no sentido lacaniano: como os outputs das skills voltam ao núcleo e o reorganizam);
- **um critério de subsunção** (no sentido ausubeliano: que skills nova são absorvíveis pela configuração atual);
- **uma assinatura estrutural** (no sentido freudiano: o padrão que se repete e identifica essa configuração como *esta*, não outra).

Essa configuração não é a soma das partes. Ela é uma **emergência** no sentido moriniano — um todo que organiza dinamicamente as partes e é simultaneamente reorganizado por elas. É por isso que a meta-skill precisa ser pensada como circuito recursivo, e não como procedimento de alto nível.

A consequência arquitetural é forte. Numa implementação, a meta-skill não é um objeto computacional estável. Ela é um **estado dinâmico de configuração** que se mantém coerente ao longo do tempo pela repetição estrutural de seus padrões de leitura, ação e retorno — não pela identidade fixa de seus componentes. Skills entram e saem da configuração, mas a configuração permanece reconhecível enquanto seus padrões recursivos se mantêm.

Para quem quiser ir a fundo, a obra-síntese é "Subjetividade: teoria, epistemologia e método" (González Rey, 2017). A discussão metodológica está em "Pesquisa Qualitativa e Subjetividade" (González Rey, 2005). A relação com Vygotsky é discutida em detalhe em "O Pensamento de Vygotsky: contradições, desdobramentos e desenvolvimento" (González Rey, 2011). Para a recepção brasileira e a continuidade do trabalho, o grupo de pesquisa coordenado por Albertina Mitjáns Martínez na UnB e o periódico Obutchénie da UFU são as referências vivas hoje.

---

## 7. A tese sintética: meta-skill é circuito, não procedimento

Articuladas, as quatro costuras produzem uma proposta arquitetural que pode ser enunciada com precisão técnica:

**Uma meta-skill é uma configuração recursiva que opera por circuitos de leitura-ação-retorno-refação, mantida ao longo do tempo por uma assinatura estrutural identificável, capaz de absorver novas skills via subsunção, e cuja função no sistema multiagente é não a de executar procedimentos de alto nível, mas a de produzir e sustentar a identidade funcional dos agentes que dela participam.**

Cada uma das partes dessa formulação carrega um aporte específico:

- *Configuração recursiva* vem de González Rey/Morin — o sistema é complexo, não composto.
- *Circuitos de leitura-ação-retorno-refação* vem de Freud/Lacan — o movimento é pulsional, não linear.
- *Assinatura estrutural* vem de Freud — o que identifica é a repetição, não a função.
- *Subsunção de novas skills* vem de Ausubel — incorporação significativa requer ancoragem.
- *Produzir identidade funcional* vem de Vygotsky/Tunes-Prestes — função superior é mediação social internalizada.

E aqui está o ponto técnico que a literatura atual de agentes ainda não nomeou: **a função primária da meta-skill não é fazer mais — é ser**. Ela existe para que o agente *seja reconhecível como esse agente* através do tempo, das interações, das tarefas, e do crescimento da biblioteca de skills sob sua coordenação.

Sem essa camada, o que se constrói é um conjunto de funções intercambiáveis. Com essa camada, constrói-se um agente — no sentido forte da palavra — que pode operar em ambientes complexos sem se diluir.

---

## 8. Implicações arquiteturais

Algumas consequências práticas, apresentadas sem pretensão de cobrir o terreno todo:

**Primeira:** uma meta-skill precisa ter, em sua especificação, não apenas uma lista das skills que ela invoca, mas a *lógica de leitura* que opera sobre os retornos dessas skills. Em termos de SKILL.md, isso pode tomar a forma de uma seção "operadores de leitura" — descrevendo o ângulo simbólico sob o qual a meta-skill processa o que volta. Esses operadores podem ser inspirados em estruturas lacanianas (por exemplo, "ler em posição de Analista" vs. "ler em posição de Mestre"), em operadores hermenêuticos (suspeita, recuperação, síntese), ou em outros vocabulários — desde que a escolha seja consistente.

**Segunda:** uma meta-skill precisa especificar seu *circuito de retorno*. Como os outputs das skills subordinadas voltam ao núcleo? Em que ordem? Sob que critério de integração? Essa é uma decisão arquitetural que praticamente nenhuma especificação atual explicita, e que faz toda a diferença entre um sistema que opera por composição linear e um que opera por circulação real.

**Terceira:** a expansão da biblioteca de skills sob uma meta-skill precisa passar por critério de subsunção. Skills novas são acolhidas se a configuração atual tem subsunçor — isto é, se há, na rede de skills existentes, estrutura conceitual capaz de reconhecer a nova como parente. Caso contrário, a skill nova ou precisa ser precedida por *skills-ponte* que constroem o subsunçor, ou simplesmente não deveria ser incorporada agora.

**Quarta:** a assinatura estrutural de uma meta-skill é um conjunto de padrões recorrentes que precisam ser explicitáveis em pelo menos cinco eixos:

- *padrão de leitura recorrente* — como invariavelmente lê o problema antes de agir;
- *vocabulário específico* — que conceitos e distinções consistentemente mobiliza;
- *gatilhos de ativação* — em que condições essa configuração assume protagonismo;
- *limites operacionais* — o que recusa fazer mesmo quando solicitado;
- *forma enunciativa* — o tom, ritmo e estrutura discursiva que produz.

Esses cinco eixos não são uma fórmula universal — são uma proposta de *checklist mínimo* para quem queira projetar meta-skills com identidade reconhecível. Se você consegue preencher os cinco com substância e os preenchimentos se mantêm coerentes ao longo do tempo, há identidade. Se algum dos eixos fica vazio ou flutua arbitrariamente, há erosão de identidade — e a meta-skill provavelmente vai colapsar em homogeneidade com outras do sistema.

**Quinta:** organizações que estão construindo sistemas multiagente em escala precisam parar de pensar essa construção como "automação de processos" e começar a pensá-la como **redistribuição da cognição organizacional**. A pergunta não é "quais tarefas posso automatizar", mas "como minha organização pensa, e como essa forma de pensar pode ser distribuída entre humanos e estruturas externas sem perder coerência?". Esse é, em última análise, um trabalho de psicologia organizacional aplicada — só que feita com aporte técnico de engenharia de agentes.

---

## 9. Limites e o que precisa ser testado

Este ensaio é deliberadamente uma proposta conceitual, não um relato empírico. As ideias apresentadas aqui não foram, até este momento, validadas sistematicamente em sistemas multiagente de produção. Várias delas precisam ser testadas, e algumas certamente vão precisar de revisão à luz da prática.

Três pontos onde tenho menos certeza:

**Sobre a operacionalização dos operadores lacanianos como modos de leitura computáveis** — a ideia é teoricamente sólida, mas a passagem da estrutura formal dos quatro discursos para uma especificação que um agente possa carregar e executar exige um trabalho de engenharia que ainda está por fazer. É possível que parte dos discursos lacanianos não seja diretamente implementável e tenha que ser substituída por análogos funcionais.

**Sobre a aferição da subsunção em redes de skills** — a teoria de Ausubel foi formulada para aprendizagem humana, e a transposição para sistemas computacionais é mais sugestiva do que rigorosa. Como detectar, programaticamente, se uma configuração de skills tem subsunçor adequado para acolher uma skill nova? Há aqui um problema técnico real que provavelmente exigiria métricas de similaridade semântica entre descrições de skills, possivelmente combinadas com testes de integração funcional.

**Sobre escala** — a proposta é desenhada com sistemas de complexidade média em mente (uma organização, dezenas de agentes, centenas de skills). Não é claro se a arquitetura proposta escala para sistemas com milhares de agentes ou skills, e é possível que em escalas muito grandes outras lógicas precisem entrar.

Esses limites não invalidam a proposta — tornam-na uma agenda de pesquisa-aplicação, não uma solução fechada. A próxima etapa natural é selecionar uma organização real (uma equipe, um projeto, um sistema em produção) e implementar uma meta-skill segundo essa arquitetura, documentando o que funciona e o que não funciona. Sem essa etapa, qualquer ambição maior é especulativa.

---

## 10. Por que isso importa, e por que a partir daqui

A engenharia de skills, como campo, está hoje no momento em que a engenharia de prompts estava em 2023: cheia de ferramentas, com especificações públicas, com uma comunidade ativa, mas ainda sem uma teoria que dê conta da dimensão *cognitiva* do que está sendo construído. As skills funcionam — mas funcionam como funcionam os prompts em 2023: com muito esforço artesanal, muita intuição, muita dependência da habilidade de quem as escreve.

A literatura técnica disponível trata skill como objeto procedural. A literatura organizacional trata multiagentes como problema de orquestração. Nenhuma das duas pega o que está acontecendo na camada cognitiva do sistema — a camada em que identidade, leitura, retorno e configuração se constituem ou se desfazem.

A psicologia, em suas tradições mais sérias, tem vocabulário para essa camada há cem anos. Freud, Lacan, Vygotsky, Ausubel, González Rey, Morin — não são autores decorativos a serem citados como ornamento. São pensadores que construíram aparatos conceituais para tratar exatamente o tipo de fenômeno que a engenharia de agentes começa a produzir.

A tese de fundo deste ensaio é que **o próximo salto na construção de sistemas multiagente não virá de mais engenharia, virá de melhor importação conceitual**. Os agentes que vão operar bem em ambientes complexos serão aqueles cujas meta-skills foram projetadas com aporte conceitual da psicologia — não como metáfora, mas como arquitetura.

E há aqui uma oportunidade específica para quem tem repertório nas duas áreas. A interseção entre engenharia de agentes e psicologia clínica, educacional e cultural-histórica é hoje uma vaga praticamente vazia. Há muita gente fazendo IA com vocabulário pop de "psicologia cognitiva". Há quase ninguém fazendo IA com aporte rigoroso de Vygotsky lido a partir do russo, de Lacan lido nos seminários, de González Rey lido na obra completa.

Essa vaga não vai ficar vazia para sempre. Mas, no momento, ela está aberta — e o próximo movimento mais útil, para quem quiser ocupá-la, não é escrever outro ensaio. É implementar.

A primeira meta-skill projetada segundo essa arquitetura, instalada em um sistema real, com resultados documentados — é o que vai transformar essas ideias em algo que outras pessoas podem usar, criticar e estender. Tudo o mais é preparação.

---

## Referências mobilizadas

**Engenharia de skills e agentes**

- Anthropic. *Equipping agents for the real world with Agent Skills*. 2025. Disponível em: anthropic.com/engineering.
- Agent Skills Specification. agentskills.io. Open standard mantido pela Anthropic, 2025-2026.
- Sutton, R. S.; Precup, D.; Singh, S. *Between MDPs and semi-MDPs: A framework for temporal abstraction in reinforcement learning*. Artificial Intelligence, 1999.
- Tran, K.-T. et al. *Multi-Agent Collaboration Mechanisms: A Survey of LLMs*. arXiv:2501.06322, 2025.

**Psicanálise**

- Freud, S. *Pulsões e destinos da pulsão* [1915]. In: Obras Completas, vol. 12. São Paulo: Companhia das Letras, 2010.
- Lacan, J. *O Seminário, livro 11: Os quatro conceitos fundamentais da psicanálise* [1964]. Rio de Janeiro: Jorge Zahar, 2008.
- Lacan, J. *O Seminário, livro 17: O avesso da psicanálise* [1969-70]. Rio de Janeiro: Jorge Zahar, 1992.
- Žižek, S. *The Sublime Object of Ideology*. London: Verso, 1989.

**Psicologia histórico-cultural**

- Vigotski, L. S. *Pensamento e Linguagem* [1934]. Tradução: várias. Edições críticas no Brasil organizadas por Z. Prestes e E. Tunes a partir do russo.
- Vigotski, L. S. *Sete aulas de L. S. Vigotski sobre os fundamentos da pedologia*. Org. e trad. Z. Prestes e E. Tunes. Rio de Janeiro: E-papers, 2018.
- Prestes, Z.; Tunes, E. *Pontes ou Muralhas: exame crítico de traduções de conceitos da teoria histórico-cultural*. Revista Educativa, v. 25, n. 1, 2022.
- Prestes, Z. *Quando não é quase a mesma coisa: traduções de Lev Semionovitch Vigotski no Brasil*. Campinas: Autores Associados, 2012.

**Aprendizagem significativa**

- Ausubel, D. P.; Novak, J. D.; Hanesian, H. *Educational Psychology: A Cognitive View*. New York: Holt, Rinehart and Winston, 1978.
- Moreira, M. A. *Aprendizagem Significativa*. Brasília: Editora UnB, 1999.

**Teoria da subjetividade e complexidade**

- González Rey, F. L. *Sujeito e subjetividade: uma aproximação histórico-cultural*. São Paulo: Pioneira Thomson, 2003.
- González Rey, F. L. *Pesquisa qualitativa e subjetividade: os processos de construção da informação*. São Paulo: Cengage, 2005.
- González Rey, F. L. *O pensamento de Vygotsky: contradições, desdobramentos e desenvolvimento*. México: Trillas, 2011.
- González Rey, F. L. *Subjetividade: teoria, epistemologia e método*. Campinas: Alínea, 2017.
- Morin, E. *Introdução ao pensamento complexo*. Porto Alegre: Sulina, 2005.
- Mitjáns Martínez, A.; González Rey, F. *Psicologia, educação e aprendizagem escolar: avançando na contribuição da leitura cultural-histórica*. São Paulo: Cortez, 2017.

---

*Este texto é uma proposta inicial, aberta a crítica e revisão. A próxima etapa é a implementação. Quem tiver interesse em testar essa arquitetura em um sistema real, ou em discutir aportes de outras tradições psicológicas que possam fortalecer ou questionar as costuras propostas, vale o diálogo.*
