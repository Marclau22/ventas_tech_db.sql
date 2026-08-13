-- ============================================================
-- Consulta 1 — Resumen ejecutivo mensual
-- Total facturado, cantidad de pedidos y ticket promedio,
-- agrupados por mes.
-- ============================================================
SELECT
    EXTRACT(MONTH FROM fecha_venta)                AS mes,
    SUM(cantidad * precio_unitario)                AS total_facturado,
    COUNT(*)                                       AS cantidad_pedidos,
    ROUND(AVG(cantidad * precio_unitario), 2)      AS ticket_promedio
FROM ventas
GROUP BY EXTRACT(MONTH FROM fecha_venta)
ORDER BY mes;


-- ============================================================
-- Consulta 2 — Ranking de productos
-- Top 5 de id_producto por total facturado, mostrando también
-- las unidades vendidas.
-- ============================================================
SELECT
    id_producto,
    SUM(cantidad)                      AS unidades_vendidas,
    SUM(cantidad * precio_unitario)    AS total_facturado
FROM ventas
GROUP BY id_producto
ORDER BY total_facturado DESC
LIMIT 5;


-- ============================================================
-- Consulta 3 — Clientes recurrentes
-- id_cliente que realizaron más de un pedido, mostrando
-- cantidad de pedidos y total gastado.
-- ============================================================
SELECT
    id_cliente,
    COUNT(*)                           AS cantidad_pedidos,
    SUM(cantidad * precio_unitario)    AS total_gastado
FROM ventas
GROUP BY id_cliente
HAVING COUNT(*) > 1
ORDER BY total_gastado DESC;


-- ============================================================
-- Consulta 4 — Meses por encima/por debajo del promedio
-- Total facturado por mes, etiquetando cada mes según si
-- quedó por encima o por debajo del promedio mensual general.
-- ============================================================
WITH ventas_mes AS (
    SELECT
        EXTRACT(MONTH FROM fecha_venta)     AS mes,
        SUM(cantidad * precio_unitario)     AS total_facturado
    FROM ventas
    GROUP BY EXTRACT(MONTH FROM fecha_venta)
),
promedio_general AS (
    SELECT AVG(total_facturado) AS promedio
    FROM ventas_mes
)
SELECT
    vm.mes,
    vm.total_facturado,
    ROUND(pg.promedio, 2) AS promedio_general,
    CASE
        WHEN vm.total_facturado >= pg.promedio THEN 'Por encima'
        ELSE 'Por debajo'
    END AS comparacion_promedio
FROM ventas_mes vm
CROSS JOIN promedio_general pg
ORDER BY vm.mes;


-- ============================================================
-- Hallazgos
-- ============================================================
-- 1. El producto 1 (id_producto = 1) concentra el 55.9% de la
--    facturación total ($3.600 de $6.444), muy por encima del
--    resto: es más del doble que el segundo producto (id 3).
--
-- 2. Los 5 clientes de la base son "recurrentes" según el criterio
--    de la Consulta 3 (más de un pedido): cada uno realizó
--    exactamente 2 compras. El cliente 1 es el de mayor gasto
--    acumulado ($2.640), seguido de cerca por el cliente 5 ($2.100).
--
-- 3. Todas las ventas cargadas corresponden a un único mes
--    (marzo 2024), por lo que la Consulta 4 no tiene todavía
--    mes con el que comparar: con un solo período, "el promedio"
--    coincide con el propio total. Este análisis va a volverse
--    realmente útil recién cuando se carguen datos de varios
--    meses en los próximos módulos.
