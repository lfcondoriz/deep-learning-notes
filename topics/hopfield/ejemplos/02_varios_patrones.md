> La diferencia entre síncrono y asíncrono aparece en la regla de actualización del estado, no en la energía ni en la matriz W.
# 2. - Ejemplo con varios patrones
Supongamos que queremos almacenar dos patrones en una red de Hopfield con 4 neuronas:
$$
\begin{aligned}
\mathbf{\xi}^1 &= [+1, -1, +1, -1] \\
\mathbf{\xi}^2 &= [-1, +1, -1, +1] \\
\mathbf{\xi}^3 &= [+1, +1, +1, -1]
\end{aligned}
$$

## 2.1 - Construcción de la matriz de pesos

Usamos la regla de Hebb para varios patrones:

$$
w_{ij} = \frac{1}{N} \sum_{\mu=1}^{p} \xi_i^\mu \xi_j^\mu,
\quad w_{ii} = 0
$$

Se puede usar cálculo matricial para obtener $W$ sin problema. Esto no entra en conflicto con usar actualización asíncrona después.

$$
\tilde{W} = \frac{1}{N} \sum_{\mu=1}^{p} \mathbf{\xi}^\mu (\mathbf{\xi}^\mu)^T
$$

Luego se eliminan las auto-conexiones:

$$
W_{ij} =
\begin{cases}
\tilde{W}_{ij} & i \neq j \\
0 & i = j
\end{cases}
$$

---

### 2.1.1 - Cálculo de los productos externos

$$
\xi^1 (\xi^1)^T =
\begin{pmatrix}
+1 & -1 & +1 & -1 \\
\end{pmatrix}
\begin{pmatrix}
+1 \\
-1 \\
+1 \\
-1
\end{pmatrix}
=
\begin{pmatrix}
+1 & -1 & +1 & -1 \\
-1 & +1 & -1 & +1 \\
+1 & -1 & +1 & -1 \\
-1 & +1 & -1 & +1
\end{pmatrix}
$$

$$
\xi^2 (\xi^2)^T =
\begin{pmatrix}
-1 & +1 & -1 & +1
\end{pmatrix}
\begin{pmatrix}
-1 \\
+1 \\
-1 \\
+1
\end{pmatrix}
=
\begin{pmatrix}
+1 & -1 & +1 & -1 \\
-1 & +1 & -1 & +1 \\
+1 & -1 & +1 & -1 \\
-1 & +1 & -1 & +1
\end{pmatrix}
$$

$$
\xi^3 (\xi^3)^T =
\begin{pmatrix}
+1 & +1 & +1 & -1
\end{pmatrix}
\begin{pmatrix}
+1 \\
+1 \\
+1 \\
-1
\end{pmatrix}
=
\begin{pmatrix}
+1 & +1 & +1 & -1 \\
+1 & +1 & +1 & -1 \\
+1 & +1 & +1 & -1 \\
-1 & -1 & -1 & +1
\end{pmatrix}
$$

---

### 2.1.3 - Suma de contribuciones

$$
\tilde{W} = \frac{1}{4} \left(
\xi^1 (\xi^1)^T +
\xi^2 (\xi^2)^T +
\xi^3 (\xi^3)^T
\right)
$$

$$
\tilde{W} =
\frac{1}{4}
\begin{pmatrix}
+3 & -1 & +3 & -3 \\
-1 & +3 & -1 & +1 \\
+3 & -1 & +3 & -3 \\
-3 & +1 & -3 & +3
\end{pmatrix}
$$

$$
\tilde{W} =
\begin{pmatrix}
+0.75 & -0.25 & +0.75 & -0.75 \\
-0.25 & +0.75 & -0.25 & +0.25 \\
+0.75 & -0.25 & +0.75 & -0.75 \\
-0.75 & +0.25 & -0.75 & +0.75
\end{pmatrix}
$$

---

### 2.1.4 - Eliminación de auto-conexiones

Finalmente, se imponen:

$$
w_{ii} = 0
$$

por lo que:

$$
W =
\begin{pmatrix}
0 & -0.25 & 0.75 & -0.75 \\
-0.25 & 0 & -0.25 & 0.25 \\
0.75 & -0.25 & 0 & -0.75 \\
-0.75 & 0.25 & -0.75 & 0
\end{pmatrix}
$$

## 2.2 - Definir el estado inicial (input o patrón ruidoso)
Supongamos que el estado inicial de la red es:
$$S(0) =
\begin{bmatrix}+1 \\
+1 \\
+1 \\
-1
\end{bmatrix}
$$

Este estado es un patrón ruidoso, ya que difiere del patrón $\xi^1$ en la segunda neurona (debería ser -1, pero es +1).

## 2.3 - Actualización de la red
La actualización se hace con la función signo:
$$
S_i(t+1) = \text{sgn}\Bigg( \sum_{j=1}^N w_{ij} S_j(t) \Bigg)
$$
### 2.3.1 - Criterio real de parada
En la práctica, se puede usar un criterio de parada como:
- No hay cambios en el estado de la red (convergencia).
    $$ S(t+1) = S(t) $$
- Se alcanza un número máximo de iteraciones para evitar ciclos infinitos.

### 2.3.2 - Primer Iteración (t=0 a t=1)
- Actualización de la neurona 1:
$$
h_1 = w_{12} S_2 + w_{13} S_3 + w_{14} S_4 = (-0.25)(+1) + (0.75)(+1) + (-0.75)(-1) = 0.25
$$
$$
S_1(1) = \text{sgn}(0.25) = +1 \quad \text{(no cambia)}
$$
- Actualización de la neurona 2:
$$
h_2 = w_{21} S_1 + w_{23} S_3 + w_{24} S_4 = (-0.25)(+1) + (-0.25)(+1) + (0.25)(-1) = -0.75
$$
$$
S_2(1) = \text{sgn}(-0.75) = -1 \quad \text{(cambia)}
$$
- Actualización de la neurona 3:
$$
h_3 = w_{31} S_1 + w_{32} S_2 + w_{34} S_4 = (0.75)(+1) + (-0.25)(-1) + (-0.75)(-1) = 0.75
$$
$$
S_3(1) = \text{sgn}(0.75) = +1 \quad \text{(no cambia)}
$$
- Actualización de la neurona 4:
$$
h_4 = w_{41} S_1 + w_{42} S_2 + w_{43} S_3 = (-0.75)(+1) + (0.25)(-1) + (-0.75)(+1) = -0.75
$$
$$
S_4(1) = \text{sgn}(-0.75) = -1 \quad \text{(no cambia)}
$$

Después de la primera actualización, el estado de la red es:

$$S(1) =
\begin{bmatrix}+1 \\
-1 \\
+1 \\
-1
\end{bmatrix}
$$

### 2.3.3 - Segunda Iteración (t=1 a t=2)

- **Actualización de la neurona 1:**
$$
h_1 = w_{12} S_2 + w_{13} S_3 + w_{14} S_4 = (-0.25)(-1) + (0.75)(+1) + (-0.75)(-1) = 0.75
$$
$$
S_1(2) = \text{sgn}(0.75) = +1 \quad \text{(no cambia)}
$$

- **Actualización de la neurona 2:**
$$
h_2 = w_{21} S_1 + w_{23} S_3 + w_{24} S_4 = (-0.25)(+1) + (-0.25)(+1) + (0.25)(-1) = -0.75
$$
$$
S_2(2) = \text{sgn}(-0.75) = -1 \quad \text{(no cambia)}
$$

- **Actualización de la neurona 3:**
$$
h_3 = w_{31} S_1 + w_{32} S_2 + w_{34} S_4 = (0.75)(+1) + (-0.25)(-1) + (-0.75)(-1) = 0.75
$$
$$
S_3(2) = \text{sgn}(0.75) = +1 \quad \text{(no cambia)}
$$

- **Actualización de la neurona 4:**
$$
h_4 = w_{41} S_1 + w_{42} S_2 + w_{43} S_3 = (-0.75)(+1) + (0.25)(-1) + (-0.75)(+1) = -0.75
$$
$$
S_4(2) = \text{sgn}(-0.75) = -1 \quad \text{(no cambia)}
$$

Después de la segunda actualización, el estado de la red es:
$$
S(2) =
\begin{bmatrix}+1 \\
-1 \\
+1 \\
-1
\end{bmatrix}
$$

### 2.3.4 - Verificación de convergencia
Comparando $S(2)$ con $S(1)$:
$$
S(2) = S(1)
$$

Esto indica que la red ha alcanzado un punto fijo.

Por lo tanto, el sistema ha convergido a un estado estable, que coincide con el patrón $\xi^1$, el cual actúa como atractor de la red.

En una red de Hopfield, este comportamiento confirma la recuperación de memoria asociativa.

## 2.4 - Función de energía (verificación)
La energía de una red de Hopfield se define como:

$$
E = -\frac{1}{2} \sum_{i,j} w_{ij} S_i S_j
$$

En forma matricial:

$$
E = -\frac{1}{2} \mathbf{S}^T W \mathbf{S}
$$

Se calcula evaluando el estado de la red en cada iteración.

---

Para el estado inicial $S(0)$:

$$
S(0)^T W S(0)
$$

y luego:

$$
E(0) = -\frac{1}{2} S(0)^T W S(0)
$$

Del mismo modo para $S(1)$:

$$
E(1) = -\frac{1}{2} S(1)^T W S(1)
$$

Como la red sigue una dinámica de descenso de energía, se cumple:

$$
E(1) \leq E(0)
$$

### 2.4.1 - Cálculos
- Para $S(0)$:
$$
S(0)^T W S(0) =
\begin{bmatrix} +1 & +1 & +1 & -1 \end{bmatrix}
\begin{pmatrix}
0 & -0.25 & 0.75 & -0.75 \\
-0.25 & 0 & -0.25 & 0.25 \\
0.75 & -0.25 & 0 & -0.75 \\
-0.75 & 0.25 & -0.75 & 0
\end{pmatrix}
\begin{bmatrix} +1 \\ +1 \\ +1 \\ -1 \end{bmatrix}
$$

$$
= \begin{bmatrix} +1 & +1 & +1 & -1 \end{bmatrix}
\begin{bmatrix} 1.25 \\ -0.75 \\ 1.25 \\ -1.25 \end{bmatrix}
= 3
$$

- Para $S(1)$:
$$S(1)^T W S(1) =
\begin{bmatrix} +1 & -1 & +1 & -1 \end{bmatrix}
\begin{pmatrix}
0 & -0.25 & 0.75 & -0.75 \\
-0.25 & 0 & -0.25 & 0.25 \\
0.75 & -0.25 & 0 & -0.75 \\
-0.75 & 0.25 & -0.75 & 0
\end{pmatrix}
\begin{bmatrix} +1 \\ -1 \\ +1 \\ -1 \end{bmatrix}
$$

$$
= \begin{bmatrix} +1 & -1 & +1 & -1 \end{bmatrix}
\begin{bmatrix} 1.75 \\ -0.75 \\ 1.75 \\ -1.75 \end{bmatrix}
= 6
$$

Entonces:
$$
E(0) = -\frac{1}{2} \cdot 3 = -1.5
$$
$$
E(1) = -\frac{1}{2} \cdot 6 = -3
$$
$$
E(1) < E(0)
$$

Como se esperaba, $E(1) < E(0)$, lo que confirma que la red ha descendido en energía al corregir el error.





## 2.5 - Crosstalk

$$
C_i^{,v} =
\frac{1}{N}\sum_{\mu\neq v}\sum_{j\neq i}
\xi_i^\mu \xi_j^\mu \xi_j^v
$$

Interpretación:

* $(v)$: patrón que quieres recuperar (ej: $(\xi^1)$)
* $(\mu \neq v)$: otros patrones (ruido)
* $(j \neq i)$: todas las neuronas excepto la $i$


### 2.5.1 - Continuando con el ejemplo.

Patrones:

$$
\xi^1 = [1,-1,1,-1]
$$
$$
\xi^2 = [-1,1,-1,1]
$$
$$
\xi^3 = [1,1,1,-1]
$$

Vamos a calcular:

👉 $(C_i^{1})$ (crosstalk al recuperar patrón 1)

$$
\xi^2 = [-1,1,-1,1]
$$
$$
\xi^3 = [1,1,1,-1]
$$

Vamos a calcular:

👉 $(C_i^{1})$ (crosstalk al recuperar patrón 1)

---
- 🔧 3. Paso 1: fijar $(i)$

    Ejemplo: calculamos **neurona $(i=1)$**

    Entonces:

    $$
    C_1^{1}=
    \frac{1}{N}\sum_{\mu\neq 1}\sum_{j\neq 1}
    \xi_1^\mu \xi_j^\mu \xi_j^1
    $$

    Aquí $(N=4)$

---
- 🔧 4. Paso 2: separar patrones (μ=2,3)

    $$
    C_1^{1} = \frac{1}{4} \left( \text{(patrón 2)} + \text{(patrón 3)} \right)
    $$

    $$
    C_1^{1} = \frac{1}{4} \left( \sum_{j\neq 1} \xi_1^2 \xi_j^2 \xi_j^1 + \sum_{j\neq 1} \xi_1^3 \xi_j^3 \xi_j^1 \right)
    $$


    - **Patrón 2:**

    $$
    \begin{aligned}
    &\sum_{j\neq 1} \xi_1^2 \xi_j^2 \xi_j^1 \\
    =& (-1)(1)(-1) + (-1)(-1)(1) + (-1)(1)(-1) \\
    =& -1 + 1 - 1 = -1
    \end{aligned}
    $$

    - **Patrón 3:**
    $$\begin{aligned}
    &\sum_{j\neq 1} \xi_1^3 \xi_j^3 \xi_j^1 \\
    =& (1)(1)(-1) + (1)(1)(1) + (1)(-1)(-1)
    =& -1 + 1 + 1 = 1
    \end{aligned}
    $$

    Entonces:
    $$
    C_1^{1} = \frac{1}{4} \cdot (-1) + \frac{1}{4} \cdot (1) = 0
    $$



### 2.5.2 - Interpretación en Hopfield

Entonces:

$$h_1^1 = \xi_1^1 + C_1^1$$

$$h_1^1 = 1 + 0 = 1$$

👉 signo positivo → neurona estable ✔️

* **señal:** $1$
* **ruido (crosstalk):** $0$

### 2.5.3 - Idea clave que debes quedarte

En una Red de Hopfield:

$$\text{recuperación correcta} \iff |\text{señal}| > |\text{crosstalk}|$$

