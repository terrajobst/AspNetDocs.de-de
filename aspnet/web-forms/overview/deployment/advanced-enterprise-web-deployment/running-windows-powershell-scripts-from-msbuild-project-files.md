---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Ausführen von Windows PowerShell-Skripts aus MSBuild-Projektdateien | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Build-und Bereitstellungs Prozesses ausgeführt wird. Sie können ein Skript lokal ausführen (d. h. auf der b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 7b09c07b8b7c2a61ca534f7a66a929593f3d04ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441447"
---
# <a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Ausführen von Windows PowerShell-Skripts aus MSBuild-Projektdateien

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Build-und Bereitstellungs Prozesses ausgeführt wird. Sie können ein Skript lokal ausführen (d. h. auf dem Buildserver) oder Remote, z. b. auf einem Zielweb Server oder Datenbankserver.
> 
> Es gibt viele Gründe, warum Sie möglicherweise ein Windows PowerShell-Skript nach der Bereitstellung ausführen möchten. Auf diese Weise können Sie beispielsweise folgende Vorgänge durchführen:
> 
> - Fügen Sie der Registrierung eine benutzerdefinierte Ereignis Quelle hinzu.
> - Generieren Sie ein Dateisystem Verzeichnis für Uploads.
> - Buildverzeichnisse bereinigen.
> - Schreiben Sie Einträge in eine benutzerdefinierte Protokolldatei.
> - Senden von e-Mails, die Benutzer zu einer neu bereitgestellten Webanwendung einladen.
> - Erstellen Sie Benutzerkonten mit den entsprechenden Berechtigungen.
> - Konfigurieren Sie die Replikation zwischen SQL Server Instanzen.
> 
> In diesem Thema erfahren Sie, wie Sie Windows PowerShell-Skripts sowohl lokal als auch Remote über ein benutzerdefiniertes Ziel in einer Microsoft-Build-Engine-Projektdatei (MSBuild) ausführen.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="task-overview"></a>Aufgaben Übersicht

Wenn Sie ein Windows PowerShell-Skript als Teil eines automatisierten oder Einzel stufigen Bereitstellungs Prozesses ausführen möchten, müssen Sie diese Aufgaben auf hoher Ebene ausführen:

- Fügen Sie das Windows PowerShell-Skript zur-Projekt Mappe und zur Quell Code Verwaltung hinzu.
- Erstellen Sie einen Befehl, mit dem das Windows PowerShell-Skript aufgerufen wird.
- Entziehen Sie alle reservierten XML-Zeichen in Ihrem Befehl.
- Erstellen Sie ein Ziel in Ihrer benutzerdefinierten MSBuild-Projektdatei, und verwenden Sie die **exec** -Aufgabe zum Ausführen des Befehls.

In diesem Thema wird gezeigt, wie Sie diese Prozeduren ausführen. In den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits mit MSBuild-Zielen und-Eigenschaften vertraut sind und wissen, wie Sie eine benutzerdefinierte MSBuild-Projektdatei verwenden, um einen Build-und Bereitstellungs Prozess zu fördern. Weitere Informationen finden Sie Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Erstellen und Hinzufügen von Windows PowerShell-Skripts

In den Tasks in diesem Thema wird ein Windows PowerShell-Beispielskript namens " **logbereitstellungs. ps1** " verwendet, um zu veranschaulichen, wie Skripts aus MSBuild ausgeführt werden Das Skript **logbereitstellungs. ps1** enthält eine einfache Funktion, die einen einzeiligen Eintrag in eine Protokolldatei schreibt:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]

Das Skript " **logbereitstellungs. ps1** " akzeptiert zwei Parameter. Der erste Parameter stellt den vollständigen Pfad zu der Protokolldatei dar, der Sie einen Eintrag hinzufügen möchten, und der zweite Parameter stellt das Bereitstellungs Ziel dar, das Sie in der Protokolldatei aufzeichnen möchten. Wenn Sie das Skript ausführen, wird der Protokolldatei eine Zeile im folgenden Format hinzugefügt:

[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]

Um das Skript **logbereitstellungs. ps1** für MSBuild verfügbar zu machen, müssen Sie folgende Schritte ausführen:

- Fügen Sie das Skript der Quell Code Verwaltung hinzu.
- Fügen Sie das Skript der Projekt Mappe in Visual Studio 2010 hinzu.

Sie müssen das Skript nicht mit ihrem Lösungs Inhalt bereitstellen, unabhängig davon, ob Sie das Skript auf dem Buildserver oder einem Remote Computer ausführen möchten. Eine Möglichkeit besteht darin, das Skript einem Projektmappenordner hinzuzufügen. Wenn Sie das Windows PowerShell-Skript im Rahmen des Bereitstellungs Prozesses verwenden möchten, ist es sinnvoll, das Skript dem Projektmappenordner veröffentlichen hinzuzufügen.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Der Inhalt von Projektmappenordnern wird als Quellmaterial auf Buildserver kopiert. Allerdings bilden Sie keinen Teil einer Projekt Ausgabe.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Ausführen eines Windows PowerShell-Skripts auf dem Buildserver

In einigen Szenarien möchten Sie möglicherweise Windows PowerShell-Skripts auf dem Computer ausführen, auf dem Ihre Projekte erstellt werden. Beispielsweise können Sie ein Windows PowerShell-Skript verwenden, um Buildordner zu bereinigen oder Einträge in eine benutzerdefinierte Protokolldatei zu schreiben.

Im Hinblick auf die Syntax entspricht das Ausführen eines Windows PowerShell-Skripts aus einer MSBuild-Projektdatei dem Ausführen eines Windows PowerShell-Skripts über eine reguläre Eingabeaufforderung. Sie müssen die ausführbare Datei "PowerShell. exe" aufrufen und den **Befehl "– Command** " verwenden, um die Befehle bereitzustellen, die von Windows PowerShell ausgeführt werden sollen. (In Windows PowerShell v2 können Sie auch den Schalter **– File** ) verwenden. Der Befehl sollte das folgende Format aufweisen:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]

Beispiel:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]

Wenn der Pfad zu Ihrem Skript Leerzeichen enthält, müssen Sie den Dateipfad in einfache Anführungszeichen einschließen, denen ein kaufmännisches und-Zeichen vorangestellt ist. Doppelte Anführungszeichen können nicht verwendet werden, da Sie Sie bereits zum Einschließen des Befehls verwendet haben:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]

Wenn Sie diesen Befehl in MSBuild aufrufen, müssen Sie einige weitere Überlegungen beachten. Zuerst sollten Sie das **– noninteractive** -Flag einschließen, um sicherzustellen, dass das Skript unbeaufsichtigt ausgeführt wird. Als nächstes sollten Sie das Flag **– ExecutionPolicy** mit einem entsprechenden Argument Wert einschließen. Dadurch wird die Ausführungs Richtlinie angegeben, die von Windows PowerShell auf das Skript angewendet wird, und Sie können die Standard Ausführungs Richtlinie außer Kraft setzen, was die Ausführung des Skripts verhindern kann. Sie können aus den folgenden Argument Werten wählen:

- Der Wert **uneingeschränkt** ermöglicht Windows PowerShell das Ausführen des Skripts, unabhängig davon, ob das Skript signiert ist.
- Der Wert **RemoteSigned** ermöglicht Windows PowerShell, nicht signierte Skripts auszuführen, die auf dem lokalen Computer erstellt wurden. Allerdings müssen Skripts, die an anderer Stelle erstellt wurden, signiert werden. (In der Praxis ist es sehr unwahrscheinlich, dass Sie ein Windows PowerShell-Skript lokal auf einem Buildserver erstellt haben).
- Der Wert **AllSigned** ermöglicht es Windows PowerShell, nur signierte Skripts auszuführen.

Die Standard Ausführungs Richtlinie ist **eingeschränkt**, wodurch verhindert wird, dass Windows PowerShell Skriptdateien ausführen kann.

Schließlich müssen Sie alle reservierten XML-Zeichen, die in Ihrem Windows PowerShell-Befehl auftreten, mit Escapezeichen versehen:

- Einfache Anführungszeichen durch **&amp;apos** ersetzen
- Doppelte Anführungszeichen durch **&amp;quot;** ersetzen
- Ersetzen Sie kaufmännische und durch **&amp;amp;**

- Wenn Sie diese Änderungen vornehmen, ähnelt der Befehl folgendem:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]

In Ihrer benutzerdefinierten MSBuild-Projektdatei können Sie ein neues Ziel erstellen und mithilfe der **exec** -Aufgabe den folgenden Befehl ausführen:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]

Beachten Sie in diesem Beispiel Folgendes:

- Alle Variablen, wie Parameterwerte und der Speicherort der ausführbaren Windows PowerShell-Datei, werden als MSBuild-Eigenschaften deklariert.
- Bedingungen sind eingeschlossen, damit Benutzer diese Werte über die Befehlszeile überschreiben können.
- Die **msdeploycomputername** -Eigenschaft wird an anderer Stelle in der Projektdatei deklariert.

Wenn Sie dieses Ziel als Teil des Buildprozesses ausführen, führt Windows PowerShell den Befehl aus und schreibt einen Protokolleintrag in die von Ihnen angegebene Datei.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Ausführen eines Windows PowerShell-Skripts auf einem Remote Computer

Windows PowerShell ist in der Lage, Skripts auf Remote Computern über [Windows-Remoteverwaltung](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM) auszuführen. Zu diesem Zweck müssen Sie das Cmdlet "Start [-Command](https://technet.microsoft.com/library/dd347578.aspx) " verwenden. Auf diese Weise können Sie das Skript auf einem oder mehreren Remote Computern ausführen, ohne das Skript auf die Remote Computer zu kopieren. Alle Ergebnisse werden an den lokalen Computer zurückgegeben, von dem aus Sie das Skript ausgeführt haben.

> [!NOTE]
> Bevor Sie das Cmdlet " **Aufruf-Command** " zum Ausführen von Windows PowerShell-Skripts auf einem Remote Computer verwenden, müssen Sie einen WinRM-Listener für die Annahme von Remote Nachrichten konfigurieren. Führen Sie hierzu den Befehl **WinRM Quick Config** auf dem Remote Computer aus. Weitere Informationen finden Sie unter [Installation und Konfiguration für Windows-Remoteverwaltung](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).

In einem Windows PowerShell-Fenster verwenden Sie diese Syntax zum Ausführen des Skripts **logbereitstellung. ps1** auf einem Remote Computer:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]

> [!NOTE]
> Es gibt verschiedene andere Möglichkeiten der Verwendung von " **aufrufen-Command** " zum Ausführen einer Skriptdatei. dieser Ansatz ist jedoch die einfachste Methode, wenn Sie Parameterwerte bereitstellen und Pfade mit Leerzeichen verwalten müssen.

Wenn Sie diesen Befehl über eine Eingabeaufforderung ausführen, müssen Sie die ausführbare Windows PowerShell-Datei aufrufen und den **–-Befehls** Parameter verwenden, um Ihre Anweisungen anzugeben:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]

Wie zuvor müssen Sie einige zusätzliche Switches angeben und alle reservierten XML-Zeichen mit Escapezeichen versehen, wenn Sie den Befehl in MSBuild ausführen:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]

Zum Schluss können Sie wie zuvor die **exec** -Aufgabe in einem benutzerdefinierten MSBuild-Ziel verwenden, um den Befehl auszuführen:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]

Wenn Sie dieses Ziel als Teil des Buildprozesses ausführen, führt Windows PowerShell das Skript auf dem Computer aus, den Sie im **– Computername** -Argument angegeben haben.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript aus einer MSBuild-Projektdatei ausgeführt wird. Sie können diesen Ansatz verwenden, um ein Windows PowerShell-Skript entweder lokal oder auf einem Remote Computer im Rahmen eines automatisierten oder einstufigen Build-und Bereitstellungs Prozesses auszuführen.

## <a name="further-reading"></a>Weitere nützliche Informationen

Anleitungen zum Signieren von Windows PowerShell-Skripts und Verwalten von Ausführungsrichtlinien finden Sie unter [Ausführen von Windows PowerShell-Skripts](https://technet.microsoft.com/library/ee176949.aspx). Anleitungen zum Ausführen von Windows PowerShell-Befehlen von einem Remote Computer finden Sie unter [Ausführen von Remote Befehlen](https://technet.microsoft.com/library/dd819505.aspx).

Weitere Informationen zur Verwendung von benutzerdefinierten MSBuild-Projektdateien zum Steuern des Bereitstellungs Prozesses finden Sie Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Zurück](taking-web-applications-offline-with-web-deploy.md)
> [Weiter](troubleshooting-the-packaging-process.md)
