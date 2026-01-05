# Oscar Vai Para...

Código usado em MySQL para manipulação de dados especificos de uma base dados sobre os indicados ao oscar de 1928 a 2024, realizando uma atividade de 12 questões.

## 🔨 Funcionalidades do projeto

Nesse código, será utilizado a função select para selecionar valores especificos e sobre certas condições. O uso do upadate para atualizar certos valores na base de dados, e por fim, o uso do Insert Into para inserir novos indicados ao oscar.

## ✔️ Técnicas e tecnologias utilizadas

- `MySQL`: Todo código foi feito nesse sistema.
- `SET SQL_SAFE_UPDATES = 0;`: função usada para ativar e desativar a segurança de Update

## 🎯 Atividades

1. Quantas vezes Natalie Portman foi indicada ao Oscar?

```sql
SELECT 
Count(*)
FROM indicados_ao_oscar 
WHERE nome_do_indicado = "Natalie Portman";
```

### Retorno

|Count(*)|
|:---:|
3
---

2. Quantos Oscars Natalie Portman ganhou?

```sql
SELECT 
Count(*)
FROM indicados_ao_oscar  
WHERE nome_do_indicado = "Natalie Portman" and vencedor = "sim";
```
### Retorno

|Count(*)|
|:---:|
1
---


3. Amy Adams já ganhou algum Oscar?

```sql
SELECT 
Count(*)
FROM indicados_ao_oscar  
WHERE nome_do_indicado = "Amy Adams" and vencedor = "Sim";
```
### Retorno

|Count(*)|
|:---:|
0
---

4. A série de filmes Toy Story ganhou um Oscar em quais anos?

```sql
SELECT 
ano_cerimonia
FROM indicados_ao_oscar  
WHERE nome_do_filme Like "%Toy Story%" and vencedor = "Sim";
```
### Retorno

|ano_cerimonia|
|:---:|
2011
2020
---

5. A partir de que ano que a categoria "Actress" deixa de existir? 

```sql
SELECT 
Max(ano_cerimonia)
FROM indicados_ao_oscar  
WHERE categoria = "Actress";
```
### Retorno

|Max(ano_cerimonia)|
|:---:|
1976
---

6.  O primeiro Oscar para melhor Atriz foi para quem? Em que ano?

```sql
SELECT 
nome_do_indicado, ano_cerimonia
FROM indicados_ao_oscar  
WHERE categoria = "Actress" and vencedor = "Sim"
ORDER BY ano_cerimonia ASC
LIMIT 1;
```
### Retorno

|nome_do_indicado|ano_cerimonia|
|:---:|:---:|
Janet Gaynor | 1928
---

7.  Na coluna/campo "Vencedor", altere todos os valores com "Sim" para 1 e todos os valores "Não" para 0.

```sql
SET SQL_SAFE_UPDATES = 0;

UPDATE indicados_ao_oscar
SET vencedor = CASE
    WHEN vencedor = 'Sim' THEN 1
    WHEN vencedor = 'Não' THEN 0
    ELSE vencedor
END;

SET SQL_SAFE_UPDATES = 1;
```
### Retorno

|vencedor|
|:---:|
0
1
0
1
0
0
1
...

#### OBS: Foi utilizado o seguinte de código para retornar os valores para sim e não no início do código  para que seja possível testar o código quantas vezes necessário:

```sql
SET SQL_SAFE_UPDATES = 0;

UPDATE indicados_ao_oscar 
SET vencedor = CASE
	WHEN vencedor = 1 THEN 'Sim'
	WHEN vencedor = 0 THEN 'Não'
	ELSE vencedor
END
WHERE vencedor REGEXP '^-?[0-9]+$' AND vencedor IN (0 , 1);

SET SQL_SAFE_UPDATES = 1;
```
---

8.  Em qual edição do Oscar "Crash" concorreu ao Oscar?

```sql
select cerimonia from indicados_ao_oscar
where nome_do_filme = "Crash";
```
### Retorno

|cerimonia|
|:---:|
78
---

9.  Bom... dê um Oscar para um filme que merece muito, mas não ganhou.

```sql
SET SQL_SAFE_UPDATES = 0;

UPDATE indicados_ao_oscar
set vencedor = 1
where nome_do_filme = "City of God";

SET SQL_SAFE_UPDATES = 1;
```
### Retorno

|ano_filmagem|ano_cerimonia|cerimonia|categoria|nome_do_indicado|nome_do_filme|vencedor|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|2003|2004|76|CINEMATOGRAPHY|Cesar Charlone|City of God|1|
|2003|2004|76|DIRECTING|Fernando Meirelles|City of God|1|
|2003|2004|76|FILM EDITING|Daniel Rezende|City of God|1|
|2003|2004|76|WRITING (Adapted Screenplay)|Screenplay by Braulio Mantovani|City of God|1|

---

10.  O filme Central do Brasil aparece no Oscar?
```sql
select * from indicados_ao_oscar
where nome_do_filme = "Central Station";
```
### Retorno

|ano_filmagem|ano_cerimonia|cerimonia|categoria|nome_do_indicado|nome_do_filme|vencedor|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1998|1999|71|ACTRESS IN A LEADING ROLE|Fernanda Montenegro|Central Station|0
1998|1999|71|FOREIGN LANGUAGE FILM|Brazil|Central Station|0

---

11.  Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser. 
```sql
INSERT INTO indicados_ao_oscar(
ano_filmagem,
ano_cerimonia,
cerimonia,
categoria,
nome_do_indicado,
nome_do_filme,
vencedor) VALUES (2021,2022,94,'MUSIC (Original Song)','We Don’t Talk About Bruno','Encanto','Sim');

INSERT INTO indicados_ao_oscar(
ano_filmagem,
ano_cerimonia,
cerimonia,
categoria,
nome_do_indicado,
nome_do_filme,
vencedor) VALUES (2004,2005,77,'CINEMATOGRAPHY','Andrew Niccol','The Terminal','Sim');

INSERT INTO indicados_ao_oscar(
ano_filmagem,
ano_cerimonia,
cerimonia,
categoria,
nome_do_indicado,
nome_do_filme,
vencedor) VALUES (2023,2024,96,'CINEMATOGRAPHY','Seth Rogen','As Tartarugas Ninja: Caos Mutante','Sim');
```
### Retorno

|ano_filmagem|ano_cerimonia|cerimonia|categoria|nome_do_indicado|nome_do_filme|vencedor|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|2021|2022|94|MUSIC (Original Song)|We Don’t Talk About Bruno|Encanto|Sim|
|2004|2005|77|CINEMATOGRAPHY|Andrew Niccol|The Terminal|Sim|
|2023|2024|96|CINEMATOGRAPHY|Seth Rogen|As Tartarugas Ninja: Caos Mutante|Sim|

---

12.  Pensando no ano em que você nasceu: Qual foi o Oscar de melhor filme, Melhor Atriz e Melhor Diretor?
```sql
select nome_do_filme
from indicados_ao_oscar
where ano_cerimonia = 2001 and vencedor = 1 and categoria = "CINEMATOGRAPHY";

select nome_do_indicado
from indicados_ao_oscar
where ano_cerimonia = 2001 and vencedor = 1 and categoria LIKE "%ACTRESS%";

select nome_do_indicado
from indicados_ao_oscar
where ano_cerimonia = 2001 and vencedor = 1 and categoria LIKE "%ACTOR%";
```
### Retorno

|nome_do_filme|
|:---:|
Crouching Tiger, Hidden Dragon

|nome_do_indicado|
|:---:|
Julia Roberts
Marcia Gay Harden

|nome_do_indicado|
|:---:|
Russell Crowe
Benicio Del Toro
---

## Autor

<div>
  <a href="https://github.com/christianwl">
    <img src="https://contrib.rocks/image?repo=christianwl/oscar-database" alt="Autor do Portfolio"/>
  </a>
</div>
