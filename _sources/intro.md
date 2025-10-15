# **Geosemántica y Embeddings**


## **Introducción a Geosemántica**

La **geosemántica** estudia cómo el significado (semántica) se representa y se procesa en relación con el espacio geográfico.  
En este marco, los *embeddings* pueden considerarse estructuras semánticas aprendidas, ya que transforman las observaciones geoespaciales en vectores de alta dimensionalidad que **preservan relaciones de similitud semántica** entre regiones del territorio.

Por ejemplo, dos píxeles correspondientes a lagunas diferentes, pero con firmas espectrales y contextos similares, ocuparán posiciones cercanas en el espacio vectorial.  
Del mismo modo, zonas urbanas o agrícolas formarán clústeres en ese mismo espacio semántico.

---

## **Los embeddings como fenómeno geosemántico**


Los *embeddings satelitales* constituyen una nueva frontera en la representación digital del territorio.  
En lugar de describir el espacio mediante variables físicas aisladas —como reflectancia, temperatura o índices espectrales—, los embeddings lo hacen a través de **vectores de significado latente** aprendidos por modelos de *deep learning* sobre grandes volúmenes de datos satelitales.

Estos vectores condensan relaciones espectrales, espaciales y temporales complejas, y por ello pueden interpretarse como una **manifestación de la geosemántica**: una forma de codificar el *significado geográfico* directamente en el espacio matemático.

---

## **Los tres niveles de la geosemántica**

La geosemántica contemporánea puede dividirse, de manera general, en tres niveles complementarios:

| Nivel | Descripción | Ejemplo |
|-------|--------------|----------|
| **Formal o ontológico** | Basado en vocabularios controlados, ontologías y modelos conceptuales normalizados (p. ej. ISO 19150, INSPIRE, Catálogo de Objetos Geográficos de IDERA). | Ontologías de objetos geográficos, taxonomías de coberturas. |
| **Lingüístico o textual** | Derivado del análisis de textos, descripciones o metadatos. | Extracción de entidades geográficas en corpus de texto. |
| **Estadístico o latente** | Aprendido a partir de datos numéricos o imágenes mediante técnicas de aprendizaje automático. | Embeddings espectrales, espaciales o espacio-temporales. |

Los *embeddings satelitales* pertenecen claramente al **tercer nivel**, el de la **semántica estadística o latente**, donde el significado no se define explícitamente, sino que **emerge de los patrones de correlación y coocurrencia** observados en los datos.

---

## **La semántica estadística como fundamento**

El concepto de **semántica estadística** se origina en la lingüística computacional a mediados del siglo XX.  
Autores como **Zellig Harris (1954)** y **J. R. Firth (1957)** formularon la hipótesis de distribución:  

> *“El significado de una palabra puede inferirse de los contextos en los que aparece.”*

En el dominio geoespacial, esta idea se traduce en una analogía poderosa:

> *El significado de un píxel o región puede inferirse del contexto espectral, temporal y espacial que lo rodea.*

Los modelos modernos de embeddings —como **Google Satellite Embedding V1**, **AlphaHertz** o **OneVision**— aprenden precisamente ese contexto.  
Cada vector no representa ya un valor físico, sino una **posición semántica** en un espacio n-dimensional que captura similitudes de paisaje, uso, textura y dinámica ambiental.


La noción de semántica estadística tiene su origen en la lingüística computacional, 
particularmente en los trabajos pioneros de {cite}`harris1954distributional` y {cite}`firth1957linguistic`.  
En el campo geoespacial, esta idea ha sido retomada recientemente por {cite}`goodchild2021semantic` 
para conceptualizar una “geografía semántica” de la Tierra.  

Los modelos de representación latente basados en observación satelital, 
como {cite}`googleresearch2024satellite`, materializan esa visión en la práctica contemporánea.  
En la comunidad latinoamericana, enfoques emergentes como {cite}`montero2025geoia` 
proponen una integración conceptual entre geosemántica y aprendizaje automático.

---

# **Hacia una geosemántica estadística**

Podemos hablar entonces de un nuevo subcampo: la **geosemántica estadística**, también denominada **semántica latente geoespacial**.  
En este marco:
- Los *embeddings* son las **unidades básicas de significado**.  
- Las relaciones de **similitud coseno** sustituyen las jerarquías ontológicas.  
- Las operaciones algebraicas (distancias, agrupamientos, proyecciones) permiten **razonar cuantitativamente sobre el significado del territorio**.

Desde esta perspectiva, un píxel deja de ser una muestra de reflectancia: se convierte en un **vector de sentido geográfico**.  
El análisis del territorio pasa de lo físico a lo semántico.

---

# **Comparación epistemológica**

| Enfoque | Naturaleza del significado | Mecanismo de inferencia |
|----------|-----------------------------|--------------------------|
| **Geosemántica clásica** | Definido explícitamente por expertos o estándares (ontologías, taxonomías). | Declarativo (conceptos definidos manualmente). |
| **Geosemántica estadística** | Aprendido implícitamente a partir de correlaciones en datos geoespaciales. | Inductivo (patrones descubiertos por aprendizaje automático). |

Ambos enfoques son complementarios:  
- La **geosemántica formal** proporciona interpretabilidad y trazabilidad.  
- La **geosemántica estadística** aporta capacidad de descubrimiento y generalización.

---

# **Implicaciones científicas y aplicadas**

Los embeddings permiten crear **espacios semánticos del planeta**, donde cada vector describe el “contexto” de un punto de la superficie terrestre.  
En este sentido, constituyen una infraestructura cognitiva que:
- Facilita búsquedas semánticas (*“lugares similares a estos humedales”*).  
- Permite detectar cambios contextuales (*“esta región ha derivado hacia la semántica de cultivos”*).  
- Mejora la interoperabilidad entre datos físicos y conceptuales.  
- Integra la visión por computadora con la ontología geográfica.

Estas capacidades abren el camino hacia una **GeoIA semántica**, capaz de interpretar, clasificar y predecir fenómenos territoriales desde un marco conceptual y no meramente espectral.

---

# **Conclusión**

En conclusión, los embeddings constituyen una expresión avanzada del campo de la geosemántica, específicamente dentro de la **semántica estadística geoespacial**.  
A través de ellos, el territorio adquiere una representación vectorial en la que el significado se **aprende de los datos** y **se mide como proximidad matemática**.

Este paradigma redefine la forma de hacer cartografía y análisis espacial:  
ya no se trata solo de ver el territorio, sino de **comprender su significado latente** en el espacio vectorial del conocimiento geográfico.

---



# Agradecimientos

Este libro y recursos didácticos han sido desarrollados en el marco del grupo de investigación **04/F023: Tecnologías de Datos Espaciales, Visualización y Realidad Virtual**, Facultad de Informática, Universidad Nacional del Comahue.


# Sobre este libro

El objetivo de este libro digital es impulsar el desarrollo de capacidades en la aplicación de técnicas de geoAI con información geoespacial, principalmente empleando imágenes satelitales disponibles en Google Earth Engine. 

````{admonition} Objetivo
:class: tip 
Propiciar la incorporación enfoques innovadores y paradigmas de ciencias de datos para maximizar la gestión, análisis y aprovechamiento de la información geoespacial, fomentando el uso de tecnologías emergentes, inteligencia artificial y modelos analíticos avanzados.

````

````{admonition} Producción del libro y de los videos
:class: note

```{admonition} Libro (Jupyter Book)
:class: tip
Este libro interactivo digital está siendo desarrollado con [**Jupyter Book**](https://jupyterbook.org) y tendrá ISBN tramitado por IDERA-IGN.
```

```{admonition} Videos
:class: tip
Los videos asociados a los distintos capítulos se produjeron con componentes de **Adobe Creative Cloud** {cite}`adobe-creative-cloud-2025` —**After Effects** {cite}`adobe-after-effects-2025`, **Illustrator** {cite}`adobe-illustrator-2025`, **Audition** {cite}`adobe-audition-2025` y **Media Encoder** {cite}`adobe-media-encoder-2025`—: **After Effects** para *animación* y *composición de gráficos en movimiento*; **Illustrator** para la *preparación de arte vectorial*; **Audition** para la *edición y mezcla de audio*; y **Media Encoder** para la *transcodificación* y *publicación de los másteres finales*. Adicionalmente, se utilizó **Adobe Stock** {cite}`adobe-stock-2025`; no obstante, una parte sustancial del trabajo consistió en la *adaptación de imágenes*, la *generación de nuevas* a partir de *material base* y la *integración de diversos recursos* durante la *edición de video*.
```

````


# Requerimientos

Para poder reproducir los ejemplos prácticos y aprovechar los contenidos de este libro, es necesario contar con una cuenta en **Google Earth Engine (GEE)**. El registro es gratuito y se realiza en línea mediante una cuenta de Google, lo que habilita el acceso inmediato a un extenso catálogo de imágenes satelitales y productos derivados, así como a la infraestructura de cómputo en la nube que distingue a esta plataforma.

Es importante señalar que el acceso **gratuito** está disponible para fines **académicos, educativos, de investigación y de desarrollo no comercial**, y resulta suficiente para todas las actividades propuestas en este libro. En este modo, estudiantes, docentes y profesionales pueden explorar datos globales, ejecutar algoritmos avanzados de análisis geoespacial y descubrir el potencial de la **GeoAI aplicada a la observación de la Tierra**, sin necesidad de equipamiento especializado.

Cuando el uso de la plataforma se orienta a fines **comerciales o productivos**, GEE requiere una **licencia empresarial de pago**, que se gestiona a través de los servicios de Google Cloud o del programa **Earth Engine for Business**. Esta modalidad ofrece soporte extendido y mayores garantías de servicio para instituciones, gobiernos y compañías que dependen de un uso intensivo en entornos de producción.

En síntesis, la **cuenta gratuita es suficiente para el aprendizaje y la investigación**, y constituye el camino recomendado para iniciarse en el mundo del análisis satelital con GEE.

Una vez que cuentes con tu cuenta en GEE, podrás utilizar el siguiente enlace para explorar el repositorio público de código en JavaScript:  
🔗 [Repositorio público de IDERA en GEE](https://code.earthengine.google.com/?accept_repo=users%2Fcdg-idera%2Fgee)

Además, para complementar la programación en JavaScript en el Code Editor de GEE, utilizaremos **Google Colab** para ejecutar código **Python**. Allí mostraremos, entre otras cosas, cómo *generar ejemplos sintéticos* y *visualizar árboles de decisión* a partir de archivos .dot exportados desde GEE, ampliando así las posibilidades de análisis y documentación de los resultados.

En síntesis, una cuenta gratuita de GEE es suficiente para el aprendizaje y la investigación, y constituye el camino recomendado para iniciarse en el mundo del análisis satelital con GEE.

# Tabla de Contenidos

```{tableofcontents}
```