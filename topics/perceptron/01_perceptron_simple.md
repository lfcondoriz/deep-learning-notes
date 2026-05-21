# 1. Perceptron
EL perceptron es la unidad básica de una red neuronal. Es un modelo de clasificación binaria que toma un vector de entrada, lo transforma linealmente y luego aplica una función de activación para producir una salida.

## 1.1. Arquitectura de una neurona
- **Capa de entrada:** Recibe el vector de características $\mathbf{x} \in \mathbb{R}^n$.
- **Capa de salida:** Produce la salida final.

## 1.2. El Forward Pass (La Predicción)
La salida de cada neurona se calcula como:
1. Dimensiónes:

    $$
    W \in \mathbb{R}^{1 \times n},\quad  \mathbf{x} \in \mathbb{R}^{n \times 1}, \quad b \in \mathbb{R}
    $$

2. Aplicamos la transformación afín:

    $$
    z_1 = W\mathbf{x} + b, \quad z_1 \in \mathbb{R}
    $$

    $$
    z_1 = w_1x_1 + w_2x_2 + \ldots + w_nx_n + b
    $$
    
3. Función de activación:

    $$
    a_1 = \phi(z_1)
    $$

4. Función de pérdida:

    $$
    L(a_1, y)
    $$

## 1.3. Minimizando la función de pérdida
Lo que se busca es minimizar la función de pérdida $L$ ajustando los parámetros $W$ y $b$. Para esto, necesitamos calcular el gradiente $\nabla_\theta L$.

$$
\begin{equation}
\theta = (w_1, w_2, \ldots, w_n, b)
\end{equation}
$$

$$
\begin{equation}
\nabla_\theta L =
\left(
\frac{\partial L}{\partial W},
\frac{\partial L}{\partial b}
\right)
\end{equation}
$$

$$
\nabla_\theta L = \left(\frac{\partial L}{\partial w_1}, \frac{\partial L}{\partial w_2}, \ldots, \frac{\partial L}{\partial w_n}, \frac{\partial L}{\partial b}\right)
$$


y la actualización:

$$
\begin{equation}
\theta := \theta - \eta \nabla_\theta L
\end{equation}
$$

Al final nos queda:

$$
\theta := \theta - \eta \left(\delta \cdot \mathbf{x}^T, \delta\right) \quad \text{donde} \quad \delta = \frac{\partial L}{\partial a_1} \cdot \frac{\partial a_1}{\partial h}
$$

## 1.4. Regla de la cadena
Para calcular $\nabla_\theta L$, aplicamos la regla de la cadena, que nos permite descomponer el gradiente en partes más manejables. Por ejemplo, para $\frac{\partial L}{\partial w_i}$

Nos queda:

$$
\begin{aligned}
\nabla_\theta L &= \left(
\frac{\partial L}{\partial W},
\frac{\partial L}{\partial b}
\right) \\
&= \left(\frac{\partial L}{\partial a_1} \cdot \frac{\partial a_1}{\partial h} \cdot \frac{\partial h}{\partial W},
\frac{\partial L}{\partial a_1} \cdot \frac{\partial a_1}{\partial h} \cdot \frac{\partial h}{\partial b}\right) \\
&= \left(\delta \cdot \mathbf{x}^T, \delta\right) 
\end{aligned}
$$

### 1.4.1. Para $W$
Tenemos:

$$
\frac{\partial L}{\partial w_i} = \frac{\partial L}{\partial a_1} \cdot \frac{\partial a_1}{\partial h} \cdot \frac{\partial h}{\partial w_i} , \quad \text{para } i = 1, 2, \ldots, n
$$

Equivalente a usar el gradiente

$$
\begin{aligned}
\nabla_W L &= \frac{\partial L}{\partial a_1} \cdot \frac{\partial a_1}{\partial h} \cdot \nabla_W h \\
&= \delta \cdot \mathbf{x}^T
\end{aligned}
$$

- Recordar que:

    $$
    \begin{aligned}
    \nabla_W h &= \left(\frac{\partial h}{\partial w_1}, \frac{\partial h}{\partial w_2}, \ldots, \frac{\partial h}{\partial w_n} \right) \\
    &= \left(x_1, x_2, \ldots, x_n\right) \\
    &= \mathbf{x}^T
    \end{aligned}
    $$ 
- Delta solo es un escalar.

    $$
    \delta = \frac{\partial L}{\partial a_1} \cdot \frac{\partial a_1}{\partial h}
    $$

Entonces, el gradiente de los pesos se puede escribir como: 
$$
\nabla_W L = \delta \cdot \mathbf{x}^T
$$

### 1.4.2. Para $b$
El sesgo se actualiza con:

$$
\begin{aligned}
\frac{\partial L}{\partial b} &= \frac{\partial L}{\partial a_1} \cdot \frac{\partial a_1}{\partial h} \cdot \frac{\partial h}{\partial b} \\
&= \delta \cdot 1 \\
&= \delta
\end{aligned}
$$

## 1.5. ¿Cómo se calcula el $\delta$ en la práctica?

Recordemos tu definición (usando ahora $a_1$):

$$
\delta = \frac{\partial L}{\partial a_1} \cdot \frac{\partial a_1}{\partial z_1}
$$

Este $\delta$ representa el **error local** de la neurona (cómo cambia la pérdida con respecto a la suma ponderada $z_1$). Para calcularlo y obtener un valor real, necesitas definir **qué función de pérdida ($L$)** y **qué función de activación ($\phi$)** estás usando. 


## 1.6. Ejemplo: MSE y Función de Activación Sigmoide

Supongamos que estamos haciendo un problema de regresión y elegimos:
1.  **Función de pérdida (Error Cuadrático Medio - MSE):** 

    $$
    L = \frac{1}{2}(a_1 - y)^2
    $$
2.  **Función de activación (Sigmoide):**

    $$
    a_1 = \phi(z_1) = \frac{1}{1 + e^{-z_1}}
    $$

**Paso A: Calcular la derivada de la pérdida respecto a la predicción ($\frac{\partial L}{\partial a_1}$)**

Si derivamos $L$ usando la regla de la cadena básica:

$$
\frac{\partial L}{\partial a_1} = 2 \cdot \frac{1}{2} (a_1 - y)^{2-1} \cdot 1 = (a_1 - y)
$$

*(Esta parte simplemente nos dice la diferencia directa entre lo que predijimos y la realidad).*

**Paso B: Calcular la derivada de la activación respecto a $z_1$ ($\frac{\partial a_1}{\partial z_1}$)**

Una propiedad matemática muy útil de la función sigmoide es que su derivada se puede expresar en términos de su propia salida:

$$
\frac{\partial a_1}{\partial z_1} = \phi'(z_1) = \phi(z_1)(1 - \phi(z_1)) = a_1(1 - a_1)
$$

**Paso C: Juntar todo para obtener $\delta$**

Multiplicamos ambas partes:

$$
\delta = (a_1 - y) \cdot a_1(1 - a_1)
$$
