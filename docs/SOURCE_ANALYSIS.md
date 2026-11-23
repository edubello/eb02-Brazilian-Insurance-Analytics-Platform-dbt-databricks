# Descobertas

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


Finding #1

Foi identificado comportamento inconsistente entre os códigos de entidade presentes nas tabelas de fatos e dimensões.

A validação mostrou diferenças de formatação dos identificadores, exigindo padronização na camada Silver antes da aplicação de joins.


Finding #2

O campo Coenti em ses_cias possui preenchimento com espaços em branco à direita.

A validação de integridade referencial entre ses_cias e ses_seguros exigiu a aplicação de TRIM antes dos joins.

A padronização será implementada na camada Silver.

**Problema identificado ✅
Problema explicado ✅
Regra de transformação descoberta ✅
**
