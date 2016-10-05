# Logik für die Vorbereitung der modulabhängigkeiten während der Veröffentlichungsvorgang
1.  Module, die als Teil von RequiredModules werden als Abhängigkeiten betrachtet.
2.  Module, die als Teil von NestedModules, dessen Grundlinie Modul nicht unter dem angegebenen Modul-Basis ist, werden als Abhängigkeiten betrachtet.

3.  Über Abhängigkeiten auf dasselbe Zielrepository verfügbar sein soll, andernfalls veröffentlichen Vorgang führt zu einem Fehler.
    Zum Beispiel: 'SnippetPx' im Repository nicht verfügbar ist, folgenden Fehler ausgelöst.
    ```powershell
    Publish-PSArtifactUtility : PowerShellGet cannot resolve the module dependency 'SnippetPx' of the module 'TypePx' on the repository 'LocalRepo'. Verify that the dependent module 'SnippetPx' is available in the repository 'LocalRepo'. If this dependent
    'SnippetPx' is managed externally, add it to the ExternalModuleDependencies entry in the PSData section of the module manifest.
    ```
4.  Einige modulabhängigkeiten extern verwaltet werden können, in diesem Fall sollten sie zu der ExternalModuleDependencies-Eintrag im Abschnitt PSData des modulmanifests hinzugefügt werden.
    Im folgenden Teil in der obigen Fehlermeldung
    ```powershell
    If this dependent 'SnippetPx' is managed externally, add it to the ExternalModuleDependencies entry in the PSData section of the module manifest.
    ```

*Während der Modulinstallation über vorbereiteten Abhängigkeiten Liste dient zur Installation von Abhängigkeiten.*

*Stellen Sie sicher, dass Ihr Modul Abhängigkeiten unter $env verfügbar sind: PSModulePath auf Ihrem System während des Veröffentlichungsvorgangs.*


<!--HONumber=Oct16_HO1-->


