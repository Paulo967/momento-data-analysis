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
