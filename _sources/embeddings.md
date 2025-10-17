# Capítulo 2: **Embeddings satelitales: una nueva semántica del territorio**

## **Introducción conceptual**

En los últimos años, la inteligencia artificial ha permitido construir *modelos de representación del mundo* que *trascienden los píxeles y los
valores espectrales*. Los **embeddings** ---representaciones numéricas densas de información compleja--- constituyen una de las *innovaciones*
más profundas en la intersección entre **aprendizaje profundo y Observación de la Tierra (EO)**.\
En el dominio lingüístico, un **embedding** *transforma palabras en vectores* que capturan su *significado contextual*; del mismo modo, en el
dominio geoespacial, los **embeddings satelitales** *traducen la
información espectral, temporal y contextual de cada píxel o región* en
un *vector semántico que codifica patrones de superficie, contextos
ambientales y relaciones espaciales*. Luego, podemos decir que los *Embeddings satelitales: nos permiten explorar el campo de una semántica geoespacial aprendida*.

## **Introducción: del píxel al concepto**

Durante décadas, el análisis de imágenes satelitales se ha basado en valores radiométricos y en la interpretación de índices derivados (NDVI,
NDWI, NDBI, etc.), que reflejan *fenómenos biofísicos* como la vegetación, el agua o lo urbano.

Sin embargo, la revolución del **deep learning** ha transformado la forma en que representamos la información. Hoy, la pregunta ya no es "¿qué
valor tiene este píxel en la banda 4?", sino **"¿a qué se parece este píxel en términos de su significado latente?**".

Los *embeddings satelitales* representan ese salto conceptual: son una forma de **codificar semánticamente el territorio**.\
Cada píxel o región es proyectado a *un espacio vectorial de alta dimensión* donde la *distancia matemática* refleja *similitud contextual y
semántica*, *no solo espectral*. 

De este modo, el planeta *deja de ser una grilla de reflectancias* y *se convierte en un espacio continuo de conceptos aprendidos*.

## **Fundamento teórico: ¿Qué es un embedding?*

````{admonition} ¿Qué es un embedding?
:class: tip

En términos formales, un **embedding** es una función:

```{math}
f: X \rightarrow \mathbb{R}^n
```

donde $X$ representa un **conjunto de observaciones complejas** —imágenes multiespectrales, series temporales o escenas completas—,  
y $\mathbb{R}^n$ es un **espacio vectorial latente**.  
La función $f$ se *aprende a partir de grandes volúmenes de datos mediante redes neuronales profundas*.  

Su objetivo no es clasificar directamente, sino **aprender una representación comprimida y significativa** de los datos.

En el dominio de la **Observación de la Tierra (EO)**, esto significa que:

- Cada píxel o parche satelital se codifica en un vector de, por ejemplo, 256 dimensiones.  
- Las *relaciones espaciales y espectrales* se preservan de modo que píxeles *similares en contexto* quedan *cercanos en el espacio latente*.  
- Los *embeddings* permiten medir similitud coseno entre lugares, como se mide *similitud semántica* entre palabras en modelos como Word2Vec o BERT.
````

En el dominio de la **Observación de la Tierra (EO)**, esto significa que:

-   Cada píxel o parche satelital se codifica en un vector de, por ejemplo, 64 dimensiones

-   Las *relaciones espaciales y espectrales* se preservan de modo que píxeles *similares en contexto* quedan *cercanos en el espacio latente*.

-   Los *embeddings* permiten medir similitud coseno entre lugares, como se mide similitud semántica entre palabras en modelos como Word2Vec o BERT.

Esta idea, proveniente del procesamiento del lenguaje natural, encuentra en las imágenes satelitales una analogía poderosa:\
así como los modelos lingüísticos aprenden que *"rey" - "hombre" + "mujer" ≈ "reina"*, los modelos de EO aprenden que *"vegetación densa" - "verde" + "suelo desnudo" ≈ "zona urbana"*.

```{figure} imagenes/concepto.png
:name: fig-concepto
:width: 100%

concepto de embeddings en un espacio multidimensional
```

Un **campo de embedding** es la *matriz continua* o *“campo” de embeddings aprendidas*. Las imágenes de las colecciones de campos de embedding representan trayectorias espacio-temporales que abarcan un año completo y tienen 64 bandas (una para cada dimensión de incrustación).

```{figure} imagenes/embedding_field_1.jpg
:name: fig-embeddingfield1
:width: 80%

vector de incrustación n-dimensional muestreado de un campo de incrustación *embedding*
```

**3. Modelos fundacionales y embeddings satelitales**

Los **modelos fundacionales para Observación de la Tierra (FM4EO)** como *OneVision*, *Prithvi*, *AlphaHertz* o el *Satellite Embedding V1* de Google, fueron entrenados sobre millones de escenas multitemporales de Sentinel-2, Landsat y MODIS.\
Estos modelos aprenden a generar vectores invariantes a cambios atmosféricos, de estación o de sensor, capturando así *la esencia estadística del paisaje*.


````{admonition} ¿Qué es un embedding?
:class: tip

El dataset **`GOOGLE/SATELLITE_EMBEDDING/V1`**, disponible en *Google Earth Engine*, representa la **primera implementación global del paradigma de embeddings satelitales**: un **mapa latente del planeta** donde cada píxel, con resolución espacial de **10 metros**, está asociado a un **vector de 64 dimensiones** que codifica su **identidad semántica aprendida**.  

Cada una de esas 64 dimensiones sintetiza patrones espectrales, espaciales y contextuales extraídos mediante aprendizaje auto-supervisado, lo que permite comparar regiones por su *significado estadístico* más que por su mera reflectancia espectral.

```javascript
var embeddings = ee.ImageCollection('GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL');
var img = embeddings.first();         // o .mosaic() para el año filtrado
print('N° de bandas (dim del embedding):', img.bandNames().size());
```
````

## Estructura matemática y significado de la similitud

La **similitud coseno** se utiliza como métrica fundamental en este espacio latente.

Dado un vector de referencia $\mathbf{s}$ (por ejemplo, el promedio de los *embeddings* de un conjunto de polígonos de agua) y un vector de píxel $\mathbf{x}$, la similitud se define como:

$$
\text{sim}(\mathbf{s}, \mathbf{x}) = \frac{\mathbf{s} \cdot \mathbf{x}}{\|\mathbf{s}\| \, \|\mathbf{x}\|}
$$

**Este valor se reescala al intervalo [0,1], donde 1 indica máxima similitud semántica.**  
Lo notable es que esta similitud no depende de índices espectrales fijos, sino de representaciones *aprendidas* que capturan patrones espaciales, texturales y de contexto ambiental.

---

En otras palabras, dos píxeles o regiones son “similares” no porque compartan el mismo valor de reflectancia o NDVI, 
sino porque sus vectores latentes apuntan en direcciones próximas dentro de un espacio multidimensional de significado.  
Este enfoque habilita la **búsqueda semántica geoespacial**, donde el criterio de comparación es el *significado estadístico* aprendido por el modelo, y no un índice calculado manualmente.

En consecuencia:

-   Los *embeddings* permiten buscar por concepto ("lugares similares a este humedal") en lugar de por valor ("NDWI \> 0.4").

-   Cada comparación en este espacio vectorial actúa como un *razonamiento semántico* entre regiones.

# Hiperesfera unitaria y operaciones de normalización en embeddings

## 🔵 Definición

Una **hiperesfera unitaria** es la generalización de una esfera común a espacios de muchas dimensiones.  
En términos matemáticos, se define como el conjunto de todos los vectores cuya **norma (longitud)** es igual a 1:

$S^{n-1} = \{\, x \in \mathbb{R}^n \; | \; \|x\| = 1 \,\}$

Por ejemplo:

| Dimensión | Nombre geométrico | Ecuación | Representación |
|------------|------------------|-----------|----------------|
| 1D | Dos puntos (–1, +1) | $x^2 = 1$ | 🔹🔹 |
| 2D | Circunferencia unitaria | $x^2 + y^2 = 1$ | ⭕ |
| 3D | Esfera unitaria | $x^2 + y^2 + z^2 = 1$ | 🟢 |
| 64D | Hiperesfera unitaria | $x_1^2 + x_2^2 + ... + x_{64}^2 = 1$ | (no visualizable, pero análoga) |

---

## Interpretación en embeddings

Los **embeddings** (como los de *Satellite Embeddings V1*, *Prithvi* o *TerraMind*) representan entidades —palabras, píxeles, regiones o escenas— como vectores en un espacio latente de alta dimensión.

Antes de compararlos, los modelos suelen **normalizarlos** a norma 1:

$\hat{x} = \frac{x}{\|x\|}$

De este modo, todos los vectores se proyectan sobre la **superficie de la hiperesfera unitaria**.  
Así, su **ángulo relativo** (y no su magnitud) representa su similitud semántica.

$\text{similitud coseno}(x, y) = \hat{x} \cdot \hat{y} = \cos(\theta)$

---

### Operaciones que normalizan vectores al espacio de la hiperesfera unitaria

| Categoría | Operación o técnica | Descripción | Geometría subyacente |
|------------|---------------------|--------------|----------------------|
| **Métrica** | **Cosine similarity** | Mide el coseno del ángulo entre dos vectores. | Comparación angular en la hiperesfera. |
| **Métrica** | **Cosine distance** *(1 - cos)* | Evalúa disimilitud angular (más grande = más distintos). | Todos los vectores tienen norma 1. |
| **Métrica** | **Angular distance** | Calcula el ángulo directo $\arccos(\hat{x}\cdot\hat{y})$. | Distancia geodésica sobre la hiperesfera. |
| **Agrupamiento** | **Spherical k-means** | Variante del k-means que usa similitud coseno en lugar de distancia euclídea. | Clustering sobre la superficie de la hiperesfera. |
| **Aprendizaje contrastivo** | **InfoNCE / SimCLR / CLIP** | Pérdidas contrastivas que comparan pares de embeddings normalizados. | Proyección L2 → todos los vectores tienen norma 1. |
| **Aprendizaje contrastivo** | **NT-Xent Loss** | Pérdida usada en *self-supervised learning* para maximizar similitud angular. | Espacio latente esférico. |
| **Aprendizaje de métricas** | **Triplet Loss (Anchor–Positive–Negative)** | Obliga a que los embeddings similares estén más próximos que los distintos. | Distancias angulares en la hiperesfera. |
| **Aprendizaje de métricas** | **ArcFace / CosFace / SphereFace** | Modelos que aprenden en el espacio angular (común en reconocimiento facial). | Embeddings confinados a una hiperesfera unitaria. |
| **Modelos fundacionales EO** | **Prithvi, TerraMind, Satellite Embeddings V1** | Embeddings multiespectrales y temporales normalizados para similitud coseno. | Espacio semántico latente sobre una hiperesfera de 64D. |

---

### Interpretación geométrica

En la hiperesfera unitaria:

- Todos los vectores tienen la **misma longitud (1)**.  
- Solo importa su **dirección**, que define su posición sobre la superficie.  
- Dos vectores cercanos (pequeño ángulo) representan **fenómenos similares**.  
- Dos vectores ortogonales (90°) representan **fenómenos distintos o no relacionados**.

Así, la **distancia angular** se convierte en una medida directa de **similitud semántica o geofísica**.

---

### En resumen

> La **hiperesfera unitaria** es el espacio geométrico donde viven los **embeddings normalizados**.  
> Allí, las operaciones basadas en ángulo o coseno comparan significado, no magnitud.  
> En modelos fundacionales de Observación de la Tierra, esta geometría es la base de la **similitud coseno** y de todo el aprendizaje contrastivo que permite mapear el planeta en el espacio latente.


## **Más: Contexto y patrones espaciales en embeddings satelitales**

El *embedding* captura **contexto más que forma fina**, lo que lo hace especialmente útil para detectar patrones espaciales amplios y coherentes.  
A continuación se detallan ejemplos de tipologías geográficas y sugerencias prácticas de uso.

**Ejemplos de detección por similitud**

* 🌊 **Océano / grandes cuerpos de agua**
  - Muy distintivos y homogéneos.  
  - **Tip:** máscara *JRC Water* (occurrence ≥ 10–30%).

* 🏞️ **Lagos / embalses medianos–grandes**
  - Formas estables y contraste claro con tierra.  
  - **Tip:** *JRC Water* + picos locales sobre el mapa de similitud.

* 🌆 **Áreas urbanas densas (CBD, manzanas compactas)**
  - Textura “gruesa”, patrones de calles y edificios.  
  - **Tip:** NDBI alto, NDVI bajo para recortar candidatos.

* 🏭 **Zonas industriales / portuarias grandes**
  - Superficies duras, depósitos, muelles, contenedores.  
  - **Tip:** NDBI↑, NDVI↓, Sentinel-1 VV/VH moderado–alto.

* 🌾 **Mosaicos agrícolas extensos (parcelas, pivotes)**
  - Geometría repetitiva; muy “aprendible”.  
  - **Tip:** recortar a áreas rurales y usar época anual similar.

* ⛏️ **Canteras / minas a cielo abierto**
  - Texturas minerales, taludes, caminos internos.  
  - **Tip:** NDBI↑, NDVI↓, SWIR↑, S1 VV/VH↑.

* 🌳 **Bosques densos / masas forestales**
  - Textura homogénea y patrón regional.  
  - **Tip:** NDVI↑ para filtrar no-vegetación.

* 🧂 **Salinas / salares**
  - Reflectancia y textura características, grandes extensiones.  
  - **Tip:** SWIR/NIR peculiar; conviene recortar con máscara de suelo desnudo.

* 🐦 **Humedales extensos**
  - Mezcla agua-vegetación con patrón espacial distintivo.  
  - **Tip:** JRC (occurrence medio) + NDVI medio/alto.

* 🚗 **Infraestructura lineal grande (autopistas, aeropuertos)**
  - Linealidad clara a escala 10–20 m.  
  - **Tip:** detectar por similitud + postprocesar con filtros morfológicos.

* ⚡ **Parques eólicos / solares grandes**
  - Patrón repetitivo (aerogeneradores, filas de paneles).  
  - **Tip:** S1 ayuda (estructuras metálicas dispersas), NDBI↑, NDVI↓.

* 🏡 **Barrios privados / countries característicos**
  - Huella y traza interna repetida, lagunas artificiales.  
  - **Tip:** recortar con urbano (NDBI↑) y usar varias muestras.

* 🏖️ **Playas / dunas extensas**
  - Textura y tonalidad de arena, bordes costeros.  
  - **Tip:** excluir agua con JRC y vegetación con NDVI↓.

---

# 🛰️ Consideraciones metodológicas y recomendaciones prácticas

El dataset **`GOOGLE/SATELLITE_EMBEDDING/V1`** constituye una representación semántica de la superficie terrestre aprendida a partir de millones de escenas multitemporales, pero su naturaleza **latente y abstracta** impone ciertas limitaciones operativas.  
En primer lugar, es importante reconocer que estos embeddings **no codifican objetos discretos ni detalles finos** —como vehículos, edificaciones individuales o elementos de pequeña escala—, ya que su resolución espacial de **10 metros** y su entrenamiento auto-supervisado están orientados a **capturar patrones espaciales amplios, contextos ambientales y estructuras territoriales coherentes**.  

Por esta razón, su mayor potencial se manifiesta en la **detección y comparación de tipologías geográficas o paisajísticas**, tales como zonas agrícolas, humedales, cuerpos de agua, áreas urbanas densas, salinas o minas a cielo abierto.  
Estas categorías presentan **huellas espaciales y texturales persistentes** que el modelo logra representar en su espacio vectorial de 64 dimensiones.

---

## 🌐 Integración con índices espectrales y capas complementarias

Un aspecto metodológico crucial consiste en **combinar los embeddings con indicadores derivados** (espectrales o radar) que aportan *atributos físicos o biofísicos interpretables*.  
La **semántica latente** del embedding debe complementarse con información **radiométrica y temática explícita**, de modo que el análisis se apoye tanto en patrones aprendidos como en métricas observables.

Entre las estrategias más efectivas se encuentran:

- **Uso de índices ópticos y radar**: NDVI, NDBI, BSI, NDWI, así como coeficientes VV/VH de Sentinel-1. Estos índices permiten refinar la interpretación del embedding y aislar falsas similitudes.  
  Por ejemplo, la detección de zonas industriales o ladrilleras se beneficia de un umbral bajo de NDVI y alto de NDBI, mientras que el radar ayuda a discriminar superficies rugosas o metálicas.

- **Máscaras temáticas auxiliares**: capas como *Dynamic World* (clasificación semántica global multitemporal), *Global Surface Water* (JRC), *ESA WorldCover* o *Copernicus Global Land Cover* son fundamentales para acotar la búsqueda o descartar clases irrelevantes (por ejemplo, excluir el agua antes de analizar áreas urbanas).

- **Postprocesamiento con reglas espaciales**: filtros morfológicos, análisis de conectividad y agregaciones por tamaño de polígono permiten eliminar “ruido” o zonas ambiguas.  

En síntesis, los embeddings deben entenderse como un **componente de una arquitectura analítica híbrida**, donde el aprendizaje profundo se articula con el conocimiento geográfico, físico y contextual.

---

## 🧩 Usos avanzados de embeddings satelitales

Los *embeddings satelitales* son una herramienta versátil que puede integrarse en múltiples flujos de trabajo de análisis geoespacial, tanto supervisados como no supervisados.  
A continuación se resumen los principales enfoques de aplicación:

### 🔹 1. Segmentación semántica

Los vectores de embeddings pueden servir como **atributos de entrada** para algoritmos de segmentación (por ejemplo, *k-means*, *Mean-Shift*, *Spectral Clustering* o *DBSCAN*), permitiendo agrupar píxeles o regiones con similaridad semántica en lugar de simple proximidad espectral.  
Esta estrategia es útil para generar **mapas de regiones homogéneas** sin necesidad de etiquetas previas, abriendo la posibilidad de descubrir patrones emergentes.

### 🔹 2. Clasificación supervisada

Los embeddings actúan como una **capa intermedia de alto nivel** sobre la cual puede entrenarse un clasificador tradicional (Random Forest, SVM, redes neuronales ligeras) utilizando muestras de entrenamiento definidas por el analista.  
Esto reduce el ruido, mejora la generalización y permite transferir conocimiento entre regiones o épocas distintas.  
Ejemplos prácticos son la detección de **ladrilleras, cultivos específicos o áreas degradadas**, donde la similitud latente se combina con información espectral y radar para obtener una discriminación más precisa.

### 🔹 3. Búsqueda semántica y análisis de similitud

Uno de los usos más innovadores es la **búsqueda por similitud** (“find places like this”), en la cual un vector de referencia —derivado de un polígono o muestra representativa— se compara con el embedding completo del territorio mediante la **similitud coseno**.  
Este procedimiento permite **detectar lugares análogos en su estructura semántica**, facilitando la identificación de ambientes similares, expansión de cultivos o detección de anomalías espaciales.

### 🔹 4. Entrenamiento no supervisado y exploración de patrones

A partir del espacio latente generado por los embeddings, pueden aplicarse técnicas de **reducción de dimensionalidad** (PCA, t-SNE, UMAP) para visualizar y explorar relaciones entre regiones.  
Estos análisis permiten **descubrir clústeres naturales** que revelan tipologías de paisaje, transiciones ecológicas o gradientes urbanos-rurales sin requerir etiquetas previas.

---

### 🧭 Síntesis epistemológica

En definitiva, los embeddings satelitales constituyen un **nuevo lenguaje estadístico del territorio**, en el que cada vector representa una *unidad mínima de conocimiento geoespacial*.  
El modelo **no “ve” colores o bandas**, sino que **razona en términos de patrones latentes aprendidos** a partir de la co-ocurrencia espacial, temporal y contextual de los datos.  

Sin embargo, la potencia de esta representación no radica en sustituir los índices o modelos clásicos, sino en **integrarse con ellos**.  
La sinergia entre *semántica latente (embeddings)* y *semántica explícita (índices, capas temáticas, reglas físicas)* define el camino hacia una **GeoIA robusta, explicativa y transferible** para la gestión del territorio, la observación ambiental y la toma de decisiones basadas en evidencia.

