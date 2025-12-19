# **Geosemántica y GeoAI**

---


# Agradecimientos

Este libro y recursos didácticos han sido desarrollados en el marco del grupo de investigación **04/F023: Tecnologías de Datos Espaciales, Visualización y Realidad Virtual**, Facultad de Informática, Universidad Nacional del Comahue.


# Sobre este libro

El objetivo de este libro digital es impulsar el desarrollo de capacidades en la aplicación de técnicas de geoAI con información geoespacial, principalmente empleando conceptos de Observacion de la Tierra y Ciencia de Datos Geoespaciales. 

````{admonition} Objetivo
:class: tip 
Propiciar la incorporación enfoques innovadores y paradigmas de ciencias de datos para maximizar la gestión, análisis y aprovechamiento de la información geoespacial, fomentando el uso de tecnologías emergentes, inteligencia artificial y modelos analíticos avanzados.

````

````{admonition} Producción del libro y de los videos
:class: note

```{admonition} Libro (Jupyter Book)
:class: tip
Este libro interactivo digital está siendo desarrollado con [**Jupyter Book**](https://jupyterbook.org) y tendrá ISBN tramitado por alguna institución estatal.
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