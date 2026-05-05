# 3. Redes Hopfield

## **3.1. Problema de memoria asociativa**

### **3.1.1 Definición**

El problema central es:

> Almacenar un conjunto de patrones $( \xi_i^\mu )$ de manera que, al presentar un patrón nuevo $( \zeta_i )$, la red **responda produciendo el patrón almacenado más parecido a $( \zeta_i )$**.

* **Patrones almacenados:** $( \xi_i^\mu )$ con $\mu = 1, 2, ..., p$.
* **Neuronas/unidades de la red:** $( i = 1, 2, ..., N )$.
* **Valores de los patrones:** 0 o 1 en cada sitio (aunque más tarde se usan -1 y 1 para conveniencia matemática).

### **3.1.2 Comparación con computadora tradicional**

En una computadora secuencial, se haría algo así:

1. Guardar todos los patrones.
2. Calcular la **distancia de Hamming** entre el patrón de prueba $( \zeta_i )$ y cada patrón almacenado $( \xi_i^\mu )$:

$$
\sum_i [\xi_i^\mu(1-\zeta_i) + (1-\xi_i^\mu)\zeta_i]
$$

3. Seleccionar el patrón almacenado que tenga **menor distancia** y devolverlo como respuesta.

**Limitación:** Este procedimiento es secuencial y no aprovecha el paralelismo.

## **3.2 Ecuación general del Hopfield Model con umbral**

$$
S_i := \text{sgn}\Bigg(\sum_j w_{ij} S_j - \theta_i \Bigg) \tag{2.2}
$$

* $(S_i)$ → estado de la neurona (i) ((+1) o (-1))
* $(w_{ij})$ → peso de la conexión de la neurona (j) a la neurona (i)
* $(\theta_i)$ → umbral de la neurona (i)
* $(\text{sgn}(x))$ → función signo:
    $$
    \text{sgn}(x) = \begin{cases}
    +1 & \text{si } x > 0 \\
    -1 & \text{si } x < 0 \\
    0 & \text{si } x = 0
    \end{cases}
    $$

---
El término $\theta_i$ es un **bias** (sesgo) de la neurona:
es un bias (sesgo) de la neurona:
- si $\theta_i > 0$: cuesta más que la neurona sea +1
- si $\theta_i < 0$: favorece que sea +1

Es simplemente un desplazamiento del umbral de activación.

---
Truco importante (equivalencia)

El umbral se puede “absorber” en los pesos agregando una neurona fija:
- Definís $S_0 = 1$ (neurona de sesgo)
- Definís $w_{i0} = -\theta_i$ (peso de la neurona de sesgo a la neurona i)

Entonces la ecuación queda:
$$
S_i := \text{sgn}\Bigg(\sum_{j=0}^N w_{ij} S_j \Bigg)
$$

## **3.4 Notas sobre actualización**

* **Actualización sincrónica**: todas las neuronas $(S_i)$ se actualizan al mismo tiempo.
* **Actualización asincrónica**: se actualiza una neurona a la vez, elegida al azar.

> La mayoría de los análisis teóricos y simulaciones usan **actualización asincrónica**, porque es más realista para redes biológicas y para hardware distribuido.
