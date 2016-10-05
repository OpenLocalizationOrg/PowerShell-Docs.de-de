---
title: "DSC für Linux-Resource „nxScript“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 4c575bbf0e0553e19e56bcc6edd605e36586cb94

---

# DSC für Linux-Resource „nxScript“

Die Ressource **nxScript** in PowerShell DSC bietet einen Mechanismus zum Ausführen von Linux-Skripts auf einem Linux-Knoten.

## Syntax

```
nxScript <string> #ResourceName
{
    GetScript = <string>
    SetScript = <string>
    TestScript = <string>
    [ User = <string> ]
    { Group = <string> ]
    [ DependsOn = <string[]> ]

}
```

## Eigenschaften

|  Eigenschaft |  Beschreibung | 
|---|---|
| GetScript| Bietet ein Skript, das beim Aufrufen des Cmdlets [Get-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521625.aspx) ausgeführt wird. Das Skript muss mit einem Shebang, z. B. „#!/bin/bash“, beginnen.| 
| SetScript| Stellt ein Skript bereit. Beim Aufruf des Cmdlets [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) wird **TestScript** zuerst ausgeführt. Wenn der **TestScript**-Block einen Exitcode ungleich 0 zurückgibt, wird der **SetScript**-Block ausgeführt. Wenn der **TestScript**-Block den Exitcode 0 zurückgibt, wird der **SetScript**-Block nicht ausgeführt. Das Skript muss mit einem Shebang, z. B. `#!/bin/bash`, beginnen.| 
| TestScript| Stellt ein Skript bereit. Beim Aufruf des Cmdlets [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) wird dieses Skript ausgeführt. Wenn es einen Exitcode ungleich 0 zurückgibt, wird der „SetScript“-Block ausgeführt. Wenn es den Exitcode 0 zurückgibt, wird der **SetScript**-Block nicht ausgeführt. Der **TestScript**-Block wird auch ausgeführt, wenn Sie das Cmdlet[Test-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx) aufrufen. In diesem Fall wird der **SetScript**-Block jedoch nicht ausgeführt, ganz gleich, welcher Exitcode vom **TestScript**-Block zurückgegeben wird. Der **TestScript**-Block muss den Exitcode 0 zurückgeben, wenn die tatsächliche Konfiguration der aktuellen Konfiguration des gewünschten Zustands entspricht. Falls nicht, muss ein Exitcode ungleich 0 zurückgegeben werden. (Die aktuelle Konfiguration des gewünschten Zustands ist die letzte Konfiguration, die auf den Knoten angewendet wurde, der DSC verwendet.) Das Skript muss mit einem Shebang, z. B. „1#!/bin/bash.1“, beginnen.| 
| User| Der Benutzer, der das Skript ausführt.| 
| Group| Die Gruppe, die das Skript ausführt.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die **ID** des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, **ResourceName** und dessen Typ **ResourceType** ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 

## Beispiel

Das folgende Beispiel veranschaulicht die Verwendung der Ressource **nxScript**, um zusätzliche Konfigurationsschritte auszuführen.

```
Import-DSCResource -Module nx 

Node $node {
nxScript KeepDirEmpty{

    GetScript = @"
#!/bin/bash
ls /tmp/mydir/ | wc -l
"@

    SetScript = @"
#!/bin/bash
rm -rf /tmp/mydir/*
"@

    TestScript = @'
#!/bin/bash
filecount=`ls /tmp/mydir | wc -l`
if [ $filecount -gt 0 ]
then
    exit 1
else
    exit 0
fi
'@
} 
}
```




<!--HONumber=Oct16_HO1-->


