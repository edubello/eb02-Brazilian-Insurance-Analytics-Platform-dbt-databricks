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
