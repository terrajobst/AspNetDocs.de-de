---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Konfigurieren von Projekteigenschaften-4 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6e63e75dca3d776fb9a1bd7e420ef48891daac69
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569815"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Konfigurieren von Projekteigenschaften-4 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht über

Einige Bereitstellungs Optionen werden in den Projekteigenschaften konfiguriert, die in der Projektdatei ( *csproj* -oder *vbproj* -Datei) gespeichert sind. In den meisten Fällen sind die Standardwerte dieser Einstellungen die gewünschten Werte, aber Sie können die in Visual Studio integrierte **Projekteigenschaften** -Benutzeroberfläche verwenden, um diese Einstellungen zu bearbeiten, wenn Sie Sie ändern müssen. In diesem Tutorial überprüfen Sie die Bereitstellungs Einstellungen in den **Projekteigenschaften**. Außerdem erstellen Sie eine Platzhalter Datei, die bewirkt, dass ein leerer Ordner bereitgestellt wird.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Konfigurieren von Bereitstellungs Einstellungen im Fenster "Projekteigenschaften"

Die meisten Einstellungen, die beeinflussen, was während der Bereitstellung geschieht, sind im Veröffentlichungs Profil enthalten, wie Sie in den folgenden Tutorials sehen werden. Einige Einstellungen, die Sie beachten sollten, befinden sich auf den Registerkarten **Paket/veröffentlichen** im Fenster **Eigenschaften des Projekts** . Diese Einstellungen werden für jede Buildkonfiguration angegeben, d. –. Sie können unterschiedliche Einstellungen für einen Releasebuild haben, als für einen Debugbuild vorhanden sind.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **condesouniversity** , wählen Sie **Eigenschaften**aus, und wählen Sie dann die Registerkarte **Web packen/veröffentlichen** aus.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Wenn das Fenster angezeigt wird, werden standardmäßig Einstellungen für die Buildkonfiguration angezeigt, die derzeit für die Lösung aktiv ist. Wenn im **Konfigurations** Feld nicht **aktiv (Release)** angegeben ist, wählen Sie **Release** aus, um die Einstellungen für die releasebuildkonfiguration anzuzeigen. Sie stellen Releasebuilds sowohl in Ihrer Test-als auch in der Produktionsumgebung bereit.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Wenn **aktiv (Release)** oder **Release** ausgewählt ist, werden die Werte angezeigt, die bei der Bereitstellung mithilfe der releasebuildkonfiguration wirksam sind:

- Im Feld bereit zustellende **Elemente** werden **nur die Dateien ausgewählt, die zum Ausführen der Anwendung erforderlich** sind. Andere Optionen sind **alle Dateien in diesem Projekt** oder **alle Dateien in diesem Projektordner**. Wenn Sie die Standardauswahl unverändert lassen, vermeiden Sie die Bereitstellung von Quell Code Dateien, z. b. Diese Einstellung ist der Grund, warum die Ordner, die die SQL Server Compact Binärdateien enthalten, in das Projekt eingeschlossen werden mussten. Weitere Informationen zu dieser Einstellung finden Sie unter **Warum werden nicht alle Dateien im Projektordner** bereitgestellt? in den [FAQ zu ASP.NET-Webanwendungs-Projekt Bereitstellung](https://msdn.microsoft.com/library/ee942158.aspx).
- **Generierte Debugsymbole ausschließen** ist ausgewählt. Sie werden nicht debuggen, wenn Sie diese Buildkonfiguration verwenden.
- **Dateien aus der APP ausschließen\_Datenordner** ist nicht ausgewählt. Ihre SQL Server Compact Datei für die Mitgliedschafts Datenbank befindet sich in diesem Ordner, und Sie müssen Sie bereitstellen. Wenn Sie Updates bereitstellen, die keine Daten Bank Änderungen enthalten, aktivieren Sie dieses Kontrollkästchen.
- **Diese Anwendung vor dem Veröffentlichen Vorkompilieren** . In den meisten Szenarien ist es nicht erforderlich, Webanwendungs Projekte vorzukompilieren. Weitere Informationen zu dieser Option finden Sie unter [packen/Veröffentlichen der Registerkarte "Web", Projekteigenschaften](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) und Dialog Feld " [Erweiterte vorkompilierungs Einstellungen](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx)".
- **Alle Datenbanken einschließen, die in der Registerkarte "packen/veröffentlichen" konfiguriert** sind, ist aktiviert. diese Option hat jedoch keine Auswirkung, da Sie die Registerkarte " **packen/veröffentlichen** " nicht konfigurieren. Diese Registerkarte ist für eine Legacy-Daten Bank Bereitstellungs Methode vorgesehen, die als einzige Option zum Bereitstellen von SQL Server Datenbanken verwendet wurde. Verwenden Sie die Registerkarte **Paket/veröffentlichen-SQL** im Tutorial [Migration zu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) .
- Der Abschnitt Einstellungen für das **Webbereitstellungs Paket** ist nicht anwendbar, da Sie in diesen Tutorials die One-Click-Veröffentlichung verwenden.

Ändern Sie das Dropdown Feld **Konfiguration** in Debuggen, um die Standardeinstellungen für Debugbuilds anzuzeigen. Die Werte sind identisch, mit der Ausnahme, dass **generierte Debugsymbole ausschließen** deaktiviert ist, sodass Sie beim Bereitstellen eines Debugbuilds Debuggen können.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Sicherstellen, dass der Ordner "ELMAH" bereitgestellt wird

Wie Sie im vorherigen Tutorial gesehen haben, stellt das [ELMAH-nuget-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) Funktionen für die Fehler Protokollierung und Berichterstellung bereit. In der Anwendung "TSO University" wurde ELMAH so konfiguriert, dass Fehlerdetails in einem Ordner mit dem Namen " *ELMAH*" gespeichert werden:

![ELMAH-Ordner](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Es ist eine häufige Anforderung, bestimmte Dateien oder Ordner von der Bereitstellung auszuschließen. ein weiteres Beispiel wäre ein Ordner, in den Benutzer Dateien hochladen können. Sie möchten nicht, dass Protokolldateien oder hochgeladene Dateien, die in Ihrer Entwicklungsumgebung erstellt wurden, in der Produktionsumgebung bereitgestellt werden. Wenn Sie ein Update für die Produktion bereitstellen, möchten Sie nicht, dass der Bereitstellungs Prozess Dateien löscht, die in der Produktionsumgebung vorhanden sind. (Je nachdem, wie Sie eine Bereitstellungs Option festlegen, wird eine Datei, die bei der Bereitstellung am Ziel Standort, jedoch nicht am Quell Standort vorhanden ist, von Web deploy gelöscht.)

Wie Sie zuvor in diesem Tutorial gesehen haben, ist die Option bereit zustellende **Elemente** auf der Registerkarte " **packen/veröffentlichen** " auf nur Dateien festgelegt, die **zum Ausführen dieser Anwendung erforderlich**sind. Folglich werden Protokolldateien, die von ELMAH in der Entwicklung erstellt werden, nicht bereitgestellt. Dies ist das, was Sie tun möchten. (Um bereitgestellt zu werden, müssten Sie in das Projekt eingeschlossen werden, und die Eigenschaft " **Buildaktion** " muss auf " **Content**" festgelegt werden. Weitere Informationen finden Sie unter **Warum werden nicht alle Dateien im Projektordner** bereitgestellt? in den [FAQ zu ASP.NET-Webanwendungs-Projekt Bereitstellung](https://msdn.microsoft.com/library/ee942158.aspx)). Web deploy erstellt jedoch keinen Ordner am Ziel Standort, es sei denn, es gibt mindestens eine Datei, in die kopiert werden soll. Daher fügen Sie dem Ordner eine *txt* -Datei hinzu, die als Platzhalter fungiert, damit der Ordner kopiert wird.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *ELMAH* , wählen Sie **Neues Element hinzufügen**aus, und erstellen Sie eine Datei namens " *plachalter. txt*". Fügen Sie den folgenden Text ein: "Dies ist eine Platzhalter Datei, um sicherzustellen, dass der Ordner bereitgestellt wird." und speichern Sie die Datei. Das ist alles, was Sie tun müssen, um sicherzustellen, dass Visual Studio diese Datei und den Ordner, **in dem Sie** sich befindet, bereitstellt, da die Eigenschaft "Buildvorgang" von *txt* -Dateien standardmäßig auf " **Content** " festgelegt ist.

Sie haben jetzt alle Bereitstellungs Aufgaben abgeschlossen. Im nächsten Tutorial stellen Sie die Site der Site "Site" in der Testumgebung bereit und testen Sie dort.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
