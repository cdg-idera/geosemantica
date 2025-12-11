# Capítulo: **CNN**

## **Introducción**

Las capas de filtros o convoluciones al inicio lo que hacen es actuar como detectores de características. Cada filtro va aprendiendo a identificar ciertas formas, bordes, texturas, etc. Y sí, efectivamente la imagen se va reduciendo y transformando en lo que llamamos "mapas de características" más compactos. Aún en esta fase no son capas densamente conectadas, sino que son capas de convolución y, a veces, de pooling (reducción de tamaño).

Luego, cuando llegas al final de estas capas convolucionales, tienes un volumen de datos que ya representa las características extraídas. Lo que se hace es "aplanar" o convertir ese conjunto de datos en un vector, que es una lista de valores. Y aquí es donde entra la capa completamente conectada, que es la que realmente funciona como una red neuronal tradicional (fully connected layer o capa densa).

En esas capas finales, cada neurona está conectada con todas las neuronas de la capa anterior. Esa parte es la que se encarga de tomar esas características ya extraídas y aprender a combinarlas para hacer la clasificación final o la tarea que necesites. Así que sí, esa es la parte "neuronal" propiamente dicha.

Resumiendo: primero las capas de convolución actúan como extractores de características, y luego la capa fully connected final es la que toma esos vectores y realmente funciona como una red neuronal clásica.


En la red neuronal, especialmente en esa última parte de la que hablábamos, tenemos lo que llamamos "layers" o capas. Estas capas son las que realmente definen la estructura de la red neuronal.

Dentro de la parte fully connected, cada "layer" o capa es un conjunto de neuronas que están conectadas con todas las neuronas de la capa anterior, y de esa forma la red va aprendiendo patrones complejos. También vas a encontrar funciones de activación, que son las que introducen la no linealidad y permiten que la red aprenda relaciones más complejas.

Así que sí, en esa parte final tienes esas capas (layers) completamente conectadas, y ahí es donde realmente la red neuronal hace su magia final.

La convolución y los filtros lo que hacen es extraer características de la imagen, pero la decisión final de clasificación, es decir, asignar una clase a cada parche o pixel, sucede más bien en la parte de la red que es totalmente conectada y en las capas finales. Así que la convolución prepara la información y luego esas capas finales son las que toman la decisión de clasificación.

Cuando una red neuronal, como esta que estás describiendo, hace una predicción, compara esa predicción con el valor real que debería obtener. Esa comparación genera un error, una diferencia entre lo que la red predijo y lo que debería haber predicho.

Ahora, la magia está en que la red ajusta sus pesos, o sea los valores internos de sus conexiones, para reducir ese error. Y para hacerlo, usa un proceso llamado "backpropagation". Lo que hace es tomar ese error y propagarlo hacia atrás a través de la red, capa por capa, desde la salida hasta las capas iniciales. De esta forma, va ajustando los pesos de cada neurona para que la próxima vez la predicción sea un poquito más precisa.

Así que en resumen, es un paso hacia adelante, donde la red hace una predicción, y luego un paso hacia atrás (backpropagation) donde la red se corrige a sí misma para mejorar.


## **Fully conected**

Una fully connected layer (o capa densa) está compuesta, por neuronas, conexiones entre esas neuronas y algunos componentes clave más. Los resumimos a continuación:

* **Neuronas y Capas**: Cada capa densa tiene un cierto número de neuronas, y cada neurona recibe entradas de todas las neuronas de la capa anterior. Esa es la idea de *totalmente conectada*: *cada neurona de una capa está unida a cada neurona de la capa anterior*.

* **Pesos y Biases**: Cada una de esas conexiones tiene un *peso* asociado. Estos pesos son los valores que la red va ajustando durante el entrenamiento para mejorar sus predicciones. Además de los pesos, cada neurona suele tener un término llamado *bias* o sesgo, que es un valor que se suma para dar flexibilidad al modelo.

* **Funciones de Activación**: Después de que una neurona calcula su suma ponderada de entradas (usando los pesos y el bias), pasa ese valor por una función de activación. Esto introduce no linealidad y permite que la red aprenda relaciones complejas. Pueden ser funciones como *ReLU*, *Sigmoid*, *Tanh*, etc.

* **Forward y Backpropagation**: Como mencionabas, en la fase de *"forward"* o propagación hacia adelante, la red hace su predicción. Luego, en la fase de *"backpropagation"* o propagación hacia atrás, compara el resultado con el valor real, calcula el error y ajusta los pesos hacia atrás para minimizar ese error en el próximo intento.

En resumen, los componentes principales de una fully connected layer son las neuronas, los pesos y biases, las funciones de activación y el proceso de forward y backward propagation que ajusta esos pesos.

## U-Net

Una U-Net es un tipo particular de red neuronal convolucional que se utiliza sobre todo en tareas de segmentación de imágenes, por ejemplo, en imágenes médicas o en cualquier tarea donde necesites separar o delinear objetos dentro de una imagen.

La U-Net tiene una forma de "U", por eso su nombre. Consiste en dos partes principales: una parte de **"contracción" (o encoder)** que reduce la imagen a características más pequeñas y una parte de **"expansión" (o decoder)** que vuelve a expandir esas características a la resolución original, generando un mapa segmentado de la misma dimensión que la imagen de entrada.

En otras palabras, la U-Net toma una imagen, la "comprime" para entender sus características, y luego la "descomprime" para producir una máscara segmentada que indica dónde están los objetos de interés. Es muy útil cuando necesitas no solo clasificar una imagen entera, sino saber exactamente qué píxeles pertenecen a qué clase.

Así que, en resumen, una U-Net es una arquitectura especial de CNN pensada para segmentación, con esa forma de U que primero contrae la imagen y luego la expande para generar un resultado segmentado.


## Ejemplo 1: Eurostar

todo este notebook implementa efectivamente una red neuronal convolucional (CNN) para clasificación de imágenes satelitales (EuroSAT), siguiendo el tutorial original (https://thepythoncode.com/article/satellite-image-classification-using-tensorflow-python) pero con ajustes prácticos (sobre todo en la gestión de tf.data, el manejo de splits y la evaluación con métricas adicionales).


**Chunk 1 – Instalación de librerías e imports generales**

En el primer bloque se instalan y cargan las librerías necesarias:

```python
!pip install -q tensorflow tensorflow_datasets tensorflow_hub seaborn matplotlib

import os
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import tensorflow as tf
import tensorflow_datasets as tfds
import tensorflow_hub as hub

```
Objetivo conceptual:

Este chunk prepara el entorno de trabajo. Se instalan TensorFlow y las librerías asociadas para:

* tensorflow: construir, entrenar y evaluar la red neuronal.
* tensorflow_datasets (tfds): descargar y gestionar el dataset EuroSAT de forma estandarizada.
* matplotlib y seaborn: visualizar curvas de entrenamiento y matrices de confusión.
* numpy: operar con tensores y arreglos numéricos.
* os: manejar rutas y archivos si es necesario.

El ecosistema: TensorFlow como motor de deep learning y tfds como atajo para acceder a datasets ya preparados.

**Chunk 2 – Carga del dataset EuroSAT y obtención de metadatos**

Objetivo conceptual:

En este bloque se descarga el dataset EuroSAT a través de tfds y se obtienen sus metadatos:

* info contiene información estructurada del conjunto de datos (número de clases, nombres, tamaño, etc.).
* Se realiza una partición manual del split train de EuroSAT en:

    * 60% para entrenamiento (train_raw),
    * 20% para validación (test_raw aquí se usa como “test intermedio”),
    * 20% para validación final (val_raw).

Además, se extraen:

* los nombres de las clases (class_names),
* el número de clases (num_classes),
* y el número total de ejemplos del split de entrenamiento.

Emplea EuroSAT: imágenes Sentinel-2 de 64×64 píxeles, multiclase, y una partición típica train/val/test para evaluar de manera justa una red.

```python
# Cargar dataset con info
all_ds, info = tfds.load("eurosat", with_info=True)

# Dividir en train / val / test (60 / 20 / 20)
train_raw = tfds.load("eurosat", split="train[:60%]")
test_raw  = tfds.load("eurosat", split="train[60%:80%]")
val_raw   = tfds.load("eurosat", split="train[80%:]")

class_names  = info.features["label"].names
num_classes  = len(class_names)
num_examples = info.splits["train"].num_examples

print("Clases:", class_names)
print("Número de clases:", num_classes)
print("Total de ejemplos:", num_examples)

```
Chunk 3 – Función de preprocesamiento y creación de tf.data pipelines

El siguiente bloque es explicado debajo:

```python
def prepare_for_training(ds,
                         batch_size=64,
                         cache=True,
                         shuffle_buffer_size=1000):
    # Opcionalmente cachea para que no reprocese en cada epoch
    if cache:
        ds = ds.cache()

    # (imagen, etiqueta one-hot)
    ds = ds.map(
        lambda d: (
            tf.cast(d["image"], tf.float32) / 255.0,
            tf.one_hot(d["label"], num_classes)
        ),
        num_parallel_calls=tf.data.AUTOTUNE
    )
    # (aquí, internamente) shuffle, repeat, batch, prefetch
    ds = ds.shuffle(shuffle_buffer_size)
    ds = ds.repeat()
    ds = ds.batch(batch_size)
    ds = ds.prefetch(tf.data.AUTOTUNE)
    return ds

batch_size = 64

train_ds = prepare_for_training(train_raw, batch_size=batch_size)
val_ds   = prepare_for_training(val_raw,   batch_size=batch_size)

# Verificar shapes
for images, labels in train_ds.take(1):
    print("Batch imágenes:", images.shape)
    print("Batch etiquetas:", labels.shape)

```
Objetivo conceptual:

Este chunk construye el pipeline de datos eficiente para entrenamiento:

1. Normalización: 

    * Convierte las imágenes a float32 y las escala a 0, 1 dividiendo por 255.

2. Codificación de las etiquetas:

* Las etiquetas enteras se transforman a one-hot (tf.one_hot) para usarlas con categorical_crossentropy.

3. Optimización del pipeline:

    * cache() evita recalcular transformaciones cada época.
    * shuffle() mezcla los ejemplos para romper dependencias entre muestras consecutivas.
    * repeat() hace que el dataset sea infinito, y el número de pasos por época se controla después.
    * batch() agrupa ejemplos en lotes de tamaño 64 (mejor uso de GPU/TPU).
    * prefetch() solapa carga de datos y entrenamiento, mejorando el rendimiento.

Este bloque responde a la pregunta: cómo convertimos un dataset “crudo” en un flujo continuo de batches listo para la CNN.


Chunk 4 – Definición y compilación de la CNN

```python
from tensorflow.keras import layers, models

# Las imágenes de EuroSAT son de 64x64x3
input_shape = (64, 64, 3)

model = models.Sequential([
    layers.Input(shape=input_shape),

    # Bloque 1
    layers.Conv2D(32, (3, 3), activation='relu', padding='same'),
    layers.MaxPooling2D((2, 2)),

    # Bloque 2
    layers.Conv2D(64, (3, 3), activation='relu', padding='same'),
    layers.MaxPooling2D((2, 2)),

    # Bloques adicionales de convolución + pooling (profundizan la red)
    ...
    layers.MaxPooling2D((2, 2)),

    # Aplanado y clasificación
    layers.Flatten(),
    layers.Dense(128, activation='relu'),
    layers.Dropout(0.5),

    # Capa final de clasificación: num_classes viene del dataset (EuroSAT)
    layers.Dense(num_classes, activation='softmax')
])

model.compile(
    loss='categorical_crossentropy',
    optimizer='adam',
    metrics=['accuracy']
)

model.summary()

```

Objetivo conceptual:

Aquí se define la arquitectura de la red convolucional:

* Varios bloques Conv2D + MaxPooling2D:

    * Las convoluciones extraen patrones espaciales (texturas, bordes, formas).
    * El pooling reduce la resolución espacial y hace el modelo más robusto al ruido y a traslaciones pequeñas.

* Una parte final densa:

    * Flatten() convierte los mapas de activación en un vector.
    * Dense(128, relu) aprende combinaciones de alto nivel.
    * Dropout(0.5) reduce el sobreajuste apagando aleatoriamente neuronas durante el entrenamiento.

*La capa final Dense(num_classes, softmax) produce una distribución de probabilidad sobre las clases de uso del suelo.

La red se compila con:

* **Pérdida** categorical_crossentropy, adecuada para clasificación multiclase con etiquetas one-hot.
* **Optimizador** *Adam*, un estándar muy usado por su buen compromiso entre estabilidad y rapidez.
* **Métrica de seguimiento:** accuracy.

En el video, este es el momento para explicar por qué una CNN es adecuada para imágenes satelitales: aprovecha la estructura 2D y aprende filtros que capturan patrones espectrales-espaciales.


Chunk 5 – Entrenamiento del modelo (fit con tf.data)

```python
# Tamaño del batch (el mismo que usaste para train_ds / val_ds)
batch_size = 64

# Número total de ejemplos en el split "train" de EuroSAT
num_examples = info.splits["train"].num_examples

# 60% train, 20% val, 20% test (como definimos antes)
train_size = int(num_examples * 0.6)
val_size   = int(num_examples * 0.2)

# Como usamos .repeat() en train_ds y val_ds, definimos steps por epoch
n_train_steps = train_size // batch_size
n_val_steps   = val_size   // batch_size

print("Ejemplos train:", train_size, "→ steps/epoch:", n_train_steps)
print("Ejemplos val:  ", val_size,   "→ val_steps:",   n_val_steps)

history = model.fit(
    train_ds,
    validation_data=val_ds,
    steps_per_epoch=n_train_steps,
    validation_steps=n_val_steps,
    epochs=5,        # podés subirlo después a 10–20
    verbose=1
)

```

Objetivo conceptual:

Este bloque controla el proceso de entrenamiento:

* Como los datasets se han configurado con .repeat(), es necesario indicar explícitamente cuántos steps componen una época.
* A partir del número total de ejemplos y del batch_size, se calcula:

    * steps_per_epoch ≈ ejemplos de entrenamiento / batch_size
    * validation_steps ≈ ejemplos de validación / batch_size

* Se entrena durante un número fijo de épocas (epochs=5 en este experimento rápido) y se almacena el historial de entrenamiento en history.

Conceptualmente, aquí la CNN “aprende” a mapear imágenes de 64×64×3 a clases de uso del suelo, ajustando sus pesos para minimizar la pérdida en el conjunto de entrenamiento, mientras se controla el desempeño en validación.


Chunk 6 – Visualización y resumen de las curvas de entrenamiento

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 4))

# Pérdida
plt.subplot(1, 2, 1)
plt.plot(history.history['loss'], marker='o', label='train loss')
plt.plot(history.history['val_loss'], marker='o', label='val loss')
plt.xlabel('Epoch')
plt.ylabel('Loss')
plt.legend()
plt.title('Pérdida')

# Exactitud
plt.subplot(1, 2, 2)
plt.plot(history.history['accuracy'], marker='o', label='train acc')
plt.plot(history.history['val_accuracy'], marker='o', label='val acc')
...
plt.tight_layout()
plt.show()

# ============================
# Imprimir valores numéricos
# ============================
print("\nResultados por época:\n")
num_epochs = len(history.history['loss'])
for epoch in range(num_epochs):
    train_loss = history.history['loss'][epoch]
    val_loss   = history.history['val_loss'][epoch]
    train_acc  = history.history['accuracy'][epoch]
    val_acc    = history.history['val_accuracy'][epoch]
    print(f"Época {epoch+1}: "
          f"loss={train_loss:.4f}, val_loss={val_loss:.4f}, "
          f"acc={train_acc:.4f}, val_acc={val_acc:.4f}")

```

Objetivo conceptual:

Este bloque analiza el comportamiento del entrenamiento:

* Se grafican las curvas de pérdida y exactitud tanto para entrenamiento como para validación.
* Visualmente se puede evaluar:
    * si el modelo está aprendiendo (la pérdida baja, la exactitud sube),
    * si hay sobreajuste (train mejora mientras val se estanca o empeora).
* Además, se imprimen los valores numéricos por época, útil para documentar resultados en informes o presentaciones.

Mostrar las curvas y comentar qué nos dicen sobre la calidad del entrenamiento.

**Chunk 7** – Preparar el conjunto de test y obtener predicciones

```python
import tensorflow as tf
import numpy as np

def prepare_for_eval(ds):
    return ds.map(
        lambda d: (
            tf.cast(d["image"], tf.float32) / 255.0,
            d["label"]
        ),
        num_parallel_calls=tf.data.AUTOTUNE
    ).batch(64).prefetch(tf.data.AUTOTUNE)

test_ds = prepare_for_eval(test_raw)

# Acumulamos todas las imágenes y etiquetas del test
all_images = []
all_labels = []

for imgs, lbls in test_ds:
    all_images.append(imgs.numpy())
    all_labels.append(lbls.numpy())

all_images = np.concatenate(all_images, axis=0)
all_labels = np.concatenate(all_labels, axis=0)

print("all_images:", all_images.shape)
print("all_labels:", all_labels.shape)

# Predicciones del modelo
probs = model.predict(all_images, batch_size=64, verbose=1)
preds = np.argmax(probs, axis=1)

```

Objetivo conceptual:

Este chunk construye un pipeline específico para evaluación:

* A diferencia del entrenamiento, aquí no se hace one_hot ni shuffle ni repeat.
* Se normalizan las imágenes igual que antes, se agrupan en batches y se prefetchan.
* Se recopilan todas las imágenes y etiquetas del conjunto de test en arreglos numpy.
* El modelo produce probabilidades (probs) para cada clase y, con argmax, se obtienen las clases predichas (preds).

Este paso es clave para poder calcular métricas globales en test y analizar el rendimiento real de la red sobre datos nunca vistos durante el entrenamiento.

**Chunk 8** – Cálculo de métricas globales (Accuracy y F1-macro)

```python
from sklearn.metrics import accuracy_score, f1_score

acc = accuracy_score(all_labels, preds)
f1_macro = f1_score(all_labels, preds, average='macro')

print("Accuracy en test:", acc)
print("F1 macro en test:", f1_macro)

```
Objetivo conceptual:

Aquí se cuantifica el desempeño del modelo en el conjunto de test usando métricas estándar de clasificación:

* Accuracy: proporción de ejemplos correctamente clasificados.
* F1-macro: media del F1 calculado por clase, sin ponderar por frecuencia; es decir, le da el mismo peso a todas las clases, incluso a las minoritarias.

En contexto académico, F1-macro es especialmente útil cuando el dataset está desbalanceado, porque evita que clases muy frecuentes dominen la métrica.

Chunk 9 – Matriz de confusión y visualización detallada de errores

```python
from sklearn.metrics import confusion_matrix
import seaborn as sns
import matplotlib.pyplot as plt

cm = confusion_matrix(all_labels, preds)

# Normalizamos por filas (clase real)
cm_norm = cm.astype('float') / cm.sum(axis=1, keepdims=True)

plt.figure(figsize=(10, 8))
sns.heatmap(
    cm_norm,
    annot=True,
    fmt=".2f",
    cmap="Blues",
    xticklabels=class_names,
    yticklabels=class_names
)
plt.ylabel("Clase real")
plt.xlabel("Clase predicha")
plt.title("Matriz de confusión (normalizada por clase real)")
plt.tight_layout()
plt.show()

```
Objetivo conceptual:

Finalmente, se construye y visualiza la matriz de confusión, normalizada por filas:

* Cada fila corresponde a la clase real.
* Cada columna, a la clase predicha.

Los valores indican la proporción de ejemplos de una clase real que fueron asignados a cada clase predicha.

Esto permite identificar:

* Qué clases se confunden más entre sí.
* Si hay clases que el modelo apenas reconoce.
* Patrones sistemáticos de error (por ejemplo, “cultivos” confundidos con “pastizales”).

Para un video, esta figura es muy visual: puedes ir señalando cuadrículas y contar historias como “el modelo tiende a confundir tal tipo de uso del suelo con tal otro, probablemente porque sus firmas espectrales son similares”.

```python

```


```python

```

```python

```


```python

```

```python

```


```python

```

```python

```


```python

```


## Ejemplo 2: Clasificación multiclase

### Exportación de mosaicos al drive

A partir de nuestro ejemplo Lab_002_ RandomForest_Rosario, generamos mosaicos estacionales (veranoGrid, otonoGrid, inviernoGrid, primaveraGrid),
la clasificación RF reproyectada (rfLandcoverAoi) y el WorldCover reproyectado (worldcoverAoi). Alinea todo a la grilla de Sentinel-2 (10 m, UTM 20S) asi saldrán perfectamente alineados píxel a píxel en los GeoTIFF exportados a Drive. Exporta imágenes completas a Drive

```javascript
// // Exportacion de datos para CNN
// Estoy en UTM 20S (EPSG:32720) → coherente para Rosario/Zona Pampeana.
// Resolución 10 m.
// Transform con paso de 10 en X y –10 en Y → grilla nativa de Sentinel-2.
// Con esto:
// Los mosaicos estacionales (veranoGrid, otonoGrid, inviernoGrid, primaveraGrid)
// La clasificación RF reproyectada (rfLandcoverAoi)
// El WorldCover reproyectado (worldcoverAoi)
// van a salir perfectamente alineados píxel a píxel en los GeoTIFF exportados a Drive.

// ============================================================
// A. DEFINIR AOI / GEOM Y PROYECCIÓN OBJETIVO CORRECTA
// ============================================================

// Suponemos que ya definiste "aoi" como ee.Geometry
var geom = aoi;

// Tomamos una imagen Sentinel-2 como referencia de proyección (10 m, UTM)
var refImage = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
  .filterBounds(geom)
  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 30))
  .first();

// Proyección nativa de S2 (de la banda B2, por ejemplo)
var targetProj  = refImage.select('B2').projection();
var targetCrs   = targetProj.crs();
var targetScale = targetProj.nominalScale();  // debería ser 10 m

print('Proyección objetivo (S2 nativo):', targetProj);
print('CRS objetivo:', targetCrs);
print('Escala objetivo (m):', targetScale);

// Función auxiliar para forzar una imagen a la grilla de Sentinel-2
function toTargetGrid(img) {
  return img
    .reproject({
      crs: targetCrs,
      scale: targetScale
    })
    .clip(geom);
}

// ============================================================
// B. COLECCIÓN SENTINEL-2 SR HARMONIZED (AÑO 2024)
// ============================================================
// Usamos la misma colección que en tu código inicial (S2_SR_HARMONIZED)
// pero filtrada por AOI y año 2024.

var s2_seasonal = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
  .filterBounds(geom)
  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 30))
  .filter(ee.Filter.date('2024-01-01', '2025-01-01'))
  .select('B.*');  // todas las bandas ópticas B*


// ============================================================
// C. FUNCIÓN PARA MOSAICO POR ESTACIÓN (AÑO 2024)
// ============================================================

function seasonalMosaic(start, end) {
  return s2_seasonal
    .filterDate(start, end)
    .median()
    .clip(geom);
}

// Año de interés
var year = 2024;

// En hemisferio sur (Argentina):
// Verano: enero–marzo, Otoño: abril–junio, Invierno: julio–septiembre,
// Primavera: octubre–diciembre
var verano    = seasonalMosaic(year + '-01-01', year + '-03-31');
var otono     = seasonalMosaic(year + '-04-01', year + '-06-30');
var invierno  = seasonalMosaic(year + '-07-01', year + '-09-30');
var primavera = seasonalMosaic(year + '-10-01', year + '-12-31');

// Ajustamos cada mosaico a la misma grilla S2 (EPSG:32720, 10 m)
var veranoGrid    = toTargetGrid(verano);
var otonoGrid     = toTargetGrid(otono);
var inviernoGrid  = toTargetGrid(invierno);
var primaveraGrid = toTargetGrid(primavera);


// ============================================================
// D. VISUALIZACIÓN RÁPIDA (OPCIONAL)
// ============================================================

Map.centerObject(geom, 11);
Map.addLayer(veranoGrid,    {bands: ['B4','B3','B2'], min: 0, max: 3000}, 'Verano 2024 RGB');
Map.addLayer(inviernoGrid,  {bands: ['B4','B3','B2'], min: 0, max: 3000}, 'Invierno 2024 RGB');
Map.addLayer(geom, {color: 'red'}, 'AOI');


// ============================================================
// E. EXPORTAR MOSAICOS ESTACIONALES A DRIVE
// ============================================================

function exportSeason(img, name) {
  Export.image.toDrive({
    image: img,
    description: 'S2_' + name + '_2024',
    folder: 'S2_Parches_AOI',   // carpeta en tu Google Drive
    fileNamePrefix: 'S2_' + name + '_2024',
    region: geom,
    scale: 10,                  // o targetScale
    maxPixels: 1e13             // sin crs / crsTransform: usa la proyección de img
  });
}

// Activá/Desactivá según necesidad:
exportSeason(veranoGrid,    'verano');
exportSeason(otonoGrid,     'otono');
exportSeason(inviernoGrid,  'invierno');
exportSeason(primaveraGrid, 'primavera');


// ============================================================
// F. EXPORTAR COBERTURA RF (CLASIFICACIÓN) PARA AOI
// ============================================================
// "classified" viene de tu código original:
// var classified = composite.classify(classifier);

var rfLandcoverAoi = toTargetGrid(classified).rename('landcover_rf');

Map.addLayer(
  rfLandcoverAoi,
  {min: 0, max: 4, palette: ['blue', 'gray', 'green', 'violet', 'orange']},
  'RF landcover (AOI)'
);

Export.image.toDrive({
  image: rfLandcoverAoi,
  description: 'RF_Landcover_2024',
  folder: 'S2_Parches_AOI',
  fileNamePrefix: 'RF_Landcover_2024',
  region: geom,
  scale: 10,
  maxPixels: 1e13
});


// ============================================================
// G. WORLD COVER REPROYECTADO / ALINEADO A LA MISMA GRILLA
// ============================================================

var wcRaw = ee.ImageCollection('ESA/WorldCover/v200')
  .first()
  .select('Map');

// Reproyectar y alinear a la grilla Sentinel-2 (nearest implícito)
var worldcoverAoi = wcRaw
  .reproject({
    crs: targetCrs,    // misma proyección que S2 y RF
    scale: targetScale // 10 m
  })
  .clip(geom)
  .rename('worldcover');  

Map.addLayer(
  worldcoverAoi,
  {min: 10, max: 100, palette: ['red','yellow','green','cyan','blue','purple']},
  'WorldCover 2021 (AOI)'
); 
 
Export.image.toDrive({
  image: worldcoverAoi,
  description: 'WorldCover_2021_AOI',
  folder: 'S2_Parches_AOI',
  fileNamePrefix: 'WorldCover_2021_AOI',
  region: geom,
  scale: 10,
  maxPixels: 1e13
});

```

### Generar Mosaicos

Desde Colab procesamos los mosaicos para generar parches de entrenamiento.

0) Instalaciones e imports básicos — Descripción en estilo académico

En este apartado se establecen los componentes fundamentales necesarios para ejecutar el procesamiento geoespacial posterior. En primer lugar, se instala la librería rasterio, indispensable para la lectura y manipulación de datos raster provenientes de sensores satelitales. Luego, se monta Google Drive en el entorno de Colab, permitiendo acceder directamente a archivos almacenados allí y evitando tener que cargarlos manualmente. Finalmente, se importan librerías esenciales: os para manejar rutas del sistema, numpy para operaciones matriciales sobre imágenes multibanda, rasterio para abrir y leer archivos geográficos en formato TIFF, y Counter para posibles análisis estadísticos de frecuencias. Este ítem cumple la función de preparar el entorno y garantizar que todas las dependencias necesarias estén disponibles antes de trabajar con los datos satelitales.

```python
# ============================================================
# 0) Instalaciones e imports básicos
# ============================================================
!pip install rasterio

from google.colab import drive
drive.mount('/content/drive')

import os
import numpy as np
import rasterio
from collections import Counter
```

Rutas de los archivos exportados desde GEE — Descripción en estilo académico

En este apartado se definen explícitamente las rutas de acceso a los archivos satelitales que fueron previamente descargados desde Google Earth Engine (GEE). La estructura establece una carpeta base (DATA_DIR) donde se encuentran todos los mosaicos y productos auxiliares. Luego, se organiza la información en un diccionario que asocia cada estación del año con su respectivo mosaico Sentinel-2, permitiendo un manejo más ordenado y programático. Además, se especifican las rutas hacia dos capas temáticas externas: una clasificación de cobertura terrestre generada mediante Random Forest (RF) y la capa global WorldCover. Esta sección cumple el objetivo de estandarizar y centralizar la ubicación de los archivos de entrada, facilitando la trazabilidad del proyecto y reduciendo la posibilidad de errores en la manipulación posterior de datos.


```python
# ============================================================
# 1) Rutas de los archivos exportados desde GEE
# ============================================================

DATA_DIR = "/content/drive/MyDrive/S2_Parches_AOI"

# Mosaicos estacionales (nombres tal como los exportaste desde GEE)
s2_files = {
    "verano":    os.path.join(DATA_DIR, "S2_verano_2024.tif"),
    "otono":     os.path.join(DATA_DIR, "S2_otono_2024.tif"),
    "invierno":  os.path.join(DATA_DIR, "S2_invierno_2024.tif"),
    "primavera": os.path.join(DATA_DIR, "S2_primavera_2024.tif"),
}

# Cobertura RF y WorldCover (nombres que pusimos en GEE)
rf_path = os.path.join(DATA_DIR, "RF_Landcover_2024.tif")
wc_path = os.path.join(DATA_DIR, "WorldCover_2021_AOI.tif")

print("Archivos de entrada:")
for k, v in s2_files.items():
    print(f"  {k}: {v}")
print("  RF:", rf_path)
print("  WorldCover:", wc_path)

```

Leer mosaicos S2, RF y WorldCover — Descripción en estilo académico

Este apartado se encarga de cargar efectivamente los datos satelitales en memoria y verificar su consistencia espacial. Para cada mosaico estacional Sentinel-2, se abre el archivo y se lee la estructura multibanda en formato (bands, H, W). El primer mosaico leído establece la referencia geoespacial (proyección, resolución, affine transform) y las dimensiones que deben compartir todos los datasets. A continuación, se realizan verificaciones estrictas para asegurar que cada mosaico adicional coincide en tamaño y alineación espacial, condición imprescindible para análisis multitemporales y generación de modelos. Posteriormente, se cargan las capas RF y WorldCover, ambas de una sola banda, comprobando nuevamente que compartan dimensiones y sistema de referencia con los mosaicos Sentinel-2. Este ítem garantiza la coherencia espacial del conjunto de datos y prepara las matrices raster para cualquier operación analítica posterior.

```python

# ============================================================
# 2) Leer mosaicos S2, RF y WorldCover
# ============================================================

# Leer mosaicos estacionales como (bands, H, W)
s2_mosaics = {}
ref_profile = None

for season, path in s2_files.items():
    with rasterio.open(path) as src:
        img = src.read()  # (bands, H, W)
        if ref_profile is None:
            ref_profile = src.profile
            H, W = img.shape[1], img.shape[2]
            print(f"Referencia: {season} → shape: {img.shape}, crs: {src.crs}")
        else:
            # chequeo rápido de consistencia
            if img.shape[1] != H or img.shape[2] != W:
                raise ValueError(f"El mosaico {season} no coincide en tamaño con el de referencia")
            if src.transform != ref_profile['transform']:
                raise ValueError(f"El mosaico {season} no coincide en transform con el de referencia")
        s2_mosaics[season] = img

# Leer RF landcover (1 banda)
with rasterio.open(rf_path) as src_rf:
    rf = src_rf.read(1)  # (H, W)
    rf_nodata = src_rf.nodata
    print("RF shape:", rf.shape, "nodata:", rf_nodata)
    # chequeos rápidos
    if rf.shape != (H, W):
        raise ValueError("RF no coincide en shape con los mosaicos S2")
    if src_rf.transform != ref_profile['transform']:
        raise ValueError("RF no coincide en transform con S2")

# Leer WorldCover (1 banda)
with rasterio.open(wc_path) as src_wc:
    wc = src_wc.read(1)  # (H, W)
    wc_nodata = src_wc.nodata
    print("WorldCover shape:", wc.shape, "nodata:", wc_nodata)
    if wc.shape != (H, W):
        raise ValueError("WorldCover no coincide en shape con los mosaicos S2")
    if src_wc.transform != ref_profile['transform']:
        raise ValueError("WorldCover no coincide en transform con S2")

```

Funciones auxiliares para mayoría y pureza

En este apartado se define la función majority_and_purity, cuya finalidad es caracterizar un parche de clasificación categórica (por ejemplo, de uso/cobertura del suelo) a partir de los valores de sus píxeles. La función recibe como entrada un arreglo bidimensional (patch) y, opcionalmente, un valor de nodata que debe excluirse del análisis. Primero, el parche se aplana a un vector unidimensional y se filtran los píxeles que coinciden con el valor de nodata, de modo que solo se consideren celdas válidas. Si luego de esta depuración no quedan píxeles válidos, la función devuelve None y una pureza de 0.0, indicando que el parche no es utilizable. En caso contrario, se calcula la frecuencia de cada clase presente utilizando np.unique y se identifica la clase mayoritaria como aquella con mayor número de ocurrencias. A continuación, se computa la pureza del parche como el cociente entre la cantidad de píxeles de la clase mayoritaria y el total de píxeles válidos del parche. Finalmente, la función retorna dos valores: la etiqueta de la clase mayoritaria y la pureza expresada como un número flotante entre 0 y 1, que cuantifica qué tan homogéneo es el parche desde el punto de vista de las clases presentes.

```python

# ============================================================
# 3) Funciones auxiliares para mayoría y pureza
# ============================================================

def majority_and_purity(patch, nodata=None):
    """
    patch: array 2D (patch_size x patch_size)
    nodata: valor a excluir (o None si no hay)
    """
    vals = patch.flatten()
    if nodata is not None:
        vals = vals[vals != nodata]
    if vals.size == 0:
        return None, 0.0
    # contaje de clases
    values, counts = np.unique(vals, return_counts=True)
    idx_max = np.argmax(counts)
    majority = values[idx_max]
    purity = counts[idx_max] / counts.sum()
    return majority, float(purity)

```

Parámetros de generación de parches

En este apartado se establecen los parámetros globales que controlan la generación de parches espaciales a partir de las imágenes originales. En primer lugar, se define el tamaño del parche (PATCH_SIZE = 10), lo que implica que cada ejemplo extraído del mosaico tendrá una dimensión de 10 × 10 píxeles. Luego se fija el STRIDE = 10, que determina el desplazamiento entre parches consecutivos en filas y columnas; al coincidir con el tamaño del parche, se produce un muestreo sin solapamiento (aunque el código deja abierta la posibilidad de usar un valor menor para permitir solapamiento entre parches). A continuación, se establecen los umbrales mínimos de pureza tanto para la clasificación de Random Forest (MIN_PURITY_RF = 0.8) como para la de WorldCover reclasificada (MIN_PURITY_WC = 0.8), lo que significa que solo se considerarán parches suficientemente homogéneos, con al menos el 80 % de los píxeles pertenecientes a la clase mayoritaria. Finalmente, se imprime información diagnóstica sobre las dimensiones de la imagen (H y W), el tamaño de parche y el stride, lo cual facilita la verificación de que la configuración seleccionada es coherente con la resolución y extensión espacial de los datos.


```python
# ============================================================
# 4) Parámetros de generación de parches
# ============================================================

PATCH_SIZE = 10   # 10 píxeles × 10 píxeles
STRIDE = 10       # sin solapamiento (podés poner 5 si querés solapar)
MIN_PURITY_RF = 0.8
MIN_PURITY_WC = 0.8

print("Tamaño imagen:", H, "x", W)
print("Patch size:", PATCH_SIZE, "stride:", STRIDE)

import numpy as np

```

Reclasificar WorldCover a la leyenda del RF

En este apartado se lleva a cabo la reclasificación del producto WorldCover para que su leyenda sea compatible con la utilizada por el modelo de Random Forest (RF). Para ello, se crea inicialmente un arreglo wc_reclass con la misma forma que wc, rellenado con el valor -1, que se utiliza como código de nodata o valor no válido. A continuación, se definen reglas de mapeo explícitas entre las clases de WorldCover y las clases enteras empleadas por RF: los píxeles con valor 80 se asignan a la clase 0 (agua), los de valor 50 a la clase 1 (urbano), los de valor 40 a la clase 2 (cultivo), los de valor 10 a la clase 3 (bosque) y los de valor 60 a la clase 4 (suelo desnudo). Este procedimiento garantiza que ambas fuentes de información compartan la misma codificación de clases, lo cual es fundamental para comparar, combinar o establecer criterios de consenso entre RF y WorldCover. Posteriormente, se imprime el conjunto de valores únicos presentes en wc_reclass como verificación rápida del resultado de la reclasificación, y se almacena el valor wc_nodata_reclass = -1 para identificar de manera consistente los píxeles que deben ser tratados como ausencia de datos en los pasos posteriores del procesamiento.


```python
import numpy as np

# ============================================================
# 6) Reclasificar WorldCover a la leyenda del RF
# ============================================================

# Crear copia reclasificable
wc_reclass = np.full_like(wc, fill_value=-1, dtype=np.int16)  # -1 = nodata

# Mapeos WorldCover → RF
wc_reclass[wc == 80] = 0   # agua
wc_reclass[wc == 50] = 1   # urbano
wc_reclass[wc == 40] = 2   # cultivo
wc_reclass[wc == 10] = 3   # bosque
wc_reclass[wc == 60] = 4   # suelo desnudo

print("Valores únicos en wc_reclass:", np.unique(wc_reclass))

wc_nodata_reclass = -1
```

Generar parches con regla mixta (RF y WorldCover)

En este apartado se implementa la lógica principal de generación de parches de entrenamiento mediante una estrategia de selección mixta que combina información de RF y del WorldCover reclasificado. Primero se inicializan listas vacías para almacenar los parches aceptados (accepted_patches), sus etiquetas (accepted_labels) y las coordenadas de su esquina superior izquierda (accepted_coords), así como contadores que permiten registrar el número total de parches evaluados y cuántos son aceptados bajo cada criterio. Se establece el orden de las estaciones en season_order para, más adelante, poder concatenar consistentemente los mosaicos estacionales de Sentinel-2. A continuación, se recorre sistemáticamente la imagen en filas y columnas utilizando el STRIDE, extrayendo para cada posición un parche de la clasificación RF (rf_patch) y el correspondiente parche reclasificado de WorldCover (wc_patch). Para cada uno de estos parches se calcula la clase mayoritaria y su pureza mediante la función majority_and_purity, tanto para RF como para WorldCover.

Si el parche RF no tiene datos válidos, se descarta inmediatamente. En caso contrario, se evalúa una regla de aceptación que distingue entre clases frecuentes (0, 1, 3) y clases menos frecuentes (2, 4). Para las clases frecuentes, se exige que la clase mayoritaria de RF coincida con la de WorldCover, y que ambas purezas superen los umbrales mínimos definidos (MIN_PURITY_RF y MIN_PURITY_WC), lo que garantiza un alto grado de consenso entre las dos fuentes. Para las clases menos frecuentes, en cambio, se permite aceptar el parche solo en función de RF, siempre que su pureza sea suficientemente alta, con el objetivo de no perder ejemplos de clases raras por falta de coincidencia con WorldCover. Si el parche no cumple las condiciones, se descarta; si las cumple, se procede a extraer los parches espectrales de Sentinel-2 para cada estación, concatenándolos a lo largo del eje de bandas para obtener un único tensor (patch_stack) que integra la información multitemporal del año. Este tensor se agrega a la lista de parches aceptados, mientras que la clase mayoritaria de RF se almacena como etiqueta y las coordenadas como referencia espacial.

Al finalizar el recorrido completo de la imagen, se imprimen estadísticas globales sobre el número total de parches evaluados y aceptados bajo cada criterio, así como la distribución final de clases. Luego, las listas se convierten en arreglos NumPy: X contiene los parches (dimensión número_de_parches × bandas_totales × PATCH_SIZE × PATCH_SIZE), y contiene las etiquetas de clase y coords las coordenadas de cada parche. Finalmente, todo el conjunto de datos, junto con el orden de las estaciones, se guarda en un archivo comprimido .npz mediante np.savez, lo que deja preparado un dataset estructurado y listo para ser utilizado en el entrenamiento de modelos de aprendizaje profundo o de otras técnicas de clasificación supervisada.

```python
# ============================================================
# 7) Generar parches con regla mixta:
#       - Clases 0,1,3 → RF==WC + purezas
#       - Clases 2 y 4 → solo RF con pureza alta
# ============================================================

accepted_patches = []
accepted_labels = []
accepted_coords = []

season_order = ["verano", "otono", "invierno", "primavera"]

num_total = 0
num_rf_wc_match = 0
num_rf_only_24 = 0

for r in range(0, H - PATCH_SIZE + 1, STRIDE):
    for c in range(0, W - PATCH_SIZE + 1, STRIDE):
        num_total += 1

        rf_patch = rf[r:r+PATCH_SIZE, c:c+PATCH_SIZE]
        wc_patch = wc_reclass[r:r+PATCH_SIZE, c:c+PATCH_SIZE]

        maj_rf, pur_rf = majority_and_purity(rf_patch, rf_nodata)
        maj_wc, pur_wc = majority_and_purity(wc_patch, wc_nodata_reclass)

        if maj_rf is None:
            continue

        accept = False

        # Caso A: clases frecuentes → requieren RF == WC
        if maj_rf in (0, 1, 3):
            if maj_wc is not None:
                if (maj_rf == maj_wc and
                    pur_rf >= MIN_PURITY_RF and
                    pur_wc >= MIN_PURITY_WC):
                    accept = True
                    num_rf_wc_match += 1

        # Caso B: clases minoritarias → solo RF + pureza
        elif maj_rf in (2, 4):
            if pur_rf >= MIN_PURITY_RF:
                accept = True
                num_rf_only_24 += 1

        if not accept:
            continue

        # Extraer parches S2 concatenando estaciones
        patches_seasons = []
        for season in season_order:
            mosaic = s2_mosaics[season]
            p = mosaic[:, r:r+PATCH_SIZE, c:c+PATCH_SIZE]
            patches_seasons.append(p)

        patch_stack = np.concatenate(patches_seasons, axis=0)

        accepted_patches.append(patch_stack)
        accepted_labels.append(maj_rf)
        accepted_coords.append((r, c))


print("========================================")
print("Parches posibles:", num_total)
print("Parches aceptados RF==WC (clases 0,1,3):", num_rf_wc_match)
print("Parches aceptados RF-only (clases 2,4):", num_rf_only_24)

X = np.stack(accepted_patches, axis=0)
y = np.array(accepted_labels)
coords = np.array(accepted_coords)

print("Parches aceptados totales:", X.shape[0])
print("Distribución de clases:", {cls:int(cnt) for cls,cnt in zip(*np.unique(y, return_counts=True))})

# Guardar dataset
out_path = os.path.join(DATA_DIR, "patches_10x10_RF_mixed_strategy.npz")
np.savez(out_path, X=X, y=y, coords=coords, season_order=np.array(season_order))
print("Dataset guardado en:", out_path)

```

### analisis de parches generados


Carga del dataset de parches

En este apartado se define la ruta al directorio de trabajo en Google Drive y se carga el archivo .npz que contiene el dataset de parches previamente generado. A partir de este archivo se extraen las variables principales: X, que almacena los parches de Sentinel-2 como un tensor de dimensión (𝑁, bands_total, 10, 10) (N,bands_total,10,10); y, que contiene la etiqueta de clase asociada a cada parche; coords, que guarda las coordenadas de fila y columna del parche dentro del mosaico original; y season_order, que indica el orden en que se concatenaron las estaciones (verano, otoño, invierno, primavera). Además, se imprime la forma de estos arreglos y se calcula la distribución de clases, lo cual permite verificar la integridad del dataset, la cantidad de muestras disponibles y el balance de clases antes de avanzar con análisis o entrenamiento de modelos.

```python
import os
import numpy as np
import matplotlib.pyplot as plt

from google.colab import drive
drive.mount('/content/drive')

# ============================================================
# 1) Cargar el dataset de parches
# ============================================================

DATA_DIR = "/content/drive/MyDrive/S2_Parches_AOI"
npz_path = os.path.join(DATA_DIR, "patches_10x10_RF_mixed_strategy.npz")

data = np.load(npz_path, allow_pickle=True)
X = data["X"]              # (N, bands_total, 10, 10)
y = data["y"]              # (N,)
coords = data["coords"]    # (N, 2)
season_order = data["season_order"]

print("X shape:", X.shape)         # (N, bands_total, 10, 10)
print("y shape:", y.shape)
print("season_order:", season_order)

# Distribución de clases
classes, counts = np.unique(y, return_counts=True)
print("Distribución de clases:")
for cls, cnt in zip(classes, counts):
    print(f"  Clase {cls}: {cnt} parches")
```

Función para construir un RGB de una estación específica

En este bloque se define la función get_rgb_from_patch, cuyo objetivo es transformar un parche multibanda en una imagen RGB interpretable para una estación concreta. La función asume que las bandas están organizadas por estaciones y que, dentro de cada estación, las bandas siguen el orden espectral estándar (B1, B2, B3, B4, …). A partir del índice de estación (season_idx), se calcula el desplazamiento correspondiente y se seleccionan las bandas asociadas al azul (B2), verde (B3) y rojo (B4). Estas bandas se combinan para formar un arreglo de tamaño 
(10, 10,3) (10,10,3) en orden RGB (B4, B3, B2). Posteriormente, se aplica una normalización basada en el percentil 98 para escalar los valores al rango [0, 1] y mejorar el contraste. El resultado es una versión visualmente adecuada del parche, apta para inspección cualitativa.

```python
# ============================================================
# 2) Función para construir un RGB de una estación específica
# ============================================================

def get_rgb_from_patch(patch, season_idx=0):
    """
    patch: array (bands_total, 10, 10)
    season_idx: índice de estación en season_order
                0 = verano, 1 = otoño, 2 = invierno, 3 = primavera

    Asumimos que:
      bands_total = n_bandas_por_estacion * n_estaciones
    y que el orden de bandas dentro de cada estación es:
      B1, B2, B3, B4, ...
    Usaremos B4 (rojo), B3 (verde), B2 (azul).
    """
    bands_total, h, w = patch.shape
    n_seasons = len(season_order)
    bands_per_season = bands_total // n_seasons

    # Offset de la estación seleccionada
    base = season_idx * bands_per_season

    # Índices de B2, B3, B4 suponiendo:
    # B1 -> idx 0, B2 -> 1, B3 -> 2, B4 -> 3
    idx_B2 = base + 1
    idx_B3 = base + 2
    idx_B4 = base + 3

    B2 = patch[idx_B2, :, :].astype(np.float32)
    B3 = patch[idx_B3, :, :].astype(np.float32)
    B4 = patch[idx_B4, :, :].astype(np.float32)

    # Stack en orden RGB = (B4, B3, B2)
    rgb = np.stack([B4, B3, B2], axis=-1)  # (10, 10, 3)

    # Normalización simple 0–1 para visualizar
    # Evitamos dividir por cero:
    max_val = np.percentile(rgb, 98)  # recorte leve para contraste
    if max_val <= 0:
        max_val = 1.0
    rgb_norm = np.clip(rgb / max_val, 0, 1)

    return rgb_norm

```

Función para mostrar ejemplos por clase

En esta sección se implementa la función show_examples_per_class, destinada a la validación visual de los parches por clase temática. La función recibe el conjunto de parches X, sus etiquetas y, el nombre de la estación a visualizar y el número de ejemplos por clase. Primero, traduce el nombre de la estación (season_name) a su índice dentro de season_order. Luego identifica todas las clases presentes en y y, para cada una, selecciona aleatoriamente un subconjunto de parches pertenecientes a esa clase. Para cada parche elegido, se genera su representación RGB mediante la función get_rgb_from_patch y se muestra en una figura de Matplotlib, incluyendo en el título el índice del parche y sus coordenadas dentro del mosaico original. De este modo, la función facilita una revisión visual sistemática de la calidad de los parches y de la correspondencia entre las clases y la apariencia de la cobertura terrestre.

```python
# ============================================================
# 3) Función para mostrar ejemplos por clase
# ============================================================

def show_examples_per_class(X, y, season_name="verano", n_examples=4):
    """
    Muestra n_examples parches por cada clase presente en y,
    usando un RGB de la estación indicada.
    """
    # Mapear nombre de estación -> índice
    season_name = str(season_name)
    season_list = list(season_order)
    if season_name not in season_list:
        raise ValueError(f"season_name debe ser uno de {season_list}")
    season_idx = season_list.index(season_name)

    classes = np.unique(y)

    for cls in classes:
        # Índices de parches de esa clase
        idxs = np.where(y == cls)[0]
        if len(idxs) == 0:
            continue

        print(f"\nClase {cls} — total parches: {len(idxs)}")

        # Elegir algunos índices al azar (sin reemplazo si alcanza)
        n_show = min(n_examples, len(idxs))
        chosen = np.random.choice(idxs, size=n_show, replace=False)

        # Plot
        fig, axes = plt.subplots(1, n_show, figsize=(3*n_show, 3))
        if n_show == 1:
            axes = [axes]

        for ax, idx in zip(axes, chosen):
            patch = X[idx]  # (bands_total, 10, 10)
            rgb = get_rgb_from_patch(patch, season_idx=season_idx)

            ax.imshow(rgb)
            r, c = coords[idx]
            ax.set_title(f"idx={idx}\ncoord=(r{r},c{c})")
            ax.axis("off")

        plt.suptitle(f"Clase {cls} — estación: {season_name}")
        plt.tight_layout()
        plt.show()


```

Llamada a la función para visualizar ejemplos

Finalmente, este apartado realiza la llamada efectiva a la función de visualización, especificando que se muestren cuatro ejemplos por clase (n_examples=4) utilizando la estación de verano (season_name="verano"). Esta ejecución genera una serie de figuras donde, para cada clase presente en el dataset, se observan distintos parches representados en RGB. Además de servir como control de calidad de los datos, esta etapa permite al analista interpretar de manera intuitiva qué tipo de patrones espectrales y espaciales están siendo capturados por los parches para cada categoría de uso/cobertura. El código sugiere explícitamente la posibilidad de repetir este proceso con otras estaciones (por ejemplo, invierno o primavera), lo que aporta una herramienta flexible para comparar la apariencia estacional de las clases y detectar posibles inconsistencias o artefactos en el dataset.

```python
# ============================================================
# 4) Llamar a la función para ver ejemplos
# ============================================================

# Por ejemplo, ver 4 ejemplos por clase en VERANO:
show_examples_per_class(X, y, season_name="verano", n_examples=4)

# Podés probar también:
# show_examples_per_class(X, y, season_name="invierno", n_examples=4)
# show_examples_per_class(X, y, season_name="primavera", n_examples=4)

```

### Modelo

En este bloque de trabajo se establece un flujo completo para preparar un conjunto de parches satelitales orientado al entrenamiento de modelos de clasificación supervisada. Primero, se cargan desde disco los mosaicos previamente procesados y empaquetados en un archivo .npz, permitiendo acceder simultáneamente a los parches multibanda, sus etiquetas de cobertura y las coordenadas espaciales asociadas. Luego, con el fin de garantizar la robustez estadística del modelo, se genera una partición estratificada del dataset en subconjuntos de entrenamiento, validación y prueba, asegurando que la distribución de clases se mantenga proporcional en cada split. A continuación, se calculan pesos de clase basados en la frecuencia relativa de cada categoría dentro del conjunto de entrenamiento, con el propósito de mitigar desbalances que podrían sesgar el aprendizaje durante la optimización de la función de pérdida. Finalmente, se guardan en disco los distintos subconjuntos generados, preservando tanto las muestras como la metainformación necesaria para su uso posterior, consolidando así un pipeline reproducible y estandarizado para el entrenamiento y evaluación de modelos de machine learning aplicados a datos de teledetección.


Bloque inicial: instalación e imports

En el bloque inicial se instala la librería scikit-learn, necesaria para disponer de funciones avanzadas de modelado y, en particular, para realizar la partición estratificada de los datos mediante train_test_split. A continuación, se importan módulos básicos de Python y NumPy para manejar rutas, arrays numéricos y estructuras de conteo (Counter) que facilitan el análisis de la distribución de clases. También se monta Google Drive en el entorno de Colab, lo que permite acceder de forma directa a los archivos almacenados en la carpeta del proyecto. En conjunto, este apartado prepara el entorno de ejecución, garantizando que tanto las dependencias externas como las rutas a los datos estén disponibles antes de comenzar con el procesamiento del dataset de parches.


```python
!pip install scikit-learn

import os
import numpy as np
from sklearn.model_selection import train_test_split
from collections import Counter

from google.colab import drive
drive.mount('/content/drive')
```
Cargar el dataset completo de parches

En el apartado 1 se carga desde disco el dataset completo de parches previamente generado, almacenado en un archivo .npz. A partir de este archivo se extraen los tensores de entrada X (que contienen, para cada parche, todas las bandas y la estructura espacial de 10×10 píxeles), el vector de etiquetas y (clase de cobertura asociada a cada parche), las coordenadas originales coords y el orden de las estaciones season_order. El código imprime las dimensiones de estos arreglos y la distribución total de clases utilizando Counter, lo que permite verificar que el conjunto de datos se ha cargado correctamente y que la cantidad de ejemplos por clase es razonable. Este paso constituye la base sobre la cual se construyen los subconjuntos de entrenamiento, validación y prueba, asegurando que se trabaja con el dataset completo y consistente.



```python
# ============================================================
# 1) Cargar el dataset completo de parches
# ============================================================

DATA_DIR = "/content/drive/MyDrive/S2_Parches_AOI"
npz_path = os.path.join(DATA_DIR, "patches_10x10_RF_mixed_strategy.npz")

data = np.load(npz_path, allow_pickle=True)
X = data["X"]              # (N, bands_total, 10, 10)
y = data["y"]              # (N,)
coords = data["coords"]    # (N, 2)
season_order = data["season_order"]

print("X shape:", X.shape)
print("y shape:", y.shape)
print("Distribución total de clases:", Counter(y.tolist()))
print("season_order:", season_order)
```
Crear splits estratificados: train / val / test

En el apartado 2 se realiza la partición del dataset en tres subconjuntos: entrenamiento, validación y prueba, siguiendo proporciones típicas de 70%, 15% y 15% respectivamente. Para ello se utiliza la función train_test_split de scikit-learn con estratificación (stratify=y), lo que garantiza que la distribución de clases se mantenga aproximadamente constante en cada split. Primero se separa un conjunto de test a partir del total, y posteriormente se divide el conjunto temporal restante en entrenamiento y validación, ajustando el tamaño relativo de la validación para conservar el porcentaje global deseado. Además, se asegura que las coordenadas asociadas a cada parche (coords) se dividan de forma coherente con X e y. Al final se imprimen las dimensiones de cada subconjunto y sus distribuciones de clases, permitiendo verificar que la partición es correcta, balanceada y adecuada para el entrenamiento y evaluación de modelos.


```python

# ============================================================
# 2) Crear splits estratificados: train / val / test
# ============================================================
# Proporciones típicas: 70% train, 15% val, 15% test

test_size = 0.15
val_size  = 0.15

# Primero separamos test
X_temp, X_test, y_temp, y_test, coords_temp, coords_test = train_test_split(
    X, y, coords, test_size=test_size, random_state=42, stratify=y
)

# Luego separamos train y val a partir de temp
# Queremos que val sea el 15% del total → relativo a temp:
val_size_rel = val_size / (1 - test_size)

X_train, X_val, y_train, y_val, coords_train, coords_val = train_test_split(
    X_temp, y_temp, coords_temp, test_size=val_size_rel, random_state=42, stratify=y_temp
)

print("Shapes:")
print("  X_train:", X_train.shape, "y_train:", y_train.shape)
print("  X_val:  ", X_val.shape,   "y_val:  ", y_val.shape)
print("  X_test: ", X_test.shape,  "y_test: ", y_test.shape)

print("\nDistribución por split:")
print("  Train:", Counter(y_train.tolist()))
print("  Val:  ", Counter(y_val.tolist()))
print("  Test: ", Counter(y_test.tolist()))

```


Calcular pesos de clase (para la pérdida)

En el apartado 3 se calculan pesos de clase destinados a compensar posibles desequilibrios en la distribución de clases dentro del conjunto de entrenamiento. Primero se cuentan las ocurrencias de cada clase en y_train y se determina el número total de muestras y el número de clases distintas. A partir de ello se define un esquema de pesos inversamente proporcional a la frecuencia: las clases menos representadas reciben un peso mayor, mientras que las clases más frecuentes reciben un peso menor. Estos pesos se almacenan en un diccionario class_weights indexado por el identificador de clase. La idea es que, al utilizar estos pesos en la función de pérdida durante el entrenamiento de la red neuronal o del modelo de clasificación, se reduzca el sesgo hacia las clases dominantes y se mejore la capacidad del modelo para aprender patrones de las clases minoritarias.

```python
# ============================================================
# 3) Calcular pesos de clase (para usarlos luego en la pérdida)
# ============================================================

class_counts = Counter(y_train.tolist())
num_classes = len(class_counts)
total_train = len(y_train)

# Peso inversamente proporcional a la frecuencia
class_weights = {}
for cls, cnt in class_counts.items():
    # Ejemplo: weight = total / (num_classes * count_cls)
    class_weights[int(cls)] = total_train / (num_classes * cnt)

print("\nPesos de clase sugeridos (para la pérdida):")
for cls, w in class_weights.items():
    print(f"  Clase {cls}: {w:.3f}")


```
Guardar splits a disco

En el apartado 4 se guardan en disco los tres subconjuntos resultantes: entrenamiento, validación y prueba, cada uno en su propio archivo .npz. Para cada split se almacena el tensor de parches X, las etiquetas y, las coordenadas coords y el orden de estaciones season_order, de forma que el modelo pueda ser entrenado o evaluado posteriormente sin necesidad de rehacer la partición. Guardar estos archivos facilita la reproducibilidad de los experimentos, permite compartir los splits con otros investigadores o scripts y asegura que las mismas particiones se utilicen de manera consistente a lo largo de todo el flujo de trabajo, desde el entrenamiento inicial hasta las evaluaciones finales y posibles ajustes de hiperparámetros.

```python

# ============================================================
# 4) Guardar splits a disco
# ============================================================

out_train = os.path.join(DATA_DIR, "patches_train_10x10_mixed.npz")
out_val   = os.path.join(DATA_DIR, "patches_val_10x10_mixed.npz")
out_test  = os.path.join(DATA_DIR, "patches_test_10x10_mixed.npz")

np.savez(out_train, X=X_train, y=y_train, coords=coords_train, season_order=season_order)
np.savez(out_val,   X=X_val,   y=y_val,   coords=coords_val,   season_order=season_order)
np.savez(out_test,  X=X_test,  y=y_test,  coords=coords_test,  season_order=season_order)

print("\nGuardado:")
print("  Train →", out_train)
print("  Val   →", out_val)
print("  Test  →", out_test)

```

### CNN

**CHUNK 1 — Preparación del dataset y definición del modelo**

0) Imports y montaje del Drive

Se instalan y cargan las librerías necesarias, incluyendo TensorFlow y NumPy. Además, se monta Google Drive para poder acceder a los datasets preprocesados (train, val y test).

1) Cargar los splits

Se leen desde disco los archivos .npz que contienen los parches de Sentinel-2 generados previamente. Se cargan las matrices de imágenes X y los vectores de etiquetas y, junto con las coordenadas geográficas de cada parche y el orden estacional. Se imprime la estructura y distribución de clases.

2) Fusionar clase 4 → 2

Dado que “suelo desnudo” (clase 4) y “cultivos” (clase 2) presentan una confusión frecuente, se decide unificarlas bajo la clase 2 para mejorar la estabilidad del entrenamiento. Se actualizan los vectores de etiquetas en los tres splits y se verifica la nueva distribución.

3) Preparación de X para Keras

Se transforma el arreglo de entrada desde formato (N, bandas, H, W) a (N, H, W, bandas), que es el formato estándar channels_last en TensorFlow. Luego se normalizan los valores espectrales al rango 0–1 asumiendo reflectancias aproximadas de 0–10000.

4) Pesos de clase

Se calculan pesos inversamente proporcionales a la frecuencia de cada clase en el conjunto de entrenamiento, lo cual permite compensar desbalances y mejorar el aprendizaje en clases minoritarias.

5) Definición del modelo CNN

Se implementa una CNN secuencial típica para clasificación de imágenes:

Tres bloques Conv2D + MaxPooling

Flatten

Capa densa de 128 neuronas + Dropout

Capa de salida softmax

El modelo se resume mostrando parámetros entrenables.

6) Compilación del modelo

Se usa Adam con LR=1e-3 y la pérdida estándar de clasificación multiclase sparse_categorical_crossentropy.

7) Entrenamiento

El bloque está comentado, pero permite entrenar la CNN utilizando los pesos de clase y validación en cada época.

8) Evaluación

Se prevé ejecutar una evaluación sobre el conjunto de test, obteniendo exactitud y pérdida final.

```python
# ============================================================
# 0) IMPORTS
# ============================================================
!pip install tensorflow

import os
import numpy as np
import tensorflow as tf
from tensorflow.keras import layers, models
from collections import Counter
from google.colab import drive

drive.mount('/content/drive')


# ============================================================
# 1) CARGAR SPLITS
# ============================================================

DATA_DIR = "/content/drive/MyDrive/S2_Parches_AOI"

train_path = os.path.join(DATA_DIR, "patches_train_10x10_mixed.npz")
val_path   = os.path.join(DATA_DIR, "patches_val_10x10_mixed.npz")
test_path  = os.path.join(DATA_DIR, "patches_test_10x10_mixed.npz")

train_data = np.load(train_path, allow_pickle=True)
val_data   = np.load(val_path, allow_pickle=True)
test_data  = np.load(test_path, allow_pickle=True)

X_train, y_train = train_data["X"], train_data["y"]
X_val,   y_val   = val_data["X"],   val_data["y"]
X_test,  y_test  = test_data["X"],  test_data["y"]

season_order = train_data["season_order"]

print("X_train:", X_train.shape)
print("X_val:",   X_val.shape)
print("X_test:",  X_test.shape)
print("Clases originales:", Counter(y_train.tolist()))
print("season_order:", season_order)


# ============================================================
# 2) FUSIONAR CLASE 4 → 2  (suelo desnudo + cultivos)
# ============================================================

y_train = y_train.copy()
y_val   = y_val.copy()
y_test  = y_test.copy()

y_train[y_train == 4] = 2
y_val[y_val == 4] = 2
y_test[y_test == 4] = 2

print("Clases después de fusionar 4→2:")
print(" Train:", Counter(y_train.tolist()))
print(" Val:  ", Counter(y_val.tolist()))
print(" Test: ", Counter(y_test.tolist()))

classes = sorted(np.unique(y_train))
num_classes = len(classes)
print("Clases finales:", classes, " — num_classes:", num_classes)


# ============================================================
# 3) PREPARAR X PARA KERAS (channels_last + normalización)
# ============================================================

def prepare_X(X):
    # Cambiar orden (N, bands, H, W) → (N, H, W, bands)
    X = np.transpose(X, (0, 2, 3, 1)).astype(np.float32)
    # Normalizar suponiendo valores ~ 0–10000
    X = np.clip(X / 10000.0, 0.0, 1.0)
    return X

X_train_keras = prepare_X(X_train)
X_val_keras   = prepare_X(X_val)
X_test_keras  = prepare_X(X_test)

input_shape = X_train_keras.shape[1:]
print("input_shape =", input_shape)


# ============================================================
# 4) PESOS DE CLASE (compensación de desbalance)
# ============================================================

train_counts = Counter(y_train.tolist())
total_train = len(y_train)

class_weights = {int(cls): total_train / (num_classes * cnt)
                 for cls, cnt in train_counts.items()}

print("Pesos de clase:")
for cls, w in class_weights.items():
    print(f"  Clase {cls}: {w:.3f}")


# ============================================================
# 5) DEFINICIÓN DEL MODELO CNN (similar al notebook anterior)
# ============================================================

inputs = layers.Input(shape=input_shape)

x = layers.Conv2D(32, (3, 3), activation="relu", padding="same")(inputs)
x = layers.MaxPooling2D()(x)

x = layers.Conv2D(64, (3, 3), activation="relu", padding="same")(x)
x = layers.MaxPooling2D()(x)

x = layers.Conv2D(128, (3, 3), activation="relu", padding="same")(x)
x = layers.MaxPooling2D()(x)

x = layers.Flatten()(x)
x = layers.Dense(128, activation="relu")(x)
x = layers.Dropout(0.3)(x)

outputs = layers.Dense(num_classes, activation="softmax")(x)

model = models.Model(inputs=inputs, outputs=outputs)
model.summary()


# ============================================================
# 6) COMPILAR EL MODELO
# ============================================================

model.compile(
    optimizer=tf.keras.optimizers.Adam(learning_rate=1e-3),
    loss="sparse_categorical_crossentropy",
    metrics=["accuracy"]
)


# ============================================================
# 7) ENTRENAMIENTO (Opcional — descomentá cuando quieras entrenar)
# ============================================================

# history = model.fit(
#     X_train_keras, y_train,
#     validation_data=(X_val_keras, y_val),
#     epochs=30,
#     batch_size=64,
#     class_weight=class_weights,
#     verbose=1
# )

# ============================================================
# 8) EVALUACIÓN EN TEST (cuando el modelo esté entrenado)
# ============================================================

# test_loss, test_acc = model.evaluate(X_test_keras, y_test, verbose=0)
# print("Test Loss:", test_loss)
# print("Test Accuracy:", test_acc)

```

Limpieza, redefinición del modelo y evaluación avanzada
1) Limpieza de datos

Se revisan y corrigen posibles valores NaN o infinitos en las imágenes, y se aseguran tipos consistentes en las etiquetas. Esto evita errores silenciosos durante el entrenamiento.

2) Redefinir el modelo

Se crea una función build_model() que reconstruye la misma CNN del chunk anterior, pero ahora totalmente modular y con un learning rate más pequeño (1e-4), lo que promueve un entrenamiento más estable.

3) Entrenamiento

Se entrena la red durante 30 épocas utilizando los conjuntos limpios y los pesos de clase. Se registra el historial completo (loss y accuracy).

4) Evaluación en test

Se evalúa rigurosamente el rendimiento final del modelo:

Exactitud

Pérdida

Distribución de predicciones

Comparación con etiquetas verdaderas

5) Curvas de entrenamiento

Se grafican las curvas de evolución de pérdida y precisión tanto en entrenamiento como en validación. Esto permite diagnosticar sobreajuste, subajuste y comportamiento del optimizador.

6) Matriz de confusión y reporte

Se calcula la matriz de confusión y se genera el classification report con métricas por clase (precision, recall, F1-score). Esto ofrece una evaluación profunda del comportamiento de la red para cada categoría.

7) Guardado del modelo

Finalmente, el modelo entrenado se guarda en formato .keras, lo que permite reusarlo sin necesidad de repetir el entrenamiento.

```python
import numpy as np
import matplotlib.pyplot as plt
from collections import Counter
from sklearn.metrics import confusion_matrix, classification_report
import tensorflow as tf
from tensorflow.keras import layers, models
import os

# ============================================================
# 1) LIMPIAR DATOS (X e y)
# ============================================================

# Aseguramos que no haya NaN / inf en los inputs
def clean_X(X):
    X = np.nan_to_num(X, nan=0.0, posinf=1.0, neginf=0.0)
    return X.astype(np.float32)

X_train_keras = clean_X(X_train_keras)
X_val_keras   = clean_X(X_val_keras)
X_test_keras  = clean_X(X_test_keras)

# Aseguramos tipo entero y valores válidos en y
y_train = y_train.astype("int32")
y_val   = y_val.astype("int32")
y_test  = y_test.astype("int32")

print("X_train_keras stats:",
      "min=", np.min(X_train_keras),
      "max=", np.max(X_train_keras),
      "has_nan=", np.isnan(X_train_keras).any(),
      "has_inf=", np.isinf(X_train_keras).any())
print("Clases en y_train:", Counter(y_train.tolist()))

input_shape = X_train_keras.shape[1:]
num_classes = len(np.unique(y_train))
print("input_shape:", input_shape, "  num_classes:", num_classes)


# ============================================================
# 2) REDEFINIR EL MODELO DESDE CERO
# ============================================================

def build_model(input_shape, num_classes):
    inputs = layers.Input(shape=input_shape)

    x = layers.Conv2D(32, (3, 3), activation="relu", padding="same")(inputs)
    x = layers.MaxPooling2D()(x)

    x = layers.Conv2D(64, (3, 3), activation="relu", padding="same")(x)
    x = layers.MaxPooling2D()(x)

    x = layers.Conv2D(128, (3, 3), activation="relu", padding="same")(x)
    x = layers.MaxPooling2D()(x)

    x = layers.Flatten()(x)
    x = layers.Dense(128, activation="relu")(x)
    x = layers.Dropout(0.3)(x)

    outputs = layers.Dense(num_classes, activation="softmax")(x)

    model = models.Model(inputs=inputs, outputs=outputs)
    return model

model = build_model(input_shape, num_classes)
model.summary()

# LR un poco más chico por las dudas
optimizer = tf.keras.optimizers.Adam(learning_rate=1e-4)

model.compile(
    optimizer=optimizer,
    loss="sparse_categorical_crossentropy",
    metrics=["accuracy"]
)


# ============================================================
# 3) ENTRENAMIENTO
# ============================================================

EPOCHS = 30
BATCH_SIZE = 64

history = model.fit(
    X_train_keras, y_train,
    validation_data=(X_val_keras, y_val),
    epochs=EPOCHS,
    batch_size=BATCH_SIZE,
    class_weight=class_weights,   # si querés probar sin pesos: poné None
    verbose=1
)


# ============================================================
# 4) EVALUACIÓN EN TEST
# ============================================================

test_loss, test_acc = model.evaluate(X_test_keras, y_test, verbose=0)
print("\n======================")
print("Resultados en TEST:")
print("  Test Loss:", test_loss)
print("  Test Accuracy:", test_acc)
print("======================\n")

# Predicciones en test
y_prob = model.predict(X_test_keras, verbose=0)
y_pred = np.argmax(y_prob, axis=1)

print("Distribución real (y_test):", Counter(y_test.tolist()))
print("Distribución predicha:", Counter(y_pred.tolist()))


# ============================================================
# 5) CURVAS DE ENTRENAMIENTO
# ============================================================

def plot_history(history):
    plt.figure(figsize=(10,4))
    # Loss
    plt.subplot(1,2,1)
    plt.plot(history.history["loss"], label="train_loss")
    plt.plot(history.history["val_loss"], label="val_loss")
    plt.xlabel("Época")
    plt.ylabel("Pérdida")
    plt.title("Evolución de Loss")
    plt.legend()
    plt.grid(True)

    # Accuracy
    plt.subplot(1,2,2)
    plt.plot(history.history["accuracy"], label="train_acc")
    plt.plot(history.history["val_accuracy"], label="val_acc")
    plt.xlabel("Época")
    plt.ylabel("Precisión")
    plt.title("Evolución de Accuracy")
    plt.legend()
    plt.grid(True)

    plt.tight_layout()
    plt.show()

plot_history(history)


# ============================================================
# 6) MATRIZ DE CONFUSIÓN Y REPORTE
# ============================================================

cm = confusion_matrix(y_test, y_pred, labels=[0,1,2,3])
print("Matriz de confusión (clases 0,1,2,3):\n", cm)

plt.figure(figsize=(6,5))
plt.imshow(cm, cmap="Blues")
plt.colorbar()
plt.xticks(range(4), [0,1,2,3])
plt.yticks(range(4), [0,1,2,3])
plt.xlabel("Predicho")
plt.ylabel("Real")

# anotar valores
for i in range(4):
    for j in range(4):
        plt.text(j, i, cm[i,j], ha="center", va="center", color="black")

plt.title("Matriz de confusión")
plt.tight_layout()
plt.show()

print("\nReporte de clasificación:")
print(classification_report(y_test, y_pred, digits=3))


# ============================================================
# 7) GUARDAR MODELO (formato nuevo .keras)
# ============================================================

model_path = os.path.join(DATA_DIR, "cnn_landcover_10x10_mixed.keras")
model.save(model_path)
print("\nModelo guardado en:", model_path)

```

### Clasificación

Este bloque de código implementa la clasificación completa de un mosaico Sentinel-2 usando la CNN previamente entrenada, pero de forma eficiente en memoria. En lugar de cargar todo en un solo tensor gigante, recorre el mosaico por parches, los clasifica por lotes (batches) y luego reconstruye un mapa continuo de clases, que después se visualiza y se exporta como GeoTIFF georreferenciado.

1) Montar Drive y definir rutas

En este primer apartado se realiza la configuración del entorno de trabajo. Se montan las unidades de Google Drive en Colab (drive.mount) y se define la carpeta base (DATA_DIR) donde se encuentran los mosaicos Sentinel-2 estacionales y el modelo de la CNN guardado. Luego se arma un diccionario s2_files con las rutas a los mosaicos de verano, otoño, invierno y primavera, y la ruta al archivo del modelo cnn_landcover_10x10_mixed.keras. El objetivo de este bloque es dejar claramente especificado de dónde se van a leer los datos de entrada y dónde está el modelo que realizará la predicción.

2) Cargar mosaicos estacionales (BANDAS, H, W)

En este apartado se leen desde disco los mosaicos estacionales usando rasterio. Para cada estación, se carga el raster como un arreglo con forma (bands, H, W) y se guarda en el diccionario s2_mosaics. El primer mosaico sirve como referencia espacial: se extraen su tamaño (H, W) y su perfil (profile, CRS, transform). Para las estaciones siguientes, se verifica que tengan el mismo tamaño espacial y la misma transformación (misma grilla y georreferenciación). Además, se calcula cuántas bandas tiene cada estación (bands_per_season) y, a partir del número de estaciones, el total de bandas (bands_total) una vez concatenadas. El objetivo de este bloque es garantizar que todos los mosaicos estén perfectamente alineados y compatibles para ser usados juntos como entrada de la CNN.

3) Parámetros de parches y funciones auxiliares

Aquí se definen los parámetros de extracción de parches y las funciones auxiliares para preparar los datos antes de enviarlos al modelo:

* PATCH_SIZE = 10 indica que se trabajará con parches de 10×10 píxeles.
* STRIDE = 1 significa que se desplaza la ventana de 1 píxel en cada dirección, produciendo una clasificación casi píxel a píxel pero suavizada por el solapamiento de parches.

La función prepare_X_for_cnn toma un lote de parches con forma (N, bands, H, W), reorganiza los ejes a (N, H, W, bands) (formato channels_last requerido por Keras/TensorFlow), normaliza los valores a un rango [0, 1] asumiendo reflectancias ~0–10000 y reemplaza NaN e infinitos por valores seguros. La función get_patch_stack(r, c) construye un parche multitemporal: para una coordenada fila-columna concreta, extrae el parche 10×10 de cada estación, y luego concatena las bandas de todas las estaciones en una sola pila ((bands_total, ps, ps)). El objetivo de este bloque es definir la unidad básica de entrada al modelo (parches multibanda y multitemporales) y asegurar que estén correctamente normalizados y listos para la CNN.

4) Cargar modelo

En este tramo se carga desde disco el modelo de CNN ya entrenado mediante tf.keras.models.load_model(model_path). Se imprime la ruta desde donde se cargó y se recupera el número de clases de salida (n_classes) a partir de la forma del tensor de salida del modelo. El objetivo de este apartado es disponer en memoria del clasificador entrenado, exactamente en el mismo estado en que fue guardado durante la etapa de entrenamiento, para poder aplicarlo ahora al mosaico completo.

5) Tensor de votos y función process_batch

Este apartado define la estructura de acumulación de resultados y la lógica de predicción por lotes:

* Se crea un tensor votes de forma (H_map, W_map, n_classes) inicializado en cero. En cada píxel se acumularán las probabilidades de pertenecer a cada clase, provenientes de todos los parches que cubren ese píxel.
* Se fijan parámetros como BATCH_SIZE y se inicializan listas para guardar temporalmente las coordenadas y los parches (coords_batch, patches_batch).
* La función process_batch toma las listas acumuladas, las convierte en un arreglo (X), las prepara para la CNN con prepare_X_for_cnn, ejecuta la predicción (model.predict) y luego, para cada parche, suma las probabilidades de salida del modelo en el área correspondiente del tensor votes.

El objetivo de este bloque es implementar una estrategia que permita clasificar una gran cantidad de parches sin desbordar la memoria, y al mismo tiempo aprovechar el solapamiento de parches para obtener una clasificación por píxel basada en el promedio (suma) de probabilidades.

6) Recorrer TODOS los parches por batches

En este apartado se implementa el barrido sistemático del mosaico. Se recorre la imagen con dos bucles anidados sobre filas (r) y columnas (c), usando el STRIDE definido. Para cada posición, se obtiene el parche multitemporal llamando a get_patch_stack(r, c) y se añade a las listas del batch. Cuando el número de parches acumulados alcanza BATCH_SIZE, se llama a process_batch para que la CNN los clasifique y sus probabilidades se agreguen en votes. Se muestra además un mensaje de progreso por fila para monitorear el avance. Al final, si quedó un batch incompleto, también se procesa. El objetivo de este bloque es recorrer exhaustivamente el mosaico y clasificarlo de forma incremental, manteniendo el consumo de memoria bajo control gracias al procesamiento por lotes.

7) Convertir votos en mapa de clases

Una vez completada la acumulación de probabilidades, en este apartado se construye el mapa final de clases:

* Se crea cnn_map inicializado con -1 (valor de nodata).
* Se define una máscara valid_mask para identificar píxeles donde se acumularon votos (es decir, fueron cubiertos por al menos un parche).
* Para esos píxeles válidos, se toma la clase con probabilidad máxima a lo largo del eje de clases (np.argmax(votes[valid_mask], axis=1)) y se asigna ese índice a cnn_map.

El objetivo de este bloque es convertir el tensor de probabilidades acumuladas en una clasificación categórica por píxel, respetando el esquema de clases de la CNN y marcando como nodata aquellos píxeles sin información.

8) Visualizar clasificación categórica

En este apartado se genera una visualización legible de la clasificación:

* Se define un diccionario class_names que asigna un nombre interpretativo a cada clase (Agua, Urbano, Cultivos+Suelo desnudo, Bosque).
* Se construye una paleta de colores (ListedColormap) con un color específico para cada clase y uno adicional para nodata.
* Se crea una copia cnn_map_vis en la que se sustituyen los -1 por el índice 4 (reservado para nodata en la paleta), y luego se representa con plt.imshow.
* Se añade una leyenda con parches de color (Patch) que explican el significado de cada código de clase, y se ajusta el diseño para que la figura sea clara y presentable.

El objetivo de este bloque es producir un mapa temático visualmente interpretable, que permita evaluar rápidamente la distribución espacial de las clases clasificadas por la CNN.


```python
# ============================================================
# CLASIFICAR UN MOSAICO SENTINEL-2 COMPLETO CON LA CNN (MEMORIA EFICIENTE)
# ============================================================

import os
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
import rasterio
import tensorflow as tf
from google.colab import drive

# 1) Montar Drive y definir rutas
drive.mount('/content/drive')

DATA_DIR = "/content/drive/MyDrive/S2_Parches_AOI"

s2_files = {
    "verano":    os.path.join(DATA_DIR, "S2_verano_2024.tif"),
    "otono":     os.path.join(DATA_DIR, "S2_otono_2024.tif"),
    "invierno":  os.path.join(DATA_DIR, "S2_invierno_2024.tif"),
    "primavera": os.path.join(DATA_DIR, "S2_primavera_2024.tif"),
}

model_path = os.path.join(DATA_DIR, "cnn_landcover_10x10_mixed.keras")

# 2) Cargar mosaicos estacionales (BANDAS, H, W)
s2_mosaics = {}
ref_profile = None

for season, path in s2_files.items():
    with rasterio.open(path) as src:
        img = src.read()  # (bands, H, W)
        if ref_profile is None:
            ref_profile = src.profile
            H, W = img.shape[1], img.shape[2]
            print(f"Referencia: {season} → shape: {img.shape}, crs: {src.crs}")
        else:
            if img.shape[1] != H or img.shape[2] != W:
                raise ValueError(f"El mosaico {season} no coincide en tamaño con el de referencia")
            if src.transform != ref_profile['transform']:
                raise ValueError(f"El mosaico {season} no coincide en transform con el de referencia")
        s2_mosaics[season] = img

bands_per_season = s2_mosaics["verano"].shape[0]
print("H, W:", H, W, "  bandas por estación:", bands_per_season)

season_order = ["verano", "otono", "invierno", "primavera"]
n_seasons = len(season_order)
bands_total = bands_per_season * n_seasons
print("Total de bandas (todas las estaciones):", bands_total)

# 3) Parámetros de parches
PATCH_SIZE = 10
STRIDE = 1  # clasificación casi pixel a pixel (muy suave)

# Función de normalización (igual que antes, pero para batches)
def prepare_X_for_cnn(X):
    # (N, bands, H, W) → (N, H, W, bands)
    X = np.transpose(X, (0, 2, 3, 1)).astype(np.float32)
    X = np.clip(X / 10000.0, 0.0, 1.0)
    X = np.nan_to_num(X, nan=0.0, posinf=1.0, neginf=0.0)
    return X

# Función para armar un parche (todas las estaciones concatenadas)
def get_patch_stack(r, c):
    patch_seasons = []
    for season in season_order:
        mosaic = s2_mosaics[season]  # (bands, H, W)
        p = mosaic[:, r:r+PATCH_SIZE, c:c+PATCH_SIZE]  # (bands, ps, ps)
        patch_seasons.append(p)
    patch_stack = np.concatenate(patch_seasons, axis=0)  # (bands_total, ps, ps)
    return patch_stack

# 4) Cargar modelo
model = tf.keras.models.load_model(model_path)
print("Modelo cargado desde:", model_path)

n_classes = model.output_shape[-1]
print("Número de clases del modelo:", n_classes)

# 5) Tensor de votos (acumulamos probabilidades por píxel)
H_map = H
W_map = W
votes = np.zeros((H_map, W_map, n_classes), dtype=np.float32)

# Tamaño de batch de parches que mandamos al modelo
BATCH_SIZE = 512  # podés bajar a 256 si seguís justo de RAM

coords_batch = []
patches_batch = []

total_patches = 0

def process_batch(coords_batch, patches_batch):
    """Clasifica un batch de parches y acumula las probabilidades en 'votes'."""
    if len(coords_batch) == 0:
        return

    X = np.stack(patches_batch, axis=0)  # (N, bands_total, ps, ps)
    X_keras = prepare_X_for_cnn(X)       # (N, ps, ps, bands_total)

    y_prob = model.predict(X_keras, batch_size=128, verbose=0)  # (N, n_classes)

    # Acumular probabilidades sobre cada píxel del parche
    for (r, c), probs in zip(coords_batch, y_prob):
        votes[r:r+PATCH_SIZE, c:c+PATCH_SIZE, :] += probs

# 6) Recorrer TODOS los parches pero por batches (sin reventar la RAM)
print("Clasificando parches por batches, esto puede demorar un poco...")

for r in range(0, H - PATCH_SIZE + 1, STRIDE):
    for c in range(0, W - PATCH_SIZE + 1, STRIDE):
        patch_stack = get_patch_stack(r, c)
        coords_batch.append((r, c))
        patches_batch.append(patch_stack)
        total_patches += 1

        # Cuando juntamos BATCH_SIZE parches, los procesamos
        if len(coords_batch) == BATCH_SIZE:
            process_batch(coords_batch, patches_batch)
            coords_batch = []
            patches_batch = []

    # info de progreso por filas
    print(f"Fila {r} / {H - PATCH_SIZE}", end="\r")

# Procesar lo que quedó en el último batch (si no era múltiplo exacto)
process_batch(coords_batch, patches_batch)

print(f"\nTotal de parches clasificados: {total_patches}")

# 7) Convertir votos en mapa de clases
cnn_map = np.full((H_map, W_map), -1, dtype=np.int16)  # nodata = -1
valid_mask = votes.sum(axis=2) > 0
cnn_map[valid_mask] = np.argmax(votes[valid_mask], axis=1)

# 8) Visualizar clasificación categórica
class_names = {
    0: "Agua",
    1: "Urbano",
    2: "Cultivos+Suelo desnudo",
    3: "Bosque"
}

colors = [
    "#1f78b4",  # 0 agua
    "#b2b2b2",  # 1 urbano
    "#33a02c",  # 2 cultivos/suelo desnudo
    "#006400",  # 3 bosque
    "#000000"   # nodata
]
cmap = ListedColormap(colors)

cnn_map_vis = cnn_map.copy()
cnn_map_vis[cnn_map_vis == -1] = 4  # nodata → índice 4

plt.figure(figsize=(6, 5))
plt.imshow(cnn_map_vis, cmap=cmap, vmin=0, vmax=4)
plt.title("Clasificación CNN (parches 10×10, stride=1, suavizada, memoria eficiente)")
plt.axis("off")

from matplotlib.patches import Patch
legend_elements = [
    Patch(facecolor=colors[0], label=f"0: {class_names[0]}"),
    Patch(facecolor=colors[1], label=f"1: {class_names[1]}"),
    Patch(facecolor=colors[2], label=f"2: {class_names[2]}"),
    Patch(facecolor=colors[3], label=f"3: {class_names[3]}"),
    Patch(facecolor=colors[4], label="Nodata"),
]
plt.legend(handles=legend_elements,
           bbox_to_anchor=(1.05, 0.5),
           loc="center left",
           borderaxespad=0.)
plt.tight_layout()
plt.show()

# 9) Exportar a GeoTIFF georreferenciado
out_path = os.path.join(DATA_DIR, "CNN_classification_10x10_stride1_suave.tif")

profile = ref_profile.copy()
profile.update({
    "count": 1,
    "dtype": "int16",
    "nodata": -1
})

with rasterio.open(out_path, "w", **profile) as dst:
    dst.write(cnn_map, 1)

print("Clasificación CNN guardada como GeoTIFF en:", out_path)

```


9) Exportar a GeoTIFF georreferenciado

El último apartado se encarga de guardar el resultado de la clasificación en un archivo raster georreferenciado:

* Se define la ruta de salida out_path para el GeoTIFF.
* Se copia el profile de referencia del mosaico original y se actualizan los campos relevantes: número de bandas (count = 1), tipo de dato (int16) y valor nodata (-1).
* Se abre un nuevo archivo con rasterio.open en modo escritura y se escribe cnn_map como banda 1.

El objetivo de este bloque es generar un producto final listo para ser usado en SIG (QGIS, ArcGIS, etc.), conservando la georreferenciación original de los mosaicos Sentinel-2 y permitiendo su integración con otras capas geoespaciales.

```python
from google.colab import files
files.download("/content/drive/MyDrive/S2_Parches_AOI/CNN_classification_10x10_stride1_suave.tif")

```