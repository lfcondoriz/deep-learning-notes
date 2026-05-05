# 2. **Modelo de McCulloch-Pitts**

* En 1943, **McCulloch y Pitts** propusieron un modelo simple de neurona:

  * La neurona **suma ponderadamente sus entradas**.
  * Produce una salida **binaria** (1 si dispara, 0 si no) según si la suma supera un umbral.
  * Se puede escribir así:

    $$
    n_i(t+1) = \Theta\Big(\sum_j w_{ij} n_j(t) - \mu_i \Big)
    $$

    Donde:

    * $(n_i)$: estado de la neurona (1 = dispara, 0 = no dispara).
    * $(w_{ij})$: peso de la conexión entre la neurona (j) y (i) (positivo = excitatorio, negativo = inhibitorio).
    * $(\mu_i)$: umbral de activación de la neurona.

    $$
        \Theta(x) = \begin{cases}
        1 & \text{si } x \geq 0 \\
        0 & \text{si } x < 0
        \end{cases}
    $$

* **Importancia:** Aunque simple, estas neuronas pueden, en principio, **realizar cualquier cálculo** que haga una computadora digital, si se eligen correctamente los pesos.

## 2.1 **Diferencias con neuronas reales**

El modelo es simplificado; algunas diferencias importantes son:

1. **Respuesta continua:** Las neuronas reales responden de forma **gradual**, no solo 0 o 1.
2. **Suma no lineal de entradas:** Una neurona real puede hacer operaciones complejas tipo AND, OR dentro de sus dendritas.
3. **Secuencia de pulsos:** En realidad, las neuronas envían **muchos impulsos**, y la fase de los pulsos puede tener información.
4. **Actualización asíncrona:** Las neuronas no se actualizan todas al mismo tiempo.
5. **Variabilidad química:** La cantidad de neurotransmisor liberado puede variar aleatoriamente.

* Para capturar estas complejidades, los modelos se **generalizan**:

  * Se usan **funciones de activación continuas** (g(x)) en lugar de la función escalón.
  * Se permite **actualización asincrónica** y efectos **estocásticos** (aleatorios).


## 2.2 **Ecuación generalizada**

La ecuación que mencionas es:

$$
n_i := g\Big(\sum_j w_{ij} n_j - u_i\Big)
$$

* El símbolo `:=` significa **“asignamos a $(n_i)$ el valor de”**, no es una igualdad continua, sino que dice “cuando se actualiza la unidad, su nuevo estado será esto”.
* Cada término tiene un significado:

| Símbolo    | Significado                                                                                                                                                                                            |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| $(n_i)$      | Estado o activación de la neurona/unidad (i). Ahora puede ser **continuo**, no solo 0 o 1.                                                                                                             |
| $(g(\cdot))$ | **Función de activación**: transforma la suma ponderada en un valor de salida. Puede ser sigmoidal, tangente hiperbólica, ReLU, etc. Se llama también **gain**, **transfer** o **squashing function**. |
| $(w_{ij})$   | Peso de la conexión desde la neurona (j) a la (i). Puede ser positivo (excitatorio) o negativo (inhibitorio).                                                                                          |
| $(n_j)$      | Estado actual de la neurona (j).                                                                                                                                                                       |
| $(u_i)$      | Umbral de activación de la neurona (i). Es lo que hay que superar para que se active.                                                                                                                  |
| $(\sum_j)$   | Suma sobre todas las neuronas conectadas a (i).                                                                                                                                                        |

### **Diferencias con el modelo original de McCulloch-Pitts**

1. **Salida continua:**

   * Antes $(n_i = 0 \text{ o } 1)$.
   * Ahora $(n_i)$ puede ser cualquier valor dentro de un rango, por ejemplo $(0 \le n_i \le 1)$ si $(g(x))$ es sigmoidal.

2. **Función de activación (g(x)) más flexible:**

   * Puede ser **lineal**, **sigmoidal**, **ReLU**, etc., mientras sea **no lineal**, que es lo crucial para la capacidad de computación.

3. **Actualización asincrónica:**

   * En redes modernas, cada neurona puede actualizarse **en momentos diferentes** en lugar de todos a la vez.

4. **Incluye umbral (u_i):**

   * Permite ajustar la sensibilidad de la neurona a sus entradas.

## 2.3 **Arquitectura**

* **Capas:** ¿deben estar las neuronas organizadas en capas (como en perceptrones) o de manera libre?
* **Conectividad:** ¿cuántas conexiones debe tener cada neurona y cómo deben organizarse?
* **Función de activación:** ¿qué tipo de (g(x)) usar (sigmoidal, ReLU, tanh…)?
* **Actualización:** ¿sincrónica o asincrónica? ¿Determinística o con elementos aleatorios?
* **Cantidad de unidades:** ¿cuántas neuronas son necesarias para resolver un problema específico?
