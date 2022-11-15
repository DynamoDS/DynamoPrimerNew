# Functions

Las funciones se pueden crear en un bloque de código y recuperar en cualquier parte de una definición de Dynamo. Esto crea otra capa de control en un archivo paramétrico y se puede ver como una versión de texto de un nodo personalizado. En este caso, se puede acceder fácilmente al bloque de código "principal", que se encuentra en cualquier parte del gráfico. No se necesitan cables.

### Elemento principal

En la primera línea, aparece la palabra clave "def", después el nombre de la función y, a continuación, los nombres de las entradas entre paréntesis. Las llaves definen el cuerpo de la función. Se devuelve un valor con "return =". Los bloques de código que definen una función no tienen puertos de entrada o salida porque se invocan desde otros bloques de código.

![](../images/8-1/4/functionsparentdef.jpg)

```
/*This is a multi-line comment,
which continues for
multiple lines*/
def FunctionName(in1,in2)
{
//This is a comment
sum = in1+in2;
return sum;
};
```

### Elementos secundarios

Invoque la función con otro bloque de código en el mismo archivo asignando el nombre y la misma cantidad de argumentos. Funciona igual que los nodos predefinidos de la biblioteca.

![](../images/8-1/4/functionschildrencalldef.jpg)

```
FunctionName(in1,in2);
```

## Ejercicio: esfera por Z

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/8-1/4/Functions_SphereByZ.dyn" %}

En este ejercicio, crearemos una definición genérica que creará esferas a partir de una lista de entrada de puntos. El radio de estas esferas depende de la propiedad Z de cada punto.

Comencemos con un intervalo de diez valores que abarca de 0 a 100. Conecte estos nodos a un nodo **Point.ByCoordinates** para crear una línea diagonal.

![](../images/8-1/4/functions-exercise-01.jpg)

Cree un **bloque de código** e introduzca nuestra definición.

![](../images/8-1/4/functions-exercise-02.jpg)

> 1.  Utilice estas líneas de código:
>
>     ```
>     def sphereByZ(inputPt)
>     {
>
>     };
>     ```
>
> El parámetro _inputPt_ es el nombre que se le ha asignado para representar los puntos que controlarán la función. En este momento, la función no realiza ninguna acción, pero la ampliaremos en los pasos siguientes.

![](../images/8-1/4/functions-exercise-03.jpg)

> 1. Al añadir la función de **bloque de código**, se coloca un comentario y una variable _sphereRadius_ que consulta la posición _Z_ de cada punto. Recuerde que _inputPt.Z_ no necesita paréntesis como método. Se trata de una _consulta_ de las propiedades de un elemento existente, por lo que no se necesita ninguna entrada:
>
> ```
> def sphereByZ(inputPt,radiusRatio)
> {
> //get Z Value, ise ot to drive radius of sphere
> sphereRadius=inputPt.Z;
> };
> ```

![](../images/8-1/4/functions-exercise-04.jpg)

> 1. Ahora, vamos a recuperar la función que hemos creado en otro **bloque de código**. Si hacemos doble clic en el lienzo para crear un nuevo _bloque de código_ y escribimos _sphereB_, observaremos que Dynamo sugiere la función _sphereByZ_ que hemos definido. La función se ha añadido a la biblioteca de IntelliSense. ¡Genial!

![](../images/8-1/4/functions-exercise-05.jpg)

> 1.  Ahora llamamos a la función y creamos una variable denominada _Pt_ para conectar los puntos creados en los pasos anteriores:
>
>     ```
>     sphereByZ(Pt)
>     ```
> 2. En la salida, podemos observar que todos los valores son nulos. ¿Por qué ocurre eso? Al definir la función, calculamos la variable _sphereRadius_, pero no hemos definido qué debe _devolver_ la función como una _salida_. Podemos solucionar esto en el siguiente paso.

![](../images/8-1/4/functions-exercise-06.jpg)

> 1. Un paso importante es definir la salida de la función mediante la adición de la línea `return = sphereRadius;` a la función _sphereByZ_.
> 2. Ahora vemos que la salida del bloque de código nos proporciona las coordenadas Z de cada punto.

Ahora vamos a crear esferas reales mediante la edición de la función _principal_.

![](../images/8-1/4/functions-exercise-07.jpg)

> 1. Definamos primero una esfera con la siguiente línea de código: `sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);`.
> 2. A continuación, cambiamos el valor de retorno para que sea _sphere_ en lugar de _sphereRadius_: `return = sphere;`. Esto nos proporciona unas esferas gigantes en la vista preliminar de Dynamo.

![](../images/8-1/4/functions-exercise-08.jpg)

> 1\. Para reducir el tamaño de estas esferas, actualicemos el valor de sphereRadius mediante la adición de un divisor: `sphereRadius = inputPt.Z/20;`. Ahora podemos ver las esferas separadas y empezar a entender la relación entre el radio y el valor Z.

![](../images/8-1/4/functions-exercise-09.jpg)

> 1. En el nodo **Point.ByCoordinates**, al cambiar el encaje de Más corto a Producto vectorial, creamos una rejilla de puntos. La función _sphereByZ_ sigue estando totalmente activa, por lo que todos los puntos crean esferas con radios basados en valores Z.

![](../images/8-1/4/functions-exercise-10.jpg)

> 1. Y, solo para tantear el terreno, conectamos la lista original de números a la entrada X de **Point.ByCoordinates**. Ahora tenemos un cubo de esferas.
> 2. Nota: Si el cálculo tarda mucho en completarse en el equipo, pruebe a cambiar el valor _\#10_ por un valor como _\#5_.

Recuerde que la función _sphereByZ_ que hemos creado es una función genérica, por lo que podemos recuperar la hélice de una lección anterior y aplicarle la función.

![](../images/8-1/4/functions-exercise-11.jpg)

Como paso final, vamos a controlar la relación de radio con un parámetro definido por el usuario. Para ello, debemos crear una entrada nueva para la función y reemplazar el divisor _20_ por un parámetro.

![](../images/8-1/4/functions-exercise-12.jpg)

> 1.  Actualice la definición de _sphereByZ_ a:
>
>     ```
>     def sphereByZ(inputPt,radiusRatio)
>     {
>     //get Z Value, use it to drive radius of sphere
>     sphereRadius=inputPt.Z/radiusRatio;
>     //Define Sphere Geometry
>     sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);
>     //Define output for function
>     return sphere;
>     };
>     ```
> 2. Actualice el **bloque de código** secundario mediante la adición de una variable "ratio" a la entrada: `sphereByZ(Pt,ratio);`. Conecte un control deslizante a la entrada de **bloque de código** recién creada y varíe el tamaño de los radios en función de la relación de radio.
