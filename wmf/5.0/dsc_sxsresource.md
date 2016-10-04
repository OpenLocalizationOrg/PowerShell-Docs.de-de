# Unterstützung der gleichzeitigen Ausführung unterschiedlicher Versionen für DSC-Ressourcen

Module unterschiedlicher Versionen mit DSC-Ressourcen können parallel installiert werden, und DSC-Konfigurationen können eine bestimmte Version der Ressource verwenden, die auf dem System installiert ist.

Weitere Informationen finden Sie unter [Verwenden von Ressourcen mit mehreren Versionen](https://msdn.microsoft.com/powershell/dsc/sxsresource).

## Bekannte Probleme

In dieser Version sind die folgenden Probleme bei paralleler Installation unterschiedlicher Versionen bekannt:

-   Das Verwenden zweier verschiedener Versionen der DSC-Ressourcen innerhalb der gleichen Konfiguration wird nicht unterstützt.



<!--HONumber=Oct16_HO1-->


