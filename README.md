# Documentacao
Projeto de Documentação de Software

## Documentação e Boas Práticas Relacionadas ao Banco de Dados

### Banco e Tabelas
Todos os nomes de bancos devem iniciar com o prefixo *`db_`*, enquanto todas as tabelas podem ter qualquer nome desde que sejam todas em lowercase. As colunas das tabelas devem ser criadas todas com nomes lowercase, seguindo sempre essa padronização para tabelas.
Tabelas de relação são escritas da seguinte maneira: `tabela1_tabela2`.  
Exemplo de Script SQL:
```sql
CREATE DATABASE db_nome;

CREATE TABLE tabela2
(
  id BIGSERIAL PRIMARY KEY,
  nome VARCHAR(50) NOT NULL
);

CREATE TABLE tabela1
(
  id BIGSERIAL PRIMARY KEY,
  nome VARCHAR(50) NOT NULL
);

CREATE TABLE tabela_tabela1
(
  id BIGSERIAL PRIMARY KEY,
  tabela1_id BIGINT NOT NULL,
  tabela2_id BIGINT NOT NULL
);
```
