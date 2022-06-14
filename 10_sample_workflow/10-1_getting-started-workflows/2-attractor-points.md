# Puntos de atractor

Los puntos de atractor son excelentes para experimentar con patrones geométricos. Se pueden utilizar para crear cambios graduales en los objetos en función de su distancia.

En este flujo de trabajo, aprenderá los siguientes procedimientos:

* Cree, administre y edite listas.
* Desplace puntos en la vista preliminar 3D mediante manipulación directa.
* Cambie el modo de ejecución.

![](../images/10-1/2/attractor1.gif)

## Definición de objetivos

En este ejercicio, vamos a crear un círculo (_objetivo_) donde la entrada de radio se define mediante una distancia hasta un punto cercano (_relación_).

![Boceto a mano de un círculo](../images/10-1/2/00-Hand-Sketch-of-Circle.png)

> Un punto que define una relación basada en la distancia se suele denominar "atractor". Aquí, la distancia a nuestro punto atractor se utilizará para especificar el tamaño del círculo.

## Pasos siguientes

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/10-1/2/DynamoSampleWorkflow-Attractors.dyn" %}

Ahora que hemos dibujado un boceto de nuestros objetivos y relaciones, podemos empezar a crear el gráfico. Necesitamos los nodos que representarán la secuencia de acciones que ejecutará Dynamo. Añadamos primero los siguientes nodos: **Number**, **Number Slider**, **Point.ByCoordinates** y **Geometry.DistanceTo y Circle.ByCenterPointRadius.**

![](<../images/10-1/2/attractor (2).png>)

> 1. Entrada > Básico > **Number**
> 2. Entrada > Básico > **Number Slider**
> 3. Geometría > Puntos > Punto > **By Coordinates(x,y,z)**
> 4. Geometría > Modificadores > Geometría > **DistanceTo**
> 5. Geometría > Curvas > Círculo > **ByCenterPointRadius**

### Conexión de nodos con cables

Ahora que tenemos algunos nodos, debemos conectar los puertos de los nodos con cables. Estas conexiones definirán el flujo de datos.

![](<../images/10-1/2/attractor (3).png>)

> 1. De **Number** a **Point.ByCoordinates**
> 2. De **Number Sliders** a **Point.ByCoordinates**
> 3. De **Point.ByCoordinates** (2) a **DistanceTo**
> 4. De **Point.ByCoordinates** y **DistanceTo** a **Circle.ByCenterPointRadius**

### Ejecución del programa

Con el flujo de programa definido, solo tenemos que indicarle a Dynamo que lo ejecute. Una vez ejecutado el programa (automáticamente o al hacer clic en Ejecutar en modo manual), los datos pasarán por los cables y se mostrará la vista preliminar 3D.

![](<../images/10-1/2/attractor (4).png>)

> 1. (Haga clic en Ejecutar): si la barra de ejecución está en modo manual, debemos hacer clic en Ejecutar para ejecutar el gráfico.
> 2. Vista preliminar de nodos: al colocar el puntero sobre el cuadro situado en la esquina inferior derecha de un nodo, se mostrará un elemento emergente con los resultados.
> 3. Vista preliminar 3D: si alguno de los nodos crea geometría, esta aparecerá en la vista preliminar 3D.
> 4. La geometría de salida en el nodo de creación.

### Adición de **un bloque de código**

Si el programa está en funcionamiento, debería aparecer un círculo en la vista preliminar 3D que atraviesa el punto atractor. Esto es genial, pero es posible que desee añadir más detalles o controles. Ajustemos la entrada al nodo del círculo para que podamos calibrar la influencia en el radio. Añada otro nodo **Number Slider** al espacio de trabajo y haga doble clic en un área en blanco del espacio de trabajo para añadir un nodo de **bloque de código**. Edite el campo en el bloque de código. Para ello, especifique `X/Y`.

![](<../images/10-1/2/attractor (5).png>)

> 1. **Bloque de código**
> 2. De **DistanceTo** y **Number Slider** al **bloque de código**
> 3. Del **bloque de código** a **Circle.ByCenterPointRadius**

### Uso de secuencias

Un comienzo sencillo al que se le añada complejidad es una manera eficaz de desarrollar el programa de manera incremental. Una vez que esto funcione para un círculo, apliquemos la potencia del programa a varios círculos. En lugar de un punto central, si utilizamos una rejilla de puntos y acomodamos el cambio en la estructura de datos resultante, el programa creará muchos círculos, cada uno con un valor de radio exclusivo definido por la distancia calibrada al punto atractor.

![](<../images/10-1/2/attractor (6).png>)

> 1. Añada un nodo **Number Sequence** y sustituya las entradas de **Point.ByCoordinates**: haga clic con el botón derecho en Point.ByCoordinates y seleccione Encaje > Referencia cruzada.
> 2. Añada un nodo **Flatten** después de Point.ByCoordinates. Para aplanar una lista por completo, mantenga la entrada `amt` en el valor por defecto `-1`.
> 3. La vista preliminar 3D se actualizará con una rejilla de círculos.

### Ajuste con manipulación directa

En ocasiones, la manipulación numérica no es el enfoque adecuado. Ahora puede empujar y estirar manualmente la geometría de punto al desplazarse por la vista preliminar 3D en segundo plano. También podemos controlar otra geometría construida por un punto. Por ejemplo, **Sphere.ByCenterPointRadius** también es capaz de realizar la manipulación directa. Podemos controlar la ubicación de un punto a partir de una serie de valores X, Y y Z con **Point.ByCoordinates**. Sin embargo, con el enfoque de manipulación directa, puede actualizar los valores de los controles deslizantes moviendo manualmente el punto en el modo de **navegación de vista preliminar 3D**. Esto ofrece un enfoque más intuitivo para controlar un conjunto de valores específicos que identifican la ubicación de un punto.

![](<../images/10-1/2/attractor (7).png>)

> 1. Para utilizar la **manipulación directa**, seleccione el panel del punto que se va a mover; aparecerán flechas sobre el punto seleccionado.
> 2. Cambie al modo de **navegación de vista preliminar 3D**.

![](../images/10-1/2/attractor\(8\).png)

> 1. Coloque el cursor sobre el punto; aparecerán los ejes X, Y y Z.
> 2. Haga clic y arrastre la flecha coloreada para mover el eje correspondiente; los valores de **Number Slider** se actualizarán al instante con el punto desplazado manualmente.

![](<../images/10-1/2/attractor (1).png>)

> 1. Tenga en cuenta que, antes de la **manipulación directa**, solo se conectó un control deslizante al componente **Point.ByCoordinates**. Al desplazar manualmente el punto en la dirección X, Dynamo generará automáticamente un nuevo componente **Number Slider** para la entrada X.

