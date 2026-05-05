# 1. Primer Ejemplo: Un patron con cuatro neuronas.
## **1.1 Definir el patrón a memorizar**

Supongamos que queremos memorizar el patrón:

$$
\xi = [+1, -1, +1, -1]
$$

* $(N = 4)$ neuronas.
* Cada neurona puede ser $(+1)$ o $(-1)$.


## **1.2. Calcular los pesos $(w_{ij})$ usando la regla de Hebb**

Usamos la regla de Hebb para un solo patrón:

$$
w_{ij} = \frac{1}{N} \xi_i \xi_j
$$



Calculamos la **matriz de pesos** (W):

| i\j | 1     | 2     | 3     | 4     |
| --- | ----- | ----- | ----- | ----- |
| 1   | 0     | -0.25 | 0.25  | -0.25 |
| 2   | -0.25 | 0     | -0.25 | 0.25  |
| 3   | 0.25  | -0.25 | 0     | -0.25 |
| 4   | -0.25 | 0.25  | -0.25 | 0     |

* La diagonal $(w_{ii} = 0)$ porque no nos conectamos a nosotros mismos.
* Cada peso es $(\pm 0.25)$ dependiendo de si las neuronas coinciden o no.


## **1.3. Estado inicial con un error**

Supongamos que comenzamos con un estado **cercano** al patrón, con un error en la segunda neurona:

$$
S(0) = [+1, +1, +1, -1]
$$
    
* La segunda neurona debería ser $(-1)$, pero está $(+1)$.

---

## **1.4. Actualización de cada neurona**

La regla de Hopfield es:

$$
S_i := \text{sgn}\Big( \sum_j w_{ij} S_j \Big)
$$

### **Neurona 1:**

$$
h_1 = w_{12} S_2 + w_{13} S_3 + w_{14} S_4 = (-0.25)(+1) + (0.25)(+1) + (-0.25)(-1)
$$

$$
h_1 = -0.25 + 0.25 + 0.25 = 0.25
$$

$(\text{sgn}(0.25) = +1)$ ✅ no cambia.

---

### **Neurona 2:**

$$
h_2 = w_{21} S_1 + w_{23} S_3 + w_{24} S_4 = (-0.25)(+1) + (-0.25)(+1) + (0.25)(-1)
$$

$$
h_2 = -0.25 -0.25 -0.25 = -0.75
$$

$(\text{sgn}(-0.75) = -1)$ ✅ la neurona se corrige.

---

### **Neurona 3:**

$$
h_3 = w_{31} S_1 + w_{32} S_2 + w_{34} S_4 = (0.25)(+1) + (-0.25)(-1) + (-0.25)(-1)
$$

$$
h_3 = 0.25 + 0.25 + 0.25 = 0.75
$$

$(\text{sgn}(0.75) = +1)$ ✅ no cambia.


### **Neurona 4:**

$$
h_4 = w_{41} S_1 + w_{42} S_2 + w_{43} S_3 = (-0.25)(+1) + (0.25)(-1) + (-0.25)(+1)
$$

$$
h_4 = -0.25 -0.25 -0.25 = -0.75
$$

$(\text{sgn}(-0.75) = -1)$ ✅ no cambia.



## **1.5. Resultado después de una actualización**

$$
S(1) = [+1, -1, +1, -1] = \xi
$$

* La red **corrigió automáticamente el error**.
* Esto demuestra que $(\xi)$ es un **atractor** y que la red tiene **memoria asociativa**: parte de un estado parcial o ruidoso y recupera el patrón completo.

# 2. Mismo ejemplo anterior pero en forma matricial:

## 2.1 Representamos el patrón como vector

$$
\xi =
\begin{bmatrix} +1 \\ -1 \\ +1 \\ -1 \end{bmatrix}
$$

y el estado actual de la red también se puede escribir como un vector:

$$
S =
\begin{bmatrix} S_1 \\ S_2 \\ S_3 \\ S_4 \end{bmatrix}
$$

Por ejemplo, el estado inicial con un error era:

$$
S(0) =
\begin{bmatrix} +1 \\ +1 \\ +1 \\ -1 \end{bmatrix}
$$

---

## 2.2 La matriz de pesos

La regla de Hebb para **un patrón** es:

$$
W = \frac{1}{N} \xi \xi^T
$$

donde $N = 4$ y $\xi^T$ es el vector transpuesto.

Calculando:

$$
\xi \xi^T =
\begin{bmatrix} +1 \\ -1 \\ +1 \\ -1 \end{bmatrix}
\begin{bmatrix} +1 & -1 & +1 & -1 \end{bmatrix}
=
\begin{bmatrix}
1 & -1 & 1 & -1 \\
-1 & 1 & -1 & 1 \\
1 & -1 & 1 & -1 \\
-1 & 1 & -1 & 1
\end{bmatrix}
$$

Dividiendo por 4 (N), y poniendo 0 en la diagonal:

$$
W =
\begin{bmatrix}
0 & -0.25 & 0.25 & -0.25 \\
-0.25 & 0 & -0.25 & 0.25 \\
0.25 & -0.25 & 0 & -0.25 \\
-0.25 & 0.25 & -0.25 & 0
\end{bmatrix}
$$

✅ Exactamente como en tu ejemplo.

---

## 2.3 Actualización de la red en forma matricial

En vez de actualizar neurona por neurona, podemos usar:

$$
S_{\text{nuevo}} = \text{sgn}( W S_{\text{actual}} )
$$

* Multiplicamos la matriz $W$ por el vector $S$
* Aplicamos la función signo **elemento por elemento**

Veamos el producto:

$$
h = W S(0) =
\begin{bmatrix}
0 & -0.25 & 0.25 & -0.25 \\
-0.25 & 0 & -0.25 & 0.25 \\
0.25 & -0.25 & 0 & -0.25 \\
-0.25 & 0.25 & -0.25 & 0
\end{bmatrix}
\begin{bmatrix} +1 \\ +1 \\ +1 \\ -1 \end{bmatrix}
=
\begin{bmatrix} 0.25 \\ -0.75 \\ 0.75 \\ -0.75 \end{bmatrix}
$$

Luego aplicamos signo:

$$
S(1) = \text{sgn}(h) =
\begin{bmatrix} +1 \\ -1 \\ +1 \\ -1 \end{bmatrix} = \xi
$$

¡Y la red corrige el error automáticamente en una sola operación matricial! 🎯

---

💡 **Ventaja de la forma matricial:**

* Puedes actualizar **todas las neuronas al mismo tiempo** con una sola multiplicación de matriz.
* Es más fácil para **computación y análisis** cuando hay muchos patrones y muchas neuronas.

