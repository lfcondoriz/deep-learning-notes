# Perceptron
```
x_1 --> w_1 --> | h = sum(w_i * x_i) | --> g(h) --> y_pred
```


## Construyendo tu "montaña de pérdida" paso a paso

Para que veas que es exactamente la misma matemática de $f(x,y)$, vamos a reducir la ecuación matricial gigante que pusiste a la red neuronal más pequeña posible: **1 peso y 1 sesgo**.

**1. El Dato Fijo (Tu realidad)**
Supongamos que tienes un único dato. Entra un $x = 1$, y sabes que la salida correcta debería ser $y_{real} = 2$.

**2. Tu Red Neuronal (El Modelo)**
Usando tu misma lógica, pero simplificada a una dimensión (sin matrices, solo números simples), y sin función de activación ($g(h) = h$):
$$
y_{pred} = w \cdot x + b
$$

**3. La Función de Pérdida (La Superficie)**
Vamos a usar el Error Cuadrático Medio, que es medir la distancia entre lo que predijiste y lo real, elevado al cuadrado:
$$
L = (y_{pred} - y_{real})^2
$$

**4. La Conexión Mágica**
Ahora, reemplazamos nuestra red y nuestros datos fijos dentro de la función de pérdida:
$$
L(w,b) = (w \cdot 1 + b - 2)^2
$$
$$
L(w,b) = (w + b - 2)^2
$$

¡Mírala de cerca! **Esta ecuación es una superficie en 3D**, exactamente igual que tu $f(x,y) = 1 - \frac{1}{10}(x^2 + 4y^2)$. 
*   En lugar de llamarse $f$, se llama $L$ (Loss).
*   En lugar de caminar cambiando las coordenadas $x$ e $y$, caminas cambiando las coordenadas $w$ y $b$.
*   Tu montaña $f(x,y)$ era un paraboloide invertido (una colina) y buscabas la cima. Esta superficie $L(w,b)$ es un paraboloide normal (un valle) y buscas el fondo, donde el error es $0$.

### Cuando actualizas los pesos...

Cuando el libro te dice que actualices los pesos:
$$
w := w - \alpha \frac{\partial L}{\partial w}
$$
Literalmente te está diciendo: "Párate en tu coordenada actual $(w,b)$, calcula el gradiente de esta nueva montaña (que apunta hacia arriba), e **invierte la dirección** (por el signo menos) para dar un pasito hacia el fondo del valle".

He creado un simulador interactivo para que experimentes exactamente esto. Verás cómo al cambiar el peso y el sesgo para intentar "acertar" la predicción de un punto, estás literalmente caminando sobre una superficie topográfica tridimensional.



# 🧠 Una neurona con 3 entradas

Ahora tu neurona recibe tres variables de entrada:

* $(x_1, x_2, x_3)$

y tiene cuatro parámetros entrenables:

* pesos $(w_1, w_2, w_3)$
* sesgo $(b)$

### 📌 Modelo (la predicción)

La neurona hace esto:

$$y_{\text{pred}} = w_1 x_1 + w_2 x_2 + w_3 x_3 + b$$

---

## 🎯 Función de pérdida (la "montaña")

Si tienes un valor real $(y_{\text{real}})$, la pérdida típica sigue siendo:

$$L = (y_{\text{pred}} - y_{\text{real}})^2$$

Sustituyendo el modelo:

$$L(w_1, w_2, w_3, b) = (w_1 x_1 + w_2 x_2 + w_3 x_3 + b - y_{\text{real}})^2$$

---

## 🗺️ ¿Qué tipo de "montaña" es ahora?

Aquí viene lo importante:

- Antes:
    * $L(w,b)$ → superficie en 3D (se puede dibujar como un valle clásico)
- Ahora:

    * $L(w_1, w_2, w_3, b)$ → **función en 5 dimensiones**

👉 Esto es una función en 5 dimensiones (4 parámetros + 1 altura)

---

## 🤯 Intuición clave

El algoritmo no cambia:

$$\theta := \theta - \alpha \nabla L$$

donde ahora:

$$\theta = (w_1, w_2, w_3, b)$$

---

## 🧩 Vista vectorial (la forma moderna)

$$\mathbf{x} = (x_1, x_2, x_3), \quad \mathbf{w} = (w_1, w_2, w_3)$$

$$y_{\text{pred}} = \mathbf{w} \cdot \mathbf{x} + b$$

$$L(\mathbf{w}, b) = (\mathbf{w} \cdot \mathbf{x} + b - y)^2$$



# Recordar la regla de actulización

Sí: cuando derivás respecto de todos los parámetros a la vez, lo que obtenés es el **gradiente**.

Pero se escribe así:

$$\nabla L(\mathbf{w}, b) = \left(\frac{\partial L}{\partial w_1}, \frac{\partial L}{\partial w_2}, \frac{\partial L}{\partial w_3}, \frac{\partial L}{\partial b}\right)$$

## Notacion Corrección clave de tu notación

$$\frac{dL(\mathbf{w}, b)}{d\mathbf{w}}$$

no es lo más correcto por dos razones:

### 1. No es derivada total $(d/d)$

Aquí no es una función de una sola variable, sino de varias variables independientes.

Por eso usamos $\partial$ (derivada parcial).

---

### 2. $\mathbf{w}$ no es una sola variable

$$\mathbf{w} = (w_1, w_2, w_3)$$

Entonces no se deriva "respecto a $\mathbf{w}$" como un todo escalar, sino respecto a **cada componente**.

---

## 🧠 Forma más limpia (la estándar en ML)

Se separa el sesgo del vector de pesos:

$$\theta = (w_1, w_2, w_3, b)$$

Entonces:

$$\nabla_\theta L = \left(\frac{\partial L}{\partial w_1}, \frac{\partial L}{\partial w_2}, \frac{\partial L}{\partial w_3}, \frac{\partial L}{\partial b}\right)$$

---

## 🚀 Interpretación geométrica (la idea importante)

Ese vector apunta hacia la dirección de mayor aumento de la pérdida, por eso en entrenamiento hacemos:

$$\theta := \theta - \alpha \nabla_\theta L$$

---

## 🧩 Conexión con tu intuición de montaña

- $L(w_1,w_2,w_3,b)$ = montaña en 4D
- $\nabla L$ = flecha que te dice "sube más rápido por aquí"
- Tú haces lo contrario = bajas al mínimo

---

## 💡 Un matiz que suele confundir

A veces se escribe $\nabla_{\mathbf{w}} L$, pero **eso normalmente excluye el bias** implícitamente.

Por eso en ML moderno es más claro usar $\nabla_\theta L$.


## Resumen claro

- ✔ Forma “teórica elegante”:
    $$
    \theta := \theta - \alpha \nabla L(\theta)
    $$
- ✔ Forma “de libro / código”:
    $$
    W := W - \alpha \frac{\partial L}{\partial W}, \quad b := b - \alpha \frac{\partial L}{\partial b}
    $$

✔ Son exactamente lo mismo



















# Para una matriz
Se tiene:

$$
W \in \mathbb{R}^{3 \times 2}, \quad x \in \mathbb{R}^{3 \times 1}, \quad b \in \mathbb{R}^{2 \times 1}
$$

$$\begin{bmatrix}h_1 \\h_2\end{bmatrix} = \begin{bmatrix}w_{11} & w_{12} & w_{13}  \\w_{21} & w_{22} & w_{23} \\\end{bmatrix} \cdot \begin{bmatrix}x_1 \\x_2 \\x_3 \\\end{bmatrix}+\begin{bmatrix}b_1 \\b_2\end{bmatrix}$$


$$
\begin{bmatrix}
w_{11}x_1 + w_{12}x_2 + w_{13}x_3 + b_1 \\
w_{21}x_1 + w_{22}x_2 + w_{23}x_3 + b_2
\end{bmatrix}
$$


## 🎯 3. La pérdida (ahora depende de h)

Supongamos una pérdida genérica:

$$
L(\mathbf{h}, \mathbf{y})
$$

donde:

* $(\mathbf{y})$ = target real
* $(\mathbf{h})$ = predicción

---

## 🧠 4. ¿Qué se deriva ahora?

Ahora viene lo importante:

### 🔹 No derivás solo un número

Derivás respecto a **todos los parámetros de la matriz y el bias**

---

## 📦 5. Gradiente completo

### Respecto a los pesos:

$$
\frac{\partial L}{\partial W}
=

\begin{bmatrix}
\frac{\partial L}{\partial w_{11}} & \frac{\partial L}{\partial w_{12}} & \frac{\partial L}{\partial w_{13}} \\
\frac{\partial L}{\partial w_{21}} & \frac{\partial L}{\partial w_{22}} & \frac{\partial L}{\partial w_{23}}
\end{bmatrix}
$$

### Respecto al bias:

$$
\frac{\partial L}{\partial \mathbf{b}}
=

\begin{bmatrix}
\frac{\partial L}{\partial b_1} \\
\frac{\partial L}{\partial b_2}
\end{bmatrix}
$$

---

## 🚀 6. Regla de actualización (gradiente descendente)

Ahora se actualiza TODO en bloque:

### Pesos:

$$
W \leftarrow W - \eta \frac{\partial L}{\partial W}
$$

### Bias:

$$
\mathbf{b} \leftarrow \mathbf{b} - \eta \frac{\partial L}{\partial \mathbf{b}}
$$

---

## 🧠 7. La idea clave (muy importante)

Aunque parezca complejo, esto es lo mismo que antes:

| Caso simple        | Caso actual               |
| ------------------ | ------------------------- |
| $(w)$ escalar        | $(W)$ matriz                |
| $(b)$ escalar        | $(\mathbf{b})$ vector       |
| 1 salida           | 2 salidas                 |
| gradiente = número | gradiente = matriz/vector |

---

## 🔥 8. Intuición profunda

Esta capa hace esto:

* cada neurona $(h₁, h₂)$ tiene su propia “fila de montaña”
* cada peso controla una dirección distinta en el espacio de entrada
* el gradiente te dice cómo mover cada conexión individual

---

## 💡 9. Forma compacta (la que se usa en ML real)

Todo junto se escribe como:

$$
\nabla_\theta L
=

\left(
\frac{\partial L}{\partial W},
\frac{\partial L}{\partial \mathbf{b}}
\right)
$$

y la actualización:

$$
\theta := \theta - \eta \nabla_\theta L
$$

---

## 🧩 10. Conexión con tu intuición de “montaña”

Antes:

* montaña 2D → $(f(x,y))$

Ahora:

* montaña en espacio de parámetros:

  * 6 dimensiones $(w₁₁…w₂₃ + b₁,b₂)$

👉 no la puedes ver, pero el gradiente sigue siendo la flecha de máxima subida.

---

Si quieres, el siguiente paso lógico es el más importante en redes neuronales:

👉 cómo se calcula este gradiente cuando agregas **activación + capa siguiente (backprop real con cadena de matrices)**
