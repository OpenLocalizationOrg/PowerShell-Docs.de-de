# Klassenbasierte DSC-Ressourcen

## Definieren von DSC-Ressourcen mit Klassen

Basierend auf Feedback haben wir das Erstellen klassenbasierter DSC-Ressourcen vereinfacht und leichter verständlich gemacht. Es folgen die wichtigsten Unterschiede zwischen einer klassenbasierten DSC-Ressource und einem DSC-Ressourcenanbieter für ein Cmdlet:

* Eine MOF-Datei für das Schema ist nicht erforderlich.
* Der Unterordner **DSCResource** im Ordner „module“ ist nicht erforderlich.
* Eine PowerShell-Moduldatei kann mehrere DSC-Ressourcenklassen enthalten.

Weitere Informationen finden Sie unter [Schreiben einer benutzerdefinierten DSC-Ressource mit PowerShell-Klassen](https://msdn.microsoft.com/powershell/dsc/authoringresource).


<!--HONumber=Oct16_HO1-->


