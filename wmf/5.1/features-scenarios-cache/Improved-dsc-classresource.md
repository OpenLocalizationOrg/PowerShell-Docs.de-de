---
title: Verbesserungen an DSC-Klassenressourcen
author: jaimeo
contributor: amitsara
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: b24e70c1e1aaf71487b00fbccaf6edb0f375b888

---

## Verbesserungen an DSC-Klassenressourcen

Mit diesem Release wurden die folgenden, bekannten Probleme von WMF 5.0 behoben:
* „Get-DscConfiguration“ gibt möglicherweise leere Werte (NULL) oder Fehler zurück, wenn von der „Get()“-Funktion einer klassenbasierten DSC-Ressource ein komplexer Typ bzw. ein Hashtabellentyp zurückgegeben wird.
* „Get-DscConfiguration“ gibt einen Fehler zurück, wenn die RunAs-Anmeldeinformationen bei der DSC-Konfiguration verwendet werden.
* Eine klassenbasierte Ressource kann nicht in einer zusammengesetzten Konfiguration verwendet werden.
* „Start-DscConfiguration“ reagiert nicht mehr, wenn die klassenbasierte Ressource eine Eigenschaft des eigenen Typs hat.
* Eine klassenbasierte Ressource kann nicht als exklusive Ressource verwendet werden.



<!--HONumber=Oct16_HO1-->


