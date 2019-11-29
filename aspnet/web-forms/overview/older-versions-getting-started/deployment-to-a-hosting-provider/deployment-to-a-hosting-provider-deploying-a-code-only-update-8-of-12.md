---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen eines reinen Code Updates-8 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: e4d094ef84a747c36ce05ddb0e3d1ce0391d5605
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74572777"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen eines reinen Code Updates-8 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht über

Nach der erstmaligen Bereitstellung wird die Wartung und Entwicklung Ihrer Website fortgesetzt, und bevor Sie mit der Zeit arbeiten, möchten Sie ein Update bereitstellen. Dieses Tutorial führt Sie durch den Prozess der Bereitstellung eines Updates für den Anwendungscode. Diese Aktualisierung umfasst keine Daten Bank Änderung. im nächsten Tutorial sehen Sie, was die Bereitstellung einer Daten Bank Änderung unterscheidet.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="making-a-code-change"></a>Vornehmen einer Code Änderung

Als einfaches Beispiel für ein Update der Anwendung fügen Sie der Seite **Dozenten** eine Liste der Kurse hinzu, die vom ausgewählten Dozenten gelehrt werden.

Wenn Sie die Seite **Dozenten** ausführen, werden Sie feststellen, dass im Raster **ausgewählte** Verknüpfungen vorhanden sind, aber Sie führen nichts anderes aus, als den Zeilen Hintergrund grau zu lassen.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Nun fügen Sie Code hinzu, der ausgeführt wird, wenn auf den Link **auswählen** geklickt wird, und zeigt eine Liste der Kurse an, die vom ausgewählten Dozenten gelehrt werden.

Fügen Sie in " *Dozenten. aspx*" direkt nach dem `Label`-Steuerelement **errormessagelabel** das folgende Markup hinzu:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Führen Sie die Seite aus, und wählen Sie einen Dozenten. Es wird eine Liste der Kurse angezeigt, die von diesem Dozenten gelehrt werden.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Bereitstellen des Code Updates in der Test Umgebung

Die Bereitstellung in der Testumgebung ist eine einfache Frage, dass die One-Click-Veröffentlichung erneut ausgeführt werden kann. Um diesen Vorgang zu beschleunigen, können Sie die Symbolleiste " **Web One Click Publish** " verwenden.

Klicken Sie im Menü **Ansicht auf** **Symbolleisten** , und wählen Sie dann **Web One Click Publish**aus.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

Wählen Sie in **Projektmappen-Explorer**das Projekt conjesouniversity aus.

**Klicken Sie im Web auf** die Symbolleiste veröffentlichen, wählen **Sie das Profil** Veröffentlichungs Profil aus, und klicken Sie dann auf **Web veröffentlichen** (das Symbol mit Pfeilen nach links und rechts).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser öffnet automatisch die Startseite. Führen Sie die Seite Dozenten aus, und wählen Sie einen Dozenten aus, um sicherzustellen, dass das Update erfolgreich bereitgestellt

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalerweise würden Sie auch Regressionstests durchführen (Testen Sie den Rest der Site, um sicherzustellen, dass die neue Änderung keinerlei vorhandene Funktionalität beeinträchtigt). In diesem Tutorial überspringen Sie diesen Schritt und stellen das Update in der Produktionsumgebung bereit.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Verhindern der erneuten Bereitstellung des anfänglichen Daten Bank Zustands in der Produktion

In einer echten Anwendung interagieren Benutzer nach ihrer ersten Bereitstellung mit ihrer Produktions Website, und die Datenbanken werden mit Livedaten aufgefüllt. Aus diesem Grund möchten Sie die Mitgliedschafts Datenbank nicht in Ihrem ursprünglichen Zustand erneut bereitstellen, wodurch alle Livedaten zurückgesetzt werden. Da SQL Server Compact-Datenbanken Dateien im Ordner " *App\_Daten* " sind, müssen Sie dies verhindern, indem Sie die Bereitstellungs Einstellungen so ändern, dass Dateien im Ordner " *App\_Data* " nicht bereitgestellt werden.

Öffnen Sie das **Projekteigenschaften** Fenster für das Projekt contosouniversity, und wählen Sie die Registerkarte **Web packen/veröffentlichen** aus. Stellen Sie sicher, dass im Dropdown Feld **Konfiguration** entweder **aktiv (Release)** oder **Release** ausgewählt ist, und wählen Sie **Dateien aus dem App-\_Datenordner ausschließen aus**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Wenn Sie in Zukunft einen Debugbuild bereitstellen möchten, empfiehlt es sich, die gleiche Änderung für die Debug-Buildkonfiguration vorzunehmen: Ändern Sie die **Konfiguration** in **Debug** , und wählen Sie dann **Dateien aus dem App-\_Datenordner ausschließen aus**.

Speichern und schließen Sie die Registerkarte **Web packen/veröffentlichen** .

> [!NOTE] 
> 
> [!IMPORTANT]
> Stellen Sie sicher, dass Sie in ihren Veröffentlichungs Profilen keine **zusätzlichen Dateien am Ziel** ausgewählt haben. Wenn Sie diese Option auswählen, löscht der Bereitstellungs Prozess die Datenbanken, die Sie in App-\_Daten am bereitgestellten Standort haben, und löscht die APP\_Datenordner selbst.

## <a name="preventing-user-access-to-the-production-site-during-update"></a>Verhindern des Benutzer Zugriffs auf den Produktionsstandort während des Updates

Die Änderung, die Sie jetzt bereitstellen, ist eine einfache Änderung an einer einzelnen Seite. Manchmal stellen Sie jedoch größere Änderungen bereit, und in diesem Fall kann sich die Website seltsam Verhalten, wenn ein Benutzer eine Seite anfordert, bevor die Bereitstellung abgeschlossen ist. Um dies zu verhindern, können Sie eine *App\_offline. htm* -Datei verwenden. Wenn Sie eine Datei mit dem Namen *App\_offline. htm* im Stamm Ordner Ihrer Anwendung ablegen, wird diese Datei automatisch von IIS angezeigt, anstatt Ihre Anwendung zu verwenden. Um den Zugriff während der Bereitstellung zu verhindern, legen Sie die *App\_offline. htm* im Stamm Ordner ab, führen den Bereitstellungs Prozess aus und entfernen dann *App\_offline. htm*.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Lösung (nicht auf eines der Projekte), und wählen Sie **neuer Projektmappenordner**aus.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nennen Sie den Ordner *solutionfiles*.

Erstellen Sie im neuen Ordner eine HTML-Seite mit dem Namen *App\_offline. htm*. Ersetzen Sie den vorhandenen Inhalt durch das folgende Markup:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Sie können die *App\_offline. htm* -Datei mithilfe einer FTP-Verbindung oder des **Datei-Manager** -Hilfsprogramms in der Systemsteuerung des hostinganbieters auf die Website kopieren. In diesem Tutorial verwenden Sie den Datei- **Manager**.

Öffnen Sie die Systemsteuerung, und wählen Sie den **Datei-Manager** wie im Tutorial bereitstellen [in der Produktionsumgebung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) aus. Wählen Sie **contosouniversity.com** und dann **wwwroot** aus, um den Stamm Ordner Ihrer Anwendung zu erhalten, und klicken Sie dann auf **hochladen**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

Wählen Sie im Dialogfeld **Datei hochladen** die Datei *App\_offline. htm* aus, und klicken Sie dann auf **hochladen**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Navigieren Sie zu Ihrer Website-URL. Sie sehen, dass die Seite *App\_offline. htm* jetzt anstelle der Startseite angezeigt wird.

[![App_offline. htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Sie sind jetzt bereit für die Bereitstellung in der Produktion.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Bereitstellen des Code Updates in der Produktionsumgebung

Klicken Sie im **Web auf** die Symbolleiste veröffentlichen, wählen Sie das **Produktions** Veröffentlichungs Profil aus, und klicken Sie dann auf **Web veröffentlichen**.

Visual Studio stellt die aktualisierte Anwendung bereit und öffnet den Browser auf der Startseite der Website. Die *App\_offline. htm* -Datei wird angezeigt. Bevor Sie testen können, um die erfolgreiche Bereitstellung zu überprüfen, müssen Sie die *App\_offline. htm* -Datei entfernen.

Kehren Sie zur **Datei-Manager** -Anwendung in der Systemsteuerung zurück. Wählen Sie **contosouniversity.com** und **wwwroot**aus, wählen Sie **App\_offline. htm**aus, und klicken Sie dann auf **Löschen**.

[![Deleting_app_offline. htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Öffnen Sie im Browser die Seite Dozenten der öffentlichen Website, und wählen Sie einen Dozenten aus, um zu überprüfen, ob das Update erfolgreich bereitgestellt wurde.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Sie haben nun ein Anwendungs Update bereitgestellt, das keine Daten Bank Änderung enthielt. Im nächsten Tutorial erfahren Sie, wie Sie eine Daten Bank Änderung bereitstellen.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
