# 🏗️ Plataforma Inteligente | Almoxarifado & Gestão de Obras

[![Python 3.12](https://img.shields.io/badge/Python-3.12-blue.svg)](https://www.python.org/)
[![Django REST Framework](https://img.shields.io/badge/DRF-3.15-red.svg)](https://www.django-rest-framework.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16%20%2B%20pgvector-blue.svg)](https://github.com/pgvector/pgvector)
[![React](https://img.shields.io/badge/React-19%20(Vite)-61DAFB.svg)](https://react.dev/)
[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B.svg)](https://flutter.dev/)
[![Docker Compose](https://img.shields.io/badge/Docker-Ready-2496ED.svg)](https://www.docker.com/)

Microsserviço de alta densidade técnica projetado para transformar a gestão de almoxarifados e a rastreabilidade de materiais na construção civil. O sistema mitiga extravios e gargalos de suprimentos por meio de travas atômicas de concorrência, sincronização offline para canteiros de obras e um motor de Inteligência Artificial Generativa com busca semântica via RAG (Retrieval-Augmented Generation).

## 🎯 Destaques de Engenharia & Arquitetura

* **Concorrência Segura (Zero Race Conditions):** Controle transacional no banco com `select_for_update` (trava pessimista), retornando `409 Conflict` padronizado sob a RFC 7807 quando o saldo é disputado simultaneamente.
* **Busca Semântica & RAG:** Integração nativa com `pgvector` e indexação `HNSW` com distância de cosseno (`<=>`), permitindo encontrar insumos por termos coloquiais de obra e consultar diários (RDO) em linguagem natural com fallback resiliente.
* **Isolamento Multi-Tenant:** `TenantManager` injetado no Django ORM, garantindo segregação lógica estrita entre empresas clientes.
* **Estratégia de Cache e Filas:** Redis como message broker para o Celery (processamento assíncrono de OCR/NF e embeddings) e cache de catálogos quentes.
* **Mobile Offline-First:** Cliente Flutter com fila local persistida (`SQLite`), sincronizando requisições sequencialmente ao restabelecer conectividade.
* **Mutações Otimistas no Frontend:** React com TanStack Query para atualização instantânea de saldo em tela, com mecanismo de rollback automático em respostas de erro.

## 🛠️ Stack Tecnológica

* **Backend:** Python 3.12, Django 5, Django REST Framework, SimpleJWT, Celery, Structlog.
* **Banco de Dados & Vetores:** PostgreSQL 16 com extensão `pgvector`.
* **Cache & Mensageria:** Redis 7.
* **Frontend Web:** React 19, TypeScript, Vite, Tailwind CSS, TanStack Query, Zustand, Axios.
* **Mobile:** Flutter 3.x, Dart, Dio, Sqflite, Flutter Secure Storage.
* **Infraestrutura & Qualidade:** Docker, Docker Compose, Pytest, Locust, DRF-Spectacular (OpenAPI 3.0).

## 🚀 Como Executar Localmente

```bash
# 1. Clonar o repositório
git clone [https://github.com/ROSILENE05/plataforma-inteligente-core.git](https://github.com/ROSILENE05/plataforma-inteligente-core.git)
cd plataforma-inteligente-core

# 2. Subir PostgreSQL (com pgvector), Redis, Backend Django e Celery
docker compose up --build -d

# 3. Executar migrações do banco de dados
docker compose exec backend python manage.py migrate
```

## 🧪 Validação e Testes de Concorrência

```bash
# Executar teste automatizado de race condition (esperado status 409 sob concorrência)
docker compose exec backend pytest tests/test_estoque_concorrencia.py -v
```
