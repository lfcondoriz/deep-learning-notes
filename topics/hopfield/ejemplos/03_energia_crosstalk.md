1. Sincrono: Todas las neuronas se actualizan al mismo tiempo, usando el estado antiguo.
2. Asincrono: Se actualiza una neurona a la vez, elegida al azar.


**regla de Hopfield**:

$$S_i := \text{sgn}\Big( \sum_j w_{ij} S_j \Big)$$

# Comparación **síncrono** vs **asíncrono**.

## 🔹 1️⃣ Ejemplo de red

Supongamos una red de 3 neuronas con pesos:

$$W = \begin{bmatrix}
0 & 1 & -1 \\
1 & 0 & 1 \\
-1 & 1 & 0
\end{bmatrix}$$

Y estado inicial:

$$\mathbf{S}^{(0)} = \begin{bmatrix} +1 \\ -1 \\ +1 \end{bmatrix}$$



## 🔹 2️⃣ Actualización SÍNCRONA

**Definición:**
Todas las neuronas se actualizan al mismo tiempo usando el estado actual:

$$\mathbf{S}^{(t+1)} = \text{sgn}\Big( W \mathbf{S}^{(t)} \Big)$$

---

### Paso 0 → estado inicial:

$$\mathbf{S}^{(0)} = \begin{bmatrix} +1 \\ -1 \\ +1 \end{bmatrix}$$

---

### Paso 1 → cálculo de $W \mathbf{S}^{(0)}$

$$\begin{align*}
h_1 &= 0\cdot 1 + 1\cdot(-1) + (-1)\cdot 1 = -2 \\
h_2 &= 1\cdot 1 + 0\cdot(-1) + 1\cdot 1 = 2 \\
h_3 &= (-1)\cdot 1 + 1\cdot(-1) + 0\cdot 1 = -2
\end{align*}$$

$$\mathbf{h} = \begin{bmatrix}-2 \\ 2 \\ -2\end{bmatrix}$$

---

### Paso 1 → actualización síncrona:

$$\mathbf{S}^{(1)} = \text{sgn}(\mathbf{h}) = \begin{bmatrix}-1 \\ +1 \\ -1\end{bmatrix}$$

> Observa que todas las neuronas cambiaron al mismo tiempo.

---

### Paso 2 → cálculo de $W \mathbf{S}^{(1)}$

$$\begin{align*}
h_1 &= 0\cdot(-1) + 1\cdot 1 + (-1)\cdot(-1) = 2 \\
h_2 &= 1\cdot(-1) + 0\cdot 1 + 1\cdot(-1) = -2 \\
h_3 &= (-1)\cdot(-1) + 1\cdot 1 + 0\cdot(-1) = 2
\end{align*}$$

$$\mathbf{h} = \begin{bmatrix}2 \\ -2 \\ 2\end{bmatrix}$$

$$\mathbf{S}^{(2)} = \text{sgn}(\mathbf{h}) = \begin{bmatrix}+1 \\ -1 \\ +1\end{bmatrix} = \mathbf{S}^{(0)}$$

✅ Observación: **oscila entre dos estados** → ejemplo típico de síncrono que no converge.

---

## 🔹 3️⃣ Actualización ASÍNCRONA

**Definición:**
Actualizamos **una neurona a la vez**, usando el estado más reciente:

$$S_i^{(t+1)} := \text{sgn}\Big( \sum_j w_{ij} S_j^{(\text{actual})} \Big)$$

---

### Paso a paso (elige neurona 1 primero):

* Estado inicial: $\mathbf{S} = [1, -1, 1]$
* Actualiza $S_1$:

$$h_1 = 0\cdot 1 + 1\cdot(-1) + (-1)\cdot 1 = -2$$

$$S_1 = \text{sgn}(-2) = -1$$

* Nuevo estado parcial: $[-1, -1, 1]$

---

### Siguiente neurona ($S_2$):

$$h_2 = 1\cdot(-1) + 0\cdot(-1) + 1\cdot 1 = 0$$

$$S_2 = \text{sgn}(0) = 1 \quad (\text{convención})$$

* Estado parcial: $[-1, 1, 1]$

---

### Siguiente neurona ($S_3$):

$$h_3 = (-1)\cdot(-1) + 1\cdot 1 + 0\cdot 1 = 2$$

$$S_3 = \text{sgn}(2) = 1$$

* Estado actualizado: $[-1, 1, 1]$

> Observa que la red **no oscila**, cada actualización usa el estado más reciente.
> Repetir iteraciones eventualmente llevará a un **mínimo de energía estable**.

---

## 🔹 4️⃣ Conclusión matemática

| Tipo      | Ecuación aplicada                                               | Comportamiento                                   |
| --------- | --------------------------------------------------------------- | ------------------------------------------------ |
| Síncrono  | $\mathbf{S}^{(t+1)} = \text{sgn}(W \mathbf{S}^{(t)})$           | Puede oscilar, no garantiza convergencia         |
| Asíncrono | $S_i^{(t+1)} = \text{sgn}(\sum_j w_{ij} S_j^{(\text{actual})})$ | Siempre converge a mínimo de energía (attractor) |

# Resumen
- Una iteracion completa de actualización asíncrona (actualizar todas las neuronas al menos una vez.)
- En tu ejemplo, actualizar $S_1, S_2, S_3$ una vez cada una constituye una iteración completa.
- Para Hopfield asíncrono, generalmente se repiten varias iteraciones hasta que ningún cambio ocurre y la red converge a un mínimo de energía estable.

| Concepto      | Qué significa                          | Ejemplo en tu caso |
| ------------- | -------------------------------------- | ------------------ |
| Actualización | Cambiar **una neurona**                | S1 → S2 → S3       |
| Iteración     | Cambiar **todas las neuronas una vez** | S1,S2,S3           |


# Función de energía (H) y criterio de parada

Vamos a calcular la **función de energía (H)** para el ejemplo asíncrono y ver cómo determina cuándo detener las iteraciones. Teniamos una red de 3 neuronas con pesos:

$$W = \begin{bmatrix}
0 & 1 & -1 \\
1 & 0 & 1 \\
-1 & 1 & 0
\end{bmatrix}, \quad \mathbf{S} = [S_1, S_2, S_3]$$


## 1️⃣ Fórmula de la energía

La **función de energía** de Hopfield es:

$$H = -\frac{1}{2} \sum_{i \neq j} w_{ij} S_i S_j$$

* La suma es sobre **pares distintos** ($i \neq j$) para evitar contar los términos de autocoupling.
* Cada vez que actualizamos una neurona asíncronamente, $H$ **no aumenta**: o disminuye o se mantiene.
* Cuando $H$ **ya no cambia**, la red está en un **mínimo de energía estable** → se puede detener la iteración.

---

## 2️⃣ Paso 1: Estado inicial

$$\mathbf{S}^{(0)} = [1, -1, 1]$$

Calculamos $H_0$:

$$\begin{align*}
H_0 &= -\frac{1}{2} \big( w_{12}S_1S_2 + w_{13}S_1S_3 + w_{21}S_2S_1 + w_{23}S_2S_3 + w_{31}S_3S_1 + w_{32}S_3S_2 \big) \\
&= -\frac{1}{2} \big( 1\cdot1\cdot(-1) + (-1)\cdot1\cdot1 + 1\cdot(-1)\cdot1 + 1\cdot(-1)\cdot1 + (-1)\cdot1\cdot1 + 1\cdot1\cdot(-1) \big) \\
&= -\frac{1}{2} \big( -1 -1 -1 -1 -1 -1 \big) \\
&= -\frac{1}{2} (-6) = 3
\end{align*}$$

✅ Energía inicial: $H_0 = 3$

---

## 3️⃣ Paso 2: Primera iteración asíncrona

**Actualizamos $S_1$:**

$$h_1 = 0\cdot1 + 1\cdot(-1) + (-1)\cdot1 = -2 \implies S_1 = -1$$

Después de actualizar todas las neuronas, llegamos al estado: $[-1, 1, 1]$

$$\begin{align*}
H_1 &= -\frac{1}{2} \big( 1\cdot(-1)\cdot1 + (-1)\cdot(-1)\cdot1 + 1\cdot1\cdot(-1) + 1\cdot1\cdot1 + (-1)\cdot1\cdot(-1) + 1\cdot1\cdot1 \big) \\
&= -\frac{1}{2} \big( -1 + 1 -1 +1 +1 +1 \big) \\
&= -\frac{1}{2} (2) = -1
\end{align*}$$

✅ Energía después de iteración 1: $H_1 = -1$ (menor que $H_0 = 3$)

---

## 4️⃣ Paso 3: Segunda iteración

El estado $[-1, 1, 1]$ **ya no cambia**, entonces:

$$H_2 = H_1 = -1$$

✅ La energía **ya no disminuye** → alcanzamos un **mínimo local estable**.

---

## 5️⃣ Conclusión

La **función de energía $H$** proporciona un criterio matemático para detener iteraciones:

$$\text{Si } H_{\text{nuevo}} = H_{\text{previo}} \implies \text{estado estable alcanzado}$$

Alternativamente, puedes usar un **criterio práctico**: cuando el estado no cambia entre iteraciones, la red ha convergido.


# Complejidad de Hopfield: O(N²)

Tu intuición es correcta: **la complejidad de Hopfield "normal" es O(N²) por iteración**, y hay razones matemáticas muy claras detrás de esto. Vamos a analizarlo con detalle.

## 1️⃣ Por qué es O(N²)

La actualización de una neurona $i$ es:

$$S_i \leftarrow \text{sgn}\Big( \sum_{j=1}^N w_{ij} S_j \Big)$$

* Para cada neurona $i$, hay que calcular la suma sobre **todas las $N$ neuronas** ($j=1..N$) → $O(N)$ operaciones.
* Si actualizamos **todas las $N$ neuronas** (una iteración completa), el total es $O(N^2)$.

En la **versión matricial**:

$$\mathbf{S}_{\text{nuevo}} = \text{sgn}(W \mathbf{S})$$

* La multiplicación $W \mathbf{S}$ es un **producto matriz-vector de tamaño $N \times N$** → $O(N^2)$.
* Aplicar la función signo elemento a elemento es $O(N)$, despreciable frente a $O(N^2)$.

Por eso, cada iteración de Hopfield **normalmente cuesta $O(N^2)$**.

---

## 2️⃣ ¿Se puede reducir?

En la forma estándar de Hopfield no mucho, pero hay algunos escenarios donde sí se puede hacer más eficiente:

1. **Matrices dispersas (sparse):**
    * Si $W$ es muy dispersa (muchos ceros), solo multiplicas las entradas no nulas → puede bajar la complejidad a $O(kN)$, con $k$ promedio de conexiones por neurona.

2. **Actualización asíncrona parcial:**
    * Actualizar solo una fracción de neuronas por iteración. No reduces la complejidad total si quieres convergencia completa, pero cada paso es más rápido.

3. **Aproximaciones o estructuras especiales:**
    * Por ejemplo, usar **redes de Hopfield modernas** basadas en matrices bajas-rango, o **Tensor Hopfield** en deep learning.
    * Estas estructuras pueden reducir el costo de memoria y multiplicación de $O(N^2)$ a algo más cercano a $O(Nr)$, donde $r$ es rango pequeño.

---

## 3️⃣ Conclusión

* **Hopfield clásico**: cada iteración completa → $O(N^2)$.
* **Reducción** posible solo con restricciones (sparse, bajo-rango) o aproximaciones, pero la **fórmula matemática estándar** $h_i = \sum_j w_{ij} S_j$ **siempre implica $O(N^2)$** para $N$ neuronas.

> En pocas palabras: sí, tu pensamiento es correcto, la complejidad $O(N^2)$ es **intrínseca a la definición estándar de Hopfield**.
