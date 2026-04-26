# Mapa Conceitual — Skill Engineering

> Material didático para quem cria skills, usa agentes e quer entender a arquitetura teórica por trás do campo.
> Baseado no SoK de Jiang et al.

---

## 1. Visão Geral — Mindmap

Este mindmap mostra o conceito central (**Skill**) e seus quatro eixos de compreensão: o que é, o que não é, como vive e como se compõe.

```mermaid
mindmap
  root((Skill<br/>S = C, π, T, R))
    Definição Formal
      C — Condições de aplicabilidade
        ::icon(fa fa-question)
        Quando invocar?
      π — Política de execução
        ::icon(fa fa-cogs)
        Como opera?
      T — Critérios de terminação
        ::icon(fa fa-stop)
        Quando conclui?
      R — Interface reutilizável
        ::icon(fa fa-plug)
        Como outras skills a invocam?
    O que NÃO é Skill
      Tool
        Atômica
        Sem política interna
      Plan
        Instância única
        Não reutilizável
      Memory
        Declarativa
        Não procedural
    Ciclo de Vida
      1 Descoberta
      2 Prática
      3 Destilação
      4 Armazenamento
      5 Composição
      6 Avaliação
      7 Atualização
    Espectro de Autonomia
      Baixa autonomia
        Metadata controlada por humanos
      Média autonomia
        Padrões intermediários
      Alta autonomia
        Meta-skills que geram skills
    Composição Hierárquica
      Alto nível
        invoca Médio
          invoca Baixo
      Árvore funcional
      Complexo a partir do simples
```

---

## 2. Anatomia da Skill — A Quádrupla S = (C, π, T, R)

O coração da definição. Cada componente responde a uma pergunta distinta.

```mermaid
flowchart TB
    S(["<b>Skill S</b><br/>Unidade procedural<br/>reutilizável"])

    C["<b>C</b> — Condições<br/><i>Quando invocar?</i><br/>Pré-condições, gatilhos,<br/>contexto de uso"]
    PI["<b>π</b> — Política<br/><i>Como operar?</i><br/>Lógica interna, passos,<br/>decisões, sub-chamadas"]
    T["<b>T</b> — Terminação<br/><i>Quando concluir?</i><br/>Critérios de sucesso,<br/>falha, timeout"]
    R["<b>R</b> — Interface<br/><i>Como ser invocada?</i><br/>Assinatura, contrato,<br/>inputs/outputs"]

    S --> C
    S --> PI
    S --> T
    S --> R

    C -.->|"ativa"| PI
    PI -.->|"encerra via"| T
    R -.->|"expõe"| S

    style S fill:#1e40af,stroke:#1e3a8a,color:#fff,stroke-width:2px
    style C fill:#dbeafe,stroke:#1e40af,color:#0f172a
    style PI fill:#dbeafe,stroke:#1e40af,color:#0f172a
    style T fill:#dbeafe,stroke:#1e40af,color:#0f172a
    style R fill:#dbeafe,stroke:#1e40af,color:#0f172a
```

---

## 3. Diferenciação Conceitual — Skill vs. Tool vs. Plan vs. Memory

A definição de skill importa porque separa de três conceitos frequentemente confundidos com ela.

```mermaid
flowchart LR
    subgraph SK["<b>SKILL</b>"]
        direction TB
        s1[Procedural]
        s2[Reutilizável]
        s3[Tem política π]
        s4[Composável]
    end

    subgraph TO["Tool"]
        direction TB
        t1[Atômica]
        t2[Sem política interna]
        t3[Chamada única]
    end

    subgraph PL["Plan"]
        direction TB
        p1[Instância única]
        p2[Não reutilizável]
        p3[Específico a um objetivo]
    end

    subgraph ME["Memory"]
        direction TB
        m1[Declarativa]
        m2[Não procedural]
        m3[Armazena fatos, não ações]
    end

    SK -.->|"usa"| TO
    SK -.->|"gera"| PL
    SK -.->|"consulta"| ME

    style SK fill:#1e40af,stroke:#1e3a8a,color:#fff
    style TO fill:#fef3c7,stroke:#d97706,color:#0f172a
    style PL fill:#fce7f3,stroke:#be185d,color:#0f172a
    style ME fill:#dcfce7,stroke:#15803d,color:#0f172a
```

---

## 4. Ciclo de Vida — Os 7 Estágios

A skill não é estática. Ela nasce, amadurece, é usada, avaliada e atualizada.

```mermaid
flowchart LR
    A([1. Descoberta]) --> B([2. Prática])
    B --> C([3. Destilação])
    C --> D([4. Armazenamento])
    D --> E([5. Composição])
    E --> F([6. Avaliação])
    F --> G([7. Atualização])
    G -.->|"refina"| D
    G -.->|"descobre novas"| A

    A:::discover
    B:::practice
    C:::distill
    D:::store
    E:::compose
    F:::eval
    G:::update

    classDef discover fill:#fef3c7,stroke:#d97706,color:#0f172a
    classDef practice fill:#fed7aa,stroke:#c2410c,color:#0f172a
    classDef distill fill:#fecaca,stroke:#b91c1c,color:#0f172a
    classDef store fill:#ddd6fe,stroke:#6d28d9,color:#0f172a
    classDef compose fill:#bfdbfe,stroke:#1d4ed8,color:#0f172a
    classDef eval fill:#a7f3d0,stroke:#047857,color:#0f172a
    classDef update fill:#fbcfe8,stroke:#be185d,color:#0f172a
```

**Leitura rápida de cada estágio:**

| Estágio | O que acontece |
|---|---|
| **1. Descoberta** | Identificar uma capacidade candidata a virar skill |
| **2. Prática** | Exercitar a capacidade em contexto real |
| **3. Destilação** | Extrair o padrão reutilizável da prática |
| **4. Armazenamento** | Persistir a skill num repositório acessível |
| **5. Composição** | Combinar com outras skills para formar capacidades maiores |
| **6. Avaliação** | Medir eficácia, custo, taxa de sucesso |
| **7. Atualização** | Refinar, substituir ou aposentar a skill |

---

## 5. Espectro de Autonomia — Os 7 Padrões de Design

Quem controla a skill? Varia de controle humano total até meta-skills que geram outras skills.

```mermaid
flowchart LR
    H["👤<br/><b>Controle Humano</b><br/>Skills carregadas via<br/>metadata controlada<br/>por humanos"]
    M["⚙️<br/><b>Padrões Intermediários</b><br/>Humano define contorno,<br/>agente decide execução<br/>e composição"]
    A["🤖<br/><b>Meta-Skills</b><br/>Skills totalmente autônomas<br/>que geram, avaliam e<br/>atualizam outras skills"]

    H ==>|"mais autonomia →"| M
    M ==>|"mais autonomia →"| A

    style H fill:#dbeafe,stroke:#1e40af,color:#0f172a,stroke-width:2px
    style M fill:#ddd6fe,stroke:#6d28d9,color:#0f172a,stroke-width:2px
    style A fill:#fce7f3,stroke:#be185d,color:#0f172a,stroke-width:2px
```

> **Ponto-chave:** o espectro tem 7 padrões, não apenas 3. Os extremos são ilustrativos — a maior parte do design real vive no meio.

---

## 6. Composição Hierárquica — Onde a arquitetura ganha poder

Esta é a ideia central operacional: **skills se compõem em árvore**. Capacidades complexas emergem de capacidades simples reutilizáveis.

```mermaid
flowchart TB
    HL["<b>Skill de Alto Nível</b><br/><i>Ex: Redigir relatório trimestral</i>"]

    ML1["<b>Skill Média</b><br/>Coletar dados"]
    ML2["<b>Skill Média</b><br/>Analisar tendências"]
    ML3["<b>Skill Média</b><br/>Formatar documento"]

    LL1["<b>Skill Baixa</b><br/>Consultar BD"]
    LL2["<b>Skill Baixa</b><br/>Ler planilha"]
    LL3["<b>Skill Baixa</b><br/>Calcular média móvel"]
    LL4["<b>Skill Baixa</b><br/>Detectar outlier"]
    LL5["<b>Skill Baixa</b><br/>Aplicar template"]
    LL6["<b>Skill Baixa</b><br/>Exportar PDF"]

    HL --> ML1
    HL --> ML2
    HL --> ML3

    ML1 --> LL1
    ML1 --> LL2
    ML2 --> LL3
    ML2 --> LL4
    ML3 --> LL5
    ML3 --> LL6

    style HL fill:#1e40af,stroke:#1e3a8a,color:#fff,stroke-width:2px
    style ML1 fill:#3b82f6,stroke:#1e40af,color:#fff
    style ML2 fill:#3b82f6,stroke:#1e40af,color:#fff
    style ML3 fill:#3b82f6,stroke:#1e40af,color:#fff
    style LL1 fill:#bfdbfe,stroke:#1e40af,color:#0f172a
    style LL2 fill:#bfdbfe,stroke:#1e40af,color:#0f172a
    style LL3 fill:#bfdbfe,stroke:#1e40af,color:#0f172a
    style LL4 fill:#bfdbfe,stroke:#1e40af,color:#0f172a
    style LL5 fill:#bfdbfe,stroke:#1e40af,color:#0f172a
    style LL6 fill:#bfdbfe,stroke:#1e40af,color:#0f172a
```

**Por que a hierarquia não é decorativa:**

- Skills de baixo nível são **reutilizadas** por múltiplas skills de nível médio
- Mudar uma skill de baixo nível propaga melhoria para todas que a invocam
- Novas capacidades de alto nível são montadas sem reescrever a base
- A árvore é o mecanismo real de **acúmulo de capacidade** no agente

---

## 7. Síntese — Como tudo se conecta

```mermaid
flowchart TB
    DEF["<b>Definição Formal</b><br/>S = (C, π, T, R)"]
    DIST["<b>Distinções</b><br/>≠ Tool, Plan, Memory"]
    CICLO["<b>Ciclo de Vida</b><br/>7 estágios"]
    AUT["<b>Espectro de Autonomia</b><br/>7 padrões de design"]
    COMP["<b>Composição Hierárquica</b><br/>Árvore de skills"]

    DEF -->|"dá critério para"| DIST
    DEF -->|"é o objeto que atravessa"| CICLO
    CICLO -->|"posiciona-se no"| AUT
    DEF -->|"habilita via R"| COMP
    COMP -->|"é onde emergem"| CAPS(["<b>Capacidades Complexas</b>"])

    style DEF fill:#1e40af,stroke:#1e3a8a,color:#fff,stroke-width:2px
    style CAPS fill:#059669,stroke:#047857,color:#fff,stroke-width:2px
    style DIST fill:#fef3c7,stroke:#d97706,color:#0f172a
    style CICLO fill:#ddd6fe,stroke:#6d28d9,color:#0f172a
    style AUT fill:#fce7f3,stroke:#be185d,color:#0f172a
    style COMP fill:#bfdbfe,stroke:#1e40af,color:#0f172a
```

---

## Como usar este arquivo

- **Obsidian / Notion / GitHub**: cole o conteúdo diretamente — todos renderizam Mermaid nativamente.
- **VSCode**: instale a extensão *Markdown Preview Mermaid Support*.
- **Exportar para Excalidraw**: cole qualquer bloco Mermaid em [mermaid-to-excalidraw](https://mermaid-to-excalidraw.vercel.app/).
- **Slides**: os diagramas 2, 4, 5 e 6 funcionam isolados como slides individuais.

---

*Fonte conceitual: Jiang et al., SoK on Skill Engineering.*
