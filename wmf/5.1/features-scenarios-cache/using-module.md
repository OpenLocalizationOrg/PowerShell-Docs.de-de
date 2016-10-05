---
title: "„FullyQualifiedModuleName“ für „using module“"
author: jaimeo
contributor: vors
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: e09cfe0994ac523fd10658955731a93b6c176c88

---

„FullyQualifiedModuleName“ für „using module“
=========================

`using module` verhält sich jetzt genauso wie andere modulbezogene Konstruktionen in PowerShell.

WMF 5.0-Probleme
----------

* Benutzer hat keine Möglichkeit zur Angabe der Modulversion in `using module`.
* Wenn auf dem System mehrere Versionen des Moduls verfügbar sind, erhält der Benutzer eine Fehlermeldung.

WMF 5.1
----------

* Benutzer können die `ModuleSpecification` [Hashtabelle](https://msdn.microsoft.com/en-us/library/jj136290(v=vs.85).aspx) verwenden. Diese Hashtable weist das gleiche Format wie `Get-Module -FullyQualifiedName`

**Beispiel:** `using module @{ModuleName = 'PSReadLine'; RequiredVersion = '1.1'}`

* Wenn mehrere Versionen des Moduls vorhanden sind, verwendet PowerShell **dieselbe Auflösungslogik** wie `Import-Module` und gibt keinen Fehler zurück.

* Dadurch entspricht das Verhalten von `using module` dem von `Import-Module` und `Import-DscResource`.



<!--HONumber=Oct16_HO1-->


