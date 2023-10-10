# Vorgehensweisen für Diagramme

In den vorangegangenen Kapiteln dieses Handbuchs wurde bereits behandelt, wie Sie die leistungsstarken Funktionen zur visuellen Programmierung in Dynamo einsetzen können. Ein gutes Verständnis dieser Funktionen ist eine solide Grundlage und der erste Schritt bei der Erstellung zuverlässiger visueller Programme. Bei der Verwendung visueller Programme in der Praxis, der Weitergabe an Kollegen, der Behebung von Fehlern oder beim Testen von Grenzen müssen zusätzliche Aspekte berücksichtigt werden. Wenn andere Benutzer mit Ihrem Programm arbeiten sollen oder Sie damit rechnen, es z. B. sechs Monate später erneut zu öffnen, müssen seine Grafik und seine Logik unmittelbar verständlich sein. Dynamo stellt zahlreiche Werkzeuge zur Verfügung, die Ihnen helfen, die Komplexität Ihres Programms zu bewältigen. In diesem Kapitel finden Sie Richtlinien zu ihren Verwendungszwecken.

![Gruppen](images/1/cad-chart-visual.jpg)

## Komplexität reduzieren

Während Sie Ihr Dynamo-Diagramm entwickeln und Ihre Ideen testen, kann es rasch an beachtlicher Größe und Komplexität zunehmen. Natürlich ist es wichtig, ein funktionsfähiges Programm zu erstellen, es sollte jedoch auch möglichst einfach gehalten werden. Das Diagramm lässt sich so nicht nur schneller und besser vorhersehbar ausführen, sondern seine Logik ist dadurch für Sie und andere Benutzer problemlos verständlich. Im Folgenden werden einige Methoden beschrieben, mit denen Sie die Logik Ihres Diagramms verdeutlichen können.

### **Modularisieren mit Gruppen**

* Gruppen ermöglichen es, bei der Entwicklung eines Programms **separate Teile mit unterschiedlichen Funktionen** zu erstellen.
* Mithilfe von Gruppen können Sie darüber hinaus **große Teile des Programms verschieben**, wobei die Modularität und Ausrichtung erhalten bleiben.
* Sie können die **Farbe einer Gruppe zur Differenzierung** ihres Verwendungszwecks (Eingaben oder Funktionen) ändern.
* Gruppen können als Ausgangspunkt beim **Organisieren des Diagramms zur Vereinfachung der Erstellung benutzerdefinierter Blöcke** verwendet werden.

![](images/1/graphstrategy2.png)

> Die Farben in diesem Programm kennzeichnen den Verwendungszweck der einzelnen Gruppen. Mithilfe dieses Verfahrens können Sie eine Hierarchie in den von Ihnen entwickelten Grafikstandards oder -vorlagen erstellen.
>
> 1. Funktionsgruppe (blau)
> 2. Eingabengruppe (orange)
> 3. Skriptgruppe (grün)
>
> Informationen zur Verwendung von Gruppen finden Sie unter [Verwalten Ihres Programms](https://primer2.dynamobim.org/v/de/9_best_practices/4-managing-your-program).

### **Effizientere Entwicklung mit Codeblöcken**

* In manchen Fällen können Sie in einem Codeblock **eine Methode für eine Zahl oder einen Block schneller eingeben, als Sie nach ihr suchen könnten** (Point.ByCoordinates, Number, String, Formula).
* Codeblöcke sind nützlich zum **Definieren benutzerdefinierter Funktionen in DesignScript, damit weniger Blöcke im Diagramm benötigt werden**.

![](images/1/graphstrategy3\(1\).png)

> 1 und 2 führen dieselbe Funktion aus. Dabei nahm das Schreiben einiger Codezeilen wesentlich weniger Zeit in Anspruch als das Suchen und Hinzufügen jedes einzelnen Blocks. Die Angaben im Codeblock sind darüber hinaus wesentlich prägnanter.
>
> 1. In Codeblock geschriebenes DesignScript
> 2. Entsprechendes Programm in Blöcken
>
> Informationen zur Verwendung von Codeblöcken finden Sie unter [Was ist ein Codeblock?](../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/1-what-is-a-code-block.md)

### **Komprimieren mit Block zu Code**

* Sie können **mithilfe von Block zu Code die Komplexität eines Diagramms reduzieren**, wobei eine Gruppe einfacher Blöcke zusammengefasst und das entsprechende DesignScript in einen einzigen Codeblock geschrieben wird.
* Block zu Code kann** Code komprimieren, ohne die Verständlichkeit des Programms zu beeinträchtigen.**
* Die Verwendung von Block zu Code bietet die folgenden **Vorteile**:
  * Einfache Komprimierung von Code in eine einzige Komponente, die nach wie vor bearbeitet werden kann
  * Vereinfachung eines großen Teils eines Diagramms
  * Nützlich, wenn das „Mini-Programm“ nicht oft bearbeitet werden muss
  * Nützlich für die Integration anderer Codeblock-Funktionalität, z. B. Funktionen
* Die Verwendung von Block zu Code bringt die folgenden **Nachteile** mit sich:
  * Schlechtere Lesbarkeit wegen allgemeiner Benennung
  * Für andere Benutzer schwieriger zu verstehen
  * Keine einfache Möglichkeit, zur Version aus der visuellen Programmierung zurückzuwechseln

![](images/1/graphstrategy3\_1.png)

> 1. Vorhandenes Programm
> 2. Mithilfe von Block zu Code erstellter Codeblock
>
> Informationen zur Verwendung von Block zu Code finden Sie unter [DesignScript-Syntax](../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/2-design-script-syntax.md).

### **Flexibler Zugriff auf Daten mit List@Level**

* List@Level kann es Ihnen erleichtern, **Ihr Diagramm durch Ersetzen der Blöcke List.Map und List.Combine zu vereinfachen**, die viel Platz im Ansichtsbereich beanspruchen können.
* List@Level bietet ein **schnelleres Verfahren zum Konstruieren von Blocklogik als List.Map/List.Combine**, indem es den Zugriff auf Daten auf einer beliebigen Ebene einer Liste direkt über den Eingabeanschluss eines Blocks ermöglicht.

![](<images/1/graphstrategy4 (1).png>)

> Sie können überprüfen, wie viele True-Werte BoundingBox.Contains zurückgibt und in welchen Listen diese enthalten sind, indem Sie List@Level für den list-Eingang von CountTrue aktivieren. List@Level ermöglicht es, die Ebene festzulegen, auf der die Eingabe Daten übernimmt. List@Level ist flexibel und effizient und wird gegenüber anderen Verfahren, die List.Map und List.Combine nutzen, dringend empfohlen.
>
> 1. Zählen der True-Werte auf Listenebene 2
> 2. Zählen der True-Werte auf Listenebene 3
>
> Informationen zur Verwendung von List@Level finden Sie unter [Listen von Listen](https://primer2.dynamobim.org/v/de/5_essential_nodes_and_concepts/5-4_designing-with-lists/3-lists-of-lists).

## Lesbarkeit gewährleisten

Gestalten Sie Ihr Diagramm nicht nur so einfach und effizient wie möglich, sondern streben Sie auch eine übersichtliche grafische Darstellung an. Beziehungen sind trotz Ihrer Bemühungen, das Diagramm intuitiv mit logischen Gruppen zu gestalten, eventuell nicht ohne Weiteres zu erkennen. Ein einfacher Block innerhalb einer Gruppe oder das Umbenennen eines Schiebereglers kann Ihnen oder anderen Benutzern unnötige Verwirrung oder das Suchen im gesamten Diagramm ersparen. Im Folgenden werden mehrere Verfahren beschrieben, mit deren Hilfe Sie eine einheitliche Grafik innerhalb eines Diagramms und diagrammübergreifend erzielen können.

### **Visuelle Kontinuität durch Ausrichten der Blöcke**

* Um den Arbeitsaufwand nach dem Erstellen des Diagramms zu reduzieren, achten Sie auf eine gute Leserlichkeit des Blocklayouts, indem Sie die **Blöcke während der Arbeit häufig ausrichten**.
* Wenn andere Benutzer mit Ihrem Diagramm arbeiten sollen, **sorgen Sie vor der Bereitstellung für ein Layout mit einem leicht verständlichen Ablauf aus Blöcken und Drähten**.
* Um die Ausrichtung zu erleichtern, **verwenden Sie die Funktion Blocklayout bereinigen zur automatischen Ausrichtung** des Diagramms. Durch manuelles Ausrichten erzielen Sie allerdings präzisere Ergebnisse.

![](<images/1/graphstrategy5 (2).png>)

> 1. Ungeordnetes Diagramm
> 2. Ausgerichtetes Diagramm
>
> Informationen zur Verwendung der Blockausrichtung finden Sie unter [Verwalten von Programmen](4-managing-your-program.md).

### **Aussagekräftige Beschriftung durch Umbenennen**

* Durch Umbenennen von Eingaben machen Sie Ihr Diagramm für andere Benutzer leicht verständlich, **insbesondere, wenn Objekte, die sich außerhalb des Bildschirms befinden, verbunden werden sollen**.
* **Benennen Sie nach Möglichkeit nicht Blöcke, sondern Eingaben um.** Als Alternative dazu können Sie einen benutzerdefinierten Block aus einer Gruppe von Blöcken erstellen und ihn umbenennen. Dabei ist ersichtlich, dass andere Elemente darin enthalten sind.

![](images/1/graphstrategy6.png)

> 1. Eingaben für die Bearbeitung der Oberfläche
> 2. Eingaben für Architekturparameter
> 3. Eingaben für das Skript zur Simulation der Entwässerung
>
> Um einen Block umzubenennen, klicken Sie mit der rechten Maustaste auf seinen Namen, und wählen Sie Block umbenennen.

### **Erläuterungen durch Anmerkungen**

* Fügen Sie eine Anmerkung hinzu, wenn ein Bestandteil des **Diagramms eine Erläuterung in Klartext benötigt**, die nicht in den Blöcken selbst gegeben werden kann.
* Fügen Sie eine Anmerkung hinzu, wenn eine Sammlung von **Blöcken oder eine Gruppe zu groß oder zu komplex ist und nicht direkt verstanden werden kann**.

![](images/1/graphstrategy7.png)

> 1. Anmerkung zur Beschreibung des Teils des Programms, der Rohwerte der Verschiebungsstrecken zurückgibt
> 2. Anmerkung zur Beschreibung des Codes, der diese Werte einer Sinuswelle zuordnet
>
> Informationen zum Hinzufügen einer Anmerkung finden Sie unter [Verwalten von Programmen](https://primer2.dynamobim.org/v/de/9_best_practices/4-managing-your-program).

## Laufendes Testen

Es ist wichtig, während der Entwicklung des visuellen Skripts zu überprüfen, ob die zurückgegebenen Ergebnisse Ihren Erwartungen entsprechen. Nicht alle Fehler oder Probleme lassen das Programm sofort fehlschlagen, dies gilt insbesondere für Nullwerte, die sich erst viel später im weiteren Verlauf auswirken können. Diese Vorgehensweise wird auch im Zusammenhang mit Textskripts unter [Vorgehensweisen zur Skripterstellung](2-scripting-strategies.md) beschrieben. Das folgende Verfahren hilft Ihnen, sicherzustellen, dass Sie das gewünschte Ergebnis erzielen:

### **Überwachen von Daten mit Beobachtungs- und Vorschaublöcken**

* Verwenden Sie während der Entwicklung des Programms Beobachtungs- oder Vorschaublöcke,** um zu überprüfen, ob wichtige Ausgaben das erwartete Ergebnis zurückgeben.**

![](images/1/graphstrategy8.png)

> Mithilfe der Beobachtungsblöcke werden verglichen:
>
> 1. Die Rohwerte der Verschiebungsstrecken
> 2. Die durch die Sinusgleichung geleiteten Werte
>
> Informationen zur Verwendung der Beobachtungsfunktion finden Sie unter [Bibliothek](../3\_user\_interface/2-library.md).

## Wiederverwendbarkeit sicherstellen

Ihr Programm wird sehr wahrscheinlich irgendwann auch von anderen Benutzern geöffnet werden, selbst wenn Sie unabhängig voneinander arbeiten. Diese Benutzer sollten in der Lage sein, anhand der Ein- und Ausgaben rasch zu bestimmen, was das Programm benötigt und was es produziert. Dies ist besonders bei der Entwicklung benutzerdefinierter Blöcke wichtig, die an die Dynamo-Community weitergegeben und in Programmen anderer Benutzer verwendet werden sollen. Mit diesen Vorgehensweisen erhalten Sie zuverlässige, wiederverwendbare Programme und Blöcke.

### **Verwalten der Ein- und Ausgaben**

* Für eine optimale Lesbarkeit und Skalierbarkeit sollten Sie **die Ein- und Ausgaben auf ein Minimum beschränken**.
* Versuchen Sie, **eine Strategie zur Entwicklung der Logik zu erarbeiten, indem Sie zunächst einen groben Plan** ihrer Funktionsweise erstellen, bevor Sie den ersten Block im Ansichtsbereich einfügen. Behalten Sie während der Arbeit an diesem Plan im Auge, welche Ein- und Ausgaben in den Skripts verwendet werden sollen.

### **Verwenden von Voreinstellungen zum Einbetten von Eingabewerten**

* Falls **bestimmte Optionen oder Bedingungen vorhanden sind, die Sie in das Diagramm einbetten möchten**, empfiehlt es sich, Voreinstellungen für den schnellen Zugriff zu verwenden.
* Mithilfe von Voreinstellungen können Sie darüber hinaus **durch Caching spezifischer Schiebereglerwerte die Komplexität** in Diagrammen mit langen Laufzeiten reduzieren.

> Informationen zur Verwendung von Voreinstellungen finden Sie unter [Verwalten von Daten mit Voreinstellungen](1-graph-strategies.md#use-presets-to-embed-input-values).

### **Verwenden von benutzerdefinierten Blöcken als Container für Programme**

* Verwenden Sie einen benutzerdefinierten Block, wenn das **Programm in einem einzelnen Container zusammengefasst werden kann**.
* Verwenden Sie einen benutzerdefinierten Block, **wenn ein Teil des Diagramms oft in anderen Programmen wiederverwendet werden soll**.
* Verwenden Sie einen benutzerdefinierten Block, wenn Sie **eine Funktion für die Dynamo-Community bereitstellen** möchten.

![](images/1/graphstrategy9.png)

> Indem Sie das Programm zur Verschiebung von Punkten in einem benutzerdefinierten Block zusammenfassen, wird dieses zuverlässige, spezielle Programm portierbar und wesentlich leichter verständlich. Aussagekräftige Namen für die Eingabeanschlüsse erleichtern es anderen Benutzern, die Verwendungsweise des Blocks zu verstehen. Achten Sie darauf, für jede Eingabe eine Beschreibung und den erforderlichen Datentyp anzugeben.
>
> 1. Bestehendes Programm für Attraktor
> 2. Benutzerdefinierter Block, in dem dieses Programm, PointGrid, enthalten ist
>
> Weitere Informationen zur Verwendung benutzerdefinierter Blöcke finden Sie unter [Einführung zu benutzerdefinierten Blöcken](../6\_custom\_nodes\_and\_packages/6-1\_custom-nodes/1-introduction.md).

### **Vorlagen erstellen**

* Mithilfe von Vorlagen können Sie **Grafikstandards für alle Ihre visuellen Diagramme einrichten, um sie Ihren Kollegen in einheitlicher, verständlicher Weise bereitzustellen**.
* Beim Erstellen einer Vorlage können Sie **Gruppenfarben und Schriftgrößen** standardisieren, um Typen von Arbeitsabläufen oder Datenaktionen zu kategorisieren.
* Sie können beim Erstellen einer Vorlage sogar **Beschriftung, Farbe oder Stil für die Unterscheidung zwischen Frontend- und Backend-Arbeitsabläufen** in Ihrem Diagramm standardisieren.

![](images/1/graphstrategy10\(2\).png)

> 1. Die Benutzeroberfläche (das Frontend) des Programms umfasst den Projektnamen, die Eingabe-Schieberegler und die Importgeometrie.
> 2. Backend des Programms.
> 3. Kategorien für Gruppenfarben (allgemeines Design, Eingaben, Python-Skripts, importierte Geometrie)

## Übung – Dach in der Architektur

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

Sie haben eine Reihe optimaler Verfahren festgelegt und wenden diese jetzt auf ein rasch zusammengestelltes Programm an. Das Programm erstellt zwar wie vorgesehen das Dach, das Diagramm stellt jedoch eher eine „Mind-Map“ des Autors dar. Ihm fehlt die Struktur, und es gibt keine Beschreibung des Verwendungszwecks. Sie ordnen, beschreiben und analysieren das Programm unter Verwendung der optimalen Verfahren so, dass andere Benutzer seine Verwendungsweise verstehen.

![](images/1/graphstrategy11.png)

> Das Programm funktioniert, aber dem Diagramm fehlt Struktur.

Bestimmen Sie als Erstes die Daten und die Geometrie, die das Programm zurückgibt.

![](images/1/graphstrategy12.png)

> Um logische Unterteilungen, d. h. Modularität zu erzielen, müssen Sie die Stellen kennen, an denen wesentliche Änderungen an den Daten erfolgen. Analysieren Sie den Rest des Programms mithilfe von Beobachtungsblöcken, um festzustellen, ob Gruppen erkennbar sind, bevor Sie mit dem nächsten Schritt fortfahren.
>
> 1. Dieser **Codeblock** mit einer mathematischen Gleichung scheint ein wichtiger Bestandteil des Programms zu sein. Ein **Watch**-Block wird angezeigt, der Listen von Verschiebungsstrecken zurückgibt.
> 2. Der Zweck dieses Bereichs ist nicht ohne Weiteres ersichtlich. Die Anordnung der True-Werte auf der Listenebene L2 aus **BoundingBox.Contains** und das Vorhandensein von **List.FilterByBoolMask** lassen darauf schließen, dass ein Teil des Punktrasters als Beispiel entnommen wird.

Nachdem Sie die zugrunde liegenden Bestandteile des Programms verstanden haben, fassen Sie sie in Gruppen zusammen.

![](images/1/graphstrategy13.png)

> Gruppen ermöglichen dem Benutzer die visuelle Unterscheidung der Programmbestandteile.
>
> 1. 3D-Grundstücksmodell importieren
> 2. Punktraster entsprechend der Sinusgleichung verschieben
> 3. Bestandteil des Punktrasters als Beispiel
> 4. Dachoberfläche der Architektur erstellen
> 5. Glasfassade erstellen

Nachdem Sie die Gruppen eingerichtet haben, richten Sie die Blöcke innerhalb des Diagramms auf einheitliche Weise aus.

![](images/1/graphstrategy14.png)

> Eine einheitliche Darstellung macht den Programmablauf und die impliziten Beziehungen zwischen den Blöcken für den Benutzer leichter erkennbar.

Machen Sie das Programm noch leichter verständlich, indem Sie eine weitere Ebene grafischer Verbesserungen hinzufügen. Fügen Sie Anmerkungen hinzu, mit denen Sie die Funktionsweise eines bestimmten Programmteils beschreiben, geben Sie den Eingaben benutzerdefinierte Namen, und weisen Sie verschiedenen Typen von Gruppen Farben zu.

![](images/1/graphstrategy15\(1\).png)

> Diese grafischen Verbesserungen geben dem Benutzer genaueren Aufschluss über den Verwendungszweck des Programms. Die unterschiedlichen Farben der Gruppen helfen bei der Unterscheidung von Eingaben und Funktionen.
>
> 1. Anmerkungen
> 2. Eingaben mit aussagekräftigen Namen

Bevor Sie damit beginnen, das Programm zusammenzufassen, suchen Sie nach einem geeigneten Platz für den Python-Skript-Entwässerungssimulator. Verbinden Sie die Ausgabe der ersten skalierten Dachoberfläche mit der dazugehörigen Skripteingabe.

![](images/1/graphstrategy16.png)

> Durch die Entscheidung, das Skript an dieser Stelle des Programms zu integrieren, wird erreicht, dass die Entwässerungssimulation für die einfache Originaloberfläche des Dachs durchgeführt wird. Diese spezielle Oberfläche wird nicht in der Vorschau angezeigt, aber durch diesen Schritt entfällt die separate Auswahl der oberen Fläche in der gefasten PolySurface.
>
> 1. Quellgeometrie für Skripteingabe
> 2. Python-Block
> 3. Eingabe-Schieberegler
> 4. „Schalter“ Ein-Aus

Damit befinden sich alle Elemente an ihrem Platz, und als Nächstes vereinfachen Sie das Diagramm.

![](images/1/graphstrategy17.png)

> Durch Zusammenfassen des Programms mit Block zu Code und benutzerdefinierten Blöcken haben Sie das Diagramm erheblich verkleinert. Die Gruppen für die Erstellung der Dachoberfläche und der Wände wurden in Code konvertiert, da sie für dieses Programm hochspezifisch sind. Die Gruppe zur Verschiebung von Punkten ist in einem benutzerdefinierten Block eingeschlossen, da sie auch in anderen Programmen verwendet werden könnte. Erstellen Sie in der Beispieldatei Ihren eigenen benutzerdefinierten Block aus der Gruppe zur Verschiebung von Punkten.
>
> 1. Benutzerdefinierter Block als Container für die Gruppe zur Verschiebung von Punkten
> 2. Block zu Code für die Zusammenfassung der Gruppen zum Erstellen der Oberfläche für das Dach in der Architektur und der Wände

Im letzten Schritt erstellen Sie Voreinstellungen für als Beispiele zu verwendende Dachformen.

![](images/1/graphstrategy18.png)

> Diese Eingaben sind die wesentlichen Angaben zum Steuern der Dachform und geben den Benutzern Hinweise auf die Möglichkeiten des Programms.

Das Programm mit Ansichten zweier Voreinstellungen.

![](images/1/graphstrategy19.png)

![](images/1/graphstrategy20.png)

> Die Muster der Dachentwässerung bieten dem Benutzer eine analytische Ansicht der jeweiligen Voreinstellungen.
