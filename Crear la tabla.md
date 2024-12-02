# Crear la tabla

```sql
create database electro;
```

# Usar la tabla

```sql
use electro;
```

# Crear la tabla *tienda*

```sql
create table tienda (
	id int not null,
    nombre char(20) not null,
    ciudad char(20) not null,
    numerotrabajadores int not null,
    superficie int not null,
    primary key (id)
    );
```

# Crear la tabla *frigorificos*

```sql
    create table frigorificos (
		foreign key (idtienda) references tienda(id),
        codigo int not null,
        marca char(20) not null,
        modelo char(20) not null,
        color char(20) not null,
        capacidad int not null,
        sistema char(20) not null,
        altura int not null,
        precio int not null,
        idtienda int not null,
        primary key(codigo)
);
```

# Añadimos valores a la tabla *tienda*

```sql
INSERT INTO tienda (
    id, 
    nombre,
    ciudad,
    numerotrabajadores,
    superficie
)
VALUES 
    (1, 'electrosol', 'madrid', 5, 1250),
    (2, 'totalfrigo', 'madrid', 8, 1750),
    (3, 'barnaelectric', 'barcelona', 10, 2000),
    (4, 'frigodiaz', 'barcelona', 5, 1000),
    (5, 'frigoelectric', 'barcelona', 15, 3000);
```

# Añadimos valores a la tabla *frigorificos*

```sql
insert into frigorificos (
    codigo, 
    marca,    
    modelo, 
    color, 
    capacidad, 
    sistema, 
    altura, 
    precio, 
    idtienda
)
values 
    (1, 'haier', 'HTR3619ENMN', 'inox', 348, 'nofrost', 190, 619, 1),
    (2, 'balay', 'LRB3DE18S', 'blanco', 311, 'estatico', 178, 1010, 2),
    (3, 'balay', 'RS650N4AC1', 'inox', 500, 'no frost', 110, 179, 2),
    (4, 'balay', 'JF-90', 'inox', 90, 'estatico', 75, 139, 3),
    (5, 'aeg', 'RB34A7', 'blanco', 344, 'no frost', 185, 949, 4),
    (6, 'haier', 'OFX211', 'negro', 80, 'estatico', 80, 129, 1),
    (7, 'aeg', 'RCB632E5MX', 'blanco', 290, 'no frost', 186, 799, 2),
    (8, 'balay', '3FAF494XE', 'inox', 533, 'no frost', 179, 1499, 1);
```

# Selecciona el nombre de la tienda y la superficie y solo coje las tienda que sean menores de 1500 y los ordena de manera ascendente por el nombre.

```sql
select nombre, superficie from tienda

where superficie <= 1500

order by nombre asc;
```

# consulta que muestre el nombre de la tienda la marca y el modelo y el precio del frigorifico.

```sql
select idtienda, modelo, precio from frigorificos

inner join tienda

on frigorificos.idtienda = tienda.nombre

order by idtienda asc;
```

# consulta que muestre la marca, le modelo de los frigoríficos que sean blancos y su capacidad sea superior a 300.

```sql
select marca, modelo, color, capacidad from frigorificos

where color = 'blanco' and capacidad >= 300;
```

# Tiendas de menos de 1500 metros, ordenadas por nombre decreciente.

```sql
SELECT nombre, superficie

FROM tienda

WHERE superficie < 1500

ORDER BY nombre DESC;
```



# Frigoríficos blancos con capacidad superior a 300.

```sql
SELECT marca, modelo

FROM frigorificos

WHERE color = 'blanco' AND capacidad > 300;
```

# Nombre de tienda, marca, modelo y precio del frigorífico.

```sql
SELECT nombre AS NombreTienda, marca, modelo, precio

FROM tienda

JOIN frigorificos ON id = idtienda;
```

# Cantidad de frigoríficos por marca.

```sql
SELECT marca, COUNT(*) AS cantidad

FROM frigorificos

GROUP BY marca;
```

# Importe total de frigoríficos por tienda.

```sql
SELECT nombre, SUM(precio) AS ImporteTotal

FROM tienda

JOIN frigorificos ON id = idtienda

GROUP BY id, nombre;


```

# Información de frigoríficos incluyendo los que no están en tiendas.

```sql
SELECT marca, modelo, precio, capacidad, nombre AS tienda

FROM frigorificos

LEFT JOIN tienda ON idtienda = id;
```

# Media de precio de frigoríficos Haier.

```sql
SELECT AVG(precio) AS PrecioMedio

FROM frigorificos

WHERE marca = 'haier';
```

# Media de capacidad y precio de frigoríficos con altura > 180.

```sql
SELECT AVG(capacidad) AS capacidad_media, AVG(precio) AS precio_medio

FROM frigorificos

WHERE altura > 180;
```

# Tiendas con suma de precios de frigoríficos > 1500€.

```sql
SELECT t.nombre, SUM(f.precio) AS suma_precios

FROM tienda t

JOIN frigorificos f ON t.id = f.idtienda

GROUP BY t.id, t.nombre

HAVING suma_precios > 1500;
```

# Frigoríficos que no están en ninguna tienda.

```sql
SELECT marca, modelo

FROM frigorificos

WHERE idtienda IS NULL;
```

# Frigoríficos con altura > 170 y precio < 800€, mostrando descuento.

```sql
SELECT marca, modelo, precio, precio * 0.1 AS descuento

FROM frigorificos

WHERE altura > 170 AND precio < 800;
```

# Frigorífico de mayor capacidad.

```sql
SELECT marca, modelo

FROM frigorificos

WHERE capacidad = (SELECT MAX(capacidad) FROM frigorificos);
```

# Frigoríficos con capacidad superior al de mayor capacidad de AEG.

```sql
SELECT marca, modelo

FROM frigorificos

WHERE capacidad > (

    SELECT MAX(capacidad)

    FROM frigorificos

    WHERE marca = 'aeg'

);
```

# Precio del frigorífico con más capacidad que no es blanco ni AEG.

```sql
SELECT precio

FROM frigorificos

WHERE color != 'blanco' AND marca != 'aeg'

ORDER BY capacidad DESC

LIMIT 1;
```

# Frigoríficos blancos en tiendas que no son de Barcelona.

```sql
SELECT *

FROM frigorificos

JOIN tienda ON idtienda = id

WHERE color = 'blanco' AND ciudad != 'barcelona';
```






