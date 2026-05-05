Tenemos


Se aplica la funcion de activacion elemento a elemento:
$$
\mathbf{a} = \sigma(\mathbf{h})
$$

3. Pérdida: Calculamos el error final comparando la salida $a$ con el valor real $y$.

$$ L = \text{Loss}(a, y) $$








# backward
Para escribir matemáticamente lo que hace PyTorch bajo el capó (el proceso exacto que ocurre cuando llamas a `loss.backward()`), usamos la notación de composición de funciones y la regla de la cadena multivariable. 

En matemáticas puras, lo que hace el *framework* se conoce como **Diferenciación en modo inverso (Reverse-mode differentiation)**.

Así es como se escribe formalmente de principio a fin:

$$
W \in \mathbb{R}^{n \times m},\quad  \mathbf{x} \in \mathbb{R}^{n \times 1}, \quad \mathbf{b} \in \mathbb{R}^{m \times 1}
$$



$$
\mathbf{h} = W^T\mathbf{x} + \mathbf{b}, \quad \mathbf{h} \in \mathbb{R}^{m \times 1}
$$


### 1. El Forward Pass (Composición de Funciones)

Definimos nuestra secuencia de transformaciones espaciales. Sea $\mathbf{x} \in \mathbb{R}^n$ el vector de entrada, $W \in \mathbb{R}^{m \times n}$ la matriz de pesos, y $\mathbf{b} \in \mathbb{R}^m$ el sesgo.

La función de la capa completa se define como la composición:

$$
L(W, \mathbf{b}, \mathbf{x}) = \mathcal{L} \Big( \sigma \big( W\mathbf{x} + \mathbf{b} \big), \mathbf{y} \Big)
$$

Donde las operaciones intermedias son:
1.  **Transformación afín:** $\mathbf{h} = W\mathbf{x} + \mathbf{b}$
2.  **Activación no lineal:** $\mathbf{a} = \sigma(\mathbf{h})$
3.  **Pérdida escalar:** $L = \mathcal{L}(\mathbf{a}, \mathbf{y})$

---

### 2. El Backward Pass (La Regla de la Cadena)

Para aplicar gradiente descendente, necesitamos $\nabla_W L$ y $\nabla_{\mathbf{b}} L$. Matemáticamente, la regla de la cadena completa se escribe como el producto de jacobianos evaluados de derecha a izquierda (desde la salida hasta la entrada):

$$
\frac{\partial L}{\partial W} = \underbrace{\frac{\partial L}{\partial \mathbf{a}}}_{\text{Gradiente}} \cdot \underbrace{\frac{\partial \mathbf{a}}{\partial \mathbf{h}}}_{\text{Jacobiano}} \cdot \underbrace{\frac{\partial \mathbf{h}}{\partial W}}_{\text{Jacobiano}}
$$

### 3. La Ejecución Computacional (VJP en Matemáticas)

Como hablamos antes, no calculamos matrices gigantes. Escribimos la ejecución matemática usando vectores intermedios que acumulan el error. En la literatura de Deep Learning, estos gradientes acumulados suelen denotarse con el símbolo $\nabla$ o con una barra superior (ej. $\bar{\mathbf{h}}$), que representa $\frac{\partial L}{\partial \text{variable}}$.

Aquí está la secuencia exacta que calcula la computadora:

**Paso 1: Gradiente de la función de pérdida**
Calculamos cómo cambia el error final respecto a la salida de la red:
$$
\nabla_{\mathbf{a}} L = \frac{\partial \mathcal{L}(\mathbf{a}, \mathbf{y})}{\partial \mathbf{a}}
$$
*(Esto resulta en un vector columna)*.

**Paso 2: Retropropagación por la activación (VJP)**
Multiplicamos el gradiente anterior por el jacobiano de la función de activación $\sigma$. Como $\sigma$ actúa elemento a elemento, el jacobiano es una matriz diagonal, lo que matemáticamente se simplifica a un producto de Hadamard (elemento a elemento, denotado por $\odot$):
$$
\nabla_{\mathbf{h}} L = \nabla_{\mathbf{a}} L \odot \sigma'(\mathbf{h})
$$
*(Este es el famoso vector de error $\delta$ de la capa).*

**Paso 3: Retropropagación por la capa lineal (VJP)**
Finalmente, usamos $\nabla_{\mathbf{h}} L$ (nuestro $\delta$) para obtener los gradientes de los parámetros y de la entrada. Así se define matemáticamente:

*   **Gradiente de los pesos (para actualizar $W$):**
    $$
    \nabla_W L = (\nabla_{\mathbf{h}} L) \cdot \mathbf{x}^T
    $$
*   **Gradiente de los sesgos (para actualizar $\mathbf{b}$):**
    $$
    \nabla_{\mathbf{b}} L = \nabla_{\mathbf{h}} L
    $$
*   **Gradiente de la entrada (para pasar a la capa anterior):**
    $$
    \nabla_{\mathbf{x}} L = W^T \cdot \nabla_{\mathbf{h}} L
    $$

### En resumen

La genialidad matemática del *Backpropagation* es que transforma un problema de "calcular derivadas hipercomplejas" en un problema iterativo de álgebra lineal elemental. Si lo quisieras escribir en una sola ecuación compacta para actualizar los pesos, quedaría así:

$$
W_{nueva} = W_{actual} - \eta \Big( \big( \nabla_{\mathbf{a}} \mathcal{L} \odot \sigma'(W_{actual}\mathbf{x} + \mathbf{b}) \big) \mathbf{x}^T \Big)
$$