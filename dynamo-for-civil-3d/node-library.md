# Biblioteca de nodos

Anteriormente, hemos mencionado que los **nodos** son los componentes básicos de un gráfico de Dynamo y que se organizan en grupos lógicos en la **biblioteca**. En Dynamo for Civil 3D, hay dos categorías (o **estantes**) de la biblioteca que contienen nodos específicos para trabajar con objetos de AutoCAD y Civil 3D, como alineaciones, perfiles, obras lineales, referencias a bloque, etc. El resto de la biblioteca contiene nodos de naturaleza más genérica y coherentes entre todas las "versiones" de Dynamo (por ejemplo, Dynamo para Revit, Dynamo Sandbox, etc.).

{% hint style="info" %}
 Consulte la sección [2-library.md](../3\_user\_interface/2-library.md "mention") para obtener más información sobre la organización de los nodos de la biblioteca principal de Dynamo. 
{% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>La biblioteca de nodos de Dynamo for Civil 3D</p></figcaption></figure>

> 1. Nodos específicos para trabajar con objetos de AutoCAD y Civil 3D
> 2. Nodos de uso general
> 3. Nodos de **paquetes** de terceros que se pueden instalar por separado

{% hint style="warning" %}
 Al utilizar los nodos que se encuentran en los estantes de AutoCAD y Civil 3D, el gráfico de Dynamo solo funcionará en Dynamo for Civil 3D. Si se abre un gráfico de Dynamo for Civil 3D en otra ubicación (por ejemplo, en Dynamo para Revit), estos nodos se marcarán con una advertencia y no se ejecutarán. 
{% endhint %}

{% hint style="info" %}
 **¿Por qué hay dos estantes independientes para AutoCAD y Civil 3D?**

Esta organización distingue los nodos de los objetos nativos de AutoCAD (líneas, polilíneas, referencias a bloque, etc.) de los nodos de los objetos de Civil 3D (alineaciones, obras lineales, superficies, etc.). Desde el punto de vista técnico, AutoCAD y Civil 3D son dos componentes independientes; AutoCAD es la aplicación principal y Civil 3D se basa en ella. 
{% endhint %}

## Jerarquía de nodos

Para trabajar con los nodos de AutoCAD y Civil 3D, es importante conocer a fondo la jerarquía de objetos dentro de cada estante. ¿Recuerda la taxonomía de la biología? Reino, phylum, clase, orden, familia, género y especie. Los objetos de AutoCAD y Civil 3D se organizan de forma similar. Veamos algunos ejemplos para explicarlo.

### Objetos de Civil

Vamos a utilizar una alineación como ejemplo.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

Supongamos que su objetivo es cambiar el nombre de la alineación. A partir de aquí, el siguiente nodo que añadiría es **CivilObject.SetName**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

Al principio, es posible que esto parezca algo lioso. ¿Qué es un **CivilObject** y por qué la biblioteca no tiene un nodo **Alignment.SetName**? La respuesta está relacionada con la _reutilización_ y la _simplicidad_. Si lo piensa, el proceso de cambio de nombre de un objeto de Civil 3D es el mismo tanto si se trata de una alineación, una obra lineal, un perfil o cualquier otro elemento. Por lo tanto, en lugar de tener nodos repetitivos que básicamente hacen lo mismo (por ejemplo, **Alignment.SetName, Corridor.SetName, Profile.SetName**, etc.), sería conveniente asignar esa funcionalidad a un solo nodo. Eso es exactamente lo que hace **CivilObject.SetName**.

Otra forma de enfocar esto es en términos de _relaciones_. Una alineación y una obra lineal son tipos de **objetos de Civil**, del mismo modo que una manzana y una pera son tipos de fruta. Al igual que usa un único pelador para pelar tanto una manzana como una pera, los nodos de objetos de Civil se aplican a cualquier objeto de este tipo. ¡Lo desordenada que estaría su cocina si tuviera un pelador distinto para cada tipo de fruta! En ese sentido, la biblioteca de nodos de Dynamo es igual que su cocina.

### Objetos

Ahora, vamos a dar un paso más allá. Supongamos que desea cambiar la capa de la alineación. Se utilizará el nodo **Object.SetLayer**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

¿Por qué no hay un nodo denominado **CivilObject.SetLayer**? Aquí se aplican los mismos principios de reutilización y simplicidad que hemos analizado anteriormente. La propiedad _layer_ es común a cualquier objeto de AutoCAD que se pueda dibujar o insertar, como una línea, una polilínea, texto, una referencia a bloque, etc. Los objetos de Civil 3D como alineaciones y obras lineales se encuentran en la misma categoría, por lo que cualquier nodo que se aplique a un **objeto** también se puede utilizar con cualquier **objeto de Civil**.

