# Abreviatura

### Abreviatura

Existen varios métodos básicos de abreviatura en el bloque de código que, sencillamente, facilitan _mucho_ la administración de datos. A continuación, vamos a desglosar los conceptos básicos y analizar cómo se puede utilizar la abreviatura para crear y consultar datos.

| **Tipo de datos** | **Dynamo estándar** | **Bloque de código equivalente** |
| ---------------------- | -------------------------------------------------------- | ------------------------------------------------------------- |
| Números | ![](<../images/8-1/3/01 node - numbers.jpg>) | ![](<../images/8-1/3/01 codeblock - numbers.jpg>) |
| Cadenas | ![](<../images/8-1/3/02 node - string.jpg>) | ![](<../images/8-1/3/02 codeblock- string.jpg>) |
| Secuencias | ![](<../images/8-1/3/03 node- sequence.jpg>) | ![](<../images/8-1/3/03 codeblock- sequence.jpg>) |
| Rangos | ![](<../images/8-1/3/04 node- range.jpg>) | ![](<../images/8-1/3/04 codeblock - range.jpg>) |
| Obtener elemento en índice | ![](<../images/8-1/3/05 node - list get item.jpg>) | ![](<../images/8-1/3/05 codeblock - list get item.jpg>) |
| Crear lista | ![](<../images/8-1/3/06 node - list create.jpg>) | ![](<../images/8-1/3/06 codeblock - list create.jpg>) |
| Concatenar cadenas | ![](<../images/8-1/3/07 node - string concat.jpg>) | ![](<../images/8-1/3/07 codeblock - string concat.jpg>) |
| Instrucciones condicionales | ![](<../images/8-1/3/08 node - conditional.jpg>) | ![](<../images/8-1/3/08 codeblock - conditional.jpg>) |

### Sintaxis adicional

|                                     |                           |                                                                                          |
| ----------------------------------- | ------------------------- | ---------------------------------------------------------------------------------------- |
| **Nodo(s)** | **Bloque de código equivalente** | **Nota** |
| Cualquier operador (+, &&, >=, Not, etc.) | +, &&, >=, !, etc. | Tenga en cuenta que "Not" se convierte en "!" pero el nodo se denomina "Not" para distinguirlo de "Factorial". |
| Boolean True | true; | Se deben respetar las minúsculas. |
| Boolean False | false; | Se deben respetar las minúsculas. |

### Rangos y secuencias

El método para definir rangos y secuencias se puede reducir a un método básico de abreviatura. Utilice la imagen siguiente como guía de la sintaxis "..." para definir una lista de datos numéricos con un bloque de código. Una vez que aprendemos a utilizar esta notación, crear datos numéricos es un proceso realmente eficaz:

![](<../images/8-1/3/shorthand - ranges and sequences.jpg>)

> 1. En este ejemplo, un rango de números se sustituye por una sintaxis de **bloque de código** que define `beginning..end..step-size;`. Representados numéricamente, obtenemos lo siguiente: `0..10..1;`.
> 2. Observe que la sintaxis `0..10..1;` es equivalente a `0..10;`. Un tamaño de paso de 1 es el valor por defecto de la notación de abreviatura. Por lo tanto, `0..10;` proporciona una secuencia de 0 a 10 con un tamaño de paso de 1.
> 3. El ejemplo de _secuencia_ es similar, excepto que se utiliza un símbolo "#" para indicar que deseamos que haya 15 valores en la lista en lugar de una lista que llegue hasta 15. En este caso, se define lo siguiente: `beginning..#ofSteps..step-size:`. La sintaxis real de la secuencia es `0..#15..2`.
> 4. Ahora vamos a introducir el símbolo _"#"_ del paso anterior en la sección _"tamaño-de-paso"_ de la sintaxis. Ahora, tenemos un _rango de números_ que abarca de _"inicio"_ a _"fin"_ y la notación de _"tamaño de paso"_ distribuye uniformemente un número de valores entre los dos elementos: `beginning..end..#ofSteps`

### Rangos avanzados

La creación de rangos avanzados nos permite trabajar con listas de listas de una forma sencilla. En los ejemplos siguientes, se aísla una variable de la notación de rango principal y se crea otro rango de dicha lista.

![](<../images/8-1/3/shorthand - advance range 01.jpg>)

> 1\. Mediante la creación de rangos anidados, compare la notación con el símbolo "#" frente a la notación sin este símbolo. Se aplica la misma lógica que en los rangos básicos, pero se vuelve un poco más compleja.
>
> 2\. Podemos definir un subrango en cualquier lugar dentro del rango principal, y observe que también podemos tener dos subrangos.
>
> 3\. Al controlar el valor "fin" de un rango, se crean más rangos de longitudes diferentes.

Como ejercicio lógico, compare las dos abreviaturas anteriores e intente analizar el modo en que los _subrangos_ y la notación _#_ controlan la salida.

![](<../images/8-1/3/shorthand - advance range 02.jpg>)

### Crear listas y obtener elementos de una lista

Además de crear listas con la función de abreviatura, también podemos crear listas sobre la marcha. Estas listas pueden contener una amplia gama de tipos de elementos y también se pueden consultar (recuerde que las listas son objetos en sí mismas). En resumen, con el bloque de código se crean listas y se consultan los elementos de una lista con corchetes:

![](<../images/8-1/3/shorthand - list & get from list 01.jpg>)

> 1\. Cree listas rápidamente con cadenas y consulte las listas mediante el índice de elementos.
>
> 2\. Cree listas con variables y realice consultas utilizando la notación de abreviatura de rango.

Además, el proceso de gestión con listas anidadas es similar. Tenga en cuenta el orden de la lista y recuerde utilizar varios conjuntos de corchetes:

![](<../images/8-1/3/shorthand - list & get from list 02.jpg>)

> 1\. Defina una lista de listas.
>
> 2\. Consulte una lista con notación de un solo corchete.
>
> 3\. Consulte un elemento con notación de doble corchete.

## Ejercicio: superficie sinusoidal

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/8-1/3/Obsolete-Nodes_Sine-Surface.dyn" %}

En este ejercicio, vamos a poner en práctica nuestras nuevas habilidades de abreviación para crear una superficie de cáscaras de huevo con mucho estilo y definida por rangos y fórmulas. A lo largo de este ejercicio, observe cómo utilizamos el bloque de código y los nodos existentes en Dynamo en tándem: se utiliza el bloque de código para la elevación de datos pesados mientras que los nodos de Dynamo se disponen visualmente para determinar la legibilidad de la definición.

Comience por crear una superficie conectando los nodos anteriores. En lugar de utilizar un nodo numérico para definir la anchura y la longitud, haga doble clic en el lienzo y escriba `100;` en un bloque de código.

![](<../images/8-1/3/shorthand - exercise 01.jpg>)

![](<../images/8-1/3/shorthand - exercise 02.jpg>)

> 1. Defina un rango entre 0 y 1 con 50 divisiones. Para ello, escriba `0..1..#50` en un **bloque de código**.
> 2. Conecte el rango en **Surface.PointAtParameter**, que toma los valores u y v entre 0 y 1 a lo largo de la superficie. No olvide cambiar el encaje a Producto vectorial. Para ello, haga clic con el botón derecho en el nodo **Surface.PointAtParameter**.

En este paso, utilizamos nuestra primera función para mover la rejilla de puntos hacia arriba en el eje Z. Esta rejilla controlará una superficie generada a partir de la función subyacente. Añada nuevos nodos, como se muestra en la imagen siguiente.

![](<../images/8-1/3/shorthand - exercise 03.jpg>)

> 1. En lugar de utilizar un nodo de fórmula, utilizamos un **bloque de código** con la siguiente línea: `(0..Math.Sin(x*360)..#50)*5;`. Para descomponerlo rápidamente, vamos a definir un rango con una fórmula dentro de él. Esta fórmula es la función de seno. La función de seno recibe entradas de grado en Dynamo, por lo que, para obtener una onda sinusoidal completa, multiplicamos los valores x (es decir, la entrada de rango de 0 a 1) por 360. A continuación, queremos que haya el mismo número de divisiones que de puntos de rejilla de control para cada fila, por lo que definimos cincuenta subdivisiones con #50. Por último, el multiplicador de 5 simplemente aumenta la amplitud de la traslación para que podamos ver el efecto en la vista preliminar de Dynamo.

![](<../images/8-1/3/shorthand - exercise 04.jpg>)

> 1. Aunque el **bloque de código** anterior, no era totalmente paramétrico. Deseamos controlar sus parámetros de forma dinámica, por lo que reemplazaremos la línea del paso anterior por `(0..Math.Sin(x*360*cycles)..#List.Count(x))*amp;`. Esto nos permite definir estos valores en función de las entradas.

Al cambiar los controles deslizantes (de 0 a 10), obtenemos resultados interesantes.

![](<../images/8-1/3/shorthand - exercise 05.gif>)

![](<../images/8-1/3/shorthand - exercise 06.jpg>)

> 1. Al realizar una transposición en el rango de números, se invierte la dirección de la onda cortina: `transposeList = List.Transpose(sineList);`

![](<../images/8-1/3/shorthand - exercise 07.jpg>)

> 1. Se obtiene una superficie de cáscara de huevo distorsionada al añadir los elementos sineList y tranposeList: `eggShellList = sineList+transposeList;`.

Cambiemos los valores de los controles deslizantes especificados a continuación para "calmar las aguas" de este algoritmo.

![](<../images/8-1/3/shorthand - exercise 08.jpg>)

Por último, vamos a consultar las partes aisladas de los datos con el bloque de código. Para regenerar la superficie con un rango específico de puntos, añada el bloque de código anterior entre los nodos **Geometry.Translate** y **NurbsSurface.ByPoints**. Tiene la siguiente línea de texto: `sineStrips[0..15..1];`. De este modo, se seleccionarán las primeras 16 filas de puntos (de 50). Al volver a crear la superficie, podemos ver que hemos generado una parte aislada de la rejilla de puntos.

![](<../images/8-1/3/shorthand - exercise 09.jpg>)

![](<../images/8-1/3/shorthand - exercise 10.jpg>)

> 1. En el paso final, para que este **bloque de código** sea más paramétrico, la consulta se controla mediante un control deslizante que va de 0 a 1. Para ello, utilizaremos esta línea de código: `sineStrips[0..((List.Count(sineStrips)-1)*u)];`. Esto puede resultar confuso, pero la línea de código nos proporciona una forma rápida de modificar la longitud de la lista con un multiplicador entre 0 y 1.

Un valor de `0.53` en el control deslizante crea una superficie justo después del punto medio de la rejilla.

![](<../images/8-1/3/shorthand - exercise 11.jpg>)

Como se puede esperar, un control deslizante de `1` crea una superficie a partir de la rejilla completa de puntos.

![](<../images/8-1/3/shorthand - exercise 12.jpg>)

En el gráfico visual, podemos resaltar los bloques de código y ver cada una de sus funciones.

![](<../images/8-1/3/shorthand - exercise 13.jpg>)

> 1\. El primer **bloque de código** sustituye al nodo **Number**.
>
> 2\. El segundo **bloque de código** sustituye al nodo **Number Range**.
>
> 3\. El tercer **bloque de código** sustituye al nodo **Formula** (así como a**List.Transpose**, **List.Count** y **Number Range**).
>
> 4\. El cuatro **bloque de código** consulta una lista de listas y sustituye al nodo **List.GetItemAtIndex**.
