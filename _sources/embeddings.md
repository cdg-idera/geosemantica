

# Capítulo 1: Embeddings satelitales

Embeddings satelitales: una nueva semántica del territorio

## Introducción conceptual

En los últimos años, la inteligencia artificial ha permitido construir modelos de representación del mundo que *trascienden los píxeles y los valores espectrales*. Los **embeddings** —representaciones numéricas densas de información compleja— constituyen una de las *innovaciones más profundas* en la *intersección entre aprendizaje profundo y Observación de la Tierra (EO)*.
En el dominio lingüístico, un **embedding** *transforma palabras en vectores* que *capturan su significado contextual*; del mismo modo, en el dominio geoespacial, los **embeddings satelitales** traducen la *información espectral, temporal y contextual de cada píxel o región* en *un vector semántico que codifica patrones de superficie, contextos ambientales y relaciones espaciales*. De este modo los embeddings satelitales, nos posibilitan explorar un nuevo espacio asociado a la semántica geoespacial aprendida.

## Introducción: del píxel al concepto

Durante décadas, el análisis de imágenes satelitales se ha basado en valores radiométricos y en la interpretación de índices derivados (NDVI, NDWI, NDBI, etc.), que reflejan fenómenos biofísicos como la vegetación, el agua o lo urbano.
Sin embargo, la revolución del **deep learning** ha transformado la forma en que representamos la información. Hoy, la pregunta ya no es *“¿qué valor tiene este píxel en la banda 4?”*, sino *“¿a qué se parece este píxel en términos de su significado latente?”*.

Los embeddings satelitales representan ese salto conceptual: son **una forma de codificar semánticamente el territorio**.
Cada píxel o región es *proyectado* a *un espacio vectorial de alta dimensión* donde la *distancia matemática refleja similitud contextual y semántica*, *no solo espectral*.
De este modo, el planeta *deja de ser una grilla de reflectancias* y *se convierte en un espacio continuo de conceptos aprendidos*.

## Fundamento teórico: ¿qué es un embedding?

En términos formales, un embedding es una función:

f:X→Rnf: X \rightarrow \mathbb{R}^nf:X→Rn 

donde 
𝑋
X representa un conjunto de observaciones complejas —imágenes multiespectrales, series temporales, o escenas completas—, y Rn\mathbb{R}^nRn es un espacio vectorial latente.
La función f
f se aprende a partir de grandes volúmenes de datos mediante redes neuronales profundas. Su objetivo no es clasificar directamente, sino aprender una representación comprimida y significativa de los datos.

En el dominio de la Observación de la Tierra (EO), esto significa que:

Cada píxel o parche satelital se codifica en un vector de, por ejemplo, 256 dimensiones.

Las relaciones espaciales y espectrales se preservan de modo que píxeles similares en contexto quedan cercanos en el espacio latente.

Los embeddings permiten medir similitud coseno entre lugares, como se mide similitud semántica entre palabras en modelos como Word2Vec o BERT.

Esta idea, proveniente del procesamiento del lenguaje natural, encuentra en las imágenes satelitales una analogía poderosa:
así como los modelos lingüísticos aprenden que “rey” - “hombre” + “mujer” ≈ “reina”, los modelos de EO aprenden que “vegetación densa” - “verde” + “suelo desnudo” ≈ “zona urbana”.

3. Modelos fundacionales y embeddings satelitales

Los modelos fundacionales para Observación de la Tierra (FM4EO) como OneVision, Prithvi, AlphaHertz o el Satellite Embedding V1 de Google, fueron entrenados sobre millones de escenas multitemporales de Sentinel-2, Landsat y MODIS.
Estos modelos aprenden a generar vectores invariantes a cambios atmosféricos, de estación o de sensor, capturando así la esencia estadística del paisaje.

El dataset GOOGLE/SATELLITE_EMBEDDING/V1 disponible en Google Earth Engine representa la primera implementación global de este paradigma: un mapa latente del planeta donde cada píxel está asociado a un vector de 256 dimensiones que codifica su identidad semántica.

4. Estructura matemática y significado de la similitud

La similitud coseno se utiliza como métrica fundamental en este espacio latente.
Dado un vector de referencia 
𝑠
s (por ejemplo, el promedio de los embeddings de un conjunto de polígonos de agua) y un vector de píxel 
𝑥
x, la similitud se define como:

sim
(
𝑠
,
𝑥
)
=
𝑠
⋅
𝑥
∣
∣
𝑠
∣
∣
 
∣
∣
𝑥
∣
∣
sim(s,x)=
∣∣s∣∣∣∣x∣∣
s⋅x
	​


Este valor se reescala a [0,1], donde 1 indica máxima similitud semántica.
Lo notable es que esta similitud no depende de índices espectrales fijos, sino de representaciones aprendidas que capturan patrones espaciales, texturales y de contexto ambiental.

En consecuencia:

Los embeddings permiten buscar por concepto (“lugares similares a este humedal”) en lugar de por valor (“NDWI > 0.4”).

Cada comparación en este espacio vectorial actúa como un razonamiento semántico entre regiones.

5. Aplicaciones ejemplificadas: tres casos de estudio
Caso 1: Detección de cuerpos de agua

El primer script aplica la similitud coseno entre un embedding mosaico anual y vectores promedio derivados de polígonos de agua.
Las zonas con similitud ≥ 0.98, 0.99 y ≈1.0 representan gradientes de confianza en la detección de superficies acuáticas.
Este enfoque elimina la necesidad de índices espectrales ad-hoc y permite detectar agua aún bajo condiciones atmosféricas o de iluminación variables, al capturar su firma semántica global.

Didácticamente, este ejemplo introduce los conceptos de:

Espacio latente

Vector de referencia

Similitud coseno

Umbral semántico probabilístico

Caso 2: Identificación de ladrilleras y áreas industriales

Aquí se introduce una noción más sofisticada: los filtros post fail-open.
El procedimiento parte de una similitud coseno inicial —que indica qué zonas del ROI “se parecen” a las ladrilleras conocidas— y la refina con criterios físicos:

NDVI para vegetación baja,

NDBI para superficies construidas,

BSI para suelo desnudo,

S1 VV/VH para textura radar coherente.

El sistema fail-open aplica un filtro solo si mantiene cobertura suficiente, garantizando robustez.
De este modo, el embedding aporta la capa semántica, mientras los índices ópticos y radar aportan la validación física, logrando una integración GeoIA de segunda generación.

Conceptos formativos aquí:

Embeddings como filtros semánticos primarios.

Fusión multimodal: espectral + radar + latente.

Estrategias de control de falsos negativos en detección.

Caso 3: Clasificación supervisada de cultivos

El tercer caso extiende los embeddings hacia el aprendizaje supervisado.
En lugar de calcular similitudes, los vectores de embedding se utilizan como features para entrenar clasificadores como Random Forest o SVM.
Cada muestra agrícola (vid, manzana, pera, alfalfa, horticultura) se representa como un punto en el espacio latente, y el modelo aprende fronteras de decisión entre clases.

La ventaja es que los embeddings:

ya están pre-entrenados globalmente (no hace falta calibrar índices locales),

reducen la variabilidad interanual,

y permiten entrenar modelos robustos con pocas muestras.

Este ejemplo articula:

Transfer learning en EO,

Representación universal de la superficie terrestre,

Reutilización semántica de embeddings para clasificación.

6. Perspectiva epistemológica: hacia una geosemántica del territorio

Los embeddings implican un cambio de paradigma epistemológico en la geografía digital.
Ya no trabajamos con valores brutos ni índices espectrales, sino con vectores que representan significados aprendidos.
Este cambio aproxima la Observación de la Tierra al campo del lenguaje y la cognición: los píxeles “hablan entre sí” en un idioma estadístico de 256 dimensiones.

Podemos entonces hablar de una geosemántica latente, donde el territorio se interpreta como un texto y el embedding como su gramática.
Cada lugar posee un “significado distribuido” en el espacio vectorial, lo que permite realizar búsquedas conceptuales, analogías espaciales y análisis de cambio semántico.

Por ejemplo:

“¿Qué zonas se están desplazando semánticamente desde ‘vegetación natural’ hacia ‘cultivos’?”

“¿Qué regiones urbanas presentan una firma latente similar a Rosario?”

Este enfoque abre un campo nuevo: la GeoIA semántica, que combina fundamentos de la lingüística estadística, la visión por computadora y la geografía cuantitativa.

7. Síntesis y conclusiones

Los embeddings satelitales no reemplazan a los métodos tradicionales, sino que los trascienden: integran lo espectral, lo espacial y lo contextual en una representación unificada.
En los tres ejemplos —agua, ladrilleras, cultivos— vemos cómo la similitud coseno actúa como una métrica de “familiaridad territorial”.

Su potencial radica en permitir:

búsquedas semánticas planetarias,

clasificación auto-supervisada,

detección de cambios multiescala,

y la construcción de datacubes semánticos.

Así como los modelos de lenguaje transformaron la comunicación, los embeddings están transformando nuestra forma de leer el territorio.
Ya no observamos bandas: interpretamos significados.
El desafío científico y didáctico consiste ahora en enseñar a pensar el territorio en clave semántica, es decir, en un espacio de relaciones aprendidas.

Referencias sugeridas

Google Research (2024). Satellite Embedding V1: A Foundation Model for Planetary Understanding.

Zhu et al. (2023). Prithvi: Self-Supervised Learning for Earth Observation. IEEE TGRS.

Tuia, D. et al. (2022). Deep Learning in Earth Observation: Foundations and Trends.

Montero, C. (2024). GeoIA y Datacubes en la era semántica. IDE Iberoamérica.

Lillesand, T., Kiefer, R., & Chipman, J. (2015). Remote Sensing and Image Interpretation. Wiley.

# Mas

El embedding captura contexto más que forma fina.

Util para detectar por ejemplo: 

* Océano / grandes cuerpos de agua
Muy distintivos y homogéneos.
Tip: máscara JRC Water (occurrence ≥ 10–30%).

* Lagos/embalses medianos–grandes
Formas estables, contraste claro con tierra.
Tip: JRC Water + picos locales sobre el mapa de similitud.

* Áreas urbanas densas (CBD, manzanas compactas)
Textura “gruesa”, patrones de calles/edificios.
Tip: NDBI alto, NDVI bajo para recortar candidatos.

* Zonas industriales/portuarias grandes
Superficies duras, depósitos, muelles, contenedores.
Tip: NDBI↑, NDVI↓, Sentinel-1 VV/VH moderado–alto.

* Mosaicos agrícolas extensos (parcelas, pivotes)
Geometría repetitiva; muy “aprendible”.
Tip: recortar a áreas rurales y usar época anual similar.

* Canteras / minas a cielo abierto
Texturas minerales, taludes, caminos internos.
Tip: NDBI↑, NDVI↓, SWIR↑, S1 VV/VH↑.

* Bosques densos / masas forestales
Textura homogénea y patrón regional.
Tip: NDVI↑ para filtrar no-vegetación.

* Salinas / salar
Reflectancia y textura características, grandes extensiones.
Tip: SWIR/NIR peculiar; conviene recortar con máscara de suelo desnudo.

* Humedales extensos
Mezcla agua-vegetación pero patrón espacial distintivo.
Tip: JRC (occurrence medio) + NDVI medio/alto.

* Infraestructura lineal grande (autopistas anchas, pistas de aeropuertos)
Linealidad clara a escala 10–20 m.
Tip: detectar por similitud + post-procesar con filtros morfológicos.

* Parques eólicos / solares grandes
Patrón repetitivo (aerogeneradores/filas de paneles).
Tip: S1 ayuda (estructuras metálicas dispersas), NDBI↑, NDVI↓.

* Barrios privados / countries con diseño característico
Huella y traza interna repetida, lagunas artificiales.
Tip: recortar con urbano (NDBI↑) y usar varias muestras.

* Playas / dunas extensas
Textura/tonalidad de arena, bordes costeros.
Tip: excluir agua con JRC y vegetación con NDVI↓.



# Busqueda: Agua

0) Entradas

Este bloque asume que ya existen dos insumos:

roi: tu región de estudio (Geometry o FeatureCollection).

samples: polígonos de referencia que representan agua.

Se extrae la geometría con var geometry = roi.geometry(); y se arma una vista rápida en el mapa:

Map.setOptions('SATELLITE') para usar el fondo satelital.

Map.addLayer(roi, …) dibuja el área de estudio.

Map.addLayer(samples, …) muestra tus polígonos de agua en celeste.

Map.centerObject(geometry, 7) centra el mapa en la zona.

1) Parámetros

Define los controles del análisis:

Ventana temporal del año 2024 (startDate, endDate).

scale: resolución de trabajo para los reductores y vectorización (20 m es un buen punto medio para cuerpos de agua no muy pequeños).

USE_JRC_MASK: si true, se enmascara el embedding con la capa JRC Global Surface Water, útil para descartar píxeles no acuáticos.

JRC_OCC_MIN: umbral mínimo de “ocurrencia de agua” (0–100). A mayor valor, más conservador.

MIN_AREA_SQM: área mínima para filtrar polígonos pequeños (“slivers”) en la vectorización final.

2) Embedding anual

Carga la colección de embeddings anuales de Google:

ee.ImageCollection('GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL').
Filtra por fechas, hace un mosaic() (apila y toma el primer pixel válido por banda en el período) y recorta a geometry.
Se guarda la lista de bandas en bandNames porque luego se necesitan para construir vectores y para asegurarse de alinear nombres al convertir a arrays.

3) (Opcional) Máscara de agua JRC

Si USE_JRC_MASK es true, se enmascaran todos los píxeles cuyo occurrence (JRC/GSW1_4) sea menor que JRC_OCC_MIN. Esto reduce ruido y acelera el cálculo enfocando sólo en áreas con probabilidad de agua.
Luego, refImage = mosaicMasked.unmask(0) rellena con 0 donde la máscara dejó huecos, evitando nulls en los reductores (muy importante para construir vectores sin valores faltantes).

4) Vector de referencia por POLÍGONO (promedio)

Se calcula un vector de referencia por cada polígono de muestra:

La función fillNullsWithZeros reemplaza valores null por 0 para todas las bandas (así cada polígono termina con un vector completo).

reduceRegion({reducer: ee.Reducer.mean(), geometry: f.geometry(), …}) promedia el embedding sobre todo el polígono de muestra (no sólo un punto).

Se agregan esos promedios como propiedades al feature (return f.set(dFilled)).
El resultado samplesMeanVec es una FC donde cada feature representa un polígono de agua y, además, contiene un vector promedio del embedding (una dimensión por banda).

5) Similitud coseno y “máximo por muestra”

Se normaliza el mosaico a norma-1 por píxel:

mosaicUnit = mosaicMasked / ||mosaicMasked||, donde ||·|| es la norma euclídea por píxel (raíz de la suma de cuadrados de todas las bandas).
Para cada polígono de muestra:

Se convierte el diccionario de bandas del feature a imagen (f.toArray(bandNames) → arrayFlatten) y se normaliza a unidad (sUnit).

La similitud coseno es el producto punto entre ambos vectores normalizados: sUnit * mosaicUnit reducido por suma → rango [-1, 1].

Se reescala a [0, 1] con (cos + 1) / 2 y se llama cosine_closeness.
Se queda con el máximo de todas las similitudes por píxel (cosinePerSample.max()), es decir, qué tan parecido es cada píxel a al menos uno de tus polígonos de agua. Esto captura la mejor coincidencia en lugar del promedio.

(Se incluye una capa opcional continua para visualizar cosineMax en el mapa con una paleta perceptual).

6) Polígonos por umbral

La función areasAtThreshold(img, th):

Umbraliza img (por ejemplo cosineMax >= 0.98) y genera una máscara binaria.

Vectoriza con reduceToVectors, respetando geometry, scale y conectividad de 8 vecinos.

Calcula atributos útiles:

cosine_close: el máximo de closeness dentro del polígono (sirve para ordenar o filtrar después).

area_m2: área en m² del polígono (f.geometry().area(scale)).

(Opcional) filtra por MIN_AREA_SQM si lo definiste > 0.
Luego se generan tres colecciones de polígonos: poly98 (≥ 0.98), poly99 (≥ 0.99) y poly100 (≥ 0.999). El 1.0 exacto es raro por precisión numérica, por eso ≈100% se modela con 0.999.

7) Mostrar polígonos

Se agregan al mapa las tres capas vectoriales:

Amarillo: ≥ 98%

Naranja: ≥ 99%

Cian: ≈ 100%
Se imprime además la cantidad de polígonos en cada umbral para una verificación rápida del resultado.

8) (Opcional) Exportar a Asset

Une todas las capas (poly98.merge(poly99).merge(poly100)) en una sola FeatureCollection y deja listo un bloque de Export.table.toAsset (comentado) para guardar los polígonos resultantes en tu proyecto (edita assetId y description según corresponda).
Esto permite versionar y reutilizar los resultados sin recalcular toda la tubería.

```javascript
// ============================================================
// DETECCIÓN DE CUERPOS DE AGUA POR SIMILARIDAD DE EMBEDDINGS
// Requiere: 'roi' (Geometry/FC) y 'samples' (FC de POLÍGONOS de agua)
// Salida: POLÍGONOS ≥98%, ≥99% y ≈100% (cosine closeness)
// ============================================================

// ---------- 0) Entradas ----------

// Definir roi (region de interés o estudio): Prov. Neuquén y Rio Negro

var geometry = roi.geometry();

Map.setOptions('SATELLITE');
Map.addLayer(roi, {color: 'red'}, 'ROI', true);
Map.addLayer(samples, {color: 'deepskyblue'}, 'Samples (polígonos de agua)', true);
Map.centerObject(geometry, 7);

// ---------- 1) Parámetros ----------
var year = 2024;
var startDate = ee.Date.fromYMD(year, 1, 1);
var endDate   = startDate.advance(1, 'year');

var scale = 20;        // 10–30 m según el tamaño de cuerpos de agua
var USE_JRC_MASK = true;   // ← poné false si NO querés enmascarar por agua
var JRC_OCC_MIN  = 30;     // % de ocurrencia mínima (10–50 típico)
var MIN_AREA_SQM = 0;      // área mínima para descartar slivers (p.ej. 5000)

// ---------- 2) Embedding anual ----------
var embeddings = ee.ImageCollection('GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL');
var mosaic = embeddings
  .filterDate(startDate, endDate)
  .mosaic()
  .clip(geometry);

var bandNames = mosaic.bandNames();

// ---------- 3) (Opcional) Máscara de agua JRC ----------
var mosaicMasked = ee.Image(ee.Algorithms.If(
  USE_JRC_MASK,
  mosaic.updateMask(ee.Image('JRC/GSW1_4/GlobalSurfaceWater')
    .select('occurrence').gte(JRC_OCC_MIN)),
  mosaic
));

// Para construir vectores de referencia, evitamos nulls:
var refImage = mosaicMasked.unmask(0);

// ---------- 4) Vector de referencia por POLÍGONO (promedio) ----------
function fillNullsWithZeros(dict, bands) {
  dict = ee.Dictionary(dict);
  bands = ee.List(bands);
  return ee.Dictionary(
    bands.iterate(function(b, acc) {
      b = ee.String(b);
      var v = dict.get(b);
      v = ee.Algorithms.If(v, v, 0); // null -> 0
      return ee.Dictionary(acc).set(b, v);
    }, ee.Dictionary({}))
  );
}

var samplesMeanVec = ee.FeatureCollection(samples)
  .filterBounds(geometry)
  .map(function(f) {
    var d = refImage.reduceRegion({
      reducer: ee.Reducer.mean(),
      geometry: f.geometry(),   // promedio sobre TODO el polígono
      scale: scale,
      maxPixels: 1e10,
      bestEffort: true,
      tileScale: 4
    });
    var dFilled = fillNullsWithZeros(d, bandNames);
    return f.set(dFilled);
  });

print('Cant. polígonos de muestra:', samplesMeanVec.size());
print('Ejemplo de vector de muestra:', samplesMeanVec.first());

// ---------- 5) Similitud coseno (0..1) y "máximo por muestra" ----------
// Normalizamos el mosaico a unidad por píxel
var mosaicNorm = mosaicMasked.pow(2).reduce('sum').sqrt();
var mosaicUnit = mosaicMasked.divide(mosaicNorm.where(mosaicNorm.eq(0), 1));

// Para cada polígono-muestra → coseno contra el mosaico unitario
var cosinePerSample = ee.ImageCollection(
  samplesMeanVec.map(function (f) {
    var sImg  = ee.Image(f.toArray(bandNames)).arrayFlatten([bandNames]);
    var sNorm = sImg.pow(2).reduce('sum').sqrt();
    var sUnit = sImg.divide(sNorm.where(sNorm.eq(0), 1));

    var cos = sUnit.multiply(mosaicUnit).reduce('sum');          // [-1, 1]
    return cos.add(1).divide(2).rename('cosine_closeness');      // [0, 1]
  })
);

// Similar a ALGÚN polígono de muestra (no promedio)
var cosineMax = cosinePerSample.max().rename('cosine_closeness');

// (Opcional) visual continuo
Map.addLayer(
  cosineMax,
  {min: 0, max: 1, palette: ['000004','2C105C','711F81','B63679','EE605E','FDAE78','FCFDBF','FFFFFF']},
  'Cosine closeness (MAX por muestra)',
  false
);

// ---------- 6) Polígonos por umbral ----------
function areasAtThreshold(img, th) {
  var mask = img.gte(th).selfMask();

  var polys = mask.reduceToVectors({
    geometry: geometry,
    scale: scale,
    eightConnected: true,
    maxPixels: 1e10,
    tileScale: 4
  });

  // Atributos y filtro de área mínima (opcional)
  polys = polys.map(function(f) {
    var maxClose = img.reduceRegion({
      reducer: ee.Reducer.max(),
      geometry: f.geometry(),
      scale: scale,
      maxPixels: 1e10,
      tileScale: 4
    }).get(img.bandNames().get(0));

    var area = f.geometry().area(scale); // m²
    return f.set({'threshold': th, 'cosine_close': maxClose, 'area_m2': area});
  });

  if (MIN_AREA_SQM > 0) {
    polys = polys.filter(ee.Filter.gte('area_m2', MIN_AREA_SQM));
  }
  return polys;
}

// SOLO ≥98%, ≥99% y ≈100%
var poly98  = areasAtThreshold(cosineMax, 0.98);
var poly99  = areasAtThreshold(cosineMax, 0.99);
var poly100 = areasAtThreshold(cosineMax, 0.999); // 1.0 exacto es raro

// ---------- 7) Mostrar POLÍGONOS ----------
Map.addLayer(poly98,  {color: 'yellow'},  'Áreas ≥ 98%', true);
Map.addLayer(poly99,  {color: 'orange'},  'Áreas ≥ 99%', true);
Map.addLayer(poly100, {color: 'cyan'},    'Áreas ≈ 100%', true);

print('Áreas ≥98%:', poly98.size(),
      ' ≥99%:',     poly99.size(),
      ' ≈100%:',    poly100.size());

// ---------- 8) (Opcional) Exportar a Asset ----------
var polysAll = ee.FeatureCollection(poly98.merge(poly99).merge(poly100));
// Export.table.toAsset({
//   collection: polysAll,
//   description: 'WaterLike_Polygons_98_99_100',
//   assetId: 'projects/tu-proyecto/assets/waterlike_polygons_98_99_100'
// });

```

# Busqueda: Hornos de Ladrillo

0) Entradas

Se prepara la geometría de trabajo y la vista del mapa.

var geometry = ee.FeatureCollection(roi).geometry(1); toma tu roi (puede ser Geometry o FC), lo convierte en FeatureCollection y extrae su geometría con un margen (tolerancia) de 1 metro para evitar topologías “degeneradas”.

Se configura el fondo satelital y se dibujan dos capas: el ROI en rojo y las muestras (polígonos de ladrilleras) en amarillo.

Map.centerObject(roi, 10); centra la vista directamente sobre tu roi.
Las dos líneas comentadas son un plan B para ROIs complejos: hacen la unión de todas las piezas con margen y aplican buffer(0) para “sanear” topología si hubiera geometrías auto-intersectadas.

1) Parámetros

Define la ventana temporal, la escala de análisis y los controles de filtrado.

year, startDate, endDate: trabajarás sobre todo 2024.

scale = 20: resolución para reducir, vectorizar y medir áreas (ajústalo a 10 m si los sitios son chicos, sabiendo que puede costar más).

USE_FILTERS: activa o desactiva todos los filtros POST a la vez.

Umbrales de índices:

NDVI_MAX: vegetación baja (ladrilleras suelen ser suelos desnudos/superficies construidas).

NDBI_MIN, BSI_MIN: realzan lo construido/suelo desnudo.

S1_VV_MINdB, S1_VH_MINdB: mínimos de retrodispersión radar (en dB) para descartar superficies muy lisas/húmedas que no correspondan.

EXCLUDE_WATER: si true, excluye agua con JRC.

MIN_AREA_SQM/MAX_AREA_SQM: filtro de área final en m² para quitar “miguitas” y polígonos exageradamente grandes.

T98, T99, T100: umbrales fijos de similitud coseno reescalada (0.98, 0.99, ~1.0).

2) Embedding (sin máscara previa)

Carga el embedding anual de Google y arma un mosaico del periodo.

GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL → filterDate(...).mosaic().clip(geometry).

bandNames guarda la lista de bandas para usarla al convertir features a imágenes y asegurar alineación de nombres.
Nota: acá no se aplica máscara de agua ni nubes al embedding (se filtra después con los POST-filtros).

3) Vector de referencia (promedio en polígono, robusto)

Construye, para cada polígono de muestra, un vector promedio de embedding.

refImage = mosaic.unmask(0) rellena con 0 donde falten datos para evitar nulls.

fillNullsWithZeros recorre todas las bandas y reemplaza nulos por 0 en el diccionario resultante del reduceRegion.

reduceRegion({ reducer: mean, geometry: f.geometry(), ... }) calcula el promedio por banda sobre todo el polígono (mejor que muestrear un punto).

El resultado (samplesMeanVec) es una FC donde cada feature (muestra) tiene, además de su geometría, todas las bandas del embedding como propiedades con el promedio correspondiente.
Esto hace el “prototipo” de cada ladrillera a partir de sus polígonos.

4) Similitud coseno (0..1) en todo el ROI

Calcula qué tan parecido es cada píxel del ROI a alguna de tus muestras.

Se normaliza el mosaico: mosaicUnit = mosaic / ||mosaic||.

Para cada muestra:

Convierte sus propiedades (bandas) en imagen (toArray → arrayFlatten) y normaliza a unidad (sUnit).

Similitud coseno = producto punto entre sUnit y mosaicUnit, reducido por suma → [-1, 1].

Se reescala a [0, 1] con (cos+1)/2 y se llama cosine_closeness.

Se toma el máximo entre todas las muestras por píxel (cosinePerSample.max()), para quedarnos con la mejor coincidencia posible.

reduceRegion saca estadísticas globales (min, max, percentiles) dentro del ROI para conocer el rango típico de similitudes.

Se añade una capa opcional continua de cosineMax para inspección visual.

5) Filtros POST con “fail-open”

Construye una máscara candidata multiplicando filtros, pero solo si no destruyen la cobertura.

candidateMask arranca como 1 (todo pasa).

maskCoverage(img) estima la cobertura (promedio 0/1) de una máscara sobre el ROI.

safeAnd(baseMask, newMask, label): intenta aplicar AND con el filtro nuevo; si la cobertura resultante cae ~0, no aplica el filtro (se queda “abierto”, fail-open). Se imprime la cobertura tentativa de cada filtro para diagnóstico.

Sentinel-2 (si hay datos en la ventana):

Se hace una mediana libre de nubes (QA60).

Se calculan NDVI, NDBI, BSI.

Se proponen máscaras laxas (NDVI<..., NDBI>..., BSI>...) y se aplican con safeAnd.

Sentinel-1 (si hay VV y VH):

Se pasa a dB, se arma una máscara VV>... && VH>... y se aplica con safeAnd.

Agua (JRC): si EXCLUDE_WATER, enmascara occurrence < 10%.

Se normaliza y renombra la máscara final y se imprime su cobertura 0..1. También se agrega como capa de diagnóstico.
Resultado: una máscara que intenta recortar la similitud a áreas plausibles de ladrilleras sin arriesgarse a borrar todo por un filtro mal seteado.

6) Aplicar mask POST a la similitud

cosMasked = cosineMax.updateMask(candidateMask) limita la similitud a las zonas que pasaron los filtros POST.
Se calculan estadísticas (min, max, p90..p99) dentro de la máscara para ver cómo se comportan las similitudes ya filtradas y, por ejemplo, confirmar que los percentiles altos son suficientemente elevados para umbrales fijos (0.98/0.99).

7) Polygonización + filtro de área

Convierte zonas con similitud alta en polígonos y aplica filtros de área.

areasAtThreshold(img, th) umbraliza (img >= th), autoselecciona (selfMask), vectoriza con reduceToVectors y calcula:

area_m2 del polígono,

cosine_close máximo dentro de ese polígono (sirve para ordenar/filtrar luego).

Luego filtra por MIN_AREA_SQM y MAX_AREA_SQM.

Se generan tres capas: ≥0.98, ≥0.99 y ≈1.0, y se agregan al mapa con colores distintos. También se imprimen los conteos para chequeos rápidos.

8) Umbral adaptativo (respaldo)

Calcula un percentil de la distribución de similitud dentro de la máscara y lo usa como umbral dinámico cuando los fijos no funcionan bien.

perc = 99 (puedes subir/bajar).

reduceRegion obtiene P99; si por alguna razón es null (datos escasos), se cae a 0.95 como respaldo.

Se vectoriza con ese umbral y se añade como capa “≥ P99 (adaptativo)”.
Útil cuando la firma de ladrilleras varía por zona o año y los 0.98/0.99 quedan demasiado estrictos o laxos.

9) (Opcional) Export

Une las colecciones (o la que elijas, p.ej. la adaptativa) y deja listo el bloque de Export.table.toAsset.

Edita description y assetId y ejecuta para persistir resultados, versionarlos y reutilizarlos sin recalcular toda la pipeline.

```javascript
// ============================================================
// LADRILLERAS con Embeddings + Filtros POST "fail-open"
// Si un filtro deja cobertura ~0, se omite automáticamente.
// Requiere: 'roi' y 'samples' (polígonos de ladrilleras) definidos por el usuario.
// ============================================================

// ---------- 0) Entradas ----------
var geometry = ee.FeatureCollection(roi).geometry(1);  // ← margen no-cero
Map.setOptions('SATELLITE');
Map.addLayer(roi, {color:'red'}, 'ROI', true);
Map.addLayer(samples, {color:'yellow'}, 'Samples (ladrilleras)', true);
Map.centerObject(roi, 10);                              // ← centrar sobre la FC

// (Opcional, por si tu ROI fuese muy complejo, descomentar la siguiente línea):
// geometry = ee.FeatureCollection(roi).union(1).geometry(1); // unión con margen
// geometry = geometry.buffer(0, 1);  // “sanear” topología si hiciera falta

// ---------- 1) Parámetros ----------
var year = 2024;
var startDate = ee.Date.fromYMD(year, 1, 1);
var endDate   = startDate.advance(1, 'year');

var scale = 20;            // probá 10 si los sitios son chicos
var USE_FILTERS = true;    // activar/desactivar filtros POST

// Filtros (arrancar LAxo; luego endurecer)
var NDVI_MAX     = 0.35;
var NDBI_MIN     = -0.05;
var BSI_MIN      = -0.05;
var S1_VV_MINdB  = -15;
var S1_VH_MINdB  = -22;
var EXCLUDE_WATER= true;

// Área mínima/máxima (m²)
var MIN_AREA_SQM = 1500;
var MAX_AREA_SQM = 300000;

// Umbrales fijos
var T98 = 0.98, T99 = 0.99, T100 = 0.999;

// ---------- 2) Embedding (SIN máscara previa) ----------
var embeddings = ee.ImageCollection('GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL');
var mosaic = embeddings.filterDate(startDate, endDate).mosaic().clip(geometry);
var bandNames = mosaic.bandNames();

// ---------- 3) Vector de referencia (promedio en polígono, robusto) ----------
var refImage = mosaic.unmask(0);
function fillNullsWithZeros(dict, bands) {
  dict = ee.Dictionary(dict); bands = ee.List(bands);
  return ee.Dictionary(bands.iterate(function(b, acc){
    b = ee.String(b);
    var v = dict.get(b);
    v = ee.Algorithms.If(v, v, 0);
    return ee.Dictionary(acc).set(b, v);
  }, ee.Dictionary({})));
}
var samplesMeanVec = ee.FeatureCollection(samples)
  .filterBounds(geometry)
  .map(function(f){
    var d = refImage.reduceRegion({
      reducer: ee.Reducer.mean(),
      geometry: f.geometry(),
      scale: scale, bestEffort: true, tileScale: 4, maxPixels: 1e10
    });
    return f.set( fillNullsWithZeros(d, bandNames) );
  });

print('Polígonos de muestra:', samplesMeanVec.size());
print('Ejemplo vector de muestra:', samplesMeanVec.first());

// ---------- 4) Similitud coseno (0..1) en TODO el ROI ----------
var mosaicNorm = mosaic.pow(2).reduce('sum').sqrt();
var mosaicUnit = mosaic.divide(mosaicNorm.where(mosaicNorm.eq(0), 1));

var cosinePerSample = ee.ImageCollection(
  samplesMeanVec.map(function (f) {
    var sImg  = ee.Image(f.toArray(bandNames)).arrayFlatten([bandNames]);
    var sNorm = sImg.pow(2).reduce('sum').sqrt();
    var sUnit = sImg.divide(sNorm.where(sNorm.eq(0), 1));
    var cos   = sUnit.multiply(mosaicUnit).reduce('sum');       // [-1,1]
    return cos.add(1).divide(2).rename('cosine_closeness');     // [0,1]
  })
);
var cosineMax = cosinePerSample.max().rename('cosine_closeness');

var statsCos = cosineMax.reduceRegion({
  reducer: ee.Reducer.minMax().combine({
    reducer2: ee.Reducer.percentile([90,95,97,98,99]),
    sharedInputs: true
  }),
  geometry: geometry, scale: scale, bestEffort: true, tileScale: 4, maxPixels: 1e10
});
print('cosine_closeness (ROI) min/max/p90..p99:', statsCos);

Map.addLayer(cosineMax,
  {min:0, max:1, palette:['000004','2C105C','711F81','B63679','EE605E','FDAE78','FCFDBF','FFFFFF']},
  'Cosine closeness (MAX, sin filtros)', false);

// ---------- 5) Filtros POST con "fail-open" ----------
var candidateMask = ee.Image(1).rename('mask');  // identidad

function maskCoverage(img) {
  return ee.Number(
    img.unmask(0).toFloat().rename('m')
      .reduceRegion({
        reducer: ee.Reducer.mean(),
        geometry: geometry, scale: scale,
        bestEffort: true, tileScale: 4, maxPixels: 1e10
      }).get('m')
  );
}

function safeAnd(baseMask, newMask, label){
  newMask = newMask.unmask(0).gt(0).toFloat(); // 0/1
  var cov = maskCoverage( baseMask.and(newMask) );
  print('Cobertura si agrego "' + label + '":', cov);
  // si la cobertura tras agregar cae a ~0, NO lo aplico
  return ee.Image(ee.Algorithms.If(cov.gt(0.001), baseMask.and(newMask), baseMask));
}

if (USE_FILTERS) {
  // Sentinel-2
  function maskS2(img){
    var qa = img.select('QA60');
    var cloud = qa.bitwiseAnd(1<<10).neq(0).or(qa.bitwiseAnd(1<<11).neq(0));
    return img.updateMask(cloud.not());
  }
  var s2col = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
      .filterBounds(geometry).filterDate(startDate,endDate)
      .map(maskS2).select(['B2','B3','B4','B8','B8A','B11']);

  var hasS2 = s2col.size().gt(0);
  print('¿Hay S2 en la ventana?:', hasS2);

  if (hasS2) {
    var s2med = s2col.median().clip(geometry);

    var ndvi = s2med.expression('(NIR-RED)/(NIR+RED)', {
      NIR: s2med.select('B8'), RED: s2med.select('B4')
    }).rename('NDVI');

    var ndbi = s2med.expression('(SWIR-NIR)/(SWIR+NIR)', {
      SWIR: s2med.select('B11'), NIR: s2med.select('B8A')
    }).rename('NDBI');

    var bsi = s2med.expression(
      '((SWIR + RED) - (NIR + BLUE)) / ((SWIR + RED) + (NIR + BLUE))', {
        SWIR: s2med.select('B11'), RED: s2med.select('B4'),
        NIR: s2med.select('B8'),  BLUE: s2med.select('B2')
    }).rename('BSI');

    var m_ndvi = ndvi.lt(NDVI_MAX);
    var m_ndbi = ndbi.gt(NDBI_MIN);
    var m_bsi  = bsi.gt(BSI_MIN);

    candidateMask = safeAnd(candidateMask, m_ndvi, 'NDVI<'+NDVI_MAX);
    candidateMask = safeAnd(candidateMask, m_ndbi, 'NDBI>'+NDBI_MIN);
    candidateMask = safeAnd(candidateMask, m_bsi,  'BSI>'+BSI_MIN);
  } else {
    print('S2 no disponible: se omiten filtros NDVI/NDBI/BSI.');
  }

  // Sentinel-1
  var s1col = ee.ImageCollection('COPERNICUS/S1_GRD')
      .filterBounds(geometry).filterDate(startDate,endDate)
      .filter(ee.Filter.eq('instrumentMode','IW'))
      .filter(ee.Filter.listContains('transmitterReceiverPolarisation','VV'))
      .filter(ee.Filter.listContains('transmitterReceiverPolarisation','VH'))
      .select(['VV','VH']);
  var hasS1 = s1col.size().gt(0);
  print('¿Hay S1 en la ventana?:', hasS1);

  if (hasS1) {
    var s1med = s1col.median().clip(geometry);
    var s1VVdB = ee.Image(10).multiply(s1med.select('VV').log10()).rename('VVdB');
    var s1VHdB = ee.Image(10).multiply(s1med.select('VH').log10()).rename('VHdB');

    var m_s1 = s1VVdB.gt(S1_VV_MINdB).and(s1VHdB.gt(S1_VH_MINdB));
    candidateMask = safeAnd(candidateMask, m_s1, 'S1 VV/VH dB');
  } else {
    print('S1 no disponible: se omiten filtros radar.');
  }

  // Agua
  if (EXCLUDE_WATER) {
    var waterOcc = ee.Image('JRC/GSW1_4/GlobalSurfaceWater').select('occurrence');
    var m_nowater = waterOcc.lt(10);
    candidateMask = safeAnd(candidateMask, m_nowater, 'no-agua (JRC<10%)');
  }

  // Normalizo y fijo nombre para diagnóstico
  candidateMask = candidateMask.unmask(0).toFloat().rename('mask');

  var covFinal = candidateMask.reduceRegion({
    reducer: ee.Reducer.mean(),
    geometry: geometry, scale: scale, bestEffort: true, tileScale: 4, maxPixels: 1e10
  }).get('mask');
  print('Cobertura FINAL Candidate mask (0..1):', covFinal);

  Map.addLayer(candidateMask.updateMask(candidateMask),
    {palette:['#ffffff']}, 'Candidate mask (POST, fail-open)', false);
}

// ---------- 6) Aplicar mask POST a la similitud ----------
var cosMasked = cosineMax.updateMask(candidateMask);

// Diagnóstico en el área enmascarada
var statsMasked = cosMasked.reduceRegion({
  reducer: ee.Reducer.minMax().combine({
    reducer2: ee.Reducer.percentile([90,95,97,98,99]),
    sharedInputs: true
  }),
  geometry: geometry, scale: scale, bestEffort: true, tileScale: 4, maxPixels: 1e10
});
print('cosine_closeness (DENTRO MASK) min/max/p90..p99:', statsMasked);

// ---------- 7) Polygonización + filtro de área ----------
function areasAtThreshold(img, th) {
  var mask = img.gte(th).selfMask();
  var polys = mask.reduceToVectors({
    geometry: geometry, scale: scale, eightConnected: true,
    tileScale: 4, maxPixels: 1e10
  }).map(function(f){
    var g = f.geometry();
    var area = g.area(scale);
    var maxClose = img.reduceRegion({
      reducer: ee.Reducer.max(), geometry: g, scale: scale,
      tileScale: 4, maxPixels: 1e10
    }).get(img.bandNames().get(0));
    return f.set({'threshold': th, 'cosine_close': maxClose, 'area_m2': area});
  })
  .filter(ee.Filter.gte('area_m2', MIN_AREA_SQM))
  .filter(ee.Filter.lte('area_m2', MAX_AREA_SQM));
  return polys;
}

var poly98  = areasAtThreshold(cosMasked, T98);
var poly99  = areasAtThreshold(cosMasked, T99);
var poly100 = areasAtThreshold(cosMasked, T100);

Map.addLayer(poly98,  {color:'#ffd000'}, 'Ladrillera-like ≥98%', true);
Map.addLayer(poly99,  {color:'#ff7f00'}, 'Ladrillera-like ≥99%', true);
Map.addLayer(poly100, {color:'#00ffff'}, 'Ladrillera-like ≈100%', true);

print('≥98%:', poly98.size(), ' ≥99%:', poly99.size(), ' ≈100%:', poly100.size());

// ---------- 8) Umbral adaptativo (respaldo) ----------
var perc = 99;
var percDict = cosMasked.reduceRegion({
  reducer: ee.Reducer.percentile([perc]),
  geometry: geometry, scale: scale, bestEffort: true, tileScale: 4, maxPixels: 1e10
});
var percKey = ee.String(ee.Dictionary(percDict).keys().get(0));
var thrP    = ee.Number(ee.Dictionary(percDict).get(percKey));
thrP = ee.Number(ee.Algorithms.If(thrP, thrP, 0.95));
print('Umbral adaptativo P' + perc + ' (fallback 0.95):', thrP);

var polyP = areasAtThreshold(cosMasked, thrP);
Map.addLayer(polyP, {color:'#ffffff'}, '≥ P' + perc + ' (adaptativo)', false);
print('≥ P' + perc + ':', polyP.size());

// ---------- 9) (Opcional) Export ----------
// var allPolys = ee.FeatureCollection(poly98.merge(poly99).merge(poly100));
// Export.table.toAsset({
//   collection: allPolys,
//   description: 'BrickKiln_like_polygons_failopen',
//   assetId: 'projects/tu-proyecto/assets/brickkiln_like_polygons_failopen'
// });


```


# Busqueda: Entrenamiento supervisado para cultivos (una aproximación)

La lógica del script:

*  **Crear muestras de entrenamiento**: 
    * Define una lista data con puntos etiquetados por clase (Vid, Manzana, Pera, Alfalfa, Horticultura) y sus coordenadas. Esa lista es tu “verdad de campo”: cada fila representa un punto conocido de cierta categoría que después se usará para entrenar y validar el clasificador.
    * **Conversión a FeatureCollection**
    * Toma cada fila de data, arma una geometría ee.Geometry.Point (en orden lon/lat) y la convierte en un ee.Feature con propiedades categoria, latitud y longitud. Con todos esos features construye una ee.FeatureCollection llamada samples, que es el insumo geoespacial estándar para el resto del flujo.
    * **Sanidad:** filtrar por bounding box + vista inicial
    Crea un rectángulo grande que cubre Neuquén/Alto Valle y filtra las muestras a ese recinto (si el filtro deja cero, usa todas para no romper el pipeline). Después setea el mapa en modo “SATELLITE”, agrega los puntos en amarillo y centra la vista en ellos para una verificación visual rápida.
    * **ROI desde muestras (robusto)**
    A partir de todas las muestras, construye una región de interés (ROI) creando un buffer de 10 km alrededor de su geometría unida. Ese polígono (en rojo) es el “ámbito de trabajo” al que luego se recortan máscaras, imágenes y resultados, y el mapa se centra sobre él.

* 1) Parámetros
Define el año de análisis (2024) y el rango temporal completo (1 de enero a 31 de diciembre). También fija parámetros de escala espacial (SCALE), área mínima para vectorizar polígonos (MIN_POLY_M2) y TILE_SCALE para operaciones pesadas, que controlan resolución y performance.
* 2) Máscara agrícola (ESA+MODIS+Dynamic World)
Construye una máscara de “probable área agrícola” combinando: (a) WorldCover (clase 40 Cropland), (b) MODIS Land Cover permitiendo cropland y mosaico agro–natural, excluyendo urbano, bosques, agua, barren y grass donde corresponde, y (c) Dynamic World (medianas del año) favoreciendo píxeles con alta probabilidad de cultivo o árboles y penalizando grass/bare. También quita explícitamente WorldCover bare (clase 60). El resultado agMask (en verde) es la máscara agrícola final, y se agregan capas de control para inspección.
* 3) Embeddings
Carga el embedding anual GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL para el año de interés, lo mosaica y recorta a la ROI. Luego añade como bandas extra las probabilidades de Dynamic World para crops y trees (renombradas dw_crops, dw_trees) para darle pistas adicionales al clasificador sobre patrones agrícolas/arbóreos.
Features extra: índices S2 estacionales
Arma una colección Sentinel-2 para la “temporada productiva” (sep–abr en hemisferio sur), enmascara nubes, calcula NDVI, EVI, NDRE y NDWI por imagen y luego resume por estadísticos robustos (mediana, p90 y desvío estándar) por índice. Concatena todo (extraFeat) y lo agrega a emb, de modo que el modelo no dependa solo de embeddings sino también de rasgos fenológicos clásicos. Además guarda ndvi_med y ndvi_std para un refinamiento posterior de “Vid”.
* 4) Preparar muestras
Filtra las muestras a las clases de interés, mapea cada categoria a un entero (label) y deja pronta una colección de puntos etiquetados (trainPts) lista para muestrear valores de las bandas (embeddings + features) y entrenar.
4B) Espaciar muestras por clase
Para evitar sobre-representación espacial de puntos muy cercanos, “cuadricula” el espacio en celdas (en EPSG:3857) del tamaño elegido (MIN_DIST_M) y conserva, aleatoriamente, un solo punto por celda y por clase. Así se reduce el sesgo y se mejora la generalización del modelo.
* 5) Extraer embeddings/features en puntos
Muestrea (sampleRegions) los valores de todas las bandas de emb en los puntos espaciados, conservando propiedades label y categoria. Filtra filas con valores nulos. El resultado (trainSam) es una tabla de entrenamiento con X = bandas e Y = etiqueta numérica.
* 6) Split 80/20, balanceo en TRAIN y entrenamiento RF
Separa estratificadamente 80% para entrenamiento y 20% para validación (hold-out). Para el set de entrenamiento, aplica un “capping” por clase (CAP_PER_CLASS) para que las mayoritarias no dominen, sin forzar a reducir todo al mínimo absoluto. Con ese balancedTrain entrena un Random Forest (500 árboles, bagging 0.7), usando todas las bandas disponibles.
6B) Evaluación en validación
Clasifica el 20% de validación, calcula matriz de confusión, accuracy, Kappa, precision/recall por clase y F1 (macro y ponderado por soporte). También construye una tabla “por clase” pensada para inspección en la consola, de modo de entender qué categorías rinden mejor/peor.
* 7) Clasificar la ROI: 
Aplica el clasificador entrenado a todas las bandas en emb para producir un raster de clases, y enmascara el resultado con agMask (para recortar a zonas agrícolas probables). Así se obtiene el mapa de cultivos sobre toda la región de interés.
Post-procesado específico para “Vid”
Calcula una condición adicional con Dynamic World completo (crops alto, grass/built/bare bajos) y con señales fenológicas (NDVI medio > 0.20 y variación estacional > 0.03) para refinar la clase “Vid”. Hace una limpieza espacial con focal_mode, reemplaza solo donde corresponde y recompone el raster clasificado final con esa “Vid” más estricta.
Visualización de la clasificación
Define una paleta de colores consistente por clase (Vid azul, Manzana rojo, Pera amarillo, Alfalfa verde, Horticultura blanco) y agrega la capa clasificada al mapa con ese estilo, lista para inspección visual.

* 8) **Vectorizar por clase**: Para cada clase presente, convierte el raster clasificado en polígonos vectoriales (conectividad de 8 vecinos), calcula el área por polígono y filtra por un umbral mínimo (MIN_POLY_M2). El resultado es una FeatureCollection con polígonos limpios y atributos class_id, categoria y area_m2, útil para análisis y exportaciones.

* 9) **Leyenda (UI)**: Crea un panel ui.Panel en la esquina inferior izquierda con título y filas color-nombre que reflejan exactamente la paleta del raster. Es una leyenda “anclada” al mapa para contextualizar la visualización.

* **Gráfico de barras (UI)**: Arma otro panel en la esquina inferior derecha que muestra barras horizontales con la superficie (hectáreas) por clase, calculadas del raster clasificado (pixelArea agregado por clase dentro de la ROI). Ordena y pinta las barras con los mismos colores de la leyenda, ajusta el ancho relativo según el máximo y renderiza todo del lado del cliente para tener un resumen cuantitativo directo en el mapa.


```javascript
// ============================================================
// MAPA CATEGÓRICO DE CULTIVOS CON EMBEDDINGS + RF + LEYENDA
// ============================================================
// ---------- 0) CREAR MUESTRAS DE ENTRENAMIENTO (versión ampliada) ----------

   
// Cargar directamente (ya trae categoria, latitud, longitud)
var samples = ee.FeatureCollection('projects/ee-cdgidera/assets/samples_AV');

print('Total de muestras cargadas:', samples.size());
print('Primer punto (lon,lat):', samples.first().geometry().coordinates());

// === Sanidad: quedarnos sólo con puntos dentro del rectángulo Neuquén/Alto Valle ===
// (lon: -75 a -52, lat: -56 a -20)
var bboxNeuquen = ee.Geometry.Rectangle([-75, -56, -52, -20], null, false);
var samplesOK = samples.filterBounds(bboxNeuquen);
print('Muestras dentro de bbox esperado:', samplesOK.size());

// Si por algún motivo no hay ninguna en bbox, usa todas para no romper el flujo
samples = ee.FeatureCollection(ee.Algorithms.If(samplesOK.size().gt(0), samplesOK, samples));

// Vista
Map.setOptions('SATELLITE');
Map.addLayer(samples, {color: 'yellow'}, 'Muestras (puntos)', true);
Map.centerObject(samples, 9);

// ---------- ROI DESDE MUESTRAS (robusto) ----------
var ROI_BUFFER_KM = 10;
var roiGeom = samples.geometry().buffer(ROI_BUFFER_KM * 1000, 1).buffer(0, 1);
var roi_fc   = ee.FeatureCollection([ee.Feature(roiGeom)]);
var geometry = roi_fc.geometry(1);

Map.addLayer(roi_fc, {color: 'red'}, 'ROI (desde muestras)', false);
Map.centerObject(roi_fc, 8);
 

// ---------- 1) PARÁMETROS ----------
var year = 2024;
var startDate = ee.Date.fromYMD(year, 1, 1);
var endDate   = startDate.advance(1, 'year');

var SCALE       = 20;
var MIN_POLY_M2 = 1e4;
var TILE_SCALE  = 4;

// ---------- 2) MÁSCARA AGRÍCOLA (menos restrictiva + DW para frutales) ----------

// ESA WorldCover v200 (base agrícola)
var worldcover = ee.ImageCollection('ESA/WorldCover/v200')
  .filterDate('2021-01-01', '2022-12-31')
  .first()
  .select('Map');

var CROPLAND = 40;
var maskESA = worldcover.eq(CROPLAND);

// MODIS Land Cover IGBP
var modisLC = ee.ImageCollection('MODIS/061/MCD12Q1')
  .filterDate('2020-01-01', '2020-12-31')
  .first()
  .select('LC_Type1');

var URBAN = 13;
var FORESTS = ee.List.sequence(1, 5);

// Zonas no urbanas/bosques/agua/barren/grass
var modisNonExcluded = modisLC
  .neq(URBAN)
  .and(modisLC.remap(FORESTS, ee.List.repeat(0, FORESTS.length()), 1).eq(1))
  .and(modisLC.neq(0))    // sin agua
  .and(modisLC.neq(14))   // sin barren
  .and(modisLC.neq(8));   // sin grassland

// MODIS permitido: cropland (10) o cropland/natural mosaic (12)
var allowed = modisLC.eq(10).or(modisLC.eq(12));

// Unión ESA ∪ MODIS_permitido y filtro por no-excluidos
var agMaskBase = (maskESA.or(allowed)).and(modisNonExcluded).clip(geometry);

// Dynamic World: favorecer cultivos y frutales, penalizar grass y bare
var dw = ee.ImageCollection('GOOGLE/DYNAMICWORLD/V1')
  .filterBounds(geometry)
  .filterDate(startDate, endDate)
  .select(['crops','trees','grass','bare'])
  .median()
  .clip(geometry);

var dwMask = dw.select('crops').gt(0.25)        // cultivos
  .or( dw.select('trees').gt(0.45) )            // frutales
  .and( dw.select('grass').lt(0.35) )           // poco pastizal
  .and( dw.select('bare').lt(0.20) );           // poco suelo desnudo

// Quitar explícitamente bare/sparse de WorldCover (clase 60)
var worldcoverBare = worldcover.eq(60);
var agMask = agMaskBase.and(dwMask).and(worldcoverBare.not()).selfMask();

// Capas de control (opcionales)
Map.addLayer(agMaskBase, {min:0,max:1,palette:['000000','00ffff']}, 'Máscara base (ESA ∪ MODIS)', false);
Map.addLayer(dwMask,     {min:0,max:1,palette:['000000','ff00ff']}, 'DW crops/trees !grass', false);
Map.addLayer(agMask,     {min:0,max:1,palette:['000000','00ff88']}, 'Máscara agrícola FINAL', false);

// ---------- 3) EMBEDDINGS ----------
var emb = ee.ImageCollection('GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL')
  .filterDate(startDate, endDate)
  .mosaic()
  .clip(geometry);

// Añadir probas de Dynamic World (crops, trees) como features al modelo
var dwFeat = ee.ImageCollection('GOOGLE/DYNAMICWORLD/V1')
  .filterBounds(geometry)
  .filterDate(startDate, endDate)
  .select(['crops','trees'])
  .median()
  .clip(geometry)
  .rename(['dw_crops','dw_trees']);

emb = emb.addBands(dwFeat);

// ---- FEATURES EXTRA: NDVI/EVI/NDRE/NDWI estacionales (mediana, p90, std) ----
// Temporada: sep–abr (hemisferio sur)
var s2Start = ee.Date.fromYMD(year - 1, 9, 1);
var s2End   = ee.Date.fromYMD(year, 4, 30);
 
function maskS2(img){
  var qa = img.select('QA60'); // bits 10 cloud, 11 cirrus
  var cloud = qa.bitwiseAnd(1 << 10).neq(0)
               .or(qa.bitwiseAnd(1 << 11).neq(0));
  return img.updateMask(cloud.not());
}

var s2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
  .filterBounds(geometry)
  .filterDate(s2Start, s2End)
  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 60))
  .map(maskS2)
  .select(['B2','B3','B4','B5','B8']);  // Blue, Green, Red, RE1, NIR

// Índices
var s2Idx = s2.map(function(img){
  var nir   = img.select('B8');
  var red   = img.select('B4');
  var blue  = img.select('B2');
  var green = img.select('B3');
  var re1   = img.select('B5');

  var ndvi = nir.subtract(red).divide(nir.add(red)).rename('NDVI');
  var evi  = nir.subtract(red)
                .multiply(2.5)
                .divide(nir.add(red.multiply(6)).subtract(blue.multiply(7.5)).add(1))
                .rename('EVI');
  var ndre = nir.subtract(re1).divide(nir.add(re1)).rename('NDRE');
  var ndwi = green.subtract(nir).divide(green.add(nir)).rename('NDWI');

  return img.addBands([ndvi, evi, ndre, ndwi]);
});

// Estadísticos estacionales (band names fijos y válidos)
var ndvi_med = s2Idx.select('NDVI').median().rename('NDVI_med');
var ndvi_p90 = s2Idx.select('NDVI').reduce(ee.Reducer.percentile([90])).rename('NDVI_p90');
var ndvi_std = s2Idx.select('NDVI').reduce(ee.Reducer.stdDev()).rename('NDVI_std');

var evi_med  = s2Idx.select('EVI').median().rename('EVI_med');
var evi_p90  = s2Idx.select('EVI').reduce(ee.Reducer.percentile([90])).rename('EVI_p90');
var evi_std  = s2Idx.select('EVI').reduce(ee.Reducer.stdDev()).rename('EVI_std');

var ndre_med = s2Idx.select('NDRE').median().rename('NDRE_med');
var ndre_p90 = s2Idx.select('NDRE').reduce(ee.Reducer.percentile([90])).rename('NDRE_p90');
var ndre_std = s2Idx.select('NDRE').reduce(ee.Reducer.stdDev()).rename('NDRE_std');

var ndwi_med = s2Idx.select('NDWI').median().rename('NDWI_med');
var ndwi_p90 = s2Idx.select('NDWI').reduce(ee.Reducer.percentile([90])).rename('NDWI_p90');
var ndwi_std = s2Idx.select('NDWI').reduce(ee.Reducer.stdDev()).rename('NDWI_std');

// Apilar y recortar al ROI
var extraFeat = ee.Image.cat([
  ndvi_med, ndvi_p90, ndvi_std,
  evi_med,  evi_p90,  evi_std,
  ndre_med, ndre_p90, ndre_std,
  ndwi_med, ndwi_p90, ndwi_std
]).clip(geometry);

// Añadir probas de Dynamic World (crops, trees) como features al modelo
var dwFeat = ee.ImageCollection('GOOGLE/DYNAMICWORLD/V1')
  .filterBounds(geometry)
  .filterDate(startDate, endDate)
  .select(['crops','trees'])
  .median()
  .clip(geometry)
  .rename(['dw_crops','dw_trees']);

// Sumar features a los embeddings
emb = emb.addBands(dwFeat).addBands(extraFeat);

// Variables que usa el bloque de VID:
var ndviMean = ndvi_med;
var ndviStd  = ndvi_std;

// Refrescar lista de bandas de entrada
var embBands = emb.bandNames();

// ---------- 4) PREPARAR MUESTRAS ----------
var cats = ['Vid','Manzana','Pera','Alfalfa','Horticultura'];
var catToInt = ee.Dictionary({'Vid':0, 'Manzana':1, 'Pera':2, 'Alfalfa':3, 'Horticultura':4});
var intToCat = ee.List(['Vid','Manzana','Pera','Alfalfa','Horticultura']);

var trainPts = samples
  .filter(ee.Filter.inList('categoria', cats))
  .map(function(f){
    var cat = ee.String(f.get('categoria'));
    return f.set('label', catToInt.get(cat));
  });

print('Muestras válidas:', trainPts.size());
print('Ejemplo de muestra:', trainPts.first());

// ---------- 4B) ESPACIAR MUESTRAS POR CLASE (1 punto por celda de rejilla) ----------
var MIN_DIST_M = 120;                     // distancia mínima deseada entre puntos (ajusta: 80–200 m)
var projMeters = ee.Projection('EPSG:3857');  // proyección en metros

// Asigna ID de celda (grid) a cada punto, independiente por clase
var trainPtsThin = trainPts
  .map(function(f){
    var pt = f.geometry().centroid(1).transform(projMeters, 1).coordinates();
    var x  = ee.Number(pt.get(0));
    var y  = ee.Number(pt.get(1));
    var ix = x.divide(MIN_DIST_M).floor();
    var iy = y.divide(MIN_DIST_M).floor();
    var cell = ix.format().cat('_').cat(iy.format());
    // clave única por clase + celda
    var key = ee.String(f.get('categoria')).cat('_').cat(cell);
    return f.set({'grid_cell': cell, 'grid_key': key});
  })
  // aleatoriza y conserva 1 por celda-clase
  .randomColumn('rand', 42)
  .sort('rand')
  .distinct(['grid_key']);   // si tu EE no acepta lista, usa .distinct('grid_key')

// Control visual (opcional)
Map.addLayer(trainPtsThin, {color:'orange'}, 'Muestras espaciadas (rejilla)', false);
print('Total muestras (original):', trainPts.size());
print('Total muestras (espaciadas):', trainPtsThin.size());

// ---------- 5) EXTRAER EMBEDDINGS ----------
var trainSam = emb.sampleRegions({
  collection: trainPtsThin,   // <-- antes: trainPts
  properties: ['label', 'categoria'],
  scale: SCALE,
  tileScale: TILE_SCALE
}).filter(ee.Filter.notNull(embBands));

// ---------- 6) BALANCEAR Y ENTRENAR RF ----------
// ---------- 6) SPLIT 80/20 + BALANCEO SOLO EN TRAIN + ENTRENAR RF ----------

// (a) Split 80/20 estratificado por clase
var byLabel = trainSam.aggregate_histogram('label');
var labelKeys = ee.List(byLabel.keys());

var splitted = ee.Dictionary(labelKeys.iterate(function(lbl, acc){
  lbl = ee.Number.parse(ee.String(lbl));
  acc = ee.Dictionary(acc);

  var subset = trainSam
    .filter(ee.Filter.eq('label', lbl))
    .randomColumn('split', 13); // semilla fija

  var trainSub = subset.filter(ee.Filter.lt('split', 0.8));
  var validSub = subset.filter(ee.Filter.gte('split', 0.8));

  var trainSoFar = ee.FeatureCollection(acc.get('train'));
  var validSoFar = ee.FeatureCollection(acc.get('valid'));

  // Devolver un diccionario nuevo con las colecciones acumuladas
  return ee.Dictionary({
    'train': trainSoFar.merge(trainSub),
    'valid': validSoFar.merge(validSub)
  });
}, ee.Dictionary({
  'train': ee.FeatureCollection([]),
  'valid': ee.FeatureCollection([])
})));

var trainSet = ee.FeatureCollection(splitted.get('train'));
var validSet = ee.FeatureCollection(splitted.get('valid'));

print('Tamaño TRAIN por clase:', trainSet.aggregate_histogram('label'));
print('Tamaño VALID por clase:', validSet.aggregate_histogram('label'));

// (b) Balanceo SOLO en el set de entrenamiento (capear mayoritarias, NO bajar a la minoritaria)
var classCountsTrain = trainSet.aggregate_histogram('label');
print('Conteos TRAIN por clase (antes de balance):', classCountsTrain);

// En vez de forzar todas a minCount, solo CAPEAMOS mayoritarias
var CAP_PER_CLASS = 200; // subí/bajá este techo según te convenga
var labelsTrain = ee.List(classCountsTrain.keys());  // claves numéricas (0..4)

var balancedTrain = ee.FeatureCollection(
  labelsTrain.iterate(function(lbl, acc){
    lbl = ee.Number.parse(ee.String(lbl));
    acc = ee.FeatureCollection(acc);

    // cuántas hay para esta clase
    var countLbl = ee.Number(classCountsTrain.get(lbl));
    // objetivo = min(conteo actual, CAP)
    var target = countLbl.min(CAP_PER_CLASS);

    var subset = trainSet
      .filter(ee.Filter.eq('label', lbl))
      .randomColumn('r', 42)
      .limit(target);

    return acc.merge(subset);
  }, ee.FeatureCollection([]))
);

print('Muestras balanceadas (TRAIN):', balancedTrain.size());
print('Distribución balanceada (TRAIN):', balancedTrain.aggregate_histogram('label'));

// (c) ENTRENAR RF sobre balancedTrain
var classifier = ee.Classifier.smileRandomForest({
  numberOfTrees: 500,     // un poco más de bosque
  minLeafPopulation: 1,
  bagFraction: 0.7,
  seed: 42
}).train({
  features: balancedTrain,
  classProperty: 'label',
  inputProperties: embBands
});


// ---------- 6B) EVALUACIÓN EN VALIDACIÓN (hold-out 20%) ----------

// Clasificar el set de validación
var validClassified = validSet.classify(classifier);

// Matriz de confusión y métricas básicas
var cm = validClassified.errorMatrix('label', 'classification');
print('Matriz de confusión (VALID 20%):', cm);
print('Accuracy global (VALID):', cm.accuracy());
print('Kappa (VALID):', cm.kappa());

// Utilidad: convertir cualquier ee.Array a lista plana
function arrToFlatList(a){
  return ee.List(ee.Array(a).toList()).flatten();
}

// Listas de precisión (consumidor = precision) y recall (productor)
var consAccList = arrToFlatList(cm.consumersAccuracy()); // precision por clase
var prodAccList = arrToFlatList(cm.producersAccuracy()); // recall por clase

// Iteraremos sobre todas las clases definidas (aunque falten en VALID)
var nClasses = intToCat.length();
var idxs = ee.List.sequence(0, nClasses.subtract(1));
var nFromCM = consAccList.length(); // tamaño real que viene del CM

// Acceso seguro: si el índice no existe en el array del CM, devuelve 0
function safeGet(list, i){
  i = ee.Number(i);
  return ee.Algorithms.If(i.lt(ee.Number(list.length())), list.get(i), 0);
}

// F1 por clase (2PR/(P+R)), cuidando vacíos
var f1List = idxs.map(function(i){
  var p = ee.Number(safeGet(consAccList, i));
  var r = ee.Number(safeGet(prodAccList, i));
  var denom = p.add(r);
  return ee.Number(ee.Algorithms.If(denom.neq(0), p.multiply(r).multiply(2).divide(denom), 0));
});

// Macro-F1
var f1Macro = ee.Number(ee.List(f1List).reduce(ee.Reducer.mean()));

// Soporte por clase en VALID (cuenta robusta por clase, sin lío de claves)
var support = idxs.map(function(i){
  i = ee.Number(i);
  return ee.Number(
    validSet.filter(ee.Filter.eq('label', i)).size()
  );
});
var totalValid = ee.Number(ee.List(support).reduce(ee.Reducer.sum()));

// F1 ponderado por soporte
var f1Weighted = ee.Number(
  idxs.iterate(function(i, acc){
    i = ee.Number(i);
    acc = ee.Number(acc);
    var f1i = ee.Number(ee.List(f1List).get(i));
    var si  = ee.Number(ee.List(support).get(i));
    return acc.add(f1i.multiply(si));
  }, 0)
).divide(ee.Number(ee.Algorithms.If(totalValid.neq(0), totalValid, 1)));

print('F1 ponderado por soporte (VALID):', f1Weighted);


// Tabla por clase (con protección de índices)
var perClassTable = ee.FeatureCollection(idxs.map(function(i){
  i = ee.Number(i);
  return ee.Feature(null, {
    'clase_id': i,
    'clase': ee.String(intToCat.get(i)),
    'precision_consumidor': ee.Number(safeGet(consAccList, i)), // precision
    'precision_productor': ee.Number(safeGet(prodAccList, i)),  // recall
    'F1': ee.Number(ee.List(f1List).get(i)),
    'soporte_VALID': ee.Number(ee.List(support).get(i))
  });
}));

print('Métricas por clase (VALID 20%):', perClassTable);
print('F1 Macro (VALID):', f1Macro);
print('F1 ponderado por soporte (VALID):', f1Weighted);


// Para el resto del flujo usaremos este classifier.
// Y, para capas posteriores que usaban "classCounts", ahora:
var classCounts = balancedTrain.aggregate_histogram('label');

// ---------- 7) CLASIFICAR ROI ----------
var classified = emb.classify(classifier).rename('class').updateMask(agMask);

// ---- POST-PROCESADO ESPECÍFICO PARA VID (clase 0) ----
var dwFull = ee.ImageCollection('GOOGLE/DYNAMICWORLD/V1')
  .filterBounds(geometry)
  .filterDate(startDate, endDate)
  .select(['crops','trees','grass','built','bare'])
  .median()
  .clip(geometry);

// Condición para VID: cultivos↑, grass↓, built↓, bare↓ y algo de fenología verde
var vidCond = dwFull.select('crops').gt(0.35)
  .and(dwFull.select('grass').lt(0.30))
  .and(dwFull.select('built').lt(0.15))
  .and(dwFull.select('bare').lt(0.20))
  .and(ndviMean.gt(0.20))         // vid no debería tener NDVI medio tan bajo como “bare”
  .and(ndviStd.gt(0.03));         // algo de variación estacional

// Limpiar ruido espacial en VID
var vidMaskRefined = classified.eq(0).and(vidCond).focal_mode(1);

// Reconstruir raster con VID refinado
var others = classified.updateMask(classified.neq(0));
var vidRef = ee.Image(0).updateMask(vidMaskRefined);
classified = others.blend(vidRef).rename('class');
// Colores según categoría
var palette = ['#0000ff','#ff0000','#ffff00','#00ff00','#ffffff']; 
// Vid (azul), Manzana (rojo), Pera (amarillo), Alfalfa (verde), Horticultura (blanco)
Map.addLayer(classified, {min:0, max:4, palette: palette}, 'Clasificación (embeddings + RF)', true);

// ---------- 8) VECTORIZAR POR CLASE ----------
function vectPorClase(classId) {
  var mask = classified.eq(classId).selfMask();
  var polys = mask.reduceToVectors({
      geometry: geometry,
      scale: SCALE,
      geometryType: 'polygon',
      eightConnected: true,
      reducer: ee.Reducer.countEvery(),
      maxPixels: 1e13,
      tileScale: TILE_SCALE
    })
    .map(function(f){
      var g = f.geometry();
      var area = g.area(SCALE);
      return ee.Feature(g).set({
        'class_id': classId,
        'categoria': intToCat.get(classId),
        'area_m2': area
      });
    })
    .filter(ee.Filter.gte('area_m2', MIN_POLY_M2));
  return polys;
}

// IDs de clases presentes (según balanced)
var presentIds = ee.List(ee.Dictionary(classCounts).keys()).map(ee.Number.parse);

var polysAll = ee.FeatureCollection(
  presentIds.map(function(id){ return vectPorClase(ee.Number(id)); })
).flatten();

Map.addLayer(polysAll, {color:'#ffffff'}, 'Polígonos clasificados (≥ área mín.)', false);
print('Polígonos vectorizados (≥ ' + MIN_POLY_M2 + ' m²):', polysAll.size());

// ---------- 9) LEYENDA (versión final corregida) ----------
var legend = ui.Panel({
  style: {
    position: 'bottom-left',
    padding: '8px 15px',
    backgroundColor: 'rgba(0, 0, 0, 0.6)',
    border: '1px solid white',
    borderRadius: '6px',
    width: '180px'
  }
});

legend.add(ui.Label({
  value: 'Clasificación de cultivos',
  style: {
    fontWeight: 'bold',
    fontSize: '14px',
    color: 'white',
    margin: '0 0 8px 0',
    backgroundColor: 'rgba(0,0,0,0)'
  }
}));

function addLegendItem(color, name) {
  var colorBox = ui.Label('', {
    backgroundColor: color,
    padding: '10px',
    margin: '0 6px 4px 0',
    border: '1px solid white'
  });
  var label = ui.Label(name, {
    margin: '0 0 4px 0',
    color: 'white',
    fontSize: '12px',
    backgroundColor: 'rgba(0,0,0,0)'
  });
  var row = ui.Panel([colorBox, label], ui.Panel.Layout.flow('horizontal'), {
    backgroundColor: 'rgba(0,0,0,0)'
  });
  legend.add(row);
}

addLegendItem('#ff0000', 'Manzana');
addLegendItem('#ffff00', 'Pera');
addLegendItem('#0000ff', 'Vid');
addLegendItem('#00ff00', 'Alfalfa');
addLegendItem('#ffffff', 'Horticultura');
Map.add(legend);

// ===== 12) GRÁFICO DE BARRAS EN EL MAPA (sin scroll, colores de la leyenda) =====
var barsPanel = ui.Panel({
  style: {
    position: 'bottom-right',
    padding: '10px',
    backgroundColor: 'rgba(0,0,0,0.6)',
    border: '1px solid white',
    borderRadius: '6px',
    width: '360px'
  }
});
barsPanel.add(ui.Label('Superficie por clase (ha)', {
  color: 'white', fontWeight: 'bold', fontSize: '14px',
  backgroundColor: 'rgba(0,0,0,0)', margin: '0 0 8px 0'
}));
Map.add(barsPanel);

// IDs presentes y nombres (consistente con balanced)
var clsIds   = presentIds;
var clsNames = clsIds.map(function(id){ return ee.String(intToCat.get(ee.Number(id))); });

// Colores iguales a la paleta del raster
var classColors = palette;

// Cálculo de ha por clase (server-side)
var areaHaImg = ee.Image.pixelArea().divide(1e4);
var areasHa = clsIds.map(function(id){
  id = ee.Number(id);
  return ee.Number(
    areaHaImg.updateMask(classified.eq(id)).reduceRegion({
      reducer: ee.Reducer.sum(),
      geometry: geometry,
      scale: SCALE,
      maxPixels: 1e13,
      tileScale: TILE_SCALE
    }).get('area')
  );
});

// Traer a cliente y dibujar
var dataDict = ee.Dictionary.fromLists(clsNames, areasHa);
dataDict.evaluate(function(obj){
  if (!obj) return;

  var orderedNames = clsNames.getInfo();
  var allColors = {
    'Vid': '#0000ff',
    'Manzana': '#ff0000',
    'Pera': '#ffff00',
    'Alfalfa': '#00ff00',
    'Horticultura': '#ffffff'
  };
  var orderedColors = [];
  orderedNames.forEach(function(n){ orderedColors.push(allColors[n]); });

  var maxVal = 0;
  orderedNames.forEach(function(n){ maxVal = Math.max(maxVal, (obj[n] || 0)); });
  if (maxVal === 0) maxVal = 1;

  var maxBarWidth = 300; // px

  barsPanel.clear();
  barsPanel.add(ui.Label('Superficie por clase (ha)', {
    color: 'white', fontWeight: 'bold', fontSize: '14px',
    backgroundColor: 'rgba(0,0,0,0)', margin: '0 0 8px 0'
  }));

  orderedNames.forEach(function(name, idx){
    var val = obj[name] || 0;
    var widthPx = Math.max(6, Math.round(maxBarWidth * (val / maxVal)));
    var color = orderedColors[idx];
 
    var label = ui.Label(
      name + ': ' + (Math.round(val)).toLocaleString('es-AR') + ' ha',
      { color: 'white', fontSize: '12px', margin: '0 0 4px 0', backgroundColor: 'rgba(0,0,0,0)' }
    );
    var bar = ui.Label('', {
      backgroundColor: color,
      padding: '10px 0px',
      margin: '0 0 10px 0',
      border: '1px solid white',
      width: widthPx + 'px'
    });
    var row = ui.Panel([label, bar], ui.Panel.Layout.flow('vertical'), {
      backgroundColor: 'rgba(0,0,0,0)', margin: '0'
    });
    barsPanel.add(row);
  });
});

```