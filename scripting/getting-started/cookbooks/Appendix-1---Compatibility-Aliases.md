---
title: "Anhang 1: Kompatibilitätsaliase"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 96ad921e-1a57-463e-8e60-424faf8b6ef8
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 61eac49bc710a2435117409b74ba02798cf720da

---

# Anhang 1: Kompatibilitätsaliase
Windows PowerShell hat mehrere Übergangsaliase, die es UNIX- und Cmd-Benutzern ermöglichen, vertraut Befehlsnamen in Windows PowerShell zu verwenden. Die am häufigsten verwendeten Aliase sind in der folgenden Tabelle zusammengestellt. Außerdem sind die Windows PowerShell-Befehle, die zu den Aliasen gehören, sowie die standardmäßigen Windows PowerShell-Aliase (sofern vorhanden) aufgeführt.

Sie können den Windows PowerShell-Befehl, auf den ein Alias verweist, in Windows PowerShell mit dem Cmdlet „Get-Alias“ ermitteln. Geben Sie z.B. **get-alias cls** ein.

```
CommandType     Name                            Definition
-----------     ----                            ----------
Alias           cls                             Clear-Host
```

|CMD-Befehl|UNIX-Befehl|PS-Befehl|PS-Alias|
|---------------|----------------|--------------|------------|
|**dir**|**ls**|**Get-ChildItem**|**GCI**|
|**CLS**|**Deaktivieren**|**Clear-Host** (Funktion)|Nicht zutreffend|
|**del "," löschen "," rmdir**|**RM**|**Element entfernen**|**RI**|
|**Kopieren**|**CP**|**Element kopieren**|**CI**|
|**Verschieben**|**MV**|**Move-Item**|**Mi**|
|**Umbenennen**|**MV**|**Rename-Item**|**RNI**|
|**Typ**|**CAT**|**Get-Content**|**GC**|
|**CD**|**CD**|**Set-Location**|**sl**|
|**MD**|**mkdir**|**Neues Element**|**Ni**|
|Nicht zutreffend|**pushd**|**Push-Location**|Nicht zutreffend|
|Nicht zutreffend|**popd**|**POP-Location**|Nicht zutreffend|




<!--HONumber=Oct16_HO1-->


