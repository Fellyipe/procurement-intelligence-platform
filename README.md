<h1 align="center">Sistema Inteligente para Descoberta de Oportunidades em Licitações Públicas</h1>

<p align="center">
  FastAPI | Next.js | Groq | PostgreSQL | Docker
</p>

<p align="center">
  <strong>Plataforma para coleta, enriquecimento semântico e matching entre empresas e licitações públicas.</strong>
</p>

> 🔒 **Nota de Portfólio:** Código privado devido a diretrizes de propriedade intelectual e viabilidade comercial. Este repositório documenta a arquitetura, decisões de engenharia e os desafios técnicos no desenvolvimento para fins de portfólio.

---

## 📝 Contexto

O Portal Nacional de Contratações Públicas (PNCP) centraliza dados de licitações públicas brasileiras, porém identificar oportunidades realmente relevantes ainda é um processo trabalhoso para muitas empresas.

Entre os principais desafios estão:

* 📦 Grande volume de licitações publicadas diariamente
* 🔍 Descrições heterogêneas e pouco padronizadas
* 🌐 Dados distribuídos em múltiplos endpoints
* ⚠️ Instabilidade operacional da API pública (timeouts e indisponibilidades)
* 🤝 Dificuldade em conectar o perfil comercial das empresas às oportunidades disponíveis

Este projeto consiste no desenvolvimento de uma plataforma para descoberta de oportunidades em licitações públicas, composta por um pipeline de coleta de dados, enriquecimento semântico com LLM, API REST e mecanismos de matching entre empresas e licitações.

---

## 🏗️ Arquitetura atual do Sistema

```mermaid
graph TD
    A(API Pública do PNCP) -->|Coleta incremental| B[Collector Worker]
    B -->|Licitações e Itens| C[(PostgreSQL)]
    C -->|Licitações pendentes| D[Analysis Worker]
    D -->|Prompt estruturado| E[Groq API<br/>Llama]
    E -->|Resultado estruturado| D
    D -->|Persiste análise| C
    C -->|Consulta de dados e análises| F[FastAPI]
    F -->|REST API| G[Frontend]

    F <--> H[(Redis)]

    G --> I(Empresas)
    I -->|Perfil Comercial| F
    F -->|Matching| C
```
---

## 🛠️ Stack

### Backend

- Python
- FastAPI
- SQLAlchemy
- PostgreSQL

### IA

- Groq API
- GPT-OSS

### Frontend

- Next.js
- React
- TypeScript
- Tailwind CSS

### Infraestrutura

- Docker
- Redis

---

## ⚙️ Funcionalidades Atuais

- Coleta incremental de licitações do PNCP
- Enriquecimento de dados utilizando LLM
- Geração automática de:
  - descrição normalizada
  - palavras-chave
  - setores relacionados
- Mecanismo de matching entre empresas e licitações
- Cadastro de perfil comercial da empresa
- Recomendações personalizadas de licitações
- Pesquisa e filtragem de oportunidades
- Visualização detalhada de licitações e itens
- Cache de resultados com Redis


---

## 🛡️ Desafios Técnicos

**1. Instabilidade da API do PNCP**
* **Problema:** Lentidão, timeouts e falhas em requisições consecutivas.
* **Solução:** Implementação de timeouts, retries controlados, delays entre chamadas e coleta incremental.

**2. Dados heterogêneos e pouco estruturados**
* **Problema:** As descrições das licitações variam significativamente em qualidade e nível de detalhe, podendo conter desde poucas palavras até longos textos administrativos, dificultando a identificação do objeto comercial da contratação.
* **Solução:** Utilização de LLM para normalizar as descrições, extrair palavras-chave relevantes e identificar os setores relacionados à contratação.

**3. Licitações multidisciplinares**
* **Problema:** Uma mesma licitação pode reunir produtos e serviços de diferentes áreas de atuação, tornando inadequada uma classificação única.
* **Solução:** Modelagem baseada em múltiplos setores relacionados, permitindo representar licitações que abrangem diferentes segmentos e melhorar estratégias de matching.

**4. Limitações dos modelos de linguagem**
* **Problema:** Modelos menores apresentaram baixa capacidade para interpretar corretamente descrições heterogêneas e identificar o contexto comercial das licitações.
* **Solução:** Comparação entre modelos executados localmente e modelos hospedados, resultando na adoção da API da Groq como estratégia para equilibrar qualidade das análises, latência operacional, custos e simplicidade da infraestrutura.

---

## 🖥️ Interface

### Oportunidades
<img width="1161" height="429" src="https://github.com/user-attachments/assets/b9af4912-1d73-4e38-8d68-aaecb0c7d335" />


### Detalhes da oportunidade
<img width="1159" height="730" src="https://github.com/user-attachments/assets/1bf803d3-b784-4a7e-91ff-5711e3460ba7" />


---

## 🚀 Próximos Passos

- Autenticação e gerenciamento de usuários e empresas
- Sistema de notificações de novas oportunidades
- Dashboard de recomendações
- Upload e gerenciamento de documentos
- Deploy e configuração do ambiente de produção
- Expansão do algoritmo de matching
