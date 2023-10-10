# Objektbindung

Dynamo for Civil 3D verfügt über einen leistungsstarken Mechanismus, mit dem sich das Programm die von jedem Block erstellten Objekte "merken" kann. Dieser Mechanismus wird als **Objektbindung** bezeichnet und ermöglicht es einem Dynamo-Diagramm, bei jeder Ausführung im selben Dokument konsistente Ergebnisse zu erzielen. Dies ist in vielen Fällen sehr wünschenswert, aber es gibt auch andere Situationen, in denen Sie mehr Kontrolle über das Verhalten von Dynamo haben möchten. In diesem Abschnitt erfahren Sie, wie die Objektbindung funktioniert und wie Sie sie nutzen können.

## Beispiel

Betrachten Sie dieses Diagramm, das einen Kreis im Modellbereich auf dem aktuellen Layer erstellt.

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>Ein einfaches Diagramm für die Erstellung eines Kreises</p></figcaption></figure>

Achten Sie darauf, was passiert, wenn der Radius geändert wird.

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>Ändern der Radiuseingabe in Dynamo</p></figcaption></figure>

Dies ist Objektbindung in Aktion. Dynamo _ändert_ vorgabemäßig den Radius des Kreises, anstatt bei jeder Änderung der Radiuseingabe einen neuen Kreis zu erstellen. Dies liegt daran, dass sich der Block **Object.ByGeometry** "merkt", dass er diesen _spezifischen_ Kreis bei jeder Ausführung des Diagramms erstellt hat. Außerdem speichert Dynamo diese Informationen, sodass sie beim nächsten Öffnen des Civil 3D-Dokuments und Ausführen des Diagramms genau das gleiche Verhalten aufweisen.

## Ein weiteres Beispiel

Sehen wir uns ein Beispiel an, in dem Sie das vorgabemäßige Objektbindungsverhalten von Dynamo möglicherweise ändern möchten. Angenommen, Sie möchten ein Diagramm erstellen, das Text in der Mitte eines Kreises platziert. Sie möchten mit diesem Diagramm jedoch erreichen, dass es immer wieder ausgeführt und jedes Mal neuer Text in den ausgewählten Kreis eingefügt werden kann. So könnte dieses Diagramm aussehen.

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>Ein einfaches Diagramm, das Text in der Mitte eines ausgewählten Kreises platziert</p></figcaption></figure>

Dies geschieht jedoch tatsächlich, wenn ein anderer Kreis ausgewählt wird.

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>Vorgabemäßiges Verhalten von Dynamo bei Auswahl eines neuen Kreises</p></figcaption></figure>

Offenbar wird der Text gelöscht und mit jedem Durchlauf des Diagramms neu erstellt. In Wirklichkeit wird die Position des Texts _geändert_, abhängig davon, welcher Kreis ausgewählt ist. Es ist also derselbe Text, nur an einer anderen Stelle! Um jedes Mal neuen Text zu erstellen, müssen Sie die Einstellungen für die Objektbindung in Dynamo ändern, sodass keine Bindungsdaten beibehalten werden (siehe [\#binding-settings](object-binding.md#binding-settings "mention") unten).

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Einstellungen für Objektbindung</p></figcaption></figure>

Nachdem wir diese Änderung vorgenommen haben, erhalten wir das gewünschte Verhalten.

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>Verhalten bei deaktivierter Objektbindung</p></figcaption></figure>

## Bindungseinstellungen

Dynamo for Civil 3D ermöglicht die Änderung des vorgegebenen Verhaltens für Objektbindungen über die Einstellungen für **Bindungsdatenspeicherung** im Menü **Dynamo**.

{% hint style="info" %}\r\n Beachten Sie, dass die Optionen für die Bindungsdatenspeicherung in **Civil 3D 2022.1** und höher verfügbar sind. \r\n{% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

Alle Optionen sind vorgabemäßig aktiviert. Hier finden Sie eine Zusammenfassung der Funktionen der einzelnen Optionen.

### Option 1: Keine Bindungsdaten beibehalten

Wenn diese Option aktiviert ist, werden die Objekte, die beim letzten Ausführen des Diagramms erstellt wurden, von Dynamo "vergessen". Das Diagramm kann also in jeder beliebigen Zeichnung ausgeführt werden und erstellt jedes Mal neue Objekte.

{% hint style="info" %}\r\n **Verwendung**

Verwenden Sie diese Option, wenn Dynamo alle Aktionen aus früheren Durchläufen "vergessen" und jedes Mal neue Objekte erstellen soll. \r\n{% endhint %}

### Option 2: In Diagramm für Dynamo speichern

Diese Option bedeutet, dass Metadaten der Objektbindung in das Diagramm (DYN-Datei) serialisiert werden, wenn es gespeichert wird. Wenn Sie das Diagramm schließen/erneut öffnen und es in **derselben Zeichnung** ausführen, sollte alles wie beim letzten Mal funktionieren. Wenn Sie das Diagramm in einer **anderen Zeichnung** ausführen, werden die Bindungsdaten aus dem Diagramm entfernt und neue Objekte erstellt. Dies bedeutet, dass beim Öffnen der ursprünglichen Zeichnung und erneuten Ausführen des Diagramms neue Objekte zusätzlich zu den alten erstellt werden.

{% hint style="info" %}\r\n **Verwendung**

Verwenden Sie diese Option, wenn sich Dynamo die Objekte "merken" soll, die beim letzten Ausführen in einer **bestimmten Zeichnung** erstellt wurden. \r\n{% endhint %}

{% hint style="warning" %}\r\n Diese Option eignet sich am besten für Situationen, in denen eine 1:1-Beziehung zwischen einer **bestimmten Zeichnung** und einem Dynamo-Diagramm beibehalten werden kann. Optionen 1 und 3 eignen sich besser für Diagramme, die für die Ausführung in mehreren Zeichnungen ausgelegt sind. \r\n{% endhint %}

### Option 3: In Zeichnung für Dynamo speichern

Dies ähnelt Option 2, mit der Ausnahme, dass die Objektbindungsdaten in der Zeichnung und nicht im Diagramm (DYN-Datei) serialisiert werden. Wenn Sie das Diagramm schließen/erneut öffnen und es in **derselben Zeichnung** ausführen, sollte alles wie beim letzten Mal funktionieren. Wenn Sie das Diagramm in einer **anderen Zeichnung** ausführen, bleiben die Bindungsdaten in der ursprünglichen Zeichnung erhalten, da sie in der Zeichnung und nicht im Diagramm gespeichert werden.

{% hint style="info" %}\r\n **Verwendung**

Verwenden Sie diese Option, wenn Sie dasselbe Diagramm in **mehreren Zeichnungen** verwenden möchten und Dynamo sich dabei "merken" soll, wie es in jeder Zeichnung dargestellt wurde. \r\n{% endhint %}

### Option 4: In Zeichnung für Dynamo Player speichern

Bei dieser Option ist zunächst zu beachten, dass sie keine Auswirkung darauf hat, wie das Diagramm mit der Zeichnung interagiert, wenn das Diagramm über die Dynamo-Hauptbenutzeroberfläche ausgeführt wird. Diese Option ist _nur_ dann relevant, wenn das Diagramm mit Dynamo Player ausgeführt wird.

{% hint style="info" %}\r\n Wenn Dynamo Player neu für Sie ist, finden Sie im Abschnitt [dynamo-player.md](../dynamo-player.md "mention") weitere Informationen. \r\n{% endhint %}

Wenn Sie das Diagramm über die Dynamo-Hauptbenutzeroberfläche ausführen, es schließen und anschließend mit Dynamo Player ausführen, werden zusätzlich zu den zuvor erstellten Objekten neue Objekte erstellt. Sobald das Diagramm jedoch einmal von Dynamo Player ausgeführt wurde, werden die Objektbindungsdaten in der Zeichnung serialisiert. Wenn Sie das Diagramm also mehrmals mit Dynamo Player ausführen, werden Objekte aktualisiert, statt neue zu erstellen. Wenn Sie das Diagramm mit Dynamo Player in einer **anderen Zeichnung** ausführen, bleiben die Bindungsdaten in der ursprünglichen Zeichnung erhalten, da sie in der Zeichnung und nicht im Diagramm gespeichert werden.

{% hint style="info" %}\r\n **Verwendung**

Verwenden Sie diese Option, wenn Sie ein Diagramm mit Dynamo Player über mehrere Zeichnungen hinweg ausführen und Dynamo Player sich dabei "merken" soll, wie es in jeder Zeichnung dargestellt wurde. \r\n{% endhint %}
