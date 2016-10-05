# Elemente mit kompatiblen PowerShell-Editionen
Ab Version 5.1 steht PowerShell in verschiedenen Editionen zur Verfügung, die unterschiedliche Featuregruppen und Plattformkompatibilität bieten.

- **Desktop-Edition:** Diese Edition basiert auf .NET Framework und bietet Kompatibilität mit Skripts und Modulen für Versionen von PowerShell, die unter Vollversionen von Windows wie Server Core und Windows Desktop ausgeführt werden.
- **Core-Edition:** Diese Edition basiert auf .NET Core und bietet Kompatibilität mit Skripts und Modulen für Versionen von PowerShell, die unter funktionsreduzierten Versionen von Windows wie Nano Server und Windows IoT ausgeführt werden.

## PowerShell-Galerie unterstützten PSEditions Metadaten extrahiert und Sie können Filter Elemente kompatibel für bestimmte Editionen von PowerShell

Verfügt ein Element kompatibel PSEditions angegeben, als Teil der "PowerShell Editionen" aufgeführt werden in der Anzeige Artikelseite und in Elemente.
![Anzeigen der Artikelseite mit PSEditions](Images/ItemDisplayPageWithPSEditions.PNG)

## Suchen Sie nach Elementen in der Galerie auf PowerShellCore funktioniert Benutzeroberfläche
Verwenden von Tags: Tags und "PSEdition_Desktop": "PSEdition_Core" zum Filtern der Elemente in der PowerShell-Galerie.

### Verwenden von Tags: "PSEdition_Core", um Elemente mit PowerShell Core Edition kompatibel zu suchen.
![Suchergebnisse für Elemente mit Core PSEdition kompatibel](Images/SearchResultsWithPSEditions.PNG)

### Verwenden von Tags: "PSEdition_Desktop", um Elemente mit PowerShell Desktop Edition kompatibel zu suchen.
![Suchergebnisse für Elemente mit Desktop PSEdition kompatibel](Images/SearchResultsWithPSEdition_Desktop.PNG)

## Weitere Informationen zu erstellen, und suchen die Elemente mit kompatiblen PowerShell-Editionen
### [Module mit PSEditions](../psget/module/modulewithpseditionsupport.md)
### [Skripts mit PSEditions](../psget/script/scriptwithpseditionsupport.md)

<!--HONumber=Oct16_HO1-->


