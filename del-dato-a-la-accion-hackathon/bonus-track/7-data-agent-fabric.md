# 🌟 Bonus Track – Tu primer Data Agent en Microsoft Fabric

## 🎯 Objetivo

Crear un **Data Agent** en Microsoft Fabric que permita a los usuarios hacer preguntas en **lenguaje natural** sobre los datos del Modelo Semántico Gold, obteniendo respuestas generadas automáticamente por inteligencia artificial sin necesidad de escribir consultas DAX o SQL.

---

## 🧩 Contexto

Microsoft Fabric incluye capacidades de **IA generativa** integradas directamente en la plataforma. Una de las más poderosas es el **Data Agent** : un agente conversacional que utiliza un Large Language Model (LLM) para traducir preguntas en lenguaje natural a consultas sobre tus datos y devolver respuestas comprensibles para el usuario de negocio.

El Data Agent se conecta a fuentes de datos dentro de Fabric (Lakehouses, Warehouses o Modelos Semánticos) y puede ser expuesto como:

- Un endpoint de API consumible desde aplicaciones externas.
- **Un copiloto conversacional integrado en Microsoft Teams**.

Esto representa el cierre natural del ciclo **del dato a la acción**: los datos que fueron ingestados, transformados y modelados a lo largo del hackathon ahora pueden ser consultados de forma conversacional por cualquier usuario del negocio.

| Capacidad del Data Agent     | Descripción                                                                 |
|------------------------------|-----------------------------------------------------------------------------|
| Lenguaje natural → consulta  | Traduce preguntas en texto a DAX o SQL automáticamente                     |
| Contexto del modelo          | Entiende las relaciones y medidas definidas en el Modelo Semántico         |
| Respuesta explicada          | Devuelve el resultado junto con una explicación en lenguaje natural         |
| Refinamiento iterativo       | El usuario puede hacer preguntas de seguimiento para profundizar el análisis|

---

## 📋 Desafío

Crear y configurar un **Data Agent** en Microsoft Fabric conectado al Modelo Semántico Gold:

**Creación del Data Agent:**
- Desde el workspace de Fabric, crear un nuevo ítem de tipo **Data Agent**.
- Seleccionar el **Modelo Semántico Gold** como fuente de datos del agente.
- Revisar el esquema de tablas y medidas que el agente utilizará para responder preguntas.

**Configuración y personalización:**
- Agregar **instrucciones de contexto** al agente para que comprenda el dominio de negocio (por ejemplo: *"Este modelo contiene datos de ventas de una empresa retail. La tabla de hechos principal es fact_ventas."*).
- Definir **ejemplos de preguntas y respuestas esperadas** (few-shot examples) para mejorar la precisión del agente.
- Configurar qué tablas y medidas el agente tiene permitido consultar.

**Prueba conversacional:**
- Realizar al menos **5 preguntas de negocio** al agente en lenguaje natural. Ejemplos:
  - *"¿Cuál fue el total de ventas del último trimestre?"*
  - *"¿Qué producto tuvo el mayor ticket promedio en 2024?"*
  - *"Muéstrame las ventas por región ordenadas de mayor a menor."*
  - *"¿Cuántos clientes nuevos tuvimos en el primer semestre?"*
  - *"Compara las ventas de este año con las del año anterior."*
- Verificar que las respuestas son correctas contrastándolas con el informe de Power BI creado previamente.


---

## 🧠 Consideraciones

- El Data Agent requiere que la **capacidad de Fabric tenga habilitada la IA generativa** (Fabric Copilot). Verificar con el administrador del tenant que esta funcionalidad esté activa.
- La calidad de las respuestas del agente depende directamente de la calidad del Modelo Semántico: nombres descriptivos de tablas y columnas, medidas bien documentadas y relaciones correctamente definidas mejoran notablemente los resultados.
- Las **instrucciones de contexto** son clave: describir el dominio del negocio, las entidades principales y cualquier regla de negocio especial ayuda al LLM a generar consultas más precisas.
- El agente no reemplaza la validación humana: siempre contrastar respuestas críticas con los datos fuente.
- Para escenarios de producción, evaluar el uso de **Microsoft Copilot Studio** para construir agentes más avanzados sobre los datos de Fabric con flujos de conversación personalizados.

---

## 🏗 Arquitectura esperada

```
Modelo_Semantico_Gold
├── Relaciones (Star Schema)
└── Medidas DAX
         │
         │  (Data Agent – IA Generativa)
         ▼
Data_Agent_Fabric
├── Instrucciones de contexto   (dominio de negocio)
├── Few-shot examples           (preguntas y respuestas de ejemplo)
└── Interfaz conversacional     (chat en lenguaje natural)
         │
         ▼
Usuario de negocio
└── Pregunta en lenguaje natural → Respuesta con datos + explicación
```

---

## ✅ Resultado esperado

Al finalizar el bonus track, el entorno debería contar con:

- Un **Data Agent** publicado en el workspace de Fabric, conectado al Modelo Semántico Gold.
- Instrucciones de contexto configuradas que describan el dominio de negocio.
- Al menos **3 ejemplos few-shot** definidos para mejorar la precisión del agente.
- Evidencia de al menos **5 preguntas respondidas correctamente** por el agente en lenguaje natural.
- Respuestas validadas contra el informe de Power BI del Bloque 3.

---

> 💡 **Reflexión final:** Al completar este bonus track, has recorrido el camino completo **del dato a la acción**: desde la ingesta cruda en Bronze, pasando por la transformación en Silver y Gold, la modelización semántica y la visualización en Power BI, hasta llegar a un agente conversacional que democratiza el acceso a los datos para cualquier usuario del negocio. ¡Eso es una arquitectura analítica moderna de extremo a extremo!
