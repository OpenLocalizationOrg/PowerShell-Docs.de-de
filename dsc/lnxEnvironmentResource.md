#DSC für Linux NxEnvironment Ressource

Die **NxEnvironment** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus, um die Systemumgebungsvariablen auf einem Linux-Knoten zu verwalten.

##Syntax

```
nxEnvironment <string> #ResourceName
{
    Name = <string>
    [ Value = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ Path = <bool> }
    [ DependsOn = <string[]> ]

}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Name| Gibt den Namen der Umgebungsvariablen, die einen bestimmten Zustand sichergestellt werden soll.|
| Wert| Der Wert der Umgebungsvariable zugewiesen.|
| Stellen Sie sicher| Bestimmt, ob zum Überprüfen, ob die Variable vorhanden ist.Legen Sie diese Eigenschaft auf "Present", um sicherzustellen, dass die Variable vorhanden ist.Bei Einstellung auf "Abwesend", um sicherzustellen, dass die Variable nicht vorhanden ist.Der Standardwert ist "Present".|
| Pfad| Definiert die Umgebungsvariable, die konfiguriert wird.Legen Sie diese Eigenschaft auf **$true** wenn die Variable ist die **Pfad** andernfalls legen Sie ihn auf **$false**.Der Standardwert ist **$false**.Ist die Variable konfiguriert wird die **Pfad** Variable der Wert zur Verfügung gestellt, über die **Wert** Eigenschaft an den vorhandenen Wert angefügt.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Weitere Informationen

* Wenn **Pfad** fehlt, oder legen Sie auf **$false**, Umgebungsvariablen in verwaltet `/Etc/Umgebung`. Ihre Programme oder Skripts erfordern Konfiguration zur Quelle der `/Etc/Umgebung` Datei Zugriff auf die Variablen der verwalteten Umgebung.
* Wenn **Pfad** minFreeThreads auf **$true**, wird die Umgebungsvariable in der Datei verwaltet `/etc/profile.d/DSCenvironment.sh`. Diese Datei wird erstellt, wenn er nicht vorhanden ist. Wenn **sicherstellen, dass** festgelegt ist, "Abwesend" und **Pfad** minFreeThreads auf **$true**, wird nur von eine vorhandenen Umgebungsvariablen entfernt `/etc/profile.d/DSCenvironment.sh` und nicht von anderen Dateien.

##Beispiel

Das folgende Beispiel zeigt, wie Sie die **NxEnvironment** Ressource, um sicherzustellen, dass **TestEnvironmentVariable** vorhanden ist, und hat den Wert "Testwert". Wenn **TestEnvironmentVariable** ist nicht vorhanden ist, wird es erstellt.

```
Import-DSCResource -Module nx 


nxEnvironment EnvironmentExample
{
    Ensure = “Present”
    Name = “TestEnvironmentVariable”
    Value = “TestValue”
}
```






