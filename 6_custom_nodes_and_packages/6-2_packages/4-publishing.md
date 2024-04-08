# Publicación de un paquete

En las secciones anteriores, examinamos cómo se configura el paquete de _MapToSurface_ con nodos personalizados y archivos de ejemplo. Pero, ¿cómo publicamos un paquete que se ha desarrollado localmente? En este caso real, se muestra cómo publicar un paquete a partir de un conjunto de archivos en una carpeta local.

\![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

Hay muchas formas de publicar un paquete. A continuación, se indica el proceso recomendado: **publique localmente, desarrolle localmente y, a continuación, publique en línea**. Comenzaremos con una carpeta que contiene todos los archivos del paquete.

### Desinstalación de un paquete

Antes de pasar a la publicación del paquete MapToSurface, si ha instalado el paquete en una lección anterior, desinstálelo para que no trabaje con paquetes idénticos.

Vaya primero a Paquetes > Package Manager > ficha Paquetes instalados > junto a MapToSurface, haga clic en el menú de puntos verticales > Suprimir.

<figure><img src="../../.gitbook/assets/delete-map-to-surface.png" alt=""><figcaption></figcaption></figure>

A continuación, reinicie Dynamo. Al volver a abrirlo, cuando active la ventana _"Gestionar paquetes"_, _MapToSurface_ ya no debería aparecer. Ya estamos listos para empezar desde el principio.

### Publicación local de un paquete

{% hint style="warning" %} Puede publicar nodos y paquetes personalizados desde Dynamo Sandbox en la versión 2.17 y posteriores, siempre que no presenten dependencias de la API del anfitrión. En versiones anteriores, la publicación de nodos y paquetes personalizados solo estaba activada en Dynamo for Revit y Dynamo para Civil 3D. {% endhint %}

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

Este es el primer envío del paquete y se han incluido todos los nodos personalizados y los archivos de ejemplo en una única carpeta. Con esta carpeta preparada, ya podemos realizar la carga en Dynamo Package Manager.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Esta carpeta contiene cinco nodos personalizados (.dyf).
> 2. Esta carpeta también contiene cinco archivos de ejemplo (.dyn) y un archivo de vectores importado (.svg). Estos archivos se utilizarán como ejercicios de introducción para mostrar al usuario cómo trabajar con los nodos personalizados.

En Dynamo, haga clic primero en _Paquetes > Package Manager > ficha Publicar paquete nuevo_.

En la ficha _Publicar un paquete_, rellene los campos correspondientes en el lado izquierdo de la ventana.

<figure><img src="../../.gitbook/assets/package-details.png" alt=""><figcaption></figcaption></figure>

A continuación, añadiremos archivos del paquete. Puede añadir archivos individualmente o carpetas enteras si selecciona Añadir directorio (1). Para añadir archivos que no sean .dyf, asegúrese de cambiar el tipo de archivo en la ventana del navegador a **"Todos los archivos (**_._**)"**. Observe que añadiremos sin distinción todos los archivos, los nodos personalizados (.dyf) o los archivos de ejemplo (.dyn). Dynamo organizará estos elementos en categorías cuando publiquemos el paquete.

<figure><img src="../../.gitbook/assets/map-to-surface-contents.png" alt=""><figcaption></figcaption></figure>

Una vez que haya seleccionado la carpeta MapToSurface, Package Manager le mostrará su contenido. Si va a cargar su propio paquete con una estructura de carpetas compleja y no desea que Dynamo realice cambios en ella, puede activar el conmutador "Conservar estructura de carpetas". Esta opción es para usuarios avanzados y, si el paquete no se ha configurado deliberadamente de una manera específica, lo mejor es dejar este conmutador desactivado y permitir que Dynamo organice los archivos según sea necesario. Pulse Siguiente para continuar.

<figure><img src="../../.gitbook/assets/map-to-surface-contents-preview.png" alt=""><figcaption></figcaption></figure>

Aquí tiene la oportunidad de previsualizar cómo organizará Dynamo los archivos del paquete antes de publicarlo. Haga clic en Finalizar para continuar.

<figure><img src="../../.gitbook/assets/publish-locally.png" alt=""><figcaption></figcaption></figure>

Para la publicación, haga clic en "Publicar localmente" (1). Si sigue este proceso, asegúrese de hacer clic en _"Publicar localmente"_ y **no** en _"Publicar en línea"_. No deseamos un conjunto de paquetes duplicados en Package Manager.

Tras la publicación, los nodos personalizados deberían estar disponibles en el grupo "DynamoPrimer" o en la biblioteca de Dynamo.

\![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Ahora veamos el directorio raíz para comprobar cómo Dynamo ha aplicado formato al paquete que acabamos de crear. Para ello, vaya a la ficha Paquetes instalados > junto a MapToSurface, haga clic en el menú de puntos verticales > seleccione Mostrar directorio raíz.

<figure><img src="../../.gitbook/assets/show-root-directory.png" alt=""><figcaption></figcaption></figure>

Observe que el directorio raíz se encuentra en la ubicación local del paquete (recuerde que hemos publicado el paquete "localmente"). Dynamo hace referencia a esta carpeta para leer nodos personalizados. Por lo tanto, es importante publicar localmente el directorio en una ubicación de carpeta permanente (es decir, no en el escritorio). A continuación, se desglosan las carpetas del paquete de Dynamo.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. La carpeta _bin_ contiene archivos .dll creados con bibliotecas de C# o Zero-Touch. No hay ninguna para este paquete, por lo que esta carpeta aparece vacía en este ejemplo.
> 2. La carpeta _dyf_ contiene los nodos personalizados. Al abrir esta, se mostrarán todos los nodos personalizados (archivos .dyf) de este paquete.
> 3. La carpeta "extra" contiene todos los archivos adicionales. Es probable que estos archivos sean archivos de Dynamo (.dyn) o archivos adicionales necesarios (.svg, .xls, .jpeg, .sat, etc.).
> 4. El archivo pkg es un archivo de texto básico que define los parámetros del paquete. Esta función está automatizada en Dynamo, pero se puede modificar si desea acceder a los detalles.

### Publicación de un paquete en línea

{% hint style="warning" %} Nota: No siga este paso a menos que publique realmente un paquete suyo. {% endhint %}

<figure><img src="../../.gitbook/assets/publish-version.png" alt=""><figcaption></figcaption></figure>

1. Cuando desee publicarlo, en Paquetes > Package Manager > ventana Paquetes instalados, seleccione el botón situado a la derecha del paquete que desee publicar y elija Publicar.
2. Si va a actualizar un paquete que ya se ha publicado, seleccione Publicar versión; Dynamo actualizará el paquete en línea en función de los nuevos archivos del directorio raíz del paquete. Así de fácil.

### Publicar versión

Al actualizar los archivos de la carpeta raíz del paquete publicado, puede publicar también una nueva versión del paquete. Para ello, seleccione _"Publicar versión"_ en la ventana _Mis paquetes_. Esta es una forma perfecta de realizar las actualizaciones necesarias del contenido y compartirlas con la comunidad. _Publicar versión_ solo funcionará si es la persona encargada del mantenimiento del paquete.
