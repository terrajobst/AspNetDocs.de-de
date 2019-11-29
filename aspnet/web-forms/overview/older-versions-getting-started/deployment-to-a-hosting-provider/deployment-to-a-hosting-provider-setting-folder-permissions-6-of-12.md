---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Festlegen der Ordner Berechtigungen-6 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 85a77a196cf3458bbb2e6308838a846936cd070b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633522"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Festlegen der Ordner Berechtigungen-6 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht über

In diesem Tutorial legen Sie Ordner Berechtigungen für den Ordner *ELMAH* auf der bereitgestellten Website fest, damit die Anwendung Protokolldateien in diesem Ordner erstellen kann.

Wenn Sie eine Webanwendung in Visual Studio mithilfe der Visual Studio Development Server (Cassini) testen, wird die Anwendung unter ihrer Identität ausgeführt. Sie sind höchstwahrscheinlich ein Administrator auf dem Entwicklungs Computer und haben die volle Berechtigung, beliebige Dateien in einem beliebigen Ordner zu erledigen. Wenn eine Anwendung jedoch unter IIS ausgeführt wird, wird Sie unter der Identität ausgeführt, die für den Anwendungs Pool definiert ist, dem die Website zugewiesen ist. Dabei handelt es sich in der Regel um ein System definiertes Konto mit eingeschränkten Berechtigungen. Standardmäßig verfügt sie über Lese-und Ausführungs Berechtigungen für die Dateien und Ordner Ihrer Webanwendung, verfügt jedoch nicht über Schreibzugriff.

Dies wird zu einem Problem, wenn Ihre Anwendung Dateien erstellt oder aktualisiert, was bei Webanwendungen häufig erforderlich ist. In der Anwendung der Anwendung "der Anwendung" erstellt ELMAH XML-Dateien im Ordner " *ELMAH* ", um Details zu Fehlern zu speichern. Auch wenn Sie nichts wie ELMAH verwenden, kann Ihre Website Benutzern das Hochladen von Dateien oder das Ausführen anderer Aufgaben ermöglichen, mit denen Daten in einen Ordner auf Ihrer Website geschrieben werden.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="testing-error-logging-and-reporting"></a>Testen der Fehler Protokollierung und Berichterstellung

Wenn Sie sehen möchten, wie die Anwendung in IIS nicht ordnungsgemäß funktioniert (obwohl dies beim Testen in Visual Studio der Fall war), können Sie einen Fehler auslösen, der normalerweise von ELMAH protokolliert wird. Öffnen Sie dann das ELMAH-Fehlerprotokoll, um die Details anzuzeigen. Wenn ELMAH keine XML-Datei erstellen und die Fehlerdetails speichern konnte, wird ein leerer Fehlerbericht angezeigt.

Öffnen Sie einen Browser, navigieren Sie zu `http://localhost/ContosoUniversity`, und fordern Sie dann eine ungültige URL wie *studentsxxx. aspx*an. Anstelle der Seite *GenericErrorPage. aspx* wird eine vom systemgenerierte Fehlerseite angezeigt, da die `customErrors` Einstellung in der Datei Web. config "RemoteOnly" ist und Sie IIS lokal ausführen:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Führen Sie nun " *ELMAH. axd* " aus, um den Fehlerbericht anzuzeigen. Es wird eine leere Fehlerprotokoll Seite angezeigt, da ELMAH keine XML-Datei im Ordner *ELMAH* erstellen konnte:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Festlegen der Schreib Berechtigung für den Ordner "ELMAH"

Ordner Berechtigungen können manuell festgelegt werden, oder Sie können einen automatischen Teil des Bereitstellungs Prozesses vornehmen. Um die automatische Ausführung zu ermöglichen, erfordert dies einen komplexen MSBuild-Code. da Sie diesen Vorgang nur bei der ersten Bereitstellung durchführen müssen, wird in diesem Tutorial nur die manuelle Vorgehensweise erläutert. (Informationen dazu, wie Sie diesen Teil des Bereitstellungs Prozesses vornehmen, finden Sie unter [Festlegen von Ordner Berechtigungen für die Webveröffentlichung](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) im Blog von Sayed Hashimi.)

Navigieren Sie in **Windows-Explorer**zu *c:\inetpub\wwwroot\condesouniversity*. Klicken Sie mit der rechten Maustaste auf den Ordner *ELMAH* , wählen Sie **Eigenschaften**, und wählen Sie dann die Registerkarte **Sicherheit** .

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Wenn **DefaultAppPool** nicht in der Liste **Gruppen-oder Benutzernamen** angezeigt wird, haben Sie wahrscheinlich eine andere Methode als die in diesem Tutorial festgelegte Methode zum Einrichten von IIS und ASP.NET 4 auf Ihrem Computer verwendet. Ermitteln Sie in diesem Fall, welche Identität vom Anwendungs Pool verwendet wird, der der Anwendung der Anwendung "die Anwendung" der Anwendung "der Anwendung" zugewiesen ist, und erteilen Sie dieser Identität Schreib Berechtigung. Weitere Informationen finden Sie unter den Links zu Anwendungs Pool Identitäten am Ende dieses Tutorials.)

Klicken Sie auf **Bearbeiten**. Wählen Sie im Dialogfeld **Berechtigungen für ELMAH** den **Eintrag DefaultAppPool**aus, und aktivieren Sie dann das Kontrollkästchen **Schreiben** in der Spalte **zulassen** .

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Klicken Sie in beiden Dialogfeldern auf **OK** .

## <a name="retesting-error-logging-and-reporting"></a>Erneute Test Fehler Protokollierung und Berichterstellung

Testen Sie, indem Sie einen Fehler auf die gleiche Weise auslösen (ungültige URL anfordern), und führen Sie die **Fehlerprotokoll** Seite aus. Dieses Mal wird der Fehler auf der Seite angezeigt.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Sie benötigen außerdem die Schreib Berechtigung für die *App\_Daten* Ordner, da Sie über SQL Server Compact Datenbankdateien in diesem Ordner verfügen und in der Lage sein möchten, Daten in diesen Datenbanken zu aktualisieren. In diesem Fall müssen Sie jedoch keine zusätzlichen Schritte durchführen, da der Bereitstellungs Prozess automatisch die Schreib Berechtigung für den *App-\_Daten* Ordner festlegt.

Sie haben jetzt alle Aufgaben abgeschlossen, die erforderlich sind, damit die Configuration Manager-Umgebung in IIS auf dem lokalen Computer ordnungsgemäß funktioniert. Im nächsten Tutorial machen Sie die Website öffentlich verfügbar, indem Sie Sie für einen Hostinganbieter bereitstellen.

## <a name="more-information"></a>Weitere Informationen

In diesem Beispiel war der Grund, warum ELMAH die Protokolldateien nicht speichern konnte, ziemlich offensichtlich. In Fällen, in denen die Ursache des Problems nicht offensichtlich ist, können Sie die IIS-Ablauf Verfolgung verwenden. Weitere Informationen finden [Sie unter Problembehandlung bei Anforderungen mit Ablauf Verfolgung in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) auf der IIS.NET-Website

Weitere Informationen zum Erteilen von Berechtigungen für Anwendungs Pool Identitäten finden Sie unter [Anwendungs Pool Identitäten](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) und [sichere Inhalte in IIS über Datei System-ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) auf der IIS.NET-Website.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
