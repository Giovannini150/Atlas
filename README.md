# 📚 Atlas

> Plataforma inteligente para organização, acompanhamento e planejamento da vida acadêmica.

<p align="center">

<a href="#-sobre">
<img src="https://img.shields.io/badge/Sobre-4A90E2?style=for-the-badge&logo=bookstack&logoColor=white">
</a>

<a href="#-inteligência-artificial">
<img src="https://img.shields.io/badge/Inteligência%20Artificial-00B894?style=for-the-badge&logo=openai&logoColor=white">
</a>

<a href="#-sistema">
<img src="https://img.shields.io/badge/Sistema-F39C12?style=for-the-badge&logo=googleclassroom&logoColor=white">
</a>

<a href="#-tecnologias">
<img src="https://img.shields.io/badge/Tecnologias-E74C3C?style=for-the-badge&logo=stackshare&logoColor=white">
</a>

</p>

<p align="center">
  <strong>📚 Organize. Planeje. Aprenda. Evolua.</strong>
</p>

---

## 📌 Sobre

O **Atlas** é uma plataforma de gestão acadêmica baseada em **Inteligência Artificial**, desenvolvida para auxiliar estudantes na organização de sua rotina acadêmica.

A plataforma centraliza informações como disciplinas, provas, trabalhos, atividades, materiais e prazos em um único ambiente.

Além da organização, o Atlas utiliza Inteligência Artificial para analisar as atividades acadêmicas do estudante e fornecer recomendações de estudo, identificação de prioridades e acompanhamento do desempenho.

A proposta é transformar uma rotina acadêmica desorganizada em uma experiência mais **simples, centralizada e inteligente**.

---

## 🎯 Objetivo

O objetivo do Atlas é facilitar a organização da vida acadêmica dos estudantes, reduzindo:

* Esquecimento de prazos;
* Acúmulo de atividades;
* Falta de organização;
* Dificuldade na definição de prioridades;
* Falta de acompanhamento do desempenho;
* Desorganização dos materiais de estudo.

A plataforma busca oferecer ao estudante uma visão clara de suas responsabilidades e ajudá-lo a decidir **o que estudar, quando estudar e quais atividades devem ser priorizadas**.

---

# 🏗️ Arquitetura

A arquitetura do Atlas é dividida em **Frontend, Backend, Inteligência Artificial e Banco de Dados**.

```mermaid
flowchart TB

    subgraph Student["Frontend — Aluno (React + TypeScript)"]
        UI["Interface do Aluno"]
        Dashboard["Dashboard"]
        Calendar["Calendário Acadêmico"]
        Tasks["Tarefas e Prazos"]
        Materials["Materiais"]
        AssistantUI["Assistente IA"]

        UI --> Dashboard
        UI --> Calendar
        UI --> Tasks
        UI --> Materials
        UI --> AssistantUI
    end

    subgraph Admin["Frontend — Administração (React)"]
        AdminUI["Painel Administrativo"]
        Students["Gestão de Alunos"]
        Courses["Gestão de Cursos"]
        Subjects["Gestão de Disciplinas"]
        Reports["Relatórios"]

        AdminUI --> Students
        AdminUI --> Courses
        AdminUI --> Subjects
        AdminUI --> Reports
    end

    subgraph Backend["Backend (Go)"]
        API["API Gateway / REST"]
        Auth["Autenticação"]
        Academic["Serviço Acadêmico"]
        TasksService["Serviço de Atividades"]
        CalendarService["Serviço de Calendário"]
        MaterialsService["Serviço de Materiais"]
        AIService["Orquestrador de IA"]
        Notifications["Notificações"]
        Metrics["Métricas"]
    end

    subgraph IA["Camada de Inteligência Artificial"]
        Planner["Planejador de Estudos"]
        Assistant["Assistente Acadêmico"]
        Analyzer["Analisador de Desempenho"]
        Ollama["Ollama"]
        Qwen["Qwen"]
    end

    DB[("PostgreSQL")]

    Student --> API
    AdminUI --> API

    API --> Auth
    API --> Academic
    API --> TasksService
    API --> CalendarService
    API --> MaterialsService
    API --> AIService
    API --> Notifications
    API --> Metrics

    Auth --> DB
    Academic --> DB
    TasksService --> DB
    CalendarService --> DB
    MaterialsService --> DB
    Metrics --> DB

    AIService --> Planner
    AIService --> Assistant
    AIService --> Analyzer

    Planner --> Qwen
    Assistant --> Qwen
    Analyzer --> Qwen

    Qwen --> Ollama
```

O arquivo-fonte da arquitetura está disponível em:

`arquitetura.puml`

---

# 🛠️ Tecnologias

## Backend

![Go](https://img.shields.io/badge/Go-00ADD8?style=for-the-badge\&logo=go\&logoColor=white)

Backend desenvolvido utilizando **Go**, responsável pela API, regras de negócio e integração entre os serviços.

---

## Frontend

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge\&logo=react\&logoColor=61DAFB)

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge\&logo=typescript\&logoColor=white)

Interface desenvolvida utilizando **React + TypeScript**.

---

## Banco de Dados

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge\&logo=postgresql\&logoColor=white)

Responsável pelo armazenamento das informações acadêmicas.

---

## Inteligência Artificial

![Ollama](https://img.shields.io/badge/Ollama-000000?style=for-the-badge\&logo=ollama\&logoColor=white)

![Qwen](https://img.shields.io/badge/Qwen-5A4FCF?style=for-the-badge)

Utilização de modelos locais através do **Ollama**, com o **Qwen** como modelo de linguagem.

---

## Arquitetura e Versionamento

![PlantUML](https://img.shields.io/badge/PlantUML-FABD14?style=for-the-badge\&logo=uml\&logoColor=black)

![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge\&logo=git\&logoColor=white)

![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge\&logo=github\&logoColor=white)

---

### Comunicação

A comunicação entre frontend e backend será realizada utilizando:

* REST API;
* WebSocket.

---

<p align="center">

<strong>📚 Atlas — Organize sua vida acadêmica de forma inteligente.</strong>

<br><br>
