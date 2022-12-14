# Level 1
## Complejidad básica

### 1. Obtener los registros que tengan id entre 1 y 10 de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
where id between 1 and 10;
```

### 2. Obtener los registros donde apellido empiece con la letra A de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
where apellido like 'A%';
```

### 3. Obtener los registros ordenados por colegiatura de forma descendente

```sql
select *
from platzi.alumnos
order by colegiatura desc;
```

### 4. Obtener los registros agrupados por tutor_id y ordenados por colegiatura de manera ascendente

```sql
select *
from platzi.alumnos
group by tutor_id
order by colegiatura;
```