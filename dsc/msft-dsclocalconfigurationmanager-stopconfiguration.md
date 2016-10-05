---
title: StopConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 365ae776cfade1c9b63dfed56f922fd9919f89fd

---

# StopConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse

Beende die Konfigurationsänderung, die gerade ausgeführt wird.

Syntax
------

```mof
uint32 StopConfiguration(
  [in] boolean force
);
```

Parameter
----------

*force* \[in\]  
**true**, um das Beenden der Konfiguration zu erzwingen.

## Rückgabewert
------------

Gibt bei Erfolg null zurück, andernfalls einen Fehlercode.

## Hinweise

Dies ist eine statische Methode.

## Anforderungen
------------
>**MOF:** DscCore.mof

>**Namespace**: Root\Microsoft\Windows\DesiredStateConfiguration


## Siehe auch


[**MSFT_DSCLocalConfigurationManager**](msft-dsclocalconfigurationmanager.md)


 

 






<!--HONumber=Oct16_HO1-->


