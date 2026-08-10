# 🔍 InsightLens

> Sistema de inteligência artificial para transformar interações comerciais dispersas em insights estruturados e acionáveis.

O **InsightLens** é uma aplicação desenvolvida para analisar automaticamente o histórico de relacionamento entre clientes e o time comercial.

A solução coleta diferentes tipos de interação armazenados no **HubSpot CRM** — como mensagens de WhatsApp registradas via Cooby, ligações e resumos de reuniões — e utiliza inteligência artificial para consolidar essas informações em uma visão única sobre cada cliente.

O objetivo é reduzir o esforço necessário para interpretar históricos extensos de comunicação e transformar dados não estruturados em informações úteis para **Sales Intelligence, acompanhamento comercial e tomada de decisão**.

O projeto foi desenvolvido para uso interno na **Lastro**.

---

## 🎯 Problema

Durante o processo comercial, informações importantes sobre um cliente podem ficar distribuídas entre diferentes canais:

- mensagens de WhatsApp;
- ligações;
- reuniões;
- notas internas;
- registros do CRM.

Isso torna mais difícil responder rapidamente perguntas como:

- Quais são as principais objeções desse cliente?
- Ele demonstrou intenção de compra?
- Quais são os próximos passos?
- Como evoluiu o interesse do lead?
- Existem sinais de fechamento?
- Quais trechos das conversas são mais relevantes?
- Qual é o contexto geral do relacionamento?

O InsightLens automatiza esse processo.

---

## ⚙️ Como funciona

O fluxo principal da aplicação é:

```text
HubSpot CRM
   │
   ├── WhatsApp / Cooby
   ├── Ligações
   └── Notas de reuniões / Elephan
   │
   ▼
Coleta e normalização dos dados
   │
   ▼
Processamento com OpenAI
   │
   ▼
Insights estruturados em JSON
   │
   ▼
Insight consolidado
   │
   ▼
Criação automática de notas no HubSpot
```

A aplicação utiliza o **ID de um contato no HubSpot** para recuperar as interações relacionadas a ele.

Depois disso, as informações são normalizadas, enviadas ao modelo de IA e transformadas em insights estruturados.

---

## 🧠 Insights gerados

A IA retorna os resultados utilizando uma estrutura padronizada em JSON.

Entre as informações geradas estão:

### Resumo da interação

Principais acontecimentos e informações relevantes encontrados nas conversas.

### Objeções

Identificação de possíveis barreiras comerciais, como:

- preço;
- prioridade;
- timing;
- necessidade;
- aprovação interna.

### Sinais de fechamento

Identificação de comportamentos que podem indicar maior intenção de compra, como:

- definição de timeline;
- discussão de orçamento;
- solicitação de proposta;
- participação de múltiplos stakeholders.

### Próximos passos

Sugestões de ações que podem ser realizadas pelo time comercial após analisar o histórico.

### Lead scoring

O sistema gera indicadores de scoring para ajudar a interpretar a evolução do lead durante as interações.

### Classificação da interação

As interações também podem ser classificadas de acordo com a qualidade do relacionamento identificado.

### Recomendações

A IA pode sugerir ações comerciais com base no contexto encontrado.

### Trechos relevantes

Partes importantes das conversas podem ser destacadas para facilitar a análise humana.

---

## 📡 Fontes de dados

Atualmente o InsightLens trabalha com três principais fontes de interação.

### 💬 WhatsApp / Cooby

As mensagens registradas pelo **Cooby dentro do HubSpot** são recuperadas através dos objetos de comunicação do CRM.

O sistema:

- busca comunicações associadas ao contato;
- identifica registros provenientes do Cooby;
- extrai o conteúdo das mensagens;
- organiza as interações cronologicamente;
- envia o histórico para análise da IA.

---

### 📞 Ligações

O InsightLens também consulta as ligações associadas ao contato no HubSpot.

São utilizados dados como:

- título da ligação;
- resultado;
- duração;
- observações;
- resumo da chamada;
- timestamp.

Quando os dados possuem conteúdo HTML, o sistema realiza a limpeza e transformação para texto antes do processamento.

---

### 📝 Reuniões / Elephan

Notas de reuniões registradas no HubSpot também podem fazer parte da análise.

O sistema identifica notas relacionadas à **Elephan**, extrai o conteúdo relevante e adiciona essas informações ao contexto utilizado pela IA.

---

## 🧩 Insight consolidado

Além de analisar cada fonte separadamente, o InsightLens gera um **insight geral do cliente**.

Esse processamento combina:

```text
WhatsApp
   +
Ligações
   +
Reuniões
   =
Insight consolidado
```

A IA cruza as informações das diferentes fontes e busca identificar:

- padrões;
- divergências;
- objeções recorrentes;
- sinais de interesse;
- mudanças no relacionamento;
- próximos passos;
- informações complementares entre os canais.

Caso uma das fontes não possua informações, o processamento continua utilizando apenas os dados disponíveis.

---

## 🔄 Integração com HubSpot

O HubSpot funciona como a principal fonte de dados e também como destino dos insights produzidos.

O InsightLens utiliza a API do HubSpot para:

- buscar comunicações associadas a contatos;
- consultar mensagens registradas pelo Cooby;
- recuperar ligações;
- recuperar notas;
- processar resumos de reuniões;
- criar novas notas;
- associar automaticamente os insights ao contato analisado.

Isso permite que o resultado da IA retorne para o mesmo ambiente utilizado pelo time comercial.

---

## 🤖 Inteligência Artificial

A camada de inteligência artificial utiliza a **OpenAI API**.

O projeto trabalha com respostas estruturadas em JSON para tornar a saída da IA previsível e utilizável por outros sistemas.

Exemplo simplificado:

```json
{
  "resumo_bullets": [
    "Cliente demonstrou interesse na solução"
  ],
  "principais_objeções": [
    "preço",
    "prioridade"
  ],
  "sinais_fechamento": [
    "timeline",
    "pedido_proposta"
  ],
  "proximos_passos": [
    {
      "descricao": "Enviar proposta comercial",
      "prazo_iso": ""
    }
  ],
  "label_interacao": "boa",
  "lead_scoring_pre": 60,
  "lead_scoring_pos": 80,
  "recomendacoes": [
    "Realizar follow-up"
  ]
}
```

Essa estrutura facilita a utilização dos resultados em:

- CRMs;
- automações;
- dashboards;
- APIs;
- workflows comerciais.

---

## 🌐 API

O projeto disponibiliza uma API construída com **FastAPI**.

### Endpoint

```http
POST /api/insights
```

### Exemplo de payload

```json
{
  "contactId": "123456789",
  "createNote": true
}
```

Também é possível utilizar:

```json
{
  "contactId": "123456789",
  "createNote": true,
  "sinceEpochMs": 1704067200000
}
```

O campo `sinceEpochMs` permite limitar a análise a interações posteriores a determinado momento.

---

## 🔐 Autenticação da API

Opcionalmente, a API pode ser protegida utilizando um Bearer Token.

Exemplo:

```http
Authorization: Bearer YOUR_AGENT_API_TOKEN
```

A autenticação é habilitada através da variável de ambiente:

```env
AGENT_API_TOKEN=
```

---

## 📤 Resultado da API

Uma execução bem-sucedida pode retornar informações como:

```json
{
  "ok": true,
  "notes": {
    "cooby": "note_id",
    "calls": "note_id",
    "elephan": "note_id",
    "geral": "note_id"
  },
  "has_calls": true,
  "has_cooby": true,
  "has_elephan": true,
  "scores": {
    "cooby_pre": 50,
    "cooby_pos": 70,
    "calls_pre": 60,
    "calls_pos": 75,
    "elephan_pre": 65,
    "elephan_pos": 80,
    "geral_pre": 60,
    "geral_pos": 82
  }
}
```

Quando `createNote` está habilitado, os insights são automaticamente criados como notas associadas ao contato no HubSpot.

---

## 🛠️ Stack

### Backend

- **Python**
- **FastAPI**
- **Pydantic**

### Inteligência Artificial

- **OpenAI API**
- Structured JSON outputs
- Prompt Engineering
- NLP / análise de linguagem natural

### Integrações

- **HubSpot CRM API**
- **Cooby / WhatsApp**
- **Elephan**

### Processamento

- Requests
- Regex
- Parsing e limpeza de HTML
- Normalização de dados

### Infraestrutura

- **Vercel**
- Python Serverless Functions

---

## 📁 Estrutura do projeto

```text
insightlens/
│
├── api/
│   └── insights.py
│
├── hubspot_client.py
├── insights_agent.py
├── run_agent.py
├── requirements.txt
├── vercel.json
└── README.md
```

### `hubspot_client.py`

Responsável pela comunicação com o HubSpot.

Inclui funções para:

- buscar comunicações;
- buscar ligações;
- buscar notas;
- extrair mensagens do Cooby;
- identificar reuniões da Elephan;
- limpar HTML;
- criar notas;
- associar notas aos contatos.

### `insights_agent.py`

Responsável pela camada de inteligência artificial.

Contém:

- definição do formato de resposta;
- prompts;
- processamento de WhatsApp;
- combinação de WhatsApp e ligações;
- consolidação entre WhatsApp, ligações e reuniões;
- geração dos insights estruturados.

### `api/insights.py`

Implementa a API FastAPI responsável pela orquestração completa:

1. recebe o `contactId`;
2. consulta as fontes no HubSpot;
3. prepara os dados;
4. gera os insights;
5. cria notas no CRM;
6. gera uma análise geral consolidada;
7. retorna scores e metadados da execução.

### `run_agent.py`

Permite executar parte do agente diretamente através da linha de comando.

---

## 🚀 Instalação

Clone o projeto:

```bash
git clone https://github.com/flavio-silva0/insightlens.git
cd insightlens
```

Crie um ambiente virtual:

```bash
python -m venv .venv
```

Ative o ambiente.

Linux/macOS:

```bash
source .venv/bin/activate
```

Windows:

```bash
.venv\Scripts\activate
```

Instale as dependências:

```bash
pip install -r requirements.txt
```

---

## 🔑 Variáveis de ambiente

O projeto utiliza as seguintes variáveis:

```env
OPENAI_API_KEY=
HUBSPOT_TOKEN=
AGENT_API_TOKEN=
```

### `OPENAI_API_KEY`

Chave utilizada para acessar a OpenAI API.

### `HUBSPOT_TOKEN`

Private App Token utilizado para acessar os dados do HubSpot e criar notas.

### `AGENT_API_TOKEN`

Token opcional utilizado para proteger o endpoint da aplicação.

> Nunca adicione credenciais reais diretamente ao código ou ao repositório.

---

## 💻 Execução via CLI

Também é possível analisar um contato diretamente pelo terminal.

```bash
python run_agent.py --contact-id CONTACT_ID
```

Para executar sem criar uma nota no HubSpot:

```bash
python run_agent.py --contact-id CONTACT_ID --dry-run
```

O modo `dry-run` mostra o resultado sem persistir a nota no CRM.

---

## ☁️ Deploy

O projeto possui configuração para deploy utilizando **Vercel Python Serverless Functions**.

O arquivo:

```text
vercel.json
```

direciona as requisições para:

```text
api/insights.py
```

No ambiente de produção, configure as variáveis:

```env
OPENAI_API_KEY
HUBSPOT_TOKEN
AGENT_API_TOKEN
```

---

## 🏗️ Arquitetura

```mermaid
flowchart TD
    A[HubSpot CRM] --> B[WhatsApp / Cooby]
    A --> C[Ligações]
    A --> D[Notas / Elephan]

    B --> E[Normalização]
    C --> E
    D --> E

    E --> F[InsightLens AI]
    F --> G[OpenAI API]

    G --> H[Insights estruturados]

    H --> I[Resumo]
    H --> J[Objeções]
    H --> K[Sinais de fechamento]
    H --> L[Lead Scoring]
    H --> M[Próximos passos]
    H --> N[Recomendações]

    H --> O[Insight consolidado]
    O --> P[Notas no HubSpot]
```

---

## 💡 Principais conceitos explorados

O desenvolvimento do InsightLens envolveu conceitos como:

- integração entre APIs;
- arquitetura de backend;
- processamento de dados não estruturados;
- integração entre CRM e IA;
- prompt engineering;
- structured outputs;
- processamento de linguagem natural;
- automação de processos comerciais;
- Sales Intelligence;
- lead scoring;
- serverless functions;
- normalização de dados;
- tratamento de HTML;
- autenticação de APIs.

---

## 🎯 Objetivo do projeto

Mais do que simplesmente resumir conversas, o InsightLens foi criado para transformar o histórico de relacionamento de um cliente em **contexto comercial acionável**.

Em vez de um vendedor precisar consultar manualmente mensagens, ligações e reuniões para entender o status de uma oportunidade, o sistema centraliza e interpreta essas informações automaticamente.

O resultado é uma camada de inteligência sobre o CRM capaz de ajudar a responder:

> **O que aconteceu com esse cliente, qual é o contexto atual e qual deveria ser o próximo passo?**

---

## 👨‍💻 Autor

**Flavio Silva**

Projeto desenvolvido como uma solução de inteligência aplicada a processos comerciais e Revenue Operations.

---

## 📄 Observação

Este repositório representa uma implementação de uma solução desenvolvida para um contexto real de operação comercial.

Integrações que dependem de contas, permissões ou dados internos precisam ser configuradas individualmente através das respectivas APIs e variáveis de ambiente.
