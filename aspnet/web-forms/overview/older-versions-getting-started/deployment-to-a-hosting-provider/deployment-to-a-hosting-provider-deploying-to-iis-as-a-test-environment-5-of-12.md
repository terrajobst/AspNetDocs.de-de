---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen in IIS als Test Umgebung-5 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5d85232ff2cb229d771d517db7173721c9e277bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515679"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen in IIS als Test Umgebung-5 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht

In diesem Tutorial wird gezeigt, wie eine ASP.NET-Webanwendung in IIS auf dem lokalen Computer bereitgestellt wird.

Wenn Sie eine Anwendung entwickeln, testen Sie Sie in der Regel, indem Sie Sie in Visual Studio ausführen. Standardmäßig bedeutet dies, dass Sie die Visual Studio Development Server (auch als Cassini bezeichnet) verwenden. Der Visual Studio Development Server erleichtert das Testen während der Entwicklung in Visual Studio, funktioniert aber nicht genauso wie IIS. Daher ist es möglich, dass eine Anwendung beim Testen in Visual Studio ordnungsgemäß ausgeführt wird, aber fehlschlägt, wenn Sie in IIS in einer Hostingumgebung bereitgestellt wird.

Sie können Ihre Anwendung auf folgende Weise zuverlässiger testen:

1. Verwenden Sie IIS Express oder vollständige IIS anstelle des Visual Studio Development Server, wenn Sie während der Entwicklung in Visual Studio testen. Diese Methode emuliert in der Regel genauer, wie Ihre Website unter IIS ausgeführt wird. Mit dieser Methode wird der Bereitstellungs Prozess jedoch nicht getestet, oder es wird nicht überprüft, ob das Ergebnis des Bereitstellungs Prozesses ordnungsgemäß ausgeführt wird.
2. Stellen Sie die Anwendung auf dem Entwicklungs Computer auf IIS bereit. verwenden Sie dazu den gleichen Prozess, den Sie später verwenden, um Sie in Ihrer Produktionsumgebung bereitzustellen. Mit dieser Methode wird der Bereitstellungs Prozess zusätzlich überprüft, ob die Anwendung unter IIS ordnungsgemäß ausgeführt wird.
3. Stellen Sie die Anwendung in einer Testumgebung bereit, die so nah wie möglich an Ihrer Produktionsumgebung ist. Da es sich bei der Produktionsumgebung für diese Tutorials um einen Drittanbieter-Hostinganbieter handelt, wäre die ideale Testumgebung ein zweites Konto mit dem Hostinganbieter. Sie sollten dieses zweite Konto nur für Tests verwenden, aber es würde auf die gleiche Weise wie das Produktions Konto eingerichtet werden.

In diesem Tutorial werden die Schritte für Option 2 angezeigt. Eine Anleitung für Option 3 finden Sie am Ende des Tutorials [zur Bereitstellung in der Produktionsumgebung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) , und am Ende dieses Tutorials finden Sie Links zu Ressourcen für Option 1.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Konfigurieren der Anwendung für die Verwendung mit mittlerer Vertrauensstellung

Vor der Installation von IIS und deren Bereitstellung ändern Sie die Einstellung der Datei "Web. config", um die Website so zu gestalten, wie Sie in einer typischen freigegebenen Hostingumgebung ausgeführt wird.

Hostinganbieter führen Ihre Website in der Regel mit *mittlerer Vertrauens*Würdigkeit aus. Dies bedeutet, dass einige Dinge nicht zulässig sind. Beispielsweise kann der Anwendungscode nicht auf die Windows-Registrierung zugreifen und keine Dateien lesen und schreiben, die sich außerhalb der Ordnerhierarchie Ihrer Anwendung befinden. Standardmäßig wird Ihre Anwendung mit *hoher Vertrauens* Würdigkeit auf dem lokalen Computer ausgeführt, was bedeutet, dass die Anwendung möglicherweise Dinge durchführt, die bei der Bereitstellung in der Produktion fehlschlagen würden. Damit die Testumgebung die Produktionsumgebung genauer widerspiegelt, konfigurieren Sie die Anwendung so, dass Sie mit mittlerer Vertrauenswürdigkeit ausgeführt wird.

Fügen Sie in der Datei "Web. config" der Anwendung ein **Trust** -Element im **System. Web** -Element hinzu, wie im folgenden Beispiel gezeigt.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Die Anwendung wird jetzt in IIS mit mittlerer Vertrauenswürdigkeit auch auf dem lokalen Computer ausgeführt. Diese Einstellung ermöglicht es Ihnen, so früh wie möglich beliebige Versuche des Anwendungs Codes abzufangen, um etwas zu tun, das in der Produktion fehlschlagen würde.

> [!NOTE]
> Wenn Sie Entity Framework Code First-Migrationen verwenden, stellen Sie sicher, dass Sie Version 5,0 oder höher installiert haben. In Entity Framework Version 4,3 ist für Migrationen volle Vertrauenswürdigkeit erforderlich, um das Datenbankschema zu aktualisieren.

## <a name="installing-iis-and-web-deploy"></a>Installieren von IIS und Web deploy

Um auf dem Entwicklungs Computer in IIS bereitzustellen, müssen IIS und Web deploy installiert sein. Diese sind nicht in der Standardkonfiguration von Windows 7 enthalten. Wenn Sie bereits IIS und Web deploy installiert haben, fahren Sie mit dem nächsten Abschnitt fort.

Die Verwendung des [Webplattform-Installers](https://www.microsoft.com/web/downloads/platform.aspx) ist die bevorzugte Methode zum Installieren von IIS und Web deploy, da der Webplattform-Installer eine empfohlene Konfiguration für IIS installiert und bei Bedarf automatisch die Voraussetzungen für IIS und Web deploy installiert.

Verwenden Sie den folgenden Link, um den Webplattform-Installer zum Installieren von IIS und Web deploy auszuführen. Wenn Sie IIS bereits installiert haben, Web deploy oder eine der erforderlichen Komponenten, installiert der Webplattform-Installer nur das, was fehlt.

- [Installieren von IIS und Web deploy mithilfe von webpi](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Festlegen des Standard Anwendungs Pools auf .NET 4

Führen Sie nach der Installation von IIS den **IIS-Manager** aus, um sicherzustellen, dass der .NET Framework Version 4 dem Standard Anwendungs Pool zugewiesen ist.

Klicken Sie im Windows- **Startmenü** auf **Ausführen**, geben Sie "inetmgr" ein, und klicken Sie dann auf **OK**. (Wenn sich der Befehl **Ausführen** nicht im **Startmenü** befindet, können Sie die Windows-Taste und R drücken, um Sie zu öffnen. Oder klicken Sie mit der rechten Maustaste auf die Taskleiste, klicken Sie auf **Eigenschaften**, wählen Sie die Registerkarte **Start** , klicken Sie auf **Anpassen**, und wählen Sie dann **Befehl ausführen**

Erweitern Sie im Bereich **Verbindungen** den Server Knoten, und wählen Sie **Anwendungs Pools**aus. Wenn **DefaultAppPool** der .NET Framework-Version 4 wie in der folgenden Abbildung zugewiesen ist, können Sie im Bereich **Anwendungs Pools** mit dem nächsten Abschnitt fortfahren.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Wenn nur zwei Anwendungs Pools angezeigt werden und beide auf den .NET Framework 2,0 festgelegt sind, müssen Sie ASP.NET 4 in IIS installieren:

- Öffnen Sie ein Eingabe Aufforderungs Fenster, indem Sie im Windows- **Startmenü** mit der rechten Maustaste auf **Eingabeaufforderung** klicken und **als Administrator ausführen**auswählen. Führen Sie dann [ASPNET\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) aus, um ASP.NET 4 in IIS zu installieren. verwenden Sie dazu die folgenden Befehle. (Ersetzen Sie in 64-Bit-Systemen "Framework" durch "Framework64".)

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP. NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Mit diesem Befehl werden neue Anwendungs Pools für die .NET Framework 4 erstellt, der Standard Anwendungs Pool wird jedoch weiterhin auf 2,0 festgelegt. Sie stellen eine Anwendung bereit, die .NET 4 für diesen Anwendungs Pool als Ziel hat, sodass Sie den Anwendungs Pool in .NET 4 ändern müssen.

Wenn Sie **IIS-Manager**geschlossen haben, führen Sie ihn erneut aus, erweitern Sie den Server Knoten, und klicken Sie auf **Anwendungs Pools** , um den Bereich **Anwendungs Pools** erneut anzuzeigen.

Klicken Sie im Bereich **Anwendungs Pools** auf **DefaultAppPool**, und klicken Sie dann im Bereich **Aktionen** auf **Grundeinstellungen**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

Ändern Sie im Dialogfeld **Anwendungs Pool bearbeiten** **.NET Framework Version** in **.NET Framework v-4.0.30319** , und klicken Sie auf **OK**.

[![Selecting_. NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Sie können jetzt in IIS veröffentlichen.

## <a name="publishing-to-iis"></a>Veröffentlichen in IIS

Es gibt mehrere Möglichkeiten, wie Sie mit Visual Studio 2010 und Web deploy bereitstellen können:

- Verwenden Sie Visual Studio One-Click-Veröffentlichung.
- Erstellen Sie ein *Bereitstellungs Paket* , und installieren Sie es mithilfe der IIS-Manager-Benutzeroberfläche Das Bereitstellungs Paket besteht aus einer *ZIP* -Datei, die alle Dateien und Metadaten enthält, die zum Installieren eines Standorts in IIS erforderlich sind.
- Erstellen Sie ein Bereitstellungs Paket, und installieren Sie es mithilfe der Befehlszeile.

Der Prozess, den Sie in den vorherigen Tutorials durchlaufen haben, um Visual Studio für die Automatisierung von Bereitstellungs Aufgaben einzurichten, gilt für alle diese drei Methoden. In diesen Tutorials verwenden Sie die erste dieser Methoden. Weitere Informationen zur Verwendung von Bereitstellungs Paketen finden Sie unter [ASP.net Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx).

Vergewissern Sie sich vor dem veröffentlichen, dass Sie Visual Studio im Administrator Modus ausführen. (Klicken Sie im **Startmenü** von Windows 7 mit der rechten Maustaste auf das Symbol für die Version von Visual Studio, die Sie verwenden, und wählen Sie **als Administrator ausführen**aus.) Der Administrator Modus ist nur für die Veröffentlichung erforderlich, wenn Sie auf dem lokalen Computer auf IIS veröffentlichen.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt conjesouniversity (nicht auf das Projekt Conto souniversity. DAL), und wählen Sie **veröffentlichen**aus.

Der Assistent **Web veröffentlichen** wird geöffnet.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Wählen Sie in der Dropdown Liste **&lt;neu...&gt;** aus.

Geben Sie im Dialogfeld **Neues Profil** "Test" ein, und klicken Sie dann auf **OK**.

![Dialogfeld "Neues Profil"](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Dieser Name ist identisch mit dem mittleren Knoten in der Transformations Datei Web. Test. config, die Sie zuvor erstellt haben. Diese Entsprechung bewirkt, dass die Web. Test. config-Transformationen angewendet werden, wenn Sie diese mithilfe dieses Profils veröffentlichen.

Der Assistent wechselt automatisch zur Registerkarte **Verbindung** .

Geben Sie im Feld **Dienst-URL** den *Namen localhost*ein.

Geben Sie im Feld **Website/Anwendung** den Eintrag *Default Web Site/Conto souniversity*ein.

Geben Sie im Feld **Ziel-URL** `http://localhost/ContosoUniversity`ein.

Die Einstellung für die **Ziel-URL** ist nicht erforderlich. Wenn Visual Studio die Bereitstellung der Anwendung abgeschlossen hat, öffnet sie automatisch Ihren Standardbrowser mit dieser URL. Wenn Sie nicht möchten, dass der Browser nach der Bereitstellung automatisch geöffnet wird, lassen Sie dieses Feld leer.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Klicken Sie auf **Verbindung** überprüfen, um sicherzustellen, dass die Einstellungen richtig sind und Sie eine Verbindung mit IIS auf dem lokalen Computer herstellen können.

Ein grünes Häkchen überprüft, ob die Verbindung erfolgreich hergestellt wurde.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Klicken Sie auf **weiter** , um zur Registerkarte **Einstellungen** zu gelangen.

Das Dropdown Feld **Konfiguration** gibt die Buildkonfiguration an, die bereitgestellt werden soll. Der Standardwert ist Release, was Sie möchten.

Lassen Sie das Kontrollkästchen **zusätzliche Dateien am Ziel entfernen** deaktiviert. Da es sich hierbei um die erste Bereitstellung handelt, sind noch keine Dateien im Zielordner vorhanden.

Geben Sie im Abschnitt **Datenbanken** den folgenden Wert in das Feld Verbindungs Zeichenfolge für **schoolContext**ein:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Der Bereitstellungs Prozess fügt diese Verbindungs Zeichenfolge in die bereitgestellte Web. config-Datei ein, da **Diese Verbindungs Zeichenfolge zur Laufzeit verwenden** ausgewählt ist.

Wählen Sie unter " **schoolContext**" die Option **Code First-Migrationen anwenden**aus. Diese Option bewirkt, dass der Bereitstellungs Prozess die bereitgestellte Web. config-Datei konfiguriert, um den `MigrateDatabaseToLatestVersion` Initialisierer anzugeben. Dieser Initialisierer aktualisiert die Datenbank automatisch auf die neueste Version, wenn die Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift.

Geben Sie im Feld Verbindungs Zeichenfolge für **DefaultConnection**den folgenden Wert ein:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Lassen Sie **Datenbank aktualisieren** deaktiviert. Die Mitgliedschafts Datenbank wird bereitgestellt, indem die SDF-Datei in App-\_Daten kopiert wird, und Sie möchten nicht, dass der Bereitstellungs Prozess nichts anderes mit dieser Datenbank durchführt.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Klicken Sie auf **weiter** , um zur Registerkarte **Vorschau** zu gelangen.

Klicken Sie auf der Registerkarte **Vorschau** auf **Vorschau starten** , um eine Liste der Dateien anzuzeigen, die kopiert werden.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Klicken Sie auf **Veröffentlichen**.

Wenn Visual Studio nicht im Administrator Modus ist, erhalten Sie möglicherweise eine Fehlermeldung, die auf einen Berechtigungs Fehler hinweist. Schließen Sie in diesem Fall Visual Studio, öffnen Sie es im Administrator Modus, und versuchen Sie erneut, zu veröffentlichen.

Wenn sich Visual Studio im Administrator Modus befindet, meldet das **Ausgabe** Fenster den erfolgreichen Build und die Veröffentlichung.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Der Browser wird automatisch auf der Startseite der lokalen Website in IIS geöffnet, die auf dem lokalen Computer in IIS ausgeführt wird.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Testen in der Test Umgebung

Beachten Sie, dass der Umgebungs Indikator "(Test)" anstelle von "(dev)" anzeigt, was anzeigt, dass die *Web. config* -Transformation für den Umgebungs Indikator erfolgreich war.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Führen Sie die Seite **Studenten** aus, um sicherzustellen, dass die bereitgestellte Datenbank keine Studenten enthält. Wenn Sie diese Seite auswählen, kann der Ladevorgang einige Minuten dauern, da Code First die Datenbank erstellt und dann die `Seed`-Methode ausführt. (Dies ist nicht der Fall, wenn Sie auf der Startseite waren, weil die Anwendung noch nicht versucht hat, auf die Datenbank zuzugreifen.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Führen Sie die Seite **Dozenten** aus, um zu überprüfen, ob die Datenbank mit den Dozenten Daten Code First:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Wählen Sie im Menü " **Students** " die Option **Studenten hinzufügen** aus, fügen Sie einen Studenten hinzu, und zeigen Sie dann die neue Student auf der Seite **Studenten** an, um sicherzustellen, dass Sie erfolgreich in die Datenbank schreiben

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Wählen Sie im Menü **Kurse** die Option **Guthaben aktualisieren**aus. Die Seite " **Guthaben aktualisieren** " erfordert Administrator Berechtigungen, sodass die **Anmelde** Seite angezeigt wird. Geben Sie die Anmelde Informationen für das Administrator Konto ein, die Sie zuvor erstellt haben ("admin" und "Pas $ w0rd"). Die Seite " **Guthaben aktualisieren** " wird angezeigt, die überprüft, ob das im vorherigen Tutorial erstellte Administrator Konto ordnungsgemäß in der Testumgebung bereitgestellt wurde.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Vergewissern Sie sich, dass ein *ELMAH* -Ordner mit nur der Platzhalter Datei vorhanden ist.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Überprüfen der automatischen Änderungen an "Web. config" für Code First-Migrationen

Öffnen Sie die Datei " *Web. config* " in der bereitgestellten Anwendung unter " *c:\inetpub\wwwroot\contosouniversity* ". Sie können sehen, wo der von Ihnen konfigurierte Bereitstellungs Prozess Code First-Migrationen, um die Datenbank automatisch auf die neueste Version zu aktualisieren.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Beim Bereitstellungs Prozess wurde außerdem eine neue Verbindungs Zeichenfolge für Code First-Migrationen erstellt, die ausschließlich zum Aktualisieren des Datenbankschemas verwendet wird:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Mit dieser zusätzlichen Verbindungs Zeichenfolge können Sie ein Benutzerkonto für Datenbankschema Updates und ein anderes Benutzerkonto für den Zugriff auf Anwendungsdaten angeben. Beispielsweise können Sie der Anwendung die Rolle "DB\_Owner" Code First-Migrationen-und DB-\_DataReader-und DB-\_-DataWriter-Rollen zuweisen. Dies ist ein gängiges tiefgreifendes Verteidigungs Muster, das verhindert, dass potenziell bösartiger Code in der Anwendung das Datenbankschema ändert. (Dies kann beispielsweise bei einem erfolgreichen SQL Injection-Angriff vorkommen.) Dieses Muster wird von diesen Tutorials nicht verwendet. Dies gilt nicht für SQL Server Compact und gilt nicht für die Migration zu SQL Server in einem späteren Tutorial dieser Reihe. Die cytanium-Site bietet nur ein Benutzerkonto für den Zugriff auf die SQL Server Datenbank, die Sie unter cytanium erstellen. Wenn Sie dieses Muster in Ihrem Szenario implementieren können, führen Sie die folgenden Schritte aus:

1. Geben Sie auf der Registerkarte **Einstellungen** des Assistenten **Web veröffentlichen** die Verbindungs Zeichenfolge ein, die einen Benutzer mit vollständigen Datenbankschema-Aktualisierungs Berechtigungen angibt, und deaktivieren Sie das Kontrollkästchen **Diese Verbindungs Zeichenfolge zur Laufzeit verwenden** . In der bereitgestellten Web. config-Datei wird dies die `DatabasePublish` Verbindungs Zeichenfolge.
2. Erstellen Sie eine Transformation für die Datei "Web. config" für die Verbindungs Zeichenfolge, die von der Anwendung zur Laufzeit verwendet werden soll.

Sie haben Ihre Anwendung nun auf dem Entwicklungs Computer auf IIS bereitgestellt und dort getestet. Dadurch wird sichergestellt, dass der Inhalt der Anwendung vom Bereitstellungs Prozess an den richtigen Speicherort kopiert wurde (ausgenommen der Dateien, die nicht bereitgestellt werden sollen) und dass IIS während der Bereitstellung ordnungsgemäß konfiguriert Web Deploy. Im nächsten Tutorial führen Sie einen weiteren Test aus, der einen Bereitstellungs Task findet, der noch nicht ausgeführt wurde: das Festlegen der Ordner Berechtigungen für den Ordner *ELMAH* .

## <a name="more-information"></a>Weitere Informationen

Informationen zum Ausführen von IIS oder IIS Express in Visual Studio finden Sie in den folgenden Ressourcen:

- [IIS Express Übersicht](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) auf der IIS.NET-Website.
- [Einführung in IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) von Scott Guthrie Blog.
- Gewusst [wie: Angeben des Webservers für Webprojekte in Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx)
- [Grundlegende Unterschiede zwischen IIS und der ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) auf der ASP.NET-Website.
- [Testen Sie Ihre ASP.NET MVC-oder Web Forms-Anwendung auf IIS 7 innerhalb von 30 Sekunden](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) im Blog von Rick Anderson. Dieser Eintrag enthält Beispiele dafür, warum das Testen mit dem Visual Studio Development Server (Cassini) nicht so zuverlässig ist wie das Testen in IIS Express und warum das Testen in IIS Express nicht so zuverlässig ist wie das Testen in IIS.

Informationen zu den Problemen, die auftreten können, wenn Ihre Anwendung mit mittlerer Vertrauenswürdigkeit ausgeführt wird, finden Sie unter [Hosting ASP.NET Applications in Mittel Trust](http://www.4guysfromrolla.com/articles/100307-1.aspx) auf The 4 guys from Rolla Site.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
