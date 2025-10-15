# FM4EO

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

\[
\text{Datos crudos} \xrightarrow[\text{auto-supervisión}]{\text{Foundation Model}} \text{Embeddings} \xrightarrow[\text{transferencia}]{\text{Fine-tuning / Similaridad}} \text{Tareas downstream}
\]

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
