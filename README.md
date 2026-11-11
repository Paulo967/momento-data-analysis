**1.1 Você acabou de ser contratado pela Momento! Vamos começar incluindo você no sistema.**
```sql
INSERT INTO funcionarios (nome, departamento_id, cargo_id, salario, data_contratacao)
VALUES ('Paulo Petrit', 1, 1, 5000.00, '2026-11-05'); 
```

**1.2 Agora que você faz parte da equipe, quantos funcionários temos ao total na empresa?**
```sql
SELECT COUNT(*) 
FROM funcionarios;
```

**1.3 Quantos funcionários trabalham especificamente no Departamento de Tecnologia?**
```sql
SELECT COUNT(*)
FROM funcionarios
INNER JOIN departamentos ON departamentos.departamento_id = funcionarios.departamento_id
WHERE departamentos.departamento_nome = 'Tecnologia';
```

**1.4 Liste todos os departamentos que existem na empresa. Quantos são?**
```sql
SELECT * FROM departamentos;
SELECT COUNT(*) FROM departamentos;
```

**1.5 Quantos escritórios a Momento possui? Em quais países?**
```sql
SELECT COUNT(*) FROM escritorios;
SELECT pais, COUNT(*) FROM escritorios GROUP BY pais;
```

**2.1 Quantos funcionários trabalham no Departamento de Vendas?**
```sql
SELECT COUNT(*)
FROM funcionarios
INNER JOIN departamentos ON departamentos.departamento_id = funcionarios.departamento_id
WHERE departamentos.departamento_nome = 'Vendas';
```

**2.2 Qual é o custo total com salários do Departamento de Vendas?**
```sql
SELECT SUM(salario)
FROM funcionarios
INNER JOIN departamentos ON departamentos.departamento_id = funcionarios.departamento_id
WHERE departamentos.departamento_nome = 'Vendas';
```

**2.3 Qual é a média salarial da empresa, excluindo os cargos de CEO, CMO e CFO?**
```sql
SELECT AVG(salario)
FROM funcionarios
INNER JOIN cargos ON cargos.cargo_id = funcionarios.cargo_id
WHERE cargos.cargo_nome NOT IN ('CEO', 'CMO', 'CFO');
```

**2.4 Qual é a média salarial do Departamento de Tecnologia?**
```sql
SELECT AVG(salario)
FROM funcionarios
INNER JOIN departamentos ON departamentos.departamento_id = funcionarios.departamento_id
WHERE departamentos.departamento_nome = 'Tecnologia';
```
**2.5 Qual departamento possui a maior média salarial?**
```sql
SELECT departamentos.departamento_nome, AVG(salario)
FROM funcionarios
INNER JOIN departamentos ON departamentos.departamento_id = funcionarios.departamento_id
GROUP BY departamentos.departamento_nome
ORDER BY AVG(salario) DESC
LIMIT 1;
```

**2.6 Qual departamento possui o menor número de funcionários?**
```sql
SELECT departamentos.departamento_nome, COUNT(funcionarios.funcionario_id) as total_funcionarios
FROM funcionarios
INNER JOIN departamentos ON departamentos.departamento_id = funcionarios.departamento_id
GROUP BY departamentos.departamento_nome
ORDER BY total_funcionarios ASC
LIMIT 1;
```

---
## Nível 3: Recursos Humanos

**3.1 Quantos funcionários da empresa Momento possuem cônjuges?**
```sql
SELECT COUNT(*) 
FROM funcionarios 
WHERE estado_civil = 'Casado';
```

**3.2 Quantos funcionários possuem filhos registrados?**
```sql
SELECT COUNT(*) 
FROM funcionarios 
WHERE numero_filhos > 0;
```

**3.3 Qual funcionário foi contratado há mais tempo na empresa?**
```sql
SELECT nome, data_contratacao 
FROM funcionarios 
ORDER BY data_contratacao ASC 
LIMIT 1;
```

**3.4 Qual funcionário foi contratado há menos tempo na empresa?**
```sql
SELECT nome, data_contratacao 
FROM funcionarios 
ORDER BY data_contratacao DESC 
LIMIT 1;
```

**3.5 Liste os 5 funcionários com mais tempo de casa, ordenados pela data de contratação.**
```sql
SELECT nome, data_contratacao 
FROM funcionarios 
ORDER BY data_contratacao ASC 
LIMIT 5;
```

**3.6 Quantos funcionários foram contratados na década de 1990 (entre 1990-1999)?**
```sql
SELECT COUNT(*) 
FROM funcionarios 
WHERE data_contratacao BETWEEN '1990-01-01' AND '1999-12-31';
```

**3.7 Como a média salarial da Momento evoluiu ao longo dos anos? Agrupe por ano de contratação e calcule a média salarial.**
```sql
SELECT EXTRACT(YEAR FROM data_contratacao) AS ano, AVG(salario) AS media_salarial
FROM funcionarios
GROUP BY EXTRACT(YEAR FROM data_contratacao)
ORDER BY ano ASC;
```

---
## Nível 4: Operações e Escritórios

**4.1 Qual é o custo total de suprimentos em cada escritório? Ordene do mais caro ao mais barato.**
```sql
SELECT escritorio_id, SUM(quantidade * preco_unitario) AS custo_total
FROM suprimentos
GROUP BY escritorio_id
ORDER BY custo_total DESC;
```

**4.2 Qual escritório possui a maior quantidade de diferentes tipos de suprimentos?**
```sql
SELECT escritorio_id, COUNT(DISTINCT nome_suprimento) AS qtd_tipos
FROM suprimentos
GROUP BY escritorio_id
ORDER BY qtd_tipos DESC
LIMIT 1;
```

**4.3 Qual é o suprimento mais caro (considerando preço unitário) em toda a empresa?**
```sql
SELECT nome_suprimento, preco_unitario 
FROM suprimentos 
ORDER BY preco_unitario DESC 
LIMIT 1;
```

**4.4 Calcule o valor total do inventário de suprimentos da empresa (quantidade × preço unitário de todos os itens em todos os escritórios).**
```sql
SELECT SUM(quantidade * preco_unitario) AS valor_total_inventario 
FROM suprimentos;
```

---
## Nível 5: Produtos e Vendas

**5.1 Quais produtos foram vendidos pela Momento? Liste todos os produtos únicos.**
```sql
SELECT DISTINCT produto_nome 
FROM vendas;
```

**5.2 Qual é o produto mais vendido (maior quantidade total)?**
```sql
SELECT produto_nome, SUM(quantidade) AS total_vendido
FROM vendas
GROUP BY produto_nome
ORDER BY total_vendido DESC
LIMIT 1;
```

**5.3 Qual é o produto menos vendido?**
```sql
SELECT produto_nome, SUM(quantidade) AS total_vendido
FROM vendas
GROUP BY produto_nome
ORDER BY total_vendido ASC
LIMIT 1;
```

**5.4 Pensando na relação quantidade × valor unitário, qual produto gerou mais receita para a empresa?**
```sql
SELECT produto_nome, SUM(quantidade * preco_unitario) AS receita_total
FROM vendas
GROUP BY produto_nome
ORDER BY receita_total DESC
LIMIT 1;
```

**5.5 Qual é o produto mais caro (maior preço unitário) no catálogo?**
```sql
SELECT produto_nome, preco_unitario 
FROM produtos 
ORDER BY preco_unitario DESC 
LIMIT 1;
```

**5.6 Qual foi o faturamento total da empresa em vendas?**
```sql
SELECT SUM(quantidade * preco_unitario) AS faturamento_total 
FROM vendas;
```

**5.7 Quantas vendas foram realizadas no mês de junho de 2023?**
```sql
SELECT COUNT(*) 
FROM vendas 
WHERE data_venda BETWEEN '2023-06-01' AND '2023-06-30';
```

**5.8 Qual vendedor realizou mais vendas (em quantidade de transações)?**
```sql
SELECT vendedor_id, COUNT(*) AS total_transacoes
FROM vendas
GROUP BY vendedor_id
ORDER BY total_transacoes DESC
LIMIT 1;
```

**5.9 Qual vendedor gerou mais receita para a empresa?**
```sql
SELECT vendedor_id, SUM(quantidade * preco_unitario) AS receita_gerada
FROM vendas
GROUP BY vendedor_id
ORDER BY receita_gerada DESC
LIMIT 1;
```

---
## Nível 6: Operações de Atualização

**6.1 Um novo departamento foi criado: Inovações. Ele será alocado no escritório "Wayne Offices". Adicione-o ao banco de dados.**
```sql
INSERT INTO departamentos (departamento_nome, escritorio_id)
VALUES ('Inovações', (SELECT escritorio_id FROM escritorios WHERE nome = 'Wayne Offices'));
```

**6.2 O departamento de Inovações está sem funcionários. Transfira 2 funcionários do departamento de Tecnologia para Inovações.**
```sql
UPDATE funcionarios
SET departamento_id = (SELECT departamento_id FROM departamentos WHERE departamento_nome = 'Inovações')
WHERE departamento_id = (SELECT departamento_id FROM departamentos WHERE departamento_nome = 'Tecnologia')
LIMIT 2;
```

**6.3 A empresa decidiu dar um aumento de 10% para todos os funcionários do departamento de Tecnologia. Atualize os salários.**
```sql
UPDATE funcionarios
SET salario = salario * 1.10
WHERE departamento_id = (SELECT departamento_id FROM departamentos WHERE departamento_nome = 'Tecnologia');
```

**6.4 O funcionário "Bruce Ernst" foi promovido a "Senior Web Developer" e recebeu um aumento para $5.000. Atualize suas informações.**
```sql
UPDATE funcionarios
SET cargo_id = (SELECT cargo_id FROM cargos WHERE cargo_nome = 'Senior Web Developer'), 
    salario = 5000.00
WHERE nome = 'Bruce Ernst';
```

**6.5 Adicione um novo suprimento ao escritório "Wayne Offices": 15 unidades de "Headsets" com preço unitário de $150 cada.**
```sql
INSERT INTO suprimentos (nome_suprimento, quantidade, preco_unitario, escritorio_id)
VALUES ('Headsets', 15, 150.00, (SELECT escritorio_id FROM escritorios WHERE nome = 'Wayne Offices'));
```

**6.6 Todos os funcionários contratados antes de 1990 estão aposentando. Remova-os do banco de dados.**
```sql
DELETE FROM funcionarios 
WHERE data_contratacao < '1990-01-01';
```

---
## Nível 7: Análise Avançada com Agregações

**7.1 Crie um relatório mostrando cada departamento com: nome do departamento, número de funcionários, salário total e média salarial.**
```sql
SELECT d.departamento_nome, 
       COUNT(f.funcionario_id) AS numero_funcionarios, 
       SUM(f.salario) AS salario_total, 
       AVG(f.salario) AS media_salarial
FROM departamentos d
LEFT JOIN funcionarios f ON d.departamento_id = f.departamento_id
GROUP BY d.departamento_nome;
```

**7.2 Liste os 3 cargos mais comuns na empresa e quantos funcionários ocupam cada cargo.**
```sql
SELECT c.cargo_nome, COUNT(f.funcionario_id) AS total_funcionarios
FROM cargos c
INNER JOIN funcionarios f ON c.cargo_id = f.cargo_id
GROUP BY c.cargo_nome
ORDER BY total_funcionarios DESC
LIMIT 3;
```

**7.3 Encontre todos os funcionários que ganham acima da média salarial de seus respectivos departamentos.**
```sql
SELECT f.nome, f.salario, d.departamento_nome
FROM funcionarios f
INNER JOIN departamentos d ON f.departamento_id = d.departamento_id
WHERE f.salario > (
    SELECT AVG(salario) 
    FROM funcionarios 
    WHERE departamento_id = f.departamento_id
);
```

**7.4 Calcule a taxa de crescimento da empresa por ano (quantos funcionários foram contratados a cada ano).**
```sql
SELECT EXTRACT(YEAR FROM data_contratacao) AS ano, COUNT(*) AS novas_contratacoes
FROM funcionarios
GROUP BY EXTRACT(YEAR FROM data_contratacao)
ORDER BY ano ASC;
```

**7.5 Crie um ranking dos vendedores, mostrando: nome do vendedor, número de vendas realizadas e receita total gerada.**
```sql
SELECT f.nome AS vendedor, 
       COUNT(v.venda_id) AS numero_vendas, 
       SUM(v.quantidade * v.preco_unitario) AS receita_total
FROM vendas v
INNER JOIN funcionarios f ON v.vendedor_id = f.funcionario_id
GROUP BY f.nome
ORDER BY receita_total DESC;
```

**7.6 Encontre os produtos que foram vendidos por apenas um vendedor.**
```sql
SELECT produto_nome
FROM vendas
GROUP BY produto_nome
HAVING COUNT(DISTINCT vendedor_id) = 1;
```
