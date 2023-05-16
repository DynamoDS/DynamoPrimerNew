# Publicar un paquete

### Publicar un paquete <a href="#publish-a-package" id="publish-a-package"></a>

Los paquetes permiten almacenar y compartir de forma cómoda nodos con la comunidad de Dynamo. Un paquete puede contener todo tipo de elementos, desde nodos personalizados creados en el espacio de trabajo de Dynamo hasta nodos derivados de NodeModel. Los paquetes se publican y se instalan mediante Package Manager. Además de esta página, [Dynamo Primer](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction) incluye una guía general sobre los paquetes.

#### ¿Qué es Package Manager? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

La herramienta Package Manager de Dynamo es un registro de software (similar a npm) al que se puede acceder desde Dynamo o en un navegador web. Package Manager permite la instalación, la publicación, la actualización y la visualización de paquetes. Al igual que npm, conserva diferentes versiones de los paquetes. También ayuda a administrar las dependencias del proyecto.

En el navegador, busque paquetes y vea estadísticas. Para ello, visite [https://dynamopackages.com/](https://dynamopackages.com).

* En Dynamo, Package Manager permite la instalación, la publicación y la actualización de paquetes.

![Búsqueda de paquetes](images/dynamopackagemanager.jpg)

> 1. Busque paquetes en línea mediante `Packages > Search for a Package...`.
> 2. Vea o edite los paquetes instalados mediante `Packages > Manage Packages...`.
> 3. Publique un nuevo paquete `Packages > Publish New Package...`.

#### Publicación de un paquete <a href="#publishing-a-package" id="publishing-a-package"></a>

Los paquetes se publican desde Package Manager en Dynamo. El proceso recomendado es publicar localmente, probar el paquete y, a continuación, publicar en línea para compartirlo con la comunidad. Mediante el caso real de NodeModel, vamos a seguir los pasos necesarios para publicar localmente el nodo RectangularGrid como un paquete y, a continuación, en línea.

Inicie Dynamo y seleccione `Packages > Publish New Package...` para abrir la ventana `Publish a Package`.

![Publicación de un paquete](images/dyn-publish-package-add-files.jpg)

> 1. Seleccione `Add file...` para buscar archivos que añadir al paquete.
> 2. Seleccione los dos archivos `.dll` del caso real de NodeModel.
> 3. Seleccione `Ok`.

Una vez que se hayan añadido los archivos al contenido del paquete, asigne al paquete un nombre, una descripción y una versión. Al publicar un paquete mediante Dynamo, se crea automáticamente un archivo `pkg.json`.

![Configuración del paquete](images/dyn-publish-package.jpg)

> Un paquete listo para su publicación.
>
> 1. Proporcione la información necesaria para el nombre, la descripción y la versión.
> 2. Haga clic en "Publicar localmente" para publicar y seleccione la carpeta de paquetes de Dynamo, `AppData\Roaming\Dynamo\Dynamo Core\1.3\packages`, para que el nodo esté disponible en Core. Realice siempre la publicación localmente hasta que el paquete esté listo para compartirse.

Después de publicar un paquete, los nodos estarán disponibles en la biblioteca de Dynamo, en la categoría `CustomNodeModel`.

![Paquete en la biblioteca de Dynamo](images/dyn-publish-package-library.jpg)

> 1. El paquete que acabamos de crear en la biblioteca de Dynamo

Una vez que el paquete esté listo para publicarse en línea, abra Package Manager, y seleccione `Publish` y, a continuación, `Publish Online`.

![Publicar un paquete en Package Manager](images/dyn-publish-package-directory.jpg)

> 1. Para ver cómo Dynamo ha aplicado formato al paquete, haga clic en los tres puntos verticales que aparecen a la derecha de "CustomNodeModel" y elija "Mostrar directorio raíz".
> 2. Seleccione `Publish` y, a continuación, `Publish Online` en la ventana "Publicar un paquete de Dynamo".
> 3. Para suprimir un paquete, seleccione `Delete`.

#### ¿Cómo se actualiza un paquete? <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

La actualización de un paquete es un proceso similar al de su publicación. Abra Package Manager, seleccione `Publish Version...` en el paquete que debe actualizarse y especifique una versión posterior.

![Publicar una versión del paquete](images/dyn-publish-package-version.jpg)

> 1. Seleccione `Publish Version` para actualizar un paquete existente con nuevos archivos en el directorio raíz y, a continuación, elija si debe publicarse localmente o en línea.

#### Cliente web de Package Manager <a href="#package-manager-web-client" id="package-manager-web-client"></a>

El cliente web de Package Manager se utiliza exclusivamente para buscar y ver datos de paquetes, como el control de versiones y las estadísticas de descarga.

Se puede acceder al cliente web de Package Manager en el siguiente vínculo: [https://dynamopackages.com/](https://dynamopackages.com).

![Cliente web de Package Manager](images/packagemanager-browser.jpg)
