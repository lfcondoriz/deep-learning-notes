# 1
Tu apunte está muy bien estructurado. Lo único que ajustaría es la parte de la función de pérdida para que quede matemáticamente completamente consistente con el resto de la notación.

El problema principal es que mezclás:

$$\mathcal L(a^{(L)}, y)$$

(con variables de salida)

y después escribís gradientes respecto de parámetros:

$$\nabla_{\theta^{(2)}} \mathcal L$$

sin explicitar que $a^{(L)}$ depende de $\theta^{(2)}$.

La forma más limpia es agregar una pequeña subsección aclaratoria.

Podrías reemplazar tu sección 4 por esto:

---

# 4. Función de pérdida

Finalmente, calculamos la pérdida comparando la predicción $a^{(L)}$ con el valor real $y$:

$$\mathcal L(a^{(L)}, y)$$

Como la predicción depende de los parámetros de la red,

$$a^{(L)} = f_\theta(x),$$

la pérdida también depende implícitamente de dichos parámetros:

$$\mathcal L(f_\theta(x), y)$$

o equivalentemente,

$$\mathcal L : \mathbb R^p \to \mathbb R$$

donde $p$ es la cantidad total de parámetros de la red.

El objetivo del entrenamiento es minimizar esta pérdida ajustando los pesos y sesgos.

Para la capa de salida:

$$\theta^{(2)} = (w_1^{(2)}, w_2^{(2)}, \ldots, w_m^{(2)}, b^{(2)}) \in \mathbb R^{m+1}$$

El gradiente respecto de estos parámetros es:

$$\nabla_{\theta^{(2)}} \mathcal L = \left( \frac{\partial \mathcal L}{\partial W^{(2)}}, \frac{\partial \mathcal L}{\partial b^{(2)}} \right) \in \mathbb R^{m+1}$$

---

Después de eso, todo tu desarrollo de backpropagation queda perfectamente consistente.

Hay además dos detalles menores que podrías corregir:

1. Acá:

$$a^{(L)} = \phi^{(2)}(z^{(2)})$$

y luego escribís:

$$a^{(L)} = w_1^{(2)}a_1^{(1)}+\cdots+b^{(2)}$$

Ahí falta la activación. Debería ser:

$$z^{(2)} = w_1^{(2)}a_1^{(1)}+\cdots+b^{(2)}$$

y recién después:

$$a^{(L)} = \phi^{(2)}(z^{(2)})$$

2. En:

$$\nabla_{\theta^{(2)}} \mathcal L = \left( \frac{\partial \mathcal L}{\partial W^{(2)}}, \frac{\partial \mathcal L}{\partial b^{(2)}} \right)$$

estrictamente hablando, eso no es un vector en $\mathbb R^{m+1}$, porque:

* $\frac{\partial \mathcal L}{\partial W^{(2)}} \in \mathbb R^{1\times m}$
* $\frac{\partial \mathcal L}{\partial b^{(2)}} \in \mathbb R$

Aunque conceptualmente sí representa el gradiente total.

Más formal sería:

$$\nabla_{\theta^{(2)}} \mathcal L = \left( \frac{\partial \mathcal L}{\partial w_1^{(2)}}, \dots, \frac{\partial \mathcal L}{\partial w_m^{(2)}}, \frac{\partial \mathcal L}{\partial b^{(2)}} \right) \in \mathbb R^{m+1}$$

y aparte definir:

$$\nabla_{W^{(2)}}\mathcal L$$

como gradiente matricial.

# 2
Sí: te falta el **paso de actualización (optimización)**, y es importante aclararlo porque ahí aparece cómo se usan los gradientes.

Y también es buena tu duda: **φ (phi) no se "separa" en W y b**. φ es la **función de activación**, no es un parámetro.

---

# 🔹 1. Qué es φ y qué NO es

En tu MLP:

* $W^{(l)}, b^{(l)}$ → **parámetros entrenables**
* $\phi^{(l)}$ → **función fija (no se entrena)**

Ejemplo:

$$\mathbf{a}^{(l)} = \phi^{(l)}(\mathbf{z}^{(l)})$$

donde:

$$\mathbf{z}^{(l)} = W^{(l)}\mathbf{a}^{(l-1)} + b^{(l)}$$

👉 φ solo transforma el resultado, no tiene W ni b adentro.

---

# 🔹 2. Lo que te falta: actualización de parámetros

Después del gradiente viene el paso clave del aprendizaje:

## Descenso por gradiente

Para los pesos:

$$W^{(2)} \leftarrow W^{(2)} - \eta \, \nabla_{W^{(2)}} \mathcal L$$

Para el sesgo:

$$b^{(2)} \leftarrow b^{(2)} - \eta \, \frac{\partial \mathcal L}{\partial b^{(2)}}$$

donde:

* $\eta$ = learning rate (tasa de aprendizaje)

---

# 🔹 3. Forma vectorial (más elegante)

$$\theta^{(2)} \leftarrow \theta^{(2)} - \eta \, \nabla_{\theta^{(2)}} \mathcal L$$

---

# 🔹 4. Cómo encaja todo

El flujo completo correcto es:

### Forward

$$\mathbf{x} \to \mathbf{z} \to \mathbf{a} \to \mathcal L$$

### Backward

$$\mathcal L \to \nabla_W, \nabla_b$$

### Update

$$W,b \to \text{se actualizan con gradiente}$$

---

# 🔥 Idea clave que te faltaba (muy importante)

* φ **NO se entrena**
* W y b **sí se entrenan**
* El aprendizaje ocurre SOLO en:
  $$W^{(l)}, b^{(l)}$$

---

# ✔️ Cómo lo podés agregar a tu apunte

Agregá una sección final:

---

## 6. Actualización de parámetros

Una vez calculado el gradiente, los parámetros se actualizan mediante descenso por gradiente:

$$W^{(l)} \leftarrow W^{(l)} - \eta \nabla_{W^{(l)}} \mathcal L$$

$$b^{(l)} \leftarrow b^{(l)} - \eta \frac{\partial \mathcal L}{\partial b^{(l)}}$$

donde $\eta$ es la tasa de aprendizaje.

---

Si querés, en el siguiente paso puedo ayudarte a dejar tu apunte "nivel libro de deep learning", corrigiendo notación para que sea 100% consistente (porque estás muy cerca de eso ya).

