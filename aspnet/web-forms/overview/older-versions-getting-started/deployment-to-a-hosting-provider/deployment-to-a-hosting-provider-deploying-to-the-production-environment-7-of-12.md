---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen in der Produktionsumgebung-7 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: db838633accdedd7c0693b126a007e254ca681e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78458169"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen in der Produktionsumgebung-7 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht

In diesem Tutorial richten Sie ein Konto mit einem Hostinganbieter ein und stellen Ihre ASP.NET-Webanwendung mithilfe der Visual Studio-Funktion zum Veröffentlichen mit nur einem Mausklick in der Produktionsumgebung bereit.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="selecting-a-hosting-provider"></a>Auswählen eines hostinganbieters

Für die Anwendung "" der Anwendung "der Anwendung" und dieses tutorialreihe benötigen Sie einen Anbieter, der ASP.NET 4 und Web deploy unterstützt. Es wurde ein bestimmtes Hostingunternehmen ausgewählt, damit die Tutorials die gesamte Umgebung für die Bereitstellung auf einer Live Website veranschaulichen. Jedes Hostingunternehmen bietet verschiedene Features, und die Umgebung für die Bereitstellung auf den Servern variiert in gewisser Weise. Der in diesem Tutorial beschriebene Prozess ist jedoch typisch für den gesamten Prozess. Der für dieses Tutorial verwendete Hostinganbieter (Cytanium.com) ist einer von vielen, die verfügbar sind, und die Verwendung in diesem Tutorial stellt keine Bestätigung oder Empfehlung dar.

Wenn Sie bereit sind, ihren eigenen Hostinganbieter auszuwählen, können Sie die Features und Preise im Katalog [der Anbieter](https://www.microsoft.com/web/hosting) auf der Microsoft.com/Web-Website vergleichen.

## <a name="creating-an-account"></a>Erstellen eines Kontos

Erstellen Sie ein Konto beim ausgewählten Anbieter. Wenn die Unterstützung für eine vollständige SQL Server Datenbank ein zusätzliches zusätzliches ist, müssen Sie Sie nicht für dieses Tutorial auswählen, aber Sie benötigen Sie für die [Migration zu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) Tutorial später in dieser Reihe.

Für diese Tutorials müssen Sie keinen neuen Domänen Namen registrieren. Sie können testen, um die erfolgreiche Bereitstellung zu überprüfen, indem Sie die temporäre URL verwenden, die vom Anbieter dem Standort zugewiesen wurde.

Nachdem das Konto erstellt wurde, erhalten Sie in der Regel eine Begrüßungs-e-Mail, die alle Informationen enthält, die Sie zum Bereitstellen und Verwalten Ihres Standorts benötigen. Die Informationen, die von Ihrem Hostinganbieter gesendet werden, ähneln den hier gezeigten Informationen. Die an neue Konto Besitzer gesendete cytanium-Willkommens-e-Mail enthält die folgenden Informationen:

- Die URL zur System Steuerungs Website des Anbieters, auf der Sie Einstellungen für Ihre Website verwalten können. Die ID und das Kennwort, die Sie angegeben haben, sind in diesem Teil der Willkommens-e-Mail enthalten. (Beide wurden in einen demowert für diese Abbildung geändert.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Die Standard .NET Framework Version und Informationen darüber, wie Sie geändert werden. Viele Hostingsites werden standardmäßig auf 2,0 festgestellt. Dies funktioniert mit ASP.NET-Anwendungen, die auf die .NET Framework 2,0, 3,0 oder 3,5 abzielen. Die "die University" ist jedoch eine .NET Framework 4-Anwendung, daher müssen Sie diese Einstellung ändern. (Bei einer ASP.NET 4,5-Anwendung verwenden Sie die .NET 4,0-Einstellung.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Die temporäre URL, die Sie für den Zugriff auf die Website verwenden können. Beim Erstellen dieses Kontos wurde "contosouniversity.com" als vorhandener Domänen Name eingegeben. Daher wird die temporäre URL `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informationen zum Einrichten von Datenbanken und die Verbindungs Zeichenfolgen, die Sie benötigen, um darauf zuzugreifen:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informationen zu Tools und Einstellungen für die Bereitstellung Ihres Standorts. (In der e-Mail von cytanium wird auch webmatrix erwähnt, die hier ausgelassen wird.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Festlegen der .NET Framework Version

Die cytanium-Willkommens-e-Mail enthält einen Link zu Anweisungen zum Ändern der Version der .NET Framework. Diese Anweisungen erläutern, dass dies über die Systemsteuerung von cytanium erfolgen kann. Andere Anbieter haben System Steuerungs Websites, die anders aussehen, oder Sie weisen Sie möglicherweise an, dies auf andere Weise zu erreichen.

Wechseln Sie zur System Steuerungs-URL. Nach der Anmeldung mit Ihrem Benutzernamen und Kennwort wird die Systemsteuerung angezeigt.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

Halten Sie den Mauszeiger auf das Websymbol im Feld **hostingplätze** , und wählen Sie im Menü den Befehl **Websites** aus.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

Klicken Sie im Feld **Websites** auf **contosouniversity.com** (den Namen der Website, die Sie beim Erstellen des Kontos verwendet haben).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

Wählen Sie im Feld **Eigenschaften der Website** die Registerkarte **Erweiterungen** aus.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Ändern Sie ASP.net von **2,0 Integrated Pipeline** in **4,0 (integrierte Pipeline)** , und klicken Sie dann auf **Aktualisieren**.

## <a name="publishing-to-the-hosting-provider"></a>Veröffentlichen für den Hostinganbieter

Die Willkommens-e-Mail des hostinganbieters umfasst alle Einstellungen, die Sie benötigen, um das Projekt zu veröffentlichen, und Sie können diese Informationen manuell in ein Veröffentlichungs Profil eingeben. Sie verwenden jedoch eine einfachere und weniger fehleranfällige Methode zum Konfigurieren der Bereitstellung für den Anbieter: Sie laden eine *publishsettings* -Datei herunter und importieren Sie in ein Veröffentlichungs Profil.

Wechseln Sie in Ihrem Browser zur Systemsteuerung cytanium, wählen Sie **Web** aus, und wählen Sie dann **Websites aus.**

![Systemsteuerung auswählen von Websites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Wählen Sie die **contosouniversity.com** -Website aus.

![Systemsteuerung contosouniversity.com auswählen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Wählen Sie die Registerkarte **Webpublishing** aus.

![Registerkarte "Webveröffentlichung"](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Erstellen Sie Anmelde Informationen für die Webveröffentlichung, indem Sie einen Benutzernamen und ein Kennwort eingeben. Sie können dieselben Anmelde Informationen eingeben, die Sie zum Anmelden bei der Systemsteuerung verwenden. Klicken Sie dann auf **Enable** (Aktivieren).

![Systemsteuerung Erstellen von Veröffentlichungs Anmelde Informationen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Klicken Sie auf **Veröffentlichungs Profil für diese Website herunterladen**.

![Download Veröffentlichungs Profil der Systemsteuerung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Wenn Sie aufgefordert werden, die Datei zu öffnen oder zu speichern, speichern Sie Sie.

![Veröffentlichungs Profil Datei speichern](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

Klicken Sie in **Projektmappen-Explorer** in Visual Studio mit der rechten Maustaste auf das Projekt conjesouniversity, und wählen Sie **veröffentlichen**aus. Das Dialogfeld **Web veröffentlichen** wird auf der Registerkarte **Vorschau** mit ausgewähltem **Test** Profil geöffnet, da es sich dabei um das letzte verwendete Profil handelt.

Wählen Sie die Registerkarte **Profil** aus, und klicken Sie auf **importieren**.

![Schaltfläche zum Veröffentlichen des Web-Assistenten](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

Wählen Sie im Dialogfeld **Veröffentlichungs Einstellungen importieren** die *publishsettings* -Datei aus, die Sie heruntergeladen haben, und klicken Sie auf **Öffnen**. Der Assistent wechselt zur Registerkarte Verbindung, in der alle Felder ausgefüllt sind.

![Registerkarte "Web-Assistent veröffentlichen"](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Die publishsettings-Datei fügt die geplante permanente URL für die Website in das Feld Ziel-URL ein. Wenn Sie diese Domäne noch nicht erworben haben, ersetzen Sie den Wert durch die temporäre URL. In diesem Beispiel ist die URL  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* Der einzige Zweck dieses Kontrollkästchens ist die Angabe der URL, nach der der Browser nach erfolgreicher Bereitstellung automatisch geöffnet wird. Wenn Sie das Feld leer lassen, besteht die einzige Konsequenz darin, dass der Browser nach der Bereitstellung nicht automatisch gestartet wird.

Klicken Sie auf **Verbindung** überprüfen, um sicherzustellen, dass die Einstellungen richtig sind und Sie eine Verbindung mit dem Server herstellen können Wie Sie bereits gesehen haben, wird durch ein grünes Häkchen überprüft, ob die Verbindung erfolgreich hergestellt wurde.

Wenn Sie auf Verbindung überprüfen klicken, wird möglicherweise das Dialogfeld **Zertifikat Fehler** angezeigt. Wenn Sie dies tun, überprüfen Sie, ob der Servername den Erwartungen entspricht. Wenn dies der Fall ist, wählen Sie **dieses Zertifikat in zukünftigen Sitzungen von Visual Studio speichern** aus, und klicken Sie auf **annehmen**. (Dieser Fehler weist darauf hin, dass der Hostinganbieter die Kosten für den Erwerb eines SSL-Zertifikats für die URL, für die Sie die Bereitstellung bereitstellen, vermieden hat. Wenn Sie eine sichere Verbindung mit einem gültigen Zertifikat herstellen möchten, wenden Sie sich an Ihren Hostinganbieter.)

![Zertifikat Fehler](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Klicken Sie auf **Weiter**.

Geben Sie auf der Registerkarte **Einstellungen** im Abschnitt **Datenbanken** dieselben Werte ein, die Sie für das Profil für die Test Veröffentlichung eingegeben haben. Sie finden die Verbindungs Zeichenfolgen, die Sie benötigen, in den Dropdown Listen.

- Wählen Sie im Feld Verbindungs Zeichenfolge für **schoolContext** `Data Source=|DataDirectory|School-Prod.sdf`
- Wählen Sie unter **schoolContext**die Option **Apply Code First-Migrationen**aus.
- Wählen Sie im Feld Verbindungs Zeichenfolge für **DefaultConnection**`Data Source=|DataDirectory|aspnet-Prod.sdf`
- Lassen Sie unter **DefaultConnection**die Option **Datenbank aktualisieren** deaktiviert.

![Registerkarte "Einstellungen" im Web veröffentlichen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Klicken Sie auf **Weiter**.

Klicken Sie auf der Registerkarte **Vorschau** auf **Vorschau starten** , um eine Liste der Dateien anzuzeigen, die kopiert werden. Die gleiche Liste wird angezeigt, die Sie zuvor bei der Bereitstellung in IIS auf dem lokalen Computer gesehen haben.

Bevor Sie veröffentlichen, ändern Sie den Namen des Profils, damit Ihre Web. Production. config-Transformations Datei angewendet wird. Wählen Sie die Registerkarte **Profil** , und klicken Sie auf **Profile verwalten**.

![Webassistent zum Veröffentlichen von Profilen verwalten](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

Wählen Sie im Dialogfeld **Webveröffentlichungs Profile bearbeiten** das Produktionsprofil aus, klicken Sie auf **Umbenennen**, und ändern Sie den Profilnamen in Production. Klicken Sie dann auf **Schließen**.

![Webveröffentlichungs Profile bearbeiten (Dialogfeld)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Klicken Sie auf **Veröffentlichen**.

Die Anwendung wird für den Hostinganbieter veröffentlicht. Das Ergebnis wird im **Ausgabe** Fenster angezeigt.

![Ausgabefenster nach der Bereitstellung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Der Browser öffnet automatisch die URL, die Sie in das Feld **Ziel-URL** auf der Registerkarte **Verbindung** des Assistenten **Web veröffentlichen** eingegeben haben. Sie sehen dieselbe Startseite wie beim Ausführen der Website in Visual Studio, mit dem Unterschied, dass in der Titelleiste kein "(Test)" oder "(dev)"-Umgebungs Indikator vorhanden ist. Dies gibt an, dass die *Web. config* -Transformation für Umgebungs Indikatoren ordnungsgemäß funktioniert hat.

> [!NOTE]
> Wenn in der Überschrift weiterhin "(Test)" angezeigt wird, löschen Sie den Ordner " *obj* " aus dem Projekt conjesouniversity, und stellen Sie erneut bereit. In vorab Versionen der Software kann die zuvor angewendete Transformations Datei (Web. Test. config) erneut angewendet werden, obwohl Sie das Produktionsprofil verwenden.

[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Stellen Sie vor dem Ausführen einer Seite, die den Datenbankzugriff verursacht, sicher, dass ELMAH alle auftretenden Fehler protokollieren kann.

## <a name="setting-folder-permissions-for-elmah"></a>Festlegen der Ordner Berechtigungen für ELMAH

Wie Sie sich aus dem vorherigen Tutorial dieser Reihe merken, müssen Sie sicherstellen, dass die Anwendung über Schreibberechtigungen für den Ordner in Ihrer Anwendung verfügt, in dem ELMAH Fehlerprotokoll Dateien speichert. Wenn Sie lokal auf Ihrem Computer in IIS bereitgestellt haben, legen Sie diese Berechtigungen manuell fest. In diesem Abschnitt erfahren Sie, wie Sie Berechtigungen für cytanium festlegen. (Einige Hostinganbieter können dies möglicherweise nicht tun. Sie bieten möglicherweise einen oder mehrere vordefinierte Ordner mit Schreibberechtigungen. In diesem Fall müssen Sie die Anwendung so ändern, dass Sie die angegebenen Ordner verwendet.)

Sie können Ordner Berechtigungen in der Systemsteuerung von cytanium festlegen. Wechseln Sie zur System Steuerungs-URL, und wählen Sie **Datei-Manager**aus.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

Wählen Sie im Feld **Datei-Manager** die Option **contosouniversity.com** und dann **wwwroot** aus, um den Stamm Ordner der Anwendung anzuzeigen. Klicken Sie auf das Vorhängeschloss-Symbol neben **ELMAH**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

Aktivieren Sie im Fenster **Datei**/**Ordner Berechtigungen** die Kontrollkästchen **Lesen** und **Schreiben** für **contosouniversity.com** , und klicken Sie auf **Berechtigungen festlegen**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Stellen Sie sicher, dass ELMAH über Schreibzugriff auf den Ordner *ELMAH* verfügt, indem Sie einen Fehler verursachen und dann den ELMAH-Fehlerbericht anzeigen. Fordern Sie eine ungültige URL wie " *studentsxxx. aspx*" an. Wie zuvor sehen Sie die Seite " *GenericErrorPage. aspx* ". Klicken Sie auf den Link **Abmelden** , und führen Sie dann *ELMAH. axd*aus. Sie erhalten zuerst die **Anmelde** Seite, die überprüft, ob die *Web. config* -Transformation die ELMAH-Autorisierung erfolgreich hinzugefügt hat. Nachdem Sie sich angemeldet haben, wird der Bericht mit dem Fehler angezeigt, den Sie soeben verursacht haben.

[![ELMAH. axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Testen in der Produktionsumgebung

Führen Sie die Seite **Studenten** aus. Die Anwendung versucht zum ersten Mal auf die Datenbank "School" zuzugreifen, wodurch Code First-Migrationen ausgelöst wird, um die Datenbank zu erstellen. Wenn die Seite nach einer Verzögerung angezeigt wird, zeigt Sie an, dass keine Studenten vorhanden sind.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Führen Sie die Seite **Dozenten** aus, um zu überprüfen, ob die Startdaten erfolgreich Dozenten Daten in die Datenbank eingefügt haben.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Wie Sie in der Testumgebung durchgeführt haben, sollten Sie überprüfen, ob Datenbankupdates in der Produktionsumgebung funktionieren, aber Sie möchten in der Regel keine Testdaten in die Produktionsdatenbank eingeben. In diesem Tutorial verwenden Sie dieselbe Methode wie beim testen. In einer realen Anwendung sollten Sie jedoch eine Methode suchen, mit der überprüft wird, ob Datenbankaktualisierungen erfolgreich sind, ohne dass Testdaten in die Produktionsdatenbank eingeführt werden. In einigen Anwendungen kann es sinnvoll sein, etwas hinzuzufügen und es dann zu löschen.

Fügen Sie einen Studenten hinzu, und zeigen Sie dann die Daten an, die Sie auf der Seite **Studenten** eingegeben haben, um sicherzustellen, dass Sie Daten in der Datenbank aktualisieren

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Überprüfen Sie, ob die Autorisierungs Regeln ordnungsgemäß funktionieren, indem Sie im Menü **Kurse** die Option **Guthaben aktualisieren** auswählen Die **Anmelde** Seite wird angezeigt. Geben Sie die Anmelde Informationen für das Administrator Konto ein, klicken Sie auf **Anmelden**, und die Seite **Update Guthaben** wird angezeigt.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Wenn die Anmeldung erfolgreich ist, wird die Seite **Guthaben aktualisieren** angezeigt. Dies gibt an, dass die ASP.net-Mitgliedschafts Datenbank (mit dem Konto mit einem Administrator) erfolgreich bereitgestellt wurde.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Sie haben Ihre Website nun erfolgreich bereitgestellt und getestet und sind öffentlich über das Internet verfügbar.

## <a name="creating-a-more-reliable-test-environment"></a>Erstellen einer zuverlässigeren Test Umgebung

Wie im Tutorial bereitstellen [der Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) erläutert, wäre die zuverlässigste Test Umgebung ein zweites Konto des hostinganbieters, der genau wie das Produktions Konto ist. Dies wäre teurer als die Verwendung der lokalen IIS als Testumgebung, da Sie sich für ein zweites Hostingkonto registrieren müssen. Wenn jedoch Fehler oder Ausfälle von Produktions Websites verhindert werden, können Sie sich entscheiden, dass die Kosten anfallen.

Der meiste Prozess zum Erstellen und Bereitstellen in einem Testkonto ähnelt dem, was Sie bereits für die Bereitstellung in der Produktion getan haben:

- Erstellen Sie eine *Web. config* -Transformations Datei.
- Erstellen Sie ein Konto beim Hostinganbieter.
- Erstellen Sie ein neues Veröffentlichungs Profil, und stellen Sie es für das Testkonto bereit.

### <a name="preventing-public-access-to-the-test-site"></a>Verhindern des öffentlichen Zugriffs auf die Test Website

Eine wichtige Überlegung für das Testkonto besteht darin, dass es im Internet Live ist, aber nicht von der Öffentlichkeit verwendet werden soll. Um den Standort privat zu halten, können Sie eine oder mehrere der folgenden Methoden verwenden:

- Wenden Sie sich an den Hostinganbieter, um Firewallregeln festzulegen, die nur von IP-Adressen, die Sie zum Testen verwenden, Zugriff auf die Test Website
- Verbergen der URL, sodass Sie nicht mit der URL der öffentlichen Website vergleichbar ist.
- Verwenden Sie die Datei " *robots. txt* ", um sicherzustellen, dass Suchmaschinen die Test Website nicht durchforsten und in den Suchergebnissen Links zu dieser Datei melden.

Die erste dieser Methoden ist offensichtlich die sicherste, aber das Verfahren für, das für jeden Hostinganbieter spezifisch ist und in diesem Tutorial nicht behandelt wird. Wenn Sie mit Ihrem Hostinganbieter anordnen, dass nur Ihre IP-Adresse zur Test Konto-URL navigieren soll, müssen Sie sich theoretisch keine Gedanken über Such Module machen. Aber auch in diesem Fall ist die Bereitstellung einer Datei " *robots. txt* " eine gute Idee als eine Sicherung, falls die Firewallregel versehentlich ausgeschaltet wird.

Die Datei " *robots. txt* " wechselt in Ihren Projektordner und sollte den folgenden Text enthalten:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

Die `User-agent` Zeile weist Suchmaschinen an, dass sich die Regeln in der Datei auf alle Suchmaschinen-Webcrawlers (Robots) beziehen, und die `Disallow` Linie gibt an, dass keine Seiten auf der Website durchlaufen werden sollen.

Wahrscheinlich möchten Sie, dass Suchmaschinen Ihren Produktionsstandort katalogisieren, sodass Sie diese Datei aus der Produktions Bereitstellung ausschließen müssen. Informationen hierzu finden Sie unter **kann ich bestimmte Dateien oder Ordner von der Bereitstellung ausschließen?** in den [FAQ zu ASP.NET-Webanwendungs-Projekt Bereitstellung](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Stellen Sie sicher, dass Sie den Ausschluss nur für das Produktions Veröffentlichungs Profil angeben.

Das Erstellen eines zweiten hostingkontos ist ein Ansatz zum Arbeiten mit einer Testumgebung, die nicht erforderlich ist, aber möglicherweise die zusätzlichen Kosten erhöht. In den folgenden Tutorials verwenden Sie weiterhin IIS als Testumgebung.

Im nächsten Tutorial aktualisieren Sie den Anwendungscode und stellen die Änderungen in den Test-und Produktionsumgebungen bereit.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
