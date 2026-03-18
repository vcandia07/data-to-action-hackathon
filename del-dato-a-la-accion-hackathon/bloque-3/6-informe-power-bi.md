# 📊 Desafío 6 – Creando un informe en Power BI

## 🎯 Objetivo

Construir un **informe interactivo en Power BI** conectado al Modelo Semántico creado en el desafío anterior, que permita a los usuarios de negocio explorar y analizar los datos de forma visual e intuitiva.

---

## 🧩 Contexto

Con el Modelo Semántico publicado en el workspace de Microsoft Fabric, el paso final de la arquitectura analítica es exponer los datos a través de un **informe de Power BI** que comunique los insights de manera clara y accionable.

Power BI permite construir informes compuestos por **páginas**, cada una con un conjunto de **visualizaciones** (gráficos, tablas, tarjetas, mapas, etc.) que interactúan entre sí gracias a los filtros cruzados definidos en el modelo semántico.

Un buen informe no solo muestra datos, sino que **cuenta una historia**: guía al usuario desde una visión general hasta el detalle, facilitando la toma de decisiones basada en datos.

Principios clave de un informe efectivo:

| Principio         | Descripción                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| Claridad          | Cada visual comunica un único mensaje con el menor ruido visual posible     |
| Jerarquía visual  | Los KPIs más importantes ocupan un lugar destacado en el informe            |
| Interactividad    | Filtros y segmentaciones permiten al usuario explorar los datos por sí mismo|
| Consistencia      | Paleta de colores, tipografía y estilo coherentes en todo el informe        |

---

## 📋 Desafío

Crear un **informe de Power BI** en Microsoft Fabric conectado al Modelo Semántico Gold siguiendo estos pasos:

**Conexión al Modelo Semántico:**
- Desde el workspace de Fabric, crear un nuevo informe usando el Modelo Semántico construido en el desafío anterior.
- Verificar que todas las tablas, columnas y medidas DAX están disponibles en el panel de campos.

**Diseño del informe:**
- Estructurar el informe en al menos **2 páginas**:
  - **Página 1 – Resumen Ejecutivo**: KPIs clave mediante tarjetas (cards) y un gráfico de tendencia temporal.
  - **Página 2 – Análisis Detallado**: tablas, gráficos de barras/columnas y segmentaciones por dimensiones (cliente, producto, periodo).
- Agregar al menos un **segmentador (slicer)** para filtrar los datos por una dimensión relevante (por ejemplo, año, categoría de producto o región).
- Incluir un **gráfico de tendencia** que muestre la evolución de una métrica clave a lo largo del tiempo usando la dimensión de fecha.

**Interactividad y formato:**
- Configurar la **interacción entre visualizaciones** (filtro cruzado o resaltado).
- Aplicar un **tema visual consistente** (colores corporativos o uno de los temas predefinidos de Power BI).
- Agregar **títulos descriptivos** a cada visual y a cada página del informe.

**Publicación:**
- Guardar y publicar el informe en el workspace de Fabric.
- Verificar que el informe es accesible para otros usuarios del workspace.

---

## 🧠 Consideraciones

- Al crear el informe desde el workspace de Fabric, seleccionar la opción *"New report"* directamente sobre el Modelo Semántico para mantener la conexión **DirectLake** activa.
- Preferir el uso de **medidas DAX** ya definidas en el modelo por sobre cálculos ad hoc en el informe, para garantizar consistencia entre reportes.
- Evitar sobrecargar una sola página con demasiadas visualizaciones; distribuir el contenido en páginas temáticas mejora la legibilidad.
- Usar **marcadores (bookmarks)** si se desean vistas pre-configuradas o una navegación guiada dentro del informe.
- Habilitar **tooltips personalizados** en los gráficos para ofrecer contexto adicional sin saturar la vista principal.
- Verificar el comportamiento del informe en la vista **móvil** si se espera consumo desde dispositivos móviles.

---

## 🏗 Arquitectura esperada

```
Modelo_Semantico_Gold
├── Relaciones (Star Schema)
└── Medidas DAX
         │
         │  (Power BI Report)
         ▼
Informe_Power_BI
├── Página 1 – Resumen Ejecutivo
│   ├── Tarjetas de KPIs         (Total Ventas, Ticket Promedio, etc.)
│   └── Gráfico de tendencia     (evolución temporal)
│
└── Página 2 – Análisis Detallado
    ├── Segmentadores (slicers)  (dimensiones: fecha, producto, cliente)
    ├── Gráfico de barras/columnas
    └── Tabla de detalle
```

---

## ✅ Resultado esperado

Al finalizar el desafío, el entorno debería contar con:

- Un **informe de Power BI** publicado en el workspace de Fabric, conectado al Modelo Semántico Gold.
- Al menos **2 páginas** con visualizaciones interactivas y títulos descriptivos.
- **KPIs clave** visibles en la página de resumen ejecutivo mediante tarjetas.
- Al menos **1 segmentador** funcional que filtre las visualizaciones del informe.
- Un **gráfico de tendencia temporal** que refleje la evolución de una métrica a lo largo del tiempo.
- Tema visual consistente aplicado en todo el informe.
