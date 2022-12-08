# Prácticas SQL
Este repositorio contiene los retos resueltos del **curso práctico de SQL** de Platzi. 
Además, se encontrarán respuestas (en forma de querie SQL) a ciertas preguntas, con el fin de practicar los conocimientos del lenguaje. 
Cabe mencionar que cualquier persona puede usar este repositorio para responder por su cuenta las preguntas y comparar los resultados; es importante tener en cuenta que existen más de un camino para llegar a dichas respuestas. 
 
## Base de datos (DB)
Yo voy a utilizar el motor de DB PostgreSQL, a continuación dejo las instrucciónes para crear la DB para resolver los ejercicios.  
 
Una vez instalado PostgreSQL y pgAdmin vamos a crear la estructura de datos. En los archivos platzi-carreras.sql y platzi-alumnos.sql se encuentra el código SQL para hacerlo.  
Primero se crea una Base de datos, luego dar click derecho en la sección Schemas, y en Create -> Schema.  
Al seleccionar la opción abrirá un cuadro de diálogo en donde debes escribir el nombre del esquema, en este caso será “platzi”. Selecciona tu usuario de postgres en el campo Owner, esto es para que asigne todos los permisos del nuevo esquema a tu usuario.  
 
Dirígete al menú superior y selecciona el menú Tools > Query Tool. 
Esto desplegará la herramienta en la ventana principal. Da click en el botón “Open File” ilustrado por un icono de folder abierto.  
Previamente debes descargar los archivos **platzi-carreras.sql** y **platzi-alumnos.sql**, para poder seleccionarlos. En caso de no descargar los archivos, puedes copiar y pegar el código en la ventana Query Tool. Es importante que primero ejecutes el código del archivo platzi-alumnos.sql  
Para ejecutar el código, debes seleccionarlo todo (con **Ctrl + A** en Windows) y presionar en el ícono de play.  
Realizar el mismo proceso con **platzi-carreras.sql** 

## Retos del curso de Platzi
### 1. Obtener los 5 primeros resultados de la tabla **platzi.alumnos**

```
select *
from platzi.alumnos
limit 5;
```

```
select *
from platzi.alumnos
fetch first 5 rows only;
```

Solución de Israel Vázquez Morales (Platzi team)
```
select *
from (
	select row_number() over() as row_id, *
	from platzi.alumnos
) as alumnos_with_row_num
where row_id <= 5;
```

### 2. Obtener la segunda mitad de la tabla **platzi.alumnos**

```
select *
from platzi.alumnos
offset (
	select count(*)
	from platzi.alumnos / 2);
```

Solución de Israel Vázquez Morales (Platzi team)
```
select row_number() over() as row_id, *
from platzi.alumnos
offset (
	select count(*) / 2
	from platzi.alumnos
	);
```

### 3. Obtener todos los registros (rows) excepto los que tengan **tutor_id = 30** de la tabla **platzi.alumnos**

```
select *
from platzi.alumnos
where id not in (
	select id
	from platzi.alumnos
	where tutor_id = 30
);
```

### 4. Extraer hora, minuto y segundo del campo **fecha_incorporacion** de la tabla **platzi.alumnos**

```
select date_part('HOUR', fecha_incorporacion) as hora_incorporacion,
				date_part('MINUTE' fecha_incorporacion) as minuto_incorporacion,
				date_part('SECOND' fecha_incorporacion) as segundo_incorporacion
from platzi.alumnos;
```

```
select extract(HOUR from fecha_incorporacion) as hora_incorporacion
from platzi.alumnos;
```

### 5. Obtener los registros que tengan fecha_incorporacion = 2019 del mes 05 de la tabla **platzi.alumnos**

```
select *
from platzi.alumnos
where (
		extract(YEAR from fecha_incorporacion) = 2019
	and extract(MONTH from fecha_incorporacion) = 05
);
```

```
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
> **AYUDA** ``` -- querie para obtener los registros duplicados
select *
from (
	select id,
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
			) as row,
	*
	from platzi.alumnos
) as duplicados
where duplicados.row > 1; ```