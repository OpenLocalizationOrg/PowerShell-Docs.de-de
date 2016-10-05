---
title: Verbesserungen beim Debuggen von DSC-Ressourcen
author: jaimeo
contributor: amitsara
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 33c3fcffdeb281b205ecc48f7cdd470b79e9e068

---


## Verbesserungen beim Debuggen von DSC-Ressourcen

In WMF 5.0 hat der PowerShell-Debugger bisher nicht direkt an der Klassenressourcenmethode (Get/Set/Test) angehalten.
Wir haben dieses Problem behoben, und der Debugger hält nun bei der Klassenressourcenmethode an, ebenso wie bei den MOF-basierten Ressourcenmethoden.



<!--HONumber=Oct16_HO1-->


