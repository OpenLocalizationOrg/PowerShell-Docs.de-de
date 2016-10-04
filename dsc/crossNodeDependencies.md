---
title: "Angeben knotenübergreifender Abhängigkeiten"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: f4c64d3603fd01f29a63f8b180057714bf3d1197

---

# Angeben knotenübergreifender Abhängigkeiten

> Gilt für: Windows PowerShell 5.0

DSC bietet spezielle Ressourcen **WaitForAll**, **WaitForAny** und **WaitForSome**, die in Konfigurationen zum Angeben von Abhängigkeiten von Konfigurationen auf anderen Knoten verwendet werden können. Die Ressourcen verhalten sich wie folgt:

* **WaitForAll**: Ist erfolgreich, wenn die angegebene Ressource auf allen Zielknoten, die in der **NodeName**-Eigenschaft definiert sind, den gewünschten Zustand hat.
* **WaitForAny**: Ist erfolgreich, wenn die angegebene Ressource auf mindestens einem der Zielknoten, die in der **NodeName**-Eigenschaft definiert sind, den gewünschten Zustand hat.
* **WaitForSome**: Gibt zusätzlich zu einer **NodeName**-Eigenschaft eine **NodeCount**-Eigenschaft an. Die Ressource ist erfolgreich, wenn sich die Ressource auf einer Mindestanzahl von Knoten (angegeben durch **NodeCount**), die von der **NodeName**-Eigenschaft definiert sind, im gewünschten Zustand befindet. 

## Verwenden von WaitForXXXX-Ressourcen

Um die **WaitForXXXX**-Ressourcen zu verwenden, erstellen Sie einen Ressourcenblock des Ressourcentyps, der die DSC-Ressource und Knoten angibt, auf die gewartet werden soll. Dann verwenden Sie die **DependsOn**-Eigenschaft in allen anderen Ressourcenblöcken Ihrer Konfiguration und warten darauf, dass die im **WaitForXXXX**-Knoten angegebenen Bedingungen erfolgreich erfüllt werden.

Bei der folgenden Konfiguration wartet der Zielknoten beispielsweise, bis die Ressource **xADDomain** auf dem Knoten **MyDC** (bei einer maximalen Anzahl von 30 Wiederholungen in 15-Sekunden-Intervallen) abgeschlossen ist, ehe der Zielknoten der Domäne beitreten kann.

```PowerShell
Configuration JoinDomain

{
    Import-DscResource -Module xComputerManagement

    Node myDomainJoinedServer
    {

        WaitForAll DC
        {
            ResourceName      = '[xADDomain]NewDomain'
            NodeName          = 'MyDC'
            RetryIntervalSec  = 15
            RetryCount        = 30
        }

        xComputer JoinDomain
        {
            Name             = 'MyPC'
            DomainName       = 'Contoso.com'
            Credential       = (Get-Credential)
            DependsOn        ='[WaitForAll]DC'
        }
    }
}
```

>**Hinweis:** Standardmäßig führen die „WaitForXXXX“-Ressourcen einen Versuch durch und verursachen dann einen Fehler. Auch wenn dies nicht erforderlich ist, sollten Sie doch in der Regel ein Wiederholungsintervall sowie eine Anzahl von Wiederholungen angeben.

## Weitere Informationen
* [DSC-Konfigurationen](configurations.md)
* [DSC-Ressourcen](resources.md)
* [Konfigurieren des lokalen Konfigurations-Managers](metaConfig.md)




<!--HONumber=Oct16_HO1-->


