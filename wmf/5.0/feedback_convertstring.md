# Convert-String
**Convert-String** bietet „magische“ Ersetzungsfunktionalität. Geben Sie „Vorher“- und „Nachher“-Beispiele an, wie Text aussehen soll. Anschließend wird Ihr Text von **Convert-String** automatisch formatiert. Bei der folgenden Demo wird der Vor- und Nachname einer Person gewählt und durch den Nachnamen, ein Komma, den ersten Buchstaben des Vornamens und einen Punkt ersetzt. Probieren Sie es mit einem regulären Ausdruck, und prüfen Sie, wie lange Sie brauchen.

```powershell
"Lee Holmes", "Steve Lee", "Jeffrey Snover" | Convert-String -Example "Bill Gates=Gates, B.","John Smith=Smith, J."

Holmes, L.
Lee, S.
Snover, J.
```


<!--HONumber=Oct16_HO1-->


