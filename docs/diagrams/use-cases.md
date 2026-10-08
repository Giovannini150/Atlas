# Casos de Uso - Atlas

Este documento descreve o que cada tipo de usuário pode fazer na plataforma Atlas.

## Atores

| Ator | Descrição |
|---|---|
| Aluno | Organiza a vida acadêmica, consulta materiais e conversa com o assistente de IA |
| Administrador | Gerencia alunos, cursos, disciplinas e acompanha relatórios |
| Módulo de IA | Sistema que planeja estudos, analisa desempenho e responde perguntas |

## Diagrama de casos de uso

```mermaid
flowchart LR
    A(["Aluno"])
    ADM(["Administrador"])
    IA(["Módulo de IA"])

    subgraph SEG["Segurança"]
        S1([Autenticar usuário])
        S2([Gerenciar sessão])
    end

    subgraph DASH["Dashboard"]
        D1([Ver atividades de hoje e urgentes])
        D2([Ver pendências e próximas provas])
        D3([Ver desempenho e resumo da semana])
        D4([Ver recomendações da IA])
    end

    subgraph ORG["Calendário e Tarefas"]
        T1([Cadastrar disciplinas, provas e trabalhos])
        T2([Criar tarefas, prazos e prioridades])
        T3([Alterar status e concluir tarefas])
        T4([Visualizar calendário])
        T5([Receber notificações de prazos])
    end

    subgraph MAT["Materiais"]
        M1([Fazer upload de materiais])
        M2([Organizar por curso, semestre, disciplina e tipo])
        M3([Criar anotações])
        M4([Salvar links úteis])
    end

    subgraph CHAT["Assistente IA"]
        C1([Consultar informações em linguagem natural])
        C2([Gerar planejamento de estudos])
        C3([Analisar desempenho])
    end

    subgraph ADMIN["Painel Administrativo"]
        AD1([Gerir alunos])
        AD2([Gerir cursos, semestres e turmas])
        AD3([Gerir disciplinas e professores])
        AD4([Gerar relatórios e indicadores])
    end

    A --> S1
    ADM --> S1
    S1 -. "include" .-> S2

    A --> D1 & D2 & D3 & D4
    A --> T1 & T2 & T3 & T4 & T5
    A --> M1 & M2 & M3 & M4
    A --> C1 & C2 & C3
    ADM --> AD1 & AD2 & AD3 & AD4

    D4 -.-> IA
    C1 -.-> IA
    C2 -.-> IA
    C3 -.-> IA

    T2 -. "include" .-> T5
    M1 -. "include" .-> M2
    C2 -. "include" .-> T4

    classDef actor fill:#1F2937,stroke:#111827,color:#FFFFFF
    classDef seg fill:#FDE68A,stroke:#B45309,color:#1F2937
    classDef dash fill:#BFDBFE,stroke:#1D4ED8,color:#1F2937
    classDef org fill:#BBF7D0,stroke:#15803D,color:#1F2937
    classDef mat fill:#FBCFE8,stroke:#BE185D,color:#1F2937
    classDef chat fill:#DDD6FE,stroke:#6D28D9,color:#1F2937
    classDef admin fill:#FED7AA,stroke:#C2410C,color:#1F2937

    class A,ADM,IA actor
    class S1,S2 seg
    class D1,D2,D3,D4 dash
    class T1,T2,T3,T4,T5 org
    class M1,M2,M3,M4 mat
    class C1,C2,C3 chat
    class AD1,AD2,AD3,AD4 admin
```

## Casos de uso por módulo

### Área do Aluno

| Módulo | Casos de uso |
|---|---|
| Dashboard | Atividades de hoje, urgentes, pendentes, próximas provas, desempenho, recomendações da IA |
| Calendário e Tarefas | Provas, trabalhos, atividades, aulas, eventos, prazos, notificações |
| Materiais | Upload, organização por curso, semestre, disciplina e tipo, anotações, links úteis |
| Assistente IA | Perguntas em linguagem natural, planejamento de estudos, análise de desempenho |

### Painel Administrativo

| Módulo | Casos de uso |
|---|---|
| Gestão de alunos | Cadastro, atualização, consulta, situação acadêmica |
| Gestão de cursos | Cursos, semestres, turmas, disciplinas |
| Gestão de disciplinas | Cadastro, professores, turmas, conteúdos |
| Relatórios | Desempenho, atividades, estatísticas, indicadores acadêmicos |

## Exemplos de interação com o assistente

- "Quais atividades tenho para essa semana?"
- "Qual é minha próxima prova?"
- "O que devo estudar hoje?"
- "Quais trabalhos estão próximos do prazo?"
- "Como está meu desempenho em cada disciplina?"
