# RevitMCP y DynamoMCP

Autodesk Assistant obtiene todas sus funciones de los servidores MCP. Cada servidor proporciona a Autodesk Assistant un conjunto de herramientas y cada una de ellas realiza una función concreta en el producto al que pertenece.

Cuando se trabaja con Revit, dos de esos servidores son importantes: **RevitMCP** y **DynamoMCP**. No se trata de opciones que compitan entre sí y no es necesario que elija entre una u otra. Funcionan a distintos niveles y Autodesk Assistant utiliza el que mejor se ajuste a su solicitud.

### RevitMCP: operaciones directas en modelos

RevitMCP, que se ofrece como [servidor MCP público de Revit](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm), agrupa un conjunto seleccionado de operaciones de la API de Revit en forma de herramientas MCP. Cada herramienta se corresponde con una acción definida en el modelo: localizar elementos, comprobar parámetros y recuentos, editar parámetros de forma masiva y capturar instantáneas de vistas.

Esta es la ruta más corta para una tarea que el conjunto de herramientas ya cubre. Una solicitud se convierte en una llamada y el modelo cambia de inmediato. No hay que crear ningún gráfico ni ejecutar nada después.

### DynamoMCP: herramientas que construyen la operación

DynamoMCP opera un nivel por encima. Sus herramientas no realizan acciones de modelado directamente. En su lugar, crean un gráfico de Dynamo; colocan nodos, los conectan entre sí, establecen valores y ejecutan el resultado. Consulte [¿Qué es el servidor MCP de Dynamo?](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md) para ver la lista de herramientas completa.

Esto es importante debido a la naturaleza de un nodo. Cada nodo de la biblioteca de Dynamo constituye en sí mismo una o varias llamadas a la API, ya escritas, probadas y documentadas. Los nodos de paquetes y Python están disponibles de la misma forma. De este modo, un pequeño conjunto de herramientas de creación de gráficos obtiene acceso a toda la biblioteca de Dynamo.

El resultado es que las herramientas de DynamoMCP generan nuevas funciones en lugar de solo utilizar las existentes. En lugar de una lista fija de acciones, Autodesk Assistant crea la acción que ha solicitado a partir de nodos. En la práctica, las herramientas MCP que generan gráficos se comportan como herramientas que crean más herramientas.

### La diferencia en la práctica

Supongamos que solicita etiquetas de puerta para un nivel.

- Al gestionarla con RevitMCP, la tarea se ejecuta directamente y aparecen las etiquetas.
- Mediante DynamoMCP, Autodesk Assistant coloca los nodos que recopilan las puertas, los filtra según el nivel que haya indicado y crea las etiquetas; a continuación, ejecuta el gráfico.

Ambos métodos generan las etiquetas de puerta. La segunda opción también le permite dejar algo en el lienzo.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| ¿Qué hace la herramienta? | Realiza una operación de Revit definida. | Genera y ejecuta un gráfico de Dynamo. |
| ¿A qué se accede? | Las operaciones incluidas en el conjunto de herramientas. | Cualquier nodo de la biblioteca, incluidos los nodos de paquetes y Python. |
| ¿Qué se obtiene? | Los cambios en el modelo. | Los cambios en el modelo, además del gráfico que los ha realizado. |
| ¿Para qué es ideal? | Una única tarea bien definida. | La lógica que desea examinar, ajustar, reutilizar o compartir. |

### ¿Por qué conviene disponer de este gráfico?

Un gráfico es un registro del trabajo, no solo de su resultado. Una vez que se encuentre en el lienzo, podrá realizar lo siguiente:

- Léalo y compruebe exactamente su funcionamiento.
- Modifique un valor y vuelva a ejecutarlo sin necesidad de volver a realizar toda la tarea por segunda vez.
- Guárdelo y utilícelo en el próximo proyecto o entrégueselo a un compañero.
- Publíquelo en el Reproductor de Dynamo para que las personas que no utilicen Dynamo puedan ejecutarlo.

La primera solicitud es en la que se invierte el tiempo. A partir de ahí, cada ejecución es prácticamente automática.

{% hint style="info" %}
En Dynamo Sandbox, Autodesk Assistant solo accede a DynamoMCP, ya que no hay ningún modelo de Revit sobre el que actuar. En Revit, ambos están disponibles.
{% endhint %}

### Decidir qué pedir

No es necesario que indique el nombre de un servidor en la indicación. Basta con describir el resultado; Autodesk Assistant seleccionará las herramientas.

Si desea un gráfico en concreto, indíquelo. Las indicaciones como *"crea un gráfico en el que se etiqueten las puertas del nivel 2"* señalan a Autodesk Assistant que lo importante es la versión reutilizable y no solo las puertas etiquetadas.

### Lecturas adicionales

- [Autodesk Assistant (vista preliminar técnica)](https://help.autodesk.com/cloudhelp/2027/ESP/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm): las acciones que realiza Autodesk Assistant en Revit en la Ayuda de Revit 2027.
- [Servidor MCP público de Revit (vista preliminar técnica)](https://help.autodesk.com/cloudhelp/2027/ESP/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm): la parte de Revit del servidor MCP en la Ayuda de Revit 2027.
- [Autodesk Assistant: descripción general y preguntas frecuentes](https://www.autodesk.com/es/support/technical/article/caas/sfdcarticles/sfdcarticles/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html): los productos que incluyen Autodesk Assistant y cómo usarlo.
