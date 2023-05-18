# Pull-Anforderungen 

Dynamo basiert auf der Kreativität und dem Engagement der Community. Das Dynamo-Team empfiehlt allen Beitragenden, Möglichkeiten zu erkunden, Ideen zu testen und die Community um Feedback zu bitten. Innovationen werden sehr gern gesehen. Änderungen werden jedoch nur dann aufgenommen, wenn sie die Verwendung von Dynamo erleichtern und die in diesem Dokument definierten Kriterien erfüllen. Änderungen, die nur im kleinen Rahmen Vorteile bringen, werden nicht eingeführt.

#### Erwartungen bei Pull-Anforderungen <a href="#pull-request-expectations" id="pull-request-expectations"></a>

Das Dynamo-Team erwartet, dass bei Pull-Anforderungen einige Richtlinien befolgt werden:

* Halten Sie unsere [Coding-Standards](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) und [Namenskonventionen für Blöcke](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards) ein.
* Führen Sie beim Hinzufügen neuer Funktionen Komponententests durch.
* Fügen Sie beim Beheben eines Fehlers einen Komponententest hinzu, aus dem hervorgeht, wie das aktuelle Verhalten unterbrochen wird.
* Konzentrieren Sie sich bei der Diskussion auf ein Problem. Erstellen Sie ein neues Problemthema, wenn ein neues oder verwandtes Thema aufkommt.

Einige Richtlinien dazu, was Sie nicht tun sollten:

* Überraschen Sie uns nicht mit großen Pull-Anforderungen. Übermitteln Sie stattdessen eine Anfrage, und beginnen Sie eine Diskussion, damit wir uns auf eine Richtung einigen können, bevor Sie viel Zeit investieren.
* Führen Sie für Code, den Sie nicht geschrieben haben, kein Commit aus. Wenn Sie Code finden, der Ihrer Meinung nach gut für Dynamo geeignet wäre, übermitteln Sie eine Anfrage und beginnen eine Diskussion, bevor Sie fortfahren.
* Übermitteln Sie keine Pull-Anforderungen, mit denen lizenzbezogene Dateien oder Header geändert werden. Wenn Sie der Meinung sind, dass es ein Problem mit diesen gibt, übermitteln Sie eine Anfrage. Wir nehmen uns der Angelegenheit gerne an.
* Nehmen Sie keine API-Erweiterungen vor, ohne eine Anfrage zu übermitteln das Thema zunächst mit uns zu besprechen.

#### Ausfüllen der Vorlage für Pull-Anforderungen <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

Verwenden Sie beim Senden einer Pull-Anforderung die [vorgabemäßige Vorlage für Pull-Anforderungen](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md). Bevor Sie Ihre Pull-Anforderung einreichen, stellen Sie sicher, dass der Zweck klar beschrieben ist und alle folgenden Aussagen als Zutreffend bestätigt werden können:

* Die Codebasis ist nach dieser Pull-Anforderung in einem besseren Zustand als zuvor.
* Die Dokumentation entspricht den [Standards](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards).
* Der Umfang der Tests für diese Pull-Anforderung ist angemessen.
* Für den Benutzer sichtbare Zeichenfolgen, sofern vorhanden, wurden in `*.resx`-Dateien extrahiert.
* Alle Tests mit Self-Service-CI sind erfolgreich.
* Snapshots von Änderungen an der Benutzeroberfläche, sofern vorhanden, liegen vor.
* Änderungen an der API folgen der [semantischen Versionsverwaltung](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions) und sind im Dokument für [API-Änderungen](https://github.com/DynamoDS/Dynamo/wiki/API-Changes) dokumentiert.

Das Dynamo-Team weist Ihrer Pull-Anforderung einen geeigneten Prüfer zu.

#### Überprüfungsprozess für Pull-Anforderungen <a href="#pull-request-review-process" id="pull-request-review-process"></a>

Nachdem eine Pull-Anforderung übermittelt wurde, werden Sie möglicherweise während des Überprüfungsprozesses weiter mit einbezogen. Beachten Sie die folgenden Überprüfungskriterien:

* Das Dynamo-Team kommt einmal im Monat zusammen, um Pull-Anforderungen zu prüfen. Es werden zunächst die ältesten, dann die neueren Anforderungen geprüft.
* Wenn eine überprüfte Pull-Anforderung Änderungen durch den Eigentümer erfordert, hat der Eigentümer der Pull-Anforderung 30 Tage Zeit, um zu antworten. Wenn bei der Pull-Anforderung bis zur nächsten Sitzung keine Aktivität stattgefunden hat, wird sie entweder vom Team geschlossen oder, je nach Nützlichkeit, von einem Mitglied des Teams übernommen.
* Für Pull-Anforderungen sollte die vorgegebene Vorlage für Pull-Anforderungen von Dynamo verwendet werden.
* Pull-Anforderungen, bei denen die Vorlage für Pull-Anforderungen von Dynamo nicht vollständig ausgefüllt ist bzw. bei denen nicht alle Richtlinien erfüllt werden, werden nicht überprüft.

#### Cherrypicking bei Dynamo Revit-Commits <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Da es auf dem Markt viele Versionen von Revit gibt, müssen Sie Ihre Änderungen möglicherweise in bestimmte Verzweigungen von DynamoRevit-Versionen integrieren, damit die neuen Funktionen in anderen Versionen von Revit aufgegriffen werden können. Während des Überprüfungsprozesses sind die Beitragenden dafür verantwortlich, die überprüften Commits nach Angabe des Dynamo-Teams in die spezifischen anderen DynamoRevit-Verzweigungen zu integrieren.
