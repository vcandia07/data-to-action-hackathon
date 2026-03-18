# 🟡 Desafío 4 – Transformación de datos: Bronze → Silver → Gold

## 🎯 Objetivo

Implementar un proceso de **limpieza, estandarización y agregación de datos** utilizando un **Notebook de PySpark** en Microsoft Fabric, moviendo los datos a través de las capas **Bronze → Silver → Gold** de la arquitectura Medallion.

---

## 🧩 Contexto

Con los datos ya disponibles en la capa **Bronze** (como Delta Tables cargadas desde Azure SQL Database), el siguiente paso es aplicar transformaciones progresivas que eleven la calidad y utilidad de los datos.

Cada capa tiene un propósito bien definido:

| Capa   | Propósito                                                                 |
|--------|---------------------------------------------------------------------------|
| Bronze | Datos crudos, sin modificaciones, tal como llegan desde la fuente        |
| Silver | Datos limpios, estandarizados y validados, listos para análisis           |
| Gold   | Datos agregados y modelados, optimizados para consumo analítico           |

Microsoft Fabric permite ejecutar **Notebooks de PySpark** directamente sobre los Lakehouses, accediendo a las Delta Tables de Bronze y escribiendo los resultados en Silver y Gold.

---

## 📋 Desafío

Crear y ejecutar un **Notebook de PySpark** en Microsoft Fabric que implemente el flujo completo de transformación:

**Bronze → Silver:**
- Leer las Delta Tables del **Lakehouse Bronze**.
- Aplicar limpieza de datos: eliminar duplicados, tratar valores nulos y corregir tipos de datos.
- Estandarizar formatos (fechas, textos, códigos).
- Escribir los datos transformados como **Delta Tables en el Lakehouse Silver**.

**Silver → Gold:**
- Leer las Delta Tables del **Lakehouse Silver**.
- Aplicar agregaciones y enriquecimiento de datos según los requerimientos analíticos.
- Escribir los datos finales como **Delta Tables en el Lakehouse Gold**.

---

## 🔧 Transformaciones a implementar

### Bronze → Silver

**`sqls_salesorderheader` → `salesorderheader`**
1. Leer tabla `sqls_salesorderheader` desde Lakehouse Bronze
2. Eliminar duplicados por `SalesOrderID`
3. Tratar valores nulos en columnas opcionales
4. Convertir columnas de fecha al tipo `Date`
5. Estandarizar el campo `Status` a texto legible (Enviado, En proceso, Otro)
6. Agregar columna de auditoría `_silver_ingestion_ts`
7. Escribir resultado como `salesorderheader` en Lakehouse Silver

**`sqls_salesorderdetail` → `salesorderdetail`**
1. Leer tabla `sqls_salesorderdetail` desde Lakehouse Bronze
2. Eliminar duplicados por `SalesOrderDetailID`
3. Validar y corregir cantidades negativas o nulas
4. Escribir resultado como `salesorderdetail` en Lakehouse Silver

**`sqls_customer` → `customer`**
1. Leer tabla `sqls_customer` desde Lakehouse Bronze
2. Tratar valores nulos en campos de email y empresa
3. Estandarizar formato de nombres (mayúsculas/minúsculas)
4. Escribir resultado como `customer` en Lakehouse Silver

**`sqls_product` → `product`**
1. Leer tabla `sqls_product` desde Lakehouse Bronze
2. Estandarizar nombres de producto
3. Tratar valores nulos en campos opcionales (color, talla, etc.)
4. Escribir resultado como `product` en Lakehouse Silver

---

### Silver → Gold

1. Leer tablas `salesorderheader`, `salesorderdetail`, `customer` y `product` desde Lakehouse Silver
2. Construir `fact_ventas`: join entre `salesorderheader` y `salesorderdetail`, seleccionando columnas clave y métricas
3. Construir `dim_cliente`: selección y renombre de atributos descriptivos del cliente
4. Construir `dim_producto`: selección y renombre de atributos descriptivos del producto
5. Construir `dim_fecha`: generación de tabla de fechas con jerarquía (año, mes, trimestre, semana)
6. Escribir `fact_ventas`, `dim_cliente`, `dim_producto` y `dim_fecha` en Lakehouse Gold

---

## 🧠 Consideraciones

- En Fabric, un Notebook puede acceder a múltiples Lakehouses agregándolos como **recursos adicionales** del Notebook.
- Usar la API de **Delta Lake** (`spark.read.format("delta")`) o directamente los accesos por nombre de tabla del Lakehouse.
- Las escrituras a Silver y Gold deben usar el modo `overwrite` o `merge` (upsert) según la estrategia de actualización elegida.
- Documentar cada sección del Notebook con celdas Markdown que expliquen la transformación aplicada.
- Mantener el principio de **no modificar Bronze**: todas las transformaciones generan nuevas tablas en Silver o Gold.

---

## 🏗 Arquitectura esperada

```
Lakehouse_Bronze
└── Tables/
    └── sqls_nombre_tabla  (Delta Table - sin modificar)
        │
        │  (PySpark Notebook – limpieza y estandarización)
        ▼
Lakehouse_Silver
└── Tables/
    └── nombre_tabla  (Delta Table - datos limpios)
        │
        │  (PySpark Notebook – agregaciones y modelado)
        ▼
Lakehouse_Gold
└── Tables/
    └── nombre_tabla_agg  (Delta Table - datos listos para análisis)
```

---

## ✅ Resultado esperado

Al finalizar el desafío, el entorno debería contar con:

- Un **Notebook de PySpark** que implemente el flujo completo Bronze → Silver → Gold.
- Delta Tables limpias y estandarizadas en el **Lakehouse Silver**.
- Delta Tables agregadas y optimizadas en el **Lakehouse Gold**.
- Los datos del **Lakehouse Gold** disponibles para consumo analítico en los siguientes desafíos.

---

## 📎 Recurso de apoyo

Se incluye el notebook de referencia [`notebook-transformacion.ipynb`](./notebook-transformacion.ipynb) con la estructura base y ejemplos de transformación PySpark para guiar la implementación.

---

## 📚 Recursos recomendados

- [Notebooks en Microsoft Fabric](https://learn.microsoft.com/es-es/fabric/data-engineering/how-to-use-notebook)
- [Trabajar con Delta Tables en Fabric](https://learn.microsoft.com/es-es/fabric/data-engineering/lakehouse-and-delta-tables)
- [PySpark – Funciones de transformación](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html)
- [Agregar Lakehouses a un Notebook en Fabric](https://learn.microsoft.com/es-es/fabric/data-engineering/notebook-multiple-lakehouses)

---

## 🚀 Próximo desafío

En el siguiente desafío se conectará el **Lakehouse Gold** a **Power BI** para construir reportes y visualizaciones sobre los datos transformados.
