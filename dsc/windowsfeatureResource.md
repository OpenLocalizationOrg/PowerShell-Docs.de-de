#DSC-WindowsFeature-Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die **WindowsFeature** Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus, um sicherzustellen, dass die Rollen und Features hinzugefügt oder werden, auf dem Zielknoten entfernt.

##Syntax

```
WindowsFeature [string] #ResourceName
{
    Name = [string]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ IncludeAllSubFeature = [bool] ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    [ Source = [string] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Name| Gibt der Namen der Rolle oder des Features, die Sie sicherstellen möchten, hinzugefügt oder entfernt wird.Dies ist identisch mit der __Name__ Eigenschaft aus der [Get-WindowsFeature](https://technet.microsoft.com/en-us/library/jj205469.aspx) -Cmdlets und nicht den Anzeigenamen, der die Rolle oder das Feature.|
| Credential| Gibt die Anmeldeinformationen zum Hinzufügen oder Entfernen der Rolle oder Feature an.|
| Stellen Sie sicher| Gibt an, ob die Rolle oder das Feature hinzugefügt wird.Um sicherzustellen, dass die Rolle oder das Feature hinzugefügt, legen dieser Eigenschaft auf "Present", um sicherzustellen, dass die Rolle oder das Feature entfernt wird, legen Sie die Eigenschaft auf "Abwesend".|
| IncludeAllSubFeature| Legen Sie diese Eigenschaft auf __$true__ um den Status aller erforderlichen Features mit dem Status des Features sicherzustellen, geben Sie mit, den __Namen__ Eigenschaft.|
| LogPath| Gibt den Pfad zu einer Protokolldatei Ressourcenanbieters des Vorgangs protokolliert werden soll.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|
| Datenquelle| Gibt den Speicherort der Quelldatei für die Installation verwendet, bei Bedarf.|

##Beispiel

```powershell
WindowsFeature RoleExample
{
    Ensure = "Present" 
    # Alternatively, to ensure the role is uninstalled, set Ensure to "Absent"
    Name = "Web-Server" # Use the Name property from Get-WindowsFeature  
}
```





