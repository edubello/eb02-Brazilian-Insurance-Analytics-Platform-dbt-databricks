# 🏗️ ARCHITECTURE

## Visão Geral

A Brazilian Insurance Analytics Platform foi projetada para demonstrar uma arquitetura moderna de Engenharia de Dados utilizando Databricks, Delta Lake, dbt e Power BI.

O projeto utiliza dados públicos da SUSEP combinados com dados sintéticos para simular uma operação de seguros e construir uma plataforma analítica ponta a ponta.

---

# Objetivos Arquiteturais

A arquitetura foi desenhada para atender aos seguintes princípios:

- Separação de responsabilidades por camadas
- Reprodutibilidade das transformações
- Governança e rastreabilidade dos dados
- Qualidade de dados automatizada
- Escalabilidade para novos datasets
- Consumo analítico otimizado
- Integração com ferramentas de BI

---

# Arquitetura de Alto Nível

```text
                     ┌─────────────────┐
                     │     SUSEP       │
                     │ Dados Públicos  │
                     └────────┬────────┘
                              │
                              ▼

                     ┌─────────────────┐
                     │ Dados Sintéticos│
                     │ Python Generator│
                     └────────┬────────┘
                              │
                              ▼

                 ┌─────────────────────────┐
                 │    Databricks Delta     │
                 │      Bronze Layer       │
                 └───────────┬─────────────┘
                             │
                             ▼

                 ┌─────────────────────────┐
                 │           dbt           │
                 │      Silver Layer       │
                 └───────────┬─────────────┘
                             │
                             ▼

                 ┌─────────────────────────┐
                 │      Gold Layer         │
                 │     Star Schema         │
                 └───────────┬─────────────┘
                             │
                             ▼

                 ┌─────────────────────────┐
                 │       Power BI          │
                 │ Semantic Model + DAX    │
                 └─────────────────────────┘
```

---

# Data Sources

## SUSEP

Dados oficiais do mercado segurador brasileiro.

### Principais conjuntos utilizados

- Prêmios Emitidos
- Sinistros
- Provisões Técnicas
- Cadastro de Seguradoras

### Características

| Item | Valor |
|--------|--------|
| Origem | SUSEP |
| Frequência | Mensal |
| Tipo | Dados Públicos |
| Formato | CSV |

---

## Dados Sintéticos

Dados gerados para simular operações de seguros sem exposição de informações sensíveis.

### Entidades

- Clientes
- Apólices
- Sinistros
- Corretores

### Objetivos

- Simular ambiente corporativo
- Introduzir problemas de qualidade
- Permitir análises em nível granular

---

# Arquitetura Medallion

A plataforma adota o padrão Medallion.

```text
Bronze
   ↓
Silver
   ↓
Gold
```

---

# Bronze Layer

## Objetivo

Persistir os dados exatamente como recebidos das fontes de origem.

## Responsabilidades

- Ingestão
- Armazenamento
- Histórico
- Auditoria

## Tabelas

```text
bronze_susep_premios
bronze_susep_sinistros

bronze_clientes
bronze_apolices
bronze_sinistros
```

## Transformações Permitidas

Apenas:

- Conversão de formato
- Inclusão de metadados
- Controle de carga

---

# Silver Layer

## Objetivo

Padronizar, limpar e validar os dados.

## Tecnologia

dbt

## Models

```text
stg_clientes
stg_apolices
stg_sinistros

stg_susep_premios
stg_susep_sinistros
```

## Responsabilidades

- Limpeza
- Validação
- Padronização
- Regras de negócio

---

# Estratégia de Qualidade

## Testes dbt

### Generic Tests

```yaml
unique
not_null
relationships
accepted_values
```

### Custom Tests

```text
Cliente deve possuir CPF válido
Idade maior ou igual a 18 anos
Prêmio maior que zero
Sinistro não pode ter data futura
```

---

# Gold Layer

## Objetivo

Disponibilizar modelos analíticos para consumo.

---

# Modelo Dimensional

## Dimensões

### dim_cliente

```text
id_cliente
sexo
idade
faixa_etaria
estado
score_risco
```

### dim_produto

```text
id_produto
produto
categoria
```

### dim_tempo

```text
id_data
dia
mes
ano
trimestre
```

### dim_seguradora

```text
id_seguradora
nome
grupo
```

---

## Fatos

### fact_apolice

```text
premio_emitido
valor_cobertura
```

### fact_sinistro

```text
valor_sinistro
quantidade_sinistros
```

### fact_mercado

```text
premio_mercado
sinistro_mercado
market_share
```

---

# Fluxo de Transformação

```text
Raw Files

    ↓

Bronze

    ↓

dbt Staging

    ↓

dbt Intermediate

    ↓

dbt Marts

    ↓

Power BI
```

---

# Governança

## Versionamento

Git + GitHub

## Branch Strategy

```text
main
develop
feature/*
```

## Documentação

- README.md
- ROADMAP.md
- ARCHITECTURE.md
- DATA_DICTIONARY.md

---

# CI/CD

## GitHub Actions

Pipeline executado a cada Push ou Pull Request.

### Etapas

```text
Checkout

    ↓

Instalação Dependências

    ↓

dbt build

    ↓

dbt test

    ↓

Validação
```

---

# Consumo Analítico

## Power BI

Camada de visualização da solução.

### Indicadores

#### Mercado

- Market Share
- Crescimento YoY
- Ranking de Seguradoras

#### Operação

- Sinistralidade
- Frequência de Sinistros

#### Clientes

- Perfil de Risco
- Índice de Fraude

---

# Escalabilidade

A arquitetura foi projetada para permitir expansão futura através de:

- Novas fontes de dados
- Novos produtos de seguros
- Novos indicadores
- Observabilidade
- Data Contracts
- Data Catalog

---

# Próximas Evoluções

## Engenharia

- dbt Snapshots
- Macros reutilizáveis
- Observabilidade

## Analytics

- Customer Lifetime Value
- Churn de Clientes
- Score de Fraude

## Governança

- Catálogo de Dados
- Data Lineage Avançado
- Data Contracts
