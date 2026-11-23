<img width="4032" height="1351" alt="image" src="https://github.com/user-attachments/assets/3a49a77c-45a2-4d3a-9263-9ffe23cc3e76" /># Descobertas

## ses_cias

Registros: 770

Granularidade:
1 linha por seguradora.

Chave candidata:
- coenti

---

## ses_ramos

Registros: 161

Granularidade:
1 linha por ramo.

Chave candidata:
- coramo

---

## ses_seguros

Registros:
1.818.607

Granularidade observada:

1 linha =
1 mês +
1 seguradora +
1 ramo

Chave candidata:

- damesano
- coenti
- coramo

Teste de duplicidade:
0 registros duplicados

## Finding 001

### Campo

Coenti

### Tabelas

- ses_cias
- ses_seguros

### Problema identificado

O campo Coenti em ses_cias apresenta preenchimento com espaços em branco à direita.

### Evidência

Join direto entre as tabelas resultou em 281 códigos órfãos.

Após aplicação de trim() em ambas as fontes:

- 0 códigos órfãos

### Regra de tratamento

Aplicar trim() no campo Coenti durante a carga da camada Silver.

Finding #002

Coramo também possui espaços em branco à direita.

Finding #003

Após aplicação de trim() nos códigos de ramo, foram identificados registros presentes na tabela de fatos e ausentes na dimensão de ramos.

Exemplos:

- 12
- 34
- 72
- 82
- 85

Hipótese:

A tabela de fatos contém códigos históricos não representados na tabela atual de domínio.


##Fiding 004

A tabela de fatos contém códigos de ramo que não existem na tabela atual de domínio.

Exemplo:
- 12

Observação:

O código aparece em registros históricos (datados de 2000), indicando possível alteração da codificação de ramos ao longo do tempo pelo órgão regulador.

Status:
Em investigação.

**
Durante a exploração dos dados da SUSEP identifiquei divergências entre os códigos históricos de ramo presentes na tabela de fatos e a tabela de domínio atual. A análise temporal mostrou que parte dos códigos utilizados em 2000 não existe mais no catálogo vigente, exigindo uma estratégia específica para preservação da integridade histórica dos dados.**

