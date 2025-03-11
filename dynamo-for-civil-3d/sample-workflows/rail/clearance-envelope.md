# Envolvente libre

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

El desarrollo de envolventes cinemáticos para la validación de la separación es una parte importante del diseño de ferrocarriles. Dynamo se puede utilizar para generar sólidos para el envolvente en lugar de crear y administrar subensamblajes de obra lineal complejos para realizar el trabajo.

## Objetivo

> :dart: Utilice un bloque de perfil de vehículo para generar sólidos 3D de envolvente libre a lo largo de una obra lineal.

## Conceptos clave

> * Trabajo con líneas características de obra lineal
> * Transformación de la geometría entre sistemas de coordenadas
> * Creación de sólidos mediante solevación
> * Control del comportamiento de los nodos con parámetros de encaje

## Compatibilidad con versiones

{% hint style="success" %} Este gráfico se ejecutará en **Civil 3D 2020** y versiones posteriores. {% endhint %}

## Conjunto de datos

Descargue primero los archivos de ejemplo que aparecen a continuación y abra el archivo DWG y el gráfico de Dynamo.

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## Solución

A continuación, se ofrece una descripción general de la lógica de este gráfico.

> 1. Obtener líneas características de la línea base de obra lineal especificada
> 2. Generar sistemas de coordenadas a lo largo de la línea característica de obra lineal con la separación deseada
> 3. Transformar la geometría de bloque de perfil en sistemas de coordenadas
> 4. Solevar un sólido entre los perfiles
> 5. Crear los sólidos en Civil 3D

¡Empecemos!

### Obtener datos de obra lineal

El primer paso es obtener datos de obra lineal. Seleccionaremos el modelo de obra lineal por su nombre, obtendremos una línea base específica dentro de la obra lineal y, a continuación, conseguiremos una línea característica dentro de la línea base por su código de punto.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>Selección de la obra lineal, la línea base y la línea característica</p></figcaption></figure>

### Generar sistemas de coordenadas

Ahora vamos a generar **sistemas de coordenadas** a lo largo de las líneas características de obra lineal entre un P.K. inicial y uno final especificados. Estos sistemas de coordenadas se utilizarán para alinear la geometría de bloque del perfil del vehículo con la obra lineal.

{% hint style="info" %} Si es la primera vez que utiliza los sistemas de coordenadas, consulte la sección [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>Obtención de sistemas de coordenadas a lo largo de las líneas características de obra lineal</p></figcaption></figure>

> 1. Observe el pequeño **XXX** en la esquina inferior derecha del nodo. Esto significa que los parámetros de encaje del nodo se han definido como _Producto vectorial_, lo que es necesario para generar los sistemas de coordenadas con los mismos valores de P.K. para ambas líneas características.

{% hint style="info" %} Si es la primera vez que utiliza el encaje de nodos, consulte la sección [1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention"). {% endhint %}

### Transformar la geometría de bloque

Ahora tenemos que crear de alguna manera una matriz de los perfiles de los vehículos a lo largo de las líneas características. Lo que vamos a hacer es transformar la geometría desde la definición de bloque del perfil del vehículo mediante el nodo **Geometry.Transform**. Se trata de un concepto difícil de visualizar, así que antes de ver los nodos, he aquí un gráfico que muestra lo que va a ocurrir.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>Visualización de la transformación de la geometría entre sistemas de coordenadas</p></figcaption></figure>

En definitiva, lo que hacemos es utilizar la geometría de Dynamo de una _única_ definición de bloque y desplazarla o rotarla, a la vez que creamos una matriz a lo largo de la línea característica. ¡Increíble! A continuación, se muestra el aspecto de la secuencia de nodos.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. De esta forma, se obtiene la definición de bloque del documento.
> 2. Estos nodos obtienen la geometría de Dynamo de los objetos del bloque.
> 3. Estos nodos definen básicamente el sistema de coordenadas _desde_ el que se está transformando la geometría.
> 4. Y, por último, este nodo realiza el trabajo real de transformar la geometría.
> 5. Observe el encaje _más largo_ en este nodo.

Y esto es lo que obtenemos en Dynamo.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>La geometría del bloque del perfil del vehículo después de la transformación</p></figcaption></figure>

### Generar sólidos

¡Buenas noticias! Ya se ha completado la parte más complicada. Ahora solo tenemos que generar sólidos entre los perfiles. Esto se consigue fácilmente con el nodo **Solid.ByLoft**.

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

Este es el resultado. Recuerde que estos son sólidos de Dynamo; aún debemos crearlos en Civil 3D.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>Los sólidos de Dynamo después de la solevación</p></figcaption></figure>

### Generar sólidos en Civil 3D

El último paso es generar los sólidos en el espacio modelo. También les asignaremos un color para que sean fáciles de ver.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>Generación de sólidos en Civil 3D</p></figcaption></figure>

### Resultado

A continuación, se muestra un ejemplo de cómo ejecutar el gráfico con el **Reproductor de Dynamo**.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>Ejecución del gráfico mediante el Reproductor de Dynamo y visualización de los resultados en Civil 3D</p></figcaption></figure>

{% hint style="info" %} Si es la primera vez que utiliza el Reproductor de Dynamo, consulte la sección [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: ¡Misión cumplida!

## Ideas

A continuación, se ofrecen algunas ideas sobre cómo podría ampliar las posibilidades de este gráfico.

{% hint style="info" %} Añada la capacidad de utilizar **diferentes intervalos de P.K.** por separado para cada vía. {% endhint %}

{% hint style="info" %} **Divida los sólidos** en segmentos más pequeños que se puedan analizar individualmente para detectar conflictos. {% endhint %}

{% hint style="info" %} Compruebe si los sólidos de envolvente **se intersecan con elementos** y coloree aquellos que entren en conflicto. {% endhint %}
