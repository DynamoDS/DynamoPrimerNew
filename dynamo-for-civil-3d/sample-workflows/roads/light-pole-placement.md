# Colocación de farolas

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Uno de los muchos y excelentes casos de uso de Dynamo es la colocación dinámica de objetos discretos a lo largo de un modelo de obra lineal. A menudo, es necesario colocar objetos en ubicaciones independientes de los ensamblajes insertados a lo largo de la obra lineal, lo que resulta una tarea muy tediosa de realizar manualmente. Y, cuando la geometría horizontal o vertical de la obra lineal cambia, esto suele ir acompañado de un importante trabajo de rectificación.

## Objetivo

> :dart: Coloque referencias a bloque de farolas a lo largo de una obra lineal en los valores de P.K. especificados de un archivo de Excel. 

## Conceptos clave

> * Lectura de datos de un archivo externo (en este caso, Excel)
> * Organización de datos en diccionarios
> * Uso de sistemas de coordenadas para controlar la posición/escala/rotación
> * Colocación de referencias a bloque
> * Visualización de geometría en Dynamo

## Compatibilidad con versiones

{% hint style="success" %}
 Este gráfico se ejecutará en **Civil 3D 2020** y versiones posteriores. 
{% endhint %}

## Conjunto de datos

Descargue primero los archivos de ejemplo que aparecen a continuación y abra el archivo DWG y el gráfico de Dynamo.

{% hint style="info" %}
 Es preferible que el archivo de Excel se guarde en el mismo directorio que el gráfico de Dynamo. 
{% endhint %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## Solución

A continuación, se ofrece una descripción general de la lógica de este gráfico.

> 1. Leer el archivo de Excel e importar los datos en Dynamo
> 2. Obtener líneas características de la línea base de obra lineal especificada
> 3. Generar sistemas de coordenadas a lo largo de la línea característica de obra lineal en los P.K. deseados
> 4. Utilizar los sistemas de coordenadas para colocar referencias a bloque en el espacio modelo

¡Empecemos!

### Obtener datos de Excel

En este gráfico de ejemplo, vamos a utilizar un archivo de Excel  a fin de almacenar los datos que utilizará Dynamo para colocar las referencias a bloque de farolas. La tabla presenta el siguiente aspecto.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Estructura de la tabla del archivo de Excel</p></figcaption></figure>

{% hint style="info" %}
 El uso de Dynamo para leer datos de un archivo externo (como un archivo de Excel) es una excelente estrategia, sobre todo, cuando los datos deben compartirse con otros miembros del equipo. 
{% endhint %}

Los datos de Excel se importan en Dynamo de este modo. 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Importación de datos de Excel en Dynamo</p></figcaption></figure>

Ahora que tenemos los datos, debemos dividirlos por columna (_Corridor_, _Baseline_, _PointCode_, etc.) para poder utilizarlos en el resto del gráfico. Para ello, lo habitual es utilizar el nodo **List.GetItemAtIndex** y especificar el número de índice de cada columna que deseemos. Por ejemplo, la columna _Corridor_ se encuentra en el índice 0, la columna _Baseline_ en el índice 1, etc.

Parece lógico, ¿verdad? Sin embargo, este planteamiento puede plantear un problema. ¿Qué ocurre si el orden de las columnas del archivo de Excel cambia en el futuro? ¿O se añade una nueva columna entre otras dos? En ese caso, el gráfico no funcionará correctamente y deberá actualizarse. Podemos preparar el gráfico para el futuro incluyendo los datos en un **diccionario**, con los encabezados de las columnas de Excel como _claves_ y el resto de los datos como _valores_.

{% hint style="info" %}
 Si es la primera vez que utiliza estos diccionarios, consulte la sección [5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>Colocación de datos de Excel en un diccionario</p></figcaption></figure>

Esto permite que el gráfico sea más flexible a la hora de cambiar el orden de las columnas en Excel. Mientras los encabezados de las columnas sigan siendo los mismos, los datos podrán recuperarse simplemente del diccionario mediante su _clave_ (es decir, el encabezado de columna), que es lo que haremos a continuación.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>Recuperación de datos del diccionario</p></figcaption></figure>

### Obtener líneas características de obra lineal

Ahora que ya tenemos los datos de Excel importados y listos, empecemos a utilizarlos para obtener información de Civil 3D sobre los modelos de obra lineal.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. Seleccione el modelo de obra lineal por su nombre.
> 2. Obtenga una línea base específica en la obra lineal.
> 3. Obtenga una línea característica dentro de la línea base mediante su código de punto.

### Generar sistemas de coordenadas

Ahora vamos a generar **sistemas de coordenadas** a lo largo de las líneas características de obra lineal en los valores de P.K. especificados del archivo de Excel. Estos sistemas de coordenadas se utilizarán para definir la posición, la rotación y la escala de las referencias a bloque de las farolas.

{% hint style="info" %}
 Si es la primera vez que utiliza los sistemas de coordenadas, consulte la sección [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>Obtención de sistemas de coordenadas a lo largo de las líneas características de obra lineal</p></figcaption></figure>

Observe aquí el uso de un bloque de código para rotar los sistemas de coordenadas en función del lado de la línea base en el que se encuentren. Esto podría lograrse mediante una secuencia de varios nodos, pero este es un buen ejemplo de una situación en la que es más fácil simplemente escribirlo.

{% hint style="info" %}
 Si es la primera vez que utiliza bloques de código, consulte la sección [8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention"). 
{% endhint %}

### Crear referencias a bloque

Ya casi hemos terminado. Disponemos de toda la información necesaria para poder colocar realmente las referencias a bloque. Lo primero que hay que hacer es obtener las definiciones de bloque que deseamos mediante la columna _BlockName_ del archivo de Excel.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>Obtención de las definiciones de bloque que deseamos del documento</p></figcaption></figure>

A continuación, el último paso es crear las referencias a bloque.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>Creación de referencias a bloque en el espacio modelo</p></figcaption></figure>

### Resultado

Al ejecutar el gráfico, deberían aparecer nuevas referencias a bloque en el espacio modelo a lo largo de la obra lineal. Y esta es la mejor parte: si el modo de ejecución del gráfico se ha establecido en Automático y edita el archivo de Excel, las referencias de bloque se actualizan automáticamente.

{% hint style="info" %}
 Puede obtener más información sobre los modos de ejecución de gráficos en la sección [3_user_interface](../../../3\_user\_interface/ "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Realización de cambios en el archivo de Excel y visualización rápida de los resultados en Civil 3D</p></figcaption></figure>

A continuación, se muestra un ejemplo de cómo ejecutar el gráfico con el **Reproductor de Dynamo**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Ejecución del gráfico mediante el Reproductor de Dynamo y visualización de los resultados en Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Si es la primera vez que utiliza el Reproductor de Dynamo, consulte la sección [dynamo-player.md](../../dynamo-player.md "mention"). 
{% endhint %}

> :tada: ¡Misión cumplida!

### Información adicional: visualización en Dynamo

Puede resultar útil visualizar la geometría de obra lineal en Dynamo para ofrecer algo de contexto. Este modelo concreto incluye sólidos de obra lineal extraídos en el espacio modelo, por lo que vamos a incorporarlos en Dynamo. 

Sin embargo, hay algo más que debemos tener en cuenta. Los sólidos son un tipo de geometría relativamente "pesado", lo que significa que esta operación ralentizará el gráfico. Se agradecería que hubiera una forma sencilla de _elegir_ si deseamos ver los sólidos o no. La respuesta obvia es simplemente desconectar el nodo **Corridor.GetSolids**, pero eso generará advertencias para todos los nodos posteriores, lo que resulta algo lioso. Esta es una situación en la que el nodo **ScopeIf** realmente es de gran utilidad.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. Observe que el nodo **Object.Geometry** presenta una barra gris en la parte inferior. Esto significa que la vista preliminar del nodo está desactivada (para acceder a ella, haga clic con el botón derecho en el nodo), lo que permite que **GeometryColor.ByGeometryColor** evite entrar en conflicto con otra geometría para obtener prioridad de visualización en la vista preliminar en segundo plano.
> 2. El nodo **ScopeIf** permite básicamente ejecutar de forma selectiva una ramificación completa de nodos. Si la entrada _test_ se ha establecido en "falso", no se ejecutará ningún nodo conectado al nodo **ScopeIf**.

Este es el resultado de la vista preliminar en segundo plano de Dynamo.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Visualización de la geometría de obra lineal en Dynamo</p></figcaption></figure>

## Ideas

A continuación, se ofrecen algunas ideas sobre cómo podría ampliar las posibilidades de este gráfico.

{% hint style="info" %}
 Añada una columna de **rotación** al archivo de Excel y utilícela para controlar la rotación de los sistemas de coordenadas. 
{% endhint %}

{% hint style="info" %}
 Añada **desfases horizontales o verticales** al archivo de Excel para que las farolas puedan desviarse de la línea característica de obra lineal si es necesario. 
{% endhint %}

{% hint style="info" %}
 En lugar de utilizar un archivo de Excel con valores de P.K., genere estos valores **directamente en Dynamo** mediante un P.K. inicial y una separación típica. 
{% endhint %}
