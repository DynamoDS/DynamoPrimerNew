# Actualización de paquetes y bibliotecas de Dynamo para Dynamo 4.x

### Introducción <a href="#introduction" id="introduction"></a>

Esta sección contiene información sobre los problemas que puede encontrar al migrar los gráficos, los paquetes y las bibliotecas a Dynamo 4.x. Dynamo 4.0 presenta lo siguiente:
* Mejoras considerables del rendimiento
* Actualizaciones de estabilidad y corrección de errores
* Modernización de la base de código
* Eliminación de las API marcadas anteriormente como obsoletas en 1.x
* Una actualización importante del tiempo de ejecución de .NET 8 a .NET 10
* PythonNet 3 es ahora el motor de Python por defecto para todos los nuevos nodos de Python

La migración a .NET 10 garantiza que Dynamo siga alineado con la hoja de ruta tecnológica de Microsoft, mucho antes de que finalice el soporte para .NET 8 en noviembre de 2026.

Al iniciar Dynamo 4.0, se le pedirá que actualice a .NET 10 si aún no lo ha hecho. Los autores de paquetes deben actualizar sus proyectos a .NET 10 para garantizar una compatibilidad completa.

Todos los nuevos nodos de Python creados en Dynamo 4.0 o superior comienzan con PythonNet3. No se preocupe por la compatibilidad con versiones anteriores; aquellos que trabajen en plataformas con varias versiones (por ejemplo, Revit o Civil 3D 2025/2026) deben instalar el paquete del motor de PythonNet3 en Dynamo 3.3-3.6 para mantener la compatibilidad. Puede encontrar más información [aquí](https://dynamobim.org/dynamo-core-4-0-release/).

La API y los nodos marcados como obsoletos en la versión 1.x se han eliminado en Dynamo 4.0. Puede consultar la [lista completa de cambios aquí](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0).

### Compatibilidad de paquetes <a href="#package-compatibility" id="package-compatibility"></a>

#### Uso de los paquetes de Dynamo 2.x y 3.x en Dynamo 4.x 
Como Dynamo 4.x ahora se ejecuta en el tiempo de ejecución de .NET 10, no se garantiza que los paquetes que se crearon para Dynamo 2.x (*mediante .NET48*) y Dynamo 3.x (*mediante .NET 8*) funcionen en Dynamo 4.x. Al intentar descargar un paquete en Dynamo 4.x que se publicó desde una versión de Dynamo inferior a la 4.0, recibirá una advertencia de que el paquete es de una versión anterior de Dynamo.

**Esto no significa que el paquete no vaya a funcionar.** Es simplemente una advertencia de que podría haber problemas de compatibilidad y, en general, es recomendable comprobar si existe una versión más reciente que se haya creado específicamente para Dynamo 4.x.

También puede observar este tipo de advertencia en los archivos de registro de Dynamo en el momento de la carga del paquete. Si todo funciona correctamente, puede ignorarla.

#### Uso de paquetes de Dynamo 4.x en Dynamo 2.x 

Es muy poco probable que un paquete creado para Dynamo 4.x (*mediante .Net 10*) funcione en Dynamo 2.x. También aparecerá la siguiente advertencia cuando intente instalar paquetes creados para Dynamo 4.x en Dynamo 2.x.

![Advertencia de compatibilidad de paquetes](images/6-2-packages-new-version-compatibility-warning.png)


#### Uso de paquetes de Dynamo 4.x en Dynamo 3.x 

El paquete creado para Dynamo 4.x (*mediante .Net 10*) podría funcionar en Dynamo 3.x siempre que todas las API utilizadas en el paquete existan en .NET 8. No obstante, no hay ninguna garantía de que funcione. También aparecerá la siguiente advertencia cuando intente instalar paquetes creados para Dynamo 4.x en Dynamo 3.x.

![Advertencia de compatibilidad de paquetes](images/6-2-packages-compatibility-warning.png)

#### Prácticas recomendadas para los autores de paquetes 
La mejor práctica es orientar el proyecto tanto a .NET 8 como a .NET 10 mediante la modificación del archivo .csproj.

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
Esto garantiza lo siguiente:
* Compatibilidad con versiones de Dynamo alojadas en Revit que siguen en .NET 8
* Compatibilidad con la instancia independiente de Dynamo 4.x en .NET 10