# Python y Civil 3D

Aunque Dynamo es extremadamente eficaz como herramienta de [programación visual](../../a\_appendix/a-1\_visual-programming-and-dynamo.md), también es posible ir más allá de los nodos y los cables, y escribir código en forma de texto. Existen dos formas de hacerlo:

1. Escribir **DesignScript** mediante un bloque de código
2. Escribir **Python** mediante un nodo de Python

Esta sección se centrará en cómo aprovechar Python en el entorno de Civil 3D para sacar partido de las API de .NET de AutoCAD y Civil 3D.

{% hint style="info" %}
 Consulte la sección [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention") para obtener información sobre cómo usar Python en Dynamo. 
{% endhint %}

## Documentación de las API

Tanto AutoCAD como Civil 3D disponen de varias API que permiten a desarrolladores como usted ampliar el producto principal con funciones personalizadas. En el contexto de Dynamo, lo pertinente son las **API de .NET administrado**. Los siguientes vínculos son esenciales para conocer la estructura de las API y su funcionamiento.

[Manual para desarrolladores de las API de .NET de AutoCAD](https://help.autodesk.com/view/OARX/2024/ESP/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[Manual de referencia de las API de .NET de AutoCAD](https://help.autodesk.com/view/OARX/2024/ESP/?guid=OARX-ManagedRefGuide-What_s_New)

[Manual para desarrolladores de las API de .NET de Civil 3D](https://help.autodesk.com/view/CIV3D/2024/ESP/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Manual de referencia de las API de .NET de Civil 3D](https://help.autodesk.com/view/CIV3D/2024/ESP/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %}
 A medida que avance por esta sección, es posible que haya algunos conceptos con los que no esté familiarizado, como bases de datos, transacciones, métodos, propiedades, etc. Muchos de estos conceptos son fundamentales para trabajar con las API de .NET y no son específicos de Dynamo o Python. Está fuera del alcance de esta sección del manual abordar estos temas en detalle, por lo que se recomienda consultar con frecuencia los vínculos anteriores para obtener más información. 
{% endhint %}

## Plantilla de código

Al editar por primera vez un nuevo nodo de Python, este se rellenará previamente con el código de la plantilla para que pueda empezar. A continuación, se muestra un desglose de la plantilla con explicaciones sobre cada bloque.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>La plantilla de Python por defecto en Civil 3D</p></figcaption></figure>

> 1. Importa los módulos `sys` y `clr`, que son necesarios para que el intérprete de Python funcione correctamente. En concreto, el módulo `clr` permite que los espacios de nombres de .NET se traten básicamente como paquetes de Python.
> 2. Carga los ensamblajes estándar (es decir, los archivos DLL) para trabajar con las API de .NET administrado para AutoCAD y Civil 3D.
> 3. Añade referencias a espacios de nombres estándar de AutoCAD y Civil 3D. Equivalen a las directivas `using` o `Imports` de C# o VB.NET (respectivamente).
> 4. Se puede acceder a los puertos de entrada del nodo mediante una lista predefinida denominada `IN`. Puede acceder a los datos de un puerto específico mediante su número de índice como, por ejemplo, `dataInFirstPort = IN[0]`.
> 5. Obtiene el documento y el editor activos.
> 6. Bloquea el documento e inicia una transacción de base de datos.
> 7. Aquí debe colocar la mayor parte de la lógica de la secuencia de comandos.
> 8. Anule los comentarios de esta línea para confirmar la transacción una vez que haya terminado el trabajo principal.
> 9. Si desea generar datos del nodo, asígnelo a la variable `OUT` al final de la secuencia de comandos.

{% hint style="info" %}
 **¿Desea personalizar la plantilla?**\
 Puede modificar la plantilla de Python por defecto. Para ello, edite el archivo `PythonTemplate.py` ubicado en `C:\ProgramData\Autodesk\C3D <version>\Dynamo`. 
{% endhint %}

## Ejemplo

Veamos un ejemplo para mostrar algunos de los conceptos básicos de la escritura de secuencias de comandos de Python en Dynamo para Civil 3D.

### Objetivo

> :dart: Obtenga la geometría de contorno de todas las cuencas vertientes de un dibujo.

### Conjunto de datos

A continuación, encontrará archivos de ejemplo que puede consultar para este ejercicio.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Descripción general de la solución

A continuación, se ofrece una descripción general de la lógica de este gráfico.

> 1. Consultar la documentación de la API de Civil 3D
> 2. Seleccionar todas las cuencas vertientes del documento por nombre de capa
> 3. "Desenvolver" los objetos de Dynamo para acceder a los miembros internos de la API de Civil 3D
> 4. Crear puntos de Dynamo a partir de puntos de AutoCAD
> 5. Crear PolyCurves a partir de los puntos

¡Empecemos!

### Consultar la documentación de la API

Antes de empezar a crear el gráfico y escribir el código, conviene echar un vistazo a la documentación de la API de Civil 3D y hacerse una idea de lo que esta pone a nuestra disposición. En este caso, hay una [propiedad de la clase Catchment](https://help.autodesk.com/view/CIV3D/2024/ESP/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c) que devolverá los puntos de contorno de la cuenca vertiente. Tenga en cuenta que esta propiedad devuelve un objeto `Point3dCollection`, que Dynamo desconoce cómo utilizar. En otras palabras, no podremos crear una PolyCurve a partir de un objeto `Point3dCollection`, por lo que al final tendremos que convertirlo todo en puntos de Dynamo. Ya abordaremos este tema más adelante.

### Obtener todas las cuencas vertientes

Ahora podemos empezar a crear la lógica del gráfico. Lo primero que hay que hacer es obtener una lista de todas las cuencas vertientes del documento. Hay nodos disponibles para ello, por lo que no necesitamos incluirla en la secuencia de comandos de Python. El uso de nodos proporciona una mejor visibilidad para otra persona que pueda leer el gráfico (en lugar de incluir mucho código en una secuencia de comandos de Python), y, además, mantiene la secuencia de comandos de Python centrada en un único objetivo, devolver los puntos límite de las cuencas vertientes.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Obtener todas las cuencas vertientes del documento por capas</p></figcaption></figure>

Observe que la salida del nodo **All Objects on Layer** es una lista de CivilObjects. Esto se debe a que Dynamo para Civil 3D no dispone actualmente de ningún nodo que permita trabajar con cuencas vertientes, que es el motivo por el que necesitamos acceder a la API a través de Python.

### Desenvoltura de objetos

Antes de continuar, debemos abordar brevemente un concepto importante. En la sección [node-library.md](../node-library.md "mention"), se ha explicado cómo se relacionan los objetos y CivilObjects. Para añadir algo más de detalle, un **objeto de Dynamo** es una envoltura alrededor de una **entidad de AutoCAD**. De forma similar, un **CivilObject de Dynamo** es una envoltura alrededor de una **entidad de Civil 3D**. Puede "desenvolver" un objeto. Para ello, acceda a sus propiedades `InternalDBObject` o `InternalObjectId`.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Tipo de Dynamo</th><th width="373">Envolturas</th></tr></thead><tbody><tr><td><strong>Objeto</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Entidad</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Entidad</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %}
 Por regla general, es más seguro obtener el ID de objeto mediante la propiedad `InternalObjectId` y, a continuación, acceder al objeto envuelto en una transacción. Esto se debe a que la propiedad `InternalDBObject` devolverá un DBObject de AutoCAD que no se encuentra en estado de escritura. 
{% endhint %}

### Secuencia de comandos de Python

A continuación, se muestra la secuencia de comandos completa de Python que realiza el trabajo de acceder a los objetos de cuenca vertiente internos para obtener sus puntos de contorno. Las líneas resaltadas representan las que se han modificado o añadido a partir del código de plantilla por defecto.

{% hint style="info" %}
 Haga clic en el texto subrayado en la secuencia de comandos para obtener una explicación de cada línea. 
{% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Cargar las bibliotecas de normas y DesignScript de Python
import sys
import clr

# Añadir ensamblajes para AutoCAD y Civil 3D
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Importar referencias de AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Importar referencias de Civil 3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# Las entradas de este nodo se almacenarán como una lista en las variables IN.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("La entrada es nula o está vacía.")</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>    
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>              
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>                
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                    
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Validar antes de finalizar la transacción
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Asigne la salida a la variable OUT.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %}
 Como regla general, es recomendable incluir la mayor parte de la lógica de la secuencia de comandos en una transacción. De este modo, se garantiza un acceso seguro a los objetos que la secuencia de comandos está leyendo o escribiendo. En muchos casos, omitir una transacción puede provocar un error grave. 
{% endhint %}

### Crear PolyCurves

En esta fase, la secuencia de comandos de Python debería generar una lista de puntos de Dynamo que se pueden ver en la vista preliminar en segundo plano. El último paso consiste en crear simplemente PolyCurves a partir de los puntos. Tenga en cuenta que esto también podría hacerse directamente en la secuencia de comandos de Python, pero lo hemos puesto intencionadamente fuera de ella en un nodo para que resulte más visible. Este es el aspecto del gráfico final.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>El gráfico final</p></figcaption></figure>

### Resultado

Y esta es la geometría final de Dynamo.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>Las PolyCurves de Dynamo resultantes para los contornos de cuenca vertiente</p></figcaption></figure>

> :tada: ¡Misión cumplida!

## Comparación de IronPython y CPython

Apenas una nota rápida aquí antes de terminar. En función de la versión de Civil 3D que esté utilizando, es posible que el nodo de Python se haya configurado de forma diferente. En **Civil 3D 2020 y 2021**, Dynamo utilizaba una herramienta denominada **IronPython** para desplazar datos entre objetos .NET y secuencias de comandos de Python. Sin embargo, en **Civil 3D 2022**, Dynamo realizó la transición al intérprete nativo de Python estándar (también conocido como **CPython**) que utiliza Python 3. Las ventajas de esta transición incluyen el acceso a las bibliotecas modernas más conocidas y a las nuevas funciones de la plataforma, el mantenimiento esencial y los parches de seguridad.

{% hint style="info" %}
 Puede obtener más información sobre esta transición y sobre cómo actualizar las secuencias de comandos existentes en el [blog de Dynamo](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). Si desea seguir utilizando IronPython, solo tendrá que instalar el paquete de **DynamoIronPython2.7** mediante Dynamo Package Manager. 
{% endhint %}

[^1]: Por defecto, la biblioteca de geometría de Dynamo no se añade al entorno de Python. Nuestro objetivo con esta secuencia de comandos es generar una lista de puntos de Dynamo para los contornos de cuenca vertiente, por lo que es necesario añadir esta línea para crear los puntos más adelante.

[^2]: Esta línea obtiene la clase específica que necesitamos de la biblioteca de geometría de Dynamo. Tenga en cuenta que aquí se especifica `import Point as DynPoint` en lugar de `import *`, ya que este último elemento provocaría conflictos de nombres.

[^3]: Aquí cambiamos el nombre de la variable `dataEnteringNode` por defecto a `objs` para que sea más descriptiva.

[^4]: Aquí especificamos exactamente el puerto de entrada que contiene los datos que deseamos en lugar del elemento `IN` por defecto, que hace referencia a la lista completa de todas las entradas.

[^5]: Esto crea una lista vacía a la que añadiremos los datos de salida más adelante.

[^6]: Esto sirve para protegerse frente a una posible entrada vacía o nula en la secuencia de comandos.

[^7]: En lugar de interrumpir la secuencia de comandos, se mostrará un mensaje útil para explicar por qué no puede continuar la secuencia de comandos.

[^8]: En el resto de la secuencia de comandos, se presupone que tenemos una lista de objetos con la que trabajar (en lugar de un único objeto). Sin embargo, en el caso de que solo haya un objeto (es decir, un dibujo con una sola cuenca vertiente), seguiremos deseando que la secuencia de comandos pueda ejecutarse.

[^9]: Si la entrada no es una lista (es decir, un solo objeto), entonces crearemos simplemente una nueva lista con el objeto como único elemento.

[^10]: Inicie un bucle para cada objeto de la lista.

[^11]: "Desenvuelva" el objeto de Dynamo obteniendo su ID de objeto.

[^12]: Recupere el objeto "envuelto" de la base de datos de AutoCAD. Observe que aquí el OpenMode se ha establecido en `ForRead` porque no pensamos realizar ninguna modificación en los objetos. Simplemente estamos "consultando" datos.

[^13]: Es posible que la lista de objetos de entrada contenga una mezcla de cuencas vertientes y otros elementos que no lo sean. Debemos comprobar esta situación y gestionarla adecuadamente (es decir, solo continuar esta iteración del bucle si el elemento es efectivamente una cuenca vertiente).

[^14]: Si la secuencia de comandos llega a este punto, sabemos que el objeto es realmente una cuenca vertiente. Añadiremos una nueva variable aquí solo para mantener la nomenclatura clara y comprensible.

[^15]: Aquí es donde recuperamos los puntos límite de la cuenca vertiente mediante la propiedad adecuada que buscamos anteriormente en los documentos de la API. Como se ha mencionado anteriormente, esto nos proporcionará un objeto `Point3dCollection`, que es esencialmente una lista de puntos de AutoCAD. Tendremos que "convertirlos" en puntos de Dynamo para que resulten útiles.

[^16]: Cree una lista vacía para almacenar los puntos de contorno de esta cuenca vertiente.

[^17]: Inicie un bucle para cada objeto `Point3d` en `Point3dCollection`.

[^18]: Cree un punto de Dynamo mediante las coordenadas del punto de AutoCAD.

[^19]: Añada el punto a la lista.

[^20]: Una vez que hayamos terminado de "convertir" todos los puntos de AutoCAD en puntos de Dynamo, añadiremos la lista resultante a nuestra salida. A continuación, el bucle exterior pasará al siguiente objeto de la lista de entrada.

[^21]: Anule los comentarios de esta línea para confirmar la transacción.

[^22]: Y, por último, pero no por ello menos importante, generamos la lista de puntos de Dynamo.
