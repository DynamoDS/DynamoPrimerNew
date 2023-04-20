# Bearbeiten

Zu den leistungsstarken Funktionen von Dynamo gehört die Möglichkeit zum Bearbeiten von Parametern auf parametrischer Ebene. Sie können beispielsweise die Parameter eines Arrays aus Elementen mithilfe eines generativen Algorithmus oder der Ergebnisse einer Simulation steuern. Auf diese Weise können Sie einer Gruppe von Exemplaren aus derselben Familie benutzerdefinierte Eigenschaften im Revit-Projekt zuweisen.

### Typen- und Exemplarparameter

\![Exercise](<../.gitbook/assets/32 (2).jpg>)

> 1. Exemplarparameter definieren die Öffnung der Elemente in der Dachoberfläche mit einem Öffnungsanteil zwischen 0.1 und 0.4.
> 2. Typenparameter werden auf sämtliche Elemente der Oberfläche angewendet, da diese zum selben Familientyp gehören. So kann beispielsweise das Material der einzelnen Elemente durch einen Typenparameter gesteuert werden.

![Übung](../.gitbook/assets/params.jpg)

> 1. Wenn Sie zuvor schon Revit-Familien eingerichtet haben, beachten Sie, dass Sie einen Parametertyp (Zeichenfolge, Zahl, Bemaßung usw.) zuweisen müssen. Achten Sie darauf, beim Zuweisen von Parametern in Dynamo den richtigen Datentyp zu verwenden.
> 2. Sie können Dynamo auch zusammen mit parametrischen Abhängigkeiten verwenden, die in den Eigenschaften einer Revit-Familie festgelegt wurden.

Rufen Sie sich zunächst ins Gedächtnis zurück, dass in Revit Typen- und Exemplarparameter verwendet werden. Beide können in Dynamo bearbeitet werden. In der folgenden Übung verwenden Sie jedoch Exemplarparameter.

{% hint style="info" %} Während Sie das breite Spektrum der Verwendungsmöglichkeiten für die Parameterbearbeitung kennenlernen, müssen Sie in manchen Fällen eventuell sehr zahlreiche Revit-Elemente mit Dynamo bearbeiten. Solche Vorgänge können sehr _rechenintensiv_ sein und laufen deshalb eventuell nur langsam ab. Bei der Bearbeitung zahlreicher Elemente kann es daher sinnvoll sein, die Ausführung von Revit-Vorgängen mithilfe der Funktion Anhalten vorübergehend zu unterbrechen, während Sie das Diagramm entwickeln. Weitere Informationen zum Anhalten von Blöcken finden Sie im [entsprechenden Abschnitt](../essential-nodes-and-concepts/5\_geometry-for-computational-design/5-6\_solids.md#freezing) im Kapitel Körper. {% endhint %}

### Einheiten

In Dynamo werden ab Version 0.8 grundsätzlich keine Einheiten verwendet. Dadurch bleibt Dynamo als abstrakte visuelle Programmierumgebung erhalten. Dynamo-Blöcke, die mit Revit-Bemaßungen interagieren, referenzieren die Einheiten aus dem Revit-Projekt. Wenn Sie beispielsweise in Revit einen Längenparameter aus Dynamo festlegen, entspricht dessen Zahlenwert in Dynamo den Vorgabeeinheiten im Revit-Projekt. Für die unten stehende Übung werden Meter verwendet.

Verwenden Sie zur schnellen Konvertierung von Einheiten den Block _Convert Between Units_. Dies ist ein sehr hilfreiches Werkzeug zum Konvertieren von Längen-, Flächen- und Volumeneinheiten nach Bedarf.

![](images/3/editing-units.jpg)

## Übung

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="datasets/3/Revit-Editing.zip" %}

{% hint style="warning" %} Die unten stehende Übung wird in Metern durchgeführt. {% endhint %}

In dieser Übung bearbeiten Sie Revit-Elemente, ohne geometrische Operationen in Dynamo auszuführen. In diesem Fall importieren Sie keine Dynamo-Geometrie, sondern bearbeiten lediglich Parameter in einem Revit-Projekt. Dies ist eine Übung zu Grundlagen. Benutzer mit fortgeschrittenen Revit-Kenntnissen sollten beachten, dass hier zwar die Exemplarparameter eines Körpers behandelt werden, mithilfe derselben Logik jedoch auch Arrays von Elementen umfassend angepasst werden können. Für diesen Vorgang wird der Element.SetParameterByName-Block verwendet.

### Bearbeiten von Gebäudekörperparametern

Beginnen Sie mit der Revit-Beispieldatei für diesen Abschnitt. Die Trägerelemente und adaptiven Fachwerkbinder aus dem vorigen Abschnitt wurden entfernt. Thema dieser Übung ist ein parametrisches Gerüst in Revit und seine Bearbeitung in Dynamo.

Wenn Sie das Gebäude in Revit unter Körper auswählen, wird in der Eigenschaftenpalette eine Reihe von Exemplarparametern angezeigt.

![](images/3/editing-exercise01.jpg)

In Dynamo können Sie die Parameter abrufen, indem Sie das Zielelement auswählen.

![](images/3/editing-exercise02.jpg)

> 1. Wählen Sie den Gebäudekörper mithilfe des _Select Model Element_-Blocks aus.
> 2. Sie können sämtliche Parameter dieses Körpers mithilfe des _Element.Paramaters_-Blocks abrufen. Dazu gehören Typen- und Exemplarparameter.

![](images/3/editing-exercise03.jpg)

> 1. Referenzieren Sie den _Element. Parameter_-Block, um Zielparameter zu finden. Sie können stattdessen auch die Eigenschaftenpalette aus dem vorigen Schritt anzeigen, um die Namen der zu bearbeitenden Parameter zu wählen. In diesem Fall suchen Sie nach den Parametern, die sich auf die großräumigen geometrischen Veränderungen des Gebäudekörpers auswirken.
> 2. Wir nehmen mithilfe des _Element.SetParameterByName_-Blocks Änderungen am Revit-Element vor.
> 3. Verwenden Sie einen _Code Block_, um eine Liste von Parametern mit Anführungszeichen um jedes Element zu definieren, um eine Zeichenfolge zu kennzeichnen. Sie können auch den List.Create-Block mit einer Reihe von _string_-Blöcken verwenden, die mit mehreren Eingaben verbunden sind, aber der Codeblock ist schneller und einfacher. Stellen Sie sicher, dass die Zeichenfolge dem exakten Namen in Revit entspricht, in diesem Fall: `{"BldgWidth","BldgLength","BldgHeight", "AtriumOffset", "InsideOffset","LiftUp"};`

![](images/3/editing-exercise04.jpg)

> 1. Darüber hinaus müssen Sie Werte für die einzelnen Parameter festlegen. Fügen Sie sechs _Integer Slider_-Blöcke in den Ansichtsbereich ein und weisen Sie ihnen die Namen der entsprechenden Parameter in der Liste zu. Legen Sie außerdem in den einzelnen Schiebereglern die in der Abbildung oben gezeigten Werte fest. Dies sind die folgenden Werte (von oben nach unten: 62, 92, 25, 22, 8, 12.
> 2. Definieren Sie einen weiteren _Code Block_ mit einer Liste von derselben Länge wie die Liste der Parameternamen. In diesem Fall geben Sie dabei Variablennamen (ohne Anführungszeichen) an und erhalten dadurch Eingaben für den _Code Block_. Verbinden Sie die _Schieberegler_ mit den entsprechenden Eingaben: `{bw,bl,bh,ao,io,lu};`
> 3. Verbinden Sie den Codeblock mit der Werteingabe _Element.SetParameterByName*_. Ist die Option Automatisch ausführen aktiviert, werden die Ergebnisse sofort angezeigt.

{% hint style="warning" %} *Diese Demonstration kann nur mit Exemplarparametern, nicht jedoch mit Typenparametern durchgeführt werden. {% endhint %}

Viele dieser Parameter sind genau wie in Revit voneinander abhängig. Dabei können manche Kombinationen selbstverständlich zu ungültiger Geometrie führen. Dieses Problem können Sie mithilfe definierter Formeln in den Parametereigenschaften beheben. Sie können stattdessen auch eine ähnliche Logik mit mathematischen Operationen in Dynamo einrichten. (Sie könnten dies als Zusatzübung ausprobieren.)

![](images/3/editing-exercise05.jpg)

> 1. Mit der folgenden Kombination erhalten Sie ein recht originelles Design für den Gebäudekörper: 100, 92, 100, 25, 13, 51.

### Bearbeiten von Fassadenparametern

Als Nächstes sehen wir uns an, wie die Fassade mit einem ähnlichen Prozess bearbeitet werden kann.

![](images/3/editing-exercise06.jpg)

> 1. Kopieren Sie für diese Aufgabe das Diagramm, um mit der Fassadenverglasung zu arbeiten, an der das Fachwerksystem angebracht werden soll. In diesem Fall isolieren Sie vier Parameter: `{"DblSkin_SouthOffset","DblSkin_MidOffset","DblSkin_NorthOffset","Facade Bend Location"};`
> 2. Erstellen Sie darüber hinaus _Number Slider_-Blöcke und ändern Sie ihre Namen in die der entsprechenden Parameter. Weisen Sie den ersten drei Schiebereglern (von oben nach unten) die Domäne [0,10], dem letzten Schieberegler _Facade Bend Location_ hingegen die Domäne [0,1] zu. Diese Werte sollten von oben nach unten, mit den folgenden Angaben beginnen (wobei diese hier beliebig gewählt wurden): 2.68, 2.64, 2.29, 0.5.
> 3. Definieren Sie einen neuen Codeblock, und verbinden Sie die Schieberegler: `{so,mo,no,fbl};`

![](images/3/editing-exercise07.jpg)

> 1. Indem Sie die Werte der _Schieberegler_ für diesen Teil des Diagramms ändern, können Sie die Fassadenverglasung erheblich verstärken: 9.98, 10.0, 9.71, 0.31.
