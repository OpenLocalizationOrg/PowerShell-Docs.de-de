#Beitragende in PowerShell-Dokumentation

##Melden Sie sich eine CLA

Bevor Sie zu jeder PowerShell-Repositorys beitragen können, müssen Sie [Melden Sie sich einen Microsoft Beitrag Licensing Agreement (CLA)](https://cla.microsoft.com/). 
Wenn Sie bereits zum PowerShell-Repositories in der Vergangenheit Herzlichen Glückwunsch beigetragen haben! Sie haben diesen Schritt bereits abgeschlossen.

##Schreiben von PowerShell-Dokumentation

Der einfachste Weg zur Teilnahme an der PowerShell ist dazu beiträgt, schreiben und Bearbeiten der Dokumentation. 
Alle unsere Dokumentation auf GitHub gehostet wird geschrieben, mit [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/).

Um [Bearbeiten Sie eine vorhandene Datei](https://help.github.com/articles/editing-files-in-another-user-s-repository/), einfach zu navigieren, und klicken Sie auf die Schaltfläche "Bearbeiten". 
GitHub erstellt automatisch eigene Verzweigung unseres Repositorys, in dem Sie die Änderungen vornehmen können. 
Sobald Sie fertig sind, speichern Sie Ihre Änderungen zu, und übermitteln Sie eine Pull-Anforderung zum Abrufen von Änderungen upstream zusammengeführt. 
Nachdem Ihre Pull Anforderung erstellt wird,

Wenn Sie neue Dokumentation beitragen möchten, suchen Sie zunächst nach [Probleme, die als "in Bearbeitung" markiert](https://github.com/PowerShell/PowerShell-Docs/labels/in%20progress) um sicherzustellen, dass Sie sind Maßnahmen nicht duplizieren.
Wenn niemand scheint arbeiten, was Sie geplant haben:
* Öffnen Sie ein neues Problem als "in Bearbeitung" andere mitteilen, was Sie gerade arbeiten
* Erstellen Sie eine Verzweigung unseres Repositorys und starten Sie neuer Abzug basierende Dokumentation hinzugefügt
* Wenn Sie bereit sind, Ihre Dokumentation beitragen, fordern Sie ein Pullabonnement auf dem *master* Verzweigung

####GitHub Flavored Markdown (GFM)

Alle Artikel in diesem Repository [GitHub Flavored Markdown (GFM)](https://help.github.com/articles/github-flavored-markdown/).

Wenn Sie ein guter Editor suchen, versuchen Sie [Abzug Pad](http://markdownpad.com/) oder 
GitHub bietet auch eine Weboberfläche für Abzug bearbeiten mit syntaxhervorhebung und die Möglichkeit, eine Vorschau der Änderungen erfolgen.

Einige der einfacheren GFM Syntax enthält:

* **Zeilenumbrüche im Vergleich zu Absätze:** gibt es In Abzug ist kein HTML `< Br / >` oder `< p / >` Element. 
   Stattdessen wird ein neuer Absatz durch eine leere Zeile zwischen zwei Textblöcke festgelegt.

> **Hinweis**: Fügen Sie ein einzelnes Zeilenumbruch nach jeder Satz vereinfachen Sie die Befehlszeilenausgabe von Differenzen und Verlauf hinzu.
> Dies ist nicht gerade über alle PowerShell-Dokumente eingeführt, aber wir gegen diese Zeit arbeiten werden. Gerne können helfen.

* **Kursiv:** HTML `< Em > Text </em >` lautet `* einige Text *`
* **Fett:** HTML `< strong > Text < / strong >` Element lautet `** einige Text **`
* **Überschriften:** HTML-Überschriften bestimmt sind, mit `#` Zeichen am Anfang der Zeile. 
   Die Anzahl der `#` Zeichen entspricht der hierarchischen Ebene der Überschrift (z. B. `#` = `< h1 >` und `###` = `< h3 >`).
* **Nummerierung:** eine nummerierte (geordnete) Liste, die die Zeile mit "1 beginnen soll. `.  
   Sie ggf. mehrere Elemente in einem einzigen Listenelement Formatieren der Liste wie folgt:
```        
1.  For the first element (like this one), insert a tab stop after the 1. 

    To include a second element (like this one), insert a line break after the first and align indentations.
```
Um diese Ausgabe zu erhalten:

1.  Legen Sie für das erste Element (wie diese) einen Tabstopp nach dem 1.
   
   Um ein zweites Element (wie diese) einzuschließen, fügen Sie einen Zeilenumbruch nach dem ersten aus, und richten Sie Einzüge.
   
   * **Aufzählungslisten:** Aufzählung (ungeordnete) sind fast identisch mit sortierten außer dass ' 1 aufgelistet. ` ersetzt wird, entweder mit `* `, `- `, oder `+ '. 
      Mehrere Element Listen funktionieren genauso wie bei sortierte Listen.
   * **Links:** ist die Syntax für einen Hyperlink `[Link sichtbaren Text](link url)`.
      Links können auch Verweise, haben die im Abschnitt "Link und Bildverweise" erläutert werden.



