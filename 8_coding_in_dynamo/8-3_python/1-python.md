# Nodos de Python

¿Por qué habría que utilizar la programación textual en el entorno de programación visual de Dynamo? La [programación visual](../../a\_appendix/a-1\_visual-programming-and-dynamo.md) tiene muchas ventajas. Permite crear programas sin necesidad de aprender sintaxis especial en una interfaz visual intuitiva. Sin embargo, un programa visual se puede sobrecargar y, a veces, su funcionalidad puede ser reducida. Por ejemplo, Python ofrece métodos mucho más eficaces para escribir instrucciones condicionales (if/then) y bucles. Python es una potente herramienta que permite ampliar las funciones de Dynamo y reemplazar muchos nodos por unas pocas líneas de código concisas.

**Programa visual:**

![](../images/8-3/1/pythonnode-visualvstextualprogramming.jpg)

**Programa textual:**

```py
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

solid = IN[0]
seed = IN[1]
xCount = IN[2]
yCount = IN[3]

solids = []

yDist = solid.BoundingBox.MaxPoint.Y-solid.BoundingBox.MinPoint.Y
xDist = solid.BoundingBox.MaxPoint.X-solid.BoundingBox.MinPoint.X

for i in xRange:
	for j in yRange:
		fromCoord = solid.ContextCoordinateSystem
		toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin,Vector.ByCoordinates(0,0,1),(90*(i+j%val)))
		vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
		toCoord = toCoord.Translate(vec)
		solids.append(solid.Transform(fromCoord,toCoord))

OUT = solids
```

### El nodo de Python

Al igual que los bloques de código, los nodos de Python son una interfaz de secuencias de comandos dentro de un entorno de programación visual. El nodo de Python se encuentra en la biblioteca, en Secuencias de comandos > Editor > Secuencia de comandos de Python.

![](../images/8-3/1/pythonnode-thepythonnode01.jpg)

Al hacer doble clic en el nodo, se abre el editor de secuencias de comandos de Python (también puede hacer clic con el botón derecho en el nodo y seleccionar _Editar_). Observará que aparece texto modelo en la parte superior como ayuda para hacer referencia a las bibliotecas necesarias. Las entradas se almacenan en la matriz IN. Los valores se devuelven a Dynamo asignándolos a la variable OUT.

![](../images/8-3/1/pythonnode-thepythonnode02.jpg)

La biblioteca Autodesk.DesignScript.Geometry permite utilizar la notación de puntos como ocurre con los bloques de código. Para obtener más información sobre la sintaxis de Dynamo, consulte [7-2_design-script-syntax.md](../../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md "mention") y la [Guía de DesignScript](https://dynamobim.org/wp-content/links/DesignScriptGuide.pdf) (Para descargar este documento PDF, haga clic con el botón derecho en el vínculo y seleccione "Guardar vínculo como"). Al escribir un tipo de geometría como, por ejemplo, "Point.", se muestra una lista de métodos para crear y consultar puntos.

![](../images/8-3/1/pythonnode-thepythonnode03.jpg)

> Los métodos incluyen constructores como _ByCoordinates_, acciones como _Add_ y consultas como las coordenadas _X_, _Y_ y _Z_.

## Ejercicio: nodo personalizado con secuencia de comandos de Python para crear patrones a partir de un módulo sólido

### Parte I: configurar la secuencia de comandos de Python

> Descargue el archivo de ejemplo. Para ello, haga clic en el vínculo siguiente.
>
> En el Apéndice, se incluye una lista completa de los archivos de ejemplo.

{% file src="../datasets/8-2/1/Python_Custom-Node.dyn" %}

En este ejemplo, vamos a escribir una secuencia de comandos de Python que crea patrones a partir de un módulo sólido y lo vamos a convertir en un nodo personalizado. Vamos a crear primero nuestro módulo sólido mediante nodos de Dynamo.

![](../images/8-3/1/pythonnode-exerciseptI-01.jpg)

> 1. **Rectangle.ByWidthLength**: cree un rectángulo que será la base del sólido.
> 2. **Surface.ByPatch**: conecte el rectángulo con la entrada "_closedCurve_" para crear la superficie inferior.

![](../images/8-3/1/pythonnode-exerciseptI-02.jpg)

> 1. **Geometry.Translate**: conecte el rectángulo a la entrada "_geometry_" para desplazarlo hacia arriba y utilice un bloque de código para especificar el grosor de base del sólido.
> 2. **Polygon.Points**: consulte el rectángulo trasladado para extraer los puntos de esquina.
> 3. **Geometry.Translate**: utilice un bloque de código para crear una lista de cuatro valores correspondientes a los cuatro puntos y traslade una esquina del sólido hacia arriba.
> 4. **Polygon.ByPoints**: utilice los puntos trasladados para reconstruir el polígono superior.
> 5. **Surface.ByPatch**: conecte el polígono para crear la superficie superior.

Ahora que tenemos las superficies superior e inferior, solevaremos los dos perfiles para crear los lados del sólido.

![](../images/8-3/1/pythonnode-exerciseptI-03.jpg)

> 1. **List.Create**: conecte el rectángulo inferior y el polígono superior a las entradas de índice.
> 2. **Surface.ByLoft**: soleve los dos perfiles para crear los lados del sólido.
> 3. **List.Create**: conecte las superficies superior, lateral e inferior a las entradas de índice para crear una lista de superficies.
> 4. **Solid.ByJoinedSurfaces**: una las superficies para crear el módulo sólido.

Ahora que ya tenemos el sólido, soltaremos un nodo de secuencia de comandos de Python en el espacio de trabajo.

![](../images/8-3/1/pythonnode-exerciseptI-04.jpg)

> 1. Para añadir entradas adicionales al nodo, haga clic en el icono + del nodo. Las entradas se denominan IN[0], IN[1] y así sucesivamente para indicar que representan elementos de una lista.

Empezaremos definiendo las entradas y la salida. Haga doble clic en el nodo para abrir el editor de Python. Siga el código mostrado a continuación para modificarlo en el editor.

![](../images/8-3/1/pythonnode-exerciseptI-05.jpg)

```py
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []

# Place your code below this line


# Assign your output to the OUT variable.
OUT = solids
```

Este código cobrará más sentido a medida que avancemos en el ejercicio. A continuación, debemos pensar en qué información se requiere para disponer la matriz de nuestro módulo sólido. En primer lugar, es necesario conocer las dimensiones del sólido para determinar la distancia de traslación. Debido a un error del cuadro delimitador, tendremos que utilizar la geometría de curva de borde para crear un cuadro delimitador.

![](../images/8-3/1/python07.png)

> Eche un vistazo al nodo de Python en Dynamo. Observe que estamos utilizando la misma sintaxis que vemos en los títulos de los nodos de Dynamo. Eche un vistazo al código comentado que aparece a continuación.

```py
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

# Assign your output to the OUT variable.
OUT = solids
```

Ya que vamos a trasladar y girar los módulos sólidos, vamos a utilizar la operación Geometry.Transform. Al observar el nodo Geometry.Transform, vemos que vamos a necesitar un sistema de coordenadas de origen y un sistema de coordenadas de destino para transformar el sólido. El origen es el sistema de coordenadas de contexto del sólido, mientras que el destino será un sistema de coordenadas diferente para cada módulo de matriz. Esto significa que tendremos que crear un bucle a través de los valores de X e Y para transformar el sistema de coordenadas de forma diferente en cada caso.

![](../images/8-3/1/pythonnode-exerciseptI-06.jpg)

```py
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

#Get the source coordinate system
fromCoord = solid.ContextCoordinateSystem

#Loop through x and y
for i in range(xCount):
    for j in range(yCount):
        #Rotate and translate the coordinate system
        toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin, Vector.ByCoordinates(0,0,1), (90*(i+j%seed)))
        vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
        toCoord = toCoord.Translate(vec)
        #Transform the solid from the source coord syste, to the target coord system and append to the list
        solids.append(solid.Transform(fromCoord,toCoord))

# Assign your output to the OUT variable.
OUT = solids
```

Haga clic en Ejecutar y, a continuación, guarde el código. Conecte el nodo de Python con la secuencia de comandos existente como se indica a continuación.

![](../images/8-3/1/pythonnode-exerciseptI-07.jpg)

> 1. Conecte la salida de **Solid.ByJoinedSurfaces** como la primera entrada para el nodo de Python y utilice un bloque de código para definir las demás entradas.
> 2. Cree un nodo **Topology.Edges** y utilice la salida del nodo de Python como entrada.
> 3. Por último, cree un nodo **Edge.CurveGeometry** y utilice la salida de Topology.Edges como entrada.

Pruebe a cambiar el valor de semilla para crear patrones diferentes. También puede cambiar los parámetros del módulo sólido para obtener distintos efectos.

![](../images/8-3/1/python10.png)

### Parte II: convertir el nodo de la secuencia de comandos de Python en un nodo personalizado

Ahora que hemos creado una secuencia de comandos de Python útil, vamos a guardarla como un nodo personalizado. Seleccione el nodo de la secuencia de comandos de Python, haga clic con el botón derecho en el espacio de trabajo y seleccione "Crear nodo personalizado".

![](../images/8-3/1/pythonnode-exerciseptII-01.jpg)

Asigne un nombre, una descripción y una categoría.

![](../images/8-3/1/pythonnode-exerciseptII-02.jpg)

Se abrirá un nuevo espacio de trabajo en el que se puede editar el nodo personalizado.

![](../images/8-3/1/pythonnode-exerciseptII-03.jpg)

> 1. **Inputs**: cambie los nombres de las entradas para que sean más descriptivos y añada tipos de datos y valores por defecto.
> 2. **Output**: cambie el nodo de la salida.

Guarde el nodo como un archivo .dyf; debería ver que el nodo personalizado refleja los cambios que acabamos de realizar.

![](../images/8-3/1/pythonnode-exerciseptII-04.jpg)
