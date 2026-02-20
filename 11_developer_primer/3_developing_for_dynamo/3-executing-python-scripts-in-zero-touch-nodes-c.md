# Provádění skriptů jazyka Python v uzlech Zero-Touch (C#)

### Provádění skriptů jazyka Python v uzlech Zero-Touch (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Pokud umíte psát skripty v jazyce Python a chcete více funkcí, než vám mohou nabídnout standardní uzly jazyka Python aplikace Dynamo, můžete si pomocí funkce Zero-Touch vytvořit vlastní uzel. Začneme jednoduchým příkladem, který nám umožní předat skript jazyka Python jako řetězec uzlu Zero-Touch, kde se skript provede a vrátí se výsledek. Tato případová studie bude vycházet z ukázek a příkladů v části Začínáme. Pokud s tvorbou uzlů Zero-Touch úplně začínáte, podívejte se na ně.

![Uzel Zero-Touch, který provede řetězec skriptu jazyka Python](../../.gitbook/assets/python-case-study.png)

> Uzel Zero-Touch, který provede řetězec skriptu jazyka Python

#### Modul jazyka Python <a href="#python-engine" id="python-engine"></a>

PythonNet3 je nyní výchozím modulem, který umožňuje bezproblémovou migraci z prostředí CPython. Všechny nové uzly jazyka Python vytvořené v aplikaci Dynamo 4.0+ jsou ve výchozím nastavení založeny na modulu PythonNet3.

Tento uzel se spoléhá na instanci skriptovacího modulu IronPython. K tomu potřebujeme odkazovat na několik dalších sestav. Podle následujících kroků nastavte základní šablonu v aplikaci Visual Studio:

* Vytvořte nový projekt třídy aplikace Visual Studio.
* Přidejte odkaz na soubor `IronPython.dll` umístěný ve složce `C:\Program Files (x86)\IronPython 2.7\IronPython.dll`.
* Přidejte odkaz na soubor `Microsoft.Scripting.dll` umístěný ve složce `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll`.
* Zahrňte do třídy příkazy `IronPython.Hosting` a `Microsoft.Scripting.Hosting` `using`.
* Přidejte soukromý prázdný konstruktor, abyste zabránili přidání dalšího uzlu do knihovny aplikace Dynamo spolu s naším balíčkem.
* Vytvořte novou metodu, která jako vstupní parametr přijímá jeden řetězec.
* V rámci této metody vytvoříme novou instanci modulu jazyka Python a prázdný rozsah skriptu. Tento rozsah si můžete představit jako globální proměnné v rámci instance interpretu jazyka Python.
* Poté pro modul volejte `Execute` a jako parametry předejte vstupní řetězec a rozsah.
* Nakonec načtěte a vraťte výsledky skriptu voláním `GetVariable` pro modul a předáním názvu proměnné ze skriptu jazyka Python, která obsahuje hodnotu, kterou se pokoušíte vrátit. (Další podrobnosti naleznete v níže uvedeném příkladu.)

Následující kód představuje příklad výše uvedených kroků. Sestavením řešení se vytvoří nová knihovna `.dll` umístěná ve složce bin našeho projektu. Tuto knihovnu `.dll` lze nyní importovat do aplikace Dynamo jako součást balíčku nebo pomocí příkazu `File < Import Library...`.

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

Skript jazyka Python vrací proměnnou `output`, což znamená, že ve skriptu jazyka Python budeme potřebovat proměnnou `output`. K otestování uzlu v aplikaci Dynamo použijte tento vzorový skript. Pokud jste někdy v aplikaci Dynamo použili uzel jazyka Python, měl by vám následující skript být povědomý. Další informace naleznete v části [Python v příručce Primer](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Více výstupů <a href="#multiple-outputs" id="multiple-outputs"></a>

Jedním z omezení standardních uzlů jazyka Python je, že mají pouze jeden výstupní port, takže pokud chceme vrátit více objektů, musíme sestavit seznam a načíst každý objekt v něm. Pokud upravíme výše uvedený příklad tak, aby vracel slovník, můžeme přidat libovolný počet výstupních portů. Další informace o slovnících naleznete v části Vrácení více hodnot v tématu Další práce s funkcí Zero-Touch.

![Tento uzel umožňuje vrátit objem kvádru i jeho těžiště.](../../.gitbook/assets/python-multi-case-study.png)

> Tento uzel umožňuje vrátit objem kvádru i jeho těžiště.

Upravte předchozí příklad pomocí následujících kroků:

* Přidejte odkaz na knihovnu `DynamoServices.dll` ze Správce balíčků NuGet.
* Kromě předchozích sestav zahrňte `System.Collections.Generic` a `Autodesk.DesignScript.Runtime`.
* Upravte návratový typ naší metody tak, aby vracela slovník, který bude obsahovat naše výstupy.
* Každý výstup musí být z rozsahu načten jednotlivě (pro větší sady výstupů zvažte vytvoření jednoduché smyčky).

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

Do vzorového skriptu jazyka Python jsme také přidali další výstupní proměnnou (`output2`). Mějte na paměti, že tyto proměnné mohou používat jakékoli dovolené konvence pojmenování v jazyce Python, název „output“ byl v tomto příkladu použit výhradně kvůli srozumitelnosti.

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```

#### PythonNet3 – známá omezení a řešení<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

Níže jsou uvedena některá známá omezení při používání modulu PythonNet3 a jejich řešení.

* Kolekce .NET nejsou automaticky převedeny na seznamy jazyka Python.
    * Před použitím ```len()```, indexování nebo iterace je nutné explicitně převést pole ```.NET``` nebo kolekce pomocí ```list(...)```.
* Obecné metody .NET mohou vyžadovat explicitní parametry typu.
    * Některé metody (např. ```GroupBy```) nebudou fungovat, pokud ručně nezadáte obecné typy a spolehnete se na jejich automatické odvození.
* Metody rozšíření nejsou dostupné prostřednictvím ```dir()``` ani automatického dokončování.
    * Metody rozšíření mohou stále fungovat, pokud jsou volány explicitně, ale nezobrazují se v introspekci ani v návrzích dokončování kódu.
* Metody rozšíření DataTable nejsou podporovány.
    * Import ```System.Data.DataTableExtensions``` se nezdaří. Tyto pomocné metody nelze používat přímo.
* Některé základní metody aplikace Dynamo se v modulu PythonNet3 chovají odlišně.
    * Některé funkce (např. vyrovnávání seznamu) nemusí fungovat podle očekávání z důvodu striktnější správy kolekcí.
* Třídy jazyka Python nelze předávat mezi uzly, pokud jsou odvozeny z typů .NET.
    * Třídy odvozené z typů .NET nebo rozhraní nelze bezpečně přenášet mezi uzly jazyka Python.
* Funkce ```set()``` v jazyce Python nepřijímá některé objekty .NET.
    * Objekty, jako je ```InvalidElementId```, je nutné místo toho odfiltrovat nebo zpracovat pomocí kolekcí .NET.
* Častá volání funkce ```print()``` mohou způsobit nárůst využití paměti.
    * Vyhněte se častému používání funkce ```print()``` ve smyčkách nebo dlouhotrvajících skriptech.
* Interoperabilita slovníků mezi aplikací Dynamo a jazykem Python je omezená.
    * Slovníky aplikace Dynamo a jazyka Python nejsou zcela vzájemně plně zaměnitelné a mohou vyžadovat ruční převod.
* Metoda ```Marshal.GetActiveObject()``` sloužící k získání spuštěné instance COM zadaného objektu již není k dispozici.
    * Pokud znáte cestu k používanému souboru, použijte metodu ```BindToMoniker```.
    * Vytvořte knihovnu v jazyce C# využívající strukturu třídy ```Marshal.GetActiveObject()```.

#### Migrace z prostředí CPython3 na modul PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Aplikace Dynamo automaticky migruje uzly CPython do modulu PythonNet3. Tento proces zahrnuje tyto kroky:

> 1. Automaticky se vytvoří záložní kopie původního souboru.
> 2. Všechny uzly CPython (včetně vlastních uzlů, které používají CPython) jsou převedeny na PythonNet3. 
> 3. Zobrazí se zpráva s informací o tom, kolik uzlů bylo migrováno.
> 4. Při ukládání se zobrazí připomenutí, že uzly jazyka Python budou nyní používat modul PythonNet3. Nemusíte se obávat zpětné kompatibility: pokud pracujete v prostředí s více verzemi (například Revit nebo Civil 3D 2025/2026), nainstalujte balíček PythonNet3 Engine v aplikaci Dynamo 3.3–3.6, čímž zajistíte zachování kompatibility. 

#### Migrace z modulu IronPython2 na PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Pokud váš graf používá modul IronPython, nedojde k automatické migraci. 

V případě, že je nainstalován odpovídající balíček IronPython, graf se spustí standardním způsobem. Jestliže balíček chybí, zobrazí se v rozšíření Reference pracovního prostoru upozornění na závislost s výzvou ke stažení balíčku. Modul IronPython můžete nadále používat přeinstalováním balíčku. Protože však IronPython již několik let nebyl aktualizován a aplikace Dynamo tyto moduly již delší dobu aktivně nepodporuje, důrazně doporučujeme migraci na modul PythonNet3, abyste zajistili, že vaše grafy budou i nadále spolehlivě fungovat. Balíčky DynamoIronPython2.7 a DynamoIronPython3 budou i nadále dostupné v nástroji Dynamo Package Manager, nebudou však již spravovány týmem aplikace Dynamo. 

V takovém případě lze provést migraci po jednotlivých uzlech pomocí nástroje Migration Assistant, který je k dispozici v editoru jazyka Python.  

Další informace o migraci naleznete v tomto [blogu](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/).