#DSC für Linux NxGroup Ressource

Die **NxGroup** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten lokaler Gruppen auf einem Linux-Knoten.

##Syntax

```powershell
nxGroup <string> #ResourceName
{
    GroupName = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ Members = <string[]> ]
    [ MebersToInclude = <string[]>]
    [ MembersToExclude = <string[]> ]
    [ DependsOn = <string[]> ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Gruppenname| Gibt den Namen der Gruppe, die einen bestimmten Zustand sichergestellt werden soll.|
| Stellen Sie sicher| Bestimmt, ob zum Überprüfen, ob die Gruppe vorhanden ist.Legen Sie diese Eigenschaft auf "Present", um sicherzustellen, dass die Gruppe vorhanden ist.Bei Einstellung auf "Abwesend", um sicherzustellen, dass die Gruppe nicht vorhanden ist.Der Standardwert ist "Present".|
| Mitglieder| Gibt die Elemente, die die Gruppe zu bilden.|
| MembersToInclude| Gibt an, um sicherzustellen, dass Benutzer Mitglieder der Gruppe sind.|
| MembersToExclude| Gibt an, um sicherzustellen, dass Benutzer nicht Mitglied der Gruppe sind.|
| PreferredGroupID| Legt die Gruppen-Id nach Möglichkeit auf den bereitgestellten Wert fest.Wenn die Gruppen-Id derzeit verwendet wird, wird die nächste verfügbare Id verwendet.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Beispiel

Im folgende Beispiel wird sichergestellt, dass der Benutzer "Monuser" vorhanden ist und ein der Gruppe "DBusers Mitglied".

```
Import-DSCResource -Module nx 

Node $node {

nxUser UserExample{
   UserName = "monuser"
   Description = "Monitoring user"
   Password  =    '$6$fZAne/Qc$MZejMrOxDK0ogv9SLiBP5J5qZFBvXLnDu8HY1Oy7ycX.Y3C7mGPUfeQy3A82ev3zIabhDQnj2ayeuGn02CqE/0'
   Ensure = "Present"
   HomeDirectory = "/home/monuser"
}

nxGroup GroupExample{
   GroupName = "DBusers"
   Ensure = "Present"
   MembersToInclude = "monuser"
   DependsOn = "[nxUser]UserExample"            
}
}
```




