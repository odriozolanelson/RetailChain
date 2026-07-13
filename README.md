# RetailChain – UNION y UNION ALL

**Autor:** Nelson Odriozola  
**Fecha:** 13/07/2026

---

## ¿Cuántas filas devuelve cada consulta y por qué son distintas?

La **Consulta 1** devuelve **11 filas**.

```sql
SELECT COUNT(*) AS filas_union
FROM (
    SELECT id_producto, nombre_producto, categoria
    FROM inventario_sucursal_norte

    UNION

    SELECT id_producto, nombre_producto, categoria
    FROM inventario_sucursal_sur
) AS resultado_union;
```

La **Consulta 2** devuelve **14 filas**.

```sql
SELECT COUNT(*) AS filas_union_all
FROM (
    SELECT *
    FROM inventario_sucursal_norte

    UNION ALL

    SELECT *
    FROM inventario_sucursal_sur
) AS resultado_union_all;
```

La diferencia se debe a que `UNION` elimina automáticamente las filas duplicadas, mientras que `UNION ALL` conserva todos los registros.

En este caso, `UNION` eliminó tres filas duplicadas correspondientes a los productos con **id_producto 103, 104 y 106**, ya que esos registros estaban presentes en ambas tablas.

---

## ¿Por qué `UNION ALL` es más eficiente que `UNION`?

`UNION ALL` es más eficiente porque simplemente concatena los resultados de ambas consultas.

En cambio, `UNION` debe realizar un procesamiento adicional para identificar y eliminar los registros duplicados. Para ello, SQL Server compara las filas devueltas por ambas consultas, lo que implica un mayor consumo de tiempo y recursos, especialmente cuando se trabaja con grandes volúmenes de datos.

---

## ¿En qué casos de negocio usarías cada uno?

### Casos de uso de `UNION`

- Obtener un listado único de clientes que realizaron compras en distintas sucursales.
- Generar un listado sin duplicados de productos comercializados por diferentes depósitos.

### Casos de uso de `UNION ALL`

- Consolidar todas las ventas de distintas sucursales para calcular el total vendido en un período, conservando cada transacción.
- Unificar los registros de ventas realizadas por distintos vendedores para obtener la cantidad total de operaciones efectuadas.

---

## ¿Qué ocurre si las consultas no tienen la misma cantidad o el mismo tipo de columnas?

Las consultas combinadas mediante `UNION` o `UNION ALL` deben devolver la misma cantidad de columnas y estas deben ser compatibles entre sí.

Si la cantidad de columnas es diferente, SQL Server genera un error indicando que todas las consultas combinadas deben tener el mismo número de expresiones en la lista de selección.

Si los tipos de datos no son compatibles, SQL Server intentará realizar una conversión implícita. Si dicha conversión no es posible, también se producirá un error.
