---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen in der Produktion | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: ddc3d15f0436c4c3a24491cf0377111768da67df
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617639"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellung in der Produktion

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht über

In diesem Tutorial richten Sie ein Microsoft Azure-Konto ein, erstellen Staging-und Produktionsumgebungen und stellen Ihre ASP.NET-Webanwendung mithilfe der Visual Studio-Funktion zum Veröffentlichen mit nur einem Mausklick in der Stagingumgebung und in der Produktionsumgebung bereit.

Wenn Sie möchten, können Sie die Bereitstellung für einen Drittanbieter-Hostinganbieter durchzuführen. Die meisten der in diesem Tutorial beschriebenen Verfahren sind für einen Hostinganbieter oder für Azure identisch, mit der Ausnahme, dass jeder Anbieter über eine eigene Benutzeroberfläche für die Konto-und Website Verwaltung verfügt. Einen Hostinganbieter finden Sie im Katalog [der Anbieter](https://www.microsoft.com/web/hosting) auf der Microsoft.com-Website.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, während Sie das Tutorial durchlaufen, überprüfen Sie die Problem Behandlungs Seite in dieser tutorialreihe.

## <a name="get-a-microsoft-azure-account"></a>Microsoft Azure Konto erhalten

Wenn Sie noch nicht über ein Azure-Konto verfügen, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Erstellen einer Stagingumgebung

> [!NOTE]
> Da dieses Tutorial verfasst wurde, Azure App Service ein neues Feature hinzugefügt, um viele der Prozesse zum Erstellen von Staging-und Produktionsumgebungen zu automatisieren. Informationen finden [Sie unter Einrichten von Stagingumgebungen für Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).

Wie im Tutorial bereitstellen [der Testumgebung](deploying-to-iis.md)erläutert, ist die zuverlässigste Test Umgebung eine Website des hostinganbieters, die genau wie die Produktions Website ist. Bei vielen Hostinganbietern müssten Sie die Vorteile dieser Vorteile vor erheblichen zusätzlichen Kosten abwägen, aber in Azure können Sie eine zusätzliche kostenlose Web-App als stagingapp erstellen. Außerdem benötigen Sie eine Datenbank, und die zusätzlichen Kosten für die Kosten der Produktionsdatenbank sind entweder keine oder nur minimal. In Azure Zahlen Sie für die Menge des verwendeten Daten Bank Speichers und nicht für jede Datenbank, und der Umfang des zusätzlichen Speichers, den Sie in der Stagingumgebung verwenden, ist minimal.

Wie im Tutorial bereitstellen [der Test Umgebung](deploying-to-iis.md)erläutert, werden Sie in Staging und Produktion ihre beiden Datenbanken in einer Datenbank bereitstellen. Wenn Sie Sie separat aufbewahren möchten, ist der Prozess identisch, mit dem Unterschied, dass Sie eine zusätzliche Datenbank für jede Umgebung erstellen und die richtige Ziel Zeichenfolge für jede Datenbank auswählen, wenn Sie das Veröffentlichungs Profil erstellen.

In diesem Abschnitt des Tutorials erstellen Sie eine Web-App und eine Datenbank, die für die Stagingumgebung verwendet werden, und Sie stellen Sie bereit, bevor Sie in der Produktionsumgebung erstellen und bereitstellen.

> [!NOTE]
> In den folgenden Schritten wird gezeigt, wie Sie eine Web-App in Azure App Service mithilfe des Azure-Verwaltungs Portals erstellen. In der neuesten Version des Azure SDK können Sie dies auch tun, ohne Visual Studio zu belassen, indem Sie Server-Explorer verwenden. In Visual Studio 2013 können Sie auch direkt über das Dialogfeld "veröffentlichen" eine Web-App erstellen. Weitere Informationen finden Sie unter [Erstellen einer ASP.net-Web-App in Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)

1. Klicken Sie im [Azure-Verwaltungsportal](https://manage.windowsazure.com/)auf **Websites**, und klicken Sie dann auf **neu**.
2. Klicken Sie auf **Website**, und klicken Sie dann auf **Benutzer definiert erstellen**.

    Der Assistent zum **Erstellen einer neuen Website** wird geöffnet. Mit dem Assistenten für **benutzerdefinierte Erstellung** können Sie gleichzeitig eine Website und eine Datenbank erstellen.
3. Geben Sie im Schritt **Website erstellen** des Assistenten eine Zeichenfolge in das Feld **URL** ein, die als eindeutige URL für die Stagingumgebung Ihrer Anwendung verwendet werden soll. Geben Sie z. b. contesouniversity-staging123 (einschließlich Zufallszahlen am Ende) ein, um diese für den Fall eindeutig zu gestalten, dass contesouniversity-Staging durchgeführt wird.

    Die vollständige URL besteht aus den hier eingegebenen Informationen sowie dem Suffix, das neben dem Textfeld angezeigt wird.
4. Wählen Sie in der Dropdown Liste **Region** die Region aus, die Ihnen am nächsten liegt.

    Diese Einstellung gibt an, in welchem Rechenzentrum Ihre Web-App ausgeführt wird.
5. Wählen Sie in der Dropdown Liste **Datenbank** die Option **neue SQL-Datenbank erstellen**aus.
6. Belassen Sie im Feld **Name der DB-Verbindungs Zeichenfolge** den Standardwert *DefaultConnection*.
7. Klicken Sie auf den Pfeil rechts unten im Feld.

    Die folgende Abbildung zeigt das Dialogfeld **Website erstellen** mit Beispiel Werten. Die URL und die Region, die Sie eingegeben haben, unterscheiden sich.

    ![Website Schritt erstellen](deploying-to-production/_static/image1.png)

    Der Assistent wechselt zum Schritt **Datenbankeinstellungen angeben** .
8. Geben Sie im Feld **Name** die Zeichenfolge *conalsouniversity* plus eine Zufallszahl ein, um Sie eindeutig zu machen, z. b. *ContosoUniversity123*.
9. Wählen Sie im Feld **Server** die Option **neuer SQL-Datenbankserver**aus.
10. Geben Sie einen Administrator Namen und ein Kennwort ein.

    Sie geben hier keinen vorhandenen Namen und kein Kennwort ein. Sie geben einen neuen Namen und ein Kennwort ein, die Sie jetzt definieren, wenn Sie auf die Datenbank zugreifen.
11. Wählen Sie im Feld **Region** die Region aus, die Sie für die Web-App ausgewählt haben.

    Wenn Sie den Webserver und den Datenbankserver in derselben Region aufbewahren, erzielen Sie die beste Leistung und minimieren die Kosten.
12. Klicken Sie unten im Feld auf das Häkchen, um anzugeben, dass Sie fertig sind.

    Die folgende Abbildung zeigt das Dialogfeld **Datenbankeinstellungen angeben** mit Beispiel Werten. Die Werte, die Sie eingegeben haben, können unterschiedlich sein.

    ![Schritt "Datenbankeinstellungen" der neuen Website-Assistent zum Erstellen mit Datenbanken](deploying-to-production/_static/image2.png)

    Der Verwaltungsportal wird zur Seite Websites zurückgegeben, und in der Spalte **Status** wird angezeigt, dass die Web-App erstellt wird. Nach einer Weile (in der Regel weniger als einer Minute) wird in der Spalte **Status** angezeigt, dass die Web-App erfolgreich erstellt wurde. In der Navigationsleiste auf der linken Seite wird die Anzahl der Web-Apps, die Sie in Ihrem Konto haben, neben dem Symbol " **Websites** " angezeigt, und die Anzahl der Datenbanken wird neben dem Symbol " **SQL-Datenbanken** " angezeigt.

    ![Website Seite Verwaltungsportal, erstellte Website](deploying-to-production/_static/image3.png)

    Ihr Web-App-Name unterscheidet sich von der Beispiel-app in der Abbildung.

## <a name="deploy-the-application-to-staging"></a>Bereitstellen der Anwendung für die Bereitstellung

Nachdem Sie nun eine Web-App und eine Datenbank für die Stagingumgebung erstellt haben, können Sie das Projekt für das Projekt bereitstellen.

> [!NOTE]
> Diese Anweisungen zeigen, wie Sie ein Veröffentlichungs Profil erstellen, indem Sie eine *publishsettings* -Datei herunterladen, die nicht nur für Azure, sondern auch für Hostinganbieter von Drittanbietern funktioniert. Mit dem neuesten Azure SDK können Sie auch direkt aus Visual Studio eine Verbindung mit Azure herstellen und aus einer Liste von Web-Apps auswählen, die Sie in Ihrem Azure-Konto haben. In Visual Studio 2013 können Sie sich über das Dialogfeld " **Webveröffentlichung** " oder über das Fenster " **Server-Explorer** " bei Azure anmelden. Weitere Informationen finden Sie unter [Erstellen einer ASP.net-Web-App in Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

### <a name="download-the-publishsettings-file"></a>Herunterladen der publishsettings-Datei

1. Klicken Sie auf den Namen der Web-App, die Sie soeben erstellt haben.

    ![Klicken Sie auf den Standort, um zum Dashboard zu gelangen.](deploying-to-production/_static/image4.png)
2. Klicken Sie auf der Registerkarte **Dashboard** unter auf einen **Blick** auf **Veröffentlichungs Profil herunterladen**.

    ![Link zum Herunterladen des Veröffentlichungs Profils](deploying-to-production/_static/image5.png)

    In diesem Schritt wird eine Datei mit allen Einstellungen heruntergeladen, die Sie benötigen, um eine Anwendung für Ihre Web-App bereitzustellen. Sie importieren diese Datei in Visual Studio, damit Sie diese Informationen nicht manuell eingeben müssen.
3. Speichern Sie die *publishsettings* -Datei in einem Ordner, auf den Sie von Visual Studio aus zugreifen können.

    ![die publishsettings-Datei wird gespeichert.](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Sicherheit: die *publishsettings* -Datei enthält Ihre (unverschlüsselten) Anmelde Informationen, die zum Verwalten Ihrer Azure-Abonnements und-Dienste verwendet werden. Die bewährte Sicherheitsmaßnahme für diese Datei besteht darin, diese temporär außerhalb der Quellverzeichnisse (z. b. im Ordner libraries\documents) zu speichern und Sie nach Abschluss des Imports zu löschen. Ein böswilliger Benutzer, der Zugriff auf die *publishsettings* -Datei erlangt, kann die Azure-Dienste bearbeiten, erstellen und löschen.

### <a name="create-a-publish-profile"></a>Erstellen eines Veröffentlichungs Profils

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt condesouniversity in **Projektmappen-Explorer** , und wählen Sie im Kontextmenü **veröffentlichen** aus.

    Der Assistent **Web veröffentlichen** wird geöffnet.
2. Klicken Sie auf die Registerkarte **Profil** .
3. Klicken Sie auf **importieren**.
4. Navigieren Sie zur Datei " *. publishsettings* ", die Sie zuvor heruntergeladen haben, und klicken Sie auf **Öffnen**.

    ![Dialogfeld "Veröffentlichungs Einstellungen importieren"](deploying-to-production/_static/image7.png)
5. Klicken Sie auf der Registerkarte **Verbindung** auf **Verbindung** überprüfen, um sicherzustellen, dass die Einstellungen richtig sind.

    Wenn die Verbindung überprüft wurde, wird neben der Schaltfläche **Verbindung** überprüfen ein grünes Häkchen angezeigt.

    Wenn Sie bei einigen Hostinganbietern auf **Verbindung**überprüfen klicken, wird möglicherweise das Dialogfeld **Zertifikat Fehler** angezeigt. Wenn Sie dies tun, überprüfen Sie, ob der Servername den Erwartungen entspricht. Wenn der Servername richtig ist, wählen Sie **dieses Zertifikat in zukünftigen Sitzungen von Visual Studio speichern** aus, und klicken Sie auf **annehmen**. (Dieser Fehler weist darauf hin, dass der Hostinganbieter die Kosten für den Erwerb eines SSL-Zertifikats für die URL, für die Sie die Bereitstellung bereitstellen, vermieden hat. Wenn Sie eine sichere Verbindung mit einem gültigen Zertifikat herstellen möchten, wenden Sie sich an Ihren Hostinganbieter.)
6. Klicken Sie auf **Weiter**.

    ![Symbol "Verbindung erfolgreich" und Schaltfläche "weiter" auf Verbindungs Register](deploying-to-production/_static/image8.png)
7. Erweitern Sie auf der Registerkarte **Einstellungen** die **Option Datei Veröffentlichungs Optionen**, und wählen Sie dann **Dateien aus dem App-\_Datenordner ausschließen aus**.

    Weitere Informationen zu den anderen Optionen unter **Datei Veröffentlichungs Optionen**finden Sie im Tutorial bereitstellen [in IIS](deploying-to-iis.md) . Der Screenshot, der das Ergebnis dieses Schritts anzeigt, und die folgenden Konfigurationsschritte für die Datenbank sind am Ende der Daten Bank Konfigurationsschritte.
8. Konfigurieren Sie im Abschnitt **Datenbanken** unter **DefaultConnection** die Daten Bank Bereitstellung für die Mitgliedschafts Datenbank.
9. 1. Wählen Sie **Datenbank aktualisieren**aus.

        Das Feld **Remote Verbindungs Zeichenfolge** direkt unterhalb von **DefaultConnection** wird mit der Verbindungs Zeichenfolge aus der publishsettings-Datei ausgefüllt. Die Verbindungs Zeichenfolge enthält SQL Server Anmelde Informationen, die als Klartext in der *pubxml* -Datei gespeichert werden. Wenn Sie diese nicht dauerhaft speichern möchten, können Sie Sie aus dem Veröffentlichungs Profil entfernen, nachdem die Datenbank bereitgestellt wurde, und Sie stattdessen in Azure speichern. Weitere Informationen finden Sie unter [Schützen Ihrer ASP.NET-Daten bankverbindungs](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) Zeichenfolgen bei der Bereitstellung in Azure über die Quelle im Blog von Scott Hanselman.
      2. Klicken Sie auf **Datenbankupdates konfigurieren**.
      3. Klicken Sie im Dialogfeld **Daten Bank Updates konfigurieren** auf **SQL-Skript hinzufügen**.
      4. Navigieren Sie im Feld **SQL-Skript hinzufügen** zum Skript *ASPNET-Data-Prod. SQL* , das Sie zuvor im Projektmappenordner gespeichert haben, und klicken Sie dann auf **Öffnen**.
      5. Schließen Sie das Dialogfeld **Daten Bank Updates konfigurieren** .
10. Wählen Sie im Abschnitt " **School Context** " im Abschnitt " **Datenbanken** " die Option **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)**

    Visual Studio zeigt **Execute Code First-Migrationen** anstelle von **Update Database** für `DbContext` Klassen an. Wenn Sie den dbdacfx-Anbieter anstelle von Migrationen verwenden möchten, um eine Datenbank bereitzustellen, auf die Sie mithilfe einer `DbContext` Klasse zugreifen, finden Sie weitere Informationen unter Gewusst wie bereitstellen [einer Code First Datenbank ohne Migrationen?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in den häufig gestellten Fragen zur Webbereitstellung für Visual Studio und ASP.net auf MSDN

    Die Registerkarte **Einstellungen** sieht nun wie im folgenden Beispiel aus:

    ![Registerkarte "Einstellungen" für Staging](deploying-to-production/_static/image9.png)
11. Führen Sie die folgenden Schritte aus, um das Profil zu speichern und in *Staging*umzubenennen:

    1. Klicken Sie auf die Registerkarte **Profil** , und klicken Sie dann auf **Profile verwalten**.
    2. Der Import hat zwei neue Profile erstellt: eine für FTP und eine für Web Deploy. Sie haben das Web deploy Profil konfiguriert: dieses Profil in *Staging*umbenennen.

        ![Profil in Staging umbenennen](deploying-to-production/_static/image10.png)
    3. Schließen Sie das Dialogfeld **Webveröffentlichungs Profile bearbeiten** .
    4. Schließen Sie den Assistenten **Web veröffentlichen** .

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Konfigurieren einer Transformation für Veröffentlichungs Profile für den Umgebungs Indikator

> [!NOTE]
> In diesem Abschnitt wird gezeigt, wie eine Web. config-Transformation für den Umgebungs Indikator eingerichtet wird. Da der Indikator im `<appSettings>`-Element enthalten ist, haben Sie eine weitere Alternative zum Angeben der Transformation, wenn Sie in Azure App Service bereitstellen. Weitere Informationen finden Sie unter [Angeben von Web. config-Einstellungen in Azure](web-config-transformations.md#watransforms).

1. Erweitern Sie in **Projektmappen-Explorer**den Knoten **Eigenschaften**, und erweitern Sie dann **publishprofiles**.
2. Klicken Sie mit der rechten Maustaste auf *Staging. pubxml*, und klicken Sie dann auf **Konfigurations Transformation hinzufügen**.

    ![Config-Transformation für Staging hinzufügen](deploying-to-production/_static/image11.png)

    Visual Studio erstellt die Transformations Datei *Web. Staging. config* und öffnet diese.
3. Fügen Sie in der Transformations Datei *Web. Staging. config* den folgenden Code direkt nach dem öffnenden `configuration`-Tag ein.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Wenn Sie das Staging-Veröffentlichungs Profil verwenden, legt diese Transformation den Umgebungs Indikator auf "Prod" fest. In der bereitgestellten Web-App sehen Sie nach der H1-Überschrift "TSO University" kein Suffix wie "(dev)" oder "(Test)".
4. Klicken Sie mit der rechten Maustaste auf die Datei *Web. Staging. config* , und klicken Sie auf **Vorschau transformieren** , um sicherzustellen, dass die von Ihnen codierte Transformation die erwarteten Änderungen

    Das **Web. config-Vorschau** Fenster zeigt das Ergebnis der Anwendung der Transformationen " *Web. Release. config* " und " *Web. Staging. config* " an.

### <a name="prevent-public-use-of-the-test-app"></a>Verhindern der öffentlichen Verwendung der Test-App

Ein wichtiger Aspekt bei der Staging-APP ist, dass Sie im Internet Live ist, aber nicht von der Öffentlichkeit verwendet werden soll. Sie können eine oder mehrere der folgenden Methoden verwenden, um die Wahrscheinlichkeit zu minimieren, dass Sie von Benutzern gefunden und verwendet werden:

- Legen Sie Firewallregeln fest, die den Zugriff auf die Staging-app nur von IP-Adressen zulassen, die Sie zum Testen der Staging-
- Verwenden Sie eine verborgene URL, die nicht erraten werden kann.
- Erstellen Sie eine Datei " *robots. txt* ", um sicherzustellen, dass Suchmaschinen die Test-App nicht durchforsten und in den Suchergebnissen Links darauf melden.

Die erste dieser Methoden ist die effektivste, wird aber in diesem Tutorial nicht behandelt, da es erforderlich wäre, dass Sie die Bereitstellung in einem Azure-clouddienst statt Azure App Service durchführt. Weitere Informationen zu Cloud Services-und IP-Einschränkungen in Azure finden Sie unter [von Azure bereitgestellte computehostingoptionen](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) und [Blockieren bestimmter IP-Adressen für den Zugriff auf eine webrolle](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Wenn Sie die Bereitstellung für einen Drittanbieter-Hostinganbieter durchführen, wenden Sie sich an den Anbieter, um herauszufinden, wie IP-Einschränkungen implementiert werden.

In diesem Tutorial erstellen Sie eine Datei " *robots. txt* ".

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt conjesouniversity, und klicken Sie auf **Neues Element hinzufügen**.
2. Erstellen Sie eine neue **Textdatei** mit dem Namen " *robots. txt*", und fügen Sie den folgenden Text ein:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    Die `User-agent` Zeile weist Suchmaschinen an, dass sich die Regeln in der Datei auf alle Suchmaschinen-Webcrawlers (Robots) beziehen, und die `Disallow` Linie gibt an, dass keine Seiten auf der Website durchlaufen werden sollen.

    Sie möchten, dass Suchmaschinen Ihre Produktions-App katalogisieren, sodass Sie diese Datei aus der Produktions Bereitstellung ausschließen müssen. Zu diesem Zweck konfigurieren Sie eine Einstellung im Veröffentlichungs Profil für die Produktion, wenn Sie Sie erstellen.

### <a name="deploy-to-staging"></a>Bereitstellung für Staging

1. Öffnen Sie den Assistenten zum Veröffentlichen von Web-Assistenten, indem Sie mit der **rechten Maustaste auf**das Projekt "Projekt der Projekt **Website**
2. Stellen Sie sicher, dass das Stagingprofil ausgewählt ist.
3. Klicken Sie auf **Veröffentlichen**.

    Das Fenster **Ausgabe** zeigt, welche Bereitstellungs Aktionen ausgeführt wurden, und meldet einen erfolgreichen Abschluss der Bereitstellung. Der Standardbrowser öffnet automatisch die URL der bereitgestellten Web-App.

## <a name="test-in-the-staging-environment"></a>Testen in der Stagingumgebung

Beachten Sie, dass der Umgebungs Indikator fehlt (es gibt kein "(Test)" oder "(dev)" nach der H1-Überschrift, die anzeigt, dass die *Web. config* -Transformation für den Umgebungs Indikator erfolgreich war.

![Staging der Startseite](deploying-to-production/_static/image12.png)

Führen Sie die Seite **Studenten** aus, um sicherzustellen, dass die bereitgestellte Datenbank keine Studenten enthält.

Führen Sie die Seite **Dozenten** aus, um zu überprüfen, ob die Datenbank mit den Dozenten Daten Code First:

Wählen Sie im Menü " **Students** " die Option **Studenten hinzufügen** aus, fügen Sie einen Studenten hinzu, und zeigen Sie dann die neue Student auf der Seite **Studenten** an, um sicherzustellen, dass Sie erfolgreich in die Datenbank schreiben

Klicken Sie auf der Seite **Kurse** auf **Guthaben aktualisieren**. Die Seite " **Guthaben aktualisieren** " erfordert Administrator Berechtigungen, sodass die **Anmelde** Seite angezeigt wird. Geben Sie die Anmelde Informationen für das Administrator Konto ein, die Sie zuvor erstellt haben ("admin" und "prodpwd"). Die Seite " **Guthaben aktualisieren** " wird angezeigt, die überprüft, ob das im vorherigen Tutorial erstellte Administrator Konto ordnungsgemäß in der Testumgebung bereitgestellt wurde.

Fordern Sie eine ungültige URL an, um einen Fehler zu verursachen, den ELMAH nachverfolgt, und fordern Sie dann den ELMAH-Fehlerbericht an. Wenn Sie die Bereitstellung für einen Hosting-Anbieter eines Drittanbieters durchführt, werden Sie wahrscheinlich feststellen, dass der Bericht aus demselben Grund leer ist, als er im vorherigen Tutorial leer war. Sie müssen die Konto Verwaltungs Tools des hostinganbieters verwenden, um Ordner Berechtigungen zu konfigurieren, damit ELMAH in den Protokoll Ordner schreiben kann.

Die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud in einer Web-App ausgeführt, die genau wie für die Produktion verwendet werden soll. Da alles ordnungsgemäß funktioniert, besteht der nächste Schritt in der Bereitstellung in der Produktion.

## <a name="deploy-to-production"></a>In Produktionsumgebungen bereitstellen

Der Prozess zum Erstellen einer Produktions-Web-App und zum Bereitstellen in der Produktion ist identisch mit dem für das Staging, mit der Ausnahme, dass Sie die " *robots. txt* " aus der Bereitstellung ausschließen müssen Zu diesem Zweck bearbeiten Sie die Veröffentlichungs Profil Datei.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Erstellen der Produktionsumgebung und des Veröffentlichungs Profils für die Produktion

1. Erstellen Sie die Produktions-Web-App und die Datenbank in Azure, und befolgen Sie dabei die gleichen Schritte wie für das Staging.

    Wenn Sie die Datenbank erstellen, können Sie Sie auf demselben Server ablegen, den Sie zuvor erstellt haben, oder einen neuen Server erstellen.
2. Laden Sie die *publishsettings* -Datei herunter.
3. Erstellen Sie das Veröffentlichungs Profil, indem Sie die Production *. publishsettings* -Datei importieren, und befolgen Sie dabei die Prozedur, die Sie für das Staging verwendet haben.

    Vergessen Sie nicht, das Daten Bereitstellungs Skript unter **DefaultConnection** im Abschnitt **Datenbanken** der Registerkarte **Einstellungen** zu konfigurieren.
4. Benennen Sie das Veröffentlichungs Profil in *Production*um.
5. Konfigurieren Sie eine Transformation für Veröffentlichungs Profile für den Umgebungs Indikator, und befolgen Sie dabei die Prozedur, die Sie für das Staging verwendet haben.

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Bearbeiten Sie die pubxml-Datei, um "robots. txt" auszuschließen.

Veröffentlichungs Profil Dateien heißen &lt;Profile Name&gt; *. pubxml* und befinden sich im Ordner *publishprofiles* . Der Ordner " *publishprofiles* " befindet sich unter dem Ordner C# "Properties" in einem Webanwendungs Projekt, unter dem Ordner " *My Project* " in einem Projekt der VB-Webanwendung oder unter dem Ordner *App\_Data* in einem Web-App-Projekt. Jede *pubxml* -Datei enthält Einstellungen, die für ein Veröffentlichungs Profil gelten. Die Werte, die Sie im Assistenten Web veröffentlichen eingeben, werden in diesen Dateien gespeichert, und Sie können Sie bearbeiten, um Einstellungen zu erstellen oder zu ändern, die nicht in der Visual Studio-Benutzeroberfläche verfügbar gemacht werden.

Standardmäßig sind *pubxml* -Dateien im Projekt enthalten, wenn Sie ein Veröffentlichungs Profil erstellen, aber Sie können Sie aus dem Projekt ausschließen, aber von Visual Studio weiterhin verwendet werden. Visual Studio sucht im Ordner *publishprofiles* nach *pubxml* -Dateien, unabhängig davon, ob Sie im Projekt enthalten sind.

Für jede *pubxml* -Datei gibt es eine *pubxml. User* -Datei. Die *. pubxml. User* -Datei enthält das verschlüsselte Kennwort, wenn Sie die Option **Kennwort speichern** ausgewählt haben, und wird standardmäßig aus dem Projekt ausgeschlossen.

Eine *pubxml* -Datei enthält die Einstellungen, die sich auf ein bestimmtes Veröffentlichungs Profil beziehen. Wenn Sie Einstellungen konfigurieren möchten, die für alle Profile gelten, können Sie eine *WPP. targets* -Datei erstellen. Der Buildprozess importiert diese Dateien in die *csproj* -oder *vbproj* -Projektdatei, sodass die meisten Einstellungen, die Sie in der Projektdatei konfigurieren können, in diesen Dateien konfiguriert werden können. Weitere Informationen zu *. pubxml* -Dateien und *WPP. targets* -Dateien finden Sie unter Gewusst [wie: Bearbeiten von Bereitstellungs Einstellungen in Veröffentlichungs Profil Dateien (. pubxml) und in der Datei ". WPP. targets" in Visual Studio-Webprojekten](https://msdn.microsoft.com/library/ff398069.aspx).

1. Erweitern Sie in **Projektmappen-Explorer**den Knoten **Eigenschaften** , und erweitern Sie **publishprofiles**.
2. Klicken Sie mit der rechten Maustaste auf *Production. pubxml* und dann auf **Öffnen**.

    ![Öffnen Sie die pubxml-Datei.](deploying-to-production/_static/image13.png)
3. Klicken Sie mit der rechten Maustaste auf *Production. pubxml* und dann auf **Öffnen**.
4. Fügen Sie direkt vor dem schließenden `PropertyGroup` Element die folgenden Zeilen hinzu:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Die pubxml-Datei sieht nun wie im folgenden Beispiel aus:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Weitere Informationen zum Ausschließen von Dateien und Ordnern finden Sie unter [kann ich bestimmte Dateien oder Ordner von der Bereitstellung ausschließen?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) in den häufig gestellten Fragen **zur Webbereitstellung für Visual Studio und ASP.net** auf MSDN.

### <a name="deploy-to-production"></a>In Produktionsumgebungen bereitstellen

1. Öffnen Sie den Assistenten **Web veröffentlichen** , stellen Sie sicher, dass das Veröffentlichungs Profil für die **Produktion** ausgewählt ist, und klicken Sie dann auf der Registerkarte **Vorschau** auf **Vorschau starten** , um zu überprüfen, ob die Datei *robots. txt* in die Produktions-App kopiert wird.

    ![Vorschau der in der Produktion zu veröffentlichenden Dateien](deploying-to-production/_static/image14.png)

    Überprüfen Sie die Liste der Dateien, die kopiert werden. Sie werden feststellen, dass alle *CS* -Dateien, einschließlich *aspx.cs*-, *aspx.Designer.cs*-, *Master.cs*-und *Master.Designer.cs* -Dateien, ausgelassen werden. Der gesamte Code wurde in die Dateien " *condesouniversity. dll* " und " *condesouniversity. pdb* " kompiliert, die Sie im Ordner " *bin* " finden. Da nur die *dll* -Datei zum Ausführen der Anwendung benötigt wird und Sie zuvor festgelegt haben, dass nur Dateien, die zum Ausführen der Anwendung erforderlich sind, bereitgestellt werden sollen, wurden keine *CS* -Dateien in die Zielumgebung kopiert. Der Ordner " *obj* " und die Dateien " *condesouniversity. csproj* " und " *. csproj. User* " werden aus demselben Grund weggelassen.

    Klicken Sie auf **veröffentlichen** , um Sie in der Produktionsumgebung bereitzustellen.
2. Testen Sie in der Produktion, und befolgen Sie dabei die gleichen Schritte wie für das Staging.

    Alles ist mit dem Staging identisch, außer die URL und das Fehlen der Datei " *robots. txt* ".

## <a name="summary"></a>Summary

Sie haben Ihre Web-App nun erfolgreich bereitgestellt und getestet und sind öffentlich über das Internet verfügbar.

![Startseite Produktion](deploying-to-production/_static/image15.png)

Im nächsten Tutorial aktualisieren Sie den Anwendungscode und stellen die Änderungen in den Test-, Staging-und Produktionsumgebungen bereit.

> [!NOTE]
> Während Ihre Anwendung in der Produktionsumgebung verwendet wird, sollten Sie einen Wiederherstellungs Plan implementieren. Das heißt, Sie müssen die Datenbanken in regelmäßigen Abständen von der Produktions-APP an einem sicheren Speicherort sichern, und Sie sollten mehrere Generationen solcher Sicherungen aufbewahren. Wenn Sie die Datenbank aktualisieren, sollten Sie direkt vor der Änderung eine Sicherungskopie erstellen. Wenn Sie dann einen Fehler machen und ihn erst ermitteln, wenn Sie ihn in der Produktionsumgebung bereitgestellt haben, können Sie die Datenbank weiterhin in dem Zustand wiederherstellen, in dem Sie sich befand, bevor Sie beschädigt wurde. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Azure SQL-Datenbanken](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> In diesem Tutorial wird als SQL Server Edition Azure SQL-Datenbank bereitgestellt. Während der Bereitstellungs Prozess anderen Editionen von SQL Server ähnelt, erfordert eine echte Produktionsanwendung in einigen Szenarien möglicherweise speziellen Code für die Azure SQL-Datenbank. Weitere Informationen finden Sie unter [Arbeiten mit Azure SQL-Datenbank](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) und [auswählen zwischen SQL Server und der Azure SQL-Datenbank](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Zurück](setting-folder-permissions.md)
> [Weiter](deploying-a-code-update.md)
