# ¿Qué es lo que Autodesk Assistant aún no es capaz de hacer?

### Actualización de paquetes

Autodesk Assistant aún no puede descargar ni instalar paquetes en Dynamo.

![](../../.gitbook/assets/Aa_Update.jpg)

Como se puede observar en la imagen anterior, Autodesk Assistant no puede modificar, instalar ni desinstalar paquetes externos asociados a Dynamo. Le ofrece instrucciones para que pueda instalarlos usted mismo.


### Lectura y escritura directa de archivos

Autodesk Assistant es compatible con los archivos de gráficos de Dynamo; puede abrir un gráfico, insertarlo en el lienzo, ejecutarlo y guardar el trabajo. No puede leer ni escribir otros archivos de la unidad de disco duro por sí mismo.

En el ejemplo siguiente, se muestra una figura geométrica creada en el lienzo. Se solicita a Autodesk Assistant que seleccione los puntos que conforman la geometría y los exporte en formato JSON. Aunque Autodesk Assistant no puede generar directamente este contenido JSON, sí sugiere que se podría crear una secuencia de comandos de Dynamo que realice esta acción.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

Esa sugerencia es el patrón que debe recordar. Se puede acceder a cualquier elemento que se encuentre fuera del gráfico a través de los nodos, no mediante Autodesk Assistant.

### Modificación del modelo de Revit sin un gráfico

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Autodesk Assistant para Dynamo no tiene ninguna conexión directa con el modelo de Revit. Solicítele que edite el modelo tal y como se lo pediría a Autodesk Assistant en Revit; le indicará que no puede realizar esta acción.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

No obstante, aún puede modificar el modelo de Revit. Lo hace tal y como lo haría usted; coloca nodos de Revit, los conecta entre sí y el modelo cambia cuando se ejecuta el gráfico.

![](../../.gitbook/assets/Aa_viewScript.jpg)

De esta forma de trabajo, se desprenden estas dos conclusiones:

- **Nada cambia hasta que se ejecuta el gráfico.** Puede consultar el gráfico y ajustar los valores primero. Solo se accede al modelo cuando se ejecuta el gráfico, ya sea por su parte o por parte de Autodesk Assistant como respuesta a su solicitud.
- **El cambio se puede repetir.** El gráfico permanece en el lienzo. Guárdelo y podrá volver a ejecutar la misma tarea en otro modelo o compartirla con su equipo.

Si desea realizar una única modificación directa sin necesidad de utilizar ningún gráfico, use Autodesk Assistant en Revit.
