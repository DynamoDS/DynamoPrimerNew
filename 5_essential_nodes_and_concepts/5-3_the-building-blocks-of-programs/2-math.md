# Matemáticas

Si la forma más sencilla de datos es el número, la forma más sencilla de relacionarlos es mediante las matemáticas. Desde operadores sencillos como la división a funciones trigonométricas y fórmulas más complejas, las matemáticas son un método excelente para empezar a explorar relaciones y patrones numéricos.

### Operadores aritméticos

Los operadores son un conjunto de componentes que utilizan funciones algebraicas con dos valores numéricos de entrada que dan como resultado un valor de salida (suma, resta, multiplicación, división, etc.). Estos se pueden encontrar en Operators > Actions.

| Icono                                                  | Nombre (sintaxis)     | Entradas                     | Salidas      |
| ----------------------------------------------------- | ----------------- | -------------------------- | ------------ |
| ![](<../images/5-1/addition(1)(1) (1) (1).jpg>)       | Sumar (**+**)       | var[]...[], var[]...[] | var[]...[] |
| ![](<../images/5-1/Subtraction(1)(1) (1) (1).jpg>)    | Restar (**-**)  | var[]...[], var[]...[] | var[]...[] |
| ![](<../images/5-1/Multiplication(1)(1) (1) (1).jpg>) | Multiplicar (**\***) | var[]...[], var[]...[] | var[]...[] |
| ![](<../images/5-1/Division(1)(1) (1) (1).jpg>)       | Dividir (**/**)    | var[]...[], var[]...[] | var[]...[] |

## Ejercicio: la fórmula de la espiral dorada

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/5-3/2/Building Blocks of Programs - Math.dyn" %}

### Parte I: fórmula paramétrica

Combine operadores y variables para formar una relación más compleja mediante **fórmulas**. Utilice los controles deslizantes para crear una fórmula que se pueda manejar con parámetros de entrada.

1\. Cree la secuencia numérica que representa la "t" de la ecuación paramétrica, por lo que debemos utilizar una lista lo suficientemente grande como para definir una espiral.

**Number Sequence**: defina una secuencia de números basada en tres entradas: _start, amount_ y _step_.

![](../images/5-3/2/math-partI-01.jpg)

2\. En el paso anterior, hemos creado una lista de números para definir el dominio paramétrico. A continuación, cree un grupo de nodos que representen la ecuación de la espiral dorada.

La espiral dorada se define como la siguiente ecuación:

$$
x = r cos θ = a cos θ e^{bθ}
$$

$$
y = r sin θ = a sin θe^{bθ}
$$

La imagen siguiente representa la espiral dorada en forma de programación visual. Al desplazarse por el grupo de nodos, intente prestar atención al paralelismo entre el programa visual y la ecuación por escrito.

![](../images/5-3/2/math-partI-02.jpg)

> a. **Number Slider**: añada dos controles deslizantes de número al lienzo. Estos controles deslizantes representarán las variables _a_ y _b_ de la ecuación paramétrica. Estas representan una constante que es flexible o parámetros que podemos ajustar hacia el resultado deseado.
>
> b. **Multiplication (\*)**: el nodo de multiplicación se representa mediante un asterisco. Lo usaremos con frecuencia para conectar variables de multiplicación.
>
> c. **Math.RadiansToDegrees**: los valores de "_t_" se deben convertir a grados para que se evalúen en las funciones trigonométricas. Recuerde que Dynamo utiliza por defecto los grados para evaluar estas funciones.
>
> d. **Math.Pow**: como función de "_t_" y del número "_e_", crea la secuencia Fibonacci.
>
> e. **Math.Cos y Math.Sin**: estas dos funciones trigonométricas distinguirán la coordenada X y la coordenada Y, respectivamente, de cada punto paramétrico.
>
> f. **Watch**: ahora vemos que las salidas son dos listas, que serán las coordenadas _x_ e _y_ de los puntos utilizados para generar la espiral.

### Parte II: de la fórmula a la geometría

El cúmulo de nodos del paso anterior funcionará correctamente, pero conlleva mucho trabajo. Para crear un flujo de trabajo más eficiente, consulte [DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/2-design-script-syntax.md) a fin de definir una cadena de expresiones de Dynamo en un nodo. En la siguiente serie de pasos, veremos cómo utilizar la ecuación paramétrica para dibujar la espiral Fibonacci.

**Point.ByCoordinates**: conecte el nodo de multiplicación superior a la entrada "_x_" y el inferior a la entrada "_y_". Ahora se muestra una espiral paramétrica de puntos en la pantalla.

![](../images/5-3/2/math-partII-01.gif)

**Polycurve.ByPoints**: conecte **Point.ByCoordinates** del paso anterior a _points_. Podemos dejar _connectLastToFirst_ sin entrada porque no estamos creando una curva cerrada. Se crea una espiral que pasa a través de cada punto definido en el paso anterior.

![](../images/5-3/2/math-partII-02.jpg)

Ya hemos completado la espiral Fibonacci. Vamos a profundizar en estos conceptos con dos ejercicios separados a partir de aquí; a uno lo llamaremos caracol y al otro girasol. Se trata de abstracciones de sistemas naturales, pero las dos aplicaciones diferentes de la espiral Fibonacci estarán bien representadas.

### Parte III: de la espiral al caracol

**Circle.ByCenterPointRadius**: utilizaremos aquí un nodo de círculo con las mismas entradas que en el paso anterior. El valor por defecto del radio es _1,0_, por lo que vemos una salida de círculos inmediata. Se hace inmediatamente evidente cómo los puntos divergen más respecto al origen.

![](../images/5-3/2/math-partIII-01.jpg)

**Number Sequence**: es la matriz original de "_t_". Al conectarlo con el valor del radio de **Circle.ByCenterPointRadius**, los centros de círculo siguen divergiendo más respecto del origen, pero el radio de los círculos aumenta, lo que crea un gráfico de círculos Fibonacci con mucho estilo.

Un premio para quién consiga convertirlo a 3D.

![](../images/5-3/2/math-partIII-02.gif)

### Parte IV: del caracol al filotaxis

Ahora que hemos hecho una concha de caracol circular, vamos a pasar a las rejillas paramétricas. Vamos a usar una rotación básica en la espiral Fibonacci para crear una rejilla Fibonacci y modelar el resultado según el [crecimiento de las semillas de girasol](https://blogs.unimelb.edu.au/sciencecommunication/2018/09/02/this-flower-uses-maths-to-reproduce/).

Como punto de partida, vamos a realizar el mismo paso del ejercicio anterior: crear una matriz de puntos de espiral con el nodo **Point.ByCoordinates**.

![](../images/5-3/2/math-partIV-01.jpg)

A continuación, siga estos pequeños pasos para generar una serie de espirales en varias rotaciones.

![](../images/5-3/2/math-partIV-02.jpg)

> a. **Geometry.Rotate**: existen varias opciones de **Geometry.Rotate**; asegúrese de que ha elegido el nodo que contiene las entradas _geometry_, _basePlane_ y _degrees_. Conecte **Point.ByCoordinates** en la entrada geometry. Haga clic con el botón derecho en este nodo y asegúrese de que el encaje se haya establecido en "Producto vectorial".
>
> <img src="../images/5-3/2/math-partIV-03crossproduct.jpg" alt="" data-size="original">
>
> b. **Plano.XY**: conéctelo a la entrada _basePlane_. Rotaremos alrededor del origen, que es la misma ubicación que la base de la espiral.
>
> c. **Number Range**: para la entrada de grados, vamos a crear varias rotaciones. Esto se puede realizar rápidamente con un componente **Number Range**. Conéctelo a la entrada _degrees_.
>
> d. **Number**: para definir el rango de números, añada tres nodos numéricos al lienzo en orden vertical. De arriba a abajo, asigne valores de _0,0; 360,0_ y _120,0_ respectivamente. Estos valores controlan la rotación de la espiral. Observe los resultados de las salidas del nodo **Number Range** después de conectar los tres nodos numéricos al mismo.

Nuestro resultado empieza a parecerse a un remolino. Ajustemos algunos de los parámetros de **Number Range** y veamos cómo cambian los resultados.

Cambie el tamaño de paso del nodo **Number Range** de _120,0_ a _36,0_. Observe que esto crea más rotaciones y que, por tanto, nos proporciona una rejilla más densa.

![](../images/5-3/2/math-partIV-04.jpg)

Cambie el tamaño de paso del nodo **Number Range** de _36,0_ a _3,6_. Esto nos da una rejilla mucho más densa y la dirección de la espiral no está clara. Damas y caballeros, hemos creado un girasol.

![](../images/5-3/2/math-partIV-05.jpg)
