# Ulteriori informazioni sul concetto di zero-touch

Dopo aver capito come si crea un progetto zero-touch, possiamo approfondire le specifiche della creazione di un nodo, illustrando l'esempio ZeroTouchEssentials nella pagina di Dynamo su Github.

![Nodi zero-touch](images/ootbzerotouch.png)

> Molti dei nodi standard di Dynamo sono essenzialmente nodi zero-touch, come la maggior parte dei nodi Math, Color e DateTime riportati sopra.

Per iniziare, scaricare il progetto ZeroTouchEssentials da qui: [https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials).

In Visual Studio, aprire il file della soluzione `ZeroTouchEssentials.sln` e creare la soluzione.

![ZeroTouchEssentials in Visual Studio](images/vs-build-zte.jpg)

> Il file `ZeroTouchEssentials.cs` contiene tutti i metodi che importeremo in Dynamo.

Aprire Dynamo e importare `ZeroTouchEssentials.dll` per ottenere i nodi a cui faremo riferimento negli esempi seguenti.

Gli esempi di codice derivano da e in genere corrispondono a [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs). La documentazione XML è stata rimossa per mantenerli concisi e ogni esempio di codice creerà il nodo nell'immagine che si trova sopra.

#### Valori di input di default <a href="#default-input-values" id="default-input-values"></a>

Dynamo supporta la definizione dei valori di default per le porte di input in un nodo. Questi valori di default verranno forniti al nodo se le porte non dispongono di connessioni. I valori di default vengono espressi utilizzando il meccanismo C# di definizione degli argomenti facoltativi nella [Guida per programmatori C#](https://msdn.microsoft.com/en-us/library/dd264739.aspx). Le impostazioni di default vengono specificate nel seguente modo:

* Impostare i parametri del metodo su un valore di default: `inputNumber = 2.0`.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // Set the method parameter to a default value
        public static double MultiplyByTwo(double inputNumber = 2.0) 
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Valore di default](images/defaultval.jpg)

> 1. Il valore di default verrà mostrato quando si posiziona il cursore sulla porta di input del nodo.

#### Restituzione di più valori <a href="#returning-multiple-values" id="returning-multiple-values"></a>

La restituzione di più valori è un po' più complessa rispetto alla creazione di più input e dovrà essere eseguita utilizzando un dizionario. Le voci del dizionario diventano porte sul lato di output del nodo. Vengono create più porte restituite nel modo seguente:

* Aggiungere `using System.Collections.Generic;` per utilizzare `Dictionary<>`.
* Aggiungere `using Autodesk.DesignScript.Runtime;` per utilizzare l'attributo `MultiReturn`. Fa riferimento a "DynamoServices.dll" dal pacchetto NuGet di DynamoServices.
* Aggiungere l'attributo `[MultiReturn(new[] { "string1", "string2", ... more strings here })]` al metodo. Le stringhe fanno riferimento alle chiavi del dizionario e diventeranno i nomi delle porte di output.
* Restituire `Dictionary<>` dalla funzione con le chiavi che corrispondono ai nomi dei parametri nell'attributo: `return new Dictionary<string, object>`.

```
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        [MultiReturn(new[] { "add", "mult" })]
        public static Dictionary<string, object> ReturnMultiExample(double a, double b)
        {
            return new Dictionary<string, object>

                { "add", (a + b) },
                { "mult", (a * b) }
            };
        }
    }
}
```

> Fare riferimento a questo esempio di codice in [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70).

Un ddo che restituisce più output.

![Più output](images/multipleoutputs.png)

> 1. Notare che ora sono presenti due porte di output denominate in base alle stringhe immesse per le chiavi del dizionario.

#### Documentazione, descrizioni comandi e ricerca <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

È buona norma aggiungere ai nodi Dynamo una documentazione che descriva la funzione del nodo, gli input, gli output, i tag di ricerca, ecc. Questa operazione viene eseguita tramite i tag della documentazione XML. La documentazione XML viene creata nel modo seguente:

* Qualsiasi testo di commento preceduto da tre barre è considerato documentazione.
  * Ad esempio: `/// Documentation text and XML goes here`
* Dopo le tre barre, creare tag XML sopra i metodi che Dynamo leggerà durante l'importazione del file .dll.
  * Ad esempio: `/// <summary>...</summary>`
* Attivare la documentazione XML in Visual Studio scegliendo `Project > Project Properties > Build` e selezionando `XML documentation file`.

![Generazione di un file XML](images/vs-xml.jpg)

> 1. Visual Studio genererà un file XML nella posizione specificata.

I tipi di tag sono i seguenti:

* `/// <summary>...</summary>` è la documentazione principale per il nodo e comparirà come descrizione comando sul nodo nella barra laterale di ricerca sinistra.
* `/// <param name="inputName">...</param>` creerà la documentazione per parametri di input specifici.
* `/// <returns>...</returns>` creerà la documentazione per un parametro di output.
* `/// <returns name = "outputName">...</returns>` creerà la documentazione per più parametri di output.
* `/// <search>...</search>` abbinerà il nodo ai risultati della ricerca in base ad un elenco separato da virgole. Ad esempio, se si crea un nodo che suddivide una maglia, si possono aggiungere tag come "mesh", "subdivision" e "catmull-clark".

Di seguito è riportato un nodo di esempio con descrizioni di input e output, nonché un riepilogo che verrà visualizzato nella libreria.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        /// <summary>
        /// This method demonstrates how to use a native geometry object from Dynamo
        /// in a custom method
        /// </summary>
        /// <param name="curve">Input Curve. This can be of any type deriving from Curve, such as NurbsCurve, Arc, Circle, etc</param>
        /// <returns>The twice the length of the Curve </returns>
        /// <search>example,curve</search>
        public static double DoubleLength(Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> Fare riferimento a questo esempio di codice in [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80).

Notare che il codice per questo nodo di esempio contiene:

> 1. Un riepilogo del nodo
> 2. Una descrizione dell'input
> 3. Un descrizione dell'output

#### Oggetti <a href="#objects" id="objects"></a>

Dynamo non dispone di una parola chiave `new`, pertanto gli oggetti dovranno essere costruiti utilizzando metodi di costruzione statici. Gli oggetti vengono costruiti nel modo seguente:

* Rendere interno il costruttore `internal ZeroTouchEssentials()`, se non diversamente richiesto.
* Costruire l'oggetto con un metodo statico, ad esempio `public static ZeroTouchEssentials ByTwoDoubles(a, b)`.

> Nota Dynamo utilizza il prefisso "By" per indicare che un metodo statico è un costruttore e, sebbene sia facoltativo, l'utilizzo di "By" aiuterà la libreria ad adattarsi meglio allo stile esistente di Dynamo.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        private double _a;
        private double _b;

        // Make the constructor internal
        internal ZeroTouchEssentials(double a, double b)
        {
            _a = a;
            _b = b;
        }

        // The static method that Dynamo will convert into a Create node
        public static ZeroTouchEssentials ByTwoDoubles(double a, double b)
        {
            return new ZeroTouchEssentials(a, b);
        }
    }
}
```

> Fare riferimento a questo esempio di codice in [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26).

Dopo l'importazione del file dll ZeroTouchEssentials, nella libreria sarà presente un nodo ZeroTouchEssentials. Questo oggetto può essere creato utilizzando il nodo `ByTwoDoubles`

![Nodo ByTwoDoubles](images/dyn-constructor.jpg)

#### Utilizzo dei tipi di geometria di Dynamo <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

Le librerie di Dynamo possono utilizzare tipi di geometria di Dynamo nativi come input e creare nuova geometria come output. I tipi di geometria vengono creati nel seguente modo:

* Fare riferimento al file ProtoGeometry.dll nel progetto includendo `using Autodesk.DesignScript.Geometry;` nella parte superiore del file C# e aggiungendo il pacchetto NuGet ZeroTouchLibrary al progetto.
* **Importante** Per gestire le risorse della geometria non restituite dalle funzioni, vedere la sezione **Istruzioni Dispose/using** riportata di seguito.

> Nota Gli oggetti geometrici di Dynamo vengono utilizzati come qualsiasi altro oggetto trasferito alle funzioni.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // "Autodesk.DesignScript.Geometry.Curve" is specifying the type of geometry input, 
        // just as you would specify a double, string, or integer 
        public static double DoubleLength(Autodesk.DesignScript.Geometry.Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> Fare riferimento a questo esempio di codice in [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86).

Un nodo che ottiene la lunghezza di una curva e la raddoppia.

![Input curve](images/doublelength.png)

> 1. Questo nodo accetta un tipo di geometria curve come input.

#### Istruzioni Dispose/using <a href="#disposeusing-statements" id="disposeusing-statements"></a>

Le risorse della geometria che non vengono restituite dalle funzioni dovranno essere gestite manualmente, a meno che non si stia utilizzando Dynamo versione 2.5 o successiva. In Dynamo 2.5 e versioni successive, le risorse della geometria vengono gestite internamente dal sistema, tuttavia, potrebbe essere ancora necessario rimuovere la geometria manualmente se si dispone di un caso di utilizzo complesso o se è necessario ridurre la memoria in un determinato momento. Il motore di Dynamo gestirà eventuali risorse della geometria restituite da funzioni. Le risorse della geometria non restituite possono essere gestite manualmente nei seguenti modi:

*   Con un'istruzione using:

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > L'istruzione using è documentata [qui](https://msdn.microsoft.com/en-us/library/yh598w02.aspx).
    >
    > Per ulteriori informazioni sulle nuove funzionalità di stabilità introdotte in Dynamo 2.5., vedere [Dynamo Geometry Stability Improvements](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297).
*   Con le chiamate manuali di Dispose:

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

#### Migrazioni <a href="#migrations" id="migrations"></a>

Quando si pubblica una versione più recente di una libreria, i nomi dei nodi potrebbero cambiare. Le modifiche ai nomi possono essere specificate in un file Migrations in modo che i grafici creati con versioni precedenti di una libreria continuino a funzionare correttamente quando si esegue un aggiornamento. Le migrazioni vengono implementate nel seguente modo:

* Creare un file `.xml` nella stessa cartella di `.dll` con il seguente formato: "BaseDLLName".Migrations.xml.
* Nel file `.xml`, creare un singolo elemento `<migrations>...</migrations>`.
* All'interno dell'elemento migrations, creare elementi `<priorNameHint>...</priorNameHint>` per ogni modifica del nome.
* Per ogni modifica del nome, fornire un elemento `<oldName>...</oldName>` e `<newName>...</newName>`.

![File Migrations](images/vs-migrations-file.jpg)

> 1. Fare clic con il pulsante destro del mouse e selezionare `Add > New Item`.
> 2. Scegliere `XML File`.
> 3. Per questo progetto, il nome del file Migrations è `ZeroTouchEssentials.Migrations.xml`.

Questo codice di esempio indica a Dynamo che qualsiasi nodo denominato `GetClosestPoint` è ora denominato `ClosestPointTo`.

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> Fare riferimento a questo esempio di codice in [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml).

#### Generics <a href="#generics" id="generics"></a>

Zero-Touch attualmente non supporta l'uso di generics. Possono essere utilizzati, ma non nel codice che viene importato direttamente dove il tipo non è impostato. I metodi, le proprietà o le classi che sono generici e senza il tipo impostato non possono essere esposti.

Nell'esempio seguente, un nodo zero-touch di tipo `T` non verrà importato. Se il resto della libreria viene importato in Dynamo, risulteranno mancante delle eccezioni del tipo.

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

L'utilizzo di un tipo generico con il tipo impostato in questo esempio verrà importato in Dynamo.

```
public class SomeWrapper
{
    public object wrapped;
    public SomeWrapper(SomeGenericClass<double> someConstrainedType)
    {
        Console.WriteLine(this.wrapped.GetType().ToString());
    }
}
```
