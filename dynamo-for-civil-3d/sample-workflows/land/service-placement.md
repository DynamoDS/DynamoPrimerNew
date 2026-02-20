# Colocación de servicios

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

El diseño de ingeniería de una construcción de viviendas típica implica trabajar con varios servicios subterráneos, como el alcantarillado sanitario, el desagüe pluvial, el agua potable u otros. En este ejemplo se mostrará cómo puede utilizarse Dynamo para trazar las conexiones de servicios desde una red de distribución hasta una parcela (es decir, un terreno) especificada. Es habitual que cada parcela requiera una conexión de servicios, lo que supone un trabajo considerablemente tedioso para colocar todos ellos. Dynamo puede agilizar el proceso al dibujar automáticamente la geometría necesaria con precisión, además de proporcionar entradas flexibles que pueden ajustarse para adaptarse a las normas de los organismos locales.

## Objetivo

> :dart: Coloque las referencias a bloque de contadores de servicios de agua a una distancia determinada de la línea de parcela y trace una línea para cada conexión de servicios perpendicular a la red de distribución.

## Conceptos clave

> * Uso del nodo **Select Object** para la entrada de usuario
> * Trabajo con sistemas de coordenadas
> * Uso de operaciones geométricas como **Geometry.DistanceTo** y **Geometry.ClosestPointTo**
> * Creación de referencias a bloque
> * Control de la configuración de enlace de objetos

## Compatibilidad con versiones

{% hint style="success" %} Este gráfico se ejecutará en **Civil 3D 2020** y versiones posteriores. {% endhint %}

## Conjunto de datos

Descargue primero los archivos de ejemplo que aparecen a continuación y abra el archivo DWG y el gráfico de Dynamo.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## Solución

A continuación, se ofrece una descripción general de la lógica de este gráfico.

> 1. Obtener la geometría de curva para la red de distribución
> 2. Obtener la geometría de curva para una línea de parcela seleccionada por el usuario, invirtiéndola si es necesario
> 3. Generar los puntos de inserción para los contadores de servicio
> 4. Obtener los puntos de la red de distribución más cercanos a las ubicaciones de los contadores de servicio
> 5. Crear referencias a bloque y líneas en el espacio modelo

¡Empecemos!

### Obtener la geometría de la red de distribución

Nuestro primer paso es introducir en Dynamo la geometría de la red de distribución. En lugar de seleccionar líneas o polilíneas individuales, utilizaremos todos los objetos de una determinada capa y los uniremos como una PolyCurve de Dynamo.

{% hint style="info" %} Si es la primera vez que utiliza la geometría de curvas de Dynamo, consulte la sección [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Obtener los objetos de Civil 3D y unir todo en una única PolyCurve</p></figcaption></figure>

### Obtener la geometría de línea de parcela

A continuación, debemos obtener la geometría de una línea de parcela seleccionada en Dynamo para poder trabajar con ella. La herramienta adecuada para la tarea es el nodo **Select Object**, que permite al usuario del gráfico seleccionar un objeto específico en Civil 3D.

También debemos ocuparnos de los posibles problemas que puedan surgir. La línea de parcela tiene un punto inicial y uno final, lo que significa que tiene una dirección. Para que el gráfico genere resultados coherentes, necesitamos que todas las líneas de parcela tengan una dirección coherente. Podemos dar cuenta de esta condición directamente en la lógica del gráfico, lo que hace que este sea más flexible. 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>Seleccionar una línea de parcela y asegurarse de que presente la dirección correcta</p></figcaption></figure>

> 1. Obtenga los puntos inicial y final de la línea de parcela.
> 2. Mida la distancia de cada punto a la red de distribución y, a continuación, calcule la distancia que sea mayor.
> 3. El resultado deseado es que el punto inicial de la línea sea el más cercano a la red de distribución. Si no es así, invertiremos la dirección de la línea de parcela. En caso contrario, simplemente restableceremos la línea de parcela original.

### Generar puntos de inserción

Es hora de decidir dónde se van a colocar los contadores de servicio. Por lo general, la colocación viene determinada por los requisitos de los organismos locales, por lo que nos limitaremos a proporcionar valores de entrada que puedan modificarse para adaptarlos a las distintas situaciones. Vamos a utilizar un **sistema de coordenadas** a lo largo de la línea de parcela como referencia para crear los puntos. Esto facilita la definición de desfases en relación con la línea de parcela, independientemente de su orientación.

{% hint style="info" %} Si es la primera vez que utiliza los sistemas de coordenadas, consulte la sección [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>Creación de los puntos de inserción para los contadores de servicio</p></figcaption></figure>

### Obtener los puntos de conexión

Ahora debemos obtener los puntos de la red de distribución más cercanos a las ubicaciones de los contadores de servicio. Esto nos permitirá dibujar las conexiones de servicios en el espacio modelo de forma que sean siempre perpendiculares a la red de distribución. El nodo **Geometry.ClosestPointTo** es la solución perfecta.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>Obtención de puntos perpendiculares en la red de distribución</p></figcaption></figure>

> 1. Esta es la PolyCurve de la red de distribución.
> 2. Estos son los puntos de inserción del contador de servicio.

### Crear objetos

El último paso consiste en crear objetos en el espacio modelo. Utilizaremos los puntos de inserción que hemos generado anteriormente para crear referencias a bloque y, a continuación, usaremos los puntos de la red de distribución para dibujar líneas en las conexiones de servicios.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### Resultado

Al ejecutar el gráfico, debe ver las nuevas referencias a bloque y las líneas de conexión de servicios en el espacio modelo. Pruebe a cambiar algunas de las entradas y observe cómo todo se actualiza automáticamente.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Ajuste de los parámetros de entrada en Dynamo y visualización inmediata de los resultados en Civil 3D</p></figcaption></figure>

### Información adicional: habilitar la colocación secuencial

Tras colocar los objetos para una línea de parcela, puede que observe que los objetos se "desplazan" al seleccionar una línea diferente.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>Comportamiento que aparece cuando se activa el enlace de objetos</p></figcaption></figure>

Este es el comportamiento por defecto de Dynamo y es muy útil en muchos casos. Sin embargo, puede que desee colocar varias conexiones de servicios de forma secuencial y que Dynamo cree nuevos objetos con cada ejecución en lugar de modificar los originales. Cambie la configuración de enlace de objetos para controlar este comportamiento.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Parámetros de enlace de objetos de Dynamo</p></figcaption></figure>

{% hint style="info" %} Para obtener más información, consulte la sección [object-binding.md](../../advanced-topics/object-binding.md "mention"). {% endhint %}

Al cambiar esta configuración, se obligará a Dynamo a "olvidar" los objetos que crea con cada ejecución. A continuación, se muestra un ejemplo de ejecución del gráfico con el enlace de objetos desactivado mediante el **Reproductor de Dynamo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Ejecución del gráfico mediante el Reproductor de Dynamo y visualización de los resultados en Civil 3D</p></figcaption></figure>

{% hint style="info" %} Si es la primera vez que utiliza el Reproductor de Dynamo, consulte la sección [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: ¡Misión cumplida!

## Ideas

A continuación, se ofrecen algunas ideas sobre cómo podría ampliar las posibilidades de este gráfico.

{% hint style="info" %} Coloque **varias conexiones de servicios** simultáneamente en lugar de seleccionar cada línea de parcela. {% endhint %}

{% hint style="info" %} Ajuste las entradas para colocar **bocas de alcantarilla** en lugar de contadores de servicio de agua. {% endhint %}

{% hint style="info" %} **Añada un conmutador** para permitir la colocación de una única conexión de servicio en un lado concreto de la línea de parcela en lugar de en ambos lados. {% endhint %}
