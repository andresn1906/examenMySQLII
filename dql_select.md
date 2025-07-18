## 3.- Consultas:

1.- Encuentra el cliente que ha realizado la mayor cantidad de alquileres en los últimos 6 meses.
```sql
SELECT cl.nombre AS Cliente,
COUNT(al.id_alquiler) AS CantidadAlquileres
FROM cliente cl
JOIN alquiler al ON al.id_alquiler = cl.id_cliente
WHERE al.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH)
GROUP BY cl.id_cliente
ORDER BY CantidadAlquileres DESC
LIMIT 1;
```
2.- Lista las cinco películas más alquiladas durante el último año.
```sql
SELECT pe.titulo AS Pelicula
COUNT(al.id_alquiler) AS VecesAlquilada
FROM alquiler al
JOIN inventario inv ON inv.id_inventario = al.id_inventario
JOIN pelicula pe ON pe.id_pelicula = inv.id_pelicula
WHERE al.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY pe.id_pelicula DESC
ORDER BY VecesAlquilada
LIMIT 5;
```
3-. Obtén el total de ingresos y la cantidad de alquileres realizados por cada categoría de película.
```sql
SELECT cat.nombre AS Categoría,
COUNT(al.id_alquiler) AS TotalAlquileres,
SUM(pag.total) AS TotalIngresos
FROM alquiler al
JOIN pago pag ON al.id_alquiler = pag.id_alquiler
JOIN inventario i ON al.id_inventario = i.id_inventario
JOIN pelicula pe ON i.id_pelicula = pe.id_pelicula
JOIN pelicula_categoria pc ON pe.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
GROUP BY cat.id_categoria; 
```

4.- Calcula el número total de clientes que han realizado alquileres por cada idioma disponible en un mes específico.
```sql
SELECT COUNT(DISTINCT cl.id_cliente) AS CantidadClientes
i.nombre AS Idioma,
FROM cliente cl
JOIN alquiler a ON cl.id_cliente = a.id_cliente
JOIN inventario inv ON a.id_inventario = inv.id_inventario
JOIN pelicula pe ON inv.id_pelicula = pe.id_pelicula
JOIN idioma i ON p.id_idioma = i.id_idioma
WHERE MONTH(a.fecha_alquiler) = 5 AND YEAR(a.fecha_alquiler) = 2005
GROUP BY i.id_idioma;
```

5.- Encuentra a los clientes que han alquilado todas las películas de una misma categoría.
```sql
SELECT cl.nombre AS Cliente, 
cat.nombre AS Categoria
FROM cliente cl
JOIN alquiler al ON cl.id_cliente = al.id_cliente
JOIN inventario inv ON al.id_inventario = inv.id_inventario
JOIN pelicula pe ON inv.id_pelicula = pe.id_pelicula
JOIN pelicula_categoria pc ON pe.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
GROUP BY cl.id_cliente, pc.id_categoria
HAVING COUNT(DISTINCT pe.id_pelicula) = (
    SELECT COUNT(*) 
    FROM pelicula_categoria 
    WHERE id_categoria = pc.id_categoria
);
```

6.- Lista las tres ciudades con más clientes activos en el último trimestre.
```sql
SELECT c.nombre AS Ciudad, 
COUNT(DISTINCT cl.id_cliente) AS CantidadClientes
FROM cliente cl
JOIN direccion d ON cl.id_direccion = d.id_direccion
JOIN ciudad c ON d.id_ciudad = c.id_ciudad
JOIN alquiler al ON cl.id_cliente = al.id_cliente
WHERE al.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
GROUP BY c.id_ciudad
ORDER BY CantidadClientes DESC
LIMIT 3;
```

7.- Muestra las cinco categorías con menos alquileres registrados en el último año.
```sql
SELECT cat.nombre AS Categoría,
COUNT(al.id_alquiler) AS CantidadAlquileres
FROM alquiler al
JOIN inventario inv ON al.id_inventario = inv.id_inventario
JOIN pelicula pe ON inv.id_pelicula = pe.id_pelicula
JOIN pelicula_categoria pc ON pe.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
WHERE al.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY cat.id_categoria
ORDER BY CantidadAlquileres ASC
LIMIT 5;
```

8.- Calcula el promedio de días que un cliente tarda en devolver las películas alquiladas.
```sql
SELECT AVG(DATEDIFF(al.fecha_devolucion, al.fecha_alquiler)) AS 'Promedio de días'
FROM alquiler al
WHERE fecha_devolucion IS NOT NULL;
```

9.- Encuentra los cinco empleados que gestionaron más alquileres en la categoría de Acción.
```sql
SELECT em.nombre As Empleado
COUNT(al.id_alquiler) AS CantidadAlquileres
FROM alquiler al
JOIN empleado em ON al.id_empleado = em.id_empleado
JOIN inventario inv ON al.id_inventario = inv.id_inventario
JOIN pelicula pe ON inv.id_pelicula = pe.id_pelicula
JOIN pelicula_categoria pc ON pe.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
WHERE cat.nombre = 'Acción'
GROUP BY em.id_empleado
ORDER BY CantidadAlquileres DESC
LIMIT 5;
```

10.- Genera un informe de los clientes con alquileres más recurrentes.
```sql

```

11.- Calcula el costo promedio de alquiler por idioma de las películas.
```sql

```

12.- Lista las cinco películas con mayor duración alquiladas en el último año.
```sql

```

13.- Muestra los clientes que más alquilaron películas de Comedia.
```sql
```

14.- Encuentra la cantidad total de días alquilados por cada cliente en el último mes.
```sql
```

15.- Muestra el número de alquileres diarios en cada almacén durante el último trimestre.
```sql
```

16.- Calcula los ingresos totales generados por cada almacén en el último semestre.
```sql
```

17.- Encuentra el cliente que ha realizado el alquiler más caro en el último año.
```sql
SELECT cl.nombre AS Cliente,
pag.total AS PagoTotal
FROM pago pag
JOIN cliente cl ON pag.id_cliente = cl.id_cliente
WHERE pag.fecha_pago >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
LIMIT 1;
```

18.- Lista las cinco categorías con más ingresos generados durante los últimos tres meses.
```sql
```

19.- Obtén la cantidad de películas alquiladas por cada idioma en el último mes.
```sql
```

20.- Lista los clientes que no han realizado ningún alquiler en el último año.
```sql
```