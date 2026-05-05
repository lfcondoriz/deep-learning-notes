# Producto exterior (`outer`)

$$p \otimes p = p \cdot p^T$$

* Esto produce una **matriz NxN**, donde el elemento $(i,j)$ es:

$$(p p^T)_{ij} = p_i \cdot p_j$$

* Cada componente del vector **se multiplica con cada componente del otro vector**.
* La matriz resultante **NO es un escalar**, es una **matriz completa**.

---

# Producto escalar (dot product)

* El **producto escalar** entre dos vectores $a$ y $b$ se denota $a \cdot b$ o `np.dot(a, b)`
* Devuelve **un número (escalar)**, no una matriz:

$$a \cdot b = \sum_{i=1}^{N} a_i b_i$$

* Dimensiones en términos de matrices: $1 \times N \cdot N \times 1 = 1 \times 1$ → escalar.

---

## Diferencia clave

| Operación         | Notación NumPy  | Resultado | Dimensiones | Comentario                                |
| ----------------- | --------------- | --------- | ----------- | ----------------------------------------- |
| Producto escalar  | `np.dot(a,b)`   | Escalar   | 1x1         | Suma de productos de componentes          |
| Producto exterior | `np.outer(a,b)` | Matriz    | NxN         | Cada $(i,j) = a_i b_j$, **no es escalar** |

