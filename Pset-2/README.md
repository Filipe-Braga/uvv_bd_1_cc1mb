## NOTA<br>
por alguma razão, as reduçoes feitas com o "AS" não funcionaram no meu terminal.


## Questão 1
SELECT numero_departamento,AVG (salario) 'média salarial por departamento'

FROM funcionario

GROUP BY numero_departamento;


## Questão 2
SELECT sexo,AVG(salario) AS 'media salarial'

FROM funcionario

WHERE sexo = 'M'

UNION

SELECT sexo, AVG(salario)

FROM funcionario

WHERE sexo = 'F';
  
  
## Questão 3
SELECT 
	departamento.nome_departamento, concat(primeiro_nome," ",nome_meio,". ",ultimo_nome) as nome_completo, funcionario.data_nascimento, year(curdate())-year(funcionario.data_nascimento) as idade, funcionario.salario

FROM
	departamento INNER JOIN funcionario on departamneto.numero_departamento=funcionario.numero_departamento;
  
## Questão 4
SELECT 
	concat(primeiro_nome," ",nome_meio,". ",ultimo_nome) as nome_completo, salario, 
  
case salario <br>
	WHEN salario <= 35000 THEN salario*1.20 <br>
	ELSE salario*1.15 <br>
	END AS salario_reajustado <br>

FROM
	funcionario;
	
## Questão 5
SELECT*
FROM (SELECT concat("gerente do departamento de ", departamento.nome_departamento) as nome_departamento, concat(funcionario.primeiro_nome, " ",funcionario.nome_meio, ". ",funcionario.ultimo_nome) as nome_funcionario, funcionario.salario 

FROM departamento 

INNER JOIN funcionario ON departamento.numero_departamento = funcionario.numero_departamento WHERE cpf_gerente = cpf 

ORDER BY nome_departamento asc) as gerente

UNION

SELECT*
FROM (SELECT departamento.nome_departamento, concat(funcionario.primeiro_nome, " ", funcionario.nome_meio, ". ",funcionario.ultimo_nome) as nome_funcionario, funcionario.salario 

FROM departamento 

INNER JOIN funcionario on departamento.numero_departamento = funcionario.numero_departamento 

WHERE NOT cpf_gerente = cpf 

ORDER BY salario desc) funcionario;

## Questão 6
SELECT distinct
    concat(dependente.nome_dependente," ",funcionario.ultimo_nome) as nome_dependente, concat( year(curdate())-year(dependente.data_nascimento), " anos") as idade, concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome) as nome_funcionario, departamento.nome_departamento,

case dependente.sexo <br>
      WHEN 'M' then 'Masculino' <br>
      WHEN 'F' then 'Feminino' <br>
END as sexo_dependente

FROM (dependente, funcionario, departamento)

INNER JOIN dependente as d ON (dependente.cpf_funcionario = funcionario.cpf)

INNER JOIN funcionario as f ON (departamento.numero_departamento = funcionario.numero_departamento) WHERE dependente.cpf_funcionario = funcionario.cpf ;


## Questão 7
SELECT distinct
	concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome) as nome_funcionario, departamento.nome_departamento, funcionario.salario 

FROM (funcionario, departamento)

LEFT JOIN dependente ON dependente.cpf_funcionario = funcionario.cpf 

INNER JOIN funcionario as f ON (departamento.numero_departamento = funcionario.numero_departamento) 

WHERE dependente.cpf_funcionario is NULL;

## Questão 8
SELECT distinct
departamento.nome_departamento, projeto.nome_projeto, concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome) AS nome_funcionario, trabalha_em.horas

FROM
(departamento, funcionario, projeto, trabalha_em)

INNER JOIN funcionario AS f ON (departamento.numero_departamento = funcionario.numero_departamento)

INNER JOIN projeto AS p ON (departamento.numero_departamento = projeto.numero_departamento)

INNER JOIN trabalha_em AS t ON (funcionario.cpf=trabalha_em.cpf_funcionario)

INNER JOIN projeto AS pr ON trabalha_em.numero_projeto = projeto.numero_projeto WHERE funcionario.numero_departamento = projeto.numero_departamento

GROUP BY departamento.nome_departamento, projeto.nome_projeto, concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome), trabalha_em.horas
ORDER BY nome_departamento asc;

## QUestão 9
SELECT distinct
	departamento.nome_departamento, projeto.nome_projeto, sum(trabalha_em.horas) AS horas_trabalhadas

FROM 
	(departamento, projeto, trabalha_em)

INNER JOIN departamento AS d ON (projeto.numero_departamento = departamento.numero_departamento)

INNER JOIN trabalha_em AS t ON (projeto.numero_projeto = trabalha_em.numero_projeto)
GROUP BY  projeto.nome_projeto, departamento.nome_departamento asc; 

## Questão 10
SELECT departamento.nome_departamento,
  AVG (funcionario.salario) 'média salarial por departamento'

FROM
  (funcionario, departamento)

INNER JOIN funcionario AS f ON (departamento.numero_departamento = funcionario.numero_departamento)
GROUP BY departamento.nome_departamento;

## Questão 11
SELECT distinct
	concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome) AS nome_funcionario, projeto.nome_projeto, trabalha_em.horas, trabalha_em.horas*50 AS valo_total

FROM 
	(departamento, funcionario, projeto, trabalha_em)

INNER JOIN departamento AS d ON (projeto.numero_departamento =  departamento.numero_departamento)

INNER JOIN funcionario AS f ON (funcionario.cpf = trabalha_em.cpf_funcionario)

INNER JOIN trabalha_em AS t ON (projeto.numero_projeto = trabalha_em.numero_projeto)
GROUP BY  concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome), projeto.nome_projeto; 

## Questão 12
SELECT distinct
concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome) AS nome_funcionario, departamento.nome_departamento, projeto.nome_projeto

FROM (funcionario, departamento, projeto, trabalha_em)

INNER JOIN funcionario AS f ON (trabalha_em.cpf_funcionario=funcionario.cpf)

INNER JOIN departamento AS d ON (funcionario.numero_departamento = departamento.numero_departamento)

INNER JOIN projeto AS p ON (trabalha_em.numero_projeto = projeto.numero_projeto)

WHERE trabalha_em.horas is NULL;

## Questão 13

SELECT
concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome," (funcionario)") AS nome, 
funcionario.sexo, 
concat( year(curdate())-year(funcionario.data_nascimento), " anos") as idade

FROM (funcionario, dependente, departamento)

UNION

SELECT 
concat(dependente.nome_dependente," ",funcionario.ultimo_nome, " (dependente)") as nome, 
dependente.sexo, concat( year(curdate())-year(dependente.data_nascimento), " anos") as idade

FROM (funcionario, dependente, departamento)

WHERE dependente.cpf_funcionario = funcionario.cpf AND departamento.numero_departamento = funcionario.numero_departamento
ORDER BY idade desc;

## Questão 14
SELECT departamento.nome_departamento, count(funcionario.cpf) AS numero_funcionarios

FROM (departamento, funcionario)

INNER JOIN funcionario AS f ON (funcionario.numero_departamento = departamento.numero_departamento)
GROUP BY departamento.nome_departamento;

 ## Questão 15
 
 #### Esta questão está por hora incompleta.
 
SELECT 
concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome) AS nome, departamento.nome_departamento, projeto.nome_projeto

FROM (funcionario, projeto, departamento)

WHERE funcionario.numero_departamento = departamento.numero_departamento AND projeto.numero_departamento = departamento.numero_departamento 
GROUP BY concat(funcionario.primeiro_nome," ",funcionario.nome_meio,". ",funcionario.ultimo_nome),departamento.nome_departamento, projeto.nome_projeto;
