# Apendice
## 1. Deduccion: Crosstalk y capacidad máxima de la red de Hopfield

Cuando guardas varios patrones en una red de Hopfield usando la **regla de Hebb**, cada patrón “empuja” los pesos en cierta dirección.

El problema es que:

*   el patrón que quieres recordar aporta una señal **útil**
*   los demás patrones meten una interferencia llamada **crosstalk** (o “diafonía”)

Entonces el campo local de una neurona $i$ en el patrón $v$ queda, conceptualmente, así:

$$
h_i^v = \text{señal deseada} + \text{crosstalk}
$$

Si el crosstalk es demasiado grande, puede hacer que un bit que debería quedarse en $+1$ o $-1$ **se vuelva inestable** y cambie de signo.


### 1.1 Fórmula del campo local

Si los pesos se construyen con Hebb:

$$
w_{ij}=\frac{1}{N}\sum_{\mu=1}^{p}\xi_i^\mu \xi_j^\mu,
\qquad w_{ii}=0
$$

entonces el campo local sobre la neurona $i$, evaluado en el patrón $v$, es:

$$
h_i^{\,v}=\sum_{j\neq i} w_{ij}\,\xi_j^v
$$

Sustituyendo $w_{ij}$:

$$
h_i^{\,v}
=\sum_{j\neq i}\left(\frac{1}{N}\sum_{\mu=1}^{p}\xi_i^\mu \xi_j^\mu\right)\xi_j^v
$$

Reordenando:

$$
h_i^{\,v}
=\frac{1}{N}\sum_{\mu=1}^{p}\xi_i^\mu \sum_{j\neq i}\xi_j^\mu \xi_j^v
$$

***

### 1.2 Separación en “señal deseada + crosstalk”

Se separa el término $\mu=v$ de los demás:

$$
h_i^{\,v}
=
\underbrace{\frac{1}{N}\xi_i^v \sum_{j\neq i}\xi_j^v\xi_j^v}_{\text{señal deseada}}
+
\underbrace{\frac{1}{N}\sum_{\mu\neq v}\xi_i^\mu \sum_{j\neq i}\xi_j^\mu \xi_j^v}_{\text{crosstalk}}
$$

Como $\xi_j^v\xi_j^v=1$, la primera parte queda aproximadamente:

$$
\frac{N-1}{N}\xi_i^v \approx \xi_i^v
$$

Entonces:

$$
h_i^{\,v}\approx \xi_i^v + C_i^{\,v}
$$

donde el **crosstalk** es

$$
\boxed{
C_i^{\,v}
=
\frac{1}{N}\sum_{\mu\neq v}\xi_i^\mu \sum_{j\neq i}\xi_j^\mu \xi_j^v
}
$$

***

### 1.3 Esa es la fórmula del crosstalk

En forma compacta, la fórmula que normalmente se usa es:

$$
\boxed{
C_i^{\,v}
=
\frac{1}{N}\sum_{\mu\neq v}\sum_{j\neq i}
\xi_i^\mu \xi_j^\mu \xi_j^v
}
$$

Las dos expresiones son equivalentes.

***

### 1.4 Interpretación

*   $v$: patrón que quieres recordar
*   $\mu \neq v$: todos los **otros** patrones almacenados
*   $C_i^{\,v}$: cuánto interfieren esos otros patrones en el bit $i$ del patrón $v$

Si:

*   $C_i^{\,v}<1$, en principio el bit puede seguir estable
*   $C_i^{\,v}>1$, la interferencia puede invertir el signo del campo local y volverlo inestable

Por eso en el texto aparece:

$$
P_{\text{error}}=\Pr(C_i^{\,v}>1)
$$

***

### 1.5 Con la convención del libro

Muchos libros escriben:

$$
h_i^{\,v}=\xi_i^v(1-C_i^{\,v})
$$

o una forma equivalente donde el crosstalk ya está “normalizado” respecto del signo de $\xi_i^v$.  
En ese caso puede aparecer una definición como:

$$
\boxed{
C_i^{\,v}
=
-\frac{1}{N}\sum_{\mu\neq v}\sum_{j\neq i}
\xi_i^\mu \xi_i^v \xi_j^\mu \xi_j^v
}
$$

o sin el signo menos, dependiendo de cómo reagrupan la expresión.


Hay **pequeñas variaciones de signo o notación** según el autor, pero la idea es siempre la misma:

> **el crosstalk es la suma de la contribución de todos los otros patrones al campo local del bit que quieres recuperar.**

***

### 1.6 Fórmula más usada para recordar

Si te lo preguntan de manera directa, la forma más estándar es:

$$
\boxed{
C_i^{\,v}
=
\frac{1}{N}\sum_{\mu\neq v}\sum_{j\neq i}
\xi_i^\mu \xi_j^\mu \xi_j^v
}
$$

y entonces

$$
h_i^{\,v}\approx \xi_i^v + C_i^{\,v}
$$

***

### 1.7 Respuesta corta “tipo examen”

> En una red de Hopfield, el término de crosstalk para el bit $i$ del patrón $v$ es la contribución de todos los patrones $\mu\neq v$ al campo local:
>
> $$
> C_i^{\,v}=\frac{1}{N}\sum_{\mu\neq v}\sum_{j\neq i}\xi_i^\mu \xi_j^\mu \xi_j^v
> $$
>
> De este modo, el campo local puede escribirse como señal deseada más interferencia:
>
> $$
> h_i^{\,v}\approx \xi_i^v + C_i^{\,v}
> $$
>
> y si el crosstalk es demasiado grande, el bit se vuelve inestable.

***