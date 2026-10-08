# Arquitetura - Atlas

Este documento descreve a arquitetura geral da plataforma Atlas e o modelo de domínio.

## Visão geral

```mermaid
flowchart TB
    U1(["Aluno"])
    U2(["Administrador"])

    subgraph FE["Frontend"]
        direction LR
        F1[pages]
        F2[components]
        F3[hooks]
        F4[services]
        F5[types]
        F1 --> F2
        F1 --> F3
        F3 --> F4
        F4 --> F5
    end

    U1 --> FE
    U2 --> FE

    FE -- "REST API" --> GW
    FE <-- "WebSocket" --> NOTI

    subgraph BE["Backend - Go"]
        GW["API e middlewares<br/>autenticação, acesso, validação"]
        AUTH["auth<br/>login, sessões, perfis"]
        ACAD["academic<br/>cursos, turmas, disciplinas"]
        TASK["tasks<br/>provas, trabalhos, atividades"]
        CAL["calendar<br/>eventos e prazos"]
        MATS["materials<br/>materiais, anotações, links"]
        NOTI["notifications<br/>alertas em tempo real"]
        MET["metrics<br/>desempenho e relatórios"]

        GW --> AUTH
        GW --> ACAD
        GW --> TASK
        GW --> CAL
        GW --> MATS
        GW --> MET
        TASK --> CAL
        TASK --> NOTI
        CAL --> NOTI
        ACAD --> MET
        TASK --> MET
    end

    subgraph AI["Camada de IA"]
        PROMPTS["prompts"]
        PLAN["planner<br/>Planejador de Estudos"]
        ASSIST["assistant<br/>Assistente Acadêmico"]
        ANAL["analyzer<br/>Analisador de Desempenho"]
        PROMPTS --> PLAN
        PROMPTS --> ASSIST
        PROMPTS --> ANAL
    end

    LLM[["Modelo de linguagem - LLM"]]

    TASK --> PLAN
    TASK --> ASSIST
    MET --> ANAL
    PLAN --> LLM
    ASSIST --> LLM
    ANAL --> LLM
    ASSIST -. "respostas via WebSocket" .-> NOTI

    subgraph DATA["Dados"]
        DB[("Banco de dados<br/>migrations e seeds")]
        FS[("Armazenamento de arquivos<br/>materiais")]
    end

    AUTH --> DB
    ACAD --> DB
    TASK --> DB
    CAL --> DB
    MET --> DB
    NOTI --> DB
    MATS --> DB
    MATS --> FS
    PLAN --> DB
    ASSIST --> DB
    ANAL --> DB

    classDef user fill:#1F2937,stroke:#111827,color:#FFFFFF
    classDef fe fill:#BFDBFE,stroke:#1D4ED8,color:#1F2937
    classDef be fill:#BBF7D0,stroke:#15803D,color:#1F2937
    classDef ai fill:#DDD6FE,stroke:#6D28D9,color:#1F2937
    classDef data fill:#FED7AA,stroke:#C2410C,color:#1F2937
    classDef ext fill:#FDE68A,stroke:#B45309,color:#1F2937

    class U1,U2 user
    class F1,F2,F3,F4,F5 fe
    class GW,AUTH,ACAD,TASK,CAL,MATS,NOTI,MET be
    class PROMPTS,PLAN,ASSIST,ANAL ai
    class DB,FS data
    class LLM ext
```

## Comunicação

A comunicação entre frontend e backend é feita por:

- REST API: operações de cadastro, consulta e atualização;
- WebSocket: notificações e respostas do assistente em tempo real.

## Segurança

- Autenticação de usuários;
- Controle de acesso;
- Validação de dados;
- Proteção das APIs;
- Gerenciamento de sessões;
- Separação entre usuários e administradores.

## Modelo de domínio

```mermaid
classDiagram
    direction TB

    class Usuario {
        <<abstract>>
        +int id
        +String nome
        +String email
        +String senhaHash
        +autenticar()
        +encerrarSessao()
    }
    class Aluno {
        +String matricula
        +String situacaoAcademica
    }
    class Administrador {
        +String nivelAcesso
    }
    class Curso {
        +int id
        +String nome
    }
    class Turma {
        +int id
        +String codigo
    }
    class Semestre {
        +int id
        +String nome
        +Date inicio
        +Date fim
    }
    class Professor {
        +int id
        +String nome
    }
    class Disciplina {
        +int id
        +String nome
        +String codigo
        +calcularDesempenho() float
    }
    class Tarefa {
        <<abstract>>
        +int id
        +String titulo
        +String descricao
        +Date prazo
        +Prioridade prioridade
        +Status status
        +float nota
        +concluir()
        +estaAtrasada() boolean
    }
    class Prova {
        +Date dataProva
        +String conteudo
    }
    class Trabalho {
        +boolean emGrupo
    }
    class Atividade {
        +String tipo
    }
    class EventoAcademico {
        +int id
        +String titulo
        +TipoEvento tipo
        +DateTime dataHora
    }
    class Material {
        +int id
        +String nome
        +String tipoConteudo
        +String caminho
        +Date enviadoEm
    }
    class Anotacao {
        +int id
        +String conteudo
    }
    class LinkUtil {
        +int id
        +String url
    }
    class Notificacao {
        +int id
        +String mensagem
        +boolean lida
        +Date criadaEm
    }
    class PlanoEstudo {
        +int id
        +Date inicio
        +Date fim
        +int tempoDisponivelMin
    }
    class SessaoEstudo {
        +Date data
        +int duracaoMin
        +boolean concluida
    }
    class Recomendacao {
        +int id
        +String texto
        +Date geradaEm
    }
    class PlanejadorEstudos {
        +gerarPlano(aluno) PlanoEstudo
    }
    class AssistenteAcademico {
        +responder(pergunta) String
    }
    class AnalisadorDesempenho {
        +evolucaoNotas(aluno) Map
        +disciplinasDificeis(aluno) List
        +analisar(aluno) Recomendacao
    }
    class Dashboard {
        +atividadesHoje() List
        +urgentes() List
        +proximasProvas() List
        +desempenho() Map
    }
    class Relatorio {
        +String tipo
        +gerar() Documento
    }

    Usuario <|-- Aluno
    Usuario <|-- Administrador
    Curso "1" --> "*" Turma : possui
    Curso "1" --> "*" Semestre : organiza
    Semestre "1" --> "*" Disciplina : contém
    Turma "*" --> "*" Disciplina : cursa
    Professor "1" --> "*" Disciplina : leciona
    Aluno "*" --> "1" Turma : pertence
    Aluno "1" --> "*" Tarefa : cria
    Disciplina "1" --> "*" Tarefa : possui
    Tarefa <|-- Prova
    Tarefa <|-- Trabalho
    Tarefa <|-- Atividade
    Aluno "1" --> "*" EventoAcademico : visualiza
    Disciplina "1" --> "*" Material : organiza
    Disciplina "1" --> "*" Anotacao : possui
    Disciplina "1" --> "*" LinkUtil : possui
    Aluno "1" --> "*" Notificacao : recebe
    Aluno "1" --> "*" PlanoEstudo : recebe
    PlanoEstudo "1" *-- "*" SessaoEstudo : compõe
    Aluno "1" --> "*" Recomendacao : recebe
    PlanejadorEstudos ..> PlanoEstudo : gera
    PlanejadorEstudos ..> Tarefa : analisa
    AssistenteAcademico ..> Tarefa : consulta
    AnalisadorDesempenho ..> Recomendacao : gera
    AnalisadorDesempenho ..> Disciplina : analisa
    Aluno ..> Dashboard : acessa
    Aluno ..> AssistenteAcademico : conversa
    Administrador ..> Relatorio : gera
    Administrador ..> Curso : gerencia
    Administrador ..> Aluno : gerencia

```

## Estrutura do projeto

```
Atlas/
├── backend/
│   ├── cmd/
│   └── internal/
│       ├── auth/
│       ├── academic/
│       ├── tasks/
│       ├── calendar/
│       ├── materials/
│       ├── notifications/
│       └── metrics/
├── frontend/
│   └── src/
│       ├── components/
│       ├── pages/
│       ├── services/
│       ├── hooks/
│       └── types/
├── ai/
│   ├── prompts/
│   ├── planner/
│   ├── assistant/
│   └── analyzer/
├── database/
│   ├── migrations/
│   └── seeds/
├── docs/
│   ├── diagrams/
│   ├── requisitos/
│   ├── casos-de-uso/
│   └── prototipos/
└── README.md
```
