# 🟤 Desafío 1 – Configuración del entorno en Microsoft Fabric

## 🎯 Objetivo

Diseñar y configurar un entorno de trabajo en **Microsoft Fabric** que permita almacenar y procesar datos siguiendo el enfoque de **arquitectura Medallion (Bronze, Silver y Gold)**.

El objetivo es establecer las bases de una plataforma de datos moderna que permita **ingestar, transformar y preparar datos para análisis posteriores**.

---

## 🧩 Contexto

Una organización necesita comenzar a centralizar sus datos en una plataforma analítica moderna utilizando **Microsoft Fabric**.

Para ello, es necesario crear un entorno de trabajo que permita almacenar datos provenientes de múltiples fuentes y estructurarlos en distintas capas que representen el nivel de procesamiento de la información.

La arquitectura elegida para este desafío es la **Medallion Architecture**, que organiza los datos en tres niveles principales:

- **Bronze** → Datos crudos provenientes de las fuentes de origen  
- **Silver** → Datos transformados y depurados  
- **Gold** → Datos optimizados para análisis y consumo

---

## 📋 Desafío

Configurar un entorno de datos en **Microsoft Fabric** que cumpla con las siguientes condiciones:

- Crear un **Workspace** dedicado al proyecto.
- Implementar un **Lakehouse** que sirva como repositorio central de datos.
- Diseñar una estructura que represente las capas de la arquitectura **Bronze, Silver y Gold**.
- Definir una organización de carpetas o tablas que permita separar claramente cada capa de procesamiento.

---

## 🧠 Consideraciones

Durante la implementación del entorno se deben considerar aspectos como:

- Organización lógica del almacenamiento de datos.
- Separación clara entre las capas de procesamiento.
- Escalabilidad para futuras transformaciones y procesos analíticos.
- Buenas prácticas de arquitectura de datos en entornos Lakehouse.

---

## 🏗 Arquitectura esperada

```
Lakehouse
│
├── Bronze
│   └── Datos crudos desde fuentes de origen
│
├── Silver
│   └── Datos limpios y transformados
│
└── Gold
    └── Datos listos para consumo analítico
```

---

## ✅ Resultado esperado

Al finalizar el desafío, el entorno debería contar con:

- Un **Workspace configurado en Microsoft Fabric**.
- Un **Lakehouse creado dentro del Workspace**.
- Una estructura inicial que represente las capas **Bronze, Silver y Gold**.
- Un entorno preparado para comenzar los procesos de **ingesta y transformación de datos** en los siguientes desafíos.

---

## 📚 Recursos recomendados

- Documentación oficial de Microsoft Fabric
- Conceptos de Lakehouse
- Arquitectura Medallion

---

## 🚀 Próximo desafío

En el siguiente desafío se realizará la **conexión a fuentes de datos**, integrando **Azure SQL Database y Azure Blob Storage** al entorno creado.