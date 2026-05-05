# Perceptron
Tenemos $n$ entradas, un sola neurona, y una función de activación $a$. 

La salida de cada neurona se calcula como:
1. Dimensiónes:
    $$
    W \in \mathbb{R}^{1 \times n},\quad  \mathbf{x} \in \mathbb{R}^{n \times 1}, \quad b \in \mathbb{R}
    $$
2. Aplicamos la transformación afín:
    $$
    h = W\mathbf{x} + b, \quad h \in \mathbb{R}
    $$

    $$
    h = w_1x_1 + w_2x_2 + \ldots + w_nx_n + b
    $$
3. Función de activación:
    $$
    \hat{y} = a(h)
    $$

4. Función de pérdida:
    $$
    L(\hat{y}, y)
    $$

### Minimizando la función de pérdida
Lo que se busca es minimizar la función de pérdida $L$ ajustando los parámetros $W$ y $b$. Para esto, necesitamos calcular los gradientes $\nabla_W L$ y $\nabla_b L$, que nos indican cómo cambiar cada parámetro para reducir la pérdida.

$$\theta = (w_1, w_2, \ldots, w_n, b)$$

$$
\nabla_\theta L =
\left(
\frac{\partial L}{\partial W},
\frac{\partial L}{\partial b}
\right)
$$

$$
\nabla_\theta L = \left(\frac{\partial L}{\partial w_1}, \frac{\partial L}{\partial w_2}, \ldots, \frac{\partial L}{\partial w_n}, \frac{\partial L}{\partial b}\right)
$$


y la actualización:

$$
\theta := \theta - \eta \nabla_\theta L
$$

Al final nos queda:
$$
\theta := \theta - \eta \left(\delta \cdot \mathbf{x}^T, \delta\right) \quad \text{donde} \quad \delta = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h}
$$



## Regla de la cadena
Nos queda:
$$
\begin{aligned}
\nabla_\theta L &= \left(
\frac{\partial L}{\partial W},
\frac{\partial L}{\partial b}
\right) \\
&= \left(\frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h} \cdot \frac{\partial h}{\partial W},
\frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h} \cdot \frac{\partial h}{\partial b}\right) \\
&= \left(\delta \cdot \mathbf{x}^T, \delta\right) 
\end{aligned}
$$

### Para $W$
Para calcular $\nabla_\theta L$, aplicamos la regla de la cadena, que nos permite descomponer el gradiente en partes más manejables. Por ejemplo, para $\frac{\partial L}{\partial w_i}$, tenemos:
$$
\frac{\partial L}{\partial w_i} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h} \cdot \frac{\partial h}{\partial w_i} , \quad \text{para } i = 1, 2, \ldots, n
$$

Equivalente a usar el gradiente
$$
\begin{aligned}
\nabla_W L &= \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h} \cdot \nabla_W h \\
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
    \delta = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h}
    $$

Entonces, el gradiente de los pesos se puede escribir como: 
$$
\nabla_W L = \delta \cdot \mathbf{x}^T
$$

### Para $b$
El sesgo se actualiza con:
$$
\begin{aligned}
\frac{\partial L}{\partial b} &= \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h} \cdot \frac{\partial h}{\partial b} \\
&= \delta \cdot 1 \\
&= \delta
\end{aligned}
$$


## ¿Cómo se calcula el $\delta$ en la práctica?

Recordemos tu definición (usando ahora $\hat{y}$):
$$
\delta = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h}
$$

Este $\delta$ representa el **error local** de la neurona (cómo cambia la pérdida con respecto a la suma ponderada $h$). Para calcularlo y obtener un valor real, necesitas definir **qué función de pérdida ($L$)** y **qué función de activación ($\phi$)** estás usando. 


### Ejemplo: MSE y Función de Activación Sigmoide

Supongamos que estamos haciendo un problema de regresión y elegimos:
1.  **Función de pérdida (Error Cuadrático Medio - MSE):** 
    $$
    L = \frac{1}{2}(\hat{y} - y)^2
    $$
2.  **Función de activación (Sigmoide):**
    $$
    \hat{y} = \sigma(h) = \frac{1}{1 + e^{-h}}
    $$

**Paso A: Calcular la derivada de la pérdida respecto a la predicción ($\frac{\partial L}{\partial \hat{y}}$)**

Si derivamos $L$ usando la regla de la cadena básica:
$$
\frac{\partial L}{\partial \hat{y}} = 2 \cdot \frac{1}{2} (\hat{y} - y)^{2-1} \cdot 1 = (\hat{y} - y)
$$
*(Esta parte simplemente nos dice la diferencia directa entre lo que predijimos y la realidad).*

**Paso B: Calcular la derivada de la activación respecto a $h$ ($\frac{\partial \hat{y}}{\partial h}$)**

Una propiedad matemática muy útil de la función sigmoide es que su derivada se puede expresar en términos de su propia salida:
$$
\frac{\partial \hat{y}}{\partial h} = \sigma(h)(1 - \sigma(h)) = \hat{y}(1 - \hat{y})
$$

**Paso C: Juntar todo para obtener $\delta$**

Multiplicamos ambas partes:
$$
\delta = (\hat{y} - y) \cdot \hat{y}(1 - \hat{y})
$$

