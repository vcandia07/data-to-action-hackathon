# 🟠 Desafío 3 – Ingesta de datos hacia Bronze

## 🎯 Objetivo

Poblar la capa **Bronze** del Lakehouse en Microsoft Fabric con datos provenientes de dos fuentes externas: **Azure SQL Database** (mediante un pipeline de copia) y **Azure Data Lake Storage Gen2** (mediante un Shortcut), preservando los datos en su forma original.

---

## 🧩 Contexto

Con las conexiones ya configuradas en el desafío anterior, el siguiente paso es **hacer llegar los datos** al Lakehouse Bronze desde ambas fuentes de la organización.

La capa Bronze tiene como propósito almacenar los datos **tal como llegan desde la fuente**, sin transformaciones. Esto garantiza trazabilidad, permite reprocesar datos en caso de errores en capas superiores y actúa como respaldo histórico de la información original.

Para cubrir ambas fuentes se utilizan dos mecanismos de Fabric:

- **Data Factory Pipeline con Copy Data** → extrae tablas desde Azure SQL Database y las carga como Delta Tables en Bronze.
- **Shortcut a ADLS Gen2** → referencia los archivos del Data Lake directamente dentro del Lakehouse Bronze sin necesidad de copiarlos.

---

## 📋 Desafío

Implementar los dos mecanismos de ingesta hacia el **Lakehouse Bronze**:

**Ingesta desde Azure SQL Database:**
- Crear un **Data Pipeline** en Microsoft Fabric.
- Configurar una actividad **Copy Data** que lea desde Azure SQL Database.
- Definir el **Lakehouse Bronze** como destino de la copia.
- Almacenar los datos directamente como **Delta Tables** en la sección de tablas del Lakehouse Bronze.
- Ejecutar el pipeline y verificar que los datos fueron cargados correctamente.

**Shortcut a Azure Data Lake Storage Gen2:**
- Crear un **Shortcut** dentro del Lakehouse Bronze apuntando al contenedor de ADLS Gen2 configurado en el desafío anterior.
- Verificar que los archivos del Data Lake sean accesibles desde el Lakehouse Bronze a través del Shortcut.

---

## 🧠 Consideraciones

- Los datos en Bronze deben almacenarse **sin transformaciones**: tal como llegan desde la fuente.
- Los datos de Azure SQL se cargan como **Delta Tables** en la sección `Tables/` del Lakehouse Bronze, lo que permite consultarlos mediante SQL Endpoint sin pasos adicionales.
- Se recomienda nombrar las tablas reflejando el origen, por ejemplo: `sqls_nombre_tabla`.
- El **Shortcut a ADLS Gen2** aparece en la sección `Files/` del Lakehouse Bronze y **no copia los datos**: los referencia en su ubicación original, optimizando el almacenamiento.
- Considerar si la carga desde SQL es **full load** (carga completa) o **incremental**, dependiendo del volumen y frecuencia de actualización de los datos.
- Registrar la fecha y hora de extracción como metadato para trazabilidad.

---

## 🏗 Arquitectura esperada

```
Azure SQL Database          ADLS Gen2
        │                      │
        │ (Copy Data Activity) │ (Shortcut)
        ▼                      ▼
             Workspace
             │
             ├── Lakehouse_Bronze
             │   ├── Tables/
             │   │   └── sqls_nombre_tabla  (Delta Table)
             │   └── Files/
             │       └── adls_shortcut → (referencia a ADLS Gen2)
             │
             ├── Lakehouse_Silver
             │   └── (pendiente – próximos desafíos)
             │
             └── Lakehouse_Gold
                 └── (pendiente – próximos desafíos)
```

---

## ✅ Resultado esperado

Al finalizar el desafío, el entorno debería contar con:

- Un **Data Pipeline** creado y ejecutado exitosamente en Microsoft Fabric.
- Los datos de Azure SQL Database **cargados como Delta Tables en el Lakehouse Bronze**.
- Las tablas Delta disponibles y consultables desde el **SQL Endpoint del Lakehouse Bronze**.
- Un **Shortcut a ADLS Gen2** creado en la sección `Files/` del Lakehouse Bronze y con acceso verificado a los archivos del Data Lake.
- El entorno preparado para comenzar las **transformaciones hacia la capa Silver** en el siguiente desafío.

---

## 📚 Recursos recomendados

- [Data Pipelines en Microsoft Fabric](https://learn.microsoft.com/es-es/fabric/data-factory/data-factory-overview)
- [Actividad Copy Data en Microsoft Fabric](https://learn.microsoft.com/es-es/fabric/data-factory/copy-data-activity)
- [Conector de Azure SQL Database en Data Factory](https://learn.microsoft.com/es-es/fabric/data-factory/connector-azure-sql-database)
- [Delta Tables en Microsoft Fabric Lakehouse](https://learn.microsoft.com/es-es/fabric/data-engineering/lakehouse-and-delta-tables)
- [Shortcuts en Microsoft Fabric Lakehouse](https://learn.microsoft.com/es-es/fabric/onelake/onelake-shortcuts)
- [Crear un Shortcut a Azure Data Lake Storage Gen2](https://learn.microsoft.com/es-es/fabric/onelake/create-adls-shortcut)

---

## 🚀 Próximo desafío

En el siguiente desafío se realizará la **transformación de datos** desde la capa Bronze hacia la capa **Silver**, aplicando limpieza, estandarización y enriquecimiento mediante **Notebooks de Spark** en Microsoft Fabric.
