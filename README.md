# ☕ Java Puro + JDBC + MySQL

Repositório com os primeiros estudos de conexão entre **Java puro** (sem frameworks como Spring) e um banco de dados relacional, dessa vez utilizando **MySQL**. O foco aqui foi entender como funciona a comunicação direta com o banco através da API **JDBC**, sem nenhuma camada de abstração como JPA/Hibernate.

---

## 🚀 Tecnologias utilizadas

- **Java**
- **JDBC** (Java Database Connectivity)
- **MySQL**
- **MySQL Connector/J** (driver JDBC para conexão com o MySQL)

---

## 🏗️ Arquitetura do projeto

O projeto segue o padrão **DAO (Data Access Object)**, isolando toda a lógica de acesso ao banco de dados em classes específicas, separadas das entidades e das regras de aplicação. A estrutura é dividida em:

| Pacote/Classe | Responsabilidade |
|---|---|
| `model.entitites` | Entidades do domínio (`Seller`, `Department`) |
| `model.dao` | Interfaces que definem os contratos de acesso a dados (`SellerDao`, `DepartmentDao`) |
| `model.dao.impl` | Implementações JDBC dessas interfaces (`SellerDaoJDBC`, `DepartmentDaoJDBC`), contendo as queries SQL de fato |
| `model.dao.DaoFactory` | Fábrica responsável por instanciar os DAOs já com a conexão configurada |
| `db` | Classes utilitárias de conexão (`DB`) e exceções customizadas (`DbException`, `DbIntegrityException`) |
| `application` | Classes de teste (`Program`, `Program2`, entre outras), usadas para validar manualmente cada operação via console |

Essa separação permite trocar o banco de dados ou a forma de conexão no futuro sem impactar o restante da aplicação — bastaria criar uma nova implementação das interfaces DAO.

---

## 🗂️ Entidades

- **Department**: representa um departamento (id, nome)
- **Seller**: representa um vendedor (id, nome, email, data de nascimento, salário base, departamento vinculado)

---

## 🧪 Testes disponíveis (classes `application`)

O repositório conta com diversas classes de teste, cada uma validando um conjunto de operações via console (usando `System.out` e `Scanner` para interações simples). Entre elas:

- **Program**: testa as operações do `SellerDao` — busca por ID, busca por departamento, listagem completa, inserção, atualização e remoção
- **Program2**: testa as operações do `DepartmentDao` — busca por ID, listagem completa, atualização e remoção
- Outras classes (ao todo, mais 6 arquivos) testam funcionalidades adicionais do JDBC puro, como o controle manual de **transações** (`commit`/`rollback`), demonstrando como reverter alterações no banco caso ocorra um erro durante a execução de múltiplos comandos SQL

> 💡 Vale destacar o exemplo de transação: nele, duas atualizações são executadas dentro de uma mesma transação (`conn.setAutoCommit(false)`), e só são efetivadas no banco (`conn.commit()`) se ambas ocorrerem sem erro — caso contrário, um `rollback()` é acionado para desfazer tudo.

---

## ⚙️ Configuração do banco de dados

Para rodar o projeto, é necessário configurar a conexão com o MySQL através do arquivo **`db.properties`**, que deve estar na raiz do projeto:

```properties
user=SEU_USUARIO_MYSQL
password=SUA_SENHA_MYSQL
dburl=jdbc:mysql://localhost:3306/nomedoBD
useSSL=false
```

| Propriedade | Descrição |
|---|---|
| `user` | Usuário de acesso ao MySQL (ex: `root`) |
| `password` | Senha correspondente ao usuário informado |
| `dburl` | URL de conexão JDBC, no formato `jdbc:mysql://localhost:[porta]/[nome_do_banco]` |
| `useSSL` | Define se a conexão deve usar SSL (`false` para ambiente local de estudo) |

> ⚠️ Esse arquivo contém dados sensíveis e normalmente não deve ser versionado em um projeto real — aqui ele é usado apenas para fins de estudo.

Além disso, o repositório inclui o arquivo **`database.sql`**, com o script SQL necessário para criar as tabelas (`seller` e `department`) e preparar o banco antes da primeira execução.

---

## ✅ Pré-requisitos para rodar o projeto

- **Java JDK** (versão 8 ou superior)
- **MySQL Server** instalado e em execução
- **MySQL Connector/J** (driver JDBC) adicionado ao classpath/dependências do projeto
- Uma IDE de sua preferência (Eclipse, IntelliJ, VS Code, etc.)

### ▶️ Como rodar

```bash
# Clone o repositório
git clone https://github.com/Manuelcamp/JavaMySQL.git

# Acesse a pasta do projeto
cd nome-do-projeto
```

1. Crie o banco de dados no MySQL e execute o script `database.sql` para gerar as tabelas
2. Crie o arquivo `db.properties` na raiz do projeto com suas credenciais (conforme estrutura acima)
3. Execute qualquer uma das classes de teste (`Program`, `Program2`, etc.) para ver as operações sendo realizadas no console

---
