
# **1. Historia de las redes neuronales artificiales**

## **1.1 Primeros pasos**

* Inspiración: desde Aristóteles hasta McCulloch y Pitts (1943).
* McCulloch y Pitts crearon el **modelo de neurona umbral** y demostraron que las redes podían realizar **computación universal**.
* Minsky (1967) analizó estas redes como **máquinas de estados finitos**.

## **1.2 Redundancia y robustez**

* Von Neumann (1956) mostró que se podía hacer una red confiable usando **redundancia**, incluso si las partes eran poco fiables.
* Esto inspiró **representaciones distribuidas redundantes**, que son fundamentales para la resiliencia de las redes neuronales.

## **1.3 Teorías de campos continuos (Neurodinámica)**

* En vez de mirar neuronas individuales, algunos estudios modelaron **conjuntos grandes de neuronas** con ecuaciones diferenciales:

  * Rashevsky, Wiener, Beurle, Wilson & Cowan, Amari…
* Estas teorías buscaban **patrones de actividad global** más que disparos individuales.

## **1.4 Perceptrón de Rosenblatt (1960s)**

* Un perceptrón organiza las neuronas en **capas**, con conexiones **feed-forward** (de una capa a la siguiente).
* Rosenblatt desarrolló un **algoritmo de aprendizaje** para ajustar pesos (w_{ij}) iterativamente y que la red realizara la tarea deseada.

### **Limitación inicial**

* Minsky y Papert (1969) mostraron que **perceptrones de una sola capa** no podían resolver problemas simples como XOR.
* La solución: **perceptrones multicapa**, pero aún no existía un algoritmo de aprendizaje eficiente.

## **1.5 Desarrollo posterior**

* Años 1970: desarrollo de redes con **memoria asociativa** y aprendizaje distribuido (Anderson, Willshaw, Kohonen, Grossberg, Marr…).
* Enfoque físico: redes como **sistemas de spin** (Cragg, Temperley, Caianiello, Hopfield)

  * Introdujeron la idea de **función de energía** y **atractores** para representar memorias estables.
* Un enfoque estadístico: redes estocásticas (Hinton, Sejnowski, Peretto)

  * Unidades pueden cometer “errores” con cierta probabilidad, parecido a la **temperatura en física estadística**.

## **1.6 Backpropagation (retropropagación)**

* Década de 1980: renace el interés por perceptrones multicapa gracias a **algoritmos de backpropagation**:

  * Werbos (1974), Rumelhart, Hinton y Williams (1986), Parker, LeCun (1985).
* Permite **ajustar pesos en redes multicapa** para resolver problemas como XOR que una sola capa no podía.
* Aunque no es universal para cualquier tarea, **marcó el inicio del éxito moderno de redes neuronales**.



















