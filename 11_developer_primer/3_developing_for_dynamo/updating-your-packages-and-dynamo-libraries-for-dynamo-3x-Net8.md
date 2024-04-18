# Actualización de paquetes y bibliotecas de Dynamo para Dynamo 3.x y .NET8

### Introducción <a href="#introduction" id="introduction"></a>

Esta sección contiene información sobre los problemas que puede encontrar al migrar los gráficos, los paquetes y las bibliotecas a Dynamo 3.x.

Dynamo 3.0 es una versión principal y se han modificado o eliminado algunas API. El mayor cambio que probablemente le afecte como desarrollador o usuario de Dynamo 3.x es la transición a .NET8.

Dotnet/.NET es el tiempo de ejecución que impulsa el lenguaje C# en el que se escribe Dynamo. Hemos actualizado a una versión moderna de este tiempo de ejecución, junto con el resto del ecosistema de Autodesk.

Puede obtener más información en la [entrada del blog](https://dynamobim.org/dynamo-on-net-8/).
***

### Compatibilidad de paquetes <a href="#package-compatibility" id="package-compatibility"></a>

#### Uso de paquetes de Dynamo 2.x en Dynamo 3.x 
Como Dynamo 3.x se ejecuta ahora en el tiempo de ejecución de .NET8, no se garantiza que los paquetes creados para Dynamo 2.x (*mediante .NET48*) funcionen en Dynamo 3.x. Al intentar descargar un paquete en Dynamo 3.x que se publicó desde una versión de Dynamo inferior a la 3.0, recibirá una advertencia de que el paquete es de una versión anterior de Dynamo. 

**Esto no significa que el paquete no vaya a funcionar.** Es simplemente una advertencia de que podría haber problemas de compatibilidad y, en general, es recomendable comprobar si existe una versión más reciente que se haya creado específicamente para Dynamo 3.x.

También puede observar este tipo de advertencia en los archivos de registro de Dynamo en el momento de la carga del paquete. Si todo funciona correctamente, puede ignorarla.

#### Uso de paquetes de Dynamo 3.x en Dynamo 2.x 

Es muy poco probable que un paquete creado para Dynamo 3.x (*mediante .NET8*) funcione en Dynamo 2.x. También aparecerá una advertencia cuando descargue paquetes creados para versiones más recientes de Dynamo mientras utiliza una versión anterior.


