# Documentacao
Projeto de Documentação de Software

## Documentação & Boas Práticas Relacionadas ao Banco de Dados

### Banco & Tabelas
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
  
O script SQL deve ser posto em um arquivo com o nome `SQL_db_nomedobanco.sql` a fim de facilitar a criação do banco do projeto.  
  
### Instrução SELECT
Em instruções select, evite usar “*”. Seja restritivo, traga somente os campos realmente necessários, isso alivia a memória do servidor, diminue tráfego na rede, etc. Algumas pessoas defendem que tambem não devem ser criados determinados campos, por exemplo, você tem A + B e pretende guardar C onde C = A + B. Ao invéz de criar uma coluna para armazenar C, passe a utilizar `SELECT (A+B) AS C FROM tabela`. Esse pensamento pode não ser necessariamente válido para Dws. Vamos supor que você tem uma tabela enorme com dados sobre salário por ano. Você pode armazenar o percentual de ajuste e o valor ajustado, fazendo ai uma “desnormalização” para que o comando que vai recuperar os valores do salário não “frite” a CPU forçando-a a fazer muitas contas e perdendo muito desempenho.  
  
Exemplo: Queremos recuperar o salário da tabela de funcionários.
```sql
-- Forma errada
SELECT * FROM funcionarios;
-- Forma correta
SELECT salario FROM funcionarios;
```

### Imagens
Não armazene imagens no banco, sempre armazene as URLs.
```sql

CREATE TABLE imagem
(
  id BIGSERIAL PRIMARY KEY,
  caminho VARCHAR(100),
  nome VARCHAR(50)
);

```

### Chaves Primárias
Utilize somente chaves primárias numéricas e de preferência do tipo BIGSERIAL (BIGINT com AUTO INCREMENT NOT NULL UNIQUE).
```sql

id BIGSERIAL PRIMARY KEY
-- Ou
id BIGINT NOT NULL UNIQUE PRIMARY KEY

```

### Stored Procedures & Functions
Utilizando-se stored procedures e functions ao invéz de escrever código no seu programa, vai garantir maior desempenho e segurança para seu sistema como um todo.

### Commits, Rollbacks & Transações
Utilize o conceito de transações. Vários problemas podem ocorer, por exemplo, a rede cair. Aprenda sobre commit e rollback.

### Tipos de dados
Use sempre o tipo de dados correto para armazenar os dados. Por exemplo, não armazene sexo, que vai ser M ou F em um campo Varchar, use apenas 1 caractere: CHAR(1).

### Cursores
Evite o uso de cursores, eles consomem muito tempo já que “navegam” registro por registro.

### Cláusula `WHERE`
Otimize a clausula WHERE: Simples exemplos são o uso de `>` e `>=`. Se você quer retornar todas as pessoas de uma tabela que tem idade `> 3`, use no where `>=4`, dessa forma o banco não fará o scan das páginas até encontrar o 3. Esse princípio é válido desde que você tenha um índice na idade.

### Intruções SQL idênticas
Quando possivel, crie instruções SQL idênticas, pois no momento da execução de uma instrução, o banco compila a mesma e a preserva em memória, na próxima execução, não vai precisar compilar novamente. Uma ótima técnica para fazer isso é utilizar variáveis nas suas instruções ao invés de passar parametros para o banco.

### Utilização de PK, FK & mecanismos para persistência
Utilize os mecanismos do banco de dados para persistência: Primary Key, Foreign Key, etc são feitos e otimizados para isso.

### Lock
Quando possivel, trave (lock) uma tabela para executar alguma operação que vai demandar muito acesso a esta tabela, por exemplo, se você vai alterar a estrutura de uma tabela grande ou importar dados neste tabela (falando-se de tabelas realmente grandes).

### Instruções SQL e o uso do `*`
Sempre utilize o nome das colunas em instruções SELECT, INSERT, UPDATE evitando utilizar `*`.

### Operador `LIKE`
Evite utilizar o operador `LIKE`, ele pode facilmente fazer o desempenho de um banco de dados ruir.

### `EXIST` & `COUNT`
Utilize `EXISTS` ao invéz de `COUNT` para verificar se existe um determinado registro em uma tabela. É comum ver desenvolvedores fazendo um `select count(X) from Y` para verificar se o COUNT é maior que 0. Utilizando-se EXISTS, o SGDB vai parar no primeiro registro encontrado, se utilizar count, o banco vai varrer toda a tabela.

### `JOINS`
Em joins de tipos de dados diferentes, o SGBD vai ter que converter o tipo hierarquicamente inferior para o outro tipo a fim de efetuar a comparação, e assim, não vai utilizar um índice caso exista.

### Índices
Sobre índices:  
  
* Não crie indices em campos que são alterados constantemente, pois o banco vai ter que atualizar toda sua estrutura de índices em qualquer update feito no campo.
* Prefira criar os indices em chaves primárias e estrangeiras, e em suas queries, utilize estes índices.
Não tenha muitos índices em seu banco, só o necessário: uma breve explicação sobre o motivo disso, é que o banco de dados mantem toda uma estrutura para gerenciar os índices, então, quanto mais índices, mais tempo/processamento o SGBD vai utilizar para a manutenção dos mesmos.
* No momento de importação/importação de uma base de dados, não exporte/importe índices. Isso vai consumir mais tempo/processamento. Tambem pode-se não fazer backup de índices.
* Não crie índices em colunas que possuem pouca variação de valores.  

### Documentação do Banco
A documentação do banco é realizada através de diagramas de banco ou modelos relacionais. Utilizando um software tal como o Astah Comunnity para a criação do mesmo.
O banco deve ser feito em um Script e salvo com o nome de `SCRIPT_V1_db_nomedobanco.sql`, alterando o V1 para a versão atual do banco, mantendo assim o controle sobre versões e a possibilidade de gerar o banco a partir de um SCRIPT SQL.
