---
title: "Modelo predictivo de inundaciones en Sarandí del Yí mediante Inteligencia Artificial Geoespacial (GeoAI) y telemetría hidrométrica in situ"
---

# **Modelo predictivo de inundaciones en Sarandí del Yí mediante GeoAI y telemetría hidrométrica in situ**

+ **Autor:* Mario Federico Girona | Fire Behavior Analyst (FBAN)* 

+ **Resumen:** Las inundaciones repentinas en cuencas fluviales de respuesta rápida representan un desafío crítico para la gestión del riesgo de desastres en centros urbanos vulnerables de Uruguay. Este capítulo presenta la formulación matemática, la arquitectura computacional y el protocolo de validación operativa de un sistema hidroinformático basado en **GeoAI** aplicado a la cuenca alta y media del río Yí en la ciudad de Sarandí del Yí (Departamento de Durazno). La metodología integra pronósticos atmosféricos estocásticos de alta resolución generados por Google WeatherNext 3 con el motor hidrológico regional Google Flood Hub Engine, fundamentado en redes neuronales recurrentes Long Short-Term Memory (LSTM) condicionadas por anomalías gravimétricas de almacenamiento de agua terrestre (misión satelital GRACE/GRACE-FO). Los caudales simulados se acoplan dinámicamente a un modelo hidrodinámico apoyado en el Modelo Digital de Elevación Copernicus GLO-30 con un horizonte de anticipación de hasta 72 horas, incorporando validación pluviométrica cruzada con The Weather Channel (TWC), asimilación en tiempo real mediante la estación limnimétrica de DINAGUA y síntesis operativa gobernada por un agente determinista (Gems). Finalmente, se documenta la verificación del sistema durante el escenario basal del 9 de octubre de 2026, confirmando un estado operativo VERDE ($H_{obs} < 3,50\text{ m}$; precipitación proyectada $< 5,0\text{ mm}/72\text{ h}$).

**Palabras clave:** GeoAI; Google WeatherNext 3; Google Flood Hub; DINAGUA; inundaciones urbanas; Sarandí del Yí; redes neuronales LSTM; Copernicus GLO-30.

# Introducción

La cuenca del río Yí, afluente principal de la margen izquierda del río Negro, exhibe en su tramo superior y medio —a la altura de la ciudad de Sarandí del Yí ($33^\circ 20'\text{ S}, 55^\circ 38'\text{ O}$; Departamento de Durazno, Uruguay)— una respuesta hidrológica marcadamente torrencial frente a eventos convectivos severos y sistemas frontales estacionarios. El área de drenaje aportante hasta la sección de control urbano comprende aproximadamente $4.200\text{ km}^2$, caracterizados por suelos de baja permeabilidad relativa sobre basamento cristalino y lomadas sedimentarias. Históricamente, la escasa densidad espacial de estaciones pluviográficas telemétricas en las nacientes de la Cuchilla Grande ha limitado la capacidad de anticipación del Sistema Nacional de Emergencias (SINAE) y del Comité Departamental de Emergencias (CECOED Durazno), dado que el tiempo de concentración ($T_c$) de esta subcuenca oscila entre 24 y 36 horas.

Las citas bibliográficas se incorporan desde el archivo `referencias.bib`. Por ejemplo, el aprendizaje profundo ha tenido un papel relevante en el análisis de imágenes de observación de la Tierra [@zhu2017] y en el desarrollo de la Inteligencia Artificial Geoespacial (GeoAI) [@janowicz2020geoai], impulsando recientemente modelos fundacionales meteorológicos e hidrológicos globales [@lam2023learning; @nearing2024global; @price2025gencast].

La **Figura 1** presenta la localización geográfica de la subcuenca alta y media del río Yí y la sección de control urbano.

![Figura 1. Localización geográfica de la subcuenca alta y media del río Yí (4.200 km²) y sección de control hidrométrico en Sarandí del Yí. Fuente: Elaboración propia.](images/fig_area_estudio.png)

Como se observa en la **Figura 1**, la red de drenaje aguas arriba converge sobre el puente nuevo de Sarandí del Yí, donde opera la estación telemétrica de validación.

# Materiales y metodología

La **Figura 2** presenta el esquema conceptual de la arquitectura hidroinformática GeoAI implementada.

![Figura 2. Esquema de la arquitectura hidroinformática acoplada de Google WeatherNext 3, Flood Hub Engine, validación cruzada TWC, telemetría DINAGUA y Agente Determinista Gems. Fuente: Elaboración propia.](images/fig_arquitectura_geoai.png)

Como se observa en la **Figura 2**, los datos geoespaciales, meteorológicos, gravimétricos y limnimétricos constituyen la entrada para la aplicación acoplada de técnicas de GeoAI.

La **Tabla 1** resume los subsistemas y familias de técnicas que forman parte del modelo.

**Tabla 1. Subsistemas y técnicas vinculadas con GeoAI aplicadas en la cuenca del río Yí. Fuente: Elaboración propia.**

| Subsistema | Modelo / Sensor | Técnica GeoAI / Resolución | Aplicación geoespacial e hidrológica |
|:---|:---|:---|:---|
| Forzante atmosférico | Google WeatherNext 3 | GNN + Difusión ($0,1^\circ \times 0,1^\circ$) | Ensambles de precipitación acumulada (24, 48 y 72 h) |
| Validación sinóptica | The Weather Channel (TWC) | Modelo numérico GRAF | Contraste volumétrico y control de divergencia ($\Delta P$) |
| Motor hidrológico | Google Flood Hub + GRACE | Redes LSTM + Gravimetría TWS | Simulación de caudal $Q_{sim}(t)$ y humedad antecedente |
| Hidrodinámica | Copernicus GLO-30 | DEM 30 m + Saint-Venant / Manning | Cota simulada $H_{sim}(t)$ y mancha de inundación urbana |
| Asimilación in situ | Estación DINAGUA | Telemetría limnimétrica | Cota observada $H_{obs}(t)$ y calibración de rugosidad ($n$) |
| Gobernanza IA | Agente Especializado Gems | LLM con RAG determinista | Emisión de boletines bajo protocolo CECOED / SINAE |

Tal como se sintetiza en la **Tabla 1**, GeoAI permite acoplar modelos fundacionales atmosféricos e hidrológicos con telemetría local en tiempo real.

## Forzante atmosférico: Arquitectura Google WeatherNext 3

El forzante meteorológico primario proviene de Google WeatherNext 3, que combina Redes Neuronales Basadas en Grafos (GNN) sobre una malla icosaédrica multiescala con modelos probabilísticos de difusión generativa en el espacio latente [@lam2023learning; @price2025gencast], generando ensambles estocásticos de precipitación para 24 h, 48 h y 72 h con resolución espacial de $0,1^\circ \times 0,1^\circ$ ($\sim 11\text{ km}$). La partición entre infiltración edáfica y precipitación efectiva se parametriza mediante la formulación exponencial de Horton [@horton1933role]:

$$f(t) = f_c + (f_0 - f_c) \cdot e^{-k \cdot t}$$

donde $f(t)$ representa la tasa instantánea de infiltración ($\text{mm}\cdot\text{h}^{-1}$) en el tiempo $t$, $f_0$ es la capacidad de infiltración máxima inicial en suelo seco, $f_c$ constituye la tasa de infiltración básica con el perfil saturado y $k$ ($\text{h}^{-1}$) es la constante de decaimiento asociada a la textura franco-arcillosa de la cuenca.

## Transformación hidrológica e hidrodinámica: Flood Hub y Copernicus GLO-30

Los hietogramas distribuidos alimentan el núcleo Google Flood Hub Engine, estructurado sobre redes neuronales recurrentes Long Short-Term Memory (LSTM) de aprendizaje regionalizado [@kratzert2018rainfall; @kratzert2019toward; @nearing2024global], cuyo estado oculto asimila mediante downscaling las anomalías regionales de almacenamiento total de agua terrestre (TWS) derivadas de las misiones gravimétricas satelitales GRACE y GRACE-FO [@tapley2004grace]. El caudal líquido simulado $Q_{sim}(t)$ ($\text{m}^3\cdot\text{s}^{-1}$) se propaga mediante las ecuaciones de aguas someras de Saint-Venant bajo aproximación difusiva sobre el Modelo Digital de Elevación Copernicus GLO-30 ($30\text{ m}$; $LE90 < 4\text{ m}$), convirtiéndose en nivel hidrométrico simulado $H_{sim}(t)$ mediante la curva de gasto $Q = f(H)$ y la ecuación de resistencia de Manning:

$$Q_{sim}(t) = \frac{1}{n} \cdot A(H) \cdot R_h(H)^{2/3} \cdot S_f^{1/2}$$

donde $A(H)$ es el área mojada transversal ($\text{m}^2$), $R_h(H)$ es el radio hidráulico ($\text{m}$), $S_f$ es la pendiente motriz de la línea de energía y $n$ ($\text{s}\cdot\text{m}^{-1/3}$) constituye el coeficiente de rugosidad de Manning.

## Validación pluviométrica cruzada y asimilación hidrométrica in situ (DINAGUA)

Para mitigar falsas alarmas convectivas, se ejecuta una validación cruzada frente a los pronósticos sinópticos de The Weather Channel (TWC / GRAF). Si la divergencia relativa $\Delta P = |P_{WN3} - P_{TWC}| / P_{media}$ supera el 35 % en la cuenca alta, el sistema declara estado de incertidumbre meteorológica. En el dominio hidráulico, la validación y el ajuste dinámico se realizan mediante la estación telemétrica automática de DINAGUA en el puente nuevo de Sarandí del Yí, evaluando el error cuadrático medio ($RMSE$) y el coeficiente de eficiencia de Nash-Sutcliffe ($NSE$) [@nash1970river]:

$$RMSE = \sqrt{\frac{1}{N} \sum_{i=1}^{N} \left( H_{obs}(t_i) - H_{sim}(t_i) \right)^2} \quad ; \quad NSE = 1 - \frac{\sum_{i=1}^{N} \left( H_{obs}(t_i) - H_{sim}(t_i) \right)^2}{\sum_{i=1}^{N} \left( H_{obs}(t_i) - \overline{H_{obs}} \right)^2}$$

La asimilación de los registros de DINAGUA permite recalibrar dinámicamente el coeficiente de rugosidad de Manning dentro del intervalo físicamente admisible ($n \in [0,028; 0,065]\text{ s}\cdot\text{m}^{-1/3}$).

## Gobernanza algorítmica e integridad de datos (Agente Especializado Gems)

La integración de flujos de telemetría y la compilación de boletines técnicos están gobernadas por un agente determinista de Inteligencia Artificial (arquitectura Gems) asistido por Generación Aumentada por Recuperación Determinista (RAG) y Restricciones Simbólicas [@mai2024opportunities]: (1) Prohibición absoluta de extrapolación subjetiva fuera de los registros validados por WeatherNext 3, Flood Hub, TWC y DINAGUA; (2) Transparencia ante discontinuidades telemétricas conservando la última cota certificada con su marca temporal; y (3) Estandarización léxica hacia los protocolos del SINAE.

# Resultados operativos y verificación de caso de estudio

El acoplamiento predictivo extiende la ventana de alerta temprana hasta 72 horas antes del impacto urbano. La **Tabla 2** resume la matriz de umbrales operativos armonizada con el CECOED Durazno y el SINAE.

**Tabla 2. Matriz de umbrales hidrométricos, horizontes de anticipación y protocolos de respuesta operativa (CECOED Durazno / SINAE). Fuente: Elaboración propia.**

| Nivel de alerta | Cota hidrométrica ($H$) | Horizonte prev. | Protocolo de acción operativa y gestión territorial |
|:---|:---|:---|:---|
| VERDE (Normal) | $< 3,50\text{ m}$ | 72 h | Monitoreo telemétrico continuo automatizado; flujo confinado en el cauce ordinario menor sin afectación ribereña. |
| AMARILLO (Atención) | $3,50 - 4,80\text{ m}$ | 48 h | Vigilancia activa en zonas bajas costeras y parques ribereños; notificación preventiva a cuadrillas municipales. |
| NARANJA (Alerta) | $4,80 - 6,00\text{ m}$ | 24 h | Aprestamiento logístico de centros de evacuación, corte preventivo de pasarelas inundables y alerta a población ribereña. |
| ROJO (Emergencia) | $> 6,00\text{ m}$ | 12 - 24 h | Activación integral del protocolo de evacuación preventiva asistida por CECOED/SINAE y relocalización de damnificados. |

Tal como se sintetiza en la **Tabla 2**, la corrida de verificación ejecutada el 9 de octubre de 2026 determinó un **Estado de Riesgo VERDE (Normal)**. La telemetría del sensor DINAGUA en el puente nuevo registró niveles estacionarios dentro del cauce menor ($H_{obs} < 3,50\text{ m}$). El pronóstico acoplado WeatherNext 3 validado con TWC ($\Delta P < 12\%$) proyectó precipitaciones marginales ($0,0\text{ a }1,5\text{ mm}$ en 24 h, $0,5\text{ a }2,0\text{ mm}$ en 48 h y $< 1,0\text{ mm}$ en 72 h; acumulado $< 5,0\text{ mm}$). Debido a la condición antecedente seca del suelo ($f(t) > P(t)$), la escorrentía directa estimada resultó nula ($0,0\text{ mm}$), descartando riesgos de anegamiento en Sarandí del Yí.

# Consideraciones finales

El marco hidroinformático desarrollado demuestra que la integración sinérgica de modelos meteorológicos generativos (GNN y difusión), redes hidrológicas recurrentes (LSTM calibradas con gravimetría satelital GRACE), topografía satelital Copernicus GLO-30 y telemetría oficial in situ (DINAGUA) subsana eficazmente la escasez de instrumental denso en cuencas uruguayas de escala intermedia. Como limitación metodológica, dado que el modelo Copernicus GLO-30 representa un modelo de superficie (DSM) que puede sobreestimar cotas en zonas de monte ribereño, se prevé incorporar relevamientos con VANT/LiDAR en futuras etapas operativas.

# Referencias