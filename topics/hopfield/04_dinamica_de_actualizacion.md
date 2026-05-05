# Red de Hopfield: Pasos a seguir para implementar
Se lo hace de forma iterativa, siguiendo estos pasos:
1. Definir los patrones a memorizar
    - Cada patrón es un vector de tamaño $N$ con valores $-1$ o $+1$.
    - Ejemplo: 
      $$\xi^1 = [+1, -1, +1, -1],\quad \xi^2 = [-1, +1, -1, +1], \quad etc.$$
2. Construir la matriz de pesos

    Se usa la regla de aprendizaje de John Hopfield (tipo Hebb):
3. Ingresar un patrón inicial
    - Puede ser el patrón original o una versión ruidosa (con algunos bits cambiados).
4. Actualizar la red iterativamente usando la regla de actualización:
    - Para cada neurona $i$, se calcula su nuevo estado basado en la suma ponderada de las entradas:
      $$S_i(t+1) = \text{sgn}\Bigg( \sum_j w_{ij} S_j(t) \Bigg)$$
5. Iterar hasta convergencia
    - Repetís el paso anterior hasta que el vector no cambie más
    - Ese estado final es un atractor (memoria almacenada
     
6. (Opcional) Función de energía 
    - En cada actualización, la energía baja o se mantiene
    - Garantiza convergencia (sin ciclos raros en modo asíncrono)
    
    
## **1. Definir los patrones a memorizar**   
    
Patrón es un vector: $\xi^{\mu} \in \mathbb{R}^{N \times 1}$,

$$
\mathbf{\xi}^\mu =
\begin{bmatrix}
\xi_1^\mu \\
\xi_2^\mu \\
\vdots \\
\xi_N^\mu
\end{bmatrix} \quad \text{donde } \xi_i^\mu \in \{-1, +1\}
$$

- $\mu = (1, 2, ..., p) \rightarrow$ índice del patrón
- $i = (1, 2, ..., N) \rightarrow$ índice de la neurona


## Construir la matriz de pesos 
Regla de Hebb con varios patrones:

$$
w_{ij} = \frac{1}{N} \sum_{\mu=1}^{p} \xi_i^\mu \xi_j^\mu \quad \quad (2.9)
$$

- $w_{ii} = 0$ para evitar autoconexiones.
- $w_{ij} = w_{ji}$ → pesos simétricos.

Matriz de pesos simétrica:

$$
W = 
\begin{bmatrix}
0 & w_{12} & \cdots & w_{1N} \\
w_{12} & 0 & \cdots & w_{2N} \\
\vdots & \vdots & \ddots & \vdots \\
w_{1N} & w_{2N} & \cdots & 0
\end{bmatrix}
$$

## Actualización de la red
La actualización de la red se hace con la función signo:
$$
S_i(t+1) = \text{sgn}\Bigg( \sum_{j=1}^N w_{ij} S_j(t) \Bigg)
$$


- La dimensión de $\mathbf{S(t)}$ es $N \times 1$, entonces:

    $$
    \mathbf{S(t)} =
    \begin{bmatrix}
    S_1(t) \\
    S_2(t) \\
    \vdots \\
    S_N(t)
    \end{bmatrix} \quad \text{donde } S_i(t) \in \{-1, +1\}
    $$

# Forma matricial Red de Hopfield
Forma matricial:

$$
W = \frac{1}{N} \sum_{\mu=1}^{p} \xi^\mu (\xi^\mu)^T
$$
- Donde $W$ es la matriz de pesos simétrica con $W_{ii} = 0$. Para evitar autoconexiones.
- $W \in \mathbb{R}^{N \times N}$,

> Nota: [Producto exterior](https://en.wikipedia.org/wiki/Outer_product).
> Si tengo $u \in \mathbb{R}^{m \times 1}$, y $v \in \mathbb{R}^{n \times 1}$, entonces el producto exterior:
> $$
> u \otimes v = u v^T \in \mathbb{R}^{m \times n}
> $$

# Sicronica vs Asincronica
- Síncrona: Todas las neuronas se actualizan al mismo tiempo usando el estado actual.
- Asíncrona: Se actualiza una neurona a la vez, elegida al azar.

| | Síncrona |  Asíncrona |
|-----------------------|-----------------------|-------------------------|
| elemento a elemento | Sí | Sí |
| matricial | Sí | No |

- ✔ Asincrónica:
  - cada cambio es local
  - garantiza ↓ energía en cada paso
  - converge siempre
- ⚠ Sincrónica:
  - todas las neuronas cambian a la vez
  - pueden “sobrecorregirse”
  - puede aparecer un ciclo:

      $$ S(t)→S(t+1)→S(t) $$

  👉 energía no necesariamente decrece monótonamente


