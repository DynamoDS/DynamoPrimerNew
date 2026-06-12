# Dynamo Cloud Compute



Dynamo Cloud Compute lleva la potencia del entorno de ejecución de programación visual de Dynamo a la nube. En lugar de ejecutar los gráficos en el equipo local, el servicio de computación los ejecuta en un entorno seguro en la nube y devuelve los resultados.

## ¿Qué es Dynamo?

Dynamo es un lenguaje de programación visual y un entorno de desarrollo que le permite crear programas mediante la conexión de nodos en un gráfico. El tiempo de ejecución de Dynamo ejecuta estos gráficos, lo que le permite automatizar tareas complejas, generar geometría e integrarse con otro software.

## ¿Cómo funciona el servicio de computación?

Al utilizar Dynamo a través de un cliente basado en la nube (como el Reproductor de Dynamo en Forma), los archivos de gráficos `.dyn` se envían al servicio de computación para su ejecución. El servicio realiza lo siguiente:

1. Recibe el gráfico y los parámetros de entrada.
2. Ejecuta el gráfico en un entorno de nube aislado.
3. Devuelve los resultados a la aplicación cliente.

Este enfoque basado en la nube le permite ejecutar gráficos de Dynamo sin necesidad de instalar Dynamo localmente y aprovechar la potencia de la computación en la nube para operaciones complejas.

## ¿Por qué debo utilizar Dynamo Cloud Compute?

Dynamo Cloud Compute permite llevar a cabo situaciones en las que desee, por ejemplo, lo siguiente.

**Ejecutar gráficos sin necesidad de instalar la aplicación de escritorio**: ejecute gráficos de Dynamo directamente desde aplicaciones web sin que los usuarios tengan que instalar la versión de escritorio de Dynamo en sus equipos.

**Colaborar y compartir**: comparta gráficos con los miembros del equipo que pueden ejecutarse a través de interfaces web como Forma, lo que facilita la distribución de flujos de trabajo automatizados en toda la organización.

**Aproveche la informática en la nube**: aproveche la infraestructura en la nube para operaciones que requieren un gran esfuerzo computacional y que podrían tardar más tiempo en ejecutarse en equipos locales.

**Estandarizar el entorno de ejecución**: garantice un comportamiento uniforme entre los distintos usuarios y equipos mediante la ejecución de gráficos en un entorno de nube controlado.

**Conectar con Forma**: interactúe con la API de Forma mediante Dynamo. [Consulte esta entrada del blog para obtener más información.](https://dynamobim.org/design-to-configuration-your-rules-in-forma-and-revit-via-dynamo-part-1/)

## Características principales

**Ejecución en la nube**: los gráficos se ejecutan en la nube, no en el ordenador local. Esto implica lo siguiente:
- No es necesario instalar la versión de escritorio de Dynamo para ejecutar gráficos.
- Acceda a recursos de computación en la nube.
- Entorno de ejecución coherente entre diferentes usuarios.

**Seguridad**: el servicio ejecuta los gráficos de cada usuario en entornos aislados para garantizar la seguridad y la privacidad de los datos. Sus gráficos y datos se mantienen separados de los de los demás usuarios.

**Procesamiento asíncrono**: la ejecución de los gráficos se lleva a cabo de forma asíncrona: los clientes envían una tarea y pueden comprobar su estado hasta que se complete. Esto permite realizar cálculos de larga duración sin interrumpir su flujo de trabajo.

## Disponibilidad actual

Dynamo Cloud Compute está disponible actualmente de la siguiente forma:
- **Versión beta abierta del Reproductor de Dynamo en Forma**: cargue, comparta y ejecute gráficos de Dynamo directamente en la interfaz web de Autodesk Forma.

## Más información

- [Diferencias entre Dynamo Cloud Compute y la versión de escritorio de Dynamo](../dynamo-in-forma-beta/dynamo-compute-service-differences-with-desktop-dynamo.md): diferencias importantes que debe tener en cuenta al escribir gráficos para su ejecución en la nube
- [Ciclo de vida del motor](engine-lifecycle.md): información sobre las versiones de motor admitidas y su ciclo de vida

-----


> **Nota:**  
 Dynamo Cloud Compute se encuentra actualmente en fase beta. Los plazos de soporte y las políticas de actualización descritos en este documento reflejan nuestras intenciones actuales mientras probamos y perfeccionamos el servicio. No se trata de garantías y pueden variar en función de los comentarios de los usuarios y la experiencia operativa.