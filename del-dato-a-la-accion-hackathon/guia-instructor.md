# 📘 Guía del Instructor – Del Dato a la Acción: Hackathon de Microsoft Fabric

> Esta guía contiene el paso a paso completo para que el instructor pueda completar, demostrar y guiar cada uno de los desafíos del hackathon. Incluye capturas de pantalla sugeridas, puntos de validación, respuestas a errores comunes y consejos pedagógicos.

---

## 📐 Estructura general del hackathon

| # | Desafío | Bloque | Tiempo estimado |
|---|---------|--------|-----------------|
| 1 | Configuración del entorno en Microsoft Fabric | Bloque 1 | 20 min |
| 2 | Conexión a fuentes de datos externas | Bloque 1 | 25 min |
| 3 | Ingesta de datos hacia Bronze | Bloque 1 | 30 min |
| 4 | Transformación: Bronze → Silver → Gold | Bloque 2 | 45 min |
| 5 | Mi primer Modelo Semántico | Bloque 2 | 30 min |
| 6 | Creando un informe en Power BI | Bloque 3 | 30 min |
| ⭐ | Bonus Track – Data Agent en Fabric | Bonus | 30 min |

---

## 🔑 Pre-requisitos del instructor

Antes de comenzar el hackathon, verificar que se cumplan **todos** los siguientes requisitos:

### Accesos y licencias
- [ ] Tenant de Microsoft 365 con **capacidad de Fabric habilitada** (F64 o superior, o capacidad de prueba).
- [ ] Licencia **Power BI Pro** o **Premium Per User (PPU)** para cada participante.
- [ ] Acceso a una **Azure SQL Database** con datos de muestra (AdventureWorks o dataset propio).
- [ ] Acceso a una cuenta de **Azure Data Lake Storage Gen2 (ADLS Gen2)** con archivos CSV/Parquet disponibles.
- [ ] Permisos de **Administrador del Workspace** en Fabric para el instructor.
- [ ] Para el Bonus Track: Fabric Copilot habilitado en el tenant (verificar con el administrador del tenant).

### Datos de muestra recomendados
- **Azure SQL Database**: tablas `SalesOrderHeader`, `SalesOrderDetail`, `Customer`, `Product`, `ProductCategory` del esquema `SalesLT` de AdventureWorks.
- **ADLS Gen2**: archivos CSV con datos complementarios (por ejemplo, datos de regiones, presupuestos o devoluciones).

### Información de conexión a tener preparada
```
Azure SQL Server:    <servidor>.database.windows.net
Base de datos:       AdventureWorksLT (o la elegida)
Usuario:             <usuario_sql>
Contraseña:          <contraseña>

ADLS Gen2:
Cuenta de almacenamiento: <nombre_cuenta>
Contenedor:               <nombre_contenedor>
Tipo de autenticación:    Account Key o Service Principal
```

---

## 🟤 Desafío 1 – Configuración del entorno en Microsoft Fabric

### Objetivo pedagógico
Familiarizar a los participantes con la interfaz de Microsoft Fabric y establecer la arquitectura base Medallion (Bronze, Silver, Gold) sobre la que trabajarán durante todo el hackathon.

---

### Paso a paso detallado

#### 1.1 – Acceder a Microsoft Fabric

1. Abrir el navegador y dirigirse a [https://app.fabric.microsoft.com](https://app.fabric.microsoft.com).
2. Iniciar sesión con la cuenta organizacional que tenga habilitada la capacidad de Fabric.
3. Una vez dentro, verificar que en la esquina inferior izquierda aparezca el selector de experiencia. Hacer clic en él y seleccionar **"Fabric"** como experiencia principal.


---

#### 1.2 – Crear el Workspace

1. En el panel lateral izquierdo, hacer clic en **"Workspaces"** (ícono de múltiples cuadros).
2. Hacer clic en el botón **"+ New workspace"** en la parte inferior del panel.
3. Completar el formulario:
   - **Name**: `Hackathon-<tu-nombre>-<nombre-empresa>` o el nombre acordado con el grupo (ej. `Hackathon-PaoloCaviedes-GOVms`).
   - **Description**: Opcional. Ej: *"Workspace del hackathon Del Dato a la Acción"*.
4. Expandir la sección **"Advanced"**:
   - En **"License mode"**, seleccionar **"Fabric capacity"** y elegir la capacidad disponible del tenant.
   - Si se usa capacidad de prueba, seleccionar **"Trial"**.
5. Hacer clic en **"Apply"**.
6. El workspace se creará y se abrirá automáticamente. Verificar que aparece vacío y con el nombre correcto.
**Nota: El paso 4 es muy importante**

> ✅ **Punto de validación**: El workspace aparece en el panel lateral y aparece con icono de *diamente* a la izquierda del nombre.

---

#### 1.3 – Crear el Lakehouse Bronze

1. Dentro del workspace recién creado, hacer clic en el botón **"+ New item"** (parte superior izquierda).
2. En el panel de creación de ítems, buscar o desplazarse hasta encontrar **"Lakehouse"**.
3. Hacer clic en **"Lakehouse"**.
4. En el cuadro de diálogo:
   - **Name**: `Lakehouse_Bronze`
5. Hacer clic en **"Create"**.
6. Esperar a que se cree el Lakehouse (puede tomar 15-30 segundos). Se abrirá automáticamente la interfaz del Lakehouse.
7. Navegar de vuelta al workspace usando las migas de pan (breadcrumb) en la parte superior.
**Nota: Dejar deshabilitada la opcion "Lakehouse Schemas"**
---

#### 1.4 – Crear el Lakehouse Silver

1. De vuelta en el workspace, hacer clic en **"+ New item"** nuevamente.
2. Seleccionar **"Lakehouse"**.
3. **Name**: `Lakehouse_Silver`
4. Hacer clic en **"Create"** y esperar a que se cree.
5. Navegar de vuelta al workspace.
**Nota: Dejar deshabilitada la opcion "Lakehouse Schemas"**

---

#### 1.5 – Crear el Lakehouse Gold

1. De vuelta en el workspace, hacer clic en **"+ New item"** nuevamente.
2. Seleccionar **"Lakehouse"**.
3. **Name**: `Lakehouse_Gold`
4. Hacer clic en **"Create"** y esperar a que se cree.
5. Navegar de vuelta al workspace.
**Nota: Dejar deshabilitada la opcion "Lakehouse Schemas"**

---

#### 1.6 – Verificar la arquitectura creada

1. En la vista principal del workspace, verificar que existen los siguientes ítems:
   - `Lakehouse_Bronze` (tipo: Lakehouse)
   - `Lakehouse_Silver` (tipo: Lakehouse)
   - `Lakehouse_Gold` (tipo: Lakehouse)
2. Notar que junto a cada Lakehouse se crean automáticamente un **SQL Endpoint** y un **Default Semantic Model**. Estos son artefactos asociados al Lakehouse y se usarán en desafíos posteriores.

> 💡 **Explicación al grupo**: Cada Lakehouse tiene dos secciones internas:
> - `Tables/` → almacena Delta Tables consultables por SQL.
> - `Files/` → almacena archivos en formato abierto (CSV, Parquet, JSON, etc.).

> ✅ **Punto de validación final del desafío**: El workspace contiene 3 Lakehouses (Bronze, Silver y Gold) correctamente creados y visibles en el workspace.

---

### Errores comunes y soluciones

| Error | Causa probable | Solución |
|-------|---------------|----------|
| No aparece la opción "Lakehouse" en "+ New item" | La experiencia activa no es "Data Engineering" | Cambiar la experiencia en el selector inferior izquierdo a "Data Engineering" |
| "You don't have permission to create items" | Sin permisos de contribuidor en el workspace | El administrador debe asignar rol "Contributor" o superior |
| El workspace no permite seleccionar capacidad Fabric | Capacidad no asignada al tenant | Verificar con el administrador de Fabric que la capacidad esté activa |

---

## 🔵 Desafío 2 – Conexión a fuentes de datos externas

### Objetivo pedagógico
Enseñar los dos mecanismos principales de integración de datos externos en Fabric: las **Conexiones** para bases de datos relacionales y los **Shortcuts** para Data Lakes.

---

### Paso a paso detallado

#### 2.1 – Crear la conexión a Azure SQL Database


1. En el portal de Microsoft Fabric, hacer clic en el ícono de engranaje (⚙️) en la parte superior derecha → **"Manage connections and gateways"**.
2. En la sección **"Connections"**, hacer clic en **"+ New"**.
3. Seleccionar el tipo de conexión: **"Azure SQL Database"**.
4. Completar los campos de configuración:
   - **Connection name**: `conn-azure-sql-<tu-nombre>-<nombre-empresa>` (nombre descriptivo y reutilizable)
   - **Server**: `Lo entrega el instructor`
   - **Database**: `Lo entrega el instructor`
   - **Authentication kind**: `Basic` (usuario y contraseña)
   - **Username**: `<usuario_sql>`
   - **Password**: `<contraseña>`
5. Hacer clic en **"Create"**.
6. Verificar que la conexión aparece en la lista con estado **"Online"** (puede tardar unos segundos).

> ✅ **Punto de validación**: La conexión aparece en la lista con ícono verde y estado "Online".

---

#### 2.2 – Crear un Shortcut a ADLS Gen2 en el Lakehouse Bronze

1. En el workspace, hacer clic en `Lakehouse_Bronze` para abrirlo.
2. En el panel izquierdo del Lakehouse explorer, localizar la sección **"Files"**.
3. Hacer clic derecho sobre **"Files"** → seleccionar **"New shortcut"**.
   - Alternativa: Hacer clic en los tres puntos (`...`) junto a "Files" → "New shortcut".
4. En el asistente de creación de Shortcuts, seleccionar **"Azure Data Lake Storage Gen2"**.
5. Completar los campos de conexión:
   - **URL**: `https://<nombre_cuenta>.dfs.core.windows.net/`
   - **Connection**: Crear una nueva conexión y darle un nombre descriptivo (`conn-adls-gen2-<tu-nombre>-<nombre-empresa>`).
   - **Authentication kind**: Account Key (o el método disponible).
   - **Account key**: La clave de acceso de la cuenta de ADLS Gen2.
6. Hacer clic en **"Next"**.
7. En la siguiente pantalla, navegar hasta el contenedor destino:
   - Seleccionar el contenedor que contiene los archivos fuente (`raw-data`).
   - Expandir la jerarquía de carpetas si es necesario hasta llegar al nivel deseado.
8. En **"Shortcut Name"**, asignar un nombre descriptivo: `adls_raw_data`.
9. Hacer clic en **"Create"**.
10. El Shortcut aparecerá dentro de `Files/adls_raw_data` en el Lakehouse Bronze.

---

#### 2.3 – Verificar el acceso a los datos de ambas fuentes

**Verificar el Shortcut ADLS Gen2:**
1. En el Lakehouse Bronze, expandir `Files/adls_raw_data`.
2. Verificar que aparecen los archivos del contenedor de ADLS Gen2 (CSV, Parquet, etc.).
3. Hacer clic derecho sobre un archivo CSV → **"Preview"** para visualizar su contenido sin descargarlo.

**Verificar la conexión a Azure SQL:**
1. La verificación plena de la conexión SQL se realizará en el Desafío 3 al ejecutar el pipeline. Sin embargo, se puede intentar una conexión de prueba desde **"Manage connections and gateways"** haciendo clic en los tres puntos de la conexión → **"Test connection"**.

> ✅ **Punto de validación final del desafío**:
> - El Shortcut `adls_raw_data` existe en `Files/` del Lakehouse Bronze y muestra los archivos del contenedor.
> - La conexión a Azure SQL Database aparece con estado "Online" en la gestión de conexiones.

---

### Errores comunes y soluciones

| Error | Causa probable | Solución |
|-------|---------------|----------|
| Error 403 al crear el Shortcut ADLS | Permisos insuficientes en la cuenta de almacenamiento | Asignar rol "Storage Blob Data Reader" o "Storage Blob Data Contributor" al Service Principal o identidad de Fabric en el recurso de Azure Storage |
| "Connection failed" en Azure SQL | Firewall del servidor SQL no permite conexiones desde Fabric | En Azure Portal → SQL Server → Networking → agregar la IP de los servicios de Azure o habilitar "Allow Azure services" |
| El Shortcut aparece pero no muestra archivos | Ruta del contenedor incorrecta | Verificar que el nombre del contenedor y la ruta son correctos en Azure Portal |

---

## 🟠 Desafío 3 – Ingesta de datos hacia Bronze

### Objetivo pedagógico
Implementar el primer movimiento de datos real de la arquitectura: desde Azure SQL Database hacia el Lakehouse Bronze usando un Data Pipeline, y verificar la funcionalidad del Shortcut creado en el desafío anterior.

---

### Paso a paso detallado

#### 3.1 – Crear el Data Pipeline

1. En el workspace, hacer clic en **"+ New item"**.
2. Seleccionar **"Data pipeline"**.
3. **Name**: `pipeline_ingesta_bronze_sql`
4. Hacer clic en **"Create"**. Se abrirá el editor de pipelines de Data Factory.

---

#### 3.2 – Agregar la actividad Copy Data

1. En el editor del pipeline, hacer clic en **"Add pipeline activity"** → seleccionar **"Copy data"**.
   - Alternativa: Arrastrar la actividad "Copy data" desde el panel de actividades en la izquierda.
2. La actividad "Copy data" aparecerá en el canvas. Hacer clic sobre ella para seleccionarla.
3. En el panel inferior de propiedades, aparecerán las pestañas: **General**, **Source**, **Destination**, **Mapping**, **Settings**.

---

#### 3.3 – Configurar el origen (Source)

1. Hacer clic en la pestaña **"Source"**.
2. Expandir la lista **"Connection"**.
3. Seleccionar la conexión creada anteriormente: `conn-azure-sql-<tu-nombre>-<nombre-empresa>`.
4. En **"Table name"**, seleccionar la tabla a ingestar, por ejemplo: `SalesLT.SalesOrderHeader`.
5. En **"Preview data"** verificar que los datos son accesibles y lucen correctos.

> 💡 **Consejo**: Comenzar con una tabla pequeña para validar el flujo completo antes de ingestar tablas grandes.

---

#### 3.4 – Configurar el destino (Destination)

1. Hacer clic en la pestaña **"Destination"**.
2. En **"Connection"**
3. En **"Fabric Item Connection"**.
4. Seleccionar el **Lakehouse_Bronze** del workspace.
5. En **"Root folder"**, seleccionar **"Tables"** (para cargar como Delta Table).
6. En **"Table name"**, selecionar el boton **"+"** Y escribir el nombre de la tabla destino: `sqls_salesorderheader`.
   - Convención de nomenclatura sugerida: prefijo `sqls_` + nombre de tabla en minúsculas sin espacios.

---

#### 3.6 – Replicar para las demás tablas (opcional en demo)

Para el hackathon se recomienda ingestar al menos **3 tablas** de Azure SQL Database:
- `SalesLT.SalesOrderHeader` → `sqls_salesorderheader`
- `SalesLT.SalesOrderDetail` → `sqls_salesorderdetail`
- `SalesLT.Customer` → `sqls_customer`
- `SalesLT.Product` → `sqls_product`

Para agregar más actividades Copy Data:
1. Hacer clic en el canvas vacío para deseleccionar.
2. Agregar nuevas actividades Copy Data como en el paso 3.2.
3. Conectar las actividades en secuencia (arrastrar la flecha verde de éxito de una actividad a la siguiente) o dejarlas en paralelo.

---

#### 3.7 – Ejecutar el pipeline

1. En la barra superior del editor, hacer clic en el botón **"Run"** (ícono de triángulo ▶).
2. Se abrirá una ventana de confirmación. Hacer clic en **"Save and run"** (el pipeline se guardará automáticamente).
3. Observar el panel inferior "Output" donde aparece el estado de ejecución de cada actividad.
4. Esperar a que todas las actividades muestren estado **"Succeeded"** (ícono verde ✅).
5. Hacer clic en el nombre de alguna actividad para ver los detalles: filas copiadas, duración, throughput.

> ✅ **Punto de validación**: Cada actividad Copy Data muestra "Succeeded" y el número de filas copiadas es mayor a 0.

---

#### 3.8 – Verificar los datos en el Lakehouse Bronze

1. Navegar al workspace y hacer clic en `Lakehouse_Bronze`.
2. En el explorador de Lakehouse en la izquierda, expandir la sección **"Tables"**.
3. Verificar que existen las tablas creadas (ej. `sqls_salesorderheader`, `sqls_customer`, etc.).
4. Hacer clic derecho sobre cualquier tabla → **"Preview data"** → verificar que los datos se ven correctamente.
5. Alternativamente, usar el **SQL Endpoint** del Lakehouse:
   - En la esquina superior derecha del Lakehouse, hacer clic en el selector de experiencia y cambiar a **"SQL analytics endpoint"**.
   - Ejecutar una consulta de prueba: `SELECT TOP 10 * FROM sqls_salesorderheader`

**Verificar el Shortcut ADLS Gen2:**
1. En el mismo Lakehouse Bronze, expandir `Files/`.
2. Verificar que el shortcut `adls_raw_data` es visible y los archivos son accesibles.
3. Hacer clic en un archivo CSV → **"Preview"**.

> ✅ **Punto de validación final del desafío**:
> - Las Delta Tables de Azure SQL están visibles y consultables en `Tables/` del Lakehouse Bronze.
> - El Shortcut de ADLS Gen2 muestra los archivos en `Files/`.

---

### Errores comunes y soluciones

| Error | Causa probable | Solución |
|-------|---------------|----------|
| Activity failed: "Could not connect to source" | Firewall de Azure SQL bloqueando Fabric | Azure Portal → SQL Server → Networking → habilitar "Allow Azure services and resources to access this server" |
| "The table already exists and Import Schema is set to None" | Tabla preexistente con esquema incompatible | Cambiar la opción "Write behavior" a "Overwrite" en la pestaña Destination |
| Las filas copiadas son 0 | Tabla vacía o filtro aplicado en la consulta | Verificar que la tabla tiene datos en Azure SQL ejecutando un SELECT COUNT(*) directamente |
| Timeout en la actividad | Consulta o volumen muy grande | Incrementar el "Timeout" en la pestaña Settings o aplicar filtros de fecha para carga incremental |

---

## 🟡 Desafío 4 – Transformación de datos: Bronze → Silver → Gold

### Objetivo pedagógico
Aplicar transformaciones de datos con PySpark en Notebooks de Fabric, entendiendo el patrón Medallion y la progresión de calidad de datos desde Bronze hasta Gold.

---

### Paso a paso detallado

#### 4.1 – Crear un Notebook en Fabric

1. En el workspace, hacer clic en **"+ New item"**.
2. Seleccionar **"Notebook"**.
3. El notebook se creará y abrirá automáticamente con una celda vacía.
4. En la parte superior, hacer clic en el nombre del notebook (por defecto "Notebook 1") y renombrarlo a `nb_transformacion_bronze_silver_gold`.

---

#### 4.2 – Agregar los Lakehouses como recursos del Notebook

Para que el notebook pueda leer y escribir en los tres Lakehouses, es necesario agregarlos como recursos:

1. En el panel lateral izquierdo del notebook, en la sección **Data items** ,hacer clic en **"Add data items"**.
2. Seleccionar **"From OneLake Catalog"**.
3. En el selector, elegir `Lakehouse_Bronze` → **Add**.
4. Repetir el proceso para agregar `Lakehouse_Silver` y `Lakehouse_Gold`.
5. En el panel lateral, verificar que los tres Lakehouses aparecen bajo "Lakehouses" y el Lakehouse por defecto (marcado con una estrella) es `Lakehouse_Bronze`.

> 💡 **Consejo**: El Lakehouse marcado como "default" es al que apuntan las operaciones `spark.read.table("nombre_tabla")` sin especificar ruta. Es recomendable cambiar el Lakehouse por defecto según la sección del código que se está ejecutando.

---

#### 4.3 – Transformación Bronze → Silver

Agregar celdas Markdown y de código según la siguiente estructura:

**Celda 1 – Markdown (documentación):**
```markdown
## Bronze → Silver
### Lectura de datos crudos desde Lakehouse Bronze y limpieza de datos
```

**Celda 2 – Leer datos desde Bronze:**
```python
# Leer la tabla cruda desde Lakehouse Bronze
df_orders_bronze = spark.read.format("delta").load(
    "abfss://<workspace_id>@onelake.dfs.fabric.microsoft.com/<lakehouse_bronze_id>/Tables/sqls_salesorderheader"
)

# Alternativa más sencilla si Bronze es el Lakehouse default:
df_orders_bronze = spark.table("sqls_salesorderheader")

# Ver el esquema y primeras filas
df_orders_bronze.printSchema()
df_orders_bronze.show(5)
```

> 💡 **Consejo**: La forma más sencilla en Fabric es usar `spark.table("nombre_tabla")` cuando el Lakehouse está agregado como recurso.

**Celda 3 – Limpieza de datos:**
```python
from pyspark.sql import functions as F
from pyspark.sql.types import DateType

# 1. Eliminar duplicados
df_orders_silver = df_orders_bronze.dropDuplicates(["SalesOrderID"])

# 2. Tratar valores nulos
df_orders_silver = df_orders_silver.fillna({
    "Comment": "Sin comentario",
    "PurchaseOrderNumber": "N/A"
})

# 3. Estandarizar tipos de datos: convertir fechas
df_orders_silver = df_orders_silver.withColumn(
    "OrderDate", F.to_date(F.col("OrderDate"))
).withColumn(
    "DueDate", F.to_date(F.col("DueDate"))
).withColumn(
    "ShipDate", F.to_date(F.col("ShipDate"))
)

# 4. Estandarizar textos: mayúsculas en campos de texto clave
df_orders_silver = df_orders_silver.withColumn(
    "Status_std", F.when(F.col("Status") == 5, "Enviado")
                   .when(F.col("Status") == 4, "En proceso")
                   .otherwise("Otro")
)

# 5. Agregar columna de auditoría
df_orders_silver = df_orders_silver.withColumn(
    "_silver_ingestion_ts", F.current_timestamp()
)

df_orders_silver.show(5)
```

**Celda 4 – Escribir en Lakehouse Silver:**
```python
# Escribir como Delta Table en Lakehouse Silver
df_orders_silver.write.format("delta").mode("overwrite").saveAsTable(
    "Lakehouse_Silver.salesorderheader"
)
print("✅ Tabla salesorderheader escrita en Lakehouse Silver")
```

Repetir el proceso para las demás tablas:
- `sqls_customer` → limpieza de emails nulos, estandarización de nombres → `Lakehouse_Silver.customer`
- `sqls_salesorderdetail` → eliminar duplicados, validar cantidades → `Lakehouse_Silver.salesorderdetail`
- `sqls_product` → estandarización de nombres de producto → `Lakehouse_Silver.product`

---

#### 4.4 – Transformación Silver → Gold

**Celda 5 – Markdown:**
```markdown
## Silver → Gold
### Agregaciones y modelado dimensional para consumo analítico
```

**Celda 6 – Crear tabla de hechos de ventas:**
```python
# Leer tablas Silver
df_orders = spark.table("Lakehouse_Silver.salesorderheader")
df_details = spark.table("Lakehouse_Silver.salesorderdetail")
df_customers = spark.table("Lakehouse_Silver.customer")
df_products = spark.table("Lakehouse_Silver.product")

# Crear fact_ventas: unión de órdenes y detalles
fact_ventas = df_orders.join(df_details, "SalesOrderID", "inner") \
    .select(
        F.col("SalesOrderID").alias("id_orden"),
        F.col("SalesOrderDetailID").alias("id_detalle"),
        F.col("CustomerID").alias("id_cliente"),
        F.col("ProductID").alias("id_producto"),
        F.col("OrderDate").alias("fecha_orden"),
        F.col("OrderQty").cast("integer").alias("cantidad"),
        F.col("UnitPrice").cast("double").alias("precio_unitario"),
        F.col("LineTotal").cast("double").alias("monto_total")
    )

fact_ventas.show(5)
```

**Celda 7 – Crear dimensión de clientes:**
```python
dim_cliente = df_customers.select(
    F.col("CustomerID").alias("id_cliente"),
    F.concat_ws(" ", F.col("FirstName"), F.col("LastName")).alias("nombre_cliente"),
    F.col("EmailAddress").alias("email"),
    F.col("CompanyName").alias("empresa")
).distinct()
```

**Celda 8 – Crear dimensión de productos:**
```python
dim_producto = df_products.select(
    F.col("ProductID").alias("id_producto"),
    F.col("Name").alias("nombre_producto"),
    F.col("ProductNumber").alias("codigo_producto"),
    F.col("StandardCost").cast("double").alias("costo_estandar"),
    F.col("ListPrice").cast("double").alias("precio_lista"),
    F.col("Color").alias("color")
).distinct()
```

**Celda 9 – Crear dimensión de fecha:**
```python
from pyspark.sql.types import StructType, StructField, DateType, IntegerType, StringType
import pandas as pd

# Generar tabla de fechas a partir del rango de datos
fecha_min = fact_ventas.agg(F.min("fecha_orden")).collect()[0][0]
fecha_max = fact_ventas.agg(F.max("fecha_orden")).collect()[0][0]

fechas = pd.date_range(start=fecha_min, end=fecha_max, freq='D')
df_fechas = pd.DataFrame({
    "fecha": fechas,
    "anio": fechas.year,
    "mes": fechas.month,
    "dia": fechas.day,
    "nombre_mes": fechas.strftime('%B'),
    "trimestre": fechas.quarter,
    "semana_anio": fechas.isocalendar().week.astype(int)
})

dim_fecha = spark.createDataFrame(df_fechas)
```

**Celda 10 – Escribir tablas Gold:**
```python
# Escribir todas las tablas en Lakehouse Gold
fact_ventas.write.format("delta").mode("overwrite").saveAsTable("Lakehouse_Gold.fact_ventas")
dim_cliente.write.format("delta").mode("overwrite").saveAsTable("Lakehouse_Gold.dim_cliente")
dim_producto.write.format("delta").mode("overwrite").saveAsTable("Lakehouse_Gold.dim_producto")
dim_fecha.write.format("delta").mode("overwrite").saveAsTable("Lakehouse_Gold.dim_fecha")

print("✅ Tablas Gold creadas exitosamente:")
print("   - fact_ventas")
print("   - dim_cliente")
print("   - dim_producto")
print("   - dim_fecha")
```

---

#### 4.5 – Ejecutar el Notebook completo

1. En la barra superior, hacer clic en **"Run all"** (ejecutar todas las celdas en orden).
2. Observar el output de cada celda para detectar errores.
3. Una vez completada la ejecución, navegar a `Lakehouse_Gold` y verificar que las 4 tablas existen en `Tables/`.

> ✅ **Punto de validación**: Las 4 tablas (fact_ventas, dim_cliente, dim_producto, dim_fecha) existen en el Lakehouse Gold y son consultables desde el SQL Endpoint.

---

### Errores comunes y soluciones

| Error | Causa probable | Solución |
|-------|---------------|----------|
| `AnalysisException: Table or view not found: sqls_salesorderheader` | El Lakehouse Bronze no está agregado como recurso o no es el default | Verificar que el Lakehouse Bronze está añadido al notebook y es el default al momento de leer |
| `PermissionDenied` al escribir en Lakehouse Gold | Lakehouse Gold no está agregado como recurso del notebook | Agregar Lakehouse Gold como recurso del notebook desde el panel lateral |
| Error de conversión de tipos (`DoubleType cannot accept IntegerType`) | Incompatibilidad en el cast de columnas | Revisar y ajustar los `.cast()` o agregar `.cast("string")` antes de cast numérico |
| El notebook se queda colgado | Spark session sin iniciar o capacidad saturada | Reiniciar el kernel del notebook (botón de reinicio en la barra superior) |

---

## 🟣 Desafío 5 – Mi primer Modelo Semántico

### Objetivo pedagógico
Crear la capa semántica sobre los datos Gold, definiendo relaciones en esquema estrella y medidas DAX que encapsulen la lógica de negocio.

---

### Paso a paso detallado

#### 5.1 – Crear el Modelo Semántico desde el Lakehouse Gold

1. En el workspace, hacer clic en `Lakehouse_Gold` para abrirlo.
2. En la barra superior derecha, hacer clic en el botón **"New semantic model"**.
3. Se abrirá un panel de selección de tablas:
   - **Name**: `Modelo_Semantico_Gold_<tu-nombre>_-_<nombre-empresa>`
   - Seleccionar las 4 tablas: `fact_ventas`, `dim_cliente`, `dim_producto`, `dim_fecha`.
   - Hacer clic en **"Confirm"**.
4. El modelo semántico se creará y se abrirá automáticamente el editor de modelos.

> 💡 **Explicación**: El modelo se crea en modo **DirectLake**, lo que significa que lee directamente los archivos Delta del Lakehouse sin importar datos. Esto combina la frescura de los datos de DirectQuery con el rendimiento de los modelos importados.

---

#### 5.2 – Configurar las relaciones entre tablas

En el editor del modelo semántico (vista de diagrama):

1. **Relación fact_ventas → dim_cliente:**
   - Arrastrar el campo `id_cliente` de `fact_ventas` hacia `id_cliente` de `dim_cliente`.
   - Configurar:
     - Cardinalidad: **Many to one (*:1)**
     - Dirección del filtro: **Single** (desde dim_cliente hacia fact_ventas)
   - Hacer clic en **"Save"** o **"Apply"**.

2. **Relación fact_ventas → dim_producto:**
   - Arrastrar `id_producto` de `fact_ventas` hacia `id_producto` de `dim_producto`.
   - Cardinalidad: **Many to one (*:1)**
   - Dirección del filtro: **Single**

3. **Relación fact_ventas → dim_fecha:**
   - Arrastrar `fecha_orden` de `fact_ventas` hacia `fecha` de `dim_fecha`.
   - Cardinalidad: **Many to one (*:1)**
   - Dirección del filtro: **Single**

4. Verificar en la vista de diagrama que el esquema estrella luce correctamente: `fact_ventas` en el centro, las tres dimensiones alrededor conectadas por líneas.

> ✅ **Punto de validación**: Las 3 relaciones aparecen en el diagrama y no hay alertas de ambigüedad.

---

#### 5.3 – Crear medidas DAX

Para crear medidas, se necesita trabajar en la vista de datos del modelo semántico. En Fabric, esto se hace desde el editor de modelos o desde Power BI Desktop conectado al modelo.

**Método en Fabric (recomendado en el hackathon):**

1. En el editor del modelo semántico, hacer clic en la tabla `fact_ventas` en la vista de lista de tablas.
2. Hacer clic en **"New measure"** en la barra superior.
3. Crear las siguientes medidas:

**Medida 1 – Total Ventas:**
```dax
Total Ventas = SUM(fact_ventas[monto_total])
```
- Formatear como: **Currency** (moneda), 2 decimales.
- Descripción: "Suma total del monto de ventas en el periodo seleccionado."

**Medida 2 – Ticket Promedio:**
```dax
Ticket Promedio = DIVIDE([Total Ventas], COUNTROWS(fact_ventas), 0)
```
- Formatear como: **Currency**, 2 decimales.
- Descripción: "Monto promedio por línea de detalle de venta."

**Medida 3 – Ventas YTD (Year-to-Date):**
```dax
Ventas YTD = TOTALYTD([Total Ventas], dim_fecha[fecha])
```
- Formatear como: **Currency**, 2 decimales.
- Descripción: "Ventas acumuladas desde el inicio del año hasta la fecha seleccionada."

**Medida 4 – Total Órdenes (adicional recomendada):**
```dax
Total Ordenes = DISTINCTCOUNT(fact_ventas[id_orden])
```
- Formatear como: **Whole number**.

4. Guardar el modelo semántico haciendo clic en el ícono de guardar (💾) o `Ctrl + S`.

---

#### 5.4 – Organizar medidas en una tabla dedicada

1. Crear una tabla de medidas para organización:
   - Hacer clic en **"Enter data"** en la barra de herramientas del modelo.
   - Crear una tabla llamada `_Medidas` con una sola columna vacía.
   - Hacer clic en **"Load"**.
2. Mover las medidas creadas a la tabla `_Medidas`:
   - Hacer clic derecho sobre cada medida → **"Move to table"** → seleccionar `_Medidas`.
3. Ocultar la columna vacía de la tabla `_Medidas` para que no aparezca en los reportes.

---

#### 5.5 – Validar el modelo con un reporte rápido

1. Desde el editor del modelo semántico, hacer clic en **"Create report"** (o "New report") en la barra superior.
2. En el reporte de prueba, arrastrar:
   - `dim_fecha[nombre_mes]` al eje X de un gráfico de barras.
   - `[Total Ventas]` medida a los Valores.
3. Verificar que el gráfico muestra ventas por mes.
4. Agregar un filtro con `dim_producto[nombre_producto]` y verificar que el gráfico responde al filtro cruzado.

> ✅ **Punto de validación final del desafío**:
> - El modelo semántico existe en el workspace.
> - Las 3 relaciones están correctamente definidas en esquema estrella.
> - Las 4 medidas DAX existen y funcionan en el reporte de validación.

---

### Errores comunes y soluciones

| Error | Causa probable | Solución |
|-------|---------------|----------|
| "Cannot create relationship: ambiguous path" | Existencia de múltiples rutas entre tablas | Revisar si hay relaciones duplicadas o directas entre dimensiones. Eliminar las redundantes |
| La medida TOTALYTD devuelve BLANK | La columna de fecha no es de tipo Date | En el notebook, verificar que `dim_fecha[fecha]` es tipo `DateType` y no `StringType` |
| DirectLake muestra error "Framing required" | La tabla Delta tiene esquema desactualizado | En el Lakehouse Gold, hacer clic derecho sobre la tabla → "Refresh" para actualizar el frame del modelo |
| Las medidas no aparecen en Power BI | El modelo no se guardó | Guardar el modelo semántico y actualizar el explorador del workspace |

---

## 📊 Desafío 6 – Creando un informe en Power BI

### Objetivo pedagógico
Construir un informe profesional e interactivo en Power BI conectado al Modelo Semántico Gold, aplicando buenas prácticas de diseño visual y storytelling con datos.

---

### Paso a paso detallado

#### 6.1 – Crear el informe desde el Modelo Semántico

1. En el workspace, localizar el ítem `Modelo_Semantico_Gold`.
2. Hacer clic en los tres puntos (`...`) del modelo semántico → **"Create report"**.
   - Alternativa: Hacer clic sobre el nombre del modelo → en la pantalla del modelo, hacer clic en **"Create report"**.
3. Se abrirá el editor de Power BI en el navegador (Power BI Service embedded en Fabric).
4. Verificar en el panel de **Datos** (derecha) que aparecen las tablas: `fact_ventas`, `dim_cliente`, `dim_producto`, `dim_fecha` y la tabla `_Medidas`.

---

#### 6.2 – Configurar el tema visual

Antes de crear visualizaciones, aplicar un tema consistente:

1. En la barra superior, hacer clic en **"View"** → **"Themes"**.
2. Seleccionar un tema predefinido (ej. "Executive", "City Park", "Classroom") o importar el tema corporativo si se dispone de uno.
3. El tema se aplicará a todas las visualizaciones del informe.

---

#### 6.3 – Crear la Página 1 – Resumen Ejecutivo

**Configurar la página:**
1. En la parte inferior, la primera página ya existe. Hacer doble clic en la pestaña "Page 1" y renombrarla a `Resumen Ejecutivo`.

**Agregar tarjetas de KPIs:**
1. En el panel de Visualizaciones, hacer clic en el ícono de **"Card"** (tarjeta).
2. Una tarjeta vacía aparece en el canvas. Arrastrarla a la esquina superior izquierda.
3. En el panel de Datos, arrastrar la medida **`[Total Ventas]`** al campo "Fields" de la tarjeta.
4. Formatear la tarjeta:
   - Hacer clic en el ícono de pincel (Format).
   - En "Call out value": tamaño de fuente 28, negrita.
   - En "Category label": cambiar el texto a "Total Ventas".
   - Agregar un borde sutil o fondo con color del tema.
5. Copiar y pegar la tarjeta (`Ctrl+C`, `Ctrl+V`). 
6. En la nueva tarjeta, reemplazar la medida por **`[Ticket Promedio]`**.
7. Copiar nuevamente y agregar **`[Total Ordenes]`**.
8. Alinear las 3 tarjetas horizontalmente en la parte superior del canvas.

**Agregar gráfico de tendencia temporal:**
1. Hacer clic en el ícono de **"Line chart"** (gráfico de líneas) en el panel de Visualizaciones.
2. Configurar los campos:
   - **X-axis**: `dim_fecha[fecha]` (o `dim_fecha[anio]` y `dim_fecha[nombre_mes]` para jerarquía).
   - **Y-axis**: `[Total Ventas]`.
   - **Legend** (opcional): vacío o segmentado por una dimensión relevante.
3. Ajustar el tamaño del gráfico para que ocupe el área central-inferior de la página.
4. Agregar título descriptivo: "Evolución de Ventas en el Tiempo".

**Agregar título de página:**
1. Insertar un cuadro de texto (Insert → Text box).
2. Escribir el título: **"Resumen Ejecutivo de Ventas"**.
3. Formatear con fuente grande (24px), negrita, color del tema.

---

#### 6.4 – Crear la Página 2 – Análisis Detallado

**Crear nueva página:**
1. Hacer clic en el ícono **"+"** en la barra de páginas (parte inferior).
2. Renombrar la nueva página a `Análisis Detallado`.

**Agregar segmentador (Slicer):**
1. Hacer clic en el ícono de **"Slicer"** en el panel de Visualizaciones.
2. Arrastrar `dim_fecha[anio]` al campo del slicer.
3. En el formato, cambiar el estilo a **"Dropdown"** o **"Tiles"** para mayor usabilidad.
4. Posicionar el slicer en la esquina superior izquierda.
5. Agregar un segundo slicer con `dim_producto[nombre_producto]` (o categoría si está disponible).

**Agregar gráfico de barras/columnas:**
1. Hacer clic en el ícono de **"Clustered bar chart"** o **"Clustered column chart"**.
2. Configurar:
   - **Axis**: `dim_producto[nombre_producto]`
   - **Values**: `[Total Ventas]`
3. Ordenar de mayor a menor (clic en los tres puntos del visual → "Sort descending by Total Ventas").
4. Limitar a Top 10 si hay muchos productos:
   - En el panel de Filtros, aplicar "Top N filter" con N=10 por `[Total Ventas]`.
5. Agregar título: "Top Productos por Ventas".

**Agregar tabla de detalle:**
1. Hacer clic en el ícono de **"Table"** en el panel de Visualizaciones.
2. Agregar los campos:
   - `dim_cliente[nombre_cliente]`
   - `dim_producto[nombre_producto]`
   - `dim_fecha[anio]`
   - `dim_fecha[nombre_mes]`
   - `[Total Ventas]`
   - `[Ticket Promedio]`
3. Habilitar "Conditional formatting" en la columna `[Total Ventas]` para resaltar los valores más altos.

**Agregar título de página:**
1. Insertar cuadro de texto con título: **"Análisis Detallado de Ventas"**.

---

#### 6.5 – Configurar interacciones entre visualizaciones

1. Hacer clic en cualquier visualización para seleccionarla.
2. En la barra superior, hacer clic en **"Format"** → **"Edit interactions"**.
3. Verificar que cada visual muestra íconos de interacción (filtro, resaltado, ninguno) sobre los demás visuals.
4. Configurar según preferencia:
   - Gráfico de barras → filtra la tabla de detalle ✅
   - Slicer de año → filtra todos los visuals ✅
5. Cuando esté configurado, hacer clic en **"Edit interactions"** nuevamente para desactivar el modo de edición.

---

#### 6.6 – Guardar y publicar el informe

1. Hacer clic en el ícono de guardar (💾) o presionar `Ctrl+S`.
2. **Name**: `Informe_Ventas_Gold`
3. Verificar que el informe se guarda en el workspace de Fabric.
4. Una vez guardado, navegar al workspace y verificar que el informe aparece como ítem de tipo "Report".
5. Hacer clic en el informe → verificar que se abre correctamente y todas las visualizaciones cargan.

> ✅ **Punto de validación final del desafío**:
> - El informe existe en el workspace con 2 páginas.
> - Página 1 muestra 3 tarjetas de KPIs y un gráfico de líneas.
> - Página 2 muestra slicers, gráfico de barras y tabla.
> - Los slicers filtran correctamente las demás visualizaciones.

---

### Errores comunes y soluciones

| Error | Causa probable | Solución |
|-------|---------------|----------|
| "Can't display visual – data is too large" | Too many rows sin filtrar en una tabla | Aplicar Top N filter o agregar un slicer de fecha para limitar los datos |
| Los slicers no filtran las demás visualizaciones | Interacciones configuradas en "None" | Usar "Edit interactions" para cambiar a "Filter" |
| Las medidas DAX devuelven ERROR en el visual | División por cero no manejada | En la medida DIVIDE, verificar que el tercer argumento sea 0 para evitar errores |
| El informe no carga DirectLake correctamente | Cache del modelo semántico no actualizado | Desde el workspace, actualizar el modelo semántico (tres puntos → "Refresh") |

---

## ⭐ Bonus Track – Tu primer Data Agent en Microsoft Fabric

### Objetivo pedagógico
Demostrar las capacidades de IA generativa integradas en Fabric, cerrando el ciclo "del dato a la acción" con una interfaz conversacional sobre los datos analíticos.

---

### Pre-requisitos específicos
- Confirmar con el administrador del tenant que **Fabric Copilot / AI features** están habilitadas.
- El Modelo Semántico Gold debe estar publicado y con datos (completar Desafío 5 antes).

---

### Paso a paso detallado

#### BT.1 – Crear el Data Agent

1. En el workspace, hacer clic en **"+ New item"**.
2. Buscar **"Data agent"** en el catálogo de ítems.
   > Si no aparece: verificar que la capacidad de Fabric tiene habilitadas las funcionalidades de IA. Puede que sea necesario activarlo en la configuración del tenant o de la capacidad.
3. Hacer clic en **"Data agent"**.
4. **Name**: `DataAgent_Ventas_Gold`
5. Hacer clic en **"Create"**. Se abrirá la interfaz de configuración del Data Agent.

---

#### BT.2 – Conectar el Modelo Semántico como fuente de datos

1. En la interfaz del Data Agent, localizar la sección **"Data sources"** o **"Add data"**.
2. Hacer clic en **"Add"** o **"+ Add data source"**.
3. Seleccionar **"Semantic model"**.
4. En el selector, elegir `Modelo_Semantico_Gold` del workspace actual.
5. Hacer clic en **"Add"** o **"Confirm"**.
6. Verificar que el modelo semántico aparece en la lista de fuentes de datos y que las tablas y medidas son visibles para el agente.

---

#### BT.3 – Configurar instrucciones de contexto

En la sección de instrucciones o "System prompt" del agente:

1. Hacer clic en **"Instructions"** o el campo de configuración del contexto.
2. Ingresar instrucciones descriptivas del dominio:

```
Este agente responde preguntas sobre datos de ventas de una empresa retail.

El modelo de datos contiene:
- fact_ventas: tabla de hechos con transacciones de ventas (id_orden, id_cliente, id_producto, fecha_orden, cantidad, precio_unitario, monto_total)
- dim_cliente: dimensión de clientes (id_cliente, nombre_cliente, email, empresa)
- dim_producto: dimensión de productos (id_producto, nombre_producto, codigo_producto, costo_estandar, precio_lista, color)
- dim_fecha: dimensión de tiempo (fecha, anio, mes, nombre_mes, trimestre, semana_anio)

Medidas disponibles:
- Total Ventas: suma del monto total de ventas
- Ticket Promedio: monto promedio por línea de detalle
- Ventas YTD: ventas acumuladas en el año
- Total Ordenes: número de órdenes distintas

Al responder, siempre indica el periodo de tiempo de los datos cuando sea relevante.
Si la pregunta es ambigua, solicita aclaración antes de responder.
```

3. Guardar la configuración.

---

#### BT.4 – Configurar ejemplos few-shot

En la sección de ejemplos del agente, agregar al menos 3 pares pregunta-respuesta:

**Ejemplo 1:**
- Pregunta: `¿Cuánto fue el total de ventas en 2023?`
- Respuesta esperada: `El total de ventas en 2023 fue de [valor]. Este cálculo utiliza la medida Total Ventas filtrando el año 2023 en la dimensión de fecha.`

**Ejemplo 2:**
- Pregunta: `¿Cuál fue el producto más vendido por monto?`
- Respuesta esperada: `El producto con mayor monto de ventas fue [nombre_producto] con un total de [valor]. Se obtiene ordenando la medida Total Ventas por dim_producto.`

**Ejemplo 3:**
- Pregunta: `Muéstrame las ventas por trimestre del último año`
- Respuesta esperada: `Las ventas por trimestre del último año disponible son: Q1: [valor], Q2: [valor], Q3: [valor], Q4: [valor].`

---

#### BT.5 – Probar el agente con preguntas de negocio

En la interfaz conversacional del Data Agent, realizar las siguientes preguntas y verificar las respuestas:

| # | Pregunta | ¿Qué verificar? |
|---|----------|----------------|
| 1 | *"¿Cuál fue el total de ventas del último trimestre disponible?"* | Valor correcto, periodo mencionado |
| 2 | *"¿Qué producto tuvo el mayor ticket promedio?"* | Nombre del producto y valor de ticket |
| 3 | *"Muéstrame las ventas por mes del año más reciente"* | Desglose mensual coherente |
| 4 | *"¿Cuántas órdenes únicas se procesaron en total?"* | Número que corresponde a Total Ordenes |
| 5 | *"¿Cuál es el cliente que más compró en términos de monto total?"* | Nombre del cliente y monto |

Para cada respuesta, **contrastar el valor devuelto por el agente con el informe de Power BI** del Desafío 6 para validar la precisión.

---

#### BT.6 – Publicar y compartir el Data Agent

1. Con el agente configurado y probado, hacer clic en **"Save"** o **"Publish"**.
2. En el workspace, verificar que el ítem `DataAgent_Ventas_Gold` aparece publicado.
3. Para compartir con otros usuarios:
   - Hacer clic en los tres puntos del Data Agent → **"Share"**.
   - Ingresar los correos de los participantes o grupos.
   - Asignar permisos: **"Can use"** (para consultar el agente) o **"Can edit"** (para configurarlo).

> ✅ **Punto de validación final del Bonus Track**:
> - El Data Agent está publicado en el workspace.
> - Las instrucciones de contexto están configuradas con información del dominio.
> - Al menos 3 ejemplos few-shot están definidos.
> - 5 preguntas respondidas correctamente y validadas contra el informe de Power BI.

---

### Errores comunes y soluciones

| Error | Causa probable | Solución |
|-------|---------------|----------|
| "Data Agent" no aparece en "+ New item" | Fabric Copilot no habilitado en el tenant o la capacidad | Contactar al administrador del tenant para habilitar "Copilot and Azure OpenAI Service" en la configuración del tenant |
| El agente responde "No tengo datos suficientes" | El modelo semántico no está correctamente conectado | Verificar que el modelo semántico se agregó como fuente de datos y que tiene datos (actualizar si es necesario) |
| Las respuestas son incorrectas o imprecisas | Instrucciones de contexto insuficientes | Mejorar las instrucciones: ser más específico sobre los nombres de tablas, las relaciones y las reglas de negocio |
| El agente no responde preguntas temporales correctamente | La dimensión de fecha no tiene jerarquía clara | Verificar que `dim_fecha` tiene columnas de año, mes, trimestre y que las relaciones con `fact_ventas` son correctas |

---

## 📋 Checklist de validación general para el instructor

Al finalizar el hackathon completo, el entorno debería tener los siguientes ítems en el workspace:

### Artifacts esperados en el workspace

```
Workspace: Hackathon-DataToAction
│
├── 🏗 Lakehouses (3)
│   ├── Lakehouse_Bronze
│   │   ├── Tables/: sqls_salesorderheader, sqls_salesorderdetail, sqls_customer, sqls_product
│   │   └── Files/: adls_raw_data (Shortcut → ADLS Gen2)
│   ├── Lakehouse_Silver
│   │   └── Tables/: salesorderheader, salesorderdetail, customer, product
│   └── Lakehouse_Gold
│       └── Tables/: fact_ventas, dim_cliente, dim_producto, dim_fecha
│
├── 🔄 Data Pipelines (1)
│   └── pipeline_ingesta_bronze_sql
│
├── 📓 Notebooks (1)
│   └── nb_transformacion_bronze_silver_gold
│
├── 🧠 Semantic Models (1)
│   └── Modelo_Semantico_Gold
│       ├── Relaciones: fact_ventas ↔ dim_cliente, dim_producto, dim_fecha
│       └── Medidas: Total Ventas, Ticket Promedio, Ventas YTD, Total Ordenes
│
├── 📊 Reports (1)
│   └── Informe_Ventas_Gold
│       ├── Página 1: Resumen Ejecutivo (KPIs + gráfico de tendencia)
│       └── Página 2: Análisis Detallado (slicers + gráfico barras + tabla)
│
└── ⭐ Data Agents (1) [Bonus Track]
    └── DataAgent_Ventas_Gold
```

---

## 🧑‍🏫 Consejos pedagógicos para el instructor

### Gestión del tiempo
- Si los participantes se retrasan en el Desafío 4 (transformación PySpark), proporcionar el notebook pre-construido (`notebook-transformacion.ipynb`) para que puedan avanzar.
- El Bonus Track es independiente: los participantes que terminen antes pueden avanzar sin esperar al grupo.

### Puntos de pausa para discusión grupal
1. **Después del Desafío 1**: Discutir por qué 3 Lakehouses separados (vs. uno solo con carpetas). Respuesta: separación de permisos, linaje de datos, evitar modificaciones accidentales de datos crudos.
2. **Después del Desafío 3**: Discutir cuándo usar Copy Data (replicación) vs. Shortcut (referencia). Respuesta: Copy Data crea una copia local optimizada; Shortcut evita duplicación pero depende de la fuente original.
3. **Después del Desafío 4**: Mostrar en el SQL Endpoint de Gold cómo los datos ya están listos para SQL Analytics sin pasos adicionales.
4. **Después del Desafío 5**: Explicar DirectLake vs. Import vs. DirectQuery y cuándo usar cada modo.

### Demo en vivo vs. guía hands-on
- Se recomienda que el instructor haga una demo rápida (~5 min) de cada desafío ANTES de que los participantes lo intenten.
- Usar un workspace separado para la demo del instructor, para no mezclar con el workspace de los participantes.

### Recursos de apoyo listos para compartir
- 📎 [`notebook-transformacion.ipynb`](bloque-2/notebook-transformacion.ipynb) — Notebook de referencia con código PySpark.
- 🔗 [Microsoft Fabric Learning Path](https://learn.microsoft.com/es-es/training/paths/get-started-fabric/)
- 🔗 [Power BI DAX Guide](https://dax.guide/)
