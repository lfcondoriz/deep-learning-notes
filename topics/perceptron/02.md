# Perceptrón Multicapa (MLP)
El perceptrón multicapa (MLP) es una extensión del perceptrón simple que introduce una o más capas ocultas entre la capa de entrada y la capa de salida. Esto permite al modelo aprender representaciones más complejas y resolver problemas no lineales.

## 1. Arquitectura
Para ender la matematica detras del **Perceptrón Multicapa (MLP)** se utilizará la siguiente arquitectura:
- **Capa de Entrada:** $n$ características.
- **Capa Oculta (Capa 1):** $m$ neuronas, con una función de activación $\phi^{(1)}$.
- **Capa de Salida:** una neurona con una función de activación $\phi$

> Regla de oro de la convención moderna:
> $$W \in \mathbb{R}^{\text{salida} \times \text{entrada}}$$

Para la Capa 1:
* La **entrada** tiene tamaño $n$.
* La **salida** (las neuronas de la capa oculta) tiene tamaño $m$.
* Por lo tanto, tu matriz $W^{(1)}$ debe ser de tamaño **$m \times n$** (es decir, $m$ filas y $n$ columnas).


## 2. Notación por Capas (El superíndice l)
El estándar absoluto en Deep Learning es usar un **superíndice entre paréntesis $(l)$** para indicar la capa a la que pertenece una variable. 

- $\mathbf{x} \in \mathbb{R}^{n \times 1}$ vector de entrada
- $\mathbf{z}^{(l)} \in \mathbb{R}^{m \times 1}$ vector de sumas ponderadas de la capa oculta
- $\mathbf{a}^{(l)} \in \mathbb{R}^{m \times 1}$ vector de activaciones de la capa oculta
- $W^{(l)} \in \mathbb{R}^{m \times n}$ matriz de pesos de la capa oculta
- $\mathbf{b}^{(l)} \in \mathbb{R}^{m \times 1}$ vector de sesgos de la capa oculta
- $\phi^{(l)}$ función de activación de la capa oculta
- $a^{(L)} \in \mathbb{R}$ predicción final (salida de la última capa)


## 3. Paso Hacia Adelante (Forward Pass)
Con esta estructura, el orden de los datos es súper claro: $\mathbf{x} \to \mathbf{z}^{(1)} \to \mathbf{a}^{(1)} \to \mathbf{z}^{(2)} \to a^{(L)}$.

### 3.1 Capa Oculta (Capa 1)
Tiene $m$ neuronas y una función de activación $\phi^{(1)}$ (por ejemplo, ReLU).
1. **Dimensiones:**
    $$
    W^{(1)} \in \mathbb{R}^{m \times n}, \quad \mathbf{x} \in \mathbb{R}^{n \times 1}, \quad \mathbf{b}^{(1)} \in \mathbb{R}^{m \times 1}
    $$
2. **Suma ponderada:**
    $$
    \mathbf{z}^{(1)} = W^{(1)}\mathbf{x} + \mathbf{b}^{(1)}
    $$

    $$
    \begin{bmatrix}
    z_1^{(1)} \\
    z_2^{(1)} \\
    \vdots \\
    z_m^{(1)}
    \end{bmatrix} = 
    \begin{bmatrix}
    w_{11}^{(1)} & w_{12}^{(1)} & \dots & w_{1n}^{(1)} \\
    w_{21}^{(1)} & w_{22}^{(1)} & \dots & w_{2n}^{(1)} \\
    \vdots & \vdots & \ddots & \vdots \\
    w_{m1}^{(1)} & w_{m2}^{(1)} & \dots & w_{mn}^{(1)}
    \end{bmatrix} \cdot 
    \begin{bmatrix}
    x_1 \\
    x_2 \\
    \vdots \\
    x_n
    \end{bmatrix} +
    \begin{bmatrix}
    b_1^{(1)} \\
    b_2^{(1)} \\
    \vdots \\
    b_m^{(1)}
    \end{bmatrix}
    $$
    

3. **Activación:** (Se aplica elemento a elemento)
    $$
    \mathbf{a}^{(1)} = \phi^{(1)}(\mathbf{z}^{(1)})
    $$

### 3.2 Capa de Salida (Capa 2)
Tiene 1 neurona y una función de activación $\phi^{(2)}$ (por ejemplo, Sigmoide).
1. **Dimensiones:**
    $$
    W^{(2)} \in \mathbb{R}^{1 \times m}, \quad \mathbf{a}^{(1)} \in \mathbb{R}^{m \times 1}, \quad b^{(2)} \in \mathbb{R}
    $$
2. **Suma ponderada:** (Nota que la entrada de esta capa es $\mathbf{a}^{(1)}$)
    $$
    z^{(2)} = W^{(2)}\mathbf{a}^{(1)} + b^{(2)}
    $$
3. **Predicción final (Activación):**
    $$
    a^{(L)} = \phi^{(2)}(z^{(2)})
    $$

    $$
    a^{(L)} = w_{1}^{(2)}a_1^{(1)} + w_{2}^{(2)}a_2^{(1)} + \ldots + w_{m}^{(2)}a_m^{(1)} + b^{(2)}
    $$

*(Nota: $z^{(2)}$ y $b^{(2)}$ no están en negrita porque, al tener solo 1 neurona de salida, son escalares).*

## 4. Función de pérdida
Finalmente, calculamos la pérdida comparando la predicción $a^{(L)}$ con el valor real $y$:
$$
\mathcal{L}(a^{(L)}, y)
$$

Como la predicción depende de los parámetros de la red, la pérdida también depende implícitamente de dichos parámetros:
$$
\mathcal{L}(\theta) = \mathcal{L}(a^{(L)}, y) = \mathcal{L}(\phi^{(2)}(z^{(2)}), y), \quad \mathcal{L}: \mathbb{R^{m+1}} \to \mathbb{R}
$$

El objetivo del entrenamiento es minimizar esta pérdida ajustando los pesos y sesgos.

Para la capa de salida:
$$
\theta^{(2)} = (w_1^{(2)}, w_2^{(2)}, \ldots, w_m^{(2)}, b^{(2)}) \in \mathbb{R}^{m+1}
$$

El gradiente respecto de estos parámetros es:
$$
\nabla_{\theta^{(2)}} \mathcal{L} 
= \left(\frac{\partial \mathcal{L}}{\partial w_1^{(2)}}, \frac{\partial \mathcal{L}}{\partial w_2^{(2)}}, \ldots, \frac{\partial \mathcal{L}}{\partial w_m^{(2)}}, \frac{\partial \mathcal{L}}{\partial b^{(2)}}\right) \in \mathbb{R}^{m+1}
$$


- Simplificando la notación, podemos escribir el gradiente como:
    $$
    \nabla_{\theta^{(2)}} \mathcal{L} 
    = \left(
    \frac{\partial \mathcal{L}}{\partial W^{(2)}},
    \frac{\partial \mathcal{L}}{\partial b^{(2)}}
    \right) \\
    $$

## 5. Actualización de los parámetros
Una vez que tenemos el gradiente, podemos actualizar los parámetros usando el método de descenso de gradiente:
$$
\theta^{(2)} := \theta^{(2)} - \eta \nabla_{\theta^{(2)}} \mathcal{L}
$$

Forma separada:
$$
\begin{aligned}
W^{(2)} &:= W^{(2)} - \eta \frac{\partial \mathcal{L}}{\partial W^{(2)}} \\
b^{(2)} &:= b^{(2)} - \eta \frac{\partial \mathcal{L}}{\partial b^{(2)}}
\end{aligned}
$$


## 6. Paso Hacia Atrás (Backward Pass): Regla de la Cadena
Nos queda:
$$
\begin{aligned}
\nabla_{\theta^{(2)}} \mathcal{L} &= \left(
\frac{\partial \mathcal{L}}{\partial W^{(2)}},
\frac{\partial \mathcal{L}}{\partial b^{(2)}}
\right) \\
&= \left(\frac{\partial \mathcal{L}}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(2)}} \cdot \frac{\partial z^{(2)}}{\partial W^{(2)}},
\frac{\partial \mathcal{L}}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(2)}} \cdot \frac{\partial z^{(2)}}{\partial b^{(2)}}\right) \\
&= \left(\delta^{(2)} \cdot (\mathbf{a}^{(1)})^T, \delta^{(2)}\right) 
\end{aligned}
$$

### 6.1 Gradiente respecto a los Pesos $(W^{(2)})$
Para calcular $\nabla_{\theta^{(2)}} \mathcal{L}$, aplicamos la regla de la cadena, que nos permite descomponer el gradiente en partes más manejables. Por ejemplo, para $\frac{\partial \mathcal{L}}{\partial w_i^{(2)}}$, tenemos:
$$
\frac{\partial \mathcal{L}}{\partial w_i^{(2)}} = \frac{\partial \mathcal{L}}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(2)}} \cdot \frac{\partial z^{(2)}}{\partial w_i^{(2)}} , \quad \text{para } i = 1, 2, \ldots, m
$$

Equivalente a usar el gradiente:
$$
\begin{aligned}
\nabla_{W^{(2)}} \mathcal{L} &= \frac{\partial \mathcal{L}}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(2)}} \cdot \nabla_{W^{(2)}} z^{(2)} \\
&= \delta^{(2)} \cdot (\mathbf{a}^{(1)})^T
\end{aligned}
$$

- Recordar que:
    $$
    \begin{aligned}
    \nabla_{W^{(2)}} z^{(2)} &= \left(\frac{\partial z^{(2)}}{\partial w_1^{(2)}}, \frac{\partial z^{(2)}}{\partial w_2^{(2)}}, \ldots, \frac{\partial z^{(2)}}{\partial w_m^{(2)}} \right) \\
    &= \left(a_1^{(1)}, a_2^{(1)}, \ldots, a_m^{(1)}\right) \\
    &= (\mathbf{a}^{(1)})^T
    \end{aligned}
    $$ 
- Delta de la capa 2 es un escalar:
    $$
    \delta^{(2)} = \frac{\partial \mathcal{L}}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(2)}}
    $$

Entonces, el gradiente de los pesos se puede escribir como: 
$$
\nabla_{W^{(2)}} \mathcal{L} = \delta^{(2)} \cdot (\mathbf{a}^{(1)})^T
$$

### 6.2 Gradiente respecto al Sesgo $b^{(2)}$
El sesgo se actualiza con:
$$
\begin{aligned}
\frac{\partial \mathcal{L}}{\partial b^{(2)}} &= \frac{\partial \mathcal{L}}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(2)}} \cdot \frac{\partial z^{(2)}}{\partial b^{(2)}} \\
&= \delta^{(2)} \cdot 1 \\
&= \delta^{(2)}
\end{aligned}
$$

### 6.3 Gradiente respecto a los Pesos $(W^{(1)})$
Queremos calcular:

$$\frac{\partial\mathcal L}{\partial W^{(1)}}$$

porque queremos actualizar los pesos ocultos.

Recordemos el camino:

$$W^{(1)} \to \mathbf z^{(1)} \to \mathbf a^{(1)} \to z^{(2)} \to a^{(L)} \to \mathcal L$$

Entonces:

$$
\frac{\partial\mathcal L}{\partial W^{(1)}} = 
\underset{\delta^{(2)}}{\underbrace{\frac{\partial\mathcal L}{\partial a^{(L)}} \frac{\partial a^{(L)}}{\partial z^{(2)}}}} 

\underset{W^{(2)}}{\underbrace{\frac{\partial z^{(2)}}{\partial \mathbf a^{(1)}}}}

\underset{\mathrm{diag}(\phi'(\mathbf z^{(1)}))}{\underbrace{\frac{\partial \mathbf a^{(1)}}{\partial \mathbf z^{(1)}}}}

\frac{\partial \mathbf z^{(1)}}{\partial W^{(1)}}
$$

#### 6.3.1 Derivada de salida respecto activación oculta
Tenemos:

$$
z^{(2)} = W^{(2)}\mathbf a^{(1)}+b^{(2)} 
\quad z^{(2)}: \mathbb{R}^{m \times 1} \to \mathbb{R}
$$

Como:
- $W^{(2)}\in\mathbb R^{1\times m}$
- $\mathbf a^{(1)} \in \mathbb{R}^{m \times 1}$
- $b^{(2)} \in \mathbb{R}$
- $z^{(2)} \in \mathbb{R}$

entonces (es un jacobiano de dimensión $1 \times m$):

$$
\frac{\partial z^{(2)}}{\partial \mathbf a^{(1)}} = W^{(2)}
$$

#### 6.3.2 Derivada de activación oculta

$$
\mathbf a^{(1)} = \phi(\mathbf z^{(1)})
$$

donde normalmente en redes neuronales:

* $(\mathbf z^{(1)} \in \mathbb{R}^m)$
* $(\mathbf a^{(1)} \in \mathbb{R}^m)$
* $(\phi)$ se aplica **componente a componente**:
  $$
  a_i^{(1)} = \phi(z_i^{(1)})
  $$


1. **Queremos calcular**

    $$
    \frac{\partial \mathbf a^{(1)}}{\partial \mathbf z^{(1)}}
    $$

    Esto es un Jacobiano:

    $$
    J \in \mathbb{R}^{m \times m}
    $$

---

2. **Calcular explícitamente**

    La entrada $((i,j))$ del Jacobiano es:

    $$
    J_{ij} = \frac{\partial a_i^{(1)}}{\partial z_j^{(1)}}
    $$

    Pero como cada neurona solo depende de su propia entrada:

    $$
    a_i^{(1)} = \phi(z_i^{(1)}) \quad \Rightarrow \quad \frac{\partial a_i^{(1)}}{\partial z_j^{(1)}} = 0 ;\quad \text{si } i \neq j
    $$

    y si $(i=j)$:

    $$
    \frac{\partial a_i^{(1)}}{\partial z_i^{(1)}} = \phi'(z_i^{(1)})
    $$

---

3. **Resultados final**

    El Jacobiano es **diagonal**:

    $$
    \frac{\partial \mathbf a^{(1)}}{\partial \mathbf z^{(1)}} =
    \begin{bmatrix}
    \phi'(z_1^{(1)}) & 0 & \dots & 0 \\
    0 & \phi'(z_2^{(1)}) & \dots & 0 \\
    \vdots & \vdots & \ddots & \vdots \\
    0 & 0 & \dots & \phi'(z_m^{(1)})
    \end{bmatrix}
    $$

4. **Forma compacta (muy usada)**

    Se escribe como:

    $$
    \mathrm{diag}(\phi'(\mathbf z^{(1)}))
    $$


Intuición importante

* No hay mezcla entre neuronas → por eso es diagonal
* Cada neurona solo “se deriva a sí misma”
* Esto es clave para backpropagation eficiente

#### 6.3.3 Gradiente respecto a los pesos $(W^{(1)})$

$$
\frac{\partial\mathcal L}{\partial W^{(1)}} = 
\underset{\delta^{(2)}}{\underbrace{\frac{\partial\mathcal L}{\partial a^{(L)}} \frac{\partial a^{(L)}}{\partial z^{(2)}}}} 

\underset{W^{(2)}}{\underbrace{\frac{\partial z^{(2)}}{\partial \mathbf a^{(1)}}}}

\underset{\mathrm{diag}(\phi'(\mathbf z^{(1)}))}{\underbrace{\frac{\partial \mathbf a^{(1)}}{\partial \mathbf z^{(1)}}}}

\frac{\partial \mathbf z^{(1)}}{\partial W^{(1)}}
$$

Mirando los primeros cuatro térmninos:
$$
\delta^{(1)} = \frac{\partial\mathcal L}{\partial \mathbf z^{(1)}}
$$

Recodando como es $\delta^{(2)}$:
$$
\delta^{(2)} = \frac{\partial\mathcal L}{\partial z^{(2)}}
$$

Ahora si expresamos a $\delta^{(1)}$ en función de $\delta^{(2)}$:
$$
\begin{aligned}
\delta^{(1)} &= \frac{\partial\mathcal L}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(2)}} \cdot \frac{\partial z^{(2)}}{\partial \mathbf a^{(1)}} \cdot \frac{\partial \mathbf a^{(1)}}{\partial \mathbf z^{(1)}} \\
&= \delta^{(2)} \cdot W^{(2)} \cdot \mathrm{diag}(\phi'(\mathbf z^{(1)}))
\end{aligned}
$$

### 6.4 Gradiente respecto al Sesgo $b^{(1)}$
Observando el camino:
$$
\mathcal L \to a^{(L)} \to z^{(2)} \to \mathbf a^{(1)} \to \mathbf z^{(1)} \to b^{(1)}
$$

El sesgo se actualiza con:
$$
\begin{aligned}
\frac{\partial \mathcal{L}}{\partial b^{(1)}} &= \frac{\partial \mathcal{L}}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(2)}} \cdot \frac{\partial z^{(2)}}{\partial \mathbf a^{(1)}} \cdot \frac{\partial \mathbf a^{(1)}}{\partial \mathbf z^{(1)}} \cdot \frac{\partial \mathbf z^{(1)}}{\partial b^{(1)}} \\
&= \delta^{(1)} \cdot 1 \\
&= \delta^{(1)}
\end{aligned}
$$

### 6.5 Resumen del Backward Pass
Patrón universal para cualquier capa oculta $l$:
- Pesos:
    $$
    \frac{\partial \mathcal{L}}{\partial W^{(l)}} = \delta^{(l)} \cdot (\mathbf{a}^{(l-1)})^T
    $$
- Sesgos:
    $$
    \frac{\partial \mathcal{L}}{\partial b^{(l)}} = \delta^{(l)}
    $$
- Delta de la capa $l$:
    $$
    \delta^{(l)} = \frac{\partial\mathcal L}{\partial \mathbf z^{(l)}}
    $$
    $$
    \delta^{(l)} = \delta^{(l+1)} \cdot W^{(l+1)} \cdot \mathrm{diag}(\phi'(\mathbf z^{(l)}))
    $$


