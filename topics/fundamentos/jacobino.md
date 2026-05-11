# Jacobino
En cálculo vectorial, la matriz jacobiana de una función vectorial de varias variables es la matriz cuyos elementos son las derivadas parciales de primer orden de dicha función. Si esta matriz es cuadrada, su determinante se llama el determinante jacobiano.

Tanto la matriz jacobiana como el determinante jacobiano reciben su nombre en honor al matemático Carl Gustav Jacobi.

# Definición formal
Sea $f: \mathbb{R}^n \to \mathbb{R}^m$ una función cuyas derivadas parciales de primer orden existen en todo $\mathbb{R}^n$ y denotamos $f(x) = (f_1(x), f_2(x), \ldots, f_m(x))$ a sus componentes escalares. Se define la matriz jacobiana de $f$ en un punto $x \in \mathbb{R}^n$ como la matriz $J_f(x) \in \mathbb{R}^{m \times n}$ dada por:
$$
J_f(x) = \begin{bmatrix}
\frac{\partial f_1}{\partial x_1} & \frac{\partial f_1}{\partial x_2} & \cdots & \frac{\partial f_1}{\partial x_n} \\
\frac{\partial f_2}{\partial x_1} & \frac{\partial f_2}{\partial x_2} & \cdots & \frac{\partial f_2}{\partial x_n} \\
\vdots & \vdots & \ddots & \vdots \\
\frac{\partial f_m}{\partial x_1} & \frac{\partial f_m}{\partial x_2} & \cdots & \frac{\partial f_m}{\partial x_n}
\end{bmatrix}
=
\begin{bmatrix}
\nabla f_1(x) \\
\nabla f_2(x) \\
\vdots \\
\nabla f_m(x)
\end{bmatrix}
$$

Donde $\nabla f_i(x)$ es el gradiente de la función escalar $f_i$ en el punto $x$.

Dimensiones del Jacobiano:
- Si $f: \mathbb{R}^n \to \mathbb{R}^m$, entonces $J_f(x) \in \mathbb{R}^{m \times n}$.

## Ejemplo
Si $f: \mathbb{R}^2 \to \mathbb{R}^3$.

$$
\mathbf{F}(x_1,x_2) = (f_1(x_1,x_2), f_2(x_1,x_2), f_3(x_1,x_2))
$$

$$
J_f(x_1,x_2) =
\begin{bmatrix}
\frac{\partial f_1}{\partial x_1} & \frac{\partial f_1}{\partial x_2} \\
\frac{\partial f_2}{\partial x_1} & \frac{\partial f_2}{\partial x_2} \\
\frac{\partial f_3}{\partial x_1} & \frac{\partial f_3}{\partial x_2}
\end{bmatrix}
$$

Notación alternativa:
$$
J_f(x_1,x_2) = \frac{\partial (f_1,f_2,f_3)}{\partial (x_1,x_2)} = \frac{\partial \mathbf{F}}{\partial \mathbf{x}}
\quad\quad\quad J_f \in \mathbb{R}^{3 \times 2}
$$