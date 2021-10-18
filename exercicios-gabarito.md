# Solução dos exercícios sobre JOINs.

**Exercício 1:** Utilizando o `INNER JOIN` crie uma consulta que retorne o nome completo do paciente (`first_name`, `last_name`) e o valor de cada pagamento efetuado (`value`) por ele.

**Dica:** _Utilize as tabelas `finance` e `clients`._

## Solução
```sql
SELECT
    CONCAT(c.first_name, ' ', c.last_name) AS Nome,
    f.value AS Valor
FROM
    finance AS f
        INNER JOIN
    clients AS c ON f.client_id = c.client_id;
```
<br>
<br>

**Exercício 2:** Utilizando a query do exercício anterior, refatore-a para que ela retorne uma coluna adicional com o método de pagamento dos pagamentos efetuados.

**Dica:** _Você precisará fazer mais de um `JOIN`._

## Solução
```sql
SELECT
    CONCAT(c.first_name, ' ', c.last_name) AS Nome,
    p.name AS `Tipo de Pagamento`,
    f.value AS Valor
FROM
    finance AS f
        INNER JOIN
    payment AS p ON p.payment_id = f.payment_id
        INNER JOIN
    clients AS c ON c.client_id = f.client_id;
```
<br>
<br>

**Exercício 3:** Utilizando o `INNER JOIN` crie uma consulta que retorne o nome completo do paciente e os procedimentos que ele já realizou.

**Dica:** _Utilize as tabelas `services`, `clients` e `client_service`._

## Solução
```sql
SELECT
    CONCAT(c.first_name, ' ', c.last_name) AS Nome,
    s.name AS Serviço
FROM
    clients AS c
        INNER JOIN
    client_service AS cs ON cs.client_id = c.client_id
        INNER JOIN
    services AS s ON s.service_id = cs.service_id;
```

<br>
<br>

**Exercício 4:** Utilizando o `RIGHT JOIN` crie uma consulta que retorne o primeiro nome do paciente e o valor de cada pagamento independentemente se ele tenha ou não efetuado pagamento.

**Dica:** _Utilize a tabela `clients` como tabela da direita._

## Solução
```sql
SELECT
    CONCAT(c.first_name, ' ', c.last_name) AS Nome,
    f.value AS Valor
FROM
    finance AS f
        RIGHT JOIN
    clients AS c ON f.client_id = c.client_id;
```

<br>
<br>

**Exercício 5:** Utilizando o `LEFT JOIN` crie uma consulta que retorne o primeiro nome do paciente, endereço e o mês que ele iniciou o tratamento independentemente se ele tenha ou não iniciado.

**Dica:** _Utilize a tabela `clients` como tabela da esquerda._

## Solução
```sql
SELECT
    c.first_name AS Nome,
    c.address AS Endereço,
    MONTH(cs.initial_date) AS Mês
FROM
    clients AS c
        LEFT JOIN
    client_service AS cs ON c.client_id = cs.client_id;
```

<br>
<br>

# Solução dos exercícios Bônus

**Exercício 6:** Utilizando o `INNER JOIN` crie uma consulta que retorne o nome completo do paciente e o total a pagar agrupado por paciente.

```sql
SELECT
    CONCAT(c.first_name, ' ', c.last_name) AS `Cliente`,
    SUM(cs.quantity * s.price) AS `Total a Pagar`
FROM
    clients AS c
        INNER JOIN
    client_service AS cs ON cs.client_id = c.client_id
        INNER JOIN
    services AS s ON s.service_id = cs.service_id
GROUP BY `Cliente`;
```

<br>
<br>

**Exercício 7:** Utilizando o `INNER JOIN` crie uma consulta que retorne o nome completo e o total pago pelo paciente.

```sql
SELECT
    CONCAT(c.first_name, ' ', c.last_name) AS Nome,
    SUM(f.value) AS `Total pago`
FROM
    clients AS c
        INNER JOIN
    finance AS f ON f.client_id = c.client_id
GROUP BY Nome;
```