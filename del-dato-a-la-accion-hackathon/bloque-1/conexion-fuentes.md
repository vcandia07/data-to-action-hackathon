# 🔵 Desafío 2 – Conexión a fuentes de datos externas

## 🎯 Objetivo

Conectar el entorno de **Microsoft Fabric** a fuentes de datos externas para habilitar la ingesta de información hacia la capa **Bronze** del Lakehouse.

El objetivo es establecer las integraciones necesarias para que los datos provenientes de **Azure SQL Database** y **Azure Data Lake Storage** estén disponibles dentro del entorno Fabric configurado en el desafío anterior.

---

## 🧩 Contexto

Con el entorno base ya configurado (Workspace y Lakehouses Bronze, Silver y Gold), el siguiente paso es **conectar las fuentes de datos de la organización**.

La organización cuenta con dos fuentes principales:

- Una base de datos relacional en **Azure SQL Database** que contiene datos transaccionales.
- Un repositorio de archivos en **Azure Data Lake Storage Gen2 (ADLS Gen2)** que almacena archivos en formatos como CSV, Parquet y JSON.

Para integrar estas fuentes, Microsoft Fabric ofrece dos mecanismos complementarios:

- **Conexiones (Data Sources)** → para conectarse a bases de datos como Azure SQL.
- **Shortcuts** → para referenciar datos externos en ADLS Gen2 sin necesidad de copiarlos físicamente.

---

## 📋 Desafío

Configurar las integraciones necesarias en **Microsoft Fabric** para acceder a las fuentes de datos externas:

- Crear una **conexión a Azure SQL Database** desde Microsoft Fabric.
- Crear un **Shortcut en el Lakehouse Bronze** que apunte a un contenedor de **Azure Data Lake Storage Gen2**.
- Verificar que los datos de ambas fuentes sean accesibles desde el entorno Fabric.

---

## 🧠 Consideraciones

Durante la configuración de las conexiones se deben tener en cuenta:

- Las credenciales de acceso a Azure SQL Database (servidor, base de datos, usuario y contraseña).
- La cuenta de almacenamiento y el contenedor de ADLS Gen2 al que se desea apuntar.
- Los permisos necesarios en Azure para que Fabric pueda acceder a los recursos (rol **Storage Blob Data Reader** como mínimo para ADLS).
- Los Shortcuts **no copian los datos**, sino que los referencian en su ubicación original, lo que optimiza el almacenamiento.
- Las conexiones creadas en Fabric pueden reutilizarse en múltiples pipelines y dataflows.

---

## 🏗 Arquitectura esperada

```
Workspace
│
├── Lakehouse_Bronze
│   ├── Shortcut → ADLS Gen2 (contenedor de archivos fuente)
│   └── (datos listos para ingesta desde Azure SQL)
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

- Una **conexión activa a Azure SQL Database** configurada en Microsoft Fabric.
- Un **Shortcut creado en Lakehouse Bronze** apuntando al contenedor de ADLS Gen2.
- Acceso verificado a los datos de ambas fuentes desde el entorno Fabric.
- El entorno preparado para comenzar los procesos de **ingesta de datos** en el siguiente desafío.

---

## 📚 Recursos recomendados

- [Conexiones en Microsoft Fabric](https://learn.microsoft.com/es-es/fabric/data-factory/connector-azure-sql-database)
- [Shortcuts en Microsoft Fabric Lakehouse](https://learn.microsoft.com/es-es/fabric/onelake/onelake-shortcuts)
- [Conectar a Azure Data Lake Storage Gen2](https://learn.microsoft.com/es-es/fabric/onelake/create-adls-shortcut)
- [Roles y permisos en Azure Storage](https://learn.microsoft.com/es-es/azure/storage/blobs/assign-azure-role-data-access)

---

## 🚀 Próximo desafío

En el siguiente desafío se realizará la **ingesta de datos** desde las fuentes conectadas hacia el **Lakehouse Bronze**, utilizando pipelines de datos en Microsoft Fabric.
