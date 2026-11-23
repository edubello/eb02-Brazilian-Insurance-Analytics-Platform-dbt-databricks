# Brazilian Insurance Analytics Platform

Discovery & Data Profiling Report

Data Source:
SUSEP

Stack:
Databricks | Delta Lake | dbt | Power BI

Author:
Eduardo Bello

---

# Visão Geral

Este documento registra a fase de descoberta e análise exploratória dos dados que servirão de base para a construção da Brazilian Insurance Analytics Platform.

O projeto tem como objetivo desenvolver uma plataforma analítica moderna utilizando dados públicos da SUSEP, Databricks, Delta Lake, dbt, modelagem dimensional, testes automatizados e práticas de Analytics Engineering.

A proposta do projeto não é apenas demonstrar habilidades técnicas em ferramentas específicas, mas reproduzir a forma como equipes de dados atuam em ambientes corporativos reais: compreendendo o problema, explorando as fontes de dados, validando hipóteses, identificando problemas de qualidade e construindo soluções fundamentadas em evidências.

---

# Objetivo da Fase de Descoberta

Antes de iniciar qualquer modelagem ou transformação, foi definida uma etapa específica para responder as seguintes perguntas:

- O que representa cada arquivo?
- Como os datasets se relacionam?
- Qual é a granularidade dos dados?
- Quais campos representam dimensões?
- Quais campos representam métricas?
- Existem problemas de qualidade de dados?
- Existem anomalias históricas?
- Qual arquitetura analítica melhor representa esses dados?

A intenção foi evitar um erro comum em projetos de dados: construir modelos analíticos antes de compreender a estrutura real das fontes.

---

# Contexto do Projeto

O projeto utiliza dados públicos da SUSEP (Superintendência de Seguros Privados), órgão responsável pela supervisão do mercado segurador brasileiro.

A ideia surgiu a partir da combinação de três fatores:

1. Interesse em aprofundar conhecimentos em Databricks e Analytics Engineering.
2. Construção de um projeto relevante para portfólio.
3. Experiência profissional prévia no mercado segurador.

---

# Arquitetura Planejada

Arquitetura alvo:

```text
Fonte SUSEP

      ↓

Landing

      ↓

Bronze (Delta)

      ↓

Silver (dbt)

      ↓

Gold (Star Schema)

      ↓

Power BI

      ↓

Camada Analítica
```

Contudo, antes da implementação da arquitetura foi criada uma fase de descoberta para compreender a estrutura real dos dados.

---

# Fontes de Dados

Foram identificados três arquivos principais.

---

## ses_cias.csv

Contém informações cadastrais das seguradoras.

Campos identificados:

```text
Coenti
Noenti
Cogrupo
Nogrupo
```

Hipótese inicial:

```text
Dimensão de Seguradoras
```

---

## ses_ramos.csv

Contém os ramos de seguros supervisionados pela SUSEP.

Campos identificados:

```text
coramo
noramo
```

Hipótese inicial:

```text
Dimensão de Ramos
```

---

## ses_seguros.csv

Tabela principal contendo métricas financeiras e operacionais.

Campos identificados:

```text
damesano
coenti
cogrupo
coramo

premio_direto
premio_de_seguros
premio_retido
premio_ganho

sinistro_direto
sinistro_retido
sinistro_ocorrido

desp_com
receita_resseguro

e outras métricas
```

Hipótese inicial:

```text
Tabela Fato
```

---

# Análise Estrutural

## Quantidade de Registros

### Seguradoras

```text
770 registros
```

### Ramos

```text
161 registros
```

### Seguros

```text
1.818.607 registros
```

A diferença de volume entre as tabelas sugeriu desde o início uma estrutura dimensional clássica.

---

# Descoberta 1
## Identificação da Granularidade da Fato

Uma das primeiras análises realizadas teve como objetivo identificar o que cada linha da tabela principal representa.

Foi testada a unicidade da combinação:

```text
damesano
coenti
coramo
```

Resultado:

```text
0 registros duplicados
```

Conclusão:

Cada linha representa:

```text
1 Mês
+
1 Seguradora
+
1 Ramo
```

Essa descoberta permitiu definir a granularidade da futura tabela fato.

---

# Descoberta 2
## Identificação das Dimensões

Foi realizada uma análise dos relacionamentos potenciais.

Resultado:

```text
ses_cias
   |
   |
   +---- coenti ----+
                    |
                    |
              ses_seguros
                    |
                    |
   +---- coramo ----+
   |
   |
ses_ramos
```

Isso confirmou a hipótese de uma estrutura dimensional composta por:

```text
Dimensão Seguradora

Dimensão Ramo

Dimensão Tempo

Tabela Fato de Seguros
```

---

# Descoberta 3
## Problema de Integridade Referencial entre Seguradoras e Fato

Foi realizada a validação referencial entre:

```text
ses_seguros.coenti
```

e

```text
ses_cias.Coenti
```

Resultado inicial:

```text
281 códigos sem correspondência
```

A princípio a hipótese era de divergência de chave ou problemas na fonte.

---

# Descoberta 4
## Investigação de Comprimento dos Códigos

Ao inspecionar o comprimento dos campos:

### Dimensão

```text
Coenti
```

Comprimento:

```text
10 caracteres
```

### Fato

```text
coenti
```

Comprimento:

```text
5 caracteres
```

Visualmente os códigos eram idênticos.

Porém os comprimentos divergiam.

---

# Descoberta 5
## Identificação de Espaços em Branco

Foi realizada inspeção detalhada dos códigos.

A investigação demonstrou que a dimensão de seguradoras armazenava códigos preenchidos com espaços à direita.

Exemplo conceitual:

```text
"01007     "
```

enquanto a fato possuía:

```text
"01007"
```

---

# Descoberta 6
## Validação da Hipótese

Foi aplicada a função:

```sql
TRIM()
```

nos campos utilizados no relacionamento.

Resultado:

```text
281 seguradoras órfãs
↓
0 seguradoras órfãs
```

Conclusão:

A integridade referencial era válida.

O problema estava na qualidade da formatação da chave.

---

# Regra de Negócio Identificada

Toda utilização de:

```text
Coenti
```

deverá aplicar:

```sql
TRIM(Coenti)
```

na futura camada Silver.

---

# Descoberta 7
## Problema Similar na Dimensão de Ramos

O mesmo processo foi aplicado ao campo:

```text
coramo
```

Resultado:

Dimensão:

```text
161 códigos
```

Todos com comprimento:

```text
10 caracteres
```

Fato:

```text
216 códigos distintos
```

Com comprimento:

```text
4 caracteres
```

Além de um código nulo identificado.

---

# Descoberta 8
## Aplicação de TRIM em Ramos

A aplicação de:

```sql
TRIM(coramo)
```

reduziu significativamente os problemas de relacionamento.

Contudo, alguns códigos continuaram sem correspondência.

Exemplos:

```text
12
34
72
82
85
```

---

# Descoberta 9
## Possível Evolução Histórica dos Ramos

Ao investigar o código:

```text
12
```

foram encontrados registros válidos na tabela de fatos.

Exemplo:

```text
damesano = 200008
```

envolvendo diversas seguradoras.

Isso demonstra que:

- o código existia operacionalmente;
- não se trata de erro de leitura;
- não se trata de um registro isolado.

---

# Hipótese Atual

A hipótese mais forte neste momento é:

```text
A tabela fato contém códigos históricos
que não estão presentes na tabela
moderna de domínio de ramos.
```

Possíveis explicações:

- mudança regulatória;
- recodificação dos ramos;
- descontinuação de categorias;
- atualização do catálogo de domínio.

Essa hipótese ainda está em investigação.

---

# Resultado da Fase Atual

Até o momento foi possível:

✅ Compreender a estrutura das fontes.

✅ Identificar dimensões e fatos.

✅ Definir a granularidade dos dados.

✅ Validar integridade referencial.

✅ Identificar problemas de qualidade.

✅ Descobrir regras de padronização.

✅ Levantar hipóteses históricas sobre o domínio de seguros.

✅ Produzir documentação técnica baseada em evidências.

---

# Próximas Etapas

## Source Analysis

- Concluir análise dos códigos históricos de ramo.
- Formalizar catálogo de regras identificadas.

## Bronze

- Persistir os dados em tabelas Delta.
- Preservar o dado original sem transformações.

## Silver

- Aplicar TRIM nas colunas identificadas.
- Padronizar formatos.
- Implementar regras de qualidade.

## Gold

- Construir modelo dimensional.
- Criar dimensões conformadas.
- Implementar tabela fato de seguros.

## Analytics

- Construir camada semântica.
- Desenvolver indicadores de negócio.
- Disponibilizar dashboards Power BI.

---

# Lições Aprendidas

A principal conclusão desta fase foi a importância de investir tempo na compreensão das fontes antes de iniciar a modelagem.

Diversos problemas de qualidade e características históricas dos dados só foram descobertos porque a análise começou pela exploração dos dados e não pela implementação das transformações.

Essa abordagem permitiu construir hipóteses fundamentadas, documentar comportamentos da fonte e reduzir significativamente os riscos de modelagem incorreta nas etapas seguintes.
