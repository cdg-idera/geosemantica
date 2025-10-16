# Capítulo 1: Modelos Fundacionales en Observación de la Tierra (FM4EO)

Los modelos **OneVision**, **Prithvi** y **AlphaHertz** constituyen ejemplos destacados de **modelos fundacionales (foundational models)** aplicados al campo de la **Observación de la Tierra (EO, Earth Observation)**.

---

## ¿Qué es un modelo fundacional?

Un **modelo fundacional** (*foundational model*) es aquel que:
- Se **entrena a gran escala** (en millones de imágenes o señales).  
- Aprende **representaciones generales** (no una tarea específica).  
- Luego puede **ajustarse o especializarse (fine-tuning)** para tareas concretas: clasificación, segmentación, detección de cambios, etc.

En el contexto de la EO, estos modelos aprenden **patrones espaciales, espectrales y temporales** a partir de datos satelitales (Sentinel, Landsat, MODIS, etc.), convirtiéndose en infraestructuras de inteligencia geoespacial reutilizables.

---

## Ejemplos destacados

### 🛰️ OneVision (Google Research, 2024)
- **Tipo:** Modelo fundacional de visión satelital global.  
- **Entrenamiento:** Billones de píxeles de imágenes ópticas y radar.  
- **Capacidades:** Embeddings multiespectrales y multimodales.  
- **Usos:** Clasificación de cobertura, detección de cambios, segmentación.  
- **Institución:** Google Research.

### 🌎 Prithvi (NASA–IBM, 2024–2025)
- **Tipo:** Modelo fundacional para ciencia climática.  
- **Entrenamiento:** Petabytes de datos satelitales NASA (MODIS, VIIRS, Landsat).  
- **Capacidades:** Fine-tuning para incendios, sequías e inundaciones.  
- **Usos:** Modelos base para investigación ambiental y climática.  
- **Institución:** NASA–IBM.

### 🌐 AlphaHertz (Up42, 2024)
- **Tipo:** Modelo fundacional geoespacial.  
- **Entrenamiento:** Series multitemporales Sentinel-1 y Sentinel-2.  
- **Capacidades:** Embeddings satelitales para búsqueda por similitud semántica.  
- **Usos:** Detección de patrones espaciales y análisis multitemporal.  
- **Institución:** Up42.

---

## 📊 Tabla comparativa de modelos fundacionales EO

| Modelo       | Institución        | Año | Tipo de Datos              | Capacidades principales                                           | Aplicaciones principales                                      |
|---------------|--------------------|-----|-----------------------------|------------------------------------------------------------------|---------------------------------------------------------------|
| **OneVision** | Google Research    | 2024 | Ópticos + Radar (globales) | Embeddings multimodales; visión satelital global                 | Clasificación, detección de cambios, segmentación             |
| **Prithvi**   | NASA–IBM           | 2024–2025 | MODIS, VIIRS, Landsat       | Fine-tuning para eventos climáticos                              | Incendios, sequías, inundaciones, predicción ambiental         |
| **AlphaHertz**| Up42               | 2024 | Sentinel-1 y Sentinel-2     | Embeddings geosemánticos; búsqueda por similitud                 | Análisis multitemporal, patrones espaciales, búsqueda semántica |

---

## 📘 En resumen académico

> **OneVision (Google Research), Prithvi (NASA–IBM) y AlphaHertz (Up42)** son efectivamente **modelos fundacionales para Observación de la Tierra**, entrenados a gran escala sobre datos satelitales multimodales, diseñados para servir como base adaptable a múltiples tareas geoespaciales mediante *fine-tuning* o *transfer learning*.


## 🧠 1. Definición relacional básica

Los **embeddings** son *productos intermedios* o *representaciones aprendidas* dentro de un **modelo fundacional** (*foundation model*).  
Podemos decir que:

> 🔹 **Un modelo fundacional aprende embeddings universales del mundo.**

Dicho de otro modo:

- Los **embeddings** son los vectores numéricos que capturan el significado latente de los datos.  
- Los **modelos fundacionales** son las arquitecturas de aprendizaje profundo que aprenden, optimizan y generalizan esas representaciones a gran escala.

---

## 🧩 2. Diferencia estructural y funcional

| **Aspecto** | **Embeddings** | **Modelos Fundacionales (FM)** |
|--------------|----------------|--------------------------------|
| **Nivel ontológico** | Representaciones latentes del conocimiento (vectores) | Sistemas que aprenden y generan esas representaciones |
| **Escala** | Un embedding describe un píxel, palabra o entidad | Un FM abarca el universo de entrenamiento multimodal |
| **Propósito** | Capturar similitud semántica en un espacio vectorial | Aprender tareas generales (clasificación, descripción, predicción, generación) |
| **Analogía** | El ADN semántico del dato | El organismo inteligente que produce y utiliza ese ADN |
| **Ejemplo** | `GOOGLE/SATELLITE_EMBEDDING/V1` (vectores de 256D por píxel) | *OneVision*, *Prithvi*, *AlphaHertz*, *SatCLIP* (modelos fundacionales EO) |

---

## 🌍 3. En el contexto de la Observación de la Tierra (EO)

Los **Foundation Models for Earth Observation (FM4EO)** —como *OneVision*, *Prithvi* o *AlphaHertz*— se entrenan sobre billones de píxeles multiespectrales y multitemporales.  
Durante este proceso, aprenden a representar patrones espaciales, temporales y contextuales en **espacios latentes de alta dimensión**: los *embeddings satelitales*.

El **embedding satelital** es, por tanto, el *lenguaje interno* del modelo fundacional:  
cada vector resume la **firma semántica** del lugar en términos que la red neuronal entiende y puede reutilizar.

Por ejemplo:

- Un **píxel de agua** y otro de **sombra montañosa** pueden tener reflectancias similares,  
  pero **embeddings distintos**, porque el modelo aprendió que uno pertenece a un lago y el otro a una ladera.  
- Un **modelo fundacional** generaliza esa distinción a escala global,  
  y luego **emite embeddings consistentes** que otros modelos o tareas pueden usar (clasificación, segmentación, detección de cambio, etc.).

---

## ⚙️ 4. Relación jerárquica: el embedding como capa latente

En un esquema conceptual:

$$
\text{Datos crudos} \xrightarrow[\text{auto-supervisión}]{\text{Foundation Model}} \text{Embeddings} \xrightarrow[\text{transferencia}]{\text{Fine-tuning / Similaridad}} \text{Tareas downstream}
$$


- Los **datos crudos** (imágenes ópticas, radar, multitemporales) son la entrada.  
- El **modelo fundacional** actúa como codificador semántico auto-supervisado.  
- El resultado son **embeddings universales**, reutilizables para múltiples tareas.  
- A partir de ellos, se puede hacer *fine-tuning*, búsqueda por similitud, *clustering* o clasificación supervisada.

En este sentido, los **embeddings** son la **capa intermedia universal** entre la percepción y el razonamiento.

---

## 🧭 5. Perspectiva epistemológica

Desde una mirada científica más profunda:

> Los **embeddings** son la *epistemología interna* de los modelos fundacionales.

Representan el conocimiento condensado que el modelo adquiere sobre la **estructura estadística del mundo**.  
El paso desde “imagen multibanda” hacia “vector semántico” implica un cambio de paradigma:  
de una **ontología física** (valores radiométricos) a una **ontología estadístico-semántica** (vectores de significado).

Así, los **modelos fundacionales** son, en última instancia, *modelos de significado*,  
y los **embeddings**, las *unidades mínimas de sentido* en ese lenguaje.
