#DSC für Linux NxSshAuthorizedKeys Ressource

Die **NxAuthorizedKeys** Ressource in PowerShell gewünscht State Configuration (DSC) bietet ein Mechanismus zum Verwalten von autorisierten ssh Keys für einen angegebenen Benutzer.

##Syntax

```
nxAuthorizedKeys <string> #ResourceName
{
    KeyComment = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ Username = <string> ]
    [ Key = <string> ]
    [ DependsOn = <string[]> ]

}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| KeyComment| Ein eindeutiger Kommentar für den Schlüssel.Dies wird verwendet, um Schlüssel eindeutig zu identifizieren.|
| Stellen Sie sicher| Gibt an, ob der Schlüssel definiert ist.Legen Sie diese Eigenschaft auf "Abwesend", um sicherzustellen, dass der Schlüssel nicht in der Benutzerdatei autorisierten Schlüssel vorhanden ist.Bei Einstellung auf "Present", um sicherzustellen, dass der Schlüssel des Benutzers autorisierten Schlüsseldatei definiert ist.|
| Benutzername| Der Benutzername ssh verwalten autorisiert Schlüssel für.Wenn nicht definiert, ist der Standardbenutzer "root".|
| Schlüssel| Der Inhalt des Schlüssels.Dies ist erforderlich, wenn **sicherstellen, dass** auf "Vorhanden" festgelegt ist.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Beispiel

Das folgende Beispiel definiert eine öffentliche ssh autorisiert Schlüssel für den Benutzer "Monuser".

```
Import-DSCResource -Module nx 

Node $node {

nxSshAuthorizedKeys myKey{
   KeyComment = "myKey"
   Ensure = "Present"
   Key = 'ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEA0b+0xSd07QXRifm3FXj7Pn/DblA6QI5VAkDm6OivFzj3U6qGD1VJ6AAxWPCyMl/qhtpRtxZJDu/TxD8AyZNgc8aN2CljN1hOMbBRvH2q5QPf/nCnnJRaGsrxIqZjyZdYo9ZEEzjZUuMDM5HI1LA9B99k/K6PK2Bc1NLivpu7nbtVG2tLOQs+GefsnHuetsRMwo/+c3LtwYm9M0XfkGjYVCLO4CoFuSQpvX6AB3TedUy6NZ0iuxC0kRGg1rIQTwSRcw+McLhslF0drs33fw6tYdzlLBnnzimShMuiDWiT37WqCRovRGYrGCaEFGTG2e0CN8Co8nryXkyWc6NSDNpMzw== rsa-key-20150401'
   UserName = "monuser"
} 
}
```





