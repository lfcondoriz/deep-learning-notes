# Polinomio de Taylor
Teorema de Taylor. Sea $k \in \mathbb{N}$ y $f: \mathbb{R} \to \mathbb{R}$ una función diferenciable $k$ veces en el punto $a \in \mathbb{R}$. Entonces existe un función $h_k: \mathbb{R} \to \mathbb{R}$ tal que:

$$
f(x) = f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + \ldots + \frac{f^{(k)}(a)}{k!}(x-a)^k + h_k(x)(x-a)^k
$$

con $\lim_{x \to a} h_k(x) = 0$. Esta es la llamada forma de Peano del resto.

# Taylor para varias variables

Para varias variables la idea es la misma, pero en lugar de potencias $(x-a)^k$ aparecen productos entre las componentes y derivadas parciales de orden superior.

Sea $f:\mathbb{R}^n \to \mathbb{R}$ una función con derivadas parciales continuas hasta orden $k$ en un punto $a \in \mathbb{R}^n$. Entonces el **polinomio de Taylor de orden $k$** alrededor de $a$ se escribe:

$$f(x) = \sum_{|\alpha|\le k} \frac{D^\alpha f(a)}{\alpha!}(x-a)^\alpha + R_k(x)$$

donde:

* $\alpha = (\alpha_1,\dots,\alpha_n)$ es un **multiíndice**
* $|\alpha| = \alpha_1 + \cdots + \alpha_n$
* $\alpha! = \alpha_1!\cdots \alpha_n!$
* $D^\alpha f = \frac{\partial^{|\alpha|} f}{\partial x_1^{\alpha_1}\cdots \partial x_n^{\alpha_n}}$
* $(x-a)^\alpha = (x_1-a_1)^{\alpha_1}\cdots(x_n-a_n)^{\alpha_n}$

## Forma de Peano (resto)

El resto se puede escribir como:

$$R_k(x) = o(|x-a|^k) \quad \text{cuando } x \to a$$

o, de forma análoga a lo que pusiste:

$$R_k(x) = h_k(x)|x-a|^k \quad \text{con } \lim_{x\to a} h_k(x)=0$$

### Ejemplo en $\mathbb{R}^2$ (más intuitivo)

Para $f(x,y)$, el desarrollo hasta orden 2 en $(a,b)$ es:

$$
\begin{aligned}
f(x,y) \approx &\ f(a,b) + f_x(a,b)(x-a) + f_y(a,b)(y-b) \\
&+ \frac{1}{2}\Big[f_{xx}(a,b)(x-a)^2 + 2f_{xy}(a,b)(x-a)(y-b) + f_{yy}(a,b)(y-b)^2\Big]
\end{aligned}
$$

más un resto $o(|(x-a,y-b)|^2)$.

### Ejemplo Ordén 1: Con gradiente
Sí, para **orden 1** definitivamente queda mucho más claro usar el gradiente. De hecho, es la forma más intuitiva de escribir el Taylor en varias variables.

Para $(f:\mathbb{R}^n \to \mathbb{R})$, el desarrollo de primer orden en (a) se escribe:

$$
f(x) = f(a) + \nabla f(a)\cdot (x-a) + o(|x-a|)
$$

donde:

* $(\nabla f(a))$ es el **gradiente** (vector de derivadas parciales)
* $(\nabla f(a)\cdot (x-a))$ es un **producto escalar**

👉 Esto es básicamente la “recta tangente” en varias variables (en realidad, el plano tangente).


### Ejemplo Ordén 2: Con Hessiana

Ahí el gradiente solo no alcanza, pero podés usar la **matriz Hessiana** (que también es bastante entendible):

$$
f(x) = f(a) + \nabla f(a)\cdot (x-a) + \frac{1}{2}(x-a)^T H_f(a)(x-a) + o(|x-a|^2)
$$

donde:

* $(H_f(a))$ es la **Hessiana** (matriz de segundas derivadas)
* $((x-a)^T H_f(a)(x-a))$ es una forma cuadrática





# Taylor + Cauchy-Schwarz
## 1. Partimos de Taylor (lo que ya escribiste)
    
Si estás en $( x_k ) \in \mathbb{R}^n $ y tomás un paso $( h ) \in \mathbb{R}^n $, entonces:

$$
f(x_k + h) \approx f(x_k) + \nabla f(x_k)\cdot h
$$

---

## 2. ¿Qué queremos?

Elegir $( h )$ tal que $( f(x_k + h) )$ aumente lo más posible.

Como usamos la aproximación:

$$
\text{maximizar } \nabla f(x_k)\cdot h
$$

pero con una restricción razonable:

$$
|h| = \alpha \quad (\text{paso chico})
$$

---

## 3. Resolviendo ese problema

Por Cauchy-Schwarz:

$$
\nabla f(x_k)\cdot h \leq |\nabla f(x_k)| \,|h|
$$

La igualdad ocurre cuando (son paralelos):

$$
h \parallel \nabla f(x_k)
$$
    

o sea:

$$
h = \alpha \,\nabla f(x_k)
$$




### 🔹Explicacion mas detallada: El problema que queremos resolver

Queremos elegir $( h )$ tal que:

$$
\nabla f(x_k)\cdot h
$$

sea lo más grande posible, **pero sin que ( h ) sea arbitrariamente grande**.

Entonces imponemos:

$$
|h| = \alpha
$$

(o sea: todos los vectores con longitud fija).

---

### 🔹 ¿Qué significa geométricamente?

Tenés:

* un vector fijo: $( \nabla f(x_k) )$
* un vector variable: $( h )$ (pero con longitud fija)

Y querés maximizar el producto escalar:

$$
\nabla f \cdot h
$$

---

##$ 🔹 Fórmula clave del producto escalar

Recordá:

$$
\nabla f \cdot h = |\nabla f| \, |h| \cos\theta
$$

donde $( \theta )$ es el ángulo entre los vectores.

---

## 🔹 Ahora sí: Cauchy-Schwarz (en versión simple)

Dice:

$$
\nabla f \cdot h \leq |\nabla f| \, |h|
$$

y la igualdad ocurre cuando los vectores son **paralelos**.

---

### 🔹 Aplicado a nuestro caso

Como ( |h| = \alpha ), entonces:

$$
\nabla f \cdot h \leq |\nabla f| \, \alpha
$$

👉 El máximo posible ocurre cuando:

$$
\cos\theta = 1 ;\Rightarrow; \theta = 0
$$

o sea:

$$
h \text{ tiene la misma dirección que } \nabla f
$$

---

### 🔹 Entonces, ¿cómo es ( h )?

Si tiene:

* dirección del gradiente
* norma $( \alpha )$

entonces:

$$
h = \alpha \frac{\nabla f(x_k)}{|\nabla f(x_k)|}
$$

---

##$ 🔹 ¿Por qué a veces aparece sin normalizar?

Muchas veces se escribe simplemente:

$$
h = \alpha \nabla f(x_k)
$$

porque:

* $( \alpha )$ ya controla el tamaño del paso
* no importa separar dirección y norma explícitamente

---

### 🔹 Idea intuitiva final

Entre todos los vectores de longitud fija:

👉 el que más hace crecer la función
es el que apunta **exactamente en la dirección del gradiente**

---

### 🧠 Resumen claro

* Querés maximizar: $( \nabla f \cdot h )$
* Con: $( |h| = \alpha )$
* El producto escalar depende del ángulo
* Se maximiza cuando $( h )$ y $( \nabla f )$ son paralelos

➡️ Por eso elegís:
$$
h \propto \nabla f
$$

## 4. Sustituís el paso

Entonces:

$$
x_{k+1} = x_k + h = x_k + \alpha \nabla f(x_k)
$$

👉 Y ahí aparece exactamente la fórmula que escribiste.

---

## 5. Idea clave (muy importante)

* Taylor te da una **aproximación local**
* Vos elegís $( h )$ para **mejorar lo máximo posible esa aproximación**
* Eso produce naturalmente la dirección del gradiente

---

## 6. Interpretación final

No es que la fórmula “sale sola” de Taylor, sino que:

> Taylor te dice cómo cambia la función
>
> * optimización local te dice cómo elegir el paso

👉 Y la combinación de ambas cosas produce:

$$
x_{k+1} = x_k + \alpha \nabla f(x_k)
$$













# Euler explícito

Es el método numérico más simple para resolver ecuaciones diferenciales.

Si tenés una ecuación como:

$$
\frac{dx}{dt} = g(x)
$$

Euler explícito dice:

$$
x_{k+1} = x_k + h \, g(x_k)
$$

donde:

* $( h )$ = tamaño de paso (como tu $( \alpha )$)
* usás la pendiente **en el punto actual** (por eso “explícito”)

---

## 🔗 Conexión con el gradiente

En tu caso:

$$
\frac{d}{dt}(x,y) = \nabla f(x,y)
$$

Euler explícito te da:

$$
(x_{k+1}, y_{k+1}) = (x_k, y_k) + \alpha \nabla f(x_k,y_k)
$$

👉 O sea:
**el gradiente ascendente ES Euler explícito aplicado a esa ecuación diferencial.**

No es coincidencia — es exactamente lo mismo.












# Taylor + Cauchy-Schwarz vs Euler explícito

## Lo que hiciste con Taylor + Cauchy-Schwarz

Eso responde a esta pregunta:

> “Si doy un paso chico, ¿en qué dirección conviene ir?”

Y la respuesta fue:

$$
h \propto \nabla f(x_k)
$$

👉 Esto es un **problema de optimización local**.

* Usás Taylor → aproximás cómo cambia ( f )
* Elegís ( h ) → para maximizar ese cambio

Resultado:

$$
x_{k+1} = x_k + \alpha \nabla f(x_k)
$$

---

## 🔹 2. ¿Qué es Euler explícito?

Eso responde a otra pregunta:

> “¿Cómo resuelvo una ecuación diferencial?”

Si tenés:

$$
\frac{dx}{dt} = g(x)
$$

Euler dice:

$$
x_{k+1} = x_k + h, g(x_k)
$$

👉 Es un **método numérico**, no un argumento geométrico.

---

## 3. ¿Dónde se conectan?

La conexión aparece cuando decís:

> “Quiero moverme SIEMPRE en dirección del gradiente”

Eso se escribe como ecuación diferencial:

$$
\frac{dx}{dt} = \nabla f(x)
$$

---

Ahora aplicás Euler a esa ecuación:

$$
x_{k+1} = x_k + \alpha \nabla f(x_k)
$$

👉 ¡Y aparece exactamente la misma fórmula!

---

## 🔥 4. Entonces, ¿son lo mismo?

❌ No son lo mismo conceptualmente
✔️ Pero producen la **misma regla de actualización**

---

## 🧠 Diferencia clave

| Enfoque         | Qué estás haciendo              |
| --------------- | ------------------------------- |
| Taylor + Cauchy | Elegís la mejor dirección local |
| Euler explícito | Aproximás una dinámica continua |

---

## 🔗 Cómo se encadenan (esto es lo importante)

1. Taylor → cómo cambia ( f ) localmente
2. Optimización → elegís dirección = gradiente
3. Idea física → “me muevo siguiendo el gradiente”
4. Eso define:
   $$
   \frac{dx}{dt} = \nabla f(x)
   $$
5. Euler → discretiza eso
6. Resultado:
   $$
   x_{k+1} = x_k + \alpha \nabla f(x_k)
   $$








































































---

# Ecuaciones diferenciales

Si decís:

> “me muevo siempre en dirección del gradiente”

eso en continuo significa:

$$
\frac{dx}{dt} = \text{componente x del gradiente}
$$
$$
\frac{dy}{dt} = \text{componente y del gradiente}
$$

En tu función:

$$
\nabla f = \left(-\frac{1}{5}x,; -\frac{4}{5}y\right)
$$

entonces:

$$
\frac{dx}{dt} = -\frac{1}{5}x
$$
$$
\frac{dy}{dt} = -\frac{4}{5}y
$$

---

## ¿Por qué se puede dividir $( \frac{dy}{dt} / \frac{dx}{dt} )$?

Esta es la parte que te generó duda, y está perfecto.

La idea es esta:

Si $( x(t) )$ y $( y(t) )$ dependen del mismo parámetro $( t )$, entonces:

$$
\frac{dy}{dx} = \frac{dy/dt}{dx/dt}
$$

👉 Esto viene de la regla de la cadena:

$$
\frac{dy}{dx} = \frac{dy}{dt} \cdot \frac{dt}{dx}
$$

pero:

$$
\frac{dt}{dx} = \frac{1}{dx/dt}
$$

entonces:

$$
\frac{dy}{dx} = \frac{dy/dt}{dx/dt}
$$

---

## 🔹 Intuición

* $( \frac{dy}{dt} )$: qué tan rápido cambia $( y )$
* $( \frac{dx}{dt} )$: qué tan rápido cambia $( x )$

👉 Dividirlos te dice:

> cuánto cambia $( y )$ **por cada cambio en $( x )$**

---

## 🔹 4. Aplicado a tu caso

Tenías:

$$
\frac{dx}{dt} = -\frac{1}{5}x
$$
$$
\frac{dy}{dt} = -\frac{4}{5}y
$$

Entonces:

$$
\frac{dy}{dx} = \frac{-\frac{4}{5}y}{-\frac{1}{5}x}
= \frac{4y}{x}
$$

👉 Esto elimina el tiempo $( t )$ y te deja solo la trayectoria en el plano.

---

## 🔹 5. ¿Por qué hacemos esto?

Porque queremos la **forma del camino**, no cómo se recorre en el tiempo.

* Con ( t ): tenés velocidad
* Sin ( t ): tenés la curva geométrica

---

## 🧠 Resumen claro

* **Euler explícito** = versión discreta:
  $$
  x_{k+1} = x_k + \alpha \nabla f(x_k)
  $$

* **Ecuación diferencial** = versión continua:
  $$
  \frac{d}{dt}x = \nabla f(x)
  $$

* **Dividir derivadas**:
  $$
  \frac{dy}{dx} = \frac{dy/dt}{dx/dt}
  $$
  sirve para eliminar el tiempo y obtener la trayectoria.

---

Si querés, en el siguiente paso puedo mostrarte **cómo resolver paso a paso** la ecuación
$$
\frac{dy}{dx} = \frac{4y}{x}
$$
para que veas exactamente de dónde sale $( y = x^4 )$ sin saltos.















# Ecuacion diferencial: donde sale $t$
Muy buena duda — acá estás tocando el punto más sutil de todo:
**¿de dónde aparece ese ( t )?**

La respuesta corta es:

> $( t )$ no “sale” de la función $( f )$.
> **Lo introducimos nosotros** para describir un movimiento.

## 🔹 1. Qué es $( f(x,y) )$

Tenés una función:

$$
f(x,y) = 1 - \tfrac{1}{10}(x^2 + 4y^2)
$$

Esto es solo una **superficie**.
No hay tiempo, no hay movimiento.

---

## 🔹 2. Queremos describir un movimiento

Decís:

> “quiero moverme siguiendo el gradiente”

Eso significa:

* ya no pensás en un punto fijo,
* sino en una **trayectoria**.

Entonces definís:

$$
x = x(t), \quad y = y(t)
$$

👉 $( t )$ es solo un **parámetro** (podés pensar “tiempo”, pero no necesariamente físico).

---

## 🔹 3. ¿Qué significa “seguir el gradiente”?

Significa:

> en cada instante, la velocidad es el gradiente

O sea:

$$
\left(\frac{dx}{dt}, \frac{dy}{dt}\right) = \nabla f(x(t), y(t))
$$

---

## 🔹 4. Ahora sí aparece tu ecuación

Como:

$$
\nabla f(x,y) = \left(-\frac{1}{5}x\,; -\frac{4}{5}y\right)
$$

reemplazás $( x = x(t), y = y(t) )$:

$$
\frac{dx}{dt} = -\frac{1}{5}x(t)
$$
$$
\frac{dy}{dt} = -\frac{4}{5}y(t)
$$

👉 Eso define cómo evoluciona la trayectoria.

---

## 🔹 5. Comparación con tu ejemplo

Dijiste:

> “¿sería como poner ( x = t ), entonces ( y = t^2+1 )?”

Eso es **parecido pero distinto**.

## Tu ejemplo:

$$
y = x^2 + 1
$$

Podés parametrizar:

$$
x = t,\quad y = t^2 + 1
$$

👉 Eso describe una curva fija.

---

## En el gradiente:

No tenés la curva de antemano.

En cambio:

$$
\frac{dx}{dt}, \frac{dy}{dt}
$$

te dicen **cómo construirla dinámicamente**.

---

## 🔥 Diferencia clave

| Caso            | Qué hacés                            |
| --------------- | ------------------------------------ |
| $( y = x^2 + 1 )$ | la curva ya está dada                |
| Gradiente       | la curva se define por cómo te movés |

---

## 🔹 6. Intuición fuerte (esto lo aclara todo)

Pensalo así:

* $( f(x,y) )$ = montaña
* $( \nabla f )$ = flechas en cada punto
* $( t )$ = “tiempo mientras caminás”

👉 Entonces:

$$
(x(t), y(t)) = \text{tu posición mientras caminás}
$$

y

$$
(x'(t), y'(t)) = \text{tu velocidad}
$$

y la regla es:

> “tu velocidad es igual a la flecha (gradiente) en ese punto”

---

## 🧠 Resumen final

* $( t )$ no estaba en la función → lo introducís vos
* sirve para describir una trayectoria
* convertís puntos en funciones: $( x(t), y(t) )$
* imponés la regla:
  $$
  (x',y') = \nabla f
  $$
* eso genera una ecuación diferencial
