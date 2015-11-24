#DSC Group-Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die Ressource der Gruppe in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten lokaler Gruppen auf dem Zielknoten.

##Syntax

```
Group [string] #ResourceName
{
    GroupName = [string]
    [ Credential = [PSCredential] ]
    [ Description = [string[]] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ Members = [string[]] ]
    [ MembersToExclude = [string[]] ]
    [ MembersToInclude = [string[]] ]
    [ DependsOn = [string[]] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Gruppenname| Gibt den Namen der Gruppe, die einen bestimmten Zustand sichergestellt werden soll.|
| Credential| Gibt die Anmeldeinformationen auf Remoteressourcen zuzugreifen.**Hinweis**: dieses Konto müssen die entsprechenden Active Directory-Berechtigungen auf alle nicht lokalen Konten zur Gruppe hinzufügen; andernfalls ein Fehler ausgegeben.
| Beschreibung| Gibt die Beschreibung der Gruppe an.|
| Stellen Sie sicher| Gibt an, ob die Gruppe vorhanden ist.Legen Sie diese Eigenschaft auf "Abwesend", um sicherzustellen, dass die Gruppe nicht vorhanden ist.Zu "vorhanden (Standardwert)" wird sichergestellt, dass die Gruppe vorhanden ist.|
| Mitglieder| Gibt an, dass Sie sicherstellen möchten, dass diese Mitglieder die Gruppe zu bilden.|
| MembersToExclude| Gibt an, dass die Benutzer, die sicherstellen soll nicht Mitglieder dieser Gruppe sind.|
| MembersToInclude| Gibt an, dass die Benutzer, die Sie sicherstellen möchten, Mitglied der Gruppe sind.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist "DependsOn ="[ResourceType] ResourceName"".|

##Beispiel

Das folgende Beispiel zeigt, wie Sie sicherstellen, dass eine Gruppe namens TestGroup nicht vorhanden ist.

```powershell
Group GroupExample
{
    # This will remove TestGroup, if present
    # To create a new group, set Ensure to "Present“
    Ensure = "Absent"
    GroupName = "TestGroup"
}
```




