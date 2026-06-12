# El ciclo de vida y la periodicidad de las actualizaciones del servicio Dynamo Compute



En este documento, se describen la periodicidad de las actualizaciones y la política de soporte de Dynamo Cloud Compute. En este documento, también se puede hacer referencia a él indistintamente como «el servicio».

En él, se describe cómo se gestionan las versiones del motor, cuándo se producen las actualizaciones y qué pueden esperar los usuarios al ejecutar gráficos de Dynamo en la nube.

---

## Actualizar periodicidad

Para satisfacer las diferentes necesidades de los usuarios, Dynamo Cloud Compute cuenta con **dos vías de motor diferenciadas**. Cada pista tiene un propósito específico y sigue su propio programa de actualización:

### Motor estable (producción)

El motor estable se ha diseñado para ofrecer fiabilidad y coherencia en entornos de producción. Se basa en la última versión estable de DynamoCore Runtime y se actualiza cuando las versiones oficiales de Dynamo están disponibles para los usuarios de la versión de escritorio de Dynamo. Inicialmente, seguiremos la frecuencia de actualización de DynamoRevit.

Esta opción se ha diseñado para cargas de trabajo de producción en las que la fiabilidad y la previsibilidad son fundamentales. Si utiliza el motor estable, las actualizaciones se ajustarán al calendario de lanzamientos públicos de Dynamo, lo que le dará tiempo para prepararse para los cambios y probar sus gráficos antes de que afecten a sus flujos de trabajo.

### Motor de vista preliminar (vista preliminar/entorno Sandbox diario)

El motor de vista preliminar proporciona acceso anticipado a los avances más recientes de Dynamo. Se basa en la compilación de desarrollo más reciente de DynamoCore Runtime y se actualiza con frecuencia a medida que se combinan nuevas funciones y correcciones de errores.

Esta pista es ideal para los usuarios que deseen probar los próximos cambios, experimentar con nuevas funciones antes de su lanzamiento oficial o comprobar que sus gráficos seguirán funcionando con futuras versiones de Dynamo. El motor de vista preliminar le permite anticiparse a los cambios y enviar comentarios al equipo de Dynamo.

---


## Secuencia temporal de soporte

Saber durante cuánto tiempo se seguirá prestando soporte a cada versión del motor le ayudará a planificar los periodos de mantenimiento y las actualizaciones de los gráficos.

### Motor estable

El motor estable recibe actualizaciones cuando Dynamo publica una nueva versión estable de Dynamo Core en Revit. Cada versión estable permanece disponible y compatible hasta que se implemente la siguiente versión estable en el servicio.

Por ejemplo, si el servicio está ejecutando actualmente Dynamo 3.6 (versión estable), seguirá ejecutando esa versión hasta que Dynamo 4.0 esté disponible para todos los usuarios (normalmente, cuando se incluya en Revit). En ese momento, el servicio se actualizará a Dynamo 4.0 (estable).

Este enfoque garantiza que el servicio en la nube esté sincronizado con lo que la mayoría de los usuarios experimentan en los entornos de escritorio.

### Motor de vista preliminar

El motor de vista preliminar se actualiza continuamente a partir de la última rama de desarrollo de Dynamo. A medida que avanza el desarrollo en cada versión, el motor de vista preliminar realiza un seguimiento de los cambios.

Por ejemplo, aunque Dynamo 4.1 se encuentra en fase de desarrollo activo, es posible que el motor de vista preliminar se denomine "Dynamo Cloud Compute Service 4.1". Una vez que el desarrollo pase a la versión 4.2, el motor de vista preliminar comenzará a reflejar esos cambios y es posible que pase a denominarse "Dynamo Cloud Compute Service 4.2".

Dado que el motor de vista preliminar se actualiza con frecuencia, es posible que se produzcan cambios de última hora o que aparezcan funciones experimentales. Es más adecuado para pruebas y validación que para flujos de trabajo de producción.

---

## Elegir el motor adecuado

A la hora de decidir qué pista de motor utilizar, tenga en cuenta lo siguiente:

- **Elija la versión estable** si necesita un comportamiento predecible y probado para los flujos de trabajo de producción o si va a implementar gráficos para usuarios finales que esperan resultados coherentes.

- **Elija la vista preliminar** si desea probar las nuevas funciones antes que nadie, comprobar que los gráficos funcionarán en las próximas versiones o aportar comentarios sobre el desarrollo de Dynamo.

Ambos motores utilizan el mismo tiempo de ejecución básico de Dynamo; la diferencia radica en cuándo y con qué frecuencia reciben actualizaciones. 

---

> **Nota:**  
 Dynamo Cloud Compute se encuentra actualmente en fase beta. Los plazos de soporte y las políticas de actualización descritos en este documento reflejan nuestras intenciones actuales mientras probamos y perfeccionamos el servicio. No se trata de garantías y pueden variar en función de los comentarios de los usuarios y la experiencia operativa.



