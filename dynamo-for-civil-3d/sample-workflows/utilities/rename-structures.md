# Renombrar estructuras

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

Al añadir tuberías y estructuras a una red de tuberías, Civil 3D utiliza una plantilla para asignar nombres automáticamente. Por lo general, esto es suficiente durante la colocación inicial, pero inevitablemente los nombres tendrán que cambiar en el futuro a medida que evolucione el diseño. Además, hay muchos patrones de nomenclatura diferentes que podrían ser necesarios, por ejemplo, denominar las estructuras de forma secuencial dentro de un tramo de tuberías empezando por la estructura más alejada aguas abajo o seguir un patrón de nomenclatura que se ajuste al esquema de datos de un organismo local. En este ejemplo, se muestra cómo se puede utilizar Dynamo para definir cualquier tipo de estrategia de nomenclatura y aplicarla de forma coherente.

## Objetivo

> :dart: Cambie el nombre de las estructuras de red de tuberías en orden según el etiquetado en formato P.K. de una alineación.

## Conceptos clave

> * Trabajo con cuadros delimitadores
> * Filtrado de datos mediante el nodo **List.FilterByBoolMask**
> * Ordenación de los datos mediante el nodo **List.SortByKey**
> * Generación y modificación de cadenas de texto

## Compatibilidad con versiones

{% hint style="success" %} Este gráfico se ejecutará en **Civil 3D 2020** y versiones posteriores. {% endhint %}

## Conjunto de datos

Descargue primero los archivos de ejemplo que aparecen a continuación y abra el archivo DWG y el gráfico de Dynamo.

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## Solución

A continuación, se ofrece una descripción general de la lógica de este gráfico.

> 1. Seleccionar las estructuras por capa
> 2. Obtener las ubicaciones de estructuras
> 3. Filtrar las estructuras por desfase y, a continuación, ordenarlas por P.K.
> 4. Generar los nuevos nombres
> 5. Renombrar las estructuras

¡Empecemos!

### Seleccionar estructuras

Lo primero que debemos hacer es seleccionar todas las estructuras con las que tenemos previsto trabajar. Para ello, basta con seleccionar todos los objetos de una capa determinada, lo que significa que podemos seleccionar estructuras de diferentes redes de tuberías (siempre que compartan la misma capa).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>Selección de estructuras en la capa especificada</p></figcaption></figure>

> 1. Este nodo garantiza que no se recuperarán accidentalmente los tipos de objeto no deseados que puedan compartir la misma capa que las estructuras.

### Obtener ubicaciones de estructuras

Ahora que disponemos de las estructuras, tenemos que averiguar su posición en el espacio para poder ordenarlas según su ubicación. Para ello, utilizaremos el cuadro delimitador de cada objeto. El **cuadro delimitador** de un objeto es el cuadro de tamaño mínimo que contiene por completo la extensión geométrica del objeto. Al calcular el centro del cuadro delimitador, obtenemos una aproximación bastante precisa del punto de inserción de la estructura.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>Uso de cuadros delimitadores para obtener el punto de inserción aproximado de cada estructura</p></figcaption></figure>

Utilizaremos estos puntos para obtener el P.K. y el desfase de las estructuras con respecto a una alineación seleccionada.

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### Filtrar y ordenar

Aquí es donde las cosas empiezan a ponerse un poco complicadas. En esta fase, tenemos una gran lista de todas las estructuras de la capa que hemos especificado y elegimos una alineación por la que deseamos ordenarlas. El problema es que puede haber estructuras en la lista que no deseamos cambiar de nombre. Por ejemplo, es posible que no formen parte del tramo concreto que nos interesa.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. La alineación seleccionada
> 2. Las estructuras que deseamos renombrar
> 3. Las estructuras que se deben ignorar

Por lo tanto, debemos filtrar la lista de estructuras para que no se tengan en cuenta las que superen un determinado desfase con respecto a la alineación. La mejor forma de conseguirlo es mediante el nodo **List.FilterByBoolMask**. Después de filtrar la lista de estructuras, utilizamos el nodo **List.SortByKey** para ordenarlas por sus valores de P.K.

{% hint style="info" %} Si es la primera vez que trabaja con listas, consulte la sección [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>Filtrado y ordenación de las estructuras</p></figcaption></figure>

> 1. Compruebe si el desfase de la estructura es menor que el valor del umbral.
> 2. Sustituya cualquier valor nulo por _false_ (falso).
> 3. Filtre la lista de estructuras y P.K.
> 4. Ordene las estructuras por P.K.

### Generar nombres nuevos

La última parte del trabajo consiste en crear los nuevos nombres de las estructuras. El formato que usaremos es `<alignment name>-STRC-<number>`. Aquí hay algunos nodos más para rellenar los números con ceros adicionales si se desea (por ejemplo, "01" en lugar de "1").

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>Generación de los nuevos nombres de las estructuras</p></figcaption></figure>

### Renombrar estructuras

Y, por último, pero no por ello menos importante, cambiamos el nombre de las estructuras.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>Establecer los nombres de las estructuras</p></figcaption></figure>

### Resultado

A continuación, se muestra un ejemplo de cómo ejecutar el gráfico con el **Reproductor de Dynamo**.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Ejecución del gráfico mediante el Reproductor de Dynamo y visualización de los resultados en Civil 3D</p></figcaption></figure>

{% hint style="info" %} Si es la primera vez que utiliza el Reproductor de Dynamo, consulte la sección [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: ¡Misión cumplida!

### Información adicional: visualización en Dynamo

Puede ser útil aprovechar la vista preliminar 3D en segundo plano de Dynamo para visualizar las salidas intermedias del gráfico en lugar de solo el resultado final. Algo fácil que podemos hacer es visualizar los cuadros delimitadores de las estructuras. Además, este conjunto de datos concreto tiene una obra lineal en el documento, por lo que podemos incorporar la geometría de línea característica de obra lineal en Dynamo para proporcionar contexto en relación con la ubicación de las estructuras en el espacio. Si el gráfico se utiliza en un conjunto de datos sin obras lineales, estos nodos no harán nada.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>Visualización de la geometría de estructuras y líneas características de obra lineal</p></figcaption></figure>

Ahora podemos entender mejor cómo funciona el proceso de filtrado de las estructuras por desfase.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Ajuste del valor de umbral de desfase de alineación y visualización de las estructuras afectadas en Dynamo</p></figcaption></figure>

## Ideas

A continuación, se ofrecen algunas ideas sobre cómo podría ampliar las posibilidades de este gráfico.

{% hint style="info" %} Cambie el nombre de las estructuras en función de su **alineación más cercana** en lugar de seleccionar una alineación específica. {% endhint %}

{% hint style="info" %} **Cambie el nombre de las tuberías**, además del de las estructuras. {% endhint %}

{% hint style="info" %} **Establezca las capas** de las estructuras en función del tramo. {% endhint %}
