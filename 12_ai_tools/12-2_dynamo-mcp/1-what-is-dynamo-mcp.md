# ¿Qué es el servidor MCP de Dynamo?

**MCP** son las siglas en inglés de "Model Context Protocol", protocolo de contexto del modelo. Se trata de un estándar abierto que permite que los modelos de IA se conecten al software a través de un protocolo común. Un servidor MCP pone a disposición las herramientas y los datos de un programa para que un cliente de IA pueda leer el contexto en tiempo real, llevar a cabo acciones y devolver resultados estructurados.

**DynamoMCP** es el servidor MCP integrado de Dynamo. Conecta Autodesk Assistant a la sesión activa de Dynamo (el gráfico, los nodos, las conexiones y el estado del área de trabajo) para que la ayuda de la IA se base en aquello en lo que está trabajando realmente.

Para los usuarios de Dynamo, esto significa que Autodesk Assistant puede hacer mucho más que responder a preguntas generales. Puede inspeccionar el gráfico abierto, ayudarle a resolver incidencias relacionadas con los nodos y actualizar el lienzo en función del contexto real de la sesión en lugar de tener que copiar y pegar manualmente.

DynamoMCP está disponible en Dynamo 4.2.0 y versiones posteriores.

### ¿Cómo se usa DynamoMCP?

DynamoMCP se utiliza a través de Autodesk Assistant; no requiere ninguna instalación independiente ni cuenta con una interfaz propia. Autodesk Assistant es el punto de partida para formular preguntas, obtener ayuda con el gráfico y permitir que la IA trabaje con el contexto de Dynamo en tiempo real.

El lugar en el que abra Autodesk Assistant dependerá de dónde esté trabajando, como se indica a continuación:

- **Dynamo Sandbox**: las herramientas MCP de Dynamo están disponibles en Autodesk Assistant, dentro de Dynamo.
- **Revit**: las herramientas MCP de Dynamo están disponibles en Autodesk Assistant, dentro de Revit.

En ambos casos, permanecerá en la sesión actual y utilizará del mismo modo Autodesk Assistant para acceder a DynamoMCP.
### ¿Qué puede hacer DynamoMCP?

DynamoMCP proporciona a Autodesk Assistant un **conjunto de herramientas**. Cada herramienta realiza una tarea concreta en Dynamo, como leer el lienzo, colocar un nodo o ejecutar el gráfico. Nunca debe llamar a estas herramientas por su cuenta. Describa lo que desea en un lenguaje sencillo y Autodesk Assistant seleccionará las herramientas necesarias.

No obstante, sigue siendo útil saber cuáles son esas herramientas. Le explica qué es lo que Autodesk Assistant es capaz de hacer y le ayuda a formular solicitudes que pueda atender.

{% hint style="info" %}
Los nombres de las herramientas que comienzan con `get_` solo leen la información de la sesión. Todas las demás herramientas pueden modificar el gráfico.
{% endhint %}

#### Lectura del gráfico actual

Estas herramientas permiten que Autodesk Assistant vea en qué está trabajando antes de responder o realizar un cambio.

| Herramienta | Función |
| --- | --- |
| `get_workspace_info` | Enumera los elementos que aparecen en el lienzo: nodos, cables, grupos, notas y detalles del área de trabajo. |
| `get_selection` | Muestra los nodos, los cables y los grupos que ha seleccionado. |
| `get_node_connections` | Muestra a qué está conectado un nodo, puerto por puerto. |
| `get_node_property` | Muestra la configuración de los nodos como vista previa, bloqueo, vista previa anclada y etiquetas. |
| `get_node_lacing` | Indica la configuración de encaje de un nodo. |
| `get_installation_info` | Indica la versión de Dynamo, el host en el que se está ejecutando y un vínculo a las notas de la versión. |

*Indicación de ejemplo: "¿Qué aparece en el lienzo actualmente?"*

#### Búsqueda del nodo adecuado

Dynamo cuenta con miles de nodos, incluidos los de los paquetes instalados. Estas herramientas ayudan a Autodesk Assistant a elegir la opción correcta en lugar de tener que adivinarla.

| Herramienta | Función |
| --- | --- |
| `get_nodes_info` | Busca en la biblioteca de nodos, incluidos los nodos de los paquetes y devuelve el nodo exacto que se debe colocar. |
| `get_node_help` | Devuelve la ayuda escrita correspondiente a un nodo, así como la ruta al gráfico de ejemplo, si existe. |
| `get_type_schema` | Devuelve el formato de datos esperado para un tipo de Dynamo, como punto, número o color. |

*Indicación de ejemplo: "¿Qué nodo debo utilizar para obtener el área de una superficie?"*

#### Creación y edición del gráfico

Estas herramientas realizan el trabajo en el lienzo.

| Herramienta | Función |
| --- | --- |
| `create_nodes` | Coloca uno o más nodos en el lienzo con valores iniciales opcionales. |
| `set_node_value` | Establece el valor de los nodos de entrada, como "Number", "String", "Code Block" y menús desplegables. |
| `connect_nodes` | Conecta un puerto de salida a uno de entrada. |
| `disconnect_nodes` | Elimina los cables entre dos nodos. |
| `delete_node` | Elimina un nodo del lienzo. |
| `rename_node` | Cambia el nombre de los nodos de entrada y "Watch" para que quede clara su finalidad. Los nodos "Function" mantienen sus nombres originales. |
| `select_nodes` | Selecciona los nodos del lienzo para que pueda ver a cuáles se refiere Autodesk Assistant. |
| `set_node_property` | Activa o desactiva la configuración de los nodos: vista previa, bloqueo, vista previa anclada, etiquetas y entrada o salida. |
| `set_node_lacing` | Establece el tipo de encaje en "Automático", "Más corto", "Más largo" o "Producto vectorial". |
| `set_node_port_list_at_level` | Establece el nivel de lista en un puerto de entrada. |

*Indicación de ejemplo: "Añade un control deslizante numérico y conéctelo a la entrada de radio del círculo".*

#### Organización y documentación del gráfico

Un gráfico funcional no siempre resulta legible. Estas herramientas permiten gestionar el diseño, la agrupación, los colores y las notas.

| Herramienta | Función |
| --- | --- |
| `create_group` | Crea un grupo en torno a un conjunto de nodos. |
| `create_groups` | Crea varios grupos en un solo paso. |
| `ungroup_by_guid` | Elimina un grupo. Los nodos que contiene permanecen en el lienzo. |
| `ungroup_by_title` | Elimina grupos por su título. Los nodos que contienen permanecen en el lienzo. |
| `get_available_group_styles` | Muestra los estilos de grupo que puede utilizar, incluidos los estilos personalizados de las preferencias. |
| `assign_group_styles` | Aplica estilos de grupo, como "Entradas", "Acciones", "Salidas" y "Revisión". |
| `get_notes` | Enumera las notas del gráfico. |
| `create_notes` | Añade notas que explican partes del gráfico. |
| `update_notes` | Modifica el texto de una nota o lo desplaza. |
| `delete_notes` | Elimina notas. |
| `set_note_state` | Bloquea una nota a un nodo para que ambos se desplacen juntos o la desbloquea. |
| `auto_layout_workspace` | Reorganiza los nodos mediante la función de diseño automático integrada en Dynamo. |
| `set_workspace_description` | Escribe una descripción del gráfico en las propiedades del archivo. |

*Indicación de ejemplo: "Agrupa este gráfico y asígnale codificación por colores mediante entradas, acciones y salidas; a continuación, añade notas breves".*

#### Ejecución del gráfico y comprobación de los resultados

| Herramienta | Función |
| --- | --- |
| `run_workspace` | Ejecuta el gráfico actual. En el modo automático, el gráfico ya se vuelve a ejecutar con cada cambio, por lo que se omite este paso. |
| `get_workspace_warnings` | Muestra los nodos que presentan advertencias o errores, el texto del mensaje y los pasos recomendados que se deben seguir a continuación. Debe ejecutarse primero el gráfico. |
| `get_node_output_values_shapes` | Resume los valores de salida de un nodo o de todos los nodos del gráfico. |
| `get_node_output_value` | Lee la salida completa de un único nodo, página a página, en el caso de resultados demasiado extensos para resumirlos. |

*Indicación de ejemplo: "Ejecuta el gráfico y explícame por qué el nodo List.GetItemAtIndex muestra una advertencia".*

#### Trabajo con archivos de gráficos

Autodesk Assistant también puede trabajar con gráficos almacenados en el disco, no solo con el que tenga abierto.

| Herramienta | Función |
| --- | --- |
| `new_workspace` | Abre un nuevo gráfico vacío. Si el gráfico actual contiene cambios sin guardar, primero se le pedirá que los guarde o los descarte. |
| `save_workspace` | Guarda el gráfico actual. Si no se especifica una ubicación, se guardará en la carpeta de Dynamo, en Documentos. |
| `edit_graph_from_path` | Abre un gráfico guardado para editarlo, sin ejecutarlo. |
| `insert_graph_from_path` | Incorpora un gráfico guardado en el lienzo actual. Así es como se importan los gráficos de ejemplo del nodo. |
| `run_graph_from_path` | Ejecuta un gráfico guardado con los valores de entrada opcionales que especifique. |
| `run_graph_from_json` | Ejecuta un gráfico proporcionado en formato JSON de Dynamo sin ningún archivo en el disco. |
| `get_graph_info_from_path` | Lee las entradas, las salidas y los metadatos de un gráfico guardado sin abrirlo. |
| `get_graph_info_from_json` | Lee los mismos datos de un gráfico proporcionado en formato JSON de Dynamo. |
| `get_graphs_info_from_path` | Analiza una carpeta de gráficos e informa de lo que contiene cada uno de ellos. |
| `get_available_graphs_folders` | Muestra las carpetas que Dynamo reconoce y que pueden contener gráficos. |

*Indicación de ejemplo: "Revisa la carpeta de gráficos de Dynamo e indícame qué gráficos colocan muros".*

{% hint style="warning" %}
Autodesk Assistant puede modificar el gráfico. Guarde el trabajo antes de realizar una modificación importante y revise el resultado antes de sobrescribir un archivo existente.
{% endhint %}
