# 1. Perceptrón Multicapa (MLP)

El perceptrón multicapa (MLP, *Multi-Layer Perceptron*) es una extensión del perceptrón simple que introduce una o más capas ocultas entre la capa de entrada y la capa de salida. Esto permite al modelo aprender representaciones más complejas y resolver problemas no lineales.

Cada neurona realiza dos operaciones fundamentales:

1. Una combinación lineal de las entradas.
2. La aplicación de una función de activación no lineal.

La composición de múltiples capas permite modelar relaciones complejas entre los datos.

---

## 1.2. Arquitectura General

Un MLP está compuesto por:

- Una capa de entrada.
- Una o más capas ocultas.
- Una capa de salida.

La red se organiza en capas indexadas mediante:

$$
l = 0,1,2,\dots,L
$$

donde:

- $l=0$ corresponde a la capa de entrada.
- $l=1,\dots,L-1$ corresponden a las capas ocultas.
- $l=L$ corresponde a la capa de salida.

Cada capa $l$ contiene $n_l$ neuronas.

---

## 1.3. Convención Moderna de Dimensiones

La convención estándar en Deep Learning es:

$$
W^{(l)} \in \mathbb{R}^{\text{salida} \times \text{entrada}}
$$

Es decir:

- Las filas representan las neuronas de salida.
- Las columnas representan las conexiones de entrada.

Para una capa general $l$:

- La entrada tiene dimensión $n_{l-1}$.
- La salida tiene dimensión $n_l$.

Por lo tanto:

$$
W^{(l)} \in \mathbb{R}^{n_l \times n_{l-1}}
$$

$$
\mathbf{b}^{(l)} \in \mathbb{R}^{n_l \times 1}
$$

---

## 1.4. Notación por Capas

La convención estándar en Deep Learning utiliza un superíndice entre paréntesis $(l)$ para indicar la capa a la que pertenece una variable.

- $\mathbf{x} \in \mathbb{R}^{n_0 \times 1}$: vector de entrada.
- $\mathbf{z}^{(l)} \in \mathbb{R}^{n_l \times 1}$: vector de sumas ponderadas de la capa $l$.
- $\mathbf{a}^{(l)} \in \mathbb{R}^{n_l \times 1}$: vector de activaciones de la capa $l$.
- $W^{(l)} \in \mathbb{R}^{n_l \times n_{l-1}}$: matriz de pesos de la capa $l$.
- $\mathbf{b}^{(l)} \in \mathbb{R}^{n_l \times 1}$: vector de sesgos de la capa $l$.
- $\phi^{(l)}$: función de activación de la capa $l$.

donde:

$$
n_l = \text{cantidad de neuronas de la capa } l
$$

---

## 1.5. Propagación hacia Adelante (Forward Propagation)

La propagación hacia adelante calcula las activaciones de cada capa de manera secuencial.

1. La entrada de la red se define como:
    $$
    \mathbf{a}^{(0)} = \mathbf{x}
    $$

2. Para cada capa:
    - Paso 1: Combinación Lineal

        $$
        \mathbf{z}^{(l)} = W^{(l)}\mathbf{a}^{(l-1)} + \mathbf{b}^{(l)}
        $$

    - Paso 2: Aplicación de la Función de Activación

        $$
        \mathbf{a}^{(l)} = \phi^{(l)}(\mathbf{z}^{(l)})
        $$

3. La salida final de la red es:

    $$
    \mathbf{a}^{(L)}
    $$

---

## 1.6. Interpretación Intuitiva

Cada neurona aprende una transformación de los datos de entrada.

- Las matrices de pesos $W^{(l)}$ controlan la importancia de cada conexión.
- Los sesgos $\mathbf{b}^{(l)}$ permiten desplazar la activación.
- Las funciones de activación introducen no linealidad.

Sin funciones de activación no lineales, múltiples capas equivaldrían a una única transformación lineal.

Las capas ocultas permiten construir representaciones progresivamente más abstractas de los datos.

---

## 1.7. Flujo General de Información

La información fluye de manera secuencial desde la entrada hasta la salida:

$$
\mathbf{x}
\rightarrow
\mathbf{z}^{(1)}
\rightarrow
\mathbf{a}^{(1)}
\rightarrow
\mathbf{z}^{(2)}
\rightarrow
\mathbf{a}^{(2)}
\rightarrow
\cdots
\rightarrow
\mathbf{z}^{(L)}
\rightarrow
\mathbf{a}^{(L)}
$$

donde:

- $\mathbf{z}^{(l)}$ representa la transformación lineal de la capa.
- $\mathbf{a}^{(l)}$ representa la activación de la capa.

---

## 1.8. Forma Compacta del MLP

Un MLP puede interpretarse como la composición de funciones parametrizadas:

$$
\mathbf{a}^{(L)} = f_\theta(\mathbf{x})
$$

donde $f_\theta$ representa toda la red neuronal, es decir:

$$
f_\theta(\mathbf{x}) =
\mathbf{a}^{(L)} =
\phi^{(L)}
\left(
W^{(L)}
\phi^{(L-1)}
\left(
\cdots
\phi^{(1)}
\left(
W^{(1)}\mathbf{x}
+
\mathbf{b}^{(1)}
\right)
\cdots
\right)
+
\mathbf{b}^{(L)}
\right)
$$

Ver "Deep Learning (Goodfellow, Bengio, Courville)"

Esta composición de transformaciones lineales y funciones no lineales es la base de la capacidad expresiva de las redes neuronales profundas.

Interpretación clave

* Cada capa es una función:

    $$
    x \mapsto \phi(Wx + b)
    $$

* La red completa es una **composición de funciones**:

    $$
    f_\theta = f^{(L)} \circ f^{(L-1)} \circ \cdots \circ f^{(1)}
    $$

## 1.9. Forward Pass (Paso hacia adelante)
La información fluye de manera secuencial desde la entrada hasta la salida:

$$
\mathbf{x}
\rightarrow
\mathbf{z}^{(1)}
\rightarrow
\mathbf{a}^{(1)}
\rightarrow
\mathbf{z}^{(2)}
\rightarrow
\mathbf{a}^{(2)}
\rightarrow
\cdots
\rightarrow
\mathbf{z}^{(L)}
\rightarrow
\mathbf{a}^{(L)}
$$

Combinación Lineal

$$
W^{(l)} \in \mathbb{R}^{n_l \times n_{l-1}}, \quad \mathbf{a}^{(l-1)} \in \mathbb{R}^{n_{l-1} \times 1}, \quad \mathbf{b}^{(l)} \in \mathbb{R}^{n_l \times 1}
$$

$$
\mathbf{z}^{(l)} = W^{(l)}\mathbf{a}^{(l-1)} + \mathbf{b}^{(l)}
$$

$$
\begin{bmatrix}
z_1^{(l)} \\
z_2^{(l)} \\
\vdots \\
z_{n_l}^{(l)}
\end{bmatrix}
=
\begin{bmatrix}
w_{11}^{(l)} & w_{12}^{(l)} & \cdots & w_{1n_{l-1}}^{(l)} \\
w_{21}^{(l)} & w_{22}^{(l)} & \cdots & w_{2n_{l-1}}^{(l)} \\
\vdots & \vdots & \ddots & \vdots \\
w_{n_l1}^{(l)} & w_{n_l2}^{(l)} & \cdots & w_{n_ln_{l-1}}^{(l)}
\end{bmatrix}
\begin{bmatrix}
a_1^{(l-1)} \\
a_2^{(l-1)} \\
\vdots \\
a_{n_{l-1}}^{(l-1)}
\end{bmatrix}
+
\begin{bmatrix}b_1^{(l)} \\
b_2^{(l)} \\
\vdots \\
b_{n_l}^{(l)}\end{bmatrix}
$$

Aplicación de la Función de Activación

$$
\mathbf{a}^{(l)} = \phi^{(l)}(\mathbf{z}^{(l)})
$$

## 1.10. Función de Pérdida y Optimización
Finalmente, comparamos la predicción de la red con el valor real:

$$
\mathcal{L}(a^{(L)}, y)
$$

La salida de la red depende de todos los parámetros del modelo, por lo tanto la pérdida puede escribirse como:

$$
\begin{equation}
\mathcal{L}(\theta) = \mathcal{L}(f_\theta(x), y)
\end{equation}
$$

donde:

$$
\begin{equation}
\theta = \{W^{(l)}, b^{(l)}\}_{l=1}^{L}
\end{equation}
$$

$$
\theta = \{W^{(1)}, b^{(1)}, W^{(2)}, b^{(2)}, \ldots, W^{(L)}, b^{(L)}\}
$$

y:

$$
\mathcal{L}: \mathbb{R}^{n_L} \times \mathbb{R}^{n_L} \to \mathbb{R}
$$

El objetivo del entrenamiento es encontrar:

$$
\begin{equation}
\theta^* = \arg\min_{\theta} \mathcal{L}(\theta)
\end{equation}
$$

---

## 1.11. Gradiente de los parámetros

El gradiente de la función de pérdida respecto a los parámetros del modelo es:

$$
\begin{equation}
\nabla_{\theta} \mathcal{L} =
\left(
\frac{\partial \mathcal{L}}{\partial W^{(1)}}, \frac{\partial \mathcal{L}}{\partial b^{(1)}}, \dots,
\frac{\partial \mathcal{L}}{\partial W^{(L)}}, \frac{\partial \mathcal{L}}{\partial b^{(L)}}
\right)
\end{equation}
$$

---

## 1.12. Actualización de parámetros

Una vez calculados los gradientes, se actualizan los parámetros mediante descenso de gradiente:

$$
\begin{equation}
\theta := \theta - \eta \nabla_{\theta} \mathcal{L}
\end{equation}
$$

Forma por capa:

$$
\begin{aligned}
W^{(l)} &:= W^{(l)} - \eta \frac{\partial \mathcal{L}}{\partial W^{(l)}} \\
b^{(l)} &:= b^{(l)} - \eta \frac{\partial \mathcal{L}}{\partial b^{(l)}}
\end{aligned}
$$

---
## 1.13. Backpropagation
Para calcular los gradientes de manera eficiente, se utiliza el algoritmo de backpropagation, que aplica la regla de la cadena de manera sistemática a través de las capas de la red.

1. Empiezas con:

    $$
    \mathcal{L}(\theta), \quad \theta = \{W^{(l)}, b^{(l)}\}
    $$

    y el objetivo real es:

    $$
    \frac{\partial \mathcal{L}}{\partial W^{(l)}}, \quad \frac{\partial \mathcal{L}}{\partial b^{(l)}}
    $$

    👉 Esto es correcto: **backprop existe para calcular esto eficientemente**

2. Partimos de:

    $$
    \mathbf{z}^{(l)} = W^{(l)} \mathbf{a}^{(l-1)} + \mathbf{b}^{(l)}
    $$

    $$
    \mathbf{a}^{(l)} = \phi^{(l)}(\mathbf{z}^{(l)})
    $$

3. Aplicar la regla de la cadena directamente:

    $$
    \begin{equation}
    \frac{\partial \mathcal{L}}{\partial W^{(l)}} =
    \frac{\partial \mathcal{L}}{\partial \mathbf{a}^{(l)}}
    \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}}
    \frac{\partial \mathbf{z}^{(l)}}{\partial W^{(l)}}
    \end{equation}
    $$

4. Tomando el tercer término

    $$
    \frac{\partial \mathbf{z}^{(l)}}{\partial W^{(l)}} = \mathbf{a}^{(l-1)}
    $$

3. Aplicar la regla de la cadena a $b^{(l)}$:

    $$
    \frac{\partial \mathcal{L}}{\partial b^{(l)}} =
    \frac{\partial \mathcal{L}}{\partial \mathbf{a}^{(l)}}
    \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}}
    \frac{\partial \mathbf{z}^{(l)}}{\partial b^{(l)}}
    $$

    y aquí:

    $$
    \frac{\partial \mathbf{z}^{(l)}}{\partial b^{(l)}} = 1
    $$

4.  Aparece el “delta” naturalmente:

    Definimos (NO se inventa, se reconoce):

    $$
    \delta^{(l)} :=
    \frac{\partial \mathcal{L}}{\partial \mathbf{z}^{(l)}}
    =
    \frac{\partial \mathcal{L}}{\partial \mathbf{a}^{(l)}}
    \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}}
    $$

5. Primer término de $\delta^{(l)}$:

    $$
    \frac{\partial \mathcal{L}}{\partial \mathbf{a}^{(l)}}
    =
    \frac{\partial \mathcal{L}}{\partial \mathbf{z}^{(l+1)}}
    \frac{\partial \mathbf{z}^{(l+1)}}{\partial \mathbf{a}^{(l)}}
    $$

    - Primer Término:

        $$
        \frac{\partial \mathcal{L}}{\partial \mathbf{z}^{(l+1)}}
        = \delta^{(l+1)}
        $$

    - Segundo Término:

        $$
        \frac{\partial \mathbf{z}^{(l+1)}}{\partial \mathbf{a}^{(l)}} = W^{(l+1)}
        $$


    Entonces:
    
    $$
    \frac{\partial \mathcal{L}}{\partial \mathbf{a}^{(l)}}
    =
    \delta^{(l+1)}(W^{(l+1)})^T
    $$

6. Segundo término de $\delta^{(l)}$ es:

    $$
    \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}}
    $$

    Esto es un Jacobiano $J \in \mathbb{R}^{m \times m}$.

    - Calcular explícitamente

        La entrada $((i,j))$ del Jacobiano es:

        $$
        J_{ij} = \frac{\partial a_i^{(l)}}{\partial z_j^{(l)}}
        $$

        Pero como cada neurona solo depende de su propia entrada:

        $$
        a_i^{(l)} = \phi^{(l)}(z_i^{(l)}) \quad \Rightarrow \quad \frac{\partial a_i^{(l)}}{\partial z_j^{(l)}} = 0 ;\quad \text{si } i \neq j
        $$

        y si $(i=j)$ (primer derivada de $\phi^{(l)}$):

        $$
        \frac{\partial a_i^{(l)}}{\partial z_i^{(l)}} = \phi^{(l)'}(z_i^{(l)})
        $$

    - El **Jacobiano de Activación (observando una única capa)** es diagonal:

        Jacobiano de la no linealidad

        $$
        \frac{\partial \mathbf a^{(l)}}{\partial \mathbf z^{(l)}} =
        \begin{bmatrix}
        \phi^{(l)'}(z_1^{(l)}) & 0 & \dots & 0 \\
        0 & \phi^{(l)'}(z_2^{(l)}) & \dots & 0 \\
        \vdots & \vdots & \ddots & \vdots \\
        0 & 0 & \dots & \phi^{(l)'}(z_m^{(l)})
        \end{bmatrix}
        $$

        $$
        \frac{\partial \mathbf a^{(l)}}{\partial \mathbf z^{(l)}} =
        \mathrm{diag}(\phi^{(l)'}(\mathbf z^{(l)}))
        $$


8. Finalmente, el $\delta^{(l)}$ se puede escribir como:

    $$
    \delta^{(l)} =
    \frac{\partial \mathcal{L}}{\partial \mathbf{z}^{(l)}}
    =
    \frac{\partial \mathcal{L}}{\partial \mathbf{a}^{(l)}}
    \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}}
    $$

    $$
    \delta^{(l)} =
    \delta^{(l+1)} (W^{(l+1)})^T  \cdot \mathrm{diag}(\phi^{(l)'}(\mathbf z^{(l)}))
    $$

    $$
    \delta^{(l)} =
    \delta^{(l+1)} (W^{(l+1)})^T  \odot \phi^{(l)'}(\mathbf z^{(l)})
    $$

    donde:
    - $\odot$ : producto elemento a elemento

## 9.1. Jacobiano
Recordemos que Una capa del MLP es:

$$
\mathbf{z}^{(l)} = W^{(l)} \mathbf{a}^{(l-1)} + \mathbf{b}^{(l)}
$$
$$
\mathbf{a}^{(l)} = \phi(\mathbf{z}^{(l)})
$$

Esto se puede ver como composición:

$$
\mathbf{a}^{(l)} = f^{(l)}(\mathbf{a}^{(l-1)})
$$

La red completa es composición:

$$
f_\theta(x) = f^{(L)} \circ f^{(L-1)} \circ \cdots \circ f^{(1)}(x)
$$


Hay DOS Jacobianos distintos:
- Jacobiano de la activación
- Jacobiano de la capa $l$
- Jacobiano usado en backprop (real)

### 9.1.1. Jacobiano de la activación (local)
Jacobiano de la no linealidad

$$
J_\phi = \frac{\partial \mathbf a^{(l)}}{\partial \mathbf z^{(l)}}
$$

$$
\frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}} =
\mathrm{diag}(\phi'(\mathbf{z}^{(l)}))
$$


### 9.1.2. Jacobiano de la capa $l$
Este es el que aparece en backprop:

$$
\frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{a}^{(l-1)}}
$$

1. Usamos regla de la cadena:

    $$
    \mathbf{a}^{(l-1)} \to \mathbf{z}^{(l)} \to \mathbf{a}^{(l)}
    $$

    Entonces:

    $$
    \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{a}^{(l-1)}}
    =
    \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}}
    \frac{\partial \mathbf{z}^{(l)}}{\partial \mathbf{a}^{(l-1)}}
    $$

2. cada parte es:
    - Activación (primer término):

        $$
        \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}}
        =

        \mathrm{diag}(\phi'(\mathbf{z}^{(l)}))
        $$

    - Parte lineal (segundo término):

        $$
        \mathbf{z}^{(l)} = W^{(l)} \mathbf{a}^{(l-1)} + b
        $$

        entonces:

        $$
        \frac{\partial \mathbf{z}^{(l)}}{\partial \mathbf{a}^{(l-1)}} = W^{(l)}
        $$

3. Resultado final:

    $$
    J^{(l)} = 
    \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{a}^{(l-1)}}
    =
    \mathrm{diag}(\phi'(\mathbf{z}^{(l)})) \, W^{(l)}
    $$

### 9.1.3. Jacobiano usado en backprop (real)
El Jacobiano que realmente se usa en backprop es:

$$
(J^{(l)})^T = \left(\frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{a}^{(l-1)}}\right)^T =
\left(\frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}} \frac{\partial \mathbf{z}^{(l)}}{\partial \mathbf{a}^{(l-1)}}\right)^T =
(W^{(l)})^T \, \mathrm{diag}(\phi'(\mathbf{z}^{(l)}))
$$