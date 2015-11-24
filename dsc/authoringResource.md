#Erstellen Sie benutzerdefinierter Windows PowerShell gewünscht State Configuration-Ressourcen

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Windows PowerShell gewünscht State Configuration (DSC) verfügt über integrierte Ressourcen, mit denen Sie Ihre Umgebung konfigurieren. (Weitere Informationen finden Sie unter [integrierte Windows PowerShell gewünschten Status Konfigurationsressourcen](builtInResource.md).) Dieses Thema enthält einen Überblick über die Entwicklung von Ressourcen und Links zu Themen mit bestimmten Informationen und Beispiele.

##DSC Komponenten

Eine DSC-Ressource ist ein Windows PowerShell-Modul. Das Modul enthält das Schema (die Definition der konfigurierbaren Eigenschaften) und die Implementierung (der Code, der die eigentliche von einer Konfiguration angegeben Arbeit) für die Ressource. Ein Schema der DSC-Ressource kann in eine MOF-Datei definiert werden, und die Implementierung erfolgt über ein Skriptmodul. Beginnen mit der Unterstützung von PowerShell-Klassen in Version 5, können das Schema und die Implementierung in einer Klasse definiert werden. Die folgenden Themen beschreiben ausführlicher DSC-Ressourcen zu erstellen.

* [Schreiben eine benutzerdefinierte DSC-Ressource mit MOF](authoringResourceMOF.md)
* [Implementieren eine Ressource DSC in c#](authoringResourceMofCS.md)
* [Schreiben eine benutzerdefinierte DSC-Ressource mit PowerShell-Klassen](authoringResourceClass.md)
* [Zusammengesetzte Ressourcen: mit einer DSC-Konfiguration als Ressource](authoringResourceComposite.md)
* [Mithilfe des Tools Ressourcen-Designer](authoringResourceMofDesigner.md)



