# Expectativas de las pruebas

En esta página, se describe lo que buscamos con las pruebas del nuevo código que se añadirá a Dynamo.

Entonces, tiene un nuevo nodo que desea añadir. Genial. Es hora de añadir algunas pruebas. Existen dos razones principales para ello:

1. Ayuda a descubrir los puntos en los que no funciona.
2. Cuando alguien modifica algo que interrumpe el nodo, esto debería mostrarse en las pruebas. De ese modo, la persona que interrumpió las pruebas deberá solucionarlo. Si no interrumpe las pruebas, deberá ocuparse en gran medida de los usuarios cuyos modelos no funcionen correctamente.

Las pruebas en Dynamo son de estos dos tipos: unitarias y del sistema.

## Pruebas unitarias

Las pruebas unitarias deben probar lo menos posible. Si ha creado un nodo que calcula raíces cuadradas a través de un nodo ZeroTouch de C#, la prueba debería comprobar solo ese DLL y realizar llamadas de C# directamente a ese código. Las pruebas unitarias ni siquiera deberían incluir a Dynamo.

Deben incluir lo siguiente:

* Pruebas positivas (hace lo correcto).
* Pruebas negativas (no genera resultados erróneos cuando se le da una entrada incorrecta).
* Pruebas de regresión (cuando alguien encuentra un error en el código, escriba una prueba para asegurarse de que no se repite)

Deben ser pequeñas, rápidas y fiables. La mayoría de las pruebas deben ser pruebas unitarias.

## Pruebas del sistema

Las pruebas del sistema operan en varios componentes y comprueban cómo encajan entre sí. Deben usarse y elaborarse con cuidado. 

¿Por qué? Su funcionamiento es caro. Necesitamos que el conjunto de pruebas se ejecute con la menor latencia posible.

Cuando escriba una prueba del sistema, lo primero que debe hacer es consultar [este](https://github.com/DynamoDS/Dynamo/blob/master/doc/system/Layer%20Diagram.pdf) diagrama para averiguar qué subsistemas abarca.

Lo ideal sería una serie progresiva de pruebas que abarquen conjuntos crecientes de bloques que interactúan, de modo que, cuando las pruebas empiecen a fallar, sea muy rápido averiguar dónde está el problema.

Entre los ejemplos de los elementos que requieren pruebas del sistema, se incluyen los siguientes:

* Un nuevo tipo de nodo de Revit que almacena varios elementos en el seguimiento en lugar de un único elemento
* Un nuevo nodo Watch que muestra los datos de forma diferente

Entre los ejemplos de los elementos que no requieren pruebas del sistema, se incluyen los siguientes:

* Un nuevo nodo matemático
* Una biblioteca de procesamiento de cadenas

Las pruebas del sistema deben:

* Confirmar el comportamiento correcto.
* Confirmar la ausencia de conductas patológicas como, por ejemplo, la ausencia de excepciones.