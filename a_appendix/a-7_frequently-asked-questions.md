# Preguntas frecuentes

## Cómo utilizar las compilaciones de Dynamo

### Compilaciones diarias frente a compilaciones estables

Es una tradición que el equipo de Dynamo en Autodesk mantenga un ritmo rápido de iteración, publicando tanto compilaciones diarias por compromiso como compilaciones de lanzamiento estables tras nuestro ciclo de pruebas y lanzamiento del sistema. A nuestro equipo le encantaría reiniciar las compilaciones diarias y estables para que los usuarios puedan controlar dónde se extrae DynamoCore en su disco de forma local y así tener confianza al utilizarlo sin que ello afecte a Dynamo para otros productos de ADSK. Hay algunos candidatos naturales para este fin, incluidos nupkg, archivos .zip o instaladores específicos en los que los usuarios pueden elegir la ruta de instalación u otras opciones.

Dado que nuestro objetivo es hacer llegar a los usuarios nuestro código más reciente de la forma más sencilla posible, hemos decidido entregar un archivo .zip que contiene los archivos binarios de DynamoCore y Dynamo Sandbox que puede utilizarse sin Revit (con algunas restricciones).

### Compilaciones del zip de Dynamo

#### Definición y fuente

La compilación zip de DynamoCoreRuntime es una instantánea de los archivos binarios de DynamoCore que se realiza durante nuestras compilaciones automatizadas.

Debería poder ejecutar DynamoSandbox.exe en la carpeta extraída para utilizar Dynamo con una configuración mínima.

#### Componentes necesarios

| Versión de Dynamo | Microsoft Visual C++ | DirectX                         |   |   |   |   |
| --------------    | -------------------- | ------------------------------- | - | - | - | - | 
| 2.0 - 2.6         | 2015 Redistributable | 10                              |   |   |   |   | 
| 2.7               | 2019 Redistributable | 11/12 (incluido con Windows 10  |   |   |   |   | 
| >=2.8             | 2019 Redistributable | 11/12 (incluido con Windows 10  |   |   |   |   |

**Microsoft DirectX, que también está disponible públicamente en nuestro repositorio de Github Dynamo** [**aquí**](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)

**7zip utilizado para descomprimir el paquete** [**aquí**](https://7zip-es.updatestar.com/download.html)

**Microsoft Visual C++ 2015-2024 Redistributable (x64) ** [**vínculo**](https://aka.ms/vs/17/release/vc_redist.x64.exe)

**Componentes opcionales**

Biblioteca de geometría (solo estará disponible con herramientas de modelado específicas de Autodesk, como Revit, Civil 3D, Advanced Steel, etc.)

### Solución de problemas

Si, al descomprimir la compilación, no ha podido iniciar DynamoSandbox.exe, asegúrese de utilizar [7-Zip](https://7zip-es.updatestar.com/download.html) para descomprimirla. También puede desbloquear manualmente el archivo .zip _antes_ de extraerlo si dispone de permisos en el equipo.

![](images/a-7/dynamo-builds-1.png)

Si falta alguno de los componentes necesarios, es posible que tenga problemas al utilizar Dynamo y que determinadas partes de la interfaz de usuario no se carguen.

Con la siguiente captura de pantalla como ejemplo, al descomprimir nuestra compilación en una VM limpia de Windows 10 sin GPU, al equipo le faltan los dos componentes necesarios. Esto se indica en la consola de Dynamo.

![](images/a-7/dynamo-builds-2.png)

**Instalación de DirectX**

Siga las instrucciones de Microsoft aquí para comprobar si ya tiene instalado DirectX. Si no es así, puede abrir DXSETUP.exe en nuestro repositorio de Github para Dynamo [aquí](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX). Una vez que vea el cuadro de diálogo que aparece a continuación, no dude en pulsar Siguiente para instalar DirectX en la ubicación por defecto.

![](images/a-7/dynamo-builds-3.png)

**Instalación de Microsoft Visual C++ 2015-2024 Redistributable (x64)**

Descargue la última versión [aquí](https://aka.ms/vs/17/release/vc_redist.x64.exe). A continuación, debería poder ejecutar el instalador denominado vc_redist.x64.exe en la ubicación de descarga del navegador. Una vez que vea el cuadro de diálogo que aparece a continuación, no dude en hacer clic en Instalar para incluir este componente en la ubicación por defecto.

![](images/a-7/dynamo-builds-4.png)

Después de instalar los dos componentes necesarios desde el vínculo anterior, vuelva a ejecutar DynamoSandbox.exe; debería ver el siguiente resultado:

![](images/a-7/dynamo-builds-5.png)

**Faltan gráficos 3D**

También puede encontrarse con incidencias de gráficos al ejecutar Sandbox por primera vez. Puede consultar las preguntas frecuentes estándar sobre incidencias de gráficos aquí:

[https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ)

En general, es probable que necesite forzar el modo de GPU de alto rendimiento para la tarjeta gráfica cuando utilice DynamoSandbox.exe.

_Panel de control de NVIDIA de ejemplo:_

![](images/a-7/dynamo-builds-6.png)

**Instalación de WebView2 Runtime**

Actualmente, los siguientes módulos de Dynamo utilizan el componente WebView2: el Navegador de documentación, las visitas guiadas y la biblioteca. Por lo tanto, para garantizar que estas partes de Dynamo muestren correctamente el contenido web, necesitamos instalar el instalador WebView2 Evergreen Runtime (es necesario validar si ya está instalado en el equipo o si es necesario instalarlo).

Este es el vínculo para instalar WebView2 Runtime: [https://developer.microsoft.com/es-es/microsoft-edge/webview2/#download-section](https://developer.microsoft.com/es-es/microsoft-edge/webview2/?form=MA13LH#download-section)

![](images/a-7/dynamo-builds-7.png)

Los componentes que se deben instalar son Evergreen Bootstrapper o Evergreen Standalone Installer. El primero descarga un instalador de 1,50 MB y, el segundo, uno de 130 MB.

Una vez instalado el tiempo de ejecución, los siguientes componentes de Dynamo deberían funcionar correctamente:

![](images/a-7/dynamo-builds-8.png)

**Incidencias de los nodos de Excel de Dynamo**

Puede consultar este [artículo](https://www.autodesk.com/es/support/technical/article/caas/sfdcarticles/sfdcarticles/ESP/Warning-Data-ImportExcel-operation-failed-Could-not-load-file-or-assembly-Microsoft-Office-Interop-Excel-when-running-the-Dynamo-script-in-Revit.html) para obtener diagnósticos.

### Ubicación de compilaciones de Dynamo

Versiones estables

[https://dynamobim.org/download/](https://dynamobim.org/download/)

[https://github.com/DynamoDS/Dynamo/releases](https://github.com/DynamoDS/Dynamo/releases)

Compilaciones diarias y versiones estables

[https://dynamobuilds.com/](https://dynamobuilds.com/)
