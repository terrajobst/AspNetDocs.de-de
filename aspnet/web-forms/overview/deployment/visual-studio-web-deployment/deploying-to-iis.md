---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellung für Test | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: c45003325832258466a787bc589bf40e844248a2
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985857"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellung zum Testen

von [Tom Dykstra](https://github.com/tdykstra)

Diese tutorialreihe zeigt, wie Sie eine ASP.NET-Webanwendung mithilfe von Visual Studio 2017 für die Azure App Service von Web-Apps oder eines Drittanbieter-hostinganbieters bereitstellen (veröffentlichen). Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

Eine aktuelle Version der Bereitstellung in Azure finden Sie unter [Erstellen einer ASP.net Core-Web-App in Azure](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="overview"></a>Übersicht

In diesem Tutorial stellen Sie eine ASP.NET-Webanwendung für Internet Information Server (IIS) auf dem lokalen Computer bereit.

Wenn Sie eine Anwendung entwickeln, führen Sie Sie in der Regel aus, und testen Sie Sie in Visual Studio. Standardmäßig verwenden Webanwendungs Projekte in Visual Studio 2017 IIS Express als entwicklungsweb Server. IIS Express verhält sich ähnlich wie die vollständige IIS als die Visual Studio Development Server (auch als Cassini bezeichnet), die Visual Studio 2017 standardmäßig verwendet. Aber keiner der Entwicklungs-Webserver funktioniert genau wie IIS. Folglich könnte eine app in Visual Studio ordnungsgemäß ausgeführt und getestet werden, schlägt jedoch fehl, wenn Sie in IIS bereitgestellt wird.

Sie können Ihre Anwendung auf zwei Arten zuverlässig testen:

1. Stellen Sie die Anwendung auf Ihrem Entwicklungs Computer auf IIS bereit, und verwenden Sie dabei den gleichen Prozess, den Sie später für die Bereitstellung in Ihrer Produktionsumgebung verwenden.

   Sie können Visual Studio so konfigurieren, dass IIS verwendet wird, wenn Sie ein Webprojekt ausführen, aber das würde den Bereitstellungs Prozess nicht testen. Mit dieser Methode wird der Bereitstellungs Prozess überprüft und die Anwendung unter IIS ordnungsgemäß ausgeführt.

2. Stellen Sie Ihre Anwendung in einer Testumgebung bereit, ähnlich der Produktionsumgebung. 
 
   Die Produktionsumgebung für diese Tutorials ist Web-Apps in Azure App Service. Die ideale Testumgebung ist eine zusätzliche Web-App, die im Azure-Dienst erstellt wird. Obwohl Sie auf dieselbe Weise wie eine Produktions-Web-App eingerichtet wird, würden Sie Sie nur für Testzwecke verwenden.

Option 2 ist die zuverlässigste Methode zum Testen. Wenn Sie Option 2 verwenden, müssen Sie nicht unbedingt Option 1 verwenden. Wenn Sie jedoch für einen Drittanbieter-Hosting-Anbieter bereitstellen, ist Option 2 möglicherweise nicht möglich oder teuer, sodass in dieser tutorialreihe beide Methoden angezeigt werden. Eine Anleitung für Option 2 finden Sie im Tutorial bereitstellen [in der Produktionsumgebung](deploying-to-production.md) .

Weitere Informationen zur Verwendung von Webservern in Visual Studio finden Sie unter [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, überprüfen Sie die [Problem Behandlungs Seite](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Herunterladen des Projekts "" der Projekt "" der "

Laden Sie die Visual Studio-Startprojekt Mappe und das Projekt für die Projekt Mappe von Visual Studio herunter Diese Lösung enthält das abgeschlossene Tutorial. 

[Starter Projekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Installieren von IIS

Vergewissern Sie sich, dass IIS und Web deploy installiert sind, um Sie auf dem Entwicklungs Computer in IIS bereitzustellen. Standardmäßig installiert Visual Studio Web deploy, aber IIS ist nicht in der Standardkonfiguration von Windows 10, Windows 8 oder Windows 7 enthalten. Wenn Sie IIS bereits installiert haben und der Standard Anwendungs Pool bereits auf .NET 4 festgelegt ist, fahren Sie mit [dem nächsten Abschnitt](#sqlexpress)fort.

1. Es wird empfohlen, dass Sie den [Webplattform-Installer (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) zum Installieren von IIS und Web deploy verwenden. WPI installiert eine empfohlene IIS-Konfiguration, die IIS und ggf. Web deploy Voraussetzungen umfasst.

   Wenn Sie bereits IIS, Web deploy oder eine der erforderlichen Komponenten installiert haben, installiert der WPI nur das, was fehlt.

   * Verwenden Sie den Webplattform-Installer zum Installieren von IIS und Web Deploy:
    
     ![Installieren von IIS mit WPI](deploying-to-iis/_static/image30.png)

     ![Installieren von Web deploy mithilfe von WPI](deploying-to-iis/_static/image31.png)

     Es werden Meldungen angezeigt, die angeben, dass IIS 7 installiert wird. Der Link funktioniert für IIS 8 unter Windows 8. für Windows 8 und höher müssen Sie jedoch die folgenden Schritte durchführen, um sicherzustellen, dass ASP.NET 4,7 installiert ist:

   * Öffnen Sie die **Systemsteuerung** > **Programme** > **Programme und Funktionen** , > aktivieren **oder deaktivieren Sie Windows-Funktionen**.

   * Erweitern Sie **Internetinformationsdienste**, **World Wide Web Dienste**und **Features für die Anwendungsentwicklung**.
   
   * Vergewissern Sie sich, dass **ASP.NET 4,7** ausgewählt ist.

     ![SELECT ASP.NET 4,7](deploying-to-iis/_static/image1a.png)

   * Vergewissern Sie sich, dass **World Wide Web Dienste** und die **IIS-Verwaltungskonsole** ausgewählt sind. Hierdurch werden IIS und IIS-Manager installiert.
    
     ![World Wide Web Dienste auswählen](deploying-to-iis/_static/image24.png)    
  
   * Klicken Sie auf **OK**. Dialog Feld Meldungen mit dem Hinweis, dass die Installation stattfindet.

Führen Sie nach der Installation von IIS den **IIS-Manager** aus, um sicherzustellen, dass der .NET Framework Version 4 dem Standard Anwendungs Pool zugewiesen ist.

1. Drücken Sie Windows + R, um das Dialogfeld **Ausführen** zu öffnen.

   (Geben Sie auf der **Start** Seite unter Windows 8 oder höher "ausführen" ein. Wählen Sie unter Windows 7 im **Startmenü** die Option **Ausführen** aus. Wenn **Ausführen** nicht im **Startmenü** angezeigt wird, klicken Sie mit der rechten Maustaste auf die Taskleiste, wählen Sie **Eigenschaften**aus, wählen Sie die Registerkarte **Start** , klicken Sie auf **Anpassen**, und wählen Sie dann **Befehl ausführen**.

2. Geben Sie "inetmgr" ein, und wählen Sie **OK**aus.

3. Erweitern Sie im Bereich **Verbindungen** den Server Knoten, und wählen Sie **Anwendungs Pools**aus. Wechseln Sie im Bereich **Anwendungs Pools** zum nächsten Abschnitt, wenn **DefaultAppPool** der .NET Framework-Version 4 zugewiesen ist, wie in der folgenden Abbildung dargestellt.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. Wenn nur zwei Anwendungs Pools angezeigt werden und beide auf .NET Framework 2,0 festgelegt sind, installieren Sie ASP.NET 4 in IIS.

   Informationen zu Windows 8 oder höher finden Sie in den Anweisungen im vorherigen Abschnitt, um sicherzustellen, dass ASP.NET 4,7 installiert ist, oder erfahren Sie, [wie Sie ASP.NET 4,5 unter Windows 8 und Windows Server 2012 installieren](https://support.microsoft.com/kb/2736284). Öffnen Sie für Windows 7 ein Eingabe Aufforderungs Fenster, indem Sie im Windows- **Startmenü** mit der rechten Maustaste auf **Eingabeaufforderung** klicken und **als Administrator ausführen**auswählen. Führen Sie [ASPNET\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) aus, um ASP.NET 4 mit den folgenden Befehlen in IIS zu installieren. (Ersetzen Sie in 32-Bit-Systemen "Framework64" durch "Framework".)

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   Mit diesem Befehl werden neue Anwendungs Pools für die .NET Framework 4 erstellt, aber der Standard Anwendungs Pool bleibt auf 2,0 festgelegt. Sie stellen eine Anwendung, die auf .NET 4 ausgerichtet ist, für diesen Anwendungs Pool bereit. ändern Sie also den Anwendungs Pool in .NET 4.

5. Wenn Sie **IIS-Manager**geschlossen haben, führen Sie ihn erneut aus, erweitern Sie den Server Knoten, und wählen Sie dann **Anwendungs Pools**aus.

6. Wählen Sie im Bereich **Anwendungs Pools** die Option **DefaultAppPool**aus. Wählen Sie im Bereich **Aktionen** die Option **Grundeinstellungen**aus.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. Ändern Sie im Dialogfeld **Anwendungs Pool bearbeiten** die **.NET CLR-Version** in **.NET CLR v 4.0.30319**. Klicken Sie auf **OK**.

   ![Selecting_. NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

Sie sind jetzt bereit, eine Webanwendung in IIS zu veröffentlichen. Erstellen Sie zunächst jedoch Datenbanken zum Testen.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Installieren von SQL Server Express

Localdb ist nicht für die Verwendung in IIS konzipiert, daher muss in Ihrer Testumgebung SQL Server Express installiert sein. Wenn Sie Visual Studio 2010 SQL Server Express verwenden, ist es bereits standardmäßig installiert. Wenn Sie Visual Studio 2012 oder höher verwenden, installieren Sie SQL Server Express.

Um SQL Server Express zu installieren, laden Sie es aus dem Download Center herunter, und installieren Sie es [: Microsoft SQL Server 2017 Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

Wählen Sie auf der ersten Seite des SQL Server-Installations Centers die **Option neu SQL Server eigenständige Installation aus, oder fügen Sie einer vorhandenen Installation Features hinzu** , und befolgen Sie die Anweisungen, um die Standardoptionen zu akzeptieren. Übernehmen Sie im Installations-Assistenten die Standardeinstellungen. Weitere Informationen zu Installationsoptionen finden Sie unter [Install SQL Server from the Installation Wizard (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Erstellen SQL Server Express Datenbanken für die Testumgebung

Die Anwendung "" der Anwendung "" der Anwendung "" der Anwendung " 

1. Mitgliedschafts Datenbank 
2. Anwendungsdatenbank 

Sie können diese Datenbanken in zwei separaten Datenbanken oder in einer einzelnen Datenbank bereitstellen. Durch das kombinieren werden die Daten bankjoins vereinfacht. 

Wenn Sie die Bereitstellung für einen Hosting-Anbieter eines Drittanbieters durchführen, gibt ihr Hostingplan möglicherweise auch einen Grund für die Kombination aus. Der Anbieter kann z. b. mehr für mehrere Datenbanken berechnen, oder er kann nicht einmal mehr als eine Datenbank zulassen.

In diesem Tutorial stellen Sie zwei Datenbanken in der Testumgebung und eine Datenbank in der Stagingumgebung und in der Produktionsumgebung bereit.

Wählen Sie in Visual Studio im Menü **Ansicht** die Option **Server-Explorer** (**Datenbank-Explorer** in Visual Web Developer). Klicken Sie mit der rechten Maustaste auf **Datenverbindungen** , und wählen Sie **neue SQL Server Datenbank erstellen**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

Geben Sie im Dialogfeld **neue SQL Server Datenbank erstellen** im Feld **Server Name** den Namen ".\sqlexpress" ein, und geben Sie im Feld **Neuer Datenbankname** den Wert ASPNET-contosouniversity ein. Klicken Sie auf **OK**.

![Erstellen von ASPNET-contosouniversity](deploying-to-iis/_static/image9.png)

Gehen Sie wie folgt vor, um eine neue SQL Server Express School-Datenbank mit dem Namen `ContosoUniversity`zu erstellen.

In **Server-Explorer** werden die beiden neuen Datenbanken angezeigt.

![Neue Datenbanken in Server-Explorer](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Erstellen eines Grant-Skripts für die neuen Datenbanken

Wenn die Anwendung in IIS auf dem Entwicklungs Computer ausgeführt wird, verwendet die Anwendung die Anmelde Informationen des Standard Anwendungs Pools für den Zugriff auf die Datenbank. Der Anwendungs Pool verfügt jedoch standardmäßig nicht über die Berechtigung zum Öffnen der Datenbanken. Dies bedeutet, dass Sie ein Skript ausführen müssen, um diese Berechtigung zu erteilen. In diesem Abschnitt erstellen Sie das Skript und führen es später aus, um sicherzustellen, dass die Anwendung die Datenbanken öffnen kann, wenn Sie in IIS ausgeführt wird.

Kopieren Sie in einem Text-Editor die folgenden SQL-Befehle in eine neue Datei, und speichern Sie Sie als *Grant. SQL*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

Öffnen Sie in Visual Studio die Projekt Mappe "setoso University". Klicken Sie mit der rechten Maustaste auf die Projekt Mappe (nicht auf eines der Projekte), und wählen Sie **Hinzufügen**. Wählen Sie **Vorhandenes Element**aus, navigieren Sie zu *Grant. SQL*, und öffnen Sie es.

> [!NOTE]
> Dieses Skript ist so konzipiert, dass es mit SQL Server Express 2012 oder höher und mit den IIS-Einstellungen in Windows 10, Windows 8 oder Windows 7 funktioniert, wie Sie in diesem Tutorial angegeben sind. Wenn Sie eine andere Version von SQL Server oder Windows verwenden oder IIS auf dem Computer anders einrichten, sind möglicherweise Änderungen an diesem Skript erforderlich. Weitere Informationen zu SQL Server Skripts finden Sie unter [SQL Server-Onlinedokumentation](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Sicherheitshinweis** Dieses Skript erteilt `db_owner` Berechtigungen für den Benutzer, der zur Laufzeit auf die Datenbank zugreift, was Sie in der Produktionsumgebung tun werden. In einigen Szenarien möchten Sie möglicherweise einen Benutzer angeben, der über vollständige Datenbankschema-Aktualisierungs Berechtigungen für die Bereitstellung verfügt, und für die Laufzeit einen anderen Benutzer angeben, der nur über Berechtigungen zum Lesen und Schreiben von Daten verfügt. Weitere Informationen finden Sie unter über [Prüfen der automatischen Änderungen an der Web. config-Datei für Code First-Migrationen](#reviewingmigrations) weiter unten in diesem Tutorial.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Ausführen des Grant-Skripts in der Anwendungsdatenbank

Sie können das Veröffentlichungs Profil so konfigurieren, dass das Grant-Skript in der Mitgliedschafts Datenbank während der Bereitstellung ausgeführt wird, da die Daten Bank Bereitstellung den [dbdacfx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) -Anbieter verwendet Skripts können während der Code First-Migrationen Bereitstellung nicht ausgeführt werden, d. h. wie Sie die Anwendungsdatenbank bereitstellen. Dies bedeutet, dass Sie das Skript vor der Bereitstellung in der Anwendungsdatenbank manuell ausführen müssen.

1. Öffnen Sie in Visual Studio die Datei *Grant. SQL* , die Sie zuvor erstellt haben.

2. Wählen Sie **verbinden**aus. 

    ![Verbindungs Schaltfläche](deploying-to-iis/_static/image11.png)

3. Geben Sie im Dialogfeld **Verbindung mit Server herstellen** den Namen *.\sqlexpress* als **Servernamen**ein. Wählen Sie **verbinden**aus.

4. Wählen Sie in der Dropdown Liste Datenbank die Option **Conto souniversity**aus. Wählen Sie **Ausführen**aus. 

   ![](deploying-to-iis/_static/image12.png)

Die standardmäßige Anwendungs Pool Identität verfügt jetzt über ausreichende Berechtigungen in der Anwendungsdatenbank, damit Code First-Migrationen die Datenbanktabellen erstellen können, wenn die Anwendung ausgeführt wird.

## <a name="publish-to-iis"></a>Veröffentlichen in IIS

Es gibt mehrere Möglichkeiten, wie Sie IIS mithilfe von Visual Studio und Web deploy bereitstellen können:

* Verwenden Sie Visual Studio One-Click-Veröffentlichung.
* Veröffentlichen von der Befehlszeile aus.
* Erstellen Sie ein Bereitstellungs Paket, und installieren Sie es mit IIS-Manager. Das Paket verfügt über eine ZIP-Datei mit allen Dateien und Metadaten, die für die Installation eines Standorts in IIS erforderlich sind.
* Erstellen Sie ein Bereitstellungs Paket, und installieren Sie es mithilfe der Befehlszeile.

Der Prozess, den Sie in den vorherigen Tutorials durchlaufen haben, um Visual Studio für die Automatisierung von Bereitstellungs Aufgaben einzurichten, gilt für alle diese Methoden. In diesen Tutorials verwenden Sie die ersten beiden Methoden. Weitere Informationen zur Verwendung von Bereitstellungs Paketen finden Sie unter Bereitstellen [einer Webanwendung durch Erstellen und Installieren eines Webbereitstellungs Pakets](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) in der Webbereitstellungs-Inhalts Karte für Visual Studio und ASP.net.

Vergewissern Sie sich vor dem veröffentlichen, dass Sie Visual Studio im Administrator Modus ausführen. Wenn **(Administrator)** nicht in der Titelleiste angezeigt wird, schließen Sie Visual Studio. Klicken Sie auf der **Start** Seite von Windows 8 (oder höher) oder dem **Startmenü** von Windows 7 mit der rechten Maustaste auf das Visual Studio-Symbol, und wählen Sie **als Administrator ausführen**aus. Der Administrator Modus ist nur für die Veröffentlichung erforderlich, wenn Sie auf dem lokalen Computer auf IIS veröffentlichen.

### <a name="create-the-publish-profile"></a>Erstellen des Veröffentlichungs Profils

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **conjesouniversity** (nicht auf das Projekt **Conto souniversity. dal** ). Wählen Sie **Veröffentlichen**. Die Seite **veröffentlichen** wird angezeigt.

2. Wählen Sie **Neues Profil**aus. Das Dialogfeld **Veröffentlichungsziel auswählen** wird angezeigt.

3. Wählen Sie **IIS, FTP usw**. Wählen Sie **Profil erstellen**aus. Der **Veröffentlichungs** -Assistent wird angezeigt.

   ![Registerkarte "Profil veröffentlichen" des Assistenten](deploying-to-iis/_static/image26.png)

4. Wählen Sie im Dropdown Menü **Veröffentlichungs Methode** die Option **Web deploy**aus.

5. Geben Sie unter **Server**den *Namen localhost*ein.

6. Geben Sie unter **Standort Name den Namen** *Default Web Site/Conto souniversity*ein.

7. Geben Sie *http://localhost/ContosoUniversity* für die **Ziel-URL**ein.

   Die Einstellung für die **Ziel-URL** ist nicht erforderlich. Wenn Visual Studio die Bereitstellung der Anwendung abgeschlossen hat, öffnet sie automatisch Ihren Standardbrowser mit dieser URL. Wenn Sie nicht möchten, dass der Browser nach der Bereitstellung automatisch geöffnet wird, lassen Sie dieses Feld leer.

8. Wählen Sie **Verbindung** überprüfen, um zu überprüfen, ob die Einstellungen korrekt sind, und Sie können eine Verbindung mit IIS auf dem lokalen Computer herstellen

   Ein grünes Häkchen überprüft, ob die Verbindung erfolgreich hergestellt wurde.

   ![Registerkarte "Web-Assistent veröffentlichen"](deploying-to-iis/_static/image27.png)

9. Wählen Sie **weiter** aus, um zur Registerkarte **Einstellungen** zu gelangen.

10. Das Dropdown Feld **Konfiguration** gibt die Buildkonfiguration an, die bereitgestellt werden soll. Legen Sie den Standardwert **Release**fest. Sie werden in diesem Tutorial keine Debugbuilds bereitstellen.

11. Erweitern Sie **Datei Veröffentlichungs Optionen**. Wählen Sie **Dateien aus dem App-\_Datenordner ausschließen aus**.

    In der Testumgebung greift die Anwendung auf die Datenbanken zu, die Sie in der lokalen SQL Server Express Instanz erstellt haben, nicht auf die MDF-Dateien im Ordner *App\_Data* .

12. Lassen Sie das Kontrollkästchen **Vorkompilierung beim Veröffentlichen** und **Entfernen zusätzlicher Dateien am Ziel** deaktiviert.

    ![Datei Veröffentlichungs Optionen auf der Registerkarte "Einstellungen"](deploying-to-iis/_static/image15a.png)

    Vorkompilierung ist eine Option, die in erster Linie für große Websites nützlich ist. Die Startzeit kann reduziert werden, wenn nach der Veröffentlichung der Website das erste Mal eine Seite angefordert wird.

    Sie müssen keine zusätzlichen Dateien entfernen, da es sich hierbei um die erste Bereitstellung handelt und noch keine Dateien im Zielordner vorhanden sind.

    > [!NOTE] 
    > Wenn Sie für eine nachfolgende Bereitstellung auf demselben Standort **zusätzliche Dateien am Ziel entfernen** auswählen, stellen Sie sicher, dass Sie das Vorschau Feature verwenden, damit Sie im voraussehen, welche Dateien gelöscht werden, bevor Sie bereitstellen. Das erwartete Verhalten ist, dass Web deploy Dateien auf dem Zielserver löscht, die Sie im Projekt gelöscht haben. Die gesamte Ordnerstruktur unter den Quell-und Ziel Ordnern wird jedoch verglichen. in einigen Szenarios können Web deploy Dateien löschen, die Sie nicht löschen möchten.
    > 
    > Wenn Sie z. b. eine Webanwendung in einem Unterordner auf dem Server haben, wenn Sie ein Projekt im Stamm Ordner bereitstellen, wird der Unterordner gelöscht. Möglicherweise verfügen Sie über ein Projekt für die Hauptwebsite unter contoso.com und ein weiteres Projekt für einen Blog unter contoso.com/Blog. Die Blog Anwendung befindet sich in einem Unterordner. Wenn Sie beim Bereitstellen des Haupt Standorts **Weitere Dateien am Ziel entfernen** auswählen, wird die Blog Anwendung gelöscht.
    > 
    > Ein weiteres Beispiel ist, dass Ihr App-\_Datenordner möglicherweise unerwartet gelöscht wird. Bestimmte Datenbanken, z. b. SQL Server Compact, speichern Datenbankdateien im App-\_Datenordner. Nach der erstmaligen Bereitstellung möchten Sie die Datenbankdateien nicht mehr in nachfolgenden bereit Stellungen kopieren, daher wählen Sie auf der Registerkarte "Web packen/veröffentlichen" die Option **App-\_Daten ausschließen** aus. Nachdem Sie die **zusätzlichen Dateien am Ziel** ausgewählt haben, werden die Datenbankdateien und der APP-\_Datenordner selbst gelöscht, wenn Sie das nächste Mal veröffentlichen.

### <a name="configure-deployment-for-the-membership-database"></a>Konfigurieren der Bereitstellung für die Mitgliedschafts Datenbank

Die folgenden Schritte gelten für die **DefaultConnection** -Datenbank im Abschnitt **Datenbanken** des Dialog Felds.

1. Geben Sie im Feld **Remote Verbindungs Zeichenfolge** die folgende Verbindungs Zeichenfolge ein, die auf die neue SQL Server Express Mitgliedschafts Datenbank verweist.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   Beim Bereitstellungs Prozess wird diese Verbindungs Zeichenfolge in die bereitgestellte Web. config-Datei eingefügt, weil **Diese Verbindungs Zeichenfolge zur Laufzeit verwenden** ausgewählt ist.

    Sie können auch die Verbindungs Zeichenfolge aus **Server-Explorer**erhalten. Erweitern Sie in **Server-Explorer**den Knoten **Datenverbindungen** , und wählen Sie den **&lt;MachineName&gt;\sqlexpress.Aspnet-condesouniversity** aus, und kopieren Sie dann im **Eigenschaften** Fenster den Wert der **Verbindungs Zeichenfolge** . Diese Verbindungs Zeichenfolge verfügt über eine zusätzliche Einstellung, die Sie löschen können: `Pooling=False`.

2. Wählen Sie **Datenbank aktualisieren**aus.

   Dies bewirkt, dass das Datenbankschema während der Bereitstellung in der Zieldatenbank erstellt wird. In den nächsten Schritten geben Sie die zusätzlichen Skripts an, die Sie ausführen müssen: eine zum Gewähren des Datenbankzugriffs auf den Standard Anwendungs Pool und eine zum Bereitstellen von Daten.

3. Wählen Sie **Datenbankupdates konfigurieren**aus.
 
4. Wählen Sie im Dialogfeld **Daten Bank Updates konfigurieren** die Option **SQL-Skript hinzufügen**aus. Navigieren Sie zum *Grant. SQL* -Skript, das Sie zuvor im Projektmappenordner gespeichert haben.

5. Wiederholen Sie den Vorgang zum Hinzufügen des *ASPNET-Data-dev. SQL* -Skripts.

   ![Konfigurieren von Datenbankaktualisierungen für die Mitgliedschafts Datenbank](deploying-to-iis/_static/image16.png)

6. Klicken Sie auf **Schließen**.

### <a name="configure-deployment-for-the-application-database"></a>Konfigurieren der Bereitstellung für die Anwendungsdatenbank

Wenn Visual Studio eine Entity Framework `DbContext` Klasse erkennt, wird ein Eintrag im **Daten** Bank Abschnitt erstellt, der anstelle des Kontrollkästchens **Datenbank aktualisieren** über das Kontrollkästchen **Code First-Migrationen ausführen** verfügt. In diesem Tutorial verwenden Sie dieses Kontrollkästchen, um Code First-Migrationen Bereitstellung anzugeben.

In einigen Szenarien verwenden Sie möglicherweise eine `DbContext` Datenbank, aber Sie möchten den dbdacfx-Anbieter anstelle von Migrationen verwenden, um die Datenbank bereitzustellen. Weitere Informationen finden Sie unter [Gewusst wie Bereitstellen einer Code First Datenbank ohne Migrationen?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in den FAQ zur ASP.net-Webbereitstellung auf MSDN.

Die folgenden Schritte gelten für die **School Context** -Datenbank im Abschnitt **Datenbanken** des Dialog Felds.

1. Geben Sie im Feld **Remote Verbindungs Zeichenfolge** die folgende Verbindungs Zeichenfolge ein, die auf die neue SQL Server Express Anwendungsdatenbank verweist.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   Beim Bereitstellungs Prozess wird diese Verbindungs Zeichenfolge in die bereitgestellte Web. config-Datei eingefügt, weil **Diese Verbindungs Zeichenfolge zur Laufzeit verwenden** ausgewählt ist.

   Sie können die Verbindungs Zeichenfolge für die Anwendungsdatenbank auch aus **Server-Explorer** auf die gleiche Weise wie die Verbindungs Zeichenfolge der Mitgliedschafts Datenbank erhalten.

2. Wählen Sie **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt) aus**.

   Diese Option bewirkt, dass der Bereitstellungs Prozess die bereitgestellte Web. config-Datei konfiguriert, um den `MigrateDatabaseToLatestVersion` Initialisierer anzugeben. Dieser Initialisierer aktualisiert die Datenbank automatisch auf die neueste Version, wenn die Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift.

### <a name="configure-publish-profile-transforms"></a>Konfigurieren von Transformationen für Veröffentlichungs profile

1. Klicken Sie auf **Schließen**. Wählen Sie **Ja** aus, wenn Sie gefragt werden, ob Sie die Änderungen speichern möchten.

2. Erweitern Sie in **Projektmappen-Explorer**den Knoten **Eigenschaften**, und erweitern Sie **publishprofiles**.

3. Klicken Sie mit der rechten Maustaste auf *CustomProfile. pubxml* , und benennen Sie es *Test. pubxml*um.

4. Klicken Sie mit der rechten Maustaste auf *Test. pubxml*. Wählen Sie **Konfigurations Transformation hinzufügen**aus.

   ![Menü zum Hinzufügen einer Konfigurations Transformation](deploying-to-iis/_static/image17.png)

   Visual Studio erstellt die Transformations Datei *Web. Test. config* und öffnet sie.

5. Fügen Sie in der Transformations Datei *Web. Test. config* den folgenden Code direkt nach dem öffnenden Konfigurationstag ein.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Wenn Sie das Profil für die Test Veröffentlichung verwenden, legt diese Transformation den Umgebungs Indikator auf "Test" fest. An der bereitgestellten Site sehen Sie "(Test)" nach der H1-Überschrift "TSO University".

6. Speichern und schließen Sie die Datei.

7. Klicken Sie mit der rechten Maustaste auf die Datei *Web. Test. config* , und wählen Sie **Vorschau Transformation** aus, um sicherzustellen, dass die codierte Transformation die erwarteten Änderungen erzeugt.

    Das **Web. config-Vorschau** Fenster zeigt das Ergebnis der Anwendung der Transformationen " *Web. Release. config* " und " *Web. Test. config* " an.

### <a name="preview-the-deployment-updates"></a>Anzeigen einer Vorschau der Bereitstellungs Updates

1. Öffnen Sie erneut den Assistenten **Web veröffentlichen** (Klicken Sie mit der rechten Maustaste auf das Projekt contosouniversity, wählen Sie **veröffentlichen**und dann **Vorschau**) aus.

2. Klicken Sie im Dialogfeld **Vorschau** auf **Vorschau starten** , um eine Liste der Dateien anzuzeigen, die kopiert werden. 

   ![Vorschau veröffentlichen](deploying-to-iis/_static/image29.png)

   Sie können auch den Link **Datenbank anzeigen** auswählen, um die Skripts anzuzeigen, die in der Mitgliedschafts Datenbank ausgeführt werden. (Für Code First-Migrationen Bereitstellung werden keine Skripts ausgeführt, sodass für die Anwendungsdatenbank keine Vorschau angezeigt werden kann.)

3. Wählen Sie **Veröffentlichen**.

   Wenn Visual Studio nicht im Administrator Modus ist, erhalten Sie möglicherweise eine Berechtigungs Fehlermeldung. Schließen Sie in diesem Fall Visual Studio, öffnen Sie es im Administrator Modus, und versuchen Sie erneut, zu veröffentlichen.

   Wenn sich Visual Studio im Administrator Modus befindet, meldet das **Ausgabe** Fenster den erfolgreichen Build und die Veröffentlichung.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Wenn Sie die URL in das Feld **Ziel-URL** auf der Registerkarte Veröffentlichungs Profil **Verbindung** eingegeben haben, wird der Browser automatisch mit der Startseite der Configuration Manager-Startseite in IIS auf Ihrem Computer geöffnet.

## <a name="test-in-the-test-environment"></a>Testen in der Testumgebung

Beachten Sie, dass der Umgebungs Indikator "(Test)" anstelle von "(dev)" anzeigt, der anzeigt, dass die *Web. config* -Transformation für den Umgebungs Indikator erfolgreich war.

Führen Sie die Seite **Dozenten** aus, um zu überprüfen, ob die Datenbank mit den Dozenten Daten Code First. Wenn Sie diese Seite auswählen, kann es einige Minuten dauern, bis Code First die Datenbank erstellt und dann die `Seed`-Methode ausführt. (Dies ist nicht der Fall, wenn Sie auf der Startseite waren, weil die Anwendung noch nicht versucht hat, auf die Datenbank zuzugreifen.)

Wählen Sie die Registerkarte **Studenten** aus, um zu überprüfen, ob die bereitgestellte Datenbank keine Studenten

Wählen Sie im Menü " **Students** " die Option " **Students** " Fügen Sie einen Studenten hinzu, und zeigen Sie dann den neuen Studenten auf der Seite " **Students** " an. Dadurch wird sichergestellt, dass Sie erfolgreich in die Datenbank schreiben können.

Wählen Sie im Menü **Kurse** die Option **Guthaben aktualisieren**aus. Die Seite " **Guthaben aktualisieren** " erfordert Administrator Berechtigungen, sodass die **Anmelde** Seite angezeigt wird. Geben Sie die Anmelde Informationen für das Administrator Konto ein, die Sie zuvor erstellt haben ("admin" und "devpwd"). Die Seite " **Guthaben aktualisieren** " wird angezeigt. Dadurch wird überprüft, ob das Administrator Konto, das Sie im vorherigen Tutorial erstellt haben, ordnungsgemäß in der Testumgebung bereitgestellt wurde.

Stellen Sie sicher, dass ein *ELMAH* -Ordner im Ordner " *c:\inetpub\wwwroot\condesouniversity* " vorhanden ist, der nur die Platzhalter Datei enthält.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Überprüfen Sie die automatischen Änderungen an "Web. config" für Code First-Migrationen

Öffnen Sie die Datei " *Web. config* " in der bereitgestellten Anwendung unter " *c:\inetpub\wwwroot\contosouniversity* ". Sie können sehen, wo der von Ihnen konfigurierte Bereitstellungs Prozess Code First-Migrationen, um die Datenbank automatisch auf die neueste Version zu aktualisieren.

![](deploying-to-iis/_static/image21.png)

Beim Bereitstellungs Prozess wurde außerdem eine neue Verbindungs Zeichenfolge für Code First-Migrationen erstellt, die ausschließlich zum Aktualisieren des Datenbankschemas verwendet wird:

![Verbindungs Zeichenfolge Database_Publish](deploying-to-iis/_static/image22.png)

Mit dieser zusätzlichen Verbindungs Zeichenfolge können Sie ein Benutzerkonto für Datenbankschema Updates und ein anderes Benutzerkonto für den Zugriff auf Anwendungsdaten angeben. Beispielsweise können Sie der Anwendung die Rolle " **DB\_Owner** " Code First-Migrationen und **DB-\_DataReader** mit **DB-\_-DataWriter** -Rollen zuweisen. Dies ist ein gängiges tiefgreifendes Verteidigungs Muster, das verhindert, dass potenziell bösartiger Code in der Anwendung das Datenbankschema ändert. (Dies kann beispielsweise bei einem erfolgreichen SQL Injection-Angriff vorkommen.) In diesen Tutorials wird dieses Muster nicht verwendet. Gehen Sie folgendermaßen vor, um dieses Muster in Ihrem Szenario zu implementieren:

1. Geben Sie im Assistenten **Web veröffentlichen** auf der Registerkarte **Einstellungen** die Verbindungs Zeichenfolge ein, die einen Benutzer mit vollständigen Datenbankschema-Aktualisierungs Berechtigungen angibt. Deaktivieren Sie das Kontrollkästchen **Diese Verbindungs Zeichenfolge zur Laufzeit verwenden** . In der bereitgestellten Web. config-Datei wird dies die `DatabasePublish` Verbindungs Zeichenfolge.

2. Erstellen Sie eine Transformation für die Datei "Web. config" für die Verbindungs Zeichenfolge, die von der Anwendung zur Laufzeit verwendet werden soll.

## <a name="summary"></a>Zusammenfassung

Sie haben Ihre Anwendung nun auf dem Entwicklungs Computer auf IIS bereitgestellt und dort getestet.

![Startseite im Test](deploying-to-iis/_static/image23.png)

Dadurch wird sichergestellt, dass der Inhalt der Anwendung vom Bereitstellungs Prozess an den richtigen Speicherort kopiert wurde (ausgenommen der Dateien, die Sie nicht bereitstellen wollten) und dass IIS während der Bereitstellung ordnungsgemäß konfiguriert Web Deploy. Im nächsten Tutorial führen Sie einen weiteren Test aus, der einen Bereitstellungs Task findet, der noch nicht ausgeführt wurde: das Festlegen der Ordner Berechtigungen für den Ordner " *Elm AH* ".

## <a name="more-information"></a>Weitere Informationen

Informationen zum Ausführen von IIS oder IIS Express in Visual Studio finden Sie in den folgenden Ressourcen:

- [IIS Express Übersicht](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) auf der IIS.NET-Website.
- [Einführung in IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) von Scott Guthrie Blog.
- [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Grundlegende Unterschiede zwischen IIS und der ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) auf der ASP.NET-Website.

Informationen zu den Problemen, die auftreten können, wenn Ihre Anwendung mit mittlerer Vertrauenswürdigkeit ausgeführt wird, finden Sie unter [Hosting ASP.NET Applications in Mittel Trust](http://www.4guysfromrolla.com/articles/100307-1.aspx) auf den vier Guys von Rolla Site.

> [!div class="step-by-step"]
> [Zurück](project-properties.md)
> [Weiter](setting-folder-permissions.md)
