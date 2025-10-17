# Apéndice C: ¿Cómo graficar un embedding en una hiperesfera normalizada?

## 1) Descripción general (definición y marco geométrico)

Una **hiperesfera unitaria** en dimensión $n$ es el conjunto de todos los vectores con **norma euclidiana 1**:

$S^{n-1} = \{\, x \in \mathbb{R}^n \mid \|x\| = 1 \,\}$

Por ejemplo:

| Dimensión | Nombre geométrico | Ecuación | Representación |
|---|---|---|---|
| 1D | Dos puntos ($-1$, $+1$) | $x^2 = 1$ | 🔹🔹 |
| 2D | Circunferencia unitaria | $x^2 + y^2 = 1$ | ⭕ |
| 3D | Esfera unitaria | $x^2 + y^2 + z^2 = 1$ | 🟢 |
| $d$D | Hiperesfera unitaria | $x_1^2 + \cdots + x_d^2 = 1$ | (no visualizable) |

En el contexto de **embeddings**, un dato se representa por un vector $x \in \mathbb{R}^d$. Para comparar por **dirección** (semántica) y no por magnitud, se **normaliza L2** (norma L2 o norma euclideana) y se proyecta sobre la hiperesfera unitaria:

$\hat{x} = \dfrac{x}{\|x\|}$

Después de normalizar, se cumple $\|\hat{x}\| = 1$. Así, la similitud entre embeddings se evalúa por su **ángulo** sobre la hiperesfera, típicamente vía **similitud coseno**:

$\operatorname{sim}(u,v) = \hat{u} \cdot \hat{v} = \cos(\theta)$

---

## 2) Flujo práctico para visualizar embeddings

1. **Normalización L2** por vector: $\hat{x} = x/\|x\|$.
2. **Reducción de dimensionalidad** si $d > 3$ (PCA/UMAP/t-SNE) para obtener $x_{\text{proj}} \in \mathbb{R}^3$ o $\mathbb{R}^2$.
3. **Re-normalización en el espacio reducido** para que los puntos queden sobre la esfera/circunferencia unitaria: $\tilde{x} = x_{\text{proj}}/\|x_{\text{proj}}\|$.
4. **Gráfica**: dibujar la esfera/circunferencia unitaria y superponer los puntos $\tilde{x}$.
5. (Opcional) **Medir proximidad** entre embeddings con $\operatorname{sim}(u,v) = \hat{u} \cdot \hat{v}$ o el **ángulo** $\arccos(\hat{u}\cdot\hat{v})$.


| Método | Nombre completo | Tipo de proyección | Qué preserva |
|---------|------------------|--------------------|--------------|
| **PCA** | *Principal Component Analysis* | Lineal | La varianza global |
| **UMAP** | *Uniform Manifold Approximation and Projection* | No lineal | La estructura local y global |
| **t-SNE** | *t-Distributed Stochastic Neighbor Embedding* | No lineal | Las vecindades locales (clústeres) |

````{admonition} Conclusiones de la representación de un embedding $\mathbb{R}^{n}$
:class: tip

En términos prácticos, un *embedding* en $\mathbb{R}^{64}$ no puede visualizarse directamente.  
Para representarlo gráficamente, se aplica una **reducción de dimensionalidad**, como el **Análisis de Componentes Principales (PCA)**, que transforma los vectores de $\mathbb{R}^{64}$ en un nuevo espacio de $\mathbb{R}^3$ conservando la mayor varianza posible.  

De este modo, se muestran solo tres componentes —las tres primeras componentes principales— que capturan la **estructura dominante** del conjunto de embeddings.  
Así, cada punto del gráfico tridimensional es una proyección aproximada de su posición original en el espacio de 64 dimensiones.

````

---

## 3) Ejemplo numérico con un embedding 3D

**Vector original**: $x = [1.4,\ 0.5,\ -0.8]$.

**Norma L2**: $\|x\| = \sqrt{1.4^2 + 0.5^2 + (-0.8)^2} = \sqrt{1.96 + 0.25 + 0.64} = \sqrt{2.85} \approx 1.688$.

**Normalización**: $\hat{x} = x/\|x\| \approx [1.4/1.688,\ 0.5/1.688,\ -0.8/1.688] \approx [0.829,\ 0.296,\ -0.474]$.

Cálculo de la norma del vector normalizado:
$|\hat{x}| = \sqrt{0.829^2 + 0.296^2 + (-0.474)^2} = \sqrt{0.687 + 0.088 + 0.225} = \sqrt{1.000} = 1.000$, por lo que $\hat{x}$ está sobre la **esfera unitaria** en $\mathbb{R}^3$.



**Interpretación**: si tuviéramos dos embeddings normalizados $\hat{u}$ y $\hat{v}$, su similitud coseno $\hat{u}\cdot\hat{v}$ mide cuán **alineados** están (qué tan similares son), independientemente de la magnitud original de $u$ y $v$.

### Coordenadas del punto en la hiperesfera unitaria

Luego, el punto en el espacio tridimensional es: $P = (x, y, z) = (0.829,\ 0.296,\ -0.474)$

Esto significa geométricamente que:

Desde el **centro del sistema de coordenadas** $(0, 0, 0)$ trazamos una flecha que apunta hacia la dirección $(0.829, 0.296, -0.474)$.  

Esa flecha tiene **longitud 1**, por lo tanto su extremo **toca exactamente la superficie de la esfera unitaria**.  

En otras palabras, el vector $\hat{x}$ indica **dirección**, mientras que su norma unitaria ($\|\hat{x}\| = 1$) garantiza que el punto se encuentra **sobre la superficie** y no dentro del volumen de la hiperesfera.

````{admonition} ¿La norma es siempre 1?
:class: tip

La norma de todos los vectores normalizados es igual a 1.

Eso significa que todos los puntos normalizados se ubican sobre la superficie de la hiperesfera unitaria, y no dentro ni fuera.

````
---

## 4) Resumen

- Un embedding $x$ se **proyecta** a la hiperesfera unitaria con $\hat{x} = x/\|x\|$.
- Para visualizar embeddings de alta dimensión ($d>3$), primero se **reduce** (PCA/UMAP/t-SNE) y luego se **re-normaliza**.
- La **similitud coseno** $\hat{u}\cdot\hat{v}$ (o el **ángulo** $\arccos(\hat{u}\cdot\hat{v})$) es la métrica natural sobre la hiperesfera.
