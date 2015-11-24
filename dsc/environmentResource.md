#DSC Umgebung Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die __Umgebung__ Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten von Umgebungsvariablen des Systems.

##Syntax

``` mof
Environment [string] #ResourceName
{
    Name = [string]
    [ Ensure = [string] { Absent | Present }  ]
    [ Path = [bool] ]
    [ DependsOn = [string[]] ]
    [ Value = [string] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Name| Gibt den Namen der Umgebungsvariablen, die einen bestimmten Zustand sichergestellt werden soll.|
| Stellen Sie sicher| Gibt an, ob eine Variable vorhanden ist.Legen Sie diese Eigenschaft auf __vorhanden__ die Umgebungsvariable zu erstellen, wenn er nicht vorhanden ist, oder um sicherzustellen, dass ihr Wert übereinstimmt, was erfolgt über die __Wert__ Eigenschaft, wenn die Variable bereits vorhanden ist.Legen Sie ihn auf __Abwesend__ an die Variable zu löschen, falls vorhanden.|
| Pfad| Definiert die Umgebungsvariable, die konfiguriert wird.Legen Sie diese Eigenschaft auf __$true__ wenn die Variable ist die __Pfad__ andernfalls legen Sie ihn auf __$false__.Der Standardwert ist __$false__.Ist die Variable konfiguriert wird die __Pfad__ Variable der Wert zur Verfügung gestellt, über die __Wert__ Eigenschaft an den vorhandenen Wert angefügt.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|
| Wert| Der Wert der Umgebungsvariable zugewiesen.|

##Beispiel

Im folgende Beispiel wird sichergestellt, dass __TestEnvironmentVariable__ vorhanden ist und es hat den Wert __Testwert__. Wenn es nicht vorhanden ist, wird er erstellt.

```powershell
Environment EnvironmentExample
{
    Ensure = "Present"  # You can also set Ensure to "Absent"
    Name = "TestEnvironmentVariable"
    Value = "TestValue"
}
```




