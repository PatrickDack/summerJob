# O que vamos aprender?

Hoje você vai aprender a criar queries que buscam informações de duas ou mais tabelas utilizando a relação entre elas. Para isso, faremos o uso do  ``INNER JOIN``, ``LEFT JOIN``, e o `RIGHT JOIN`.

<br>

# Você será capaz de:

* Criar queries muito mais dinâmicas, informativas e buscar dados de outras tabelas, dando significado às colunas compostas por `FOREING KEYS`.

* Buscar informações de duas ou mais tabelas através da relação entre elas com o ``INNER JOIN``.

* Comparar colunas entre duas tabelas priorizando as informações da tabela da esquerda com o `LEFT JOIN`.

* Comparar colunas entre duas tabelas priorizando as informações da tabela da direita com o `RIGHT JOIN`.

<br>

# Por que isso é importante?

Nem sempre você vai encontrar toda a informação que precisa em apenas uma tabela. Logo surge a necessidade de consultar mais tabelas para complementar o resultado das queries. E é aí que entra os diversos tipos de `JOIN`.

<br>

# Conteúdos

## Inner Join

O `INNER JOIN` ou apenas `JOIN`, permite que os dados de várias tabelas sejam combinados com base na relação existente entre elas. Por meio dessa cláusula, os dados de uma tabela são usados para selecionar os dados pertencentes à outra tabela.

Com a cláusula `JOIN`, podemos especificar quais colunas das tabelas serão associadas. Para isso, será preciso definir uma chave estrangeira de uma tabela e a chave relacionada em outra tabela.

### Sintaxe:
```sql
SELECT coluna1, coluna2
FROM tabela1 AS t1
INNER JOIN tabela2 AS t2
ON t2.campo-em-comum = t1.campo-em-comum;
```
Repare que podemos consultar colunas de outras tabelas no mesmo `SELECT`, desde que, esta tabela seja referenciada no `JOIN`.

### Exemplo:

Imagine que em um banco de dados tenhamos as seguintes tabelas. A tabela `Clientes` que possui a coluna `Endereco` representado por uma `FOREING KEY`, e a tabela `Enderecos` que possuí a coluna `Logradouro` com a descrição do endereço.

```sql
SELECT Nome, Endereco FROM Clientes;
```

Se utilizarmos o código acima, obteremos como resultado uma coluna com o _nome do cliente_ e outra com o _ID do endereço_, porém esse _ID_ não nos diz muita coisa, seria mais interessante se no lugar do _ID_ obtivéssemos a _descrição do logradouro_ que está na tabela `Enderecos`.

Sabendo que essas duas tabelas se relacionam atravéz da **PRIMARY KEY** `Endereco_id` na tabela `Enderecos` e da **FOREIGN KEY** `Endereco` na tabela `Clientes`, podemos fazer a seguinte query.

```sql
SELECT c.Nome, e.Endereco
FROM Clientes AS c
INNER JOIN Enderecos AS e
ON e.Endereco_id = c.Endereco;
```

No código acima foi utilizado o _Alias_ `AS` para apelidar as colunas, é normal existirem colunas com o mesmo nome em tabelas diferentes, use _Alias_ para suas colunas sempre que possível para evitar confusão e erros de ambiguidade.

<br>

## Left Join e Right Join

Dependendo de como você queira analisar os dados, o `INNER JOIN` pode não ser suficiente porque o resultado contém apenas dados que pertencem as duas ou mais tabelas da query.

Se as tabelas que vamos consultar possuírem dados assimétricos, então nós podemos utilizar o `LEFT JOIN` ou `RIGHT JOIN` para garantir que esses dados não fiquem de fora do resultado, ou seja, isto permite obter não apenas os dados relacionados de duas tabelas, mas também os dados não relacionados. Caso não existam dados relacionados entre as tabelas à esquerda e a direita do `JOIN`, os valores resultantes de todas as colunas da lista de seleção da tabela serão nulos.

A sintaxe é semelhante a do `INNER JOIN`.

Sintaxe `LEFT JOIN`:

```sql
SELECT coluna1, coluna2
FROM tabela1 AS t1
LEFT JOIN tabela2 AS t2
ON t2.campo-em-comum = t1.campo-em-comum;
```

Sintaxe `RIGHT JOIN`:

```sql
SELECT coluna1, coluna2
FROM tabela1 AS t1
RIGHT JOIN tabela2 AS t2
ON t2.campo-em-comum = t1.campo-em-comum;
```
<br>

# Vamos praticar!

Para os exercícios a seguir você irá utilizar o Banco de Dados `OdontoTrybe`, que é o banco do consultório odontológico da Trybe, onde temos informações dos nossos pacientes, produtos e serviços.

<br>

>Após restaurar o Banco OdontoTrybe, antes de começar os exercícios, dê uma boa olhada nele para se familiarizar com a estrutura do banco e com as suas tabelas, com certeza os exercícios ficarão muito mais simples.

<br>

```sql
DROP DATABASE IF EXISTS OdontoTrybe;
CREATE DATABASE OdontoTrybe;

USE OdontoTrybe;

CREATE TABLE clients (
    client_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    address VARCHAR(50),
    phone_number VARCHAR(11),
    comments VARCHAR(150)
);

CREATE TABLE services (
    service_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(60),
    price DECIMAL(7 , 2 )
);

CREATE TABLE payment (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(40) NOT NULL
);

CREATE TABLE finance (
    payment_id INT,
    client_id INT,
    `value` DECIMAL(7 , 2 ),
    payment_date DATETIME,
    PRIMARY KEY (payment_id , client_id),
    FOREIGN KEY (payment_id)
        REFERENCES payment (payment_id),
    FOREIGN KEY (client_id)
        REFERENCES clients (client_id)
);

CREATE TABLE client_service (
    client_id INT,
    service_id INT,
    quantity INT NOT NULL,
    finished BOOL,
    initial_date DATETIME,
    final_date DATE,
    FOREIGN KEY (client_id)
        REFERENCES clients (client_id),
    FOREIGN KEY (service_id)
        REFERENCES services (service_id),
    PRIMARY KEY (client_id , service_id)
);

INSERT INTO clients(first_name, last_name, address, phone_number, comments)
	values
        ("Peter", "Rodrigues", "Rua Um, 02", 11988763456, "Possui alergia a antibióticos."),
        ("John", "Dolmayan", "Rua Trinta e Três, 34", 33088765432, "Tem diabetes"),
        ("Stan", "Ferreira", "Rua Cento e Nove, 110", 55978652345, "Tem alergia a anestesia"),
        ("Eric", "Baltasar", "Rua Noventa e Nove, 100", 66945323456, "Toma remédio controlado"),
        ("Scott", "Santos", "Rua Quarenta, 41", 88965763454, ""),
        ("Dean", "Costa", "Av. Seiscentos, 601", 99076549876, "Baixa imunidade");

INSERT INTO payment(name)
	VALUES
        ("Dinheiro"),
        ("Pix"),
        ("Cartão de Crédito"),
        ("Cartão de Débito"),
        ("Cheque");

INSERT INTO services(name, price)
	VALUES
        ("Extração", 250),
        ("Canal", 500),
        ("Implante", 1800),
        ("Limpeza", 70),
        ("Manutenção de Aparelho", 80),
        ("Clareamento", 450),
        ("Restauração", 60),
        ("Cirurgia de Siso", 350),
        ("Protocolo", 10000);

INSERT INTO client_service(client_id, service_id, quantity, finished, initial_date)
	VALUES
        (1, 1, 1, FALSE, NOW()),
        (1, 8, 1, FALSE, NOW()),
        (1, 3, 2, FALSE, NOW()),
        (2, 6, 1, FALSE, NOW()),
        (2, 4, 3, FALSE, NOW()),
        (3, 6, 1, FALSE, NOW()),
        (3, 2, 2, FALSE, NOW()),
        (4, 9, 1, FALSE, NOW());

INSERT INTO finance(payment_id, client_id, value, payment_date)
	VALUES
        (1, 1, 2000, NOW()),
        (2, 1, 1500, NOW()),
        (1, 2, 330, NOW()),
        (3, 2, 330, NOW()),
        (5, 3, 1000, NOW()),
        (4, 3, 200, NOW()),
        (2, 4, 10000, NOW());
```

**Exercício 1:** Utilizando o `INNER JOIN` crie uma consulta que retorne o nome completo do paciente (`first_name`, `last_name`) e o valor de cada pagamento efetuado (`value`) por ele.

**Dica:** _Utilize as tabelas `finance` e `clients`._

**Exercício 2:** Utilizando a query do exercício anterior, refatore-a para que ela retorne uma coluna adicional com o método de pagamento dos pagamentos efetuados.

**Dica:** _Você precisará fazer mais de um `JOIN`._

**Exercício 3:** Utilizando o `INNER JOIN` crie uma consulta que retorne o nome completo do paciente e os procedimentos que ele já realizou.

**Dica:** _Utilize as tabelas `services`, `clients` e `client_service`._

**Exercício 4:** Utilizando o `RIGHT JOIN` crie uma consulta que retorne o primeiro nome do paciente e o valor de cada pagamento independentemente se ele tenha ou não efetuado pagamento.

**Dica:** _Utilize a tabela `clients` como tabela da direita._


**Exercício 5:** Utilizando o `LEFT JOIN` crie uma consulta que retorne o primeiro nome do paciente, endereço e o mês que ele iniciou o tratamento independentemente se ele tenha ou não iniciado.

**Dica:** _Utilize a tabela `clients` como tabela da esquerda._

# Bônus
### Parabéns por ter chegado aos exercícios bônus, se prepare, pois aqui não teremos dicas.

**Exercício 6:** Utilizando o `INNER JOIN` crie uma consulta que retorne o nome completo do paciente e o total a pagar agrupado por paciente.

**Exercício 7:** Utilizando o `INNER JOIN` crie uma consulta que retorne o nome completo e o total pago pelo paciente.

# Recursos Adicionais

* Como utilizar INNER JOIN , LEFT LEFT e RIGHT JOIN , por [Alura](https://www.alura.com.br/artigos/join-em-sql?gclid=CjwKCAjwh5qLBhALEiwAioods7PlCK-Mx3wF3MtM9qAH8Vb1g-o8ir15s4sT6vPCPNAwRwgEOKfzFBoCv4QQAvD_BwE).

* Vídeo aula sobre [INNER JOIN](https://www.youtube.com/watch?v=4nbECYDlAwc) do canal Bóson Treinamentos.

* Vídeo aula sobre [RIGHT JOIN e LEFT JOIN](https://www.youtube.com/watch?v=mGbOaA1xWwk) do canal Bóson Treinamentos.