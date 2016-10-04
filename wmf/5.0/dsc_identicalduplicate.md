# Zulassen identischer doppelter Ressourcen in einer Konfiguration

DSC lässt keine in Konflikt stehenden Ressourcendefinitionen in einer Konfiguration zu bzw. behandeln diese nicht. Anstatt zu versuchen, den Konflikt zu lösen, tritt ein Fehler auf. Sobald die Wiederverwendung von Konfigurationen mittels zusammengesetzter Ressourcen häufiger vorkommt, werden Konflikte häufiger auftreten. Wenn in Konflikt stehende Ressourcendefinitionen identisch sind, sollte DSC diese intelligent zulassen. In dieser Version unterstützen wir mehrere Ressourceninstanzen, die identische Definitionen aufweisen:

```powershell
Configuration IIS_FrontEnd
{
    WindowsFeature FE_IIS         #Identical resource
    {
        Ensure = 'Present'
        Name = 'Web-Server'
    }

    WindowsFeature FTP
    {
        Ensure = 'Present'
        Name = 'Web-FTP-Server'
    }
}

Configuration IIS_Worker
{
    WindowsFeature Worker_IIS      #Identical resource
    {
        Ensure = 'Present'
        Name = 'Web-Server'
    }

    WindowsFeature ASP
    {
        Ensure = 'Present'
        Name = 'Web-ASP-Net45'
    }
}

Configuration WebApplication
{
    IIS_Frontend Web {}

    IIS_Worker ASP {}
}
```

In früheren Versionen wäre das Ergebnis eine fehlerhafte Kompilierung aufgrund eines Konflikts zwischen der „WindowsFeature FE_IIS“-Instanz und der „WindowsFeature Worker_IIS“-Instanz beim Versuch sicherzustellen, dass die Roll „Web-Server“ installiert ist. Beachten Sie, dass *alle* Eigenschaften, die konfiguriert werden, in diesen beiden Konfigurationen identisch sind. Da *alle* Eigenschaften in diesen beiden Ressourcen identisch sind, führt dies jetzt zu einer erfolgreichen Kompilierung. 

Wenn Eigenschaften in beiden Ressourcen unterschiedlich sind, werden sie nicht als identisch eingestuft, weshalb die Kompilierung misslingt:

```powershell
Configuration IIS_FrontEnd
{
    WindowsFeature FE_IIS
    {
        Ensure = 'Present'     # Ensure is Present here
        Name = 'Web-Server'
    }

    WindowsFeature FTP
    {
        Ensure = 'Present'
        Name = 'Web-FTP-Server'
    }
}

Configuration IIS_Worker
{
    WindowsFeature Worker_IIS
    {
        Ensure = 'Absent'      # Ensure property is Absent instead of Present
        Name = 'Web-Server'
    }

    WindowsFeature ASP
    {
        Ensure = 'Present'
        Name = 'Web-ASP-Net45'
    }
}

Configuration WebApplication
{
    IIS_Frontend Web {}

    IIS_Worker ASP {}
}
```

Diese sehr ähnliche Konfiguration ist nicht erfolgreich, da die Ressourcen „WindowsFeature FE_IIS“ und „WindowsFeature Worker_IIS“ nicht mehr identisch sind und daher in Konflikt stehen.

<!--HONumber=Oct16_HO1-->


