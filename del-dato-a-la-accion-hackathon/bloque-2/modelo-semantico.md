# 🟣 Desafío 5 – Mi primer Modelo Semántico

## 🎯 Objetivo

Crear un **Modelo Semántico** en Microsoft Fabric a partir de las Delta Tables de la capa **Gold**, definiendo relaciones entre tablas, medidas DAX y una estructura optimizada para el consumo analítico en Power BI.

---

## 🧩 Contexto

Con los datos ya transformados y almacenados en el **Lakehouse Gold**, el siguiente paso natural en la arquitectura analítica es construir una **capa semántica** que abstraiga la complejidad del modelo físico de datos y exponga la información de forma amigable para los usuarios de negocio.

En Microsoft Fabric, el **Modelo Semántico** (anteriormente conocido como *Power BI Dataset*) cumple exactamente ese rol:

- Define las **relaciones** entre tablas del modelo Gold.
- Centraliza la lógica de negocio en **medidas DAX** reutilizables.
- Sirve como fuente única de verdad para todos los reportes y dashboards de Power BI conectados al workspace.
- Permite que usuarios de negocio exploren los datos sin necesidad de conocer SQL o PySpark.

Un modelo semántico bien diseñado sigue el esquema en **estrella (Star Schema)**: una o más **tablas de hechos** (fact tables) rodeadas de **tablas de dimensiones** (dimension tables).

| Tipo de tabla   | Propósito                                                              |
|-----------------|------------------------------------------------------------------------|
| Tabla de hechos | Contiene métricas y eventos cuantitativos (ventas, transacciones, etc.)|
| Tabla de dimensión | Contiene atributos descriptivos (clientes, productos, fechas, etc.) |

---

## 📋 Desafío

Crear y configurar un **Modelo Semántico** en Microsoft Fabric siguiendo estos pasos:

**Creación del modelo:**
- Desde el Lakehouse Gold, seleccionar las Delta Tables que formarán el modelo.
- Generar el Modelo Semántico automáticamente o crearlo desde cero en el workspace.

**Definición de relaciones:**
- Identificar las claves primarias y foráneas entre tablas.
- Crear las relaciones en el editor del modelo semántico (tipo: muchos a uno, dirección del filtro).
- Verificar la integridad referencial del esquema en estrella.

**Creación de medidas DAX:**
- Definir al menos **3 medidas DAX** relevantes para el negocio.  
  Ejemplos:
  - `Total Ventas = SUM(fact_ventas[monto])`
  - `Ticket Promedio = DIVIDE([Total Ventas], COUNTROWS(fact_ventas))`
  - `Ventas YTD = TOTALYTD([Total Ventas], dim_fecha[fecha])`
- Organizar las medidas en una **tabla de medidas** dedicada.

**Validación:**
- Crear un reporte de prueba rápido en Power BI conectado al modelo semántico.
- Verificar que los filtros cruzados entre dimensiones y hechos funcionan correctamente.

---

## 🗂 Modelo a construir

El modelo sigue un **esquema en estrella** con una tabla de hechos central y tres dimensiones:

```
                    ┌─────────────────────┐
                    │     dim_fecha        │
                    │─────────────────────│
                    │ 🔑 fecha            │
                    │    anio             │
                    │    mes              │
                    │    nombre_mes       │
                    │    trimestre        │
                    │    semana_anio      │
                    └──────────┬──────────┘
                               │ 1
                               │
┌─────────────────────┐        │        ┌─────────────────────┐
│     dim_cliente      │        │        │     dim_producto     │
│─────────────────────│        │        │─────────────────────│
│ 🔑 id_cliente       │        │        │ 🔑 id_producto      │
│    nombre_cliente   │        │        │    nombre_producto  │
│    email            │        │        │    codigo_producto  │
│    empresa          │        │        │    costo_estandar   │
└──────────┬──────────┘        │        │    precio_lista     │
           │ 1                 │        │    color            │
           │                   │        └──────────┬──────────┘
           │          ┌────────▼────────┐          │ 1
           │          │   fact_ventas   │          │
           │          │────────────────│          │
           └─────── * │ id_orden       │ * ────────┘
                      │ id_detalle     │
                      │ 🔗 id_cliente  │
                      │ 🔗 id_producto │
                      │ 🔗 fecha_orden │
                      │ cantidad       │
                      │ precio_unit.   │
                      │ monto_total    │
                      └────────────────┘
```

### Relaciones definidas

| Desde (fact_ventas) | Hacia (dimensión) | Tipo | Dirección del filtro |
|---------------------|-------------------|------|----------------------|
| `fecha_orden` | `dim_fecha[fecha]` | Muchos a uno (*:1) | Único (dim → fact) |
| `id_cliente` | `dim_cliente[id_cliente]` | Muchos a uno (*:1) | Único (dim → fact) |
| `id_producto` | `dim_producto[id_producto]` | Muchos a uno (*:1) | Único (dim → fact) |

### Medidas DAX a crear

| Medida | Descripción | Formato |
|--------|-------------|---------|
| `Total Ventas` | Suma el monto total de todas las líneas de venta del contexto de filtro activo | Moneda, 2 decimales |
| `Ticket Promedio` | Divide el Total Ventas entre el número de líneas de detalle, devolviendo 0 si no hay datos | Moneda, 2 decimales |
| `Ventas YTD` | Acumula el Total Ventas desde el inicio del año hasta la fecha seleccionada, usando la columna de fecha de `dim_fecha` | Moneda, 2 decimales |
| `Total Ordenes` | Cuenta la cantidad de órdenes únicas (sin repetición) presentes en el contexto de filtro | Número entero |

---

## 🧠 Consideraciones

- El Modelo Semántico en Fabric puede generarse **directamente desde el Lakehouse** usando la opción *"New semantic model"* sobre las tablas Gold.
- Utilizar el modo de almacenamiento **DirectLake** para aprovechar el acceso directo a los archivos Delta del Lakehouse sin necesidad de importar los datos.
- En **DirectLake**, los datos se leen directamente desde OneLake, combinando el rendimiento de los modelos importados con la frescura de los datos en tiempo real.
- Las medidas DAX deben crearse en el modelo semántico, **no como columnas calculadas**, para preservar el rendimiento de DirectLake.
- Asignar nombres de columnas y tablas **amigables para el usuario final** (sin prefijos técnicos ni guiones bajos cuando sea posible).
- Documentar brevemente cada medida DAX con una descripción en sus propiedades.

---

## 🏗 Arquitectura esperada

```
Lakehouse_Gold
└── Tables/
    ├── fact_ventas        (Delta Table – tabla de hechos)
    ├── dim_cliente        (Delta Table – dimensión de clientes)
    ├── dim_producto       (Delta Table – dimensión de productos)
    └── dim_fecha          (Delta Table – dimensión de tiempo)
         │
         │  (Modelo Semántico – DirectLake)
         ▼
Modelo_Semantico_Gold
├── Relaciones definidas   (Star Schema)
├── Medidas DAX            (lógica de negocio centralizada)
└── Power BI Report        (consumo analítico)
```

---

## ✅ Resultado esperado

Al finalizar el desafío, el entorno debería contar con:

- Un **Modelo Semántico** publicado en el workspace de Fabric, conectado a las tablas Gold mediante **DirectLake**.
- Relaciones configuradas correctamente entre las tablas del modelo en un **esquema en estrella**.
- Al menos **3 medidas DAX** que encapsulen lógica de negocio relevante.
- Un **reporte de validación** en Power BI que confirme que el modelo responde correctamente a los filtros y agregaciones.
