# Comandos da Etapa 3

## Saída

### 1 alunos que nasceram antes de 2009
```sql
SELECT id, nome, data_de_nascimento
FROM alunos
WHERE data_de_nascimento < '2009-01-01';
```

### 2 Faça uma consulta que calcule a média das notas de cada aluno e as mostre com duas casas decimais.

```sql
SELECT alunos.nome AS aluno,
   SUM(alunos.primeira_nota) AS "nota 1",
   SUM(alunos.segunda_nota) AS "nota 2",
   ROUND((SUM(alunos.primeira_nota) + SUM(alunos.segunda_nota)) / 2, 2) AS "media"
   FROM alunos
   GROUP BY alunos.nome;
```

### 3 Faça uma consulta que calcule o limite de faltas de cada curso de acordo com a carga horária. Considere o limite como 25% da carga horária. Classifique em ordem crescente pelo título do curso.

```sql
SELECT id,
       titulo,
       carga_horaria,
       ROUND(carga_horaria * 0.25) AS limite_faltas_em_horas
FROM cursos
ORDER BY titulo ASC;
```

### 4 Faça uma consulta que mostre os nomes dos professores que são somente da área "desenvolvimento".

```sql
SELECT professores.nome
FROM professores
WHERE professores.atuacao = 'desenvolvimento'
AND EXISTS (
    SELECT 1
    FROM professores
    WHERE id = professores.id
      AND atuacao = 'desenvolvimento'
);

```

### 5 Faça uma consulta que mostre a quantidade de professores que cada área ("design", "infra", "desenvolvimento") possui.

```sql
SELECT atuacao, COUNT(*) AS quantidade_de_professores
FROM professores
WHERE atuacao IN ('design', 'infra', 'desenvolvimento')
GROUP BY atuacao;
```

### 6 Faça uma consulta que mostre o nome dos alunos, o título e a carga horária dos cursos que fazem.

```sql
SELECT alunos.nome, cursos.titulo, cursos.carga_horaria 
FROM alunos 
INNER JOIN cursos ON alunos.curso_id = cursos.id;
```

### 7 Faça uma consulta que mostre o nome dos professores e o título do curso que lecionam. Classifique pelo nome do professor.

```sql
SELECT professores.nome, cursos.titulo 
FROM professores
INNER JOIN cursos ON professores.curso_id = cursos.id
GROUP BY professores.nome
```

### 8 Faça uma consulta que mostre o nome dos alunos, o título dos cursos que fazem, e o professor de cada curso.

```sql
SELECT alunos.nome, cursos.titulo, professores.nome AS Professor
FROM alunos
INNER JOIN cursos ON alunos.curso_id = cursos.id
INNER JOIN professores ON professores.curso_id = cursos.id
```

### 9 Faça uma consulta que mostre a quantidade de alunos que cada curso possui. Classifique os resultados em ordem descrecente de acordo com a quantidade de alunos.

```sql
SELECT cursos.titulo AS curso,
       COUNT(*) AS quantidade_de_alunos
FROM cursos
JOIN alunos ON cursos.id = alunos.curso_id
GROUP BY cursos.titulo
ORDER BY quantidade_de_alunos DESC;
```

### 10 Faça uma consulta que mostre o nome dos alunos, suas notas, médias, e o título dos cursos que fazem. Devem ser considerados somente os alunos de Front-End e Back-End. Mostre os resultados classificados pelo nome do aluno.

```sql
SELECT alunos.nome, alunos.primeira_nota, alunos.segunda_nota, 
   ROUND((alunos.primeira_nota + alunos.segunda_nota) / 2, 2) AS "media",
   cursos.titulo
FROM alunos
INNER JOIN cursos ON cursos.id = alunos.curso_id
WHERE curso_id IN (7, 8)
ORDER BY alunos.nome DESC;
```

### 11 Faça uma consulta que altere o nome do curso de Figma para Adobe XD e sua carga horária de 10 para 15.

```sql
UPDATE cursos SET titulo = 'Adobe XD'
WHERE id = 10
```

### 12 Faça uma consulta que exclua um aluno do curso de Redes de Computadores e um aluno do curso de UX/UI.

```sql
DELETE FROM alunos WHERE id IN(11, 8) 
```

### 13 Faça uma consulta que mostre a lista de alunos atualizada e o título dos cursos que fazem, classificados pelo nome do aluno.

```sql
SELECT alunos.nome AS aluno, cursos.titulo
FROM alunos
INNER JOIN cursos ON cursos.id = alunos.curso_id
ORDER BY alunos.nome
```

### DESAFIOS
- Criar uma consulta que calcule a idade do aluno
```sql
SELECT 
    alunos.nome, 
    alunos.data_de_nascimento,
    TIMESTAMPDIFF(YEAR, alunos.data_de_nascimento, CURDATE()) AS idade
FROM 
    alunos;

```

- Criar uma consulta que calcule a média das notas de cada aluno e mostre somente os alunos que tiveram a média maior ou igual a 7.
```sql
SELECT alunos.id,
       CAST((primeira_nota + segunda_nota) / 2 AS DECIMAL(10, 2)) AS media
FROM alunos
WHERE (primeira_nota + segunda_nota) / 2 >= 7;

```

- Criar uma consulta que calcule a média das notas de cada aluno e mostre somente os alunos que tiveram a média menor que 7.
```sql
SELECT alunos.id,
       CAST((primeira_nota + segunda_nota) / 2 AS DECIMAL(10, 2)) AS media
FROM alunos
WHERE (primeira_nota + segunda_nota) / 2 < 7;
```

- Criar uma consulta que mostre a quantidade de alunos com média maior ou igual a 7.
```sql
SELECT COUNT(*) AS quantidade_alunos
FROM (
    SELECT alunos.id
    FROM alunos
    WHERE (primeira_nota + segunda_nota) / 2 >= 7
) AS alunos_aprovados;


```
