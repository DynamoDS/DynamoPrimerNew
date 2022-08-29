# Nodos y cables

## Nodos

En Dynamo, los **nodos** son los objetos que se conectan para formar un programa visual. Cada **nodo** realiza una operación: en ocasiones, es algo tan sencillo como almacenar un número, o puede ser una acción más compleja, como crear o consultar geometría.

### Anatomía de un nodo

La mayoría de los nodos de Dynamo están compuestos por cinco partes. Aunque existen excepciones, como los nodos de entrada, la anatomía de cada nodo se puede describir como se indica a continuación:

![](<images/nodes and wires - nodes anatomy.jpg>)

> 1. Nombre: nombre del nodo con la convención de nomenclatura `Category.Name`.
> 2. Cuerpo principal: el cuerpo principal del nodo. Al hacer clic con el botón derecho, se presentan opciones en el nivel de todo el nodo.
> 3. Puertos (entrada y salida): los receptores de los cables que proporcionan los datos de entrada al nodo, así como los resultados de la acción del nodo.
> 4. Valor por defecto (haga clic con el botón derecho en un puerto de entrada): algunos nodos tienen valores por defecto que se pueden utilizar o no.
> 5. Icono de encaje: indica la [opción de encaje](../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md#lacing) especificada para las entrada de lista coincidentes (se explicará detalladamente más adelante).

### Puerto de entrada/salida de los nodos

Las entradas y las salidas de los nodos se denominan puertos y actúan como receptores de cables. Los datos entran por la izquierda en el nodo a través de los puertos y salen del nodo por la derecha después de que se haya ejecutado su operación.

Los puertos esperan recibir datos de un tipo determinado. Por ejemplo, al conectar un número como _2.75_ a los puertos de un nodo Point.ByCoordinates, se creará correctamente un punto. Sin embargo, si se proporciona _Red_ al mismo puerto, se producirá un error.

{% hint style="info" %}Consejo: Coloque el cursor sobre un puerto para ver la información de herramientas que contiene el tipo de datos esperado. {% endhint %}

![](<images/nodes and wires - nodes input and tooltip.jpg>)

> 1. Etiqueta de puerto
> 2. Información de herramientas
> 3. Tipo de datos
> 4. Valor por defecto

### Estados de nodo

Dynamo proporciona una indicación del estado de ejecución del programa visual mediante la renderización de los nodos con diferentes esquemas de color en función del estado de cada nodo. La jerarquía de estados sigue esta secuencia: Error > Advertencia > Información > Vista preliminar.

Al colocar el cursor sobre el nombre o los puertos, o al hacer clic con el botón derecho en ellos, se ofrecen información y opciones adicionales.

![](<../.gitbook/assets/nodes and wires - node states.png>)

> 1. Entradas satisfactorias: un nodo con barras verticales azules sobre sus puertos de entrada está bien conectado y todas sus entradas se han conectado correctamente.
> 2. Entradas insatisfactorias: un nodo con una barra vertical roja sobre uno o más puertos de entrada debe tener esas entradas conectadas.
> 3. Función: un nodo que genera una función y tiene una barra vertical gris sobre un puerto de salida es un nodo de función.
> 4. Seleccionados: los nodos seleccionados actualmente se resaltan con un borde de color turquesa.
> 5. Inutilizados: un nodo azul translúcido está inutilizado, por lo que se suspende su ejecución.
> 6. Vista preliminar desactivada: una barra de estado gris debajo del nodo y un icono de ojo <img src="images/nodes and wires - preview off.jpg" alt="" data-size="line"> indican que la vista preliminar de geometría del nodo está desactivada.
> 7. Advertencia: una barra de estado amarilla debajo de los nodos indica un estado de advertencia, lo que significa que no tienen datos de entrada o que pueden tener tipos de datos incorrectos.
> 8. Error: una barra de estado roja debajo del nodo indica que este presenta un estado de error.
> 9. Información: la barra de estado azul debajo del nodo indica el estado de información, que ofrece detalles útiles sobre los nodos. Este estado se puede activar al aproximarse a un valor máximo admitido por el nodo, usar un nodo de forma que pueda afectar al rendimiento, etc.

#### Administración de nodos de error o advertencia

Si el programa visual contiene advertencias o errores, Dynamo proporcionará información adicional sobre el problema. Todos los nodos en amarillo también presentarán una información de herramientas sobre el nombre. Coloque el cursor sobre el icono de información de herramientas de advertencia ![](<images/nodes and wires - node warning icon.png>) o error ![](<images/nodes and wires - node error icon.png>) para expandirlo.

{% hint style="info" %} Consejo: Con esta información de herramientas a mano, examine los nodos ascendentes para ver si el tipo de datos o la estructura de datos necesarios presentan errores. {% endhint %}

![](<images/nodes and wires - nodes with warning tooltip.jpg>)

> 1. Información de herramientas de advertencia: el valor "null" (nulo) o la falta de datos no se pueden considerar como doble, por ejemplo, un número.
> 2. Utilice el nodo Watch para examinar los datos de entrada.
> 3. El nodo Number ascendente almacena "Red", no un número.

## Cables

Los cables se conectan entre nodos para crear relaciones y establecer el flujo de nuestro programa visual. Podemos considerarlos literalmente como cables eléctricos que transportan pulsos de datos de un objeto al siguiente.

### Flujo del programa <a href="#program-flow" id="program-flow"></a>

Los cables conectan el puerto de salida de un nodo al puerto de entrada de otro nodo. Esta dirección establece el **flujo de datos** en el programa visual.

Los puertos de entrada se encuentran en el lado izquierdo de los nodos y los de salida se encuentran en el lado derecho, por lo que podemos afirmar que el flujo del programa se suele desplazar de izquierda a derecha.

![](<images/nodes and wires - flow of data.jpg>)

### Creación de cables <a href="#creating-wires" id="creating-wires"></a>

Cree un cable. Para ello, haga clic con el botón izquierdo en un puerto y, a continuación, haga clic con el botón izquierdo en el puerto de otro nodo para crear una conexión. Durante el proceso de creación de una conexión, el cable se mostrará discontinuo y se ajustará para convertirse en líneas continuas cuando se establezca correctamente la conexión.

Los datos siempre fluirán a través de este cable desde la salida hasta la entrada. No obstante, podemos crear el cable en cualquier dirección en cuanto a la secuencia en la que se hace clic en los puertos conectados.

![](<images/nodes and wires - creating a wire.gif>)

### Edición de cables <a href="#editing-wires" id="editing-wires"></a>

Con frecuencia, desearemos ajustar el flujo de programa de nuestro programa visual mediante la edición de las conexiones representadas por los cables. Para editar un cable, haga clic con el botón izquierdo en el puerto de entrada del nodo que ya está conectado. Ahora tiene dos opciones:

* Cambie la conexión a un puerto de entrada; haga clic con el botón izquierdo en otro puerto de entrada.

![](<images/nodes and wires - edit wire change port (2).gif>)

* Para eliminar el cable, retire el cable y haga clic con el botón izquierdo en el espacio de trabajo.

![](<images/nodes and wires - edit wires remove.gif>)

* Vuelva a conectar varios cables. Para ello, mantenga pulsada la tecla Mayús mientras hace clic con el botón izquierdo.

![](<images/nodes and wires - edit multi ports.gif>)

* Duplique un cable. Para ello, mantenga pulsada la tecla Ctrl mientras hace clic con el botón izquierdo.

![](<images/nodes and wires - duplicate wire.gif>)

#### Cables por defecto frente a resaltados <a href="#wire-previews" id="wire-previews"></a>

Por defecto, nuestros cables se previsualizarán con un trazo gris. Cuando se selecciona un nodo, se renderizará cualquier cable de conexión con el mismo resaltado de color turquesa que el nodo.

![](<images/nodes and wires - default vs highlighted wires.jpg>)

> 1. Cable resaltado
> 2. Cable por defecto

**Ocultar cables por defecto**

Si prefiere ocultar los cables en el gráfico, vaya a Vista > Conectores > desmarque Mostrar conectores.

Con este parámetro, solo los nodos seleccionados y sus cables de unión se mostrarán resaltados en turquesa claro.

![](<images/nodes and wires - hide wires setting (1).gif>)

#### Ocultar solo un cable individual

También puede ocultar el conductor seleccionado. Para ello, haga clic con el botón derecho en la salida Nodos > seleccione Ocultar cables.

![](<images/nodes and wires - hide selected wire.gif>)
