# Level 1
## Complejidad básica

### 1. Obtener los registros que tengan id entre 1 y 10 de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
where id between 1 and 10;
```

### 2. Obtener las carreras vigentes de la tabla **platzi.carreras**

```sql
select *
from platzi.carreras
where vigente;
```

### 3. Obtener los registros donde apellido empiece con la letra A de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
where apellido like 'A%';
```

### 4. Obtener los registros ordenados por colegiatura de forma descendente

```sql
select *
from platzi.alumnos
order by colegiatura desc;
```

### 5. Obtener los registros ordenados por tutor_id y colegiatura de manera ascendente

```sql
select colegiatura, tutor_id
from platzi.alumnos
order by tutor_id, colegiatura;
```

### 6. Obtener los registros que NO tengan appelido NULL de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
where apellido is not null;
```