# Level 2
## Agregamos joins

### 1. Obtener los alumnos con su carrera. Mostrar solo las columnas nombre y apellido de la tabla **platzi.alumnos**, y la columna carrera de la tabla **platzi.carreras**

```sql
select a.nombre, a.apellido, c.carrera
from platzi.alumnos as a
left join platzi.carreras as c
	on a.carrera_id = c.id;
```

### 2. Obtener las carreras que no tengan ningún alumno asociado

```sql
select *
from platzi.carreras as c
left join platzi.alumnos as a
	on c.id = a.carrera_id
where a.id is null;
```

### 3. Obtener las carreras que no tengan ningún alumno asociado, y los alumnos que no tengan una carrera asociada

```sql
select a.nombre, a.apellido, c.carrera
from platzi.alumnos as a
full outer join platzi.carreras as c
	on a.carrera_id = c.id
where c.id is null or a.carrera_id is null;
```

### 4. Obtener la cantidad alumnos agrupados por carrera. Mostrar solo las columnas Carrera y Cantidad

```sql
select c.carrera as carrera, count(a.nombre) as cantidad
from platzi.alumnos as a
left join platzi.carreras as c
	on a.carrera_id = c.id
group by c.carrera;
```

### 5. Obtener la fecha de incorporación más reciente de un alumno por cada carrera. Solo mostrar el nombre de la carrera y la fecha de incorporación

```sql
select c.carrera, max(a.fecha_incorporacion)
from platzi.alumnos as a
left join platzi.carreras as c
	on a.carrera_id = c.id
group by c.carrera;
```