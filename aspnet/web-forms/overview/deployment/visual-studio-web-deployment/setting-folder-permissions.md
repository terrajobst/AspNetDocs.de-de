---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Festlegen der Ordner Berechtigungen | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 410525bb2e3f6e5a0be6d7d6b33fb3a40509041a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614937"
---
# <a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Festlegen von Ordner Berechtigungen

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht über

In diesem Tutorial legen Sie Ordner Berechtigungen für den Ordner *ELMAH* auf der bereitgestellten Website fest, damit die Anwendung Protokolldateien in diesem Ordner erstellen kann.

Wenn Sie eine Webanwendung in Visual Studio mithilfe der Visual Studio Development Server (Cassini) oder IIS Express testen, wird die Anwendung unter ihrer Identität ausgeführt. Sie sind höchstwahrscheinlich ein Administrator auf dem Entwicklungs Computer und haben die volle Berechtigung, beliebige Dateien in einem beliebigen Ordner zu erledigen. Wenn eine Anwendung jedoch unter IIS ausgeführt wird, wird Sie unter der Identität ausgeführt, die für den Anwendungs Pool definiert ist, dem die Website zugewiesen ist. Dabei handelt es sich in der Regel um ein System definiertes Konto mit eingeschränkten Berechtigungen. Standardmäßig verfügt sie über Lese-und Ausführungs Berechtigungen für die Dateien und Ordner Ihrer Webanwendung, verfügt jedoch nicht über Schreibzugriff.

Dies wird zu einem Problem, wenn Ihre Anwendung Dateien erstellt oder aktualisiert, was bei Webanwendungen häufig erforderlich ist. In der Anwendung der Anwendung "der Anwendung" erstellt ELMAH XML-Dateien im Ordner " *ELMAH* ", um Details zu Fehlern zu speichern. Auch wenn Sie nichts wie ELMAH verwenden, kann Ihre Website Benutzern das Hochladen von Dateien oder das Ausführen anderer Aufgaben ermöglichen, mit denen Daten in einen Ordner auf Ihrer Website geschrieben werden.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](troubleshooting.md)Behandlung überprüfen.

## <a name="test-error-logging-and-reporting"></a>Test Fehler Protokollierung und Berichterstellung

Wenn Sie sehen möchten, wie die Anwendung in IIS nicht ordnungsgemäß funktioniert (obwohl dies beim Testen in Visual Studio der Fall war), können Sie einen Fehler auslösen, der normalerweise von ELMAH protokolliert wird. Öffnen Sie dann das ELMAH-Fehlerprotokoll, um die Details anzuzeigen. Wenn ELMAH keine XML-Datei erstellen und die Fehlerdetails speichern konnte, wird ein leerer Fehlerbericht angezeigt.

Öffnen Sie einen Browser, navigieren Sie zu `http://localhost/ContosoUniversity`, und fordern Sie dann eine ungültige URL wie *studentsxxx. aspx*an. Anstelle der Seite *GenericErrorPage. aspx* wird eine vom systemgenerierte Fehlerseite angezeigt, da die `customErrors` Einstellung in der Datei Web. config "RemoteOnly" ist und Sie IIS lokal ausführen:

![HTTP 404-Fehlerseite](setting-folder-permissions/_static/image1.png)

Führen Sie nun " *ELMAH. axd* " aus, um den Fehlerbericht anzuzeigen. Nachdem Sie sich mit den Anmelde Informationen für das Administrator Konto (&quot;admin&quot; und &quot;devpwd-&quot;) angemeldet haben, wird eine leere Fehlerprotokoll Seite angezeigt, da ELMAH keine XML-Datei im Ordner *ELMAH* erstellen konnte:

![Fehlerprotokoll leer](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Festlegen der Schreib Berechtigung für den Ordner "ELMAH"

Ordner Berechtigungen können manuell festgelegt werden, oder Sie können einen automatischen Teil des Bereitstellungs Prozesses vornehmen. Für eine automatische Ausführung ist ein komplexer MSBuild-Code erforderlich, und da Sie diesen Schritt nur bei der ersten Bereitstellung durchführen müssen, gehen Sie wie folgt vor. (Informationen dazu, wie Sie diesen Teil des Bereitstellungs Prozesses vornehmen, finden Sie unter [Festlegen von Ordner Berechtigungen für die Webveröffentlichung](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) im Blog von Sayed Hashimi.)

1. Navigieren Sie im **Datei-Explorer**zu " *c:\inetpub\wwwroot\condesouniversity*". Klicken Sie mit der rechten Maustaste auf den Ordner *ELMAH* , wählen Sie **Eigenschaften**, und wählen Sie dann die Registerkarte **Sicherheit** .
2. Klicken Sie auf **Bearbeiten**.
3. Wählen Sie im Dialogfeld **Berechtigungen für ELMAH** den **Eintrag DefaultAppPool**aus, und aktivieren Sie dann das Kontrollkästchen **Schreiben** in der Spalte **zulassen** .

    ![Berechtigungen für den ELMAH-Ordner](setting-folder-permissions/_static/image3.png)

    (Wenn **DefaultAppPool** nicht in der Liste **Gruppen-oder Benutzernamen** angezeigt wird, haben Sie wahrscheinlich eine andere Methode als die in diesem Tutorial festgelegte Methode zum Einrichten von IIS und ASP.NET 4 auf Ihrem Computer verwendet. Ermitteln Sie in diesem Fall, welche Identität vom Anwendungs Pool verwendet wird, der der Anwendung der Anwendung "die Anwendung" der Anwendung "der Anwendung" zugewiesen ist, und erteilen Sie dieser Identität Schreib Berechtigung. Weitere Informationen finden Sie unter den Links zu Anwendungs Pool Identitäten am Ende dieses Tutorials.) Klicken Sie in beiden Dialogfeldern auf **OK** .

## <a name="retest-error-logging-and-reporting"></a>Fehler Protokollierung und Berichterstellung erneut testen

Testen Sie, indem Sie einen Fehler auf die gleiche Weise auslösen (ungültige URL anfordern), und führen Sie die **Fehlerprotokoll** Seite aus. Dieses Mal wird der Fehler auf der Seite angezeigt.

![ELMAH-Fehlerprotokoll Seite](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Summary

Sie haben jetzt alle Aufgaben abgeschlossen, die erforderlich sind, damit die Configuration Manager-Umgebung in IIS auf dem lokalen Computer ordnungsgemäß funktioniert. Im nächsten Tutorial machen Sie die Website öffentlich verfügbar, indem Sie Sie in Azure bereitstellen.

## <a name="more-information"></a>Weitere Informationen

In diesem Beispiel war der Grund, warum ELMAH die Protokolldateien nicht speichern konnte, ziemlich offensichtlich. In Fällen, in denen die Ursache des Problems nicht offensichtlich ist, können Sie die IIS-Ablauf Verfolgung verwenden. Weitere Informationen finden [Sie unter Problembehandlung bei Anforderungen mit Ablauf Verfolgung in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) auf der IIS.NET-Website

Weitere Informationen zum Erteilen von Berechtigungen für Anwendungs Pool Identitäten finden Sie unter [Anwendungs Pool Identitäten](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) und [sichere Inhalte in IIS über Datei System-ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) auf der IIS.NET-Website.

> [!div class="step-by-step"]
> [Zurück](deploying-to-iis.md)
> [Weiter](deploying-to-production.md)
