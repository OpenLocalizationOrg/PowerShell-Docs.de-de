# Extrahieren und Analysieren von strukturierten Objekten aus einer Zeichenfolge
Auch das Cmdlet „ConvertFrom-String“ weist einige neue Funktionen auf:

-   Die „ExtentText“-Eigenschaft wird standardmäßig entfernt. Sie können sie mit dem „-IncludeExtent“-Parameter einschließen.

-   Viele Fehlerbehebungen beim lernenden Algorithmus basierend auf Feedback von MVPs und Community.

-   Neuer „-UpdateTemplate“-Parameter zum Speichern der Ergebnisse des lernenden Algorithmus in einem Kommentar in der Vorlagendatei. Dadurch wird der Lernprozess (die langsamste Phase) zu einem einmaligen Aufwand. Das Ausführen von „Convert-String“ mit einer Vorlage, die den codierten lernenden Algorithmus enthält, erfolgt nun fast unmittelbar.


Extrahieren und Analysieren von strukturierten Objekten aus einer Zeichenfolge
----------------------------------------------------------

In Zusammenarbeit mit [Microsoft Research](http://research.microsoft.com/) wurde das neue Cmdlet **ConvertFrom-String** hinzugefügt.

Dieses Cmdlet unterstützt zwei Modi: getrennte Analyse und von einem automatisch generierten Beispiel gesteuerte Analyse.

Bei der getrennten Analyse wird die Eingabe standardmäßig bei Leerzeichen getrennt, wobei den resultierenden Gruppen Eigenschaftsnamen zugewiesen werden. Sie können das Trennzeichen anpassen:

> 1 \[C:\\temp\] &gt;&gt; "Hello World" | ConvertFrom-Zeichenfolge | Format-Table-automatisch

P1    P2
--    --

Das Cmdlet unterstützt auch die von einem automatisch generierten Beispiel gesteuerte Analyse basierend auf den Forschungsarbeiten zu [FlashExtract](http://research.microsoft.com/en-us/um/people/sumitg/flashextract.html) von [Microsoft Research](http://research.microsoft.com).

Nehmen wir zum Einstieg ein textbasiertes Adressbuch:

    Ana Trujillo

    Redmond, WA

    Antonio Moreno

    Renton, WA

    Thomas Hardy

    Seattle, WA

    Christina Berglund

    Redmond, WA

    Hanna Moos

    Puyallup, WA

Kopieren Sie einige Beispiele in eine Datei, die Sie als Vorlage verwenden werden:

    Ana Trujillo

    Redmond, WA

    Antonio Moreno

    Renton, WA

   

Setzen Sie Daten, die Sie extrahieren möchten, in geschweifte Klammern, und geben Sie ihnen einen Namen. Da die **Name**-Eigenschaft (und ihre zugehörigen anderen Eigenschaften) mehrfach vorkommen können, verwenden Sie ein Sternchen (\*), um anzugeben, dass dadurch mehrere Datensätze generiert werden (statt eine Reihe von Eigenschaften in einen Datensatz zu extrahieren):

    {Name\*:Ana Trujillo}

    {City:Redmond}, {State:WA}

    {Name\*:Antonio Moreno}

    {City:Renton}, {State:WA}

Ausgehend von diesen Beispielen kann **ConvertFrom-String** nun eine objektbasierte Ausgabe automatisch aus Eingabedateien mit ähnlicher Struktur extrahieren.

> 2 \[C:\\temp\]
>
> &gt;&gt; Get-Content.\\addresses.output.txt | ConvertFrom-Zeichenfolge TemplateFile -.\\addresses.template.txt | &gt;&gt;&gt; Format-Table-automatisch
>
> ExtentText                     Name               City     State
> ----------                     ----               ----     -----
> Ana Trujillo...                Ana Trujillo       Redmond  WA Antonio Moreno...              Antonio Moreno     Renton   WA Thomas Hardy...                Thomas Hardy       Seattle  WA Christina Berglund...          Christina Berglund Redmond  WA Hanna Moos...                  Hanna Moos         Puyallup WA

Um extrahierten Text weiter zu bearbeiten, erfasst die **ExtentText**-Eigenschaft den unformatierten Text, anhand dessen der Datensatz extrahiert wurde. Zum Bereitstellen von Feedback zu dieser Funktion und Freigeben von Inhalten für die Sie mehr Beispiele schreiben Sie eine e-Mail an <psdmfb@microsoft.com>.



<!--HONumber=Oct16_HO1-->


