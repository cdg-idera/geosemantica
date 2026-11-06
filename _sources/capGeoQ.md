# **GeoQ: un modelo hidrológico automatizado con sistemas de información geográfica**

+ **Autores:** Fabio Alejandro Montealegre Medina[^af1] y Fernanda Julia Gaspari[^af2].

+ **Resumen:** El análisis espacial de la respuesta de una cuenca frente a una precipitación requiere integrar información hidrológica, edáfica, de cobertura y uso del suelo en una estructura que permita representar la variabilidad territorial del proceso lluvia–escorrentía. El complemento GeoQ, desarrollado para QGIS, automatiza la aplicación espacial del método del Número de Curva (NC) y transforma dos capas vectoriales estandarizadas (grupos hidrológicos de suelo y cobertura/uso del suelo) junto con una condición de humedad antecedente y un evento de precipitación, en una cartografía distribuida de variables hidrológicas. Para ello, se presenta la aplicación paso a paso del complemento GeoQ versión 2.8 para QGIS, una herramienta de código abierto que automatiza el cálculo espacial de variables hidrológicas mediante el método del Número de Curva (NC). Como caso de estudio, se utiliza la cuenca A1 del río Salado, ubicada en el noroeste de la provincia de Buenos Aires, Argentina, que abarca una superficie de 17.737,63 km² y representa un ambiente de llanura con características distintivas: pendientes extremadamente bajas, densidad de drenaje reducida y alta susceptibilidad a anegamientos. El análisis hidrológico GeoQ se desarrolla con capas de información geoespacial fundamentales: (1) la clasificación de grupos hidrológicos de suelo (GH), donde predomina el grupo A (franco-arenoso); (2) la cobertura y uso del suelo, dominada por cultivos de herbáceas graminoides y vegetación de pastizales que ocupan la mayor parte de la cuenca; y tres escenarios de precipitación modal derivados del análisis de datos pluviométricos históricos (1990-2020) de las estaciones meteorológicas del Servicio Meteorológico Nacional de Junín, Laboulaye y Venado Tuerto y un cuarto escenario con un mapa de una tormenta extrema con valores a nivel geoespacial. Los escenarios corresponden a los siguientes eventos pluviales: 33 mm (evento I, de cuartil 3, representando lluvias frecuentes), 89 mm (evento II, condición media entre lluvias moderadas), y 145 mm (evento III, precipitación máxima registrada, representando lluvias extremas) y una tormenta adquirida en el Portal CHRS, que aporta datos e información global sobre precipitaciones obtenidos por satélite. A través de GeoQ, se modeliza la cuenca para generar cartografía temática que zonifica el coeficiente de escorrentía (CE), la capacidad de infiltración (CF), las abstracciones iniciales y la capacidad de campo (CIo), considerando una condición de humedad antecedente media. Los resultados permiten visualizar cómo la interacción entre el complejo suelo-vegetación y la intensidad de la tormenta determina la respuesta hidrológica de la cuenca, identificando zonas críticas donde precipitaciones intensas pueden generar escorrentía significativa y potenciales problemas de anegamiento. Este capítulo demuestra cómo GeoQ facilita el análisis hidrológico distribuido en cuencas de llanura, proporcionando información cuantitativa y espacialmente explícita relevante para la planificación territorial, la evaluación de vulnerabilidad hidrológica y la gestión del riesgo de inundaciones en la región pampeana.

+ **Palabras clave:** Modelación hidrológica, QGis, escorrentía, infiltración.

[^af1]: Ingeniero Catastral y Geodesta. Magíster en Manejo Integral de Cuencas Hidrográficas. Doctor de la FCAyF, UNLP. Centro de Estudios Integrales de la Dinámica Exógena (CEIDE), Facultad de Ciencias Agrarias y Forestales, Universidad Nacional de La Plata. Profesional de Apoyo de la Comisión de Investigaciones Cientificas de la Provincia de Buenos Aires CIC PBA. <fabio.montealegre@agro.unlp.edu.ar>

[^af2]: Ingeniera Forestal. Magíster en Conservación y Gestión del Medio Natural. Doctora en Ingeniería. Profesora Titular de la Cátedra de Manejo de Cuencas Hidrográficas Directora del Centro de Estudios Integrales de la Dinámica Exógena (CEIDE), Facultad de Ciencias Agrarias y Forestales y Facultad de Ciencias Naturales y Museo, Universidad Nacional de La Plata. Asociado al Comisión de Investigaciones Cientificas de la Provincia de Buenos Aires CIC PBA. <fgaspari@agro.unlp.edu.ar>

---

(geoq-introduccion)=
## Introducción

(geoq-modelizacion-sig)=
### Modelización hidrológica y sistemas de información geográfica

La cuantificación del excedente superficial de una lluvia constituye la base de la planificación del recurso hídrico, debido a que permite diseñar acciones orientadas tanto a favorecer la infiltración, para mantener el agua disponible para los cultivos, como a reducir los caudales pico que originan inundaciones por desborde de los cauces {cite:p}`geoq-rodriguez2010`. En ese marco, la modelización hidrológica se ha consolidado como una herramienta clave para comparar escenarios de gestión y de cambio de uso del suelo en cuencas hidrográficas {cite:p}`geoq-deantueno2020`. Esta transformación de la cobertura modifica de manera sustancial la dinámica del agua superficial y subterránea, por ejemplo, mediante el aumento de las superficies impermeables, la eliminación de una cubierta vegetal absorbente o la intensificación de la extracción de agua subterránea para riego {cite:p}`geoq-rodriguez2014`.

La integración de modelos hidrológicos con la cartografía de uso del suelo alcanza un grado de fiabilidad aceptable gracias a la eficacia y la exactitud espacial que aportan las bases de datos geográficas gestionadas en un sistema de información geográfica (SIG) {cite:p}`geoq-sandoval2004,geoq-rodriguez2014,geoq-gaspari2019,geoq-gaspari2021`. El almacenamiento y el procesamiento de información georreferenciada en un SIG permiten, además, derivar nueva información para el análisis de la hidrología de superficie de manera coherente y sistemática, tanto en el espacio como en el tiempo {cite:p}`geoq-gaspari2019`. Diversos autores señalan que los modelos de simulación espacial aplicados a evaluar los cambios provocados sobre la escorrentía superficial constituyen una aproximación metodológica útil para comprender el comportamiento de las variables hidrológicas y mejorar la gestión de la información territorial {cite:p}`geoq-trivino2004,geoq-henriquez2006`.

No obstante, la implementación de los métodos hidrológicos tradicionales que simulan el proceso precipitación-escorrentía, enfrenta dos dificultades recurrentes. La primera es la escasez de datos meteorológicos y la segunda son las incompatibilidades entre la estructura de los modelos y las características geográficas y digitales de los datos generados con SIG {cite:p}`geoq-rango1985,geoq-shi2007`. Frente a ello, los métodos paramétricos basados en relaciones empíricas ofrecen una alternativa pragmática y simple, pero se debe considerar que cuantos menos parámetros intervienen, más sencilla resulta la aplicación, pero menores son las garantías. Por ello, sus resultados tienen un valor orientativo {cite:p}`geoq-lopezcadenas1998,geoq-gaspari2013`. Entre estos métodos, el del Número de Curva (NC), desarrollado por el Soil Conservation Service (SCS) de los Estados Unidos, es el de mayor difusión cuando se busca una aproximación adecuada a la lámina de escorrentía {cite:p}`geoq-mockus1972,geoq-chow1994`, cuyos fundamentos se desarrollan en la {numref}`sección %s <geoq-fundamentos>`.

A la disponibilidad de SIG para la implementación de estos modelos, se suma la de productos satelitales de precipitación de acceso abierto. Su cobertura sinóptica permite observar de manera simultánea grandes extensiones, incluidas áreas remotas o con baja densidad de estaciones. Su alta frecuencia temporal, del orden de minutos a horas, posibilita registrar eventos de corta duración y alta intensidad. Estas características los convierten en insumos valiosos para alimentar, calibrar y validar modelos hidrológicos, así como para el monitoreo y la anticipación de eventos extremos {cite:p}`geoq-montealegre2024a`. La incorporación de este tipo de información como dato de entrada espacialmente distribuido constituye, precisamente, una de las innovaciones de la última versión de GeoQ.

(geoq-llanuras)=
### La respuesta hidrológica de las llanuras

La región pampeana argentina ha gestionado históricamente sus excesos hídricos mediante obras de saneamiento y canalización, como el dragado de cauces y la construcción de canales que desvían cursos de agua o drenan áreas anegables. En menor medida, ha recurrido a prácticas culturales de manejo y uso del suelo {cite:p}`geoq-montealegre2021a`. Desde la perspectiva del manejo de cuencas hidrográficas, estas actuaciones pueden sintetizarse en dos líneas compatibles y complementarias, que son la utilización racional de las áreas de cultivo, que ocupan los sectores de menor pendiente y suelos más fértiles y el establecimiento de una cubierta vegetal protectora en el resto del territorio, bajo formas de praderas, humedales y matorrales hasta bosques naturales o implantados {cite:p}`geoq-mintegui1990`. Rodríguez Vagaría y Gaspari {cite:yearpar}`geoq-rodriguez2010` expresan que la restauración agrohidrológica es una práctica que procura englobar esta problemática común, que pretende aplicar en suelos, agua y vegetación, un plan unificado de acción, que combina la organización espacial de la producción con la modelización hidrológica, como fuente de la variable hídrica.


En particular en las llanuras, este enfoque adquiere una relevancia particular. Los ambientes de la Pampa Deprimida en Argentina presentan un relieve muy suave, casi horizontal, salpicado de depresiones locales en las que la precipitación que alcanza la superficie se almacena en encharcamientos someros y bañados {cite:p}`geoq-mosp1999,geoq-gomez2017`. La escasa pendiente y la reducida densidad de drenaje limitan la evacuación del agua. En consecuencia, la capacidad del complejo suelo-vegetación para interceptar, retener e infiltrar la lluvia adquiere un peso decisivo en la generación de excedentes y en la ocurrencia de anegamientos. Gómez et al. {cite:yearpar}`geoq-gomez2017` indican que a ello se suma una dificultad de que los modelos digitales de elevación disponibles requieren correcciones altimétricas específicas para su uso hidrológico en áreas de llanura.

Estas limitaciones refuerzan el interés por métodos que, como el NC, estiman la respuesta de la cuenca a partir de la textura del suelo y de la cobertura vegetal, variables cuya cartografía es, en general, de fácil disponibilidad {cite:p}`geoq-montealegre2021a`.

(geoq-formulacion-espacial)=
### De la formulación hidrológica a la representación espacial

En su forma convencional, el procesamiento del método del NC en un SIG consiste en una tabulación cruzada de la cartografía de grupos hidrológicos de suelo y de cobertura vegetal. A continuación, se asigna el NC a cada combinación y se aplica un modelo algebraico cartográfico que resuelve la ecuación de la escorrentía {cite:p}`geoq-gaspari2011,geoq-gaspari2016`. Este procedimiento, descrito paso a paso en la literatura de cátedra {cite:p}`geoq-gaspari2025`, exige una secuencia de operaciones manuales: disolución de polígonos, intersección de capas, edición de la tabla de atributos para cargar el NC de cada combinación y cálculo de campos. Repetir esta secuencia para distintas tormentas o condiciones de humedad resulta laborioso y propenso a errores de transcripción, lo que justifica su automatización.

Zhan y Huang {cite:yearpar}`geoq-zhan2004` desarrollaron ArcCN-Runoff, una extensión de ESRI ArcGIS® que genera mapas de NC y de escorrentía a escala de cuenca. En el ámbito regional, Rodríguez Vagaría y Gaspari {cite:yearpar}`geoq-rodriguez2010` crearon la primera versión de GeoQ para Idrisi Andes®. Esta versión estaba orientada a estimar descargas y a predecir la respuesta hidrológica de una cuenca ante cambios de uso del suelo, como la implementación de medidas de conservación, las modificaciones de la cubierta vegetal o el avance de la urbanización, que se aplicó, entre otros casos, a la evaluación del efecto de la restauración agrohidrológica sobre el escurrimiento superficial {cite:p}`geoq-rodriguez2012`. Como señala Eastman {cite:yearpar}`geoq-eastman2006`, herramientas de este tipo constituyen modelos geoespaciales automatizados que generan nueva información útil para el análisis de la hidrología de superficie.

El paso siguiente fue la reimplementación de GeoQ en QGIS®, una aplicación SIG de código abierto distribuida bajo la licencia pública general GNU. QGIS amplía sus capacidades mediante complementos escritos en Python y dispone de un entorno gráfico para construir y ejecutar modelos de pasos múltiples que procesan capas temáticas en formato tabular, vectorial o ráster {cite:p}`geoq-montealegre2021a`. Sobre esa base se desarrollaron las versiones que se sintetizan en la Tabla 14.1.

**Tabla 14.1.** Evolución de la herramienta GeoQ.

| Versión | Plataforma | Año | Entrada de precipitación | Capacidades principales | Referencia |
|---|---|---|---|---|---|
| GeoQ para Idrisi | Idrisi Andes® | 2010 | Valor único | Determinación del NC y de la escorrentía por complejo suelo-vegetación | Rodríguez Vagaría y Gaspari {cite:yearpar}`geoq-rodriguez2010` |
| GeoQ 1.x | QGIS 3.x (desarrollada en 3.18) | 2021-2022 | Valor único para toda la cuenca | Tablas de NC por CHA cargadas por el usuario en formato CSV; capas de resultados GeoQ, CE, CF y CIo | Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2021a` |
| GeoQ 2.8 | QGIS 3.x | 2024 | Valor numérico o capa ráster | Límite del área de estudio como insumo; tablas de NC integradas y CHA seleccionable; simbología automática de NC, CE, CF y CIo | Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a,geoq-montealegre2024b` |

*Fuente: elaboración propia con base en Rodríguez Vagaría y Gaspari {cite:yearpar}`geoq-rodriguez2010` y Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2021a,geoq-montealegre2024a,geoq-montealegre2024b`.*

Desde su publicación en el repositorio oficial de complementos de QGIS, GeoQ se ha aplicado en cuencas serranas de los sistemas de Ventania y Tandilia y en la cuenca alta del río Reconquista. También se ha utilizado en estudios comparativos de áreas urbanas y rurales y en la evaluación de la respuesta hidrológica ante la invasión de especies leñosas exóticas. Además, ha dado soporte a tesis de grado y de posgrado en la Facultad de Ciencias Agrarias y Forestales de la Universidad Nacional de La Plata {cite:p}`geoq-montealegre2024a`. El detalle de estos trabajos se ofrece en la sección de {ref}`recursos complementarios <geoq-recursos>`.

El objetivo del capítulo es presentar, de manera aplicada y paso a paso, el complemento GeoQ 2.8 para QGIS, y demostrar su utilidad para el análisis hidrológico distribuido en cuencas de llanura a partir del caso de la cuenca A1 del río Salado. Específicamente, se propone sintetizar los fundamentos del método del NC y los supuestos que condicionan la interpretación de sus resultados, describir la arquitectura del complemento, la preparación de sus insumos y su ejecución, modelizar la respuesta hidrológica de la cuenca A1 ante tres escenarios pluviales en condición de humedad antecedente media y formular recomendaciones de uso orientadas a la planificación territorial y a la gestión del riesgo hídrico.

---

(geoq-fundamentos)=
## Fundamentos del método del Número de Curva

El método del Número de Curva fue desarrollado en 1954 por el Servicio de Conservación de Suelos del Departamento de Agricultura de los Estados Unidos (USDA-SCS), actual Servicio de Conservación de Recursos Naturales (NRCS). Se trata de un procedimiento empírico para estimar la escorrentía directa producida por una lluvia aislada {cite:p}`geoq-usdanrcs2004,geoq-gaspari2025`, que surgió de la observación del fenómeno hidrológico en distintos tipos de suelo, estados de la cobertura vegetal y condiciones de humedad antecedente {cite:p}`geoq-mockus1972,geoq-chow1994`.

El método parte de un balance según el cual la precipitación total de una tormenta ($P$) se reparte en tres componentes, según la ecuación 14.1:

$$
P = I_0 + F + Q \tag{14.1}
$$

Cada componente tiene una definición precisa:

- **Abstracción inicial ($I_0$):** comprende la interceptación por la vegetación, la infiltración durante los primeros momentos de la tormenta y el almacenamiento superficial en pequeñas depresiones del terreno {cite:p}`geoq-mockus1972,geoq-kent1973,geoq-chow1994,geoq-mishra2013`.
- **Infiltración ($F$):** es la proporción de la lluvia que penetra en el suelo una vez satisfecha la abstracción inicial. Por ello se la considera una abstracción continuada.
- **Escorrentía superficial o precipitación efectiva ($Q$):** es la lluvia que no se retiene ni se infiltra {cite:p}`geoq-montealegre2021a`. Su valor es nulo hasta que la precipitación acumulada supera la abstracción inicial {cite:p}`geoq-gaspari2016`.

La base del método es que cada complejo suelo-vegetación se comporta de la misma forma frente a la infiltración bajo una condición de humedad antecedente dada {cite:p}`geoq-munoz2005`, que se sintetiza en un único parámetro adimensional, el Número de Curva (NC), que varía entre 0 y 100. Cuanto mayor es el NC, más predomina la escorrentía y menor es la infiltración {cite:p}`geoq-lopezcadenas1998,geoq-gaspari2009`.

(geoq-formulacion)=
### Formulación matemática

El procedimiento de cálculo se inicia con la retención potencial máxima ($S$), expresada en milímetros, que representa la capacidad máxima del complejo suelo-vegetación para retener agua ante una tormenta, y se obtiene a partir del NC {cite:p}`geoq-gaspari2013` según la ecuación 14.2.

$$
S = \frac{25\,400}{NC} - 254 \tag{14.2}
$$

La abstracción inicial ($I_0$) se estima como una fracción fija de $S$ (ecuación 14.3):

$$
I_0 = 0{,}2\,S \tag{14.3}
$$

La escorrentía superficial ($Q$) se calcula según la ecuación 14.4:

$$
Q = \frac{(P - I_0)^2}{P + 4\,I_0} \quad \text{si } P > I_0; \qquad Q = 0 \quad \text{si } P \le I_0 \tag{14.4}
$$

El denominador $P + 4\,I_0$ equivale a la expresión clásica $(P - I_0 + S)$, dado que $S = 5\,I_0$.

La infiltración se obtiene por diferencia {cite:p}`geoq-mockus1972,geoq-kent1973,geoq-gaspari2013,geoq-gaspari2015` según la ecuación 14.5:

$$
F = \frac{S\,(P - I_0)}{P - I_0 + S} \tag{14.5}
$$

Para comparar complejos y escenarios con independencia de la magnitud de la tormenta, cada componente se expresa como proporción de la precipitación total. Así se definen el coeficiente de escorrentía (CE), el coeficiente de infiltración (CF) y el coeficiente de abstracciones iniciales (CIo), cuya suma es igual al 100 % {cite:p}`geoq-gaspari2009,geoq-gaspari2013` y se muestran en la ecuación 14.6:

$$
CE = \frac{Q}{P}\cdot 100 \qquad CF = \frac{F}{P}\cdot 100 \qquad CI_0 = \frac{I_0}{P}\cdot 100 \tag{14.6}
$$

La Figura 14.1 muestra el comportamiento de los tres coeficientes en función del NC para tres magnitudes de tormenta. Para una precipitación dada, el CE crece de manera monótona con el NC, mientras que el CIo decrece. El CF, en cambio, presenta un máximo en valores intermedios de NC: se anula tanto en superficies impermeables (NC = 100, $S = 0$) como en los complejos cuya abstracción inicial absorbe toda la lluvia. Al aumentar la precipitación, el dominio de la escorrentía se extiende hacia NC progresivamente menores.

```{figure} imagenes/geoq/fig_14-01_particion_nc.png
:align: center
:width: 90%

Partición de la precipitación en CE, CF y CIo en función del NC para tormentas de 33, 89 y 145 mm. La línea discontinua indica el NC crítico (Ec. 14.7). Fuente: elaboración propia a partir de las ecuaciones 14.2 a 14.6.
```

(geoq-grupos-hidrologicos)=
### Grupos hidrológicos de suelo

La información edáfica se estructura en cuatro categorías de textura y permeabilidad, denominadas grupos hidrológicos (GH), que expresan el potencial de escorrentía del suelo {cite:p}`geoq-mintegui1990,geoq-chow1994,geoq-lopezcadenas1998,geoq-neilsen1998,geoq-gaspari2009,geoq-mishra2013`. La asignación de cada unidad cartográfica de suelos a un GH se basa en la textura.

En GeoQ, cada GH se identifica mediante un código numérico entero denominado `Cod_Sue`, que debe consignarse en la tabla de atributos de la capa vectorial de suelos según la Tabla 14.2.

**Tabla 14.2.** Grupos hidrológicos de suelo y códigos requeridos por GeoQ.

| GH | Potencial de escorrentía | Características | Texturas asociadas | Cod_Sue |
|---|---|---|---|---|
| A | Mínimo | Arenas profundas con poco limo y arcilla; loess muy permeables | Arenosa, arenosa franca, franco arenosa | 1 |
| B | Moderadamente bajo | Suelos mayormente francos, menos profundos y compactos que los del grupo A | Franco limosa, franca, limosa | 2 |
| C | Moderadamente alto | Suelos poco profundos con abundante arcilla y coloides | Franco arcillo arenosa, franco arcillo limosa, franco arcillosa | 3 |
| D | Máximo | Suelos arcillosos poco profundos, con subhorizontes casi impermeables cerca de la superficie | Arcillo arenosa, arcillo limosa, arcillosa | 4 |

*Fuente: adaptado de Gaspari et al. {cite:yearpar}`geoq-gaspari2009` y Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`.*

(geoq-cobertura)=
### Cobertura y uso del suelo

El segundo componente del complejo es la vegetación o el uso del suelo, incluido el tratamiento cultural que recibe {cite:p}`geoq-gaspari2006`. La clasificación de Mintegui Aguirre y López Unzú {cite:yearpar}`geoq-mintegui1990` combina tres criterios: el tipo de cobertura; la práctica de manejo, que puede consistir en laboreo sin prácticas de conservación, cultivo en curvas de nivel o curvas de nivel con terrazas, y la condición hidrológica, desde muy pobre hasta muy buena.

Para facilitar su aplicación en la Argentina, GeoQ armoniza esa clasificación con las clases de la cartografía nacional de cobertura del suelo elaborada con el sistema LCCS {cite:p}`geoq-volante2006,geoq-inta2009`. El resultado es una tipología de 58 clases codificadas, en intervalos de diez, en el campo `Cod_Veg` en el archivo vectorial (Anexo, Tabla 14.17).

(geoq-cha)=
### Condición de humedad antecedente

La respuesta de un mismo complejo suelo-vegetación varía según el contenido de humedad del suelo al inicio de la tormenta. El método la representa mediante tres condiciones de humedad antecedente (CHA), definidas a partir de la lluvia acumulada en los 5 a 10 días previos al evento (Tabla 14.3) {cite:p}`geoq-mintegui1990,geoq-chow1994,geoq-lopezcadenas1998,geoq-gaspari2009,geoq-gaspari2013`.

**Tabla 14.3.** Condiciones de humedad antecedente en función de la lluvia previa.

| CHA | Estado de humedad del suelo | Lluvia antecedente en los 5 a 10 días previos (mm) |
|---|---|---|
| I | Seco | Menor de 12,7 |
| II | Medio | 12,7 a 38,1 |
| III | Húmedo o saturado | Mayor de 38,1 |

*Fuente: adaptado de Gaspari et al. {cite:yearpar}`geoq-gaspari2013`.*

El efecto de la CHA sobre el NC es considerable, el cual aumenta con el mayor estado de humedad del suelo.

La elección de la CHA constituye una decisión de modelización que debe fundamentarse en el registro pluviométrico previo al evento analizado. La condición II es la referencia habitual - normal para escenarios de diseño; la condición III es la adecuada para evaluar tormentas que se producen sobre suelos húmedos tras períodos lluviosos; y la CHA I se considera en estado de sequedad edáfica.

(geoq-supuestos)=
### Supuestos, alcance y limitaciones

La interpretación de los resultados exige tener presentes los supuestos del método.

En primer lugar, se trata de un modelo de evento, por lo cual opera con la lámina total de una tormenta aislada, sin considerar su intensidad, su duración ni su distribución temporal, y no incluye el tránsito del agua por la cuenca. Por ello estima volúmenes de escorrentía, pero no caudales pico ni hidrogramas.

En segundo lugar, no incorpora de manera explícita la topografía. La pendiente solo interviene de forma indirecta a través de la clasificación de la cobertura, y el almacenamiento en depresiones queda subsumido en la abstracción inicial. En cuencas serranas, este aspecto puede complementarse con el cruce entre el NC y la pendiente, como en la determinación del potencial erosivo cualitativo {cite:p}`geoq-gaspari2025`.

En tercer lugar, la relación $I_0 = 0{,}2\,S$ (Ec. 14.3) es una simplificación empírica que ha sido objeto de revisión, reconociendo que hay diversos análisis sugieren coeficientes menores para numerosas cuencas {cite:p}`geoq-usdanrcs2004,geoq-hawkins2009`.

Por último, como todo método paramétrico, sus resultados tienen un valor orientativo y se desaconseja su uso con series meteorológicas cortas o incompletas {cite:p}`geoq-lopezcadenas1998,geoq-gaspari2013`.

Un caso particular merece atención. Cuando la abstracción inicial iguala o supera la precipitación, la escorrentía y la infiltración son nulas, y toda la lluvia se retiene en superficie. Al reemplazar $I_0 = P$ en las ecuaciones 14.2 y 14.3 (es decir, $S = 5\,P$) se obtiene el NC crítico por debajo del cual no se genera escorrentía:

$$
NC_{crit} = \frac{25\,400}{254 + 5\,P} \tag{14.7}
$$

El número de curva crítico (frecuentemente asociado al umbral de escorrentía) es la cantidad mínima de lluvia acumulada (en milímetros) que debe precipitar sobre una cuenca hidrográfica para que empiece a generarse escorrentía superficial, es decir, para que el agua deje de abstraerse por la vegetación y/o infiltrarse por completo, y empiece a fluir por la superficie.

---

(geoq-complemento)=
## El complemento GeoQ 2.8

(geoq-arquitectura)=
### Arquitectura y flujo de procesamiento

GeoQ se construyó con el Diseñador de modelos de QGIS como un modelo gráfico de pasos múltiples. El modelo encadena geoprocesos vectoriales, asignación automática de valores, operaciones matemáticas entre capas y reclasificación de categorías en un procedimiento interconectado {cite:p}`geoq-montealegre2021a`. La versión 2.8 conserva esa lógica e incorpora el límite del área de estudio y la precipitación ráster como insumos. El flujo resultante comprende ocho etapas y se sintetiza en la Figura 14.2.


1. Reproyección de las capas de entrada al sistema de referencia de coordenadas (SRC) del límite del área de estudio.
2. Incorporación de las columnas de `Cod_Sue` y `Cod_Veg` a los mapas de suelos y vegetación, respectivamente.
3. Intersección de las capas de GH y de cobertura, que produce las unidades homogéneas o complejos suelo-vegetación.
4. Construcción de la clave `Cod_NC` como suma de `Cod_Sue` y `Cod_Veg`.
5. Unión con la tabla de NC correspondiente a la CHA seleccionada, integrada en el complemento.
6. Asignación de la precipitación a cada complejo, ya sea como valor único o a partir de la capa ráster.
7. Cálculo de $S$, $I_0$, $Q$ y $F$ (Ec. 14.2 a 14.5) y de los coeficientes (Ec. 14.6), con el tratamiento del caso $I_0 \ge P$.
8. Generación de la capa de resultados y de las capas temáticas simbolizadas.

```{figure} imagenes/geoq/fig_14-02_flujo_geoq.png
:align: center
:width: 90%

Flujo de procesamiento del complemento GeoQ 2.8. Fuente: elaboración propia con base en Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2021a,geoq-montealegre2024a`.
```

La codificación de `Cod_Sue` y `Cod_Veg` permite que la suma de ambos códigos identifique de manera unívoca cada complejo suelo-vegetación, puesto que los GH ocupan las unidades (1 a 4) y las coberturas, las decenas y centenas.

El código resultante (`Cod_NC = Cod_Veg + Cod_Sue`) es la clave con la que el complemento recupera el NC correspondiente.

Por ejemplo, el código 31 designa un cultivo de herbáceas graminoides sobre un suelo del grupo A. La Tabla 14.4 ilustra la construcción de la clave para un uso en barbecho.

**Tabla 14.4.** Ejemplo de construcción de la clave Cod_NC para un uso en barbecho (CHA II).

| GH | Cod_Sue | Vegetación y/o uso del suelo | Cod_Veg | Cod_NC | NC |
|---|---|---|---|---|---|
| A | 1 | Barbecho | 10 | 11 | 77 |
| B | 2 | Barbecho | 10 | 12 | 86 |
| C | 3 | Barbecho | 10 | 13 | 91 |
| D | 4 | Barbecho | 10 | 14 | 94 |

*Fuente: Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`.*

Los valores de NC para todas las combinaciones, adaptados de las tablas del SCS {cite:p}`geoq-mockus1972`, se presentan en el Anexo (Tabla 14.18).

(geoq-instalacion)=
### Instalación

GeoQ se distribuye a través del repositorio oficial de complementos de QGIS {cite:p}`geoq-montealegre2024b`. Para instalarlo, se accede al menú *Complementos > Administrar e instalar complementos*, se busca "GeoQ" en la pestaña *Todos* y se selecciona *Instalar complemento* (Figura 14.3, superior).

Como alternativa, el código se encuentra en el repositorio <https://github.com/fabalmon/GeoQ>.

Desde allí puede descargarse en formato ZIP mediante la opción *Code > Download ZIP* e instalarse con la opción *Instalar a partir de ZIP* del mismo administrador de complementos. Una vez instalado, el complemento aparece en el panel *Caja de herramientas de Procesos* con el nombre GeoQ (Figura 14.3, inferior).

El repositorio declara compatibilidad con QGIS 3.0 y posteriores, aunque el manual recomienda emplear versiones iguales o superiores a la 3.18 {cite:p}`geoq-montealegre2024a`.

```{figure} imagenes/geoq/fig_14-03_instalacion.png
:align: center
:width: 90%

Instalación de GeoQ desde el administrador de complementos de QGIS. Superior: instalación del complemento desde el repositorio de QGIS; inferior: instalación por medio de archivo comprimido ZIP.
```

(geoq-insumos)=
### Preparación de los insumos

La calidad de los resultados depende directamente de la preparación de los insumos, que deben adecuarse previamente al formato que se resume en la Tabla 14.5.

**Tabla 14.5.** Insumos de GeoQ 2.8 y requisitos de formato.

| Insumo | Formato | Requisito | Observaciones |
|---|---|---|---|
| Límite del área de estudio | Vector de polígonos | Georreferenciado, en coordenadas planas o geográficas | Su SRC define el de todos los resultados |
| Precipitación | Valor numérico (mm) o ráster (mm por píxel) | Un único evento pluvial | El valor numérico es opcional si se ingresa el ráster, y viceversa |
| Grupos hidrológicos | Vector de polígonos | Campo `Cod_Sue` de tipo entero (1 a 4) | Codificación según la Tabla 14.2 |
| Cobertura y uso del suelo | Vector de polígonos | Campo `Cod_Veg` de tipo entero (10 a 580) | Codificación según el Anexo, Tabla 14.17 |
| Condición de humedad antecedente | Lista desplegable | I, II o III | Tablas de NC integradas en el complemento |

*Fuente: elaboración propia con base en Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`.*

**Límite del área de estudio.** Puede representar una cuenca hidrográfica, una parcela o cualquier polígono de interés. Su SRC es determinante, porque el complemento reproyecta a él todas las demás capas y lo transfiere a los productos. Se recomienda adoptar un SRC proyectado, por ejemplo, una faja POSGAR 2007 en el caso de la Argentina, para que las superficies de los resultados puedan calcularse directamente en unidades métricas.

**Precipitación.** La tormenta puede ingresarse de dos maneras.

- Con un valor numérico representativo o un evento pluvial, que se aplica de manera uniforme.
- Con un ráster en el que cada píxel contiene la lámina precipitada del evento, lo cual permite representar su variabilidad espacial. Para esta opción se recomienda utilizar el portal de datos del Center for Hydrometeorology and Remote Sensing (CHRS) de la Universidad de California en Irvine, que distribuye los productos de la familia PERSIANN. Entre ellos, se aconseja el producto PDIR-Now, que es una estimación casi en tiempo real basada en información infrarroja con una relación dinámica entre la temperatura de tope de nube y la tasa de lluvia {cite:p}`geoq-nguyen2020`. El ráster debe corresponder al acumulado del evento que se desea modelizar y cubrir la totalidad del área de estudio. *Es importante mencionar que los píxeles donde no se registró precipitación tienen valor nulo, reconocido como "sin dato".*

**Grupos hidrológicos de suelo.** La capa se deriva de la cartografía edáfica disponible, por ejemplo, las cartas de suelos, asignando a cada unidad cartográfica su GH según la textura del horizonte superficial y las características descritas en la Tabla 14.2. El código se registra en un campo nuevo denominado `Cod_Sue`, de tipo entero.

**Cobertura y uso del suelo.** Las clases de la cartografía de origen deben reclasificarse según la tipología estandarizada del Anexo (Tabla 14.17) y registrarse en un campo entero denominado `Cod_Veg`. Cuando la leyenda local no coincide exactamente con esa tipología, se asigna la clase de comportamiento hidrológico más próximo. Es recomendable documentar la tabla de correspondencias utilizada, dado que constituye una de las principales fuentes de incertidumbre del modelo {cite:p}`geoq-montealegre2021a`.

**Control geométrico.** Para ambas capas vectoriales se recomienda corregir la geometría, convertir los registros a partes simples y revisar la topología antes de la ejecución {cite:p}`geoq-montealegre2024a`. Además, puede recortar previamente las capas de GH y cobertura y uso del suelo con el límite del área de estudio, porque reduce el tiempo de procesamiento {cite:p}`geoq-gaspari2025`.

(geoq-ejecucion)=
### Ejecución del modelo

El complemento se ejecuta desde el panel *Caja de herramientas de Procesos*. La interfaz solicita los parámetros que se detallan en la Tabla 14.6, en el orden en que aparecen en pantalla (Figura 14.4). Las capas de entrada pueden seleccionarse de la lista desplegable, si ya están cargadas en el proyecto, o buscarse en el equipo.

**Tabla 14.6.** Parámetros de la interfaz de GeoQ 2.8.

| N.º | Parámetro | Acción requerida |
|---|---|---|
| 1 | Límite del área de estudio | Seleccionar la capa vectorial del límite |
| 2a | Precipitación (mm) | Ingresar el valor numérico del evento (opcional si se usa ráster) |
| 2b | Precipitación ráster (mm) | Seleccionar el ráster del evento (opcional si se usa valor numérico) |
| 3 | Grupo hidrológico de suelo | Seleccionar la capa codificada con `Cod_Sue` |
| 4 | Cobertura vegetal y uso del suelo | Seleccionar la capa codificada con `Cod_Veg` |
| 5 | Condición de humedad antecedente | Elegir I, II o III en la lista desplegable |
| 6 | GeoQ | Definir la ruta y el nombre de la capa de resultados, o dejarla como temporal |
| 7 | NC | Definir la ruta y el nombre de la capa de NC |
| 8 | CE | Definir la ruta y el nombre de la capa de CE |
| 9 | CF | Definir la ruta y el nombre de las capas de CF |
| 10 | CIo | Definir la ruta y el nombre de las capas de CIo |

*Fuente: elaboración propia con base en Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`.*

Si no se define una ruta de salida, los resultados se generan como capas temporales en la carpeta de procesamiento del perfil de usuario de QGIS. Se recomienda guardarlos con nombres explícitos, ya que las capas temporales se cargan con denominaciones genéricas que dificultan su identificación.

```{figure} imagenes/geoq/fig_14-04_interfaz.png
:align: center
:width: 80%

Interfaz de GeoQ 2.8 con los parámetros de entrada y salida numerados.
```

(geoq-productos)=
### Productos del modelo

Al finalizar la ejecución, GeoQ carga automáticamente cinco nuevas capas en el proyecto abierto de QGIS.

La primera, denominada GeoQ, no tiene simbología y contiene en su tabla de atributos, para cada complejo, los resultados de todas las etapas del cálculo (Tabla 14.7, Figura 14.5). Las otras cuatro corresponden a la zonificación del NC, el CE, el CF y el CIo, y se presentan con simbología predefinida por clases.

La versión 2.8 no calcula la superficie de cada registro. Por ello, para cuantificar la extensión de cada clase debe agregarse un campo con la expresión `$area` en un SRC proyectado.


**Tabla 14.7.** Variables de la tabla de atributos de la capa GeoQ.

| Variable | Descripción | Unidad |
|---|---|---|
| NC | Número de curva del complejo para la CHA seleccionada | Adimensional |
| S | Retención potencial máxima (Ec. 14.2) | mm |
| I₀ | Abstracción inicial (Ec. 14.3) | mm |
| Q | Escorrentía superficial (Ec. 14.4) | mm |
| F | Infiltración (Ec. 14.5) | mm |
| CE, CF, CIo | Coeficientes de escorrentía, infiltración y abstracciones iniciales (Ec. 14.6) | % |

*Fuente: elaboración propia con base en Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`.*

Cuando en un complejo la abstracción inicial resulta mayor que la precipitación, GeoQ asigna un CIo del 100 % y valores nulos a CE y CF {cite:p}`geoq-montealegre2021a`. Esta regla es coherente con la ecuación 14.4 y con el NC crítico (Ec. 14.7): esos complejos retienen en superficie la totalidad de la tormenta y no generan escorrentía.

```{figure} imagenes/geoq/fig_14-05_tabla_atributos.png
:align: center
:width: 90%

Tabla de atributos de la capa de resultados GeoQ.
```

(geoq-advertencias)=
### Advertencias y buenas prácticas

Durante la ejecución pueden aparecer advertencias en color rojo que no afectan a los resultados. Se deben a la inserción de geometrías de tipo POLYGON en capas de tipo MULTIPOLYGON, una operación que la especificación GeoPackage no admite formalmente pero que el controlador resuelve. Si se desea un GeoPackage estrictamente conforme, puede utilizarse la opción `-nlt` de `ogr2ogr` para forzar el tipo de geometría.

El complemento no sobrescribe resultados previos. Antes de una nueva ejecución deben cerrarse las capas generadas en la corrida anterior o indicarse un directorio de salida diferente {cite:p}`geoq-montealegre2024a`.

Por último, se recomiendan dos controles de calidad sencillos. El primero consiste en recalcular manualmente, con las ecuaciones 14.2 a 14.6, los valores de al menos un registro por cobertura dominante. El segundo consiste en verificar que la suma de CE, CF y CIo sea igual al 100 % en todos los registros.

---

(geoq-aplicacion)=
## Ejemplo de aplicación: cuenca A1 del río Salado

(geoq-area-estudio)=
### Área de estudio

La cuenca A1 es una de las subregiones en que se divide la cuenca del río Salado. Se localiza en el noroeste de la provincia de Buenos Aires y limita con las provincias de Córdoba y Santa Fe (Figura 14.6), ubicada en la Pampa Deprimida, cuyos rasgos fisiográficos de llanura se describieron en la {numref}`sección %s <geoq-llanuras>`.

Su límite se obtuvo a partir de un modelo digital de elevación SRTM corregido altimétricamente para uso hidrológico en áreas de llanura {cite:p}`geoq-gomez2017`. La A1 abarca una superficie de 17.737,63 km², con cotas comprendidas entre aproximadamente 170 y 50 m s. n. m. y un relieve en el que predominan las superficies casi horizontales {cite:p}`geoq-montealegre2021b`.

```{figure} imagenes/geoq/fig_14-06_localizacion.png
:align: center
:width: 90%

Localización de la cuenca A1 en la cuenca del río Salado, provincia de Buenos Aires, Argentina.
```

(geoq-insumos-a1)=
### Insumos cartográficos

La capa de grupos hidrológicos se elaboró asignando los códigos de la Tabla 14.2 a las unidades edáficas de la cuenca (Tabla 14.8). Predomina el grupo A, que ocupa el 78,8 % del área, con suelos de textura gruesa, que transmiten el agua con fluidez a través del perfil y presentan un bajo potencial de escorrentía {cite:p}`geoq-diazcarvajal2017`. Luego el grupo D, con el 14,3 %, y por último, los grupos C y B tienen una participación menor.

**Tabla 14.8.** Superficie ocupada por los grupos hidrológicos de suelo en la cuenca A1.

| GH | Cod_Sue | Superficie (km²) | Proporción (%) |
|---|---|---|---|
| A | 1 | 13.971,46 | 78,8 |
| B | 2 | 112,94 | 0,6 |
| C | 3 | 1.115,38 | 6,3 |
| D | 4 | 2.537,85 | 14,3 |
| **Total** | | **17.737,63** | **100,0** |

*Fuente: elaboración propia con base en Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2021b`.*

La capa de cobertura y uso del suelo se adaptó de la cartografía de cobertura del suelo de la República Argentina {cite:p}`geoq-volante2006`. En la cuenca se identificaron siete clases, que se codificaron según la tipología de GeoQ (Tabla 14.9). Los cultivos de herbáceas graminoides (código 30) y las herbáceas cerradas en áreas regularmente anegadas (código 20) ocupan en conjunto el 88,8 % de la superficie. Los cuerpos de agua (código 410), correspondientes a cañadas y bañados, representan el 5,2 %.

**Tabla 14.9.** Superficie ocupada por las clases de cobertura y uso del suelo en la cuenca A1.

| Cod_Veg | Cobertura y uso del suelo | Superficie (km²) | Proporción (%) |
|---|---|---|---|
| 20 | Herbáceas cerradas en áreas anegables | 4.816,33 | 27,2 |
| 30 | Cultivos de herbáceas graminoides | 10.927,87 | 61,6 |
| 300 | Bosques cerrados | 151,26 | 0,9 |
| 390 | Áreas urbanas densamente pobladas | 77,16 | 0,4 |
| 410 | Cuerpos de agua (cañadas y bañados) | 923,36 | 5,2 |
| 490 | Cultivos de especies arbóreas | 456,09 | 2,6 |
| 580 | Vegas de ríos | 385,56 | 2,2 |
| | **Total** | **17.737,63** | **100,0** |

*Fuente: elaboración propia con base en Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2021b`.*

La distribución espacial de ambas capas y del NC resultante se presenta en la Figura 14.7.


```{figure} imagenes/geoq/fig_14-07_cobertura_gh_nc.png
:align: center
:width: 100%

Distribución de la cobertura y el uso del suelo (a), de los grupos hidrológicos (b) y del número de curva en CHA II (c) en la cuenca A1.
```

(geoq-escenarios)=
### Escenarios pluviales

Los escenarios de precipitación se definieron a partir de los registros pluviométricos del período 1990-2020 de tres estaciones del Servicio Meteorológico Nacional (SMN): Junín Aero, Laboulaye Aero y Venado Tuerto Aero. Estas estaciones se ubican en el interior y en los extremos de la cuenca (Figura 14.8). Además, se incorporó un cuarto registro a nivel geoespacial, perteneciente a una imagen satelital de la misión Persiann, obtenido de CHRS, de un evento extremo registrado en la estación meteorológica de Junin.

Los eventos de precipitación diaria del SMN, superiores a 12,7 mm (con base a CHA), se procesaron y ordenaron de forma ascendente para obtener sus cuartiles. Posteriormente, a partir del promedio de los datos de las tres estaciones se definieron tres escenarios (Tabla 14.10):

- **Escenario I (33 mm):** corresponde al tercer cuartil y representa lluvias frecuentes.
- **Escenario III (145 mm):** corresponde al máximo de la serie, equivalente al cuarto cuartil, y representa lluvias extremas.
- **Escenario II (89 mm):** es el valor intermedio entre ambos y representa lluvias moderadas.
- **Escenario IV (ráster de precipitación del 31 de marzo de 2013).**

**Tabla 14.10.** Cuartiles de precipitación (mm) de las estaciones de referencia de la cuenca A1, período 1990-2020.

| Cuartil | Junín | Laboulaye | Venado Tuerto | Promedio |
|---|---|---|---|---|
| Q1 | 14,3 | 14,1 | 13,5 | 14,0 |
| Q2 | 21,6 | 21,3 | 20,0 | 21,0 |
| Q3 (escenario I) | 35,0 | 33,0 | 32,0 | **33,3** |
| Promedio de Q3 y Q4 (escenario II) | 98,2 | 85,0 | 84,3 | **89,2** |
| Q4, máximo (escenario III) | 161,4 | 137,0 | 136,5 | **145,0** |

*Fuente: Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2021b`, con datos del SMN.*

La diferencia de 25 mm entre los máximos de Junín y de Venado Tuerto anticipa una variabilidad espacial de la lluvia que no puede representarse con un valor único, por ello, es que creo el cuatro escenario de un evento pluvial a nivel ráster que cubra toda el área en estudio. Este aspecto se retoma en la discusión.

```{figure} imagenes/geoq/fig_14-08_estaciones_raster.png
:align: center
:width: 90%

Localización de las estaciones meteorológicas de referencia para la cuenca A1 y la imagen satelital del evento pluvial.
```

(geoq-parametrizacion)=
### Parametrización hidrológica

GeoQ se ejecutó en condición de humedad antecedente media (CHA II), una vez para cada escenario, con la precipitación ingresada como valor numérico.

La intersección de las capas de cobertura y de GH generó 24 complejos suelo-vegetación. Sus NC oscilan entre 15, en los bosques cerrados sobre suelos del grupo A, y 100, en los cuerpos de agua. El NC medio ponderado por superficie es de 72,2. Los dos complejos dominantes concentran el 71,9 % de la cuenca: el 31 (cultivos de herbáceas graminoides sobre GH A), con un NC de 67, y el 21 (herbáceas en áreas anegables sobre GH A), con un NC de 72.

La Tabla 14.11 reúne los parámetros de cada complejo y el CE resultante en los tres escenarios.

**Tabla 14.11.** Parámetros hidrológicos y coeficiente de escorrentía por complejo suelo-vegetación en la cuenca A1 (CHA II).

| Cod_NC | Cobertura | GH | Superficie (km²) | NC | I₀ (mm) | CE Esc. I (%) | CE Esc. II (%) | CE Esc. III (%) |
|---|---|---|---|---|---|---|---|---|
| 21 | Herbáceas cerradas en áreas anegables | A | 3.262,57 | 72 | 19,8 | 4,7 | 32,1 | 48,3 |
| 22 | Herbáceas cerradas en áreas anegables | B | 38,47 | 81 | 11,9 | 16,7 | 48,9 | 63,4 |
| 23 | Herbáceas cerradas en áreas anegables | C | 649,53 | 88 | 6,9 | 33,9 | 64,8 | 76,1 |
| 24 | Herbáceas cerradas en áreas anegables | D | 865,76 | 91 | 5,0 | 44,7 | 72,6 | 81,8 |
| 31 | Cultivos de herbáceas graminoides | A | 9.485,75 | 67 | 25,0 | 1,4 | 24,3 | 40,5 |
| 32 | Cultivos de herbáceas graminoides | B | 63,86 | 78 | 14,3 | 11,7 | 42,8 | 58,2 |
| 33 | Cultivos de herbáceas graminoides | C | 317,16 | 85 | 9,0 | 25,4 | 57,6 | 70,6 |
| 34 | Cultivos de herbáceas graminoides | D | 1.061,10 | 89 | 6,3 | 37,2 | 67,4 | 78,0 |
| 301 | Bosques cerrados | A | 132,75 | 15 | 287,9 | 0,0 | 0,0 | 0,0 |
| 304 | Bosques cerrados | D | 18,51 | 61 | 32,5 | 0,0 | 16,4 | 31,8 |
| 391 | Áreas urbanas densamente pobladas | A | 71,35 | 77 | 15,2 | 10,3 | 40,9 | 56,5 |
| 393 | Áreas urbanas densamente pobladas | C | 0,78 | 90 | 5,6 | 40,8 | 70,0 | 79,9 |
| 394 | Áreas urbanas densamente pobladas | D | 5,03 | 92 | 4,4 | 48,9 | 75,4 | 83,8 |
| 411 | Cuerpos de agua (cañadas y bañados) | A | 410,45 | 100 | 0,0 | 100,0 | 100,0 | 100,0 |
| 412 | Cuerpos de agua (cañadas y bañados) | B | 4,45 | 100 | 0,0 | 100,0 | 100,0 | 100,0 |
| 413 | Cuerpos de agua (cañadas y bañados) | C | 121,80 | 100 | 0,0 | 100,0 | 100,0 | 100,0 |
| 414 | Cuerpos de agua (cañadas y bañados) | D | 386,66 | 100 | 0,0 | 100,0 | 100,0 | 100,0 |
| 491 | Cultivos de especies arbóreas | A | 408,47 | 49 | 52,9 | 0,0 | 4,9 | 16,4 |
| 493 | Cultivos de especies arbóreas | C | 7,52 | 75 | 16,9 | 7,8 | 37,2 | 53,2 |
| 494 | Cultivos de especies arbóreas | D | 40,10 | 79 | 13,5 | 13,2 | 44,8 | 59,9 |
| 581 | Vegas de ríos | A | 200,12 | 30 | 118,5 | 0,0 | 0,0 | 0,8 |
| 582 | Vegas de ríos | B | 6,16 | 58 | 36,8 | 0,0 | 13,0 | 27,6 |
| 583 | Vegas de ríos | C | 18,59 | 71 | 20,7 | 3,9 | 30,4 | 46,7 |
| 584 | Vegas de ríos | D | 160,69 | 78 | 14,3 | 11,7 | 42,8 | 58,2 |

*Fuente: elaboración propia.*

(geoq-zonificacion-ce)=
### Zonificación del Coeficiente de escorrentía

La distribución espacial del coeficiente de escorrentía (CE), expresada en clases porcentuales y en función de la superficie ocupada por cada una de ellas, se presenta en la Tabla 14.12 y la Figura 14.9.

A partir de la tabla se identifican diferencias en la distribución del CE entre los escenarios analizados.

**Tabla 14.12.** Clases de escorrentía por unidad de superficie de la cuenca A1 (km²).

```{raw} html
<div style="overflow-x:auto;margin:1em 0;">
<table style="border-collapse:collapse;margin:0 auto;font-size:0.95em;">
<thead>
<tr><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;"></th><th colspan="4" style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">Superficie de ocupación por escenario (km²)</th></tr>
<tr><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">Clase de CE (%)</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">I</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">II</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">III</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">IV</th></tr>
</thead>
<tbody>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">0–20</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 78.45%,transparent 78.45%);">13.914,91</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 4.32%,transparent 4.32%);">766,01</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 4.18%,transparent 4.18%);">741,34</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 71.38%,transparent 71.38%);">12.661,07</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">20–40</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 11.43%,transparent 11.43%);">2.027,79</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 72.02%,transparent 72.02%);">12.774,43</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 0.14%,transparent 0.14%);">24,67</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 11.43%,transparent 11.43%);">2.028,29</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">40–60</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 4.91%,transparent 4.91%);">871,57</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 3.90%,transparent 3.90%);">691,63</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 73.91%,transparent 73.91%);">13.110,43</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 9.08%,transparent 9.08%);">1.610,76</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">60–80</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 14.56%,transparent 14.56%);">2.582,20</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 11.65%,transparent 11.65%);">2.067,04</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 2.34%,transparent 2.34%);">415,27</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">80–100</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 5.21%,transparent 5.21%);">923,36</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 5.21%,transparent 5.21%);">923,36</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 10.11%,transparent 10.11%);">1.794,15</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 5.20%,transparent 5.20%);">923,11</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">Sin dato</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(31,119,180,.55) 0%,rgba(31,119,180,.55) 0.56%,transparent 0.56%);">99,15</td></tr>
</tbody>
</table>
</div>
```

*Fuente: elaboración propia. La longitud de la barra de cada celda es proporcional a la superficie respecto del total de la cuenca (17.737,63 km²).*

En el **escenario I** (33 mm), el 78 % de la cuenca presenta un CE inferior al 20 %. La respuesta queda dominada por las abstracciones iniciales. Solo generan escorrentía relevante los complejos con suelos de los grupos C y D, con un CE de entre 25 % y 49 %, y los cuerpos de agua, que transfieren íntegramente la lluvia (CE = 100 %).

En el **escenario II** (89 mm), la distribución se desplaza hacia la clase de 20 % a 40 %, que pasa a abarcar el 72 % de la cuenca por el cambio de clase de los dos complejos dominantes. En paralelo, los complejos de herbáceas y cultivos sobre suelos de los grupos C y D, que suman 2.893,55 km² (16,3 % del área), alcanzan un CE superior al 57 %.

En el **escenario III** (145 mm), el 74 % de la superficie se ubica en la clase de 40 % a 60 % y el 22 % supera el 60 %.

En el **escenario IV**, el ráster correspondiente al evento pluvial refleja de manera más ajustada la distribución espacial de la tormenta, evidenciando su carácter heterogéneo. Se observa una precipitación mínima o incluso la ausencia de registros en el sector norte del área de estudio, mientras que los mayores valores de precipitación se concentran en el sector sureste. En cuanto al CE, este escenario pone de manifiesto la heterogeneidad espacial del escurrimiento, asociada tanto a las características particulares del evento pluvial como a la influencia del complejo suelo-vegetación.

En la cartografía de resultados (Figura 14.9), la escorrentía se concentra en las zonas bajas y de escasa pendiente, donde la baja capacidad de evacuación de la llanura favorece la acumulación del agua {cite:p}`geoq-montealegre2021b`.

```{figure} imagenes/geoq/fig_14-09_ce_escenarios.png
:align: center
:width: 100%

Distribución espacial del coeficiente de escorrentía en la cuenca A1 para todos los escenarios en CHA II.
```

(geoq-zonificacion-cf)=
### Zonificación del Coeficiente de infiltración

La distribución del coeficiente de infiltración que refleja la lectura de la respuesta hidrológica por el suelo se presenta en la Tabla 14.13 y Figura 14.10.

Con relación al CF, este alcanza su máximo relativo en el escenario II (39,8 %) y desciende en el escenario III (34,6 %), aunque la lámina infiltrada continúa creciendo en términos absolutos, de 35,5 a 50,1 mm en promedio. Este comportamiento reproduce, a escala de cuenca, el máximo del CF en función del NC y de la precipitación. A medida que la tormenta aumenta, la retención del complejo se aproxima a su valor potencial y el excedente se deriva de manera creciente a la escorrentía. El escenario IV muestra la carga hídrica del suelo por el evento pluvial hacia el sur de la cuenca y al norte, el efecto de nulidad de la precipitación se expresa con nula a baja infiltración.

**Tabla 14.13.** Clases de infiltración por unidad de superficie de la cuenca A1 (km²).

```{raw} html
<div style="overflow-x:auto;margin:1em 0;">
<table style="border-collapse:collapse;margin:0 auto;font-size:0.95em;">
<thead>
<tr><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;"></th><th colspan="4" style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">Superficie de ocupación por escenario (km²)</th></tr>
<tr><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">Clase de CF (%)</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">I</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">II</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">III</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">IV</th></tr>
</thead>
<tbody>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">0–20</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 9.52%,transparent 9.52%);">1.689,37</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 7.11%,transparent 7.11%);">1.261,26</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 21.64%,transparent 21.64%);">3.838,43</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 30.49%,transparent 30.49%);">5.409,06</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">20–40</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 72.00%,transparent 72.00%);">12.771,94</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 18.84%,transparent 18.84%);">3.341,27</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 22.44%,transparent 22.44%);">3.980,31</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 30.27%,transparent 30.27%);">5.368,34</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">40–60</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 18.47%,transparent 18.47%);">3.276,32</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 74.05%,transparent 74.05%);">13.135,10</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 55.92%,transparent 55.92%);">9.918,89</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 38.68%,transparent 38.68%);">6.861,08</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">60–80</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">80–100</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">Sin dato</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(255,127,14,.55) 0%,rgba(255,127,14,.55) 0.56%,transparent 0.56%);">99,15</td></tr>
</tbody>
</table>
</div>
```

*Fuente: elaboración propia. La longitud de la barra de cada celda es proporcional a la superficie respecto del total de la cuenca (17.737,63 km²).*

El mapa resultante de la zonificación de CF se muestra en la Figura 14.10, donde la infiltración se distribuye según la textura edáfica superficial, conformada en el grupo hidrológico.

```{figure} imagenes/geoq/fig_14-10_cf.png
:align: center
:width: 100%

Distribución espacial del coeficiente de infiltración en la cuenca A1 para los escenarios I a IV (CHA II).
```

(geoq-zonificacion-cio)=
### Zonificación del Coeficiente de abstracciones iniciales

La distribución espacial del coeficiente de abstracciones iniciales (CIo), que expresa la influencia de las distintas coberturas vegetales y usos del suelo sobre la respuesta hidrológica de la cuenca, se presenta en la Tabla 14.14 y la Figura 14.11.

A partir de la Tabla 14.14 se observa que, a medida que aumenta la magnitud de la tormenta, la distribución del CIo tiende a desplazarse hacia las clases de menores porcentajes. Este comportamiento se evidencia en la disminución de su media ponderada, que pasa del 60,0 % en el escenario I al 23,9 % en el escenario II y al 15,2 % en el escenario III. Esta tendencia responde a que la abstracción inicial constituye una lámina asociada a las características de cada tipo de cobertura vegetal y uso del suelo, cuya proporción respecto de la precipitación total disminuye a medida que aumenta la magnitud de la tormenta.

Por su parte, el escenario IV presenta una distribución espacial más heterogénea del CIo, con superficies representadas en las distintas clases, desde los valores más bajos hasta los más elevados. Este comportamiento puede relacionarse con la distribución espacial no uniforme de la precipitación durante el evento, caracterizada por mayores registros en el sector sur de la cuenca respecto del norte, lo que condiciona una respuesta hidrológica diferenciada en función de la interacción entre la precipitación y las características del complejo suelo-vegetación.

**Tabla 14.14.** Clases de abstracciones iniciales por unidad de superficie de la cuenca A1 (km²).

```{raw} html
<div style="overflow-x:auto;margin:1em 0;">
<table style="border-collapse:collapse;margin:0 auto;font-size:0.95em;">
<thead>
<tr><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;"></th><th colspan="4" style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">Superficie de ocupación por escenario (km²)</th></tr>
<tr><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">Clase de CIo (%)</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">I</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">II</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">III</th><th style="padding:4px 8px;border:1px solid rgba(127,127,127,.45);background:rgba(127,127,127,.18);text-align:center;font-weight:bold;">IV</th></tr>
</thead>
<tbody>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">0–20</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 16.10%,transparent 16.10%);">2.856,03</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 23.71%,transparent 23.71%);">4.204,71</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 95.68%,transparent 95.68%);">16.971,62</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 17.72%,transparent 17.72%);">3.143,15</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">20–40</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 5.67%,transparent 5.67%);">1.005,16</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 72.08%,transparent 72.08%);">12.785,42</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 2.44%,transparent 2.44%);">433,14</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 20.26%,transparent 20.26%);">3.593,43</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">40–60</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 20.33%,transparent 20.33%);">3.606,09</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 2.34%,transparent 2.34%);">414,63</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 22.68%,transparent 22.68%);">4.023,02</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">60–80</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 53.58%,transparent 53.58%);">9.504,34</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 13.50%,transparent 13.50%);">2.394,02</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">80–100</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 4.32%,transparent 4.32%);">766,01</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 1.88%,transparent 1.88%);">332,87</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 1.88%,transparent 1.88%);">332,87</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 25.28%,transparent 25.28%);">4.484,85</td></tr>
<tr><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;text-align:center;font-weight:bold;">Sin dato</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 0.00%,transparent 0.00%);">0,00</td><td style="padding:3px 8px;border:1px solid rgba(127,127,127,.35);text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap;background:linear-gradient(90deg,rgba(44,160,44,.55) 0%,rgba(44,160,44,.55) 0.56%,transparent 0.56%);">99,15</td></tr>
</tbody>
</table>
</div>
```

*Fuente: elaboración propia. La longitud de la barra de cada celda es proporcional a la superficie respecto del total de la cuenca (17.737,63 km²).*

```{figure} imagenes/geoq/fig_14-11_cio.png
:align: center
:width: 100%

Distribución espacial del coeficiente de abstracciones iniciales en la cuenca A1 para los escenarios I a IV (CHA II).
```

(geoq-nc-critico)=
### Cuantificación del NC crítico

Considerando que número de curva crítico ($NC_{crit}$) es el que representa el punto de quiebre hidrológico, por debajo de este valor, toda el agua de lluvia es retenida o infiltrada en el suelo, se refleja diferente en cada uno de los escenarios estudiados.

En relación con los complejos suelo-vegetación que no generan escorrentía, para el **escenario I** el $NC_{crit}$ es de 60,6. Los complejos cuyo NC es inferior a este valor retienen la totalidad de la precipitación, sin generar escurrimiento. Estos corresponden a los bosques cerrados y cultivos arbóreos sobre grupos hidrológicos (GH) A, así como a las vegas de ríos sobre GH A y B, que en conjunto ocupan una superficie de 747,50 km².

Para el **escenario II**, con una precipitación de 89 mm, el $NC_{crit}$ desciende a 36,3 y la superficie que no genera escorrentía se reduce a 332,87 km².

Finalmente, para el **escenario III**, con una precipitación de 145 mm, el $NC_{crit}$ alcanza un valor de 25,9, por lo que únicamente los bosques cerrados sobre GH A mantienen la capacidad de retener la totalidad de la lluvia, ocupando una superficie de 132,75 km².

Por su parte, el **escenario IV** presenta un comportamiento espacial diferenciado, condicionado por la distribución heterogénea de la precipitación. El ráster del evento pluvial refleja una marcada variabilidad espacial, con registros mínimos o ausencia de precipitación en el sector norte del área de estudio y una mayor concentración de los valores de precipitación en el sector sureste. Esta distribución se traduce en una respuesta espacialmente heterogénea del coeficiente de escorrentía (CE), que expresa tanto la variabilidad del evento como la influencia de las características del complejo suelo-vegetación en la generación de escurrimiento.

La Tabla 14.15 y la Figura 14.12 sintetizan la partición media de la precipitación en los tres escenarios de tormenta analizados, expresada como proporción de la lámina precipitada destinada al escurrimiento (CE), al flujo de infiltración (CF) y a las abstracciones iniciales (CIo). Se observa una modificación progresiva de esta distribución a medida que aumenta la magnitud de la precipitación. En el escenario I (33 mm), predominan las abstracciones iniciales, que representan el 60,0 % de la precipitación, mientras que el CE alcanza el 13,2 %. En el escenario II (89 mm), se incrementa la proporción destinada al escurrimiento (36,2 %) y al CF (39,8 %), mientras que el CIo disminuye al 23,9 %. Esta tendencia se acentúa en el escenario III (145 mm), donde el escurrimiento representa el 50,2 % de la precipitación, superando tanto al CF (34,6 %) como al CIo (15,2 %).

Este comportamiento evidencia que, ante eventos pluviales de mayor magnitud, las abstracciones iniciales representan una proporción cada vez menor de la precipitación total, mientras que aumenta la fracción que contribuye al escurrimiento superficial. En términos de lámina, los valores de escurrimiento (Q) pasan de 4,4 mm en el escenario I a 32,2 mm en el escenario II y 72,8 mm en el escenario III. En consecuencia, los volúmenes escurridos estimados alcanzan los 77, 572 y 1.292 hm³, respectivamente. Estos valores se obtienen como producto de la lámina media de escurrimiento por la superficie de la cuenca y deben interpretarse como estimaciones del volumen de excedente generado, dado que el método aplicado no contempla los procesos de tránsito ni el almacenamiento del agua en la red de drenaje.

**Tabla 14.15.** Valores medios ponderados por superficie de los componentes del balance en la cuenca A1.

| Escenario | P (mm) | CE (%) | CF (%) | CIo (%) | Q (mm) | F (mm) | I₀ (mm) | Volumen escurrido (hm³) |
|---|---|---|---|---|---|---|---|---|
| I | 33 | 13,2 | 26,8 | 60,0 | 4,4 | 8,8 | 19,8 | 77 |
| II | 89 | 36,2 | 39,8 | 23,9 | 32,2 | 35,5 | 21,3 | 572 |
| III | 145 | 50,2 | 34,6 | 15,2 | 72,8 | 50,1 | 22,1 | 1.292 |

*Fuente: elaboración propia. En los complejos con I₀ ≥ P, la abstracción se limita al valor de P.*

```{figure} imagenes/geoq/fig_14-12_particion_media.png
:align: center
:width: 80%

Proporción media ponderada de la precipitación destinada al escurrimiento (CE), al flujo de infiltración (CF) y a las abstracciones iniciales (CIo) para los escenarios I, II y III. Fuente: elaboración propia.
```

(geoq-consideraciones)=
### Consideraciones finales

Los resultados muestran que, aun en una cuenca dominada por suelos de alta permeabilidad, la respuesta hidrológica cambia de manera sustancial con la magnitud de la tormenta. Por ejemplo, con lluvias frecuentes, la cuenca retiene la mayor parte del agua en abstracciones e infiltración, y con lluvias extremas, la mitad de la precipitación se transforma en escorrentía directa.

Los complejos de herbáceas y cultivos sobre suelos de los grupos C y D, y los cuerpos de agua, constituyen las zonas críticas de la cuenca. En ellos, los eventos moderados ya producen coeficientes superiores al 57 %, y en una llanura con capacidad de evacuación limitada esa escorrentía tiende a acumularse y a prolongar los anegamientos {cite:p}`geoq-montealegre2021b`.

La sensibilidad a la condición de humedad antecedente es igualmente determinante. Los resultados de este ejemplo corresponden a la CHA II (Tabla 14.3). Sin embargo, el NC de cada complejo suelo-vegetación aumenta con el estado de humedad del suelo (Tabla 14.18) y, con él, la proporción de la lluvia que se convierte en escorrentía. Es decir, la misma tormenta produce algo más del doble de escorrentía sobre un suelo húmedo o saturado que sobre un suelo en condición media. En consecuencia, los coeficientes presentados en esta sección no deben extrapolarse a otras condiciones de humedad: si el evento ocurre después de un período lluvioso (más de 38,1 mm en los 5 a 10 días previos) el CE será mayor, y si ocurre después de un período seco (menos de 12,7 mm) será menor.


El análisis presenta dos limitaciones principales.

- El uso de un valor único de precipitación para una superficie de más de 17.000 km². La dispersión entre las estaciones (Tabla 14.10) indica que la lluvia no es homogénea en la cuenca {cite:p}`geoq-montealegre2021b`. La incorporación de precipitación ráster (en la versión 2.8 de GeoQ) permite superar esta restricción mediante productos satelitales como PDIR-Now, cuya representación de la variabilidad espacial de eventos concretos constituye la línea de continuidad natural de este caso de estudio.
- La segunda es la ausencia de validación con observaciones. Los coeficientes obtenidos no se contrastaron con aforos ni con superficies anegadas observadas. La comparación con la extensión de agua superficial derivada de imágenes de radar u ópticas posteriores a un evento permitiría evaluar la capacidad predictiva del modelo en la llanura.

---

(geoq-conclusiones)=
## Conclusiones y recomendaciones de uso

### Conclusiones

GeoQ 2.8 automatiza en el entorno de QGIS la secuencia completa del método del Número de Curva, que se inicia con el cruce de la cartografía edáfica y de cobertura vegetal, la asignación del NC según la condición de humedad antecedente, el cálculo del balance del evento y la zonificación de sus componentes.

La estandarización de los insumos mediante los códigos `Cod_Sue` y `Cod_Veg` transforma un procedimiento manual extenso y propenso a errores en una operación reproducible y a nivel geoespacial. Esa misma estandarización permite repetir el análisis para distintas tormentas, condiciones de humedad o configuraciones de uso del suelo con un esfuerzo mínimo.

El ejemplo de aplicación en la cuenca A1 del río Salado demuestra la utilidad de la herramienta en una cuenca de llanura de gran extensión, que permitió cuantificar cómo una cuenca dominada por suelos permeables pasa de retener la mayor parte de las lluvias frecuentes a transformar en escorrentía directa la mitad de una lluvia extrema. También procedió a identificar los complejos suelo-vegetación que concentran la generación de excedentes, como es el de las herbáceas y los cultivos sobre suelos de los grupos C y D, y los cuerpos de agua.

Es importante mencionar, que la determinación de los coeficientes de infiltración y de abstracciones iniciales aportan información complementaria sobre la capacidad de regulación del territorio, que no se obtiene del coeficiente de escorrentía por sí solo.

El análisis confirma, asimismo, que la condición de humedad antecedente es un factor tan determinante como la magnitud de la tormenta y su elección debe responder al propósito de la evaluación. La interpretación de los resultados debe considerar los supuestos del método: modelo de evento, ausencia de tránsito y de topografía explícita, y valor orientativo de sus estimaciones.

### Recomendaciones de uso

Por su carácter libre, su bajo requerimiento de datos y la claridad de sus productos, GeoQ resulta adecuado para los siguientes ámbitos de aplicación:

- **Planificación territorial y ordenamiento agrohidrológico.** La zonificación del CE y del CF permite identificar los sectores en los que conviene conservar o ampliar coberturas con mayor capacidad de retención y aquellos en los que deben priorizarse prácticas de manejo del suelo {cite:p}`geoq-mintegui1990,geoq-rodriguez2010`.
- **Evaluación de la vulnerabilidad hidrológica y gestión del riesgo de inundaciones.** La cartografía de escenarios, en particular en CHA III, delimita las zonas críticas de generación de excedentes y aporta una base espacialmente explícita para priorizar intervenciones.
- **Análisis de escenarios de cambio de uso del suelo.** La sustitución de la capa de cobertura permite comparar la respuesta de la cuenca ante la expansión agrícola, la urbanización, la forestación o la invasión de especies leñosas {cite:p}`geoq-deantueno2020,geoq-gaspari2021`.
- **Evaluación de servicios ecosistémicos.** La proporción de lluvia infiltrada frente a tormentas modales o de recurrencia conocida constituye un indicador de regulación hídrica {cite:p}`geoq-laterra2011`. En ámbitos urbanos, permite valorar el papel de los espacios verdes en la mitigación de las precipitaciones intensas {cite:p}`geoq-montico2019`.
- **Docencia y transferencia.** La transparencia del cálculo y la disponibilidad de la herramienta la convierten en un recurso didáctico para la enseñanza de la hidrología de superficie y del manejo de cuencas.

En cuanto a su aplicación operativa, se recomienda:

- **Adecuar la escala de los insumos a la del análisis.** El nivel de detalle de las capas de suelo y de cobertura condiciona el de los resultados, y la reclasificación de las leyendas locales a la tipología de GeoQ debe documentarse.
- **Fundamentar la elección de la CHA** en el registro pluviométrico de los días previos al evento y, en estudios de riesgo, evaluar como mínimo las condiciones II y III.
- **Utilizar precipitación ráster en áreas extensas o con alta variabilidad pluvial.** Productos como PDIR-Now permiten modelizar eventos concretos con su distribución espacial.
- **Contrastar los resultados con observaciones** siempre que sea posible, como aforos, marcas de inundación o superficies anegadas detectadas mediante teledetección, y tratar las estimaciones como orientativas cuando no se disponga de ellas.
- **Complementar el análisis con la topografía** en cuencas con relieve marcado, por ejemplo, mediante el cruce del NC con la pendiente para evaluar el potencial erosivo {cite:p}`geoq-gaspari2025`.

### Perspectivas

El desarrollo de GeoQ prevé la incorporación de modelos digitales de elevación para considerar la influencia de la pendiente en la respuesta hidrológica {cite:p}`geoq-montealegre2021a`. Junto con la precipitación distribuida ya disponible, este avance permitiría acercar la herramienta a una modelización plenamente distribuida del proceso de precipitación-escorrentía.

El repositorio del complemento permanece abierto a comentarios y sugerencias de la comunidad de usuarios. Esta apertura constituye el principal mecanismo para su mejora continua y para su adopción en la gestión de los recursos suelo, agua y vegetación.

(geoq-recursos)=
### Recursos complementarios

Los siguientes recursos permiten reproducir el procedimiento descrito en este capítulo:

- **Complemento GeoQ en el repositorio oficial de QGIS:** <https://plugins.qgis.org/plugins/GeoQ-main/>
- **Código fuente, manual de usuario y documentación:** <https://github.com/fabalmon/GeoQ>
- **Videotutorial de la versión 2.8:** <https://youtu.be/iXmaXzW4_aQ>
- **Portal de datos de precipitación satelital del CHRS:** <https://chrs.web.uci.edu> y <https://chrsdata.eng.uci.edu/>
- **Repositorio institucional SEDICI (UNLP), con trabajos que aplican GeoQ:** <http://sedici.unlp.edu.ar>

La Tabla 14.16 reúne los trabajos que han aplicado GeoQ, según el registro de Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`.

**Tabla 14.16.** Trabajos de aplicación de GeoQ.

| Tipo | Autores y año | Tema o ámbito de aplicación |
|---|---|---|
| Artículo | Rodríguez Vagaría y Gaspari {cite:yearpar}`geoq-rodriguez2010` | Primera versión de GeoQ en Idrisi Andes® |
| Artículo | Rodríguez Vagaría et al. {cite:yearpar}`geoq-rodriguez2012` | Efecto de la restauración agrohidrológica |
| Artículo | Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2021a` | GeoQ en QGIS; cuatro cuencas testigo bonaerenses |
| Congreso | Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2021b` | Cuenca A1 del río Salado; escenarios pluviales |
| Congreso | Botán, Delgado y Gaspari {cite:yearpar}`geoq-botan2022` | Cuenca serrana bonaerense ante eventos pluviales y cambio de uso |
| Congreso | Anselmino, de Antueno y Gaspari {cite:yearpar}`geoq-anselmino2022` | Cuenca alta del arroyo Napaleofú |
| Congreso | Gaspari, Montealegre Medina, Besteiro, Velázquez y Delgado {cite:yearpar}`geoq-gaspari2023` | Procesos hidrológicos en áreas urbanas y rurales |
| Congreso | Mondelo, Mazzucchelli, Jelinski y Gaspari {cite:yearpar}`geoq-mondelo2023` | Escenarios pluviales extremos en la cuenca alta del río Reconquista |
| Congreso | Sáenz, de Antueno y Gaspari {cite:yearpar}`geoq-saenz2024` | Cuenca alta del arroyo Napaleofú |
| Congreso | Besteiro et al. {cite:yearpar}`geoq-besteiro2024` | Precipitación-escorrentía en el sistema serrano de Ventania |
| Congreso | Gaspari et al. {cite:yearpar}`geoq-gaspari2024` | Análisis geohidrológico de la cuenca del arroyo Las Piedras |
| Congreso | Anselmino, Giovenale, Galarco y Gaspari {cite:yearpar}`geoq-anselmino2024` | Respuesta hidrológica ante la invasión de *Gleditsia triacanthos* L. |
| Tesis de posgrado | Guzmán Guaraca (doctorado) | Vulnerabilidad hídrica por cambio de uso en cuencas serranas |
| Tesis de posgrado | Rodríguez Vagaría (maestría) | Restauración agrohidrológica en la sierra de Pillahuinco |
| Tesis de posgrado | Senisterra (maestría) | Cambio de uso del suelo en la cuenca del arroyo Napaleofú |
| Tesis de grado | Anselmino | Servicios hidrológicos ecosistémicos en la cuenca alta del arroyo Napaleofú |
| Tesis de grado | Giovenale | Servicios hidrológicos ambientales en Pinamar |
| Tesis de grado | Puigdemasa | Avance de especies exóticas y dinámica hídrica en la cuenca del arroyo Ventana |

*Fuente: Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`.*

---

(geoq-anexo)=
## Anexo

**Tabla 14.17.** Códigos de vegetación y/o uso del suelo (Cod_Veg) utilizados por GeoQ.

| Cod_Veg | Cobertura del suelo (LCCS, PNECO 1643) | Vegetación y/o uso del suelo (Mintegui Aguirre y López Unzú {cite:yearpar}`geoq-mintegui1990`) |
|---|---|---|
| 10 | Áreas naturales desnudas consolidadas | Barbecho |
| 20 | Herbáceas cerradas en áreas regularmente inundadas/anegadas o acuáticas | Cultivos alineados R P |
| 30 | Cultivos de herbáceas graminoides | Cultivos alineados R B |
| 40 | — | Cultivos alineados C P |
| 50 | — | Cultivos alineados C B |
| 60 | — | Cultivos alineados C-T P |
| 70 | — | Cultivos alineados C-T B |
| 80 | — | Cultivos no alineados o surcos pequeños R P |
| 90 | Cultivos de herbáceas (no determinado) | Cultivos no alineados o surcos pequeños R B |
| 100 | — | Cultivos no alineados o surcos pequeños C P |
| 110 | — | Cultivos no alineados o surcos pequeños C B |
| 120 | — | Cultivos no alineados o surcos pequeños C-T P |
| 130 | — | Cultivos no alineados o surcos pequeños C-T B |
| 140 | — | Cultivos densos leguminosas R P |
| 150 | Cultivos de herbáceas no graminoides | Cultivos densos leguminosas R B |
| 160 | — | Cultivos densos leguminosas C P |
| 170 | — | Cultivos densos leguminosas C B |
| 180 | — | Cultivos densos leguminosas C-T P |
| 190 | — | Cultivos densos leguminosas C-T B |
| 200 | — | Matorral, mezcla matorral y maleza Cubierta 50% |
| 210 | — | Matorral, mezcla matorral y maleza Cubierta 50-75% |
| 220 | — | Matorral, mezcla matorral y maleza Cubierta >75% |
| 230 | Arbustal abierto | Montes con pastos P |
| 240 | Arbustal disperso | Montes con pastos R |
| 250 | Arbustal cerrado | Montes con pastos B |
| 260 | Bosques abiertos en áreas regularmente inundadas/anegadas o acuáticas | Bosques MP |
| 270 | Bosques abiertos | Bosques P |
| 280 | Bosques abiertos, con arbustos | Bosques R |
| 290 | Bosques cerrados en áreas regularmente inundadas/anegadas o acuáticas | Bosques B |
| 300 | Bosques cerrados | Bosques MB |
| 310 | Áreas terrestres cultivadas y/o manejadas | Prados permanentes |
| 320 | Pastizal abierto (herbáceas graminoides) | Pastizal P |
| 330 | Pastizal disperso | Pastizal R |
| 340 | Pastizal cerrado (herbáceas graminoides) | Pastizal B |
| 350 | — | Pastizal C P |
| 360 | — | Pastizal C R |
| 370 | — | Pastizal C B |
| 380 | Áreas urbanas de baja densidad | Residencial baja densidad |
| 390 | Áreas urbanas densamente pobladas | Residencial alta densidad |
| 400 | Rocas, fragmentos | Superficies impermeables |
| 410 | Cuerpos de agua en movimiento - cañadas; Cuerpos de agua estacionario - bañado; Nieve o hielo | Espejos de agua - humedales |
| 420 | Árboles dispersos en áreas regularmente inundadas/anegadas o acuáticas | — |
| 430 | Arbustal cerrado a abierto en áreas regularmente inundadas/anegadas o acuáticas | — |
| 440 | Áreas urbanas de densidad media | — |
| 450 | Áreas urbanas dispersas - semiurbanas | — |
| 460 | Áreas urbanas vegetadas | — |
| 470 | Cultivos de arbustos bajo riego | — |
| 480 | Cultivos de arbustos en secano | — |
| 490 | Cultivos de especies arbóreas (no determinado) | — |
| 500 | Cultivos de especies arbóreas bajo riego | — |
| 510 | Cultivos de especies arbóreas en secano | — |
| 520 | Dunas / desiertos naturales | — |
| 530 | Salares (GH D - áreas/rocas impermeables) | — |
| 540 | Salinas (GH D - áreas/rocas impermeables) | — |
| 550 | Suelos pedregosos | — |
| 560 | Superficies artificiales y áreas asociadas. Superficies construidas (no determinado) - Distritos urbanos comerciales | — |
| 570 | Superficies no construidas | — |
| 580 | Vegas de ríos en condiciones óptimas | — |

*Referencias: R, laboreo sin prácticas de conservación; C, curvas de nivel; C-T, curvas de nivel y terrazas abiertas. Condición de infiltración: MP, muy pobre; P, pobre; M, mala; B, buena; MB, muy buena. Fuente: Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`, adaptado de Volante {cite:yearpar}`geoq-volante2006`, INTA {cite:yearpar}`geoq-inta2009` y Mintegui Aguirre y López Unzú {cite:yearpar}`geoq-mintegui1990`.*

**Tabla 14.18.** Número de curva por clase de cobertura (Cod_Veg), grupo hidrológico (A a D) y condición de humedad antecedente (I, II y III). El Cod_NC de cada celda es la suma del Cod_Veg y el código del GH (A = 1, B = 2, C = 3, D = 4).


| Cod_Veg | A-I | A-II | A-III | B-I | B-II | B-III | C-I | C-II | C-III | D-I | D-II | D-III |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 10 | 58 | 77 | 89 | 72 | 86 | 93 | 81 | 91 | 96 | 87 | 94 | 97 |
| 20 | 52 | 72 | 86 | 64 | 81 | 91 | 75 | 88 | 94 | 81 | 91 | 96 |
| 30 | 46 | 67 | 82 | 60 | 78 | 89 | 70 | 85 | 93 | 77 | 89 | 95 |
| 40 | 49 | 70 | 84 | 61 | 79 | 90 | 69 | 84 | 92 | 75 | 88 | 94 |
| 50 | 44 | 65 | 81 | 56 | 75 | 87 | 66 | 82 | 91 | 72 | 86 | 93 |
| 60 | 45 | 66 | 82 | 54 | 74 | 87 | 63 | 80 | 90 | 66 | 82 | 91 |
| 70 | 41 | 62 | 79 | 51 | 71 | 85 | 60 | 78 | 89 | 64 | 81 | 91 |
| 80 | 44 | 65 | 81 | 57 | 76 | 88 | 69 | 84 | 92 | 75 | 88 | 94 |
| 90 | 42 | 63 | 80 | 56 | 75 | 87 | 67 | 83 | 92 | 74 | 87 | 94 |
| 100 | 42 | 63 | 80 | 54 | 74 | 87 | 66 | 82 | 91 | 70 | 85 | 93 |
| 110 | 40 | 61 | 78 | 53 | 73 | 86 | 64 | 81 | 91 | 69 | 84 | 92 |
| 120 | 40 | 61 | 78 | 52 | 72 | 86 | 61 | 79 | 90 | 66 | 82 | 91 |
| 130 | 38 | 59 | 77 | 49 | 70 | 84 | 60 | 78 | 89 | 64 | 81 | 91 |
| 140 | 45 | 66 | 82 | 58 | 77 | 89 | 69 | 84 | 92 | 75 | 88 | 94 |
| 150 | 37 | 58 | 76 | 52 | 72 | 86 | 64 | 81 | 91 | 70 | 85 | 93 |
| 160 | 43 | 64 | 80 | 56 | 75 | 87 | 67 | 83 | 92 | 70 | 85 | 93 |
| 170 | 34 | 55 | 74 | 48 | 69 | 84 | 60 | 78 | 89 | 67 | 83 | 92 |
| 180 | 42 | 63 | 80 | 53 | 73 | 86 | 63 | 80 | 90 | 67 | 83 | 92 |
| 190 | 30 | 51 | 71 | 46 | 67 | 82 | 57 | 76 | 88 | 63 | 80 | 90 |
| 200 | 28 | 48 | 68 | 46 | 67 | 82 | 58 | 77 | 89 | 67 | 83 | 92 |
| 210 | 18 | 35 | 55 | 35 | 56 | 75 | 49 | 70 | 84 | 58 | 77 | 89 |
| 220 | 15 | 30 | 50 | 28 | 48 | 68 | 44 | 65 | 81 | 53 | 73 | 86 |
| 230 | 26 | 45 | 65 | 45 | 66 | 82 | 58 | 77 | 89 | 67 | 83 | 92 |
| 240 | 19 | 36 | 56 | 39 | 60 | 78 | 53 | 73 | 86 | 61 | 79 | 90 |
| 250 | 12 | 25 | 43 | 34 | 55 | 74 | 49 | 70 | 84 | 58 | 77 | 89 |
| 260 | 35 | 56 | 75 | 56 | 75 | 87 | 72 | 86 | 93 | 81 | 91 | 96 |
| 270 | 26 | 46 | 66 | 47 | 68 | 83 | 60 | 78 | 89 | 69 | 84 | 92 |
| 280 | 19 | 36 | 56 | 39 | 60 | 78 | 49 | 70 | 84 | 57 | 76 | 88 |
| 290 | 13 | 26 | 45 | 31 | 52 | 71 | 42 | 63 | 80 | 48 | 69 | 84 |
| 300 | 7 | 15 | 29 | 25 | 44 | 64 | 33 | 54 | 73 | 40 | 61 | 78 |
| 310 | 15 | 30 | 50 | 37 | 58 | 76 | 51 | 71 | 85 | 60 | 78 | 89 |
| 320 | 47 | 68 | 83 | 61 | 79 | 90 | 72 | 86 | 93 | 77 | 89 | 95 |
| 330 | 29 | 49 | 69 | 48 | 69 | 84 | 61 | 79 | 90 | 69 | 84 | 92 |
| 340 | 21 | 39 | 60 | 40 | 61 | 78 | 54 | 74 | 87 | 63 | 80 | 90 |
| 350 | 27 | 47 | 67 | 46 | 67 | 82 | 64 | 81 | 91 | 75 | 88 | 94 |
| 360 | 12 | 25 | 43 | 38 | 59 | 77 | 56 | 75 | 87 | 67 | 83 | 92 |
| 370 | 3 | 6 | 13 | 18 | 35 | 55 | 49 | 70 | 84 | 61 | 79 | 90 |
| 380 | 33 | 54 | 73 | 49 | 70 | 84 | 63 | 80 | 90 | 70 | 85 | 93 |
| 390 | 58 | 77 | 89 | 70 | 85 | 93 | 79 | 90 | 95 | 83 | 92 | 96 |
| 400 | 95 | 98 | 99 | 95 | 98 | 99 | 95 | 98 | 99 | 95 | 98 | 99 |
| 410 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 |
| 420 | 27 | 47 | 67 | 45 | 66 | 82 | 58 | 77 | 89 | 67 | 83 | 92 |
| 430 | 37 | 57 | 73 | 52 | 72 | 86 | 66 | 82 | 91 | 72 | 86 | 93 |
| 440 | 40 | 61 | 78 | 56 | 75 | 87 | 67 | 77 | 84 | 74 | 83 | 91 |
| 450 | 37 | 57 | 73 | 52 | 72 | 86 | 64 | 81 | 91 | 72 | 86 | 93 |
| 460 | 30 | 51 | 71 | 47 | 68 | 83 | 63 | 79 | 90 | 69 | 84 | 92 |
| 470 | 20 | 37 | 57 | 39 | 60 | 78 | 53 | 73 | 86 | 61 | 79 | 90 |
| 480 | 22 | 42 | 61 | 40 | 62 | 78 | 57 | 75 | 91 | 64 | 81 | 91 |
| 490 | 29 | 49 | 69 | 45 | 65 | 83 | 56 | 75 | 87 | 61 | 79 | 90 |
| 500 | 18 | 35 | 55 | 38 | 59 | 77 | 52 | 72 | 86 | 61 | 79 | 90 |
| 510 | 29 | 49 | 69 | 47 | 68 | 83 | 60 | 78 | 89 | 69 | 84 | 92 |
| 520 | 42 | 63 | 80 | 58 | 77 | 89 | 70 | 85 | 93 | 75 | 88 | 94 |
| 530 | 65 | 83 | 95 | 84 | 93 | 97 | 84 | 93 | 97 | 84 | 93 | 97 |
| 540 | 65 | 83 | 95 | 84 | 93 | 97 | 84 | 93 | 97 | 84 | 93 | 97 |
| 550 | 80 | 91 | 96 | 80 | 91 | 96 | 81 | 91 | 96 | 82 | 92 | 97 |
| 560 | 77 | 89 | 97 | 82 | 92 | 98 | 88 | 94 | 98 | 87 | 95 | 99 |
| 570 | 29 | 49 | 69 | 48 | 69 | 84 | 61 | 79 | 90 | 69 | 84 | 92 |
| 580 | 15 | 30 | 50 | 37 | 58 | 76 | 51 | 71 | 85 | 60 | 78 | 89 |

*Fuente: Montealegre Medina y Gaspari {cite:yearpar}`geoq-montealegre2024a`, adaptado de Mockus {cite:yearpar}`geoq-mockus1972`.*


## Referencias bibliográficas

```{bibliography}
:filter: docname in docnames
```