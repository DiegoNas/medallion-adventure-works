# Medallion Architecture – Adventure Works (Azure SQL → Databricks Lakehouse)

## Visão Geral
Este projeto demonstra a construção de um pipeline de dados moderno utilizando
a **arquitetura Medallion (Bronze, Silver e Gold)**, com dados do banco
**Adventure Works (Azure SQL)**, processados e persistidos em **Delta Lake** no Databricks.

O objetivo é simular um cenário real de engenharia de dados,
desde a ingestão de dados operacionais até a disponibilização de dados analíticos
prontos para consumo por ferramentas de BI como Power BI.

---

## 🏗️ Arquitetura Geral

**Fonte**
- Azure SQL Database (Adventure Works – OLTP)

**Plataforma**
- Databricks Lakehouse
- Delta Lake
- Unity Catalog / External Catalog
- Delta Live Tables (versão alternativa do pipeline)

**Camadas**
- Bronze → dados brutos
- Silver → dados tratados e validados
- Gold → modelo analítico (Star Schema)

---

## 🥉 Bronze – Ingestão de Dados

### Objetivo
A camada Bronze é responsável pela **ingestão inicial dos dados**, preservando:
- esquema original
- tipos de dados
- histórico
- rastreabilidade

Nenhuma transformação de negócio é aplicada nesta etapa.

### O que é feito
- Leitura dos dados via **External Catalog (Azure SQL)**
- Carga full das tabelas de origem
- Persistência em **Delta Lake**
- Inclusão de metadados técnicos:
  - `_ingestion_timestamp`
  - `_source_system`
  - `_load_type`

### Características
- Dados brutos
- Alta fidelidade à fonte
- Base para ingestões incrementais futuras

---

## 🥈 Silver – Tratamento e Qualidade dos Dados

### Objetivo
A camada Silver prepara os dados para consumo analítico, aplicando:
- limpeza
- padronização
- regras de negócio básicas
- controle de qualidade

### O que é feito
- Deduplicação de registros (uso de `ROW_NUMBER`)
- Padronização de colunas e tipos
- Conversão de datas e valores
- Validações de integridade
- Garantia de consistência entre entidades

### Exemplos
- Dimensões como **Product** e **Customer**
- Tabelas transacionais como **Sales Order**

---

## 🥇 Gold – Modelagem Analítica

### Objetivo
A camada Gold entrega dados prontos para análise, modelados em
**Star Schema**, focando em:
- desempenho
- simplicidade
- escalabilidade

### O que é feito
- Criação de tabelas dimensão
- Criação de tabelas fato
- Relacionamentos analíticos
- Otimização para ferramentas de BI

### Consumo
- Power BI
- Consultas SQL analíticas
- Dashboards executivos

---

## 🔄 Abordagens Implementadas

Este projeto apresenta **duas abordagens válidas** de engenharia de dados:

### 1️⃣ Pipeline com SQL / PySpark
- Controle total do fluxo
- Ideal para cenários customizados
- Fácil depuração

### 2️⃣ Delta Live Tables (DLT)
- Pipeline declarativo
- Controle automático de dependências
- Visualização gráfica do fluxo
- Ideal para ambientes produtivos modernos

---

## 📈 Boas Práticas Aplicadas
- Arquitetura Medallion
- Delta Lake
- Versionamento de dados
- Separação clara de responsabilidades
- Preparação para ingestão incremental
- Pronto para otimizações (OPTIMIZE, Z-ORDER, Materialized Views)

---

## 🎯 Objetivo do Projeto
Este projeto foi desenvolvido com foco em:
- Portfólio de Engenharia de Dados
- Simulação de ambientes corporativos reais
- Consolidação de conceitos modernos de Lakehouse
- Preparação para pipelines produtivos em Databricks, Azure e AWS

---

## 🚀 Próximos Passos (Evoluções)
- Ingestão incremental completa
- Validações de qualidade automatizadas
- Materialized Views
- Exposição via Power BI
- Orquestração e versionamento de pipelines

