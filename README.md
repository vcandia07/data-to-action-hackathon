# 🚀 Del Dato a la Acción – Hackatón Microsoft Fabric GOVms & DataKnow

Transformando **datos en decisiones** utilizando **Microsoft Fabric, Lakehouse y Power BI**


---

# 📖 Descripción

Este repositorio contiene los **desafíos y soluciones desarrollados durante la hackatón "Del Dato a la Acción" organizada por Microsoft**.

El objetivo es construir una solución **end-to-end de analítica de datos** utilizando **Microsoft Fabric**, desde la **ingesta de datos** hasta la **visualización en Power BI**, aplicando buenas prácticas de **ingeniería de datos y arquitectura moderna**.

---

# 🎯 Objetivo

Transformar datos en **insights accionables** mediante una arquitectura moderna basada en:

- Data Engineering
- Data Pipelines
- Lakehouse
- Modelado Semántico
- Visualización de datos

---

# 🏗 Arquitectura de Datos

El proyecto sigue el patrón **Medallion Architecture**, ampliamente utilizado en plataformas modernas de datos.
```
      ┌───────────────┐
      │  Data Sources │
      │ Azure SQL     │
      │ Blob Storage  │
      └───────┬───────┘
              │
              ▼
       🟤 Bronze Layer
    Datos crudos (Raw Data)
              │
              ▼
       ⚙️ Silver Layer
 Datos limpios y transformados
              │
              ▼
       🟡 Gold Layer
 Datos listos para análisis
              │
              ▼
         📊 Power BI
        Insights & Reportes

```
---

# 🧠 Contenido de la Hackatón

La hackatón está dividida en **3 bloques principales + bonus**.

| Bloque | Desafío | Descripción | Enlace |
|------|------|------|------|
| 🟤 Bloque 1 | Crear y configurar Workspace y Lakehouse | Configuración del entorno de trabajo en Fabric y creación de las capas **Bronze, Silver y Gold** | [Ver desafío](del-dato-a-la-accion-hackathon/bloque-1/1-workspace-fabric.md) |
| 🟤 Bloque 1 | Conectarse a fuentes de datos | Integración con **Azure SQL Database** y **Azure Blob Storage** | [Ver desafío](del-dato-a-la-accion-hackathon/bloque-1/2-conexion-fuentes.md) |
| 🟤 Bloque 1 | Mi primer Pipeline de ingesta | Construcción de un **pipeline de ingesta de datos en Fabric** | [Ver desafío](del-dato-a-la-accion-hackathon/bloque-1/3-ingesta-bronze.md) |
| ⚙️ Bloque 2 | Arquitectura Medallion | Implementación de la arquitectura **Bronze → Silver → Gold** | [Ver desafío](del-dato-a-la-accion-hackathon/bloque-2/4-transformacion-bronze-to-gold.md) |
| ⚙️ Bloque 2 | Mi primer Modelo Semántico | Creación de un **modelo semántico para análisis** | [Ver desafío](del-dato-a-la-accion-hackathon/bloque-2/5-modelo-semantico.md) |
| 📊 Bloque 3 | Creando un Informe en Power BI | Construcción de **reportes analíticos en Power BI** | [Ver desafío](del-dato-a-la-accion-hackathon/bloque-3/6-informe-power-bi.md) |
| ⭐ Bonus | Creando un Fabric Data Agent | Implementación de un **Data Agent en Microsoft Fabric** | [Ver desafío](del-dato-a-la-accion-hackathon/bonus-track/7-data-agent-fabric.md) |

---

# 🛠 Tecnologías utilizadas
Principales herramientas utilizadas:

- Microsoft Fabric
- Lakehouse
- Data Pipelines
- Power BI
- Azure SQL Database
- Azure Blob Storage
- Python

---

# 📂 Estructura del repositorio
```
del-dato-a-la-accion-hackathon
│
├── bloque-1
│   ├── workspace-fabric
│   ├── conexiones-datos
│   └── pipeline-ingesta
│
├── bloque-2
│   ├── arquitectura-medallion
│   └── modelo-semantico
│
├── bloque-3
│   └── powerbi-report
│
└── bonus
    └── fabric-data-agent
```
---

# 📊 Flujo de datos
```
Data Sources
│
▼
Data Ingestion (Pipelines)
│
▼
Lakehouse - Bronze
│
▼
Lakehouse - Silver
│
▼
Lakehouse - Gold
│
▼
Semantic Model
│
▼
Power BI Reports
```