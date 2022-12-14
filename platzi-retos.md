# Retos del curso de Platzi

### 1. Obtener los 5 primeros resultados de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
limit 5;
```

```sql
select *
from platzi.alumnos
fetch first 5 rows only;
```

Solución de Israel Vázquez Morales (Platzi team)
```sql
select *
from (
	select row_number() over() as row_id, *
	from platzi.alumnos
) as alumnos_with_row_num
where row_id <= 5;
```

### 2. Obtener la segunda mitad de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
offset (
	select count(*)
	from platzi.alumnos / 2);
```

Solución de Israel Vázquez Morales (Platzi team)
```sql
select row_number() over() as row_id, *
from platzi.alumnos
offset (
	select count(*) / 2
	from platzi.alumnos
	);
```

### 3. Obtener todos los registros (rows) excepto los que tengan **tutor_id = 30** de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
where id not in (
	select id
	from platzi.alumnos
	where tutor_id = 30
);
```

### 4. Extraer hora, minuto y segundo del campo **fecha_incorporacion** de la tabla **platzi.alumnos**

```sql
select date_part('HOUR', fecha_incorporacion) as hora_incorporacion,
				date_part('MINUTE' fecha_incorporacion) as minuto_incorporacion,
				date_part('SECOND' fecha_incorporacion) as segundo_incorporacion
from platzi.alumnos;
```

```sql
select extract(HOUR from fecha_incorporacion) as hora_incorporacion
from platzi.alumnos;
```

### 5. Obtener los registros que tengan fecha_incorporacion = 2019 del mes 05 de la tabla **platzi.alumnos**

```sql
select *
from platzi.alumnos
where (
		extract(YEAR from fecha_incorporacion) = 2019
	and extract(MONTH from fecha_incorporacion) = 05
);
```

```sql
select *
from (
	select *,
		date_part('YEAR', fecha_incorporacion) as año_incorporacion,
		date_part('MONTH', fecha_incorporacion) as mes_incorporacion
	from platzi.alumnos
) as alumnos_with_año
where año_incorporacion = 2019
	and mes_incorporacion = 05;
```

### 6. Eliminar los registros duplicados
> **AYUDA**: querie para obtener los registros duplicados
> ```
> select *
> from (
> 	select id,
>		row_number() over(
>			partition by
>				nombre,
>				apellido,
>				email,
>				colegiatura,
>				fecha_incorporacion,
>				carrera_id,
>				tutor_id
>			order by id asc
>			) as row,
>	*
>	from platzi.alumnos
> ) as duplicados
> where duplicados.row > 1;
> ```


```sql
delete from platzi.alumnos
where id in (select id
from (
	select *
	from (
		select
			row_number() over(
				partition by
					nombre,
					apellido,
					email,
					colegiatura,
					fecha_incorporacion,
					carrera_id,
					tutor_id
				order by id asc
				) as row, *
		from platzi.alumnos
		) as duplicados
where duplicados.row > 1) as duplicados_id);
```

### 7. Obtener la intersección entre tutor_id y carrera_id de la tabla **platzi.alumnos**
> Es decir, los alumnos que comparten tutor y carrera

```sql
select numrange(
	(select min(tutor_id) from platzi.alumnos),
	(select max(tutor_id) from platzi.alumnos)
	)
	*
	numrange(
		(select min(carrera_id) from platzi.alumnos),
		(select max(carrera_id) from platzi.alumnos)
	);
```

### 8. Obtener el mínimo nombre (alfabéticamente) de la tabla **platzi.alumnos**

```sql
select min(nombre)
from platzi.alumnos;
```

```
select *
from platzi.alumnos
order by nombre desc
limit 1;
```

### 9. Obtener el promedio de alumnos por tutor de la tabla **platzi.alumnos**

```sql
select avg(alumnos_por_tutor) as promedio_alumnos_por_tutor
from (
	select concat(t.nombre, ' ', t.apellido) as tutor,
				count(*) as alumnos_por_tutor
	from platzi.alumnos as a
	inner join platzi.alumnos as t
		on a.tutor_id = t.id
	group by tutor
	order by alumnos_por_tutor desc
	);
```

### 10. Obtener todos los registros de ambas tablas

```sql
select a.nombre, a.apellido, a,carrera_id, c.id, c.carrera
from platzi.alumnos as a
full outer join platzi.carreras as c
	on a.carrera_id = c.id;
```

### 11. Mostrar un triangulo de *
> RESULTADO  
> ```
> *
> **
> ***
> ``` 

```sql
select lpad('*', cast(row_id as int), '*')
from (
		select row_number() over(order by carrera_id) as row_id, *
		from platzi.alumnos
	) as alumnos_with_row_id
where row_id <= 5
order by carrera_id;
```

```
select rpad('*', cast(row_id as int), '*')
from (
		select row_number() over(order by carrera_id) as row_id, *
		from platzi.alumnos
	) as alumnos_with_row_id
where row_id <= 5
order by carrera_id;
```

### 12. Generar el triángulo del reto anterior pero con rangos

```sql
select lpad('*',cast(s.a as int),'*')
from generate_series(1,10) as s(a);

select lpad('*',cast(ordinality as int),'*')
from generate_series(10,2,-2) with ordinality;
```