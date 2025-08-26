# Administración y actualización de dependencias en Dynamo

### ¿Cuándo se aplica esta wiki?

Cuando trabaje en una nueva función o simplemente actualice una dependencia existente, debe evaluar lo siguiente antes de incorporar una nueva dependencia al repositorio de Dynamo.

### Consideraciones

1. ¿Cuál es la licencia de la dependencia nueva o actualizada? Solo algunas licencias de código abierto se aprueban sin consultar antes al departamento legal de ADSK.
   * Tras establecer la licencia, asegúrese de que la dependencia y la versión se registren en la wiki interna.
   * Si la licencia es `LGPL`, `GPL` o `Apache`, el archivo de licencia debe copiarse en la subcarpeta _"Licencias de código abierto"_ de la compilación de Dynamo.
   * Si la licencia es `LGPL`, el código fuente completo de todos los componentes de terceros, junto con copias de texto de las licencias de código abierto correspondientes, deben cargarse en [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution).
2. Si se trata de una actualización, ¿ha cambiado el tipo de licencia con respecto a la versión anterior?
3. ¿La dependencia es multiplataforma?
   * ¿Tiene componentes nativos (como `CEFSharp` o `ImageMagick`)? Esto dificultará la implementación en varias plataformas.
   * ¿Tiene referencias solo para Windows? Si es así, no debe ser una dependencia de DynamoCore ni de otros elementos multiplataforma de Dynamo (la capa del modelo).
4. ¿Está la dependencia correctamente agrupada en la carpeta bin de la compilación con todas sus dependencias necesarias?
   * Si se trata de una actualización, ¿se eliminan algunos archivos como consecuencia de la actualización? ¿Esta versión de Dynamo está pensada para una versión secundaria concreta de los productos anfitriones? Si es así, deberá conservar los archivos binarios antiguos hasta que se lance la versión global del año siguiente con el fin de ofrecer compatibilidad con los instaladores de parches. Consulte [esta información](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me).
5. ¿La dependencia o su árbol de dependencias entran en conflicto con otras dependencias existentes en Dynamo?
6. ¿Entra en conflicto la dependencia o su árbol de dependencias con las dependencias existentes en los productos que integran Dynamo en el proceso (Revit, Civil, etc.)? **Esto es importante, ya que estos problemas solo se pueden detectar en el momento de la integración, a menos que el trabajo se realice por adelantado.**
