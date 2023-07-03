# Conceptos avanzados de Zero-Touch

Una vez que sabemos cómo crear un proyecto Zero-Touch, podemos profundizar en los detalles de la creación de un nodo mediante el ejemplo ZeroTouchEssentials en el GitHub de Dynamo.

![Nodos Zero-Touch](images/ootbzerotouch.png)

> Muchos de los nodos estándar de Dynamo son básicamente nodos Zero-Touch, como la mayoría de los nodos Math, Color y DateTime anteriores.

Para empezar, descargue el proyecto ZeroTouchEssentials desde aquí: [https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials).

En Visual Studio, abra el archivo `ZeroTouchEssentials.sln` y compile la solución.

![ZeroTouchEssentials en Visual Studio](images/vs-build-zte.jpg)

> El archivo `ZeroTouchEssentials.cs` contiene todos los métodos que se importarán en Dynamo.

Abra Dynamo e importe `ZeroTouchEssentials.dll` para obtener los nodos a los que haremos referencia en los ejemplos siguientes.

Los ejemplos de código se extraen de [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs) y, en general, coinciden con él. Se ha eliminado la documentación XML para mantener la brevedad y cada ejemplo de código creará el nodo de la imagen superior.

#### Valores de entrada por defecto <a href="#default-input-values" id="default-input-values"></a>

Dynamo admite la definición de valores por defecto para los puertos de entrada de un nodo. Estos valores por defecto se proporcionarán al nodo si los puertos no tienen conexiones. Los valores por defecto se expresan mediante el mecanismo de C# de especificación de argumentos opcionales en el [manual de programación de C#](https://msdn.microsoft.com/en-us/library/dd264739.aspx). Los valores por defecto se especifican de la siguiente forma:

* Establezca los parámetros del método en un valor por defecto: `inputNumber = 2.0`.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // Set the method parameter to a default value
        public static double MultiplyByTwo(double inputNumber = 2.0) 
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Valor por defecto](images/defaultval.jpg)

> 1. El valor por defecto se mostrará al colocar el cursor sobre el puerto de entrada del nodo.

#### Devolución de varios valores <a href="#returning-multiple-values" id="returning-multiple-values"></a>

La devolución de varios valores es algo más complejo que crear varias entradas y será necesario usar un diccionario para ello. Las entradas del diccionario se convierten en puertos en el lado de salida del nodo. Se crean varios puertos de devolución de la siguiente forma:

* Añada `using System.Collections.Generic;` para utilizar `Dictionary<>`.
* Añada `using Autodesk.DesignScript.Runtime;` para utilizar el atributo `MultiReturn`. Esto hace referencia a "DynamoServices.dll" desde el paquete NuGet de DynamoServices.
* Añada el atributo `[MultiReturn(new[] { "string1", "string2", ... more strings here })]` al método. Las cadenas hacen referencia a claves del diccionario y se convertirán en nombres de puertos de salida.
* Se devuelve `Dictionary<>` desde la función con claves que coinciden con los nombres de parámetro del atributo, `return new Dictionary<string, object>`.

```
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        [MultiReturn(new[] { "add", "mult" })]
        public static Dictionary<string, object> ReturnMultiExample(double a, double b)
        {
            return new Dictionary<string, object>

                { "add", (a + b) },
                { "mult", (a * b) }
            };
        }
    }
}
```

> Consulte este ejemplo de código en [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70).

Un nodo que devuelve varias salidas.

![Varias salidas](images/multipleoutputs.png)

> 1. Observe que ahora hay dos puertos de salida a los que se les ha asignado un nombre en función de las cadenas que hemos introducido para las claves del diccionario.

#### Documentación, información de herramientas y búsqueda <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

Se recomienda añadir documentación a los nodos de Dynamo que describa su función, entradas, salidas, etiquetas de búsqueda, etc. Esto se realiza mediante etiquetas de documentación XML. La documentación XML se crea de la siguiente forma:

* Cualquier comentario precedido de tres barras inclinadas se considera documentación.
  * Por ejemplo, `/// Documentation text and XML goes here`.
* Después de las tres barras, cree etiquetas XML por encima de los métodos que Dynamo leerá al importar el archivo .dll.
  * Por ejemplo, `/// <summary>...</summary>`.
* Active la documentación XML en Visual Studio. Para ello, seleccione `Project > Project Properties > Build` y active `XML documentation file`.

![Generar un archivo XML](images/vs-xml.jpg)

> 1. Visual Studio generará un archivo XML en la ubicación especificada.

Los tipos de etiquetas son los siguientes:

* `/// <summary>...</summary>` es la documentación principal del nodo y aparecerá como información de herramientas sobre el nodo en la barra lateral de búsqueda izquierda.
* `/// <param name="inputName">...</param>` creará documentación para parámetros de entrada específicos.
* `/// <returns>...</returns>` creará documentación para un parámetro de salida.
* `/// <returns name = "outputName">...</returns>` creará documentación para varios parámetros de salida.
* `/// <search>...</search>` asociará el nodo con los resultados de búsqueda basados en una lista separada por comas. Por ejemplo, si creamos un nodo que subdivide una malla, es posible que deseemos añadir etiquetas como, por ejemplo, "malla", "subdivisión" y "catmull-clark".

A continuación, se muestra un nodo de ejemplo con descripciones de entrada y salida, así como un resumen que aparecerá en la biblioteca.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        /// <summary>
        /// This method demonstrates how to use a native geometry object from Dynamo
        /// in a custom method
        /// </summary>
        /// <param name="curve">Input Curve. This can be of any type deriving from Curve, such as NurbsCurve, Arc, Circle, etc</param>
        /// <returns>The twice the length of the Curve </returns>
        /// <search>example,curve</search>
        public static double DoubleLength(Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> Consulte este código de ejemplo en [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80).

Tenga en cuenta que el código de este nodo de ejemplo contiene lo siguiente:

> 1. Un resumen del nodo
> 2. Una descripción de la entrada
> 3. Una descripción de la salida

#### Objetos <a href="#objects" id="objects"></a>

Dynamo no cuenta con una palabra clave `new`, por lo que los objetos deberán generarse mediante métodos de creación estáticos. Los objetos se crean de la siguiente forma:

* Cree el constructor interno `internal ZeroTouchEssentials()`, a menos que se requiera lo contrario.
* Cree el objeto con un método estático como, por ejemplo, `public static ZeroTouchEssentials ByTwoDoubles(a, b)`.

> Nota: Dynamo utiliza el prefijo "By" para indicar que un método estático es un constructor y, aunque es opcional, su uso ayudará a que la biblioteca se ajuste mejor al estilo de Dynamo existente.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        private double _a;
        private double _b;

        // Make the constructor internal
        internal ZeroTouchEssentials(double a, double b)
        {
            _a = a;
            _b = b;
        }

        // The static method that Dynamo will convert into a Create node
        public static ZeroTouchEssentials ByTwoDoubles(double a, double b)
        {
            return new ZeroTouchEssentials(a, b);
        }
    }
}
```

> Consulte este código de ejemplo en [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26).

Una vez que se haya importado el archivo dll ZeroTouchEssentials, habrá un nodo ZeroTouchEssentials en la biblioteca. Este objeto se puede crear mediante el nodo `ByTwoDoubles`.

![Nodo ByTwoDoubles](images/dyn-constructor.jpg)

#### Uso de tipos de geometría de Dynamo <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

Las bibliotecas de Dynamo pueden utilizar tipos de geometría nativa de Dynamo como entradas y crear nueva geometría como salidas. Los tipos de geometría se crean de la siguiente forma:

* Haga referencia a "ProtoGeometry.dll" en el proyecto. Para ello, incluya `using Autodesk.DesignScript.Geometry;` en la parte superior del archivo de C# y añada el paquete NuGet ZeroTouchLibrary al proyecto.
* **Importante:** Administre los recursos de geometría que no se devuelven a partir de las funciones. Consulte la sección **Instrucciones "Dispose" o "using"** mostrada a continuación.

> Nota: Los objetos de geometría de Dynamo se utilizan como cualquier otro objeto transferido a funciones.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // "Autodesk.DesignScript.Geometry.Curve" is specifying the type of geometry input, 
        // just as you would specify a double, string, or integer 
        public static double DoubleLength(Autodesk.DesignScript.Geometry.Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> Consulte este código de ejemplo en [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86).

Un nodo que obtiene la longitud de una curva y la duplica.

![Entrada de curva](images/doublelength.png)

> 1. Este nodo acepta un tipo de geometría de curva como entrada.

#### Instrucciones "Dispose" o "using"<a href="#disposeusing-statements" id="disposeusing-statements"></a>

Los recursos de geometría que no se devuelven a partir de las funciones deberán administrarse manualmente, a menos que se utilice la versión 2.5 o posterior de Dynamo. En Dynamo 2.5 y versiones posteriores, el sistema administra internamente los recursos de geometría. Sin embargo, es posible que aún deba eliminar la geometría manualmente si se trata de un caso de uso complejo o tiene que reducir la memoria en un determinado momento. El motor de Dynamo gestionará los recursos de geometría que se devuelvan a partir de las funciones. Los recursos de geometría que no se devuelven se pueden gestionar manualmente de las siguientes formas:

*   Con una instrucción "using", como se muestra a continuación:

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > Puede encontrar documentación sobre la instrucción "using" [aquí](https://msdn.microsoft.com/en-us/library/yh598w02.aspx).
    >
    > Consulte el artículo sobre [mejoras en la estabilidad de la geometría de Dynamo](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297) para obtener más información sobre las nuevas funciones de estabilidad introducidas en Dynamo 2.5.
*   Se muestra lo siguiente con llamadas manuales a "Dispose":

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

#### Migraciones <a href="#migrations" id="migrations"></a>

Al publicar una versión más reciente de una biblioteca, los nombres de nodo pueden cambiar. Los cambios de nombre se pueden especificar en un archivo de migraciones para que los gráficos generados en versiones anteriores de una biblioteca sigan funcionando correctamente cuando se realice una actualización. Las migraciones se implementan de la siguiente forma:

* Cree un archivo `.xml` en la misma carpeta que el archivo `.dll` con el siguiente formato: "BaseDLLName".Migrations.xml.
* En el archivo `.xml`, cree un único elemento `<migrations>...</migrations>`.
* En el elemento de migraciones, cree elementos `<priorNameHint>...</priorNameHint>` para cada cambio de nombre.
* Para cada cambio de nombre, proporcione un elemento `<oldName>...</oldName>` y `<newName>...</newName>`.

![Archivo de migraciones](images/vs-migrations-file.jpg)

> 1. Haga clic con el botón derecho y seleccione `Add > New Item`.
> 2. Seleccione `XML File`.
> 3. En este proyecto, al archivo de migraciones se le asignará el nombre `ZeroTouchEssentials.Migrations.xml`.

Este código de ejemplo indica a Dynamo que cualquier nodo con el nombre `GetClosestPoint` ahora se denomina `ClosestPointTo`.

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> Consulte este ejemplo de código en [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml).

#### Genéricos <a href="#generics" id="generics"></a>

Zero-Touch no admite actualmente el uso de genéricos. Se pueden utilizar, pero no en el código que se importa directamente en la ubicación en la que no se ha establecido el tipo. No se pueden mostrar las clases, las propiedades o los métodos genéricos para los que no se haya establecido el tipo.

En el siguiente ejemplo, no se importará un nodo Zero-Touch de tipo `T`. Si el resto de la biblioteca se importa en Dynamo, se mostrarán excepciones de tipo no encontrado.

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

Si se utiliza un tipo genérico con el tipo definido en este ejemplo, se importará a Dynamo.

```
public class SomeWrapper
{
    public object wrapped;
    public SomeWrapper(SomeGenericClass<double> someConstrainedType)
    {
        Console.WriteLine(this.wrapped.GetType().ToString());
    }
}
```
