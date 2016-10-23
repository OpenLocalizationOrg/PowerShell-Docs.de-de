# Beiträge zur PowerShell-Dokumentation

## CLA unterzeichnen

Bevor Sie zu PowerShell-Repositorys beitragen können, müssen Sie [ein Microsoft Contribution Licensing Agreement (CLA) unterzeichnen](https://cla.microsoft.com/). 
Falls Sie bereits in der Vergangenheit zu PowerShell-Repositorys beigetragen haben, Glückwunsch! Sie haben diesen Schritt bereits abgeschlossen.

## PowerShell-Dokumentation schreiben

Einer der einfachsten Wege, zu PowerShell beizutragen, besteht darin, Dokumentation zu schreiben und zu bearbeiten.
Unsere gesamte auf GitHub gespeicherte Dokumentation ist mit [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/) geschrieben.

Zum [Bearbeiten einer vorhandenen Datei](https://help.github.com/articles/editing-files-in-another-user-s-repository/) navigieren Sie einfach zu dieser Datei und klicken auf die Schaltfläche „Bearbeiten“. 
GitHub erstellt dann automatisch Ihren eigenen Fork unseres Repositorys, in dem Sie die Änderungen vornehmen können.
Sobald Sie fertig sind, speichern Sie Ihre Bearbeitung und übermitteln Sie einen Pull Request, damit Ihre Änderungen mit dem Basis-Repository zusammengeführt werden können.

Wenn Sie neue Dokumentation beitragen möchten, werfen Sie zunächst einen Blick auf [Vorgänge, die als „in progress“ gekennzeichnet sind](https://github.com/PowerShell/PowerShell-Docs/labels/in%20progress), damit Ihre Arbeit nicht umsonst ist, da bereits jemand anders daran arbeitet.
Falls scheinbar niemand an dem arbeitet, was Sie im Sinn haben:
* Öffnen Sie einen neuen Vorgang, der als „in progress“ gekennzeichnet ist, sodass andere erfahren, woran Sie arbeiten.
* Erstellen Sie einen Fork unseres Repositorys und fügen Sie neue Markdown-basierte Dokumentation hinzu.
* Wenn Sie bereit sind, Ihre Dokumentation beizutragen, übermitteln Sie einen Pull Request an den *master*-Branch.

#### GitHub Flavored Markdown (GFM)

Alle Artikel in diesem Repository nutzen [GitHub Flavored Markdown (GFM)](https://help.github.com/articles/github-flavored-markdown/).

Sie sind auf der Suche nach einem guten Editor? Versuchen Sie es mit [Markdown Pad](http://markdownpad.com/).
Alternativ bietet GitHub auch eine Web-Oberfläche zur Markdown-Bearbeitung an, die über Syntax-Hervorhebung und eine Vorschau-Funktion für Änderungen verfügt.

Einige grundlegende GFM-Elemente:

* **Zeilenumbrüche oder neue Absätze:** In Markdown gibt es kein HTML-Element `<br />` oder `<p />`. 
Stattdessen wird ein neuer Absatz durch eine Leerzeile zwischen zwei Textblöcken gekennzeichnet.

> **Hinweis**: Fügen Sie nach jedem Satz einen einzelnen Zeilenumbruch ein, damit Diffs und der Verlauf in der Befehlszeile leichter angezeigt werden können.
Dies ist momentan nicht im gesamten Repository PowerShell-Docs der Fall, wir arbeiten aber darauf hin. Sie können gern mithelfen.

* **Kursiv:** Das HTML-Element `<em>Text-Beispiel</em>` wird als `*Text-Beispiel*` geschrieben.
* **Fett:** Das HTML-Element `<strong>Text-Beispiel</strong>` wird als `**Text-Beispiel**` geschrieben.
* **Überschriften:** HTML-Überschriften werden durch `#`-Zeichen am Zeilenanfang kenntlich gemacht. 
Dabei entspricht die Anzahl der `#`-Zeichen der hierarchischen Ebene der Überschrift (beispielsweise steht `#` für `<h1>` und `###` für `<h3>`).
* **Nummerierung:** Zur Erstellung einer Nummerierung (nummerierten Liste) beginnen Sie die Zeile mit `1. `.
Möchten Sie mehrere Elemente in ein einzelnes Listen-Element aufnehmen, formatieren Sie die Liste wie folgt:
```      
1.  Beim ersten Listen-Element (wie bei diesem) fügen Sie einen Tabstopp nach der 1. ein.

    Bei weiteren Elementen (wie bei diesem) fügen Sie einen Zeilenumbruch nach dem ersten Element ein und gleichen Sie den Einzug aus.
```
to get this output:

1.  Beim ersten Listen-Element (wie bei diesem) fügen Sie einen Tabstopp nach der 1. ein.

    Bei weiteren Elementen (wie bei diesem) fügen Sie einen Zeilenumbruch nach dem ersten Element ein und gleichen Sie den Einzug aus.

* **Aufzählungen:** Listen ohne vorangestellte Ziffern (unsortierte Listen, auch Aufzählungen genannt) sind mit Nummerierungen fast identisch, mit einer Ausnahme: `1. ` wird durch `* `, `- `, oder `+ ` ersetzt. 
Aufzählungen mit mehreren Elementen werden genauso formatiert wie bei Nummerierungen.
* **Links:** Die Syntax für einen Hyperlink lautet `[sichtbarer Link-Text](Link-URL)`.
Links können auch Referenzen enthalten, die im Abschnitt „Link- und Bild-Referenzen“ erläutert werden.
