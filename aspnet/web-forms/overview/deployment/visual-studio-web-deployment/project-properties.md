---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Projekteigenschaften | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: b2811791a897c9166f6222c23dddc6921e5267ab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614945"
---
# <a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Projekteigenschaften

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht über

Einige Bereitstellungs Optionen werden in den Projekteigenschaften konfiguriert, die in der Projektdatei ( *csproj* -oder *vbproj* -Datei) gespeichert sind. In den meisten Fällen sind die Standardwerte dieser Einstellungen die gewünschten Werte, aber Sie können die in Visual Studio integrierte **Projekteigenschaften** -Benutzeroberfläche verwenden, um diese Einstellungen zu bearbeiten, wenn Sie Sie ändern müssen. In diesem Tutorial überprüfen Sie die Bereitstellungs Einstellungen in den **Projekteigenschaften**. Außerdem erstellen Sie eine Platzhalter Datei, die bewirkt, dass ein leerer Ordner bereitgestellt wird.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Konfigurieren von Bereitstellungs Einstellungen im Fenster "Projekteigenschaften"

Die meisten Einstellungen, die beeinflussen, was während der Bereitstellung geschieht, sind im Veröffentlichungs Profil enthalten, wie Sie in den folgenden Tutorials sehen werden. Einige Einstellungen, die Sie beachten sollten, befinden sich auf den Registerkarten **Paket/veröffentlichen** im Fenster **Eigenschaften des Projekts** . Diese Einstellungen werden für jede Buildkonfiguration angegeben, d. –. Sie können unterschiedliche Einstellungen für einen Releasebuild haben, als für einen Debugbuild vorhanden sind.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **condesouniversity** , wählen Sie **Eigenschaften**aus, und wählen Sie dann die Registerkarte **Web packen/veröffentlichen** aus.

![Registerkarte "Web packen/veröffentlichen"](project-properties/_static/image1.png)

Wenn das Fenster angezeigt wird, werden standardmäßig Einstellungen für die Buildkonfiguration angezeigt, die derzeit für die Lösung aktiv ist. Wenn im **Konfigurations** Feld nicht **aktiv (Release)** angegeben ist, wählen Sie **Release** aus, um die Einstellungen für die releasebuildkonfiguration anzuzeigen. Sie stellen Releasebuilds sowohl in Ihrer Test-als auch in der Produktionsumgebung bereit.

![Auswählen der Releasebuild-Konfiguration](project-properties/_static/image2.png)

Wenn **aktiv (Release)** oder **Release** ausgewählt ist, werden die Werte angezeigt, die bei der Bereitstellung mithilfe der releasebuildkonfiguration wirksam sind:

- Im Feld bereit zustellende **Elemente** werden **nur die Dateien ausgewählt, die zum Ausführen der Anwendung erforderlich** sind. Andere Optionen sind **alle Dateien in diesem Projekt** oder **alle Dateien in diesem Projektordner**. Wenn Sie die Standardauswahl unverändert lassen, vermeiden Sie die Bereitstellung von Quell Code Dateien, z. b. Diese Einstellung ist der Grund, warum die Ordner, die die SQL Server Compact Binärdateien enthalten, in das Projekt eingeschlossen werden mussten. Weitere Informationen zu dieser Einstellung finden Sie unter **Warum werden nicht alle Dateien im Projektordner** bereitgestellt? in den [FAQ zu ASP.NET-Webanwendungs-Projekt Bereitstellung](https://msdn.microsoft.com/library/ee942158.aspx).
- **Generierte Debugsymbole ausschließen** ist ausgewählt. Sie werden nicht debuggen, wenn Sie diese Buildkonfiguration verwenden.
- **Alle in der Registerkarte "Paket/veröffentlichen" konfigurierten Datenbanken einschließen** ausgewählt ist. Gibt an, ob Visual Studio-Datenbanken und-Dateien bereitstellt. Obwohl die Kontrollkästchen Bezeichnung nur die Registerkarte " **packen/veröffentlichen** " erwähnt, würde das Deaktivieren dieses Kontrollkästchens auch die im Veröffentlichungs Profil konfigurierte Daten Bank Bereitstellung deaktivieren. Dies wird später der Fall sein, damit das Kontrollkästchen aktiviert bleiben muss. Die Registerkarte **Paket-/Veröffentlichungs-SQL** wird für eine Legacy Datenbank-Veröffentlichungs Methode verwendet, die Sie in diesen Tutorials nicht verwenden.
- Der Abschnitt Einstellungen für das **Webbereitstellungs Paket** ist nicht anwendbar, da Sie in diesen Tutorials die One-Click-Veröffentlichung verwenden.

Ändern Sie das Dropdown Feld **Konfiguration** in Debuggen, um die Standardeinstellungen für Debugbuilds anzuzeigen. Die Werte sind identisch, mit der Ausnahme, dass **generierte Debugsymbole ausschließen** deaktiviert ist, sodass Sie beim Bereitstellen eines Debugbuilds Debuggen können.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Stellen Sie sicher, dass der Ordner ELMAH bereitgestellt wird.

Wie Sie im vorherigen Tutorial gesehen haben, stellt das [ELMAH-nuget-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) Funktionen für die Fehler Protokollierung und Berichterstellung bereit. In der Anwendung "TSO University" wurde ELMAH so konfiguriert, dass Fehlerdetails in einem Ordner mit dem Namen " *ELMAH*" gespeichert werden:

![ELMAH-Ordner](project-properties/_static/image3.png)

Es ist eine häufige Anforderung, bestimmte Dateien oder Ordner von der Bereitstellung auszuschließen. ein weiteres Beispiel wäre ein Ordner, in den Benutzer Dateien hochladen können. Sie möchten nicht, dass Protokolldateien oder hochgeladene Dateien, die in Ihrer Entwicklungsumgebung erstellt wurden, in der Produktionsumgebung bereitgestellt werden. Wenn Sie ein Update für die Produktion bereitstellen, möchten Sie nicht, dass der Bereitstellungs Prozess Dateien löscht, die in der Produktionsumgebung vorhanden sind. (Je nachdem, wie Sie eine Bereitstellungs Option festlegen, wird eine Datei, die bei der Bereitstellung am Ziel Standort, jedoch nicht am Quell Standort vorhanden ist, von Web deploy gelöscht.)

Wie Sie zuvor in diesem Tutorial gesehen haben, ist die Option bereit zustellende **Elemente** auf der Registerkarte " **packen/veröffentlichen** " auf nur Dateien festgelegt, die **zum Ausführen dieser Anwendung erforderlich**sind. Folglich werden Protokolldateien, die von ELMAH in der Entwicklung erstellt werden, nicht bereitgestellt. Dies ist das, was Sie tun möchten. (Um bereitgestellt zu werden, müssten Sie in das Projekt eingeschlossen werden, und die Eigenschaft " **Buildaktion** " muss auf " **Content**" festgelegt werden. Weitere Informationen finden Sie unter **Warum werden nicht alle Dateien im Projektordner** bereitgestellt? in den [FAQ zu ASP.NET-Webanwendungs-Projekt Bereitstellung](https://msdn.microsoft.com/library/ee942158.aspx)). Web deploy erstellt jedoch keinen Ordner am Ziel Standort, es sei denn, es gibt mindestens eine Datei, in die kopiert werden soll. Daher fügen Sie dem Ordner eine *txt* -Datei hinzu, die als Platzhalter fungiert, damit der Ordner kopiert wird.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *ELMAH* , wählen Sie **Neues Element hinzufügen**aus, und erstellen Sie eine Textdatei namens " *plachalter. txt*". Fügen Sie den folgenden Text ein: "Dies ist eine Platzhalter Datei, um sicherzustellen, dass der Ordner bereitgestellt wird." und speichern Sie die Datei. Das ist alles, was Sie tun müssen, um sicherzustellen, dass Visual Studio diese Datei und den Ordner, **in dem Sie** sich befindet, bereitstellt, da die Eigenschaft "Buildvorgang" von *txt* -Dateien standardmäßig auf " **Content** " festgelegt ist.

## <a name="summary"></a>Summary

Sie haben jetzt alle Bereitstellungs Aufgaben abgeschlossen. Im nächsten Tutorial stellen Sie die Site der Site "Site" in der Testumgebung bereit und testen Sie dort.

> [!div class="step-by-step"]
> [Zurück](web-config-transformations.md)
> [Weiter](deploying-to-iis.md)
