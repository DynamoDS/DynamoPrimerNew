# Introducción

Ahora que ya sabe un poco más sobre el panorama general, vamos a empezar a crear el primer gráfico de Dynamo en Civil 3D.

{% hint style="info" %} Este es un ejemplo sencillo que pretende demostrar la funcionalidad básica de Dynamo. Es recomendable seguir los pasos descritos en un nuevo documento vacío de Civil 3D. {% endhint %}

## Abrir Dynamo

Lo primero que debe hacer es abrir un documento vacío en Civil 3D. Una vez allí, vaya a la ficha **Administrar** de la cinta de opciones de Civil 3D y busque el grupo **Programación visual**.

\![](<../.gitbook/assets/image (7).png>)

Haga clic en el botón **Dynamo**, que iniciará Dynamo en una ventana independiente.

{% hint style="info" %} **¿Cuál es la diferencia entre Dynamo y el Reproductor de Dynamo?**

Dynamo es la herramienta que se utiliza para crear y ejecutar gráficos. El Reproductor de Dynamo permite ejecutar fácilmente gráficos sin tener que abrirlos en Dynamo.

Vaya a la sección [dynamo-player.md](dynamo-player.md "mention") cuando desee probarlo. {% endhint %}

## Iniciar un nuevo gráfico

Una vez que Dynamo esté abierto, aparecerá la pantalla de inicio. Haga clic en **Nuevo** para abrir un espacio de trabajo en blanco.

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Pantalla de inicio de Dynamo</p></figcaption></figure>

{% hint style="info" %} **¿Y los ejemplos?**

Dynamo for Civil 3D incluye algunos gráficos prediseñados que pueden servirle de inspiración para otras ideas sobre cómo usar Dynamo. Es recomendable que les eche un vistazo en algún momento, así como a los [sample-workflows](sample-workflows/ "mention") aquí en el manual de introducción. {% endhint %}

## Añadir nodos

Ahora debería aparecer un espacio de trabajo vacío. Vamos a ver Dynamo en acción. Este es nuestro objetivo:

>  :dart: **Cree un gráfico de Dynamo que inserte texto en el espacio modelo.**

Muy sencillo, ¿verdad? Sin embargo, antes de empezar, debemos abordar algunos aspectos fundamentales.

Los componentes básicos de un gráfico de Dynamo se denominan **nodos**. Un nodo es como una pequeña máquina; se le introducen datos, realiza algún trabajo con ellos y genera un resultado. Dynamo for Civil 3D cuenta con una **biblioteca** de nodos que se pueden conectar mediante **cables** para formar un **gráfico** que realiza tareas más eficaces y de mayor tamaño que las que puede realizar un nodo por sí solo.

{% hint style="info" %} **Espere... ¿Y si nunca he utilizado Dynamo?**

Puede que algunos de estos conceptos le resulten bastante nuevos, pero no se preocupe. Estas secciones le ayudarán a ponerse al día.

[3_user_interface](../3\_user\_interface/ "mention")\
 [4_nodes_and_wires](../4\_nodes\_and\_wires/ "mention")\
 [5_essential_nodes_and_concepts](../5\_essential\_nodes\_and\_concepts/ "mention") {% endhint %}

Ahora, vamos a crear el gráfico. Aquí hay una lista de todos los nodos que necesitaremos.

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

Para encontrar estos nodos, escriba sus nombres en la barra de búsqueda de la biblioteca o haga clic con el botón derecho en cualquier lugar del lienzo y realice allí la búsqueda.

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>Los nodos pueden colocarse desde la biblioteca o haciendo clic con el botón derecho en el lienzo.</p></figcaption></figure>

{% hint style="info" %} **¿Cómo puedo saber qué nodos utilizar y dónde encontrarlos?**

Los nodos de la biblioteca se agrupan en categorías lógicas según su finalidad. Consulte la sección [node-library.md](node-library.md "mention") para realizar un recorrido más detallado. {% endhint %}

El gráfico final debería presentar el siguiente aspecto.

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>El gráfico final</p></figcaption></figure>

Resumamos todo lo que hemos hecho aquí:

> 1. Hemos elegido el documento en el que se va a trabajar. En este caso (y en la mayoría), deseamos trabajar en el documento activo de Civil 3D.
> 2. Hemos definido el bloque de destino en el que se debe crear el objeto de texto (en este caso, el espacio modelo).
> 3. Se ha utilizado un nodo _String_ para especificar la capa en la que se debe colocar el texto.
> 4. Se ha creado un punto mediante el nodo _Point.ByCoordinates_ para definir la posición en la que se debe colocar el texto.
> 5. Se han definido las coordenadas X e Y del punto de inserción de texto mediante dos nodos _Number Slider_.
> 6. Se ha utilizado otro nodo _String_ para definir el contenido del objeto de texto.
> 7. Y, por último, hemos creado el objeto de texto.

Veamos los resultados de nuestro nuevo y reluciente gráfico.

## Ver el resultado

De nuevo en Civil 3D, asegúrese de que la ficha **Modelo** esté seleccionada. Debería ver el nuevo objeto de texto que ha creado Dynamo.

{% hint style="info" %} Si no aparece el texto, es posible que deba ejecutar el comando ZOOM -> EXTENSIÓN para ampliar la vista del espacio correcto. {% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

¡Fantástico! Ahora vamos a realizar algunas mejoras en el texto.

De vuelta en el gráfico de Dynamo, cambie algunos de los valores de entrada, como la cadena de texto, las coordenadas del punto de inserción, etc. Debería observar cómo se actualiza automáticamente el texto en Civil 3D. Observe también que si desconecta uno de los puertos de entrada, se elimina el texto. Si vuelve a conectar todo, el texto se crea de nuevo. 

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>El gráfico final en acción</p></figcaption></figure>

</div>

{% hint style="info" %} **¿Por qué Dynamo no inserta un nuevo objeto de texto cada vez que se ejecuta el gráfico?**

Por defecto, Dynamo "recordará" los objetos que crea. Si cambia los valores de entrada del nodo, los objetos de Civil 3D se actualizan en lugar de crear objetos nuevos. Obtenga más información sobre este comportamiento en la sección [object-binding.md](advanced-topics/object-binding.md "mention"). {% endhint %}

> :tada: ¡Misión cumplida!

## Siguientes pasos

Este ejemplo es apenas una pequeña muestra de lo que puede hacer con Dynamo for Civil 3D. Siga leyendo para obtener más información.
