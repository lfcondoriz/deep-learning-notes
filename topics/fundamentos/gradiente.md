# Gradiente

Para una función $ f: \mathbb{R}^n \to \mathbb{R} $, el gradiente se define como:

$$
\nabla f(x_1, \ldots, x_n) = \left( \frac{\partial f}{\partial x_1}, \ldots, \frac{\partial f}{\partial x_n} \right)
$$

# Plano (hiperplano) tangente

Sea $ F: \mathbb{R}^n \to \mathbb{R} $ diferenciable y la superficie definida por:

$$
F(x_1, \ldots, x_n) = 0
$$

Entonces, el hiperplano tangente en $ a = (a_1, \ldots, a_n) $ es:

$$
\sum_{i=1}^{n} \frac{\partial F}{\partial x_i}(a),(x_i - a_i) = 0
$$

o en forma vectorial:

$$
\nabla F(a) \cdot (x - a) = 0
$$

# Ejemplo
Tomamos el ejemplo que se encuentra en el libro de Edwards,Calculo 2-ed 9 edicion, página 948, donde se tiene la función:
$$
f(x,y) = 1- \frac{1}{10}(x^2 + 4y^2)
$$

La evaluaremos en el punto $P = (1,1)$.
$$f(1,1) = 1 - \frac{1}{10}(1^2 + 4 \cdot 1^2) = 1 - \frac{5}{10} = \frac{1}{2}$$

Entonces $A = (1,1,\frac{1}{2})$ es un punto en la superficie definida por $f$.

## Cálculo del gradiente
El gradiente de esta función se calcula como:
$$
\nabla f(x,y) = \left( \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y} \right)
$$

Calculamos cada componente del gradiente:
1. Para $\frac{\partial f}{\partial x}$:
    $$
    \frac{\partial f}{\partial x} = -\frac{1}{5}x
    $$
2. Para $\frac{\partial f}{\partial y}$:
    $$
    \frac{\partial f}{\partial y} = -\frac{4}{10} \cdot 2y = -\frac{8}{10}y = -\frac{4}{5}y
    $$

Entonces, el gradiente de la función es:
$$
\nabla f(x,y) = \left( -\frac{1}{5}x, -\frac{4}{5}y \right)
$$

En el punto $P(1,1)$, el gradiente se evalúa como:
$$\nabla f(1,1) = \left( -\frac{1}{5}, -\frac{4}{5} \right)$$

## Plano Tangente
$$
F(x,y,z) = z - f(x,y) = 0
$$
$$
\nabla F(x,y,z) = \left( -\frac{\partial f}{\partial x}, -\frac{\partial f}{\partial y}, \frac{\partial F}{\partial z} \right) = \left( \frac{1}{5}x, \frac{4}{5}y, 1 \right)
$$

El gradiente $\nabla F$ es un vector normal a la superficie $f(x,y)$, por lo que el plano tangente a la superficie en el punto $A$ es perpendicular a este vector.

El vector normal en el punto $A$ es:
$$
\boxed{
\nabla F(1,1,\frac{1}{2}) = \left( \frac{1}{5}, \frac{4}{5}, 1 \right)
}
$$


Entonces el plano tangente es:

$$
\nabla F(a) \cdot (x - a) = 0
$$
$$
\left( \frac{1}{5}a_1, \frac{4}{5}a_2, 1 \right) \cdot (x - a_1, y - a_2, z - a_3) = 0
$$

El punto es $A = (1,1,\frac{1}{2})$, entonces:
$$
\left( \frac{1}{5}, \frac{4}{5}, 1 \right) \cdot (x - 1, y - 1, z - \frac{1}{2}) = 0
$$  
El plano tangente a la superficie en el punto $A$ es:
$$
\frac{1}{5}(x - 1) + \frac{4}{5}(y - 1) + 1(z - \frac{1}{2}) = 0
$$
$$
\boxed{
\frac{1}{5}x + \frac{4}{5}y + z - \frac{3}{2} = 0
}
$$


# Interpretación geométrica del gradiente
Intuitivamente, el gradiente en un punto de una función de varias variables apunta en la dirección de mayor aumento de la función. En el ejemplo, el gradiente en el punto $P(1,1)$ apunta hacia la dirección donde la función $f$ aumenta más rápidamente. El plano tangente en ese punto es perpendicular al gradiente, lo que significa que cualquier movimiento a lo largo del plano tangente no cambiará el valor de la función $f$ (es decir, permanecerá constante).

> Dado un punto $p$ queremos desplazarnos por la superficie de $f$ hacia un punto donde $f$ sea mayor. El gradiente nos indica la dirección de mayor aumento, pero para permanecer en la superficie. Tenemos que buscar un siguiente punto calcular su gradiente y repetir el proceso. Esto es lo que se llama **ascenso por gradiente**.

## Encontrar el vector sobre el plano tangente
* Sí: en $(P(1,1))$, la curva de nivel es $(f(x,y)=0.5)$, porque
  $$
  f(1,1)=1-\tfrac{1}{10}(1+4)=0.5.
  $$
* El gradiente es perpendicular a la curva de nivel en ese punto.
* El gradiente apunta hacia donde la función **aumenta más rápido** (hacia curvas de nivel más altas).

---

## El punto clave: NO “saltas” entre curvas de nivel

La idea de “ir a la curva de nivel 0.6 y buscar su punto” es donde aparece el error conceptual.

Las curvas de nivel no son escalones por los que vas saltando, sino **infinitas superficies continuas muy cercanas entre sí**.

Entre $(0.5)$ y $(0.6)$ hay infinitas curvas de nivel:
$$
0.5001,; 0.5002,; 0.5003,;\dots
$$

Entonces no existe “el siguiente punto” único en la curva 0.6 que te diga por dónde ir.

## Lo que realmente haces: movimiento local (gradiente continuo)

El ascenso por gradiente es un proceso **local e incremental**:

En vez de saltar, haces esto:

$$
(x_{k+1},y_{k+1}) = (x_k,y_k) + \alpha \nabla f(x_k,y_k)
$$

donde:

* $(\alpha)$ es un paso pequeño,
* $(\nabla f(x_k,y_k))$ se recalcula en cada punto.

Esto produce:

* un movimiento continuo,
* que cruza curvas de nivel una tras otra,
* sin “teletransportarse” a ninguna.

---

## Interpretación geométrica correcta

* El gradiente en cada punto te dice: “da un paso corto en esta dirección”.
* Ese paso te lleva a una región donde $(f)$ es un poco mayor.
* Allí el gradiente cambia, porque la superficie cambió.
* Repites.

Resultado: una **trayectoria curva**, no una secuencia de saltos entre niveles fijos.

---

