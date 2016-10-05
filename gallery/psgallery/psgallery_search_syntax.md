# Suchsyntax für die Galerie

PowerShell-Galerie bietet eine Text-Searchbox, in dem Wörtern, Ausdrücken und Schlüsselwort Ausdrücke verwenden können, um Suchergebnisse einzuschränken.

## Suche nach Schlüsselwörtern

    dsc azure sql

Suche wird führen Sie nach Möglichkeit die relevanten Dokumente, die alle 3 Schlüsselwörter finden und übereinstimmende Dokumente zurückgeben.

## Suchen Sie mithilfe von Ausdrücken und Schlüsselwörter

    "azure sql" deployment

Eingabe eines Ausdrucks zwischen Anführungszeichen ("") ändern Sie die Suche nach dem bestimmten Begriff statt getrennte Schlüsselwörter suchen.
Übereinstimmende Dokumente sollte in der Regel den exakten Ausdruck "Azure Sql", einschließlich der Varianten nach Groß-/Kleinschreibung z. B. enthalten "SQL Azure", und auch in der Regel das Wort 'Bereitstellung' enthalten.

## Filtern von Feldern

Sie können für ein bestimmtes Element-ID (oder "Id" oder "Id") suchen, oder bestimmte andere Felder werden mit dem Präfix Suchbegriffe mit dem Namen des Felds.

Aktuell sind die durchsuchbaren Felder "Id", "Version", "Tags", 'Author', "Besitzer", "Funktionen", 'Cmdlets', 'DscResources' und 'PowerShellVersion'.

[Was ist der Unterschied zwischen ID und Titel? ID ist der Name, den Sie in der Konsole verwenden. Der Titel ist was am oberen Rand der Seite Element in den Suchergebnissen angezeigt werden.]

## Beispiele

    ID:"PSReadline"
    id:"AzureRM.Profile"

Elemente mit "PSReadline" oder "AzureRM.Profile" bzw. die Feld-ID gefunden.

    Id:"AzureRM.Profile"

ist eine Möglichkeit, Elemente mit "AzureRM.Profile" in die Feld-ID gefunden.

Der Filter "Id" ist eine Teilzeichenfolge überein, so suchen Sie die folgenden:

    Id:"azure"
    
Sie erhalten die Ergebnisse wie 'AzureRM.Profile' und 'Azure.Storage'.

Sie können auch mehrere Schlüsselwörter in einem einzelnen Feld Suchen. Oder mischen und Zuordnen von Feldern.

    id:azure tags:intellisense
    id:azure id:storage

Und Sie können die Suche nach Ausdrücken ausführen:

    id:"azure.storage"


Um alle Elemente mit DSC-Tag zu suchen.

    Tags:"DSC"

Um alle Elemente mit der angegebenen Funktion zu suchen.

    Functions:"Update-AzureRM"

Um alle Elemente mit dem angegebenen Cmdlet zu suchen.
    
    Cmdlets:"Get-AzureRmEnvironment"

Um alle Elemente mit dem angegebenen Namen der DSC-Ressourcen zu suchen.

    DscResources:"xArchive"

Um alle Elemente mit dem angegebenen PowerShellVersion zu suchen.

    PowerShellVersion:"5.0"
    PowerShellVersion:"3.0"
    PowerShellVersion:"2.0"


Abschließend, wenn Sie ein Feld verwenden, die wir unterstützen, z. B. "Befehle", wir einfach ignorieren und suchen alle Felder. Damit die folgende Abfrage

    commands:blobs storage
    
Ist genau wie diese Abfrage interpretiert:

    blobs storage



<!--HONumber=Oct16_HO1-->


