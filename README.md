# eb_02-Brazilian-Insurance-Analytics-Platform-dbt-databricks
 Data Engineering project built using public data from SUSEP and synthetic data to simulate the actual operations of Brazilian insurance companies. - Projeto de Engenharia de Dados construído com dados públicos da SUSEP e dados sintéticos para simular operações reais de seguradoras brasileiras.
# 🛡️ Brazilian Insurance Analytics Platform

Projeto de Engenharia de Dados construído com dados públicos da SUSEP e dados sintéticos para simular operações reais de seguradoras brasileiras.

O objetivo é demonstrar a construção de uma plataforma analítica moderna utilizando arquitetura Medallion (Bronze, Silver e Gold), Delta Lake, dbt, testes automatizados, CI/CD e modelagem dimensional voltada para indicadores estratégicos do mercado segurador.
<br> 
Desenvolver experiência prática no uso de dbt para transformar dados e conectar as áreas de engenharia e análise.



---

## 🎯 Problema de Negócio

Seguradoras precisam monitorar continuamente:

- Prêmios emitidos
- Sinistros pagos
- Sinistralidade
- Participação de mercado
- Crescimento por ramo
- Perfil de risco dos clientes

Os dados públicos da SUSEP fornecem uma visão agregada do mercado, enquanto dados sintéticos permitem simular operações detalhadas de clientes, apólices e sinistros sem expor informações confidenciais.

---

## 🏗️ Arquitetura

```text
                Dados SUSEP
                      +
            Dados Sintéticos

                      │
                      ▼

             Bronze (Delta Lake)

                      │
                      ▼

           Silver (dbt Staging)

                      │
                      ▼

      Gold (Star Schema / Marts)

                      │
                      ▼

              Power BI Model

                      │
                      ▼

          Dashboards Executivos
```

---

## ⚙️ Stack Tecnológica

| Categoria | Tecnologia |
|------------|------------|
| Cloud Platform | Databricks |
| Storage | Delta Lake |
| Processamento | PySpark |
| Transformações | dbt |
| Versionamento | Git + GitHub |
| CI/CD | GitHub Actions |
| Modelagem | Star Schema |
| Analytics | Power BI |
| Linguagens | Python, SQL |
| Qualidade de Dados | dbt Tests |

---

## 📂 Estrutura do Projeto

```text
insurance-analytics-platform

├── data/
│
├── notebooks/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── dbt/
│   ├── models/
│   ├── tests/
│   ├── macros/
│   └── snapshots/
│
├── docs/
│
├── powerbi/
│
├── .github/
│   └── workflows/
│
└── README.md
```

---

## 🥉 Bronze Layer

Objetivo:

Ingerir dados sem transformações significativas.

### Fontes

#### SUSEP

- Prêmios Emitidos
- Sinistros
- Provisões Técnicas
- Seguradoras

#### Dados Sintéticos

- Clientes
- Apólices
- Sinistros
- Corretores

### Tabelas

```text
bronze_susep_premios
bronze_susep_sinistros

bronze_clientes
bronze_apolices
bronze_sinistros
```

---

## 🥈 Silver Layer

Objetivo:

Padronizar, limpar e validar dados.

### Regras Aplicadas

- Remoção de duplicidades
- Tratamento de valores nulos
- Padronização de datas
- Validação de relacionamentos
- Regras de negócio

### Modelos dbt

```text
stg_clientes
stg_apolices
stg_sinistros

stg_susep_premios
stg_susep_sinistros
```

---

## 🥇 Gold Layer

Objetivo:

Disponibilizar dados para análise.

### Dimensões

```text
dim_cliente
dim_produto
dim_tempo
dim_seguradora
```

### Fatos

```text
fact_apolice
fact_sinistro
fact_mercado
```

---

## 📊 KPIs Implementados

### Mercado

- Market Share
- Crescimento YoY
- Ranking de Seguradoras

### Performance Operacional

- Sinistralidade
- Frequência de Sinistros
- Ticket Médio de Sinistros

### Clientes

- Perfil de Risco
- Score de Fraude
- Lifetime Value

---

## ✅ Qualidade de Dados

Testes automatizados implementados com dbt.

### Generic Tests

- unique
- not_null
- accepted_values
- relationships

### Custom Tests

- prêmio > 0
- idade >= 18
- data_sinistro <= data_atual
- apólice vinculada a cliente válido

---

## 🔄 CI/CD

O pipeline é validado automaticamente através do GitHub Actions.

### Execuções

```bash
dbt build
dbt test
```

### Validações

- SQL
- Qualidade
- Dependências
- Integridade de Modelos

---

## 📈 Dashboard

### Visão Executiva

- Evolução dos Prêmios
- Evolução dos Sinistros
- Sinistralidade
- Market Share

### Visão Operacional

- Performance por produto
- Performance por seguradora

### Visão Clientes

- Distribuição de risco
- Segmentação de carteira
- Indicadores de fraude

---

## 🎓 Objetivos de Aprendizado

Este projeto foi desenvolvido para aprofundar conhecimentos em:

- Databricks
- Delta Lake
- dbt
- Data Quality
- Modelagem Dimensional
- Analytics Engineering
- Engenharia de Dados Moderna

---

## 🚀 Como Executar

### Clonar repositório

```bash
git clone https://github.com/seuusuario/insurance-analytics-platform.git
```

### Instalar dependências

```bash
pip install -r requirements.txt
```

### Executar dbt

```bash
dbt build
```

### Executar testes

```bash
dbt test
```

---

## 📚 Fonte dos Dados

### Dados Públicos

Superintendência de Seguros Privados (SUSEP)

https://www2.susep.gov.br/menuestatistica/ses/

### Dados Sintéticos

Gerados exclusivamente para fins educacionais e demonstração técnica.

---

## 👨‍💻 Autor

Seu Nome

LinkedIn: https://linkedin.com/in/eduardo-bello

GitHub: https://github.com/edu_bello

---

⭐ Caso tenha gostado do projeto, considere deixar uma estrela no repositório.
