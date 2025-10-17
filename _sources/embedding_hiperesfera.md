# Apéndice C: Cómo graficar un embedding en una hiperesfera normalizada

## 1) Descripción general

Un **embedding** es un vector en un espacio latente de dimensión \(d\), por ejemplo, \(x \in \mathbb{R}^d\). Para compararlo geométricamente por **orientación** (y no por magnitud), se **normaliza L2** y se proyecta sobre la **hiperesfera unitaria**:

$\hat{x} = \frac{x}{\|x\|}$

donde $\|x\|$ es la norma euclidiana (longitud) del vector. Tras la normalización, $\|\hat{x}\| = 1$, por lo que $\hat{x}$ vive sobre la **superficie** de la hiperesfera de radio 1 en dimensión $d$.

### Flujo típico para visualizar embeddings
1. **Normalizá L2** cada vector: $\hat{x} = x/\|x\|$.
2. **Reducí dimensionalidad** si $d > 3$ (PCA/UMAP/t-SNE) a 2D/3D para poder graficar.
3. **Re-normalizá en el espacio reducido** para ubicar los puntos exactamente sobre la esfera/circunferencia unitaria: $\tilde{x} = x_{\text{proj}}/\|x_{\text{proj}}\|$.
4. **Graficá** los puntos normalizados junto con la esfera/circunferencia unitaria.
5. (Opcional) Medí **similitud coseno** entre embeddings normalizados: $\text{sim}(u,v) = \hat{u} \cdot \hat{v} = \cos(\theta)$.

> Idea central: la comparación semántica entre embeddings ocurre por **ángulos** (direcciones) sobre la hiperesfera, no por magnitudes.

---

## 2) Ejemplo con un embedding 3D (paso a paso)

### 2.1 Vector original
Supongamos un embedding 3D: $x = [1.4,\ 0.5,\ -0.8]$.

### 2.2 Norma L2
$\|x\| = \sqrt{1.4^2 + 0.5^2 + (-0.8)^2} = \sqrt{1.96 + 0.25 + 0.64} = \sqrt{2.85} \approx 1.688$

### 2.3 Normalización a la esfera unitaria
$\hat{x} = x/\|x\| \approx [1.4/1.688,\ 0.5/1.688,\ -0.8/1.688] \approx [0.829,\ 0.296,\ -0.474]$

Ahora $\|\hat{x}\| = 1$. Geométricamente, $\hat{x}$ es un punto sobre la **esfera unitaria** en 3D.

### 2.4 Cómo graficarlo (idea práctica)
- Dibujá una **malla de esfera unitaria** (radio 1).  
- Graficá el **punto normalizado** $\hat{x}$ como un marcador sobre esa esfera.  
- Si tuvieras varios embeddings, repetís el proceso para cada uno y comparás las **distancias angulares** (o la **similitud coseno**).

### 2.5 Pseudocódigo (Python)
```python
import numpy as np

# Vector 3D
x = np.array([1.4, 0.5, -0.8], dtype=float)

# 1) Normalización L2
x_hat = x / np.linalg.norm(x)   # vive sobre la esfera unitaria

# 2) (Opcional) Para muchos vectores: stack, normalizar fila a fila
# X = np.stack([...])  # (n, 3)
# X_hat = X / np.linalg.norm(X, axis=1, keepdims=True)

# 3) Graficar: usar matplotlib 3D, dibujar malla de esfera y scatter de x_hat
```

> Nota: si tu embedding fuera de 64D (como en Satellite Embeddings V1), primero normalizás en 64D, luego reducís a 3D con PCA/UMAP/t-SNE, y **volvés a normalizar** ese 3D antes de graficar para ubicar los puntos exactamente sobre la esfera unitaria.

---

## 3) Resumen

- **Normalización L2**: $\hat{x} = x/\|x\|$ coloca el embedding sobre la **hiperesfera unitaria**.  
- **Visualización**: si $d \le 3$, graficás directamente; si $d > 3$, **proyectás** a 2D/3D y **re-normalizás**.  
- **Comparación**: la **similitud coseno** $\hat{u} \cdot \hat{v}$ (o el **ángulo** $\arccos(\hat{u}\cdot\hat{v})$) mide proximidad semántica en la hiperesfera.
