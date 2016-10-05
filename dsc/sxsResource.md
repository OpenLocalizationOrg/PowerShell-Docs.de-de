---
title: Verwenden von Ressourcen mit mehreren Versionen
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 716ecd9b14976dd70b69a740850ab53670387956

---

# Verwenden von Ressourcen mit mehreren Versionen

> Gilt für: Windows PowerShell 5.0

In PowerShell 5.0 können DSC-Ressourcen mehrere Versionen haben, und verschiedene Versionen können parallel auf einem Computer installiert sein. Dies wird dadurch implementiert, dass mehrere Versionen eines Ressourcenmoduls vorhanden sind, die sich im selben Modulordner befinden.

## Paralleles Installieren mehrerer Ressourcenversionen

Sie können die Parameter **MinimumVersion**, **MaximumVersion** und **RequiredVersion** des Cmdlets [Install-Module](https://technet.microsoft.com/en-us/library/dn807162.aspx) verwenden, um anzugeben, welche Version eines Moduls installiert werden soll. Wenn Sie **Install-Module** ohne Angabe eine Version aufrufen, wird die neueste Version installiert.

So gibt es beispielsweise mehrere Versionen des **xFailOverCluster**-Moduls, von denen jede eine **xCluster**-Ressource enthält. Das Ergebnis des Aufrufs von **Install-Module** ohne Angabe der Versionsnummer sieht wie folgt aus:

```powershell
C:\Program Files\WindowsPowerShell\Modules\xFailOverCluster> Install-Module xFailOverCluster
C:\Program Files\WindowsPowerShell\Modules\xFailOverCluster> Get-DscResource xCluster

ImplementedAs   Name                      ModuleName                     Version    Properties
-------------   ----                      ----------                     -------    ----------
PowerShell      xCluster                  xFailOverCluster               1.2.0.0    {DomainAdministratorCredential, ...
```

Wenn Sie nun **Install-Module** erneut aufrufen, dabei aber eine **RequiredVersion** von „1.1.0.0“ angeben, erhalten Sie Folgendes:

```powershell
C:\Program Files\WindowsPowerShell\Modules\xFailOverCluster> Install-Module xFailOverCluster -RequiredVersion 1.1
C:\Program Files\WindowsPowerShell\Modules\xFailOverCluster> Get-DscResource xCluster

ImplementedAs   Name                      ModuleName                     Version    Properties
-------------   ----                      ----------                     -------    ----------
PowerShell      xCluster                  xFailOverCluster               1.1        {DomainAdministratorCredential, Name, ...
PowerShell      xCluster                  xFailOverCluster               1.2.0.0    {DomainAdministratorCredential, Name, ...
```

## Angeben einer Ressourcenversion in einer Konfiguration

Wenn Sie mehrere Ressourcen auf einem Computer installiert haben, müssen Sie die Version dieser Ressource angeben, wenn Sie sie in einer Konfiguration verwenden. Hierzu geben Sie den Parameter **ModuleVersion** des Schlüsselworts **Import-DscResource** an. Wenn Sie bei einer Ressource, von der Sie mehr als eine Version installiert haben, keine Version des Ressourcenmoduls angeben, generiert die Konfiguration einen Fehler.

Die folgende Konfiguration zeigt, wie Sie die Version der aufzurufenden Ressource angeben:

```powershell
configuration VersionTest
{
    Import-DscResource -ModuleName xFailOverCluster -ModuleVersion 1.1

    Node 'localhost'
    {
       xCluster ClusterTest
       {
            Name                          = 'TestCluster'
            StaticIPAddress               = '10.0.0.3'
            DomainAdministratorCredential = Get-Credential
        }
     }
}     
```

>Hinweis: Der Parameter „ModuleVersion“ von „Import-DscResource“ ist in PowerShell 4.0 nicht verfügbar. In PowerShell 4.0 können Sie eine Modulversion angeben, indem Sie ein Modulspezifikationsobjekt an den Parameter „ModuleName“ von „Import-DscResource“ übergeben. Ein Modulspezifikationsobjekt ist eine Hashtabelle, die die Schlüssel „ModuleName“ und „RequiredVersion“ enthält. Beispiel:

```powershell
configuration VersionTest
{
    Import-DscResource -ModuleName (@{ModuleName='xFailOverCluster'; RequiredVersion='1.1'} )

    Node 'localhost'
    {
       xCluster ClusterTest
       {
            Name                          = 'TestCluster'
            StaticIPAddress               = '10.0.0.3'
            DomainAdministratorCredential = Get-Credential
        }
     }
}     
```

Dies funktioniert auch in PowerShell 5.0, aber es wird empfohlen, dass Sie den Parameter **ModuleVersion** verwenden.

## Siehe auch
* [DSC-Konfigurationen](configurations.md)
* [DSC-Ressourcen](resources.md)




<!--HONumber=Oct16_HO1-->


