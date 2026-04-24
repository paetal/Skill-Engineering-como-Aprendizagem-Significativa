# Skill Engineering como Aprendizagem Significativa

## Uma fundamentação teórica para hierarquias de conhecimento operacional em agentes de IA

---

## 0. O problema

Quando se publica uma skill no GitHub, faz-se uma escolha de design que parece técnica mas é, na origem, pedagógica: como decompor um saber-fazer em unidades reutilizáveis, e como organizar essas unidades de modo que se ativem quando devem.

A literatura recente de skill engineering — Jiang et al. no SoK de fevereiro de 2026, os trabalhos sobre externalização e compilação de skills, o framework HERAKLES — está formalizando essas escolhas com vocabulário próprio: *skill composition*, *hierarchical orchestration*, *skill compilation*, *subsumption hierárquica*. Há uma convergência silenciosa, porém, que o campo não reconhece explicitamente: essas formalizações reencontram, com sessenta anos de atraso, descobertas que David Ausubel articulou em 1963 e Joseph Novak operacionalizou nos anos 1970 sob o nome de *meaningful learning theory* e *concept mapping*.

Este documento propõe uma leitura integrada. A tese é que skill engineering, no estado da arte de 2026, é a engenharia computacional de uma intuição psicopedagógica anterior — e que reconhecer essa filiação não é exercício historiográfico, mas aporta vocabulário operacional que o campo ainda não tem.

---

## 1. O que o campo de skill engineering chama de skill

A definição mais rigorosa em circulação vem do SoK de Jiang et al. Uma skill é uma quádrupla `S = (C, π, T, R)`:

- **C** — condições de aplicabilidade (quando essa skill deve ser invocada)
- **π** — política de execução (como ela opera)
- **T** — critérios de terminação (quando ela conclui)
- **R** — interface reutilizável (como outras skills a invocam)

A definição importa porque distingue skill de três coisas que frequentemente se confundem com ela: *tool* (atômica, sem política interna), *plan* (instância única, não reutilizável) e *memory* (declarativa, não procedural).

O ciclo de vida proposto pelo SoK percorre sete estágios — descoberta, prática, destilação, armazenamento, composição, avaliação, atualização — e o campo identifica sete padrões de design ao longo de um espectro de autonomia que vai de skills carregadas via metadata controlada por humanos até meta-skills totalmente autônomas que geram outras skills.

O ponto crítico para o que vem a seguir: skills se compõem hierarquicamente. Uma skill de alto nível invoca skills de nível médio, que invocam skills de baixo nível. Essa árvore não é decorativa — é o que permite que capacidades complexas sejam montadas a partir de capacidades simples reutilizáveis.

---

## 2. Os quatro vetores recentes da literatura

Quatro contribuições recentes são particularmente úteis para fundamentar uma prática autoral de skill engineering.

**Composição e orquestração hierárquica (SoK, 2026).** O paper formaliza explicitamente a árvore: tarefas são pareadas a skills via *embedding-based retrieval* ou *LLM-mediated routing*, e as skills selecionadas se decompõem hierarquicamente em sub-skills. A analogia com o *options framework* do reinforcement learning hierárquico é direta: uma skill é uma ação temporalmente estendida que encapsula uma política completa.

**Externalização (Externalization in LLM Agents, 2026).** Quando uma combinação particular de skills existentes é repetidamente validada como eficaz, essa combinação pode ela mesma ser empacotada como uma nova skill de nível mais alto. A árvore se auto-constrói: emerge do uso. Repertórios hierárquicos não são desenhados a priori — eles se sedimentam.

**Compilação de skills (HERAKLES, 2025).** Um LLM atua como política de alto nível e uma rede neural pequena como política de baixo nível. Skills aprendidas pelo agente hierárquico como um todo são *compiladas* na política de baixo nível, expandindo o espaço de ação sobre o qual a política de alto nível pode operar. É a metáfora neural levada a sério: a rede cresce por derivação.

**Skill engineering como campo (Agent Skills for LLMs, 2026).** Nomeia a virada histórica em três fases: prompt engineering (2022-2023), tool use e function calling (2023-2024), skill engineering (2025-presente). E faz uma afirmação emergentista importante: a composição de skills pode gerar capacidades que excedem o que qualquer skill individual oferece.

---

## 3. O que Ausubel e Novak já tinham articulado

A teoria da aprendizagem significativa de David Ausubel (1963, 1968, 2000) parte de um princípio que ele próprio considerava o mais importante de toda a psicologia educacional: *o fator isolado mais relevante para a aprendizagem é aquilo que o aprendiz já sabe*.

A partir desse princípio, Ausubel constrói um modelo cuja arquitetura interna é estranhamente familiar para quem leu skill engineering recente.

**Estrutura cognitiva como hierarquia.** Para Ausubel, a estrutura cognitiva é organizada hierarquicamente: conceitos altamente inclusivos no topo, conceitos menos inclusivos e dados específicos subsumidos abaixo. Não há armazenamento aleatório — há subordinação.

**Subsunção como mecanismo central.** Aprender significativamente é *subsumir* novo material a estruturas conceituais preexistentes que sirvam como pontos de ancoragem (*subsumers*). Ausubel distingue quatro processos:

- *subsunção derivativa* — o novo é instância direta do já conhecido;
- *subsunção correlativa* — o novo estende ou modifica o conceito ancorador;
- *aprendizagem superordenada* — instâncias específicas anteriormente conhecidas são reorganizadas sob um conceito mais geral recém-adquirido;
- *aprendizagem combinatória* — o novo conecta-se ao existente sem relação direta de subordinação ou superordenação.

**Diferenciação progressiva e reconciliação integrativa.** Conceitos se desdobram em componentes mais finos (diferenciação progressiva) e, simultaneamente, conceitos antes percebidos como distintos podem ser reconhecidos como relacionados (reconciliação integrativa). Os dois movimentos operam em direções opostas e juntos.

**Organizadores prévios.** Material introdutório de alto nível conceitual apresentado antes de conteúdo específico, com a função de fornecer pontos de ancoragem para a subsunção subsequente. A função é estrutural, não decorativa.

Joseph Novak, trabalhando com Ausubel, traduziu a teoria em ferramenta visual operável: o *concept map*. Em um mapa conceitual, conceitos mais inclusivos ficam no topo, conceitos subordinados abaixo, e relações nomeadas explicitamente em proposições. O mapa não é um esquema didático — é uma representação externalizada da estrutura cognitiva tal como Ausubel a teorizou.

---

## 4. A correspondência

A leitura cuidadosa dos dois corpos teóricos revela uma correspondência que vai além de analogia frouxa. Em vários pontos, as estruturas formais são quase idênticas.

| Skill engineering (2025-2026) | Aprendizagem significativa (1963-2008) |
|---|---|
| Skill como `(C, π, T, R)` — condições, política, terminação, interface | Conceito como unidade portadora de regularidades aplicáveis em condições reconhecíveis |
| Composição hierárquica de skills | Hierarquia de inclusividade conceitual |
| Skill de alto nível invoca sub-skills | Conceito superordenado subsume conceitos subordinados |
| Externalização: combinações validadas viram nova skill | Aprendizagem superordenada: instâncias geram conceito mais geral |
| Skill compilation (HERAKLES) | Subsunção com automatização progressiva |
| Skill discovery e refinement | Diferenciação progressiva |
| Skill consolidation entre domínios | Reconciliação integrativa |
| Metadata de skill, descrição que controla ativação | Organizador prévio |
| Embedding-based retrieval para invocação | Ancoragem em estrutura cognitiva preexistente |
| *Obliterative subsumption* (skills pouco usadas decaem) | Esquecimento por perda de distintividade do subsumer |

A correspondência não é mística. As duas teorias estão modelando um mesmo problema estrutural: como sistemas cognitivos — biológicos ou artificiais — organizam saber-fazer e saber-que de modo que o conhecimento prévio sirva de andaime para o conhecimento novo, sem que cada situação exija reconstrução desde o zero.

O que skill engineering acrescenta, e que Ausubel não tinha como ter, é a parte computacional: como representar, armazenar, indexar, recuperar e versionar essas unidades em sistemas reais. O que Ausubel e Novak oferecem, e que skill engineering ainda não tem articulado, é o vocabulário fino para os movimentos qualitativamente distintos de incorporação — a diferença entre subsunção derivativa e correlativa, por exemplo, é uma distinção que o campo computacional ainda colapsa em "skill composition" sem mais.

---

## 5. Implicações operacionais para uma prática de skill engineering

Reconhecer a filiação muda decisões concretas no momento de escrever uma skill.

**A descrição da skill é um organizador prévio.** O campo `description` em uma skill não é metadado administrativo — é o subsumer que determina quando o agente reconhecerá a aplicabilidade. Descrições genéricas produzem ancoragem fraca, da mesma forma que organizadores prévios vagos produzem aprendizagem rasa em sala de aula. O critério de qualidade de uma descrição é o mesmo critério de qualidade de um advance organizer ausubeliano: precisa ser específica o suficiente para ancorar, geral o suficiente para acolher casos novos.

**A árvore de skills é um mapa conceitual executável.** A diferença entre um mapa conceitual de Novak e uma árvore de skills bem desenhada é apenas que a segunda é executável. As leis de boa construção são análogas: hierarquia clara entre o mais inclusivo e o mais específico, relações nomeadas explicitamente, evitar conceitos ilhados sem ancoragem, evitar redundância sem reconciliação.

**Decompor uma skill é fazer diferenciação progressiva.** Quando uma skill se torna muito grande e se quebra em sub-skills, o movimento é o mesmo da diferenciação progressiva: o conceito-mãe se desdobra em componentes mais finos sem perder a relação com o ancorador. Skills que se decompõem mal — em que as sub-skills não preservam relação clara com a skill-mãe — produzem o equivalente computacional de aprendizagem mecânica: as peças funcionam isoladas mas não compõem.

**Combinar skills entre domínios é reconciliação integrativa.** Quando uma skill desenvolvida em um domínio (por exemplo, análise contratual) é reconhecida como aplicável em outro (por exemplo, parecer técnico ambiental), o movimento não é composição banal — é reconciliação integrativa, e exige trabalho explícito de reformulação das condições de aplicabilidade. Pular esse trabalho gera transferência espúria.

**Skill marketplaces precisam de pedagogia, não só de governança.** O SoK dedica seção extensa a riscos de supply chain e prompt injection via skills maliciosas distribuídas em marketplaces. A discussão é necessária, mas insuficiente. Um marketplace de skills é também um currículo distribuído, e a curadoria pedagógica — que skills um agente deve adquirir em que ordem para construir estrutura cognitiva robusta — é um problema irmão da segurança e ainda mais negligenciado.

---

## 6. Onde isso aponta

A tese implícita deste documento é que skill engineering ganha potência ao se reconhecer como uma engenharia pedagógica para sistemas não-humanos. O que está sendo construído quando se publica uma skill no GitHub não é apenas um módulo de software — é uma unidade curricular destinada a ser subsumida na estrutura cognitiva de agentes que a recuperarão sob condições específicas.

Essa releitura abre frentes concretas:

- critérios pedagógicos (não só técnicos) para avaliar qualidade de skills publicadas;
- ferramentas de visualização que tratem repertórios de skills como mapas conceituais executáveis, com hierarquia, relações nomeadas e advance organizers explícitos;
- vocabulário fino para distinguir tipos de composição — derivativa, correlativa, superordenada, combinatória — que hoje são colapsados em "skill composition";
- protocolos de externalização que respeitem reconciliação integrativa, evitando que combinações repetidas sejam empacotadas sem reformulação conceitual.

A literatura de skill engineering está, em 2026, mais ou menos onde a psicologia educacional estava antes de Ausubel sistematizar a aprendizagem significativa: com bons mecanismos descritos, mas sem a teoria que diga por que alguns funcionam e outros não. A teoria já existe. Está em uma estante adjacente.

---

## Referências principais

**Skill engineering (2025-2026)**

- Jiang, Y. et al. *SoK: Agentic Skills — Beyond Tool Use in LLM Agents*. arXiv:2602.20867, fevereiro de 2026.
- *Externalization in LLM Agents*. 2026.
- *HERAKLES: Hierarchical Skill Compilation in LLM Agents*. agosto de 2025.
- *Agent Skills for LLMs*. fevereiro de 2026.

**Aprendizagem significativa e mapas conceituais**

- Ausubel, D. P. *The Psychology of Meaningful Verbal Learning*. New York: Grune & Stratton, 1963.
- Ausubel, D. P., Novak, J. D., & Hanesian, H. *Educational Psychology: A Cognitive View*. Holt, Rinehart and Winston, 1968.
- Ausubel, D. P. *The Acquisition and Retention of Knowledge: A Cognitive View*. Kluwer, 2000.
- Novak, J. D. & Gowin, D. B. *Learning How to Learn*. Cambridge University Press, 1984.
- Novak, J. D. *Meaningful learning: The essential factor for conceptual change in limited or inappropriate propositional hierarchies leading to empowerment of learners*. Science Education, 86, 548-571, 2002.
- Novak, J. D. & Cañas, A. J. *The Theory Underlying Concept Maps and How to Construct Them*. IHMC, 2008.
