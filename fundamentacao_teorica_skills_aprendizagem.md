# Skill Engineering como Aprendizagem Significativa

## Uma fundamentação teórica para hierarquias de conhecimento operacional em agentes de inteligência artificial

---

## Resumo

Este texto propõe uma releitura do campo emergente de *skill engineering* a partir de três tradições da psicologia da aprendizagem: a teoria da aprendizagem significativa de David Ausubel, a técnica de mapeamento conceitual de Joseph Novak, e a teoria de chunks e templates de William Chase, Herbert Simon e Fernand Gobet. A tese é que as formalizações computacionais que emergiram entre 2023 e 2026 — particularmente após a publicação da especificação `SKILL.md` pela Anthropic e dos surveys sistemáticos de Jiang et al. e Xu — reencontram, com vocabulário próprio, descobertas articuladas pela psicologia educacional desde os anos 1960. Reconhecer essa filiação não é exercício historiográfico, mas operação teórica que aporta vocabulário fino para distinções que o campo computacional ainda colapsa em termos genéricos como *skill composition* e *progressive disclosure*.

---

## 1. O problema

Quando um engenheiro publica uma skill no GitHub para uso em Claude Code, OpenClaw ou qualquer outro agente compatível com a especificação `SKILL.md`, ele faz uma cadeia de escolhas que parecem técnicas mas têm origem pedagógica. Como decompor um saber-fazer em unidades reutilizáveis. Como descrever cada unidade para que ela seja invocada quando deve, e apenas quando deve. Como organizar as unidades numa hierarquia que permita composição sem inflar o contexto. Como avaliar se uma skill nova é redundante com outra existente, ou se merece ser tratada como conceito independente.

Essas escolhas têm sido tratadas, na literatura recente, como problemas de engenharia de prompt, de governança de marketplaces, ou de eficiência de janela de contexto. Os surveys mais ambiciosos do campo — o *SoK: Agentic Skills* de Jiang et al. (arXiv:2602.20867, fevereiro de 2026) e *Agent Skills for Large Language Models* de Xu (arXiv:2602.12430, fevereiro de 2026) — formalizam o problema com rigor crescente, mas tratam a estrutura cognitiva subjacente como dado, não como objeto teórico.

A tese deste texto é que a estrutura cognitiva subjacente *já foi tratada como objeto teórico*, em outra disciplina, sessenta anos atrás. E que a tradução desse tratamento para o vocabulário operacional do skill engineering produz refinamentos imediatos.

---

## 2. O que o campo de skill engineering tem hoje

### 2.1 A definição formal

A definição mais rigorosa em circulação é a do SoK. Uma skill é uma quádrupla `S = (C, π, T, R)`:

- **C** — condições de aplicabilidade
- **π** — política de execução
- **T** — critérios de terminação
- **R** — interface reutilizável

A definição importa porque distingue skill de três coisas frequentemente confundidas com ela. Uma *tool* é primitiva atômica, sem política interna. Um *plan* é artefato pontual, não persistente entre sessões. Uma *memory* é declarativa, não procedural. Uma skill, ao contrário, é módulo callable que persiste entre sessões, carrega política executável, e expõe interface reutilizável.

### 2.2 A especificação operacional

A Anthropic publicou em outubro de 2025 a especificação `SKILL.md`, hoje adotada como padrão aberto por Claude, Cursor, GitHub, OpenAI Codex e OpenClaw. A especificação é deliberadamente minimalista: uma pasta contendo um arquivo `SKILL.md` com frontmatter YAML obrigatório (`name` e `description`) seguido de instruções em markdown. Arquivos auxiliares e scripts podem ser referenciados a partir do SKILL.md principal.

O princípio arquitetural central é o que a Anthropic chama de *progressive disclosure*. Apenas o frontmatter (nome e descrição) é carregado no system prompt no início da sessão. O corpo do SKILL.md só é carregado se o agente decidir que a skill é relevante para a tarefa em curso. Arquivos auxiliares, por sua vez, só são carregados quando o agente, já dentro do SKILL.md, segue uma referência específica para eles.

A consequência arquitetural é importante: o `description` no frontmatter não é metadado administrativo. É o único sinal disponível para o agente decidir se a skill se aplica. Se a descrição é vaga, o agente subutiliza a skill; se é genérica demais, o agente a invoca em contextos errados. A própria documentação interna da Anthropic, no skill `skill-creator` mantido por eles, reconhece que "Claude tem tendência a *subdisparar* skills" e recomenda que descrições sejam ligeiramente "pushy" para combater esse problema.

Esse fenômeno tem nome em outra disciplina, como veremos.

### 2.3 A composição hierárquica

Skills compõem-se hierarquicamente. Uma skill de alto nível invoca skills de nível médio, que por sua vez invocam skills de baixo nível. O exemplo canônico do SoK: uma skill "deploy de aplicação web" invoca "configurar banco de dados", "configurar servidor", "rodar testes", que invocam "executar migração SQL", "escrever config de Nginx".

Essa decomposição não é nova. Ela formaliza, no contexto de LLMs, o *options framework* do reinforcement learning hierárquico — opções como ações temporalmente estendidas que encapsulam políticas completas. O que muda em 2025-2026 é que as opções não são mais sub-políticas neurais opacas: são módulos legíveis, versionáveis, auditáveis, que podem circular entre humanos e entre agentes.

### 2.4 Os quatro vetores recentes

Quatro contribuições da literatura de 2023-2026 estabelecem os vetores principais do campo.

**Voyager** (Wang, Xie, Jiang, Mandlekar, Xiao, Zhu, Fan, Anandkumar, arXiv:2305.16291, 2023) é o trabalho seminal que demonstrou agentes LLM capazes de aprendizado contínuo via biblioteca de skills crescente. Seus três componentes — currículo automático que maximiza exploração, biblioteca de skills em código executável, e mecanismo iterativo de prompting com auto-verificação — são o template arquitetural que estruturas posteriores refinaram. Crucialmente, Voyager demonstrou que skills aprendidas num mundo podem ser transferidas para outro mundo do mesmo domínio, e que "as habilidades desenvolvidas são temporalmente estendidas, interpretáveis e composicionais, o que compõe rapidamente as capacidades do agente e mitiga o esquecimento catastrófico".

**HERAKLES** (Carta, Romac, Gaven, Oudeyer, Sigaud, Lamprier, arXiv:2508.14751, agosto de 2025) avançou ao formalizar a *compilação de skills*. Um LLM atua como política de alto nível e uma rede neural pequena como política de baixo nível. À medida que o agente domina objetivos, a trajetória completa é compilada como nova skill na política de baixo nível, expandindo dinamicamente o espaço de ação sobre o qual a política de alto nível pode operar. A linguagem dos autores é precisa: "a árvore de habilidades não é pré-especificada — ela emerge".

**SoK: Agentic Skills** (Jiang et al., arXiv:2602.20867, fevereiro de 2026) sistematizou o campo sob a perspectiva do ciclo de vida (descoberta, prática, destilação, armazenamento, composição, avaliação, atualização) e propôs duas taxonomias: sete padrões de design ao longo de um espectro de autonomia (de skills carregadas via metadata controlada por humanos até meta-skills que geram outras skills), e uma taxonomia ortogonal de representação × escopo. O paper documenta também o caso ClawHavoc — quase 1.200 skills maliciosas infiltradas em um marketplace, exfiltrando chaves de API e credenciais — como evidência de que a governança do ecossistema é problema aberto.

**Agent Skills for LLMs** (Xu, arXiv:2602.12430, fevereiro de 2026) nomeou a virada histórica em três fases: prompt engineering (2022-2023), tool use e function calling (2023-2024), skill engineering (2025-presente). E documentou empiricamente que 26,1% das skills contribuídas pela comunidade contêm vulnerabilidades, motivando seu Skill Trust and Lifecycle Governance Framework com quatro portões de verificação.

A esses quatro pilares somam-se contribuições laterais relevantes: SAGE e SEAgent (skills aprendidas via RL e descoberta autônoma), Corpus2Skill (compilação offline de corpus em árvore de skills navegável), AutoRefine (extração de expertise reutilizável a partir de trajetórias), e SkillsBench/SkillLearnBench (benchmarks empíricos da relação entre curadoria de skills e taxa de sucesso).

---

## 3. O que a psicologia da aprendizagem articulou antes

### 3.1 Ausubel e a aprendizagem significativa

David Ausubel publicou em 1963 *The Psychology of Meaningful Verbal Learning* e, em 1968, em coautoria com Joseph Novak e Helen Hanesian, *Educational Psychology: A Cognitive View*. Sua tese central é resumida pelo próprio Ausubel numa formulação que ficou célebre: "se eu tivesse que reduzir toda a psicologia educacional a um único princípio, eu diria isto: o fator isolado mais importante influenciando a aprendizagem é aquilo que o aprendiz já sabe."

A partir desse princípio, Ausubel constrói um modelo cuja arquitetura interna é estranhamente familiar para quem trabalha com skill engineering.

**Estrutura cognitiva como hierarquia.** Para Ausubel, a estrutura cognitiva é organizada hierarquicamente: conceitos altamente inclusivos no topo, conceitos menos inclusivos e dados específicos subsumidos abaixo. Não há armazenamento aleatório — há subordinação. A metáfora que circula na literatura é a do "caixote chinês": dentro do caixote maior estão caixotes menores, e dentro destes, ainda menores.

**Subsunção como mecanismo central.** Aprender significativamente é subsumir novo material a estruturas conceituais preexistentes que sirvam como pontos de ancoragem (*subsumers*). Ausubel distingue quatro processos:

- *subsunção derivativa* — o novo é instância direta do já conhecido (alguém que sabe o que é "carro" aprende sobre "Jeep" como instância);
- *subsunção correlativa* — o novo estende ou modifica o conceito ancorador (alguém aprende sobre "limusine" e expande seu conceito de carro para acomodar novas dimensões);
- *aprendizagem superordenada* — instâncias específicas anteriormente conhecidas são reorganizadas sob um conceito mais geral recém-adquirido (alguém que conhece pardais, sabiás e tordos aprende o conceito de "passeriforme");
- *aprendizagem combinatória* — o novo conecta-se ao existente sem relação direta de subordinação ou superordenação (a aprendizagem mais difícil, exigindo novas pontes).

**Diferenciação progressiva e reconciliação integrativa.** Conceitos se desdobram em componentes mais finos (diferenciação progressiva) e, simultaneamente, conceitos antes percebidos como distintos podem ser reconhecidos como relacionados (reconciliação integrativa). Os dois movimentos operam em direções opostas e juntos.

**Subsunção obliterativa.** Quando uma ideia subsumida torna-se gradualmente menos distintiva do seu subsumer, ela é eventualmente esquecida. O esquecimento, em Ausubel, não é decaimento aleatório — é perda de distintividade. Skills pouco invocadas decaem por mecanismo análogo.

**Organizadores prévios.** Material introdutório de alto nível conceitual apresentado *antes* do conteúdo específico, com a função explícita de fornecer pontos de ancoragem para a subsunção subsequente. A função é estrutural. Sem organizador prévio adequado, a subsunção ou não acontece, ou acontece mal — gerando o que Ausubel chama de aprendizagem mecânica (*rote learning*), oposta à aprendizagem significativa.

### 3.2 Novak e a operacionalização visual

Joseph Novak, trabalhando com Ausubel, traduziu a teoria em ferramenta visual operável: o *concept map*. Em um mapa conceitual, conceitos mais inclusivos ficam no topo, conceitos subordinados abaixo, e relações entre eles são nomeadas explicitamente em proposições. O mapa não é um esquema didático genérico — é uma representação externalizada da estrutura cognitiva tal como Ausubel a teorizou.

A obra fundamental — Novak & Gowin, *Learning How to Learn* (1984) — e o trabalho subsequente de Novak (2002, 2010) e Novak & Cañas (2008) consolidaram três processos centrais que governam a construção de mapas conceituais:

- **subsunção** — conceitos de ordem inferior são subsumidos sob conceitos de ordem superior, criando a hierarquia;
- **diferenciação progressiva** — conceitos são desmembrados em componentes mais finos;
- **reconciliação integrativa** — conceitos previamente isolados ou aparentemente distintos são reconhecidos como relacionados.

O ponto crítico, frequentemente esquecido, é que mapas conceituais bem construídos não são apenas hierárquicos — eles têm relações *nomeadas*. Cada arco do grafo carrega um verbo ou expressão proposicional ("é tipo de", "causa", "deriva de", "compõe-se de") que torna a relação inspecionável.

### 3.3 Chase, Simon e Gobet — chunks e templates

A tradição complementar vem da psicologia cognitiva do *expertise*. William Chase e Herbert Simon, em "Perception in Chess" (Cognitive Psychology, 1973), formalizaram o conceito de *chunk* a partir do estudo clássico de mestres de xadrez: especialistas não têm memória de trabalho maior que iniciantes, mas perceben o tabuleiro em chunks maiores, padrões reutilizáveis armazenados em memória de longo prazo.

A teoria foi estendida por Fernand Gobet e Herbert Simon em "Templates in Chess Memory" (Cognitive Psychology, 1996) e "Expert Chess Memory: Revisiting the Chunking Hypothesis" (Memory, 1998). A contribuição decisiva é o conceito de *template*: estrutura de dados mais complexa que um chunk simples, que evolui a partir do uso repetido, e que unifica aspectos perceptivos de baixo nível com conhecimento esquemático de alto nível e capacidade de planejamento. Templates emergem da experiência, não são desenhados a priori.

A formulação de Gobet e Simon merece transcrição parafraseada cuidadosa: chunks são acessados por meio de uma *rede de discriminação* na qual características perceptuais simples são testadas, e podem evoluir para estruturas de dados mais complexas — templates — específicas para classes de situações.

Note-se a sobreposição com HERAKLES: trajetórias bem-sucedidas, repetidas, são *compiladas* em estruturas reutilizáveis. A diferença é que HERAKLES descreve o processo em redes neurais e Gobet & Simon o descrevem em cognição humana, mas o mecanismo formal é o mesmo.

### 3.4 Vygotsky e Oudeyer — a ponte que poucos veem

Há uma quarta tradição relevante, e ela aparece de modo inesperado dentro da própria literatura de skill engineering. Pierre-Yves Oudeyer, coautor de HERAKLES, é figura central da chamada *developmental AI* — abordagem que modela aquisição de habilidades em agentes a partir de princípios da psicologia do desenvolvimento, particularmente Piaget e Vygotsky.

O paper de Colas, Karch, Sigaud e Oudeyer publicado na *Nature Machine Intelligence* em 2022 ("Language and culture internalization for human-like autotelic AI") propõe explicitamente *Vygotskian autotelic agents* — agentes intrinsecamente motivados que aprendem habilidades imergindo em mundos socioculturais ricos, internalizando interações com outros agentes (humanos ou artificiais) e transformando-as em ferramentas cognitivas próprias.

Aqui a tradição da aprendizagem significativa encontra sua complementação. Ausubel oferece o mecanismo intra-psíquico (como o novo se ancora no já conhecido). Vygotsky, na leitura de Oudeyer, oferece o mecanismo interpsíquico (como o socialmente compartilhado é internalizado e se torna estrutura individual).

Um marketplace de skills, sob essa luz, não é apenas distribuição de software. É infraestrutura para internalização vygotskyana em escala — cada skill publicada é uma ferramenta cognitiva externalizada que pode ser internalizada por qualquer agente que a invoque.

---

## 4. As correspondências

A leitura cuidadosa dos dois corpos teóricos revela correspondências que vão além de analogia frouxa. Em vários pontos, as estruturas formais são quase idênticas.

### 4.1 Tabela de correspondências

| Skill engineering (2023-2026) | Aprendizagem significativa e cognição (1963-2008) |
|---|---|
| Skill como `(C, π, T, R)` | Conceito como unidade portadora de regularidades aplicáveis em condições reconhecíveis |
| Composição hierárquica de skills | Hierarquia de inclusividade conceitual (Ausubel) |
| Skill de alto nível invoca sub-skills | Conceito superordenado subsume conceitos subordinados |
| Skill discovery e externalização | Aprendizagem superordenada (instâncias geram conceito mais geral) |
| Skill compilation (HERAKLES) | Templates a partir de chunks repetidos (Gobet & Simon) |
| Skill refinement por feedback | Subsunção correlativa (o novo modifica o ancorador) |
| Skill reuse entre domínios | Reconciliação integrativa (Ausubel-Novak) |
| Decomposição de skill em sub-skills | Diferenciação progressiva |
| `description` no frontmatter | Organizador prévio (Ausubel) |
| Embedding-based retrieval | Ancoragem em estrutura cognitiva preexistente |
| Skills pouco invocadas decaem | Subsunção obliterativa (perda de distintividade) |
| Marketplace como ecossistema | Internalização vygotskyana de ferramentas culturais |
| Currículo automático (Voyager) | Zona de Desenvolvimento Proximal (Vygotsky) |
| Progressive disclosure | Apresentação do geral ao específico (Ausubel) |
| Curated skills outperform self-generated | Mediação cultural supera descoberta solitária |

A correspondência não é mística. As duas tradições estão modelando o mesmo problema estrutural: como sistemas cognitivos — biológicos ou artificiais — organizam saber-fazer e saber-que de modo que o conhecimento prévio sirva de andaime para o conhecimento novo, sem que cada situação exija reconstrução desde zero.

### 4.2 O que cada lado acrescenta

O que skill engineering acrescenta, e que Ausubel não tinha como ter, é a parte computacional concreta: como representar, armazenar, indexar, recuperar, versionar e auditar essas unidades em sistemas reais com latência mensurável e janelas de contexto finitas.

O que Ausubel, Novak, Chase, Simon, Gobet e Vygotsky oferecem, e que skill engineering ainda não tem articulado, é o vocabulário fino para os movimentos qualitativamente distintos de incorporação. A diferença entre subsunção derivativa e correlativa é uma distinção que o campo computacional ainda colapsa em "skill composition". A diferença entre chunk e template é uma distinção que o campo computacional ainda colapsa em "skill" indistintamente. A diferença entre internalização vygotskyana e descoberta autônoma é uma distinção que o campo ainda colapsa em "skill acquisition".

---

## 5. Implicações operacionais

Reconhecer a filiação produz refinamentos imediatos no momento de escrever, organizar e publicar skills.

### 5.1 Descrições como organizadores prévios

O campo `description` no frontmatter SKILL.md não é metadado administrativo. É o subsumer que determina quando o agente reconhecerá a aplicabilidade. Descrições genéricas produzem ancoragem fraca, do mesmo modo que organizadores prévios vagos produzem aprendizagem rasa em sala de aula.

O critério ausubeliano de qualidade de um organizador prévio é o mesmo critério para uma boa descrição de skill: precisa ser específico o suficiente para ancorar o caso concreto, e geral o suficiente para acolher casos futuros que pertençam à mesma classe. A recomendação interna da Anthropic — "descrições devem ser ligeiramente pushy" — é uma compensação ad hoc para um problema que tem nome teórico há sessenta anos: ancoragem fraca por organizador prévio insuficiente.

Recomendação prática: ao escrever uma descrição, pergunte-se duas coisas. *Que cenário específico essa skill resolve?* (especificidade que ancora). *Que outros cenários da mesma família ela também resolve?* (generalidade que acolhe). Se você não consegue responder ambas, a descrição está mal calibrada.

### 5.2 A árvore de skills como mapa conceitual executável

A diferença entre um mapa conceitual de Novak e uma árvore de skills bem desenhada é apenas uma: a segunda é executável. As leis de boa construção são análogas:

- hierarquia clara entre o mais inclusivo e o mais específico;
- relações explícitas entre os níveis (uma skill que invoca outra deve nomear *como* a invoca, não apenas *que* a invoca);
- evitar conceitos ilhados sem ancoragem (uma skill que nenhuma outra invoca e que não está conectada ao fluxo principal é candidata a ser revisada ou removida);
- evitar redundância sem reconciliação (duas skills que cobrem o mesmo território conceitual sem que essa sobreposição seja explicitamente tratada produzem conflito de invocação).

Recomendação prática: para qualquer repositório de skills com mais de dez unidades, mantenha um mapa conceitual real (em CmapTools, Obsidian Canvas, ou qualquer ferramenta análoga) que represente a hierarquia inclusiva e as relações nomeadas. Esse mapa não é documentação acessória — é o organizador prévio do próprio repositório, para você e para colaboradores futuros.

### 5.3 Decomposição como diferenciação progressiva

Quando uma skill cresce demais e precisa ser quebrada em sub-skills, o movimento estrutural é diferenciação progressiva: o conceito-mãe se desdobra em componentes mais finos sem perder relação com o ancorador. Skills que se decompõem mal — em que as sub-skills não preservam relação clara com a skill-mãe — produzem o equivalente computacional de aprendizagem mecânica: as peças funcionam isoladas, mas não compõem.

Recomendação prática: ao quebrar uma skill, escreva primeiro a relação entre a skill-mãe e cada sub-skill em uma frase declarativa. "A skill X invoca Y *para realizar* a etapa Z." Se você não consegue escrever essa frase, a decomposição é arbitrária.

### 5.4 Composição entre domínios como reconciliação integrativa

Quando uma skill desenvolvida num domínio (análise contratual, por exemplo) é reconhecida como aplicável noutro domínio (parecer técnico ambiental, por exemplo), o movimento não é composição banal — é reconciliação integrativa, e exige trabalho explícito de reformulação das condições de aplicabilidade. Pular esse trabalho gera transferência espúria: a skill é invocada em contextos onde sua política não se aplica realmente, e o agente produz output ruim.

Recomendação prática: antes de marcar uma skill como aplicável a um novo domínio, edite explicitamente o `description` para incluir o novo domínio nas condições de ancoragem. Não confie em transferência implícita por similaridade semântica de embeddings.

### 5.5 Skill marketplaces precisam de pedagogia

O SoK dedica seção extensa a riscos de supply chain e prompt injection via skills maliciosas distribuídas em marketplaces. O Trust and Lifecycle Governance Framework de Xu propõe quatro portões de verificação. As discussões são necessárias, mas insuficientes.

Um marketplace de skills é também um *currículo distribuído*, e a curadoria pedagógica — que skills um agente deve adquirir em que ordem para construir estrutura cognitiva robusta — é um problema irmão da segurança e ainda mais negligenciado. A evidência empírica do SkillsBench de que skills curadas substancialmente melhoram desempenho enquanto skills auto-geradas frequentemente o degradam é, do ponto de vista vygotskyano, esperada. Mediação cultural supera descoberta solitária.

Recomendação prática: marketplaces deveriam permitir que skills declarem pré-requisitos (skills que devem estar presentes ou dominadas antes desta) e progressões (skills que esta possibilita). Isso não é apenas governança — é arquitetura curricular.

### 5.6 Avaliação de skills como diagnóstico de subsunção

Os benchmarks atuais (SkillsBench, SkillLearnBench) avaliam skills por taxa de sucesso em tarefas. É métrica útil, mas grosseira. Ausubel ofereceria diagnóstico mais fino: a skill produz subsunção significativa (o agente generaliza para casos novos da mesma classe) ou produz subsunção mecânica (o agente acerta apenas em casos vistos)?

Recomendação prática: para cada skill publicada, mantenha um conjunto de queries-de-teste que inclui não apenas casos típicos, mas também *casos-limite* (que testam diferenciação progressiva), *casos-cruzados* (que testam reconciliação integrativa) e *casos-distratores* (que testam se a descrição ancora apropriadamente, evitando ativação espúria).

---

## 6. Onde isso aponta

A tese implícita deste documento é que skill engineering ganha potência ao se reconhecer como uma engenharia pedagógica para sistemas não-humanos. O que está sendo construído quando se publica uma skill no GitHub não é apenas módulo de software — é unidade curricular destinada a ser subsumida na estrutura cognitiva de agentes que a recuperarão sob condições específicas.

Essa releitura abre frentes concretas:

- critérios pedagógicos (não só técnicos) para avaliar qualidade de skills publicadas;
- ferramentas de visualização que tratem repertórios de skills como mapas conceituais executáveis, com hierarquia, relações nomeadas e advance organizers explícitos;
- vocabulário fino para distinguir tipos de composição — derivativa, correlativa, superordenada, combinatória — que hoje são colapsados em "skill composition";
- protocolos de externalização que respeitem reconciliação integrativa, evitando que combinações repetidas sejam empacotadas sem reformulação conceitual;
- frameworks curriculares para marketplaces, com pré-requisitos e progressões explícitas;
- benchmarks que avaliem qualidade de subsunção, não apenas taxa de sucesso.

A literatura de skill engineering está, em 2026, mais ou menos onde a psicologia educacional estava antes de Ausubel sistematizar a aprendizagem significativa: com bons mecanismos descritos, mas sem a teoria que diga por que alguns funcionam e outros não. A teoria já existe. Está em uma estante adjacente.

---

## Referências

### Skill engineering e agentes LLM (2023-2026)

Wang, G., Xie, Y., Jiang, Y., Mandlekar, A., Xiao, C., Zhu, Y., Fan, L., & Anandkumar, A. (2023). *Voyager: An Open-Ended Embodied Agent with Large Language Models*. arXiv:2305.16291.

Carta, T., Romac, C., Gaven, L., Oudeyer, P.-Y., Sigaud, O., & Lamprier, S. (2025). *HERAKLES: Hierarchical Skill Compilation for Open-ended LLM Agents*. arXiv:2508.14751.

Jiang, Y., Wang, Q., et al. (2026). *SoK: Agentic Skills — Beyond Tool Use in LLM Agents*. arXiv:2602.20867.

Xu, R. (2026). *Agent Skills for Large Language Models: Architecture, Acquisition, Security, and the Path Forward*. arXiv:2602.12430.

Anthropic. (2025). *Equipping agents for the real world with Agent Skills*. Engineering blog. anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills.

Anthropic. (2025-2026). *Agent Skills documentation*. platform.claude.com/docs/en/agents-and-tools/agent-skills.

### Aprendizagem significativa e mapas conceituais

Ausubel, D. P. (1963). *The Psychology of Meaningful Verbal Learning*. New York: Grune & Stratton.

Ausubel, D. P., Novak, J. D., & Hanesian, H. (1968). *Educational Psychology: A Cognitive View*. Holt, Rinehart and Winston.

Ausubel, D. P. (2000). *The Acquisition and Retention of Knowledge: A Cognitive View*. Kluwer Academic Publishers.

Novak, J. D., & Gowin, D. B. (1984). *Learning How to Learn*. Cambridge University Press.

Novak, J. D. (2002). Meaningful learning: The essential factor for conceptual change in limited or inappropriate propositional hierarchies leading to empowerment of learners. *Science Education*, 86, 548-571.

Novak, J. D., & Cañas, A. J. (2008). *The Theory Underlying Concept Maps and How to Construct Them*. Technical Report IHMC CmapTools 2006-01.

Novak, J. D. (2010). *Learning, Creating, and Using Knowledge: Concept Maps as Facilitative Tools in Schools and Corporations* (2nd ed.). Routledge.

### Expertise, chunks e templates

Chase, W. G., & Simon, H. A. (1973). Perception in chess. *Cognitive Psychology*, 4(1), 55-81.

Gobet, F., & Simon, H. A. (1996). Templates in chess memory: A mechanism for recalling several boards. *Cognitive Psychology*, 31, 1-40.

Gobet, F., & Simon, H. A. (1998). Expert chess memory: Revisiting the chunking hypothesis. *Memory*, 6(3), 225-255.

Gobet, F. (1998). Expert memory: A comparison of four theories. *Cognition*, 66(2), 115-152.

### Aprendizagem desenvolvimentista, motivação intrínseca e Vygotsky em IA

Oudeyer, P.-Y., Kaplan, F., & Hafner, V. V. (2007). Intrinsic motivation systems for autonomous mental development. *IEEE Transactions on Evolutionary Computation*, 11(2), 265-286.

Colas, C., Karch, T., Sigaud, O., & Oudeyer, P.-Y. (2022). Autotelic agents with intrinsically motivated goal-conditioned reinforcement learning: A short survey. *Journal of Artificial Intelligence Research*, 74, 1159-1199.

Colas, C., Karch, T., Moulin-Frier, C., & Oudeyer, P.-Y. (2022). Language and culture internalization for human-like autotelic AI. *Nature Machine Intelligence*, 4, 1068-1076.

Vygotsky, L. S. (1934/1986). *Thought and Language*. MIT Press.
