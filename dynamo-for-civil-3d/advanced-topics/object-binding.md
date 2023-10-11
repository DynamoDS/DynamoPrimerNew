# Enlace de objetos

Dynamo para Civil 3D contiene un mecanismo muy eficaz para "recordar" los objetos que crea cada nodo. Este mecanismo se denomina **Enlace de objetos** y permite que un gráfico de Dynamo genere resultados coherentes cada vez que se ejecuta en el mismo documento. Aunque esto es muy deseable en muchas situaciones, hay otras en las que es posible que desee tener más control sobre el comportamiento de Dynamo. Esta sección le ayudará a comprender cómo funciona el enlace de objetos y cómo puede aprovecharlo.

## Ejemplo

Observe este gráfico que crea un círculo en el espacio modelo de la capa actual.

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>Un gráfico sencillo para la creación de un círculo</p></figcaption></figure>

Observe lo que sucede cuando se cambia el radio.

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>Modificación de la entrada de radio en Dynamo</p></figcaption></figure>

Esto es un enlace de objetos en acción. El comportamiento por defecto de Dynamo es _modificar_ el radio del círculo en lugar de crear un nuevo círculo cada vez que se modifica la entrada del radio. Esto se debe a que el nodo **Object.ByGeometry** "recuerda" que crea este círculo _específico_ cada vez que se ejecuta el gráfico. Además, Dynamo guardará esta información para que la próxima vez que abra el documento de Civil 3D y ejecute el gráfico, este presente exactamente el mismo comportamiento.

## Otro ejemplo

Veamos un ejemplo en el que puede que desee cambiar el comportamiento por defecto de enlace de objetos de Dynamo. Supongamos que desea crear un gráfico que coloque texto en el centro de un círculo. Sin embargo, lo que pretende con este gráfico es que pueda ejecutarse una y otra vez, y colocar cada vez un nuevo texto para cualquier Círculo que se seleccione. Este es el aspecto que presentaría el gráfico.

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>Un gráfico sencillo que coloca el texto en el centro del círculo seleccionado</p></figcaption></figure>

Sin embargo, esto es lo que sucede realmente cuando se selecciona un círculo diferente.

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>Comportamiento por defecto de Dynamo al seleccionar un nuevo círculo</p></figcaption></figure>

Parece que el texto se suprime y se vuelve a crear con cada ejecución del gráfico. En realidad, la posición del texto se _modifica_ en función del círculo seleccionado. Por lo tanto, es el mismo texto, solo que en un lugar diferente. Para crear un nuevo texto cada vez, debemos modificar la configuración de enlace de objetos de Dynamo para que no se conserve ningún dato de enlace (consulte [\#binding-settings](object-binding.md#binding-settings "mention") a continuación).

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Parámetros de enlace de objetos</p></figcaption></figure>

Tras realizar ese cambio, obtenemos el comportamiento que buscamos.

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>Comportamiento con el enlace de objetos desactivado</p></figcaption></figure>

## Parámetros de enlace

Dynamo para Civil 3D permite modificar el comportamiento por defecto de enlace de objetos a través de los parámetros de **Almacenamiento de datos de enlace** en el menú de **Dynamo**.

{% hint style="info" %}
 Tenga en cuenta que las opciones de almacenamiento de datos de enlace están disponibles en **Civil 3D 2022.1** y versiones posteriores. 
{% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

Todas las opciones están activadas por defecto. A continuación, se resumen las acciones de cada opción.

### Opción 1: No se conservan datos de enlace

Si esta opción está activada, Dynamo "olvidará" los objetos que creó la última vez que se ejecutó el gráfico. De este modo, el gráfico puede ejecutarse en cualquier dibujo y situación, y creará nuevos objetos cada vez.

{% hint style="info" %}
 **Cuándo utilizar esta opción**

Utilice esta opción cuando desee que Dynamo "olvide" todo lo que ha realizado en ejecuciones anteriores y cree nuevos objetos cada vez. 
{% endhint %}

### Opción 2: Guardar en el gráfico para Dynamo

Esta opción permite serializar los metadatos de enlace de objetos en el gráfico (archivo .dyn) al guardarlo. Si cierra o vuelve a abrir el gráfico y lo ejecuta en el **mismo dibujo**, todo debería funcionar igual que lo dejó. Si ejecuta el gráfico en un **dibujo diferente**, los datos de enlace se eliminarán del gráfico y se crearán nuevos objetos. Esto significa que, si abre el dibujo original y vuelve a ejecutar el gráfico, se crearán nuevos objetos, además de los antiguos.

{% hint style="info" %}
 **Cuándo utilizar esta opción**

Utilice esta opción cuando desee que Dynamo "recuerde" los objetos que creó la última vez que se ejecutó en un **dibujo específico**. 
{% endhint %}

{% hint style="warning" %}
 Esta opción es la más adecuada para situaciones en las que es posible mantener una relación de 1:1 entre un **dibujo específico** y un gráfico de Dynamo. Las opciones 1 y 3 son más idóneas para gráficos diseñados para ejecutarse en varios dibujos. 
{% endhint %}

### Opción 3: Guardar en dibujo para Dynamo

Es similar a la opción 2, salvo que los datos de enlace de objetos se serializan en el dibujo en lugar de en el gráfico (archivo .dyn). Si cierra o vuelve a abrir el gráfico y lo ejecuta en el **mismo dibujo**, todo debería funcionar igual que lo dejó. Si ejecuta el gráfico en un **dibujo diferente**, los datos de enlace se conservan en el dibujo original, ya que se guardan en el dibujo y no en el gráfico.

{% hint style="info" %}
 **Cuándo utilizar esta opción**

Utilice esta opción cuando desee utilizar el mismo gráfico en **varios dibujos** y conseguir que Dynamo "recuerde" lo que realizó en cada uno de ellos. 
{% endhint %}

### Opción 4: Guardar en dibujo para el Reproductor de Dynamo

Lo primero que hay que tener en cuenta con esta opción es que no afecta al modo en que el gráfico interactúa con el dibujo cuando se ejecuta el gráfico a través de la interfaz principal de Dynamo. Esta opción _solo_ se aplica cuando el gráfico se ejecuta con el Reproductor de Dynamo.

{% hint style="info" %}
 Si es la primera vez que utiliza el Reproductor de Dynamo, consulte la sección [dynamo-player.md](../dynamo-player.md "mention"). 
{% endhint %}

Si ejecuta el gráfico mediante la interfaz principal de Dynamo y, a continuación, cierra y ejecuta el mismo gráfico mediante el Reproductor de Dynamo, este creará nuevos objetos sobre los que creó anteriormente. Sin embargo, una vez que el Reproductor de Dynamo haya ejecutado el gráfico una vez, serializará los datos de enlace de objetos en el dibujo. Por lo tanto, si ejecuta el gráfico varias veces a través del Reproductor de Dynamo, este actualizará los objetos en lugar de crear otros nuevos. Si ejecuta el gráfico a través del Reproductor de Dynamo en un **dibujo diferente**, los datos de enlace seguirán conservándose en el dibujo original, ya que se guardan en el dibujo y no en el gráfico.

{% hint style="info" %}
 **Cuándo utilizar esta opción**

Utilice esta opción cuando desee ejecutar un gráfico con el Reproductor de Dynamo en varios dibujos y que este "recuerde" lo que realizó en cada uno de ellos. 
{% endhint %}
