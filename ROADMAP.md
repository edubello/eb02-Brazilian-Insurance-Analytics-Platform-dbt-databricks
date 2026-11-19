# 🚀 ROADMAP - Brazilian Insurance Analytics Platform

# ROADMAP

Este documento descreve o plano de desenvolvimento da Brazilian Insurance Analytics Platform.
O objetivo é consolidar as etapas de construção da solução, desde a ingestão dos dados públicos da SUSEP até a disponibilização de dashboards analíticos, utilizando Databricks, Delta Lake, dbt, Power BI e práticas modernas de Engenharia de Dados.
O roadmap está dividido em entregas que permitem acompanhar a evolução técnica do projeto, mantendo foco em qualidade de dados, modelagem dimensional, automação e governança.

## Visão Geral

Este projeto tem como objetivo construir uma plataforma analítica para o mercado segurador brasileiro utilizando dados públicos da SUSEP combinados com dados sintéticos de clientes, apólices e sinistros.

Além de explorar indicadores relevantes do setor de seguros, o projeto busca demonstrar competências práticas em Engenharia de Dados Moderna utilizando Databricks, Delta Lake, dbt, CI/CD e Power BI.

---

# Objetivos do Projeto

## Negócio

- Analisar o mercado segurador brasileiro
- Calcular indicadores de desempenho das seguradoras
- Simular uma operação real de seguros
- Construir KPIs executivos e operacionais

## Técnicos

- Implementar arquitetura Medallion
- Aplicar modelagem dimensional
- Utilizar Delta Lake
- Implementar transformações com dbt
- Criar testes automatizados
- Implementar CI/CD
- Desenvolver dashboards analíticos

---

# Arquitetura Alvo

```text
                 Dados SUSEP
                        +
              Dados Sintéticos

                        │
                        ▼

            Bronze Layer (Delta)

                        │
                        ▼

           Silver Layer (dbt)

                        │
                        ▼

        Gold Layer (Star Schema)

                        │
                        ▼

            Power BI Semantic Model

                        │
                        ▼

             Dashboards e KPIs
```

---

# Stack Tecnológica

| Categoria | Tecnologia |
|------------|------------|
| Plataforma | Databricks |
| Armazenamento | Delta Lake |
| Transformação | dbt |
| Processamento | PySpark |
| Linguagem | SQL |
| Versionamento | Git |
| Repositório | GitHub |
| CI/CD | GitHub Actions |
| Analytics | Power BI |
| Modelagem | Star Schema |

---

# Etapas de Implementação

## 1 - Fundação e Ingestão

### Objetivo

Preparar o ambiente e realizar a primeira carga de dados.

### Atividades

- [ ] Criar repositório GitHub
- [ ] Configurar ambiente Databricks
- [ ] Criar schemas Bronze, Silver e Gold
- [ ] Selecionar datasets da SUSEP
- [ ] Criar gerador de dados sintéticos
- [ ] Ingerir dados para Delta Lake

### Entrega

✅ Camada Bronze criada

---

## 2 — Qualidade de Dados

### Objetivo

Implementar controles básicos de qualidade.

### Atividades

- [ ] Criar relatório de profiling
- [ ] Simular problemas de qualidade
- [ ] Identificar duplicidades
- [ ] Validar completude
- [ ] Validar consistência

### Problemas Simulados

- CPF duplicado
- Idade inválida
- UF inválida
- Cliente inexistente
- Datas inconsistentes

### Entrega

✅ Data Quality Report

---

## 3 — Camada Silver com dbt

### Objetivo

Padronizar e transformar os dados.

### Atividades

- [ ] Instalar dbt-core
- [ ] Configurar dbt-databricks
- [ ] Criar modelos staging
- [ ] Implementar testes genéricos
- [ ] Gerar documentação dbt

### Models

- stg_clientes
- stg_apolices
- stg_sinistros
- stg_susep_premios
- stg_susep_sinistros

### Entrega

✅ Pipeline dbt funcional

✅ Data Lineage disponível

---

## 4 — Camada Gold e Modelagem Dimensional

### Objetivo

Construir a camada analítica.

### Dimensões

- [ ] dim_cliente
- [ ] dim_produto
- [ ] dim_tempo
- [ ] dim_seguradora

### Fatos

- [ ] fact_apolice
- [ ] fact_sinistro
- [ ] fact_mercado

### Atividades

- [ ] Criar Star Schema
- [ ] Documentar modelagem
- [ ] Criar diagrama dimensional

### Entrega

✅ Modelo dimensional completo

---

## 5 — Analytics e Power BI

### Objetivo

Transformar dados em informação de negócio.

### KPIs

#### Mercado

- [ ] Market Share
- [ ] Crescimento YoY
- [ ] Ranking de Seguradoras

#### Operação

- [ ] Sinistralidade
- [ ] Frequência de Sinistros
- [ ] Ticket Médio

#### Clientes

- [ ] Perfil de Risco
- [ ] Índice de Fraude
- [ ] Segmentação

### Power BI

- [ ] Modelo semântico
- [ ] Medidas DAX
- [ ] Dashboard executivo

### Entrega

✅ Dashboard finalizado

---

## 6 — CI/CD e Publicação

### Objetivo

Finalizar o projeto e prepará-lo para portfólio.

### GitHub Actions

- [ ] dbt build
- [ ] dbt test
- [ ] SQL Lint

### Documentação

- [ ] README.md
- [ ] ARCHITECTURE.md
- [ ] DATA_DICTIONARY.md

### Divulgação

- [ ] Publicar projeto
- [ ] Criar post técnico para LinkedIn
- [ ] Atualizar currículo

### Entrega

✅ Projeto publicado

✅ CI/CD funcional

✅ Documentação completa

---

# Backlog Futuro

## Observabilidade

- [ ] Pipeline Health Dashboard
- [ ] Tempo de execução
- [ ] Monitoramento de cargas

## Engenharia de Dados

- [ ] Snapshots dbt
- [ ] Macros dbt
- [ ] Data Contracts

## Analytics Avançado

- [ ] Customer Lifetime Value
- [ ] Churn
- [ ] Modelagem de Risco

---

# Resultado Esperado

Ao final do projeto será possível demonstrar conhecimento prático em:

- Databricks
- Delta Lake
- PySpark
- SQL
- dbt
- Data Quality
- Modelagem Dimensional
- GitHub Actions
- Analytics Engineering
- Power BI
- Mercado Segurador Brasileiro

---

# Sucesso do Projeto

O projeto será considerado concluído quando:

- [ ] Dados SUSEP estiverem integrados
- [ ] Arquitetura Medallion estiver implementada
- [ ] Star Schema estiver documentado
- [ ] Testes dbt estiverem automatizados
- [ ] Pipeline CI/CD estiver operacional
- [ ] Dashboard executivo estiver publicado
- [ ] Repositório estiver pronto para apresentação em entrevistas
