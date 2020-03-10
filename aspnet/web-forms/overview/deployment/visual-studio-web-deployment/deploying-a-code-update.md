---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Code Updates | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457641"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Code Updates

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht

Nach der erstmaligen Bereitstellung wird die Wartung und Entwicklung Ihrer Website fortgesetzt, und bevor Sie mit der Zeit arbeiten, möchten Sie ein Update bereitstellen. Dieses Tutorial führt Sie durch den Prozess der Bereitstellung eines Updates für den Anwendungscode. Das von Ihnen in diesem Tutorial implementierte und bereitgestellte Update umfasst keine Daten Bank Änderung. im nächsten Tutorial sehen Sie, was die Bereitstellung einer Daten Bank Änderung unterscheidet.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](troubleshooting.md)Behandlung überprüfen.

## <a name="make-a-code-change"></a>Vornehmen einer Codeänderung

Als einfaches Beispiel für ein Update der Anwendung fügen Sie der Seite **Dozenten** eine Liste der Kurse hinzu, die vom ausgewählten Dozenten gelehrt werden.

Wenn Sie die Seite **Dozenten** ausführen, werden Sie feststellen, dass im Raster **ausgewählte** Verknüpfungen vorhanden sind, aber Sie führen nichts anderes aus, als den Zeilen Hintergrund grau zu lassen.

![Dozenten Seite mit Auswahl](deploying-a-code-update/_static/image1.png)

Nun fügen Sie Code hinzu, der ausgeführt wird, wenn auf den Link **auswählen** geklickt wird, und zeigt eine Liste der Kurse an, die vom ausgewählten Dozenten gelehrt werden.

1. Fügen Sie in " *Dozenten. aspx*" direkt nach dem `Label`-Steuerelement **errormessagelabel** das folgende Markup hinzu:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Führen Sie die Seite aus, und wählen Sie einen Dozenten. Es wird eine Liste der Kurse angezeigt, die von diesem Dozenten gelehrt werden.

    ![Dozenten Seite mit Lehrkursen](deploying-a-code-update/_static/image2.png)
3. Schließen Sie den Browser.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Bereitstellen des Code Updates in der Testumgebung

Bevor Sie Ihre Veröffentlichungs Profile für die Bereitstellung in Test-, Staging-und Produktionsumgebungen verwenden können, müssen Sie die Optionen für die Daten Bank Veröffentlichung ändern. Sie müssen die Grant-und Daten Bereitstellungs Skripts für die Mitgliedschafts Datenbank nicht mehr ausführen.

1. Öffnen Sie den Assistenten **Web veröffentlichen** , indem Sie mit der rechten Maustaste auf das Projekt contosouniversity und dann auf **veröffentlichen**klicken.
2. Klicken Sie in der Dropdown Liste **Profil** auf das Profil **Test** .
3. Klicken Sie auf die Registerkarte **Settings** .
4. Deaktivieren Sie im Abschnitt **Datenbanken** unter **DefaultConnection** das Kontrollkästchen **Datenbank aktualisieren** .
5. Klicken Sie auf die Registerkarte **Profil** , und klicken Sie dann in der Dropdown Liste **Profil** **auf das** Stagingprofil.
6. Wenn Sie gefragt werden, ob Sie die am **Test** Profil vorgenommenen Änderungen speichern möchten, klicken Sie auf **Ja**.
7. Nehmen Sie die gleiche Änderung im Stagingprofil vor.
8. Wiederholen Sie den Vorgang, um die gleiche Änderung im Produktionsprofil vorzunehmen.
9. Schließen Sie den Assistenten **Web veröffentlichen** .

Die Bereitstellung in der Testumgebung ist nun eine einfache Frage, dass die One-Click-Veröffentlichung erneut ausgeführt werden kann. Um diesen Vorgang zu beschleunigen, können Sie die Symbolleiste " **Web One Click Publish** " verwenden.

1. Klicken Sie im Menü **Ansicht auf** **Symbolleisten** , und wählen Sie dann **Web One Click Publish**aus.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. Wählen Sie in **Projektmappen-Explorer**das Projekt conjesouniversity aus.
3. **Klicken Sie im Web auf** die Symbolleiste veröffentlichen, wählen **Sie das Profil** Veröffentlichungs Profil aus, und klicken Sie dann auf **Web veröffentlichen** (das Symbol mit Pfeilen nach links und rechts).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser öffnet automatisch die Startseite.
5. Führen Sie die Seite Dozenten aus, und wählen Sie einen Dozenten aus, um sicherzustellen, dass das Update erfolgreich bereitgestellt

Normalerweise würden Sie auch Regressionstests durchführen (Testen Sie den Rest der Site, um sicherzustellen, dass die neue Änderung keinerlei vorhandene Funktionalität beeinträchtigt). In diesem Tutorial überspringen Sie diesen Schritt und stellen das Update für Staging und Produktion bereit.

Wenn Sie die Bereitstellung erneut durchzuführen, bestimmt Web deploy automatisch, welche Dateien geändert wurden, und kopiert nur geänderte Dateien auf den Server. Standardmäßig verwendet Web deploy Datum der letzten Änderung von Dateien, um zu bestimmen, welche geändert wurden. Einige Quell Code Verwaltungssysteme ändern Datumsangaben, auch wenn Sie den Dateiinhalt nicht ändern. In diesem Fall möchten Sie möglicherweise Web deploy so konfigurieren, dass Datei Prüfsummen verwendet werden, um zu bestimmen, welche Dateien geändert wurden. Weitere Informationen finden Sie unter [Warum werden alle meine Dateien erneut bereitgestellt, obwohl ich Sie nicht geändert habe?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in den FAQ zur ASP.NET-Bereitstellung.

## <a name="take-the-application-offline-during-deployment"></a>Offline schalten der Anwendung während der Bereitstellung

Die Änderung, die Sie jetzt bereitstellen, ist eine einfache Änderung an einer einzelnen Seite. Manchmal stellen Sie jedoch größere Änderungen bereit, oder Sie stellen sowohl Code als auch Daten Bank Änderungen bereit, und der Standort verhält sich möglicherweise falsch, wenn ein Benutzer eine Seite anfordert, bevor die Bereitstellung abgeschlossen ist. Wenn Sie verhindern möchten, dass Benutzer auf die Website zugreifen, während die Bereitstellung ausgeführt wird, können Sie eine *App\_offline. htm* -Datei verwenden. Wenn Sie eine Datei mit dem Namen *App\_offline. htm* im Stamm Ordner Ihrer Anwendung ablegen, wird diese Datei automatisch von IIS angezeigt, anstatt Ihre Anwendung zu verwenden. Um den Zugriff während der Bereitstellung zu verhindern, können Sie die *App\_offline. htm* im Stamm Ordner ablegen, den Bereitstellungs Prozess ausführen und dann die *App\_offline. htm* nach erfolgreicher Bereitstellung entfernen.

Sie können Web deploy so konfigurieren, dass beim Starten der Bereitstellung automatisch eine Standard- *App\_Offline* Datei auf dem Server abgelegt wird. Zu diesem Zweck müssen Sie lediglich das folgende XML-Element zu ihrer Veröffentlichungs Profil Datei (. pubxml) hinzufügen:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

In diesem Tutorial erfahren Sie, wie Sie eine benutzerdefinierte *App\_Datei "offline. htm* " erstellen und verwenden.

Die Verwendung von *App-\_offline. htm* auf der Stagingwebsite ist nicht erforderlich, da Benutzer nicht auf die Stagingwebsite zugreifen. Es empfiehlt sich jedoch, die Staging-Methode zu verwenden, um alles so zu testen, wie Sie die Bereitstellung in der Produktion planen.

### <a name="create-app_offlinehtm"></a>App\_offline. htm erstellen

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe, und klicken Sie auf **Hinzufügen**und dann auf **Neues Element**.
2. Erstellen Sie eine **HTML-Seite** mit dem Namen *App\_offline. htm* (Löschen Sie die endgültige "l" in der *HTML* -Erweiterung, die Visual Studio standardmäßig erstellt).
3. Ersetzen Sie das Vorlagen Markup durch das folgende Markup:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Speichern und schließen Sie die Datei.

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a>Kopieren Sie die APP\_"offline. htm" in den Stamm Ordner der Website.

Sie können ein beliebiges FTP-Tool zum Kopieren von Dateien auf die Website verwenden. [FileZilla](http://filezilla-project.org/) ist ein beliebtes FTP-Tool, das in den Screenshots angezeigt wird.

Zum Verwenden eines FTP-Tools benötigen Sie drei Dinge: die FTP-URL, den Benutzernamen und das Kennwort.

Die URL wird auf der Dashboardseite der Website im Azure-Verwaltungsportal angezeigt, und der Benutzername und das Kennwort für FTP finden Sie in der Datei " *. publishsettings* ", die Sie zuvor heruntergeladen haben. Die folgenden Schritte zeigen, wie Sie diese Werte erhalten.

1. Klicken Sie im Azure-Verwaltungsportal auf die Registerkarte **Websites** , und klicken Sie dann auf die Stagingwebsite.
2. Scrollen Sie auf der Seite **Dashboard** nach unten, um den FTP-Hostnamen im Abschnitt **kurzer Blick** zu suchen.

    ![FTP-Hostname](deploying-a-code-update/_static/image5.png)
3. Öffnen Sie die Datei "Staging *. publishsettings* " im Editor oder in einem anderen Text-Editor.
4. Suchen Sie das `publishProfile`-Element für das FTP-Profil.
5. Kopieren Sie die Werte für `userName` und `userPWD`.

    ![FTP-Anmelde Informationen](deploying-a-code-update/_static/image6.png)
6. Öffnen Sie das FTP-Tool, und melden Sie sich bei der FTP-URL an.
7. Kopieren Sie *App-\_"offline. htm* " aus dem Projektmappenordner in den Ordner " */Site/wwwroot* " der Stagingwebsite.

    ![Kopieren App_offline](deploying-a-code-update/_static/image7.png)
8. Navigieren Sie zur URL Ihrer Stagingsite. Sie sehen, dass die Seite *App\_offline. htm* jetzt anstelle der Startseite angezeigt wird.

    ![App_offline. htm im Browserfenster](deploying-a-code-update/_static/image8.png)

Sie sind jetzt bereit für die Bereitstellung in der Staging-Umgebung.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Bereitstellen des Code Updates für Staging und Produktion

1. Klicken Sie im **Web auf** die Symbolleiste veröffentlichen, wählen Sie das **Staging** -Veröffentlichungs Profil aus, und klicken Sie dann auf **Web veröffentlichen**.

    Visual Studio stellt die aktualisierte Anwendung bereit und öffnet den Browser auf der Startseite der Website. Die *App\_offline. htm* -Datei wird angezeigt. Bevor Sie testen können, um die erfolgreiche Bereitstellung zu überprüfen, müssen Sie die *App\_offline. htm* -Datei entfernen.
2. Kehren Sie zu Ihrem FTP-Tool zurück, und löschen Sie **App-\_offline. htm** von der Stagingsite.
3. Öffnen Sie im Browser die Seite Dozenten der Stagingsite, und wählen Sie einen Dozenten aus, um zu überprüfen, ob das Update erfolgreich bereitgestellt wurde.
4. Befolgen Sie das gleiche Verfahren für die Produktion wie bei der Staging-Umgebung.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Überprüfen von Änderungen und Bereitstellen spezifischer Dateien

Visual Studio 2012 bietet Ihnen auch die Möglichkeit, einzelne Dateien bereitzustellen. Für eine ausgewählte Datei können Sie Unterschiede zwischen der lokalen Version und der bereitgestellten Version anzeigen, die Datei in der Zielumgebung bereitstellen oder die Datei aus der Zielumgebung in das lokale Projekt kopieren. In diesem Abschnitt des Tutorials erfahren Sie, wie Sie diese Funktionen verwenden.

### <a name="make-a-change-to-deploy"></a>Vornehmen einer Änderung an der Bereitstellung

1. Öffnen Sie *Content/Site. CSS*, und suchen Sie den Block für das `body`-Tag.
2. Ändern Sie den Wert für `background-color` von `#fff` in `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Anzeigen der Änderung im Vorschau Fenster der Veröffentlichung

Wenn Sie den Assistenten **Web veröffentlichen** verwenden, um das Projekt zu veröffentlichen, können Sie sehen, welche Änderungen veröffentlicht werden, indem Sie im **Vorschaufenster** auf die Datei doppelklicken.

1. Klicken Sie mit der rechten Maustaste auf das Projekt conjesouniversity und dann auf **veröffentlichen**.
2. Wählen Sie in der Dropdown Liste **Profil** das Profil für die **Test** Veröffentlichung aus.
3. Klicken Sie auf **Vorschau**und dann auf **Vorschau starten**.
4. Doppelklicken Sie im **Vorschau** Bereich auf **Site. CSS**.

    ![Doppelklicken Sie auf Site. CSS.](deploying-a-code-update/_static/image9.png)

    Im Dialogfeld **Vorschau der Änderungen** wird eine Vorschau der Änderungen angezeigt, die bereitgestellt werden.

    ![Vorschau der Änderungen an Site. CSS](deploying-a-code-update/_static/image10.png)

    Wenn Sie auf die Datei " *Web. config* " doppelklicken, zeigt das Dialogfeld **Vorschau der Änderungen** die Auswirkung der Transformationen für die Buildkonfiguration und das Veröffentlichen von Profilen an. An diesem Punkt haben Sie nichts unternommen, das dazu führt, dass sich die Datei " *Web. config* " auf dem Server ändert, sodass Sie keine Änderungen sehen. Das Fenster " **Vorschau der Änderungen** " zeigt jedoch fälschlicherweise zwei Änderungen an. Es sieht so aus, als dass zwei XML-Elemente entfernt werden. Diese Elemente werden durch den Veröffentlichungsprozess hinzugefügt, wenn Sie **Code First-Migrationen beim Anwendungsstart ausführen** für eine Code First-Kontext Klasse auswählen. Der Vergleich erfolgt vor dem Hinzufügen dieser Elemente durch den Veröffentlichungsprozess, sodass Sie anscheinend entfernt werden, obwohl Sie nicht entfernt werden. Dieser Fehler wird in einer zukünftigen Version korrigiert.
5. Klicken Sie auf **Schließen**.
6. Klicken Sie auf **Veröffentlichen**.
7. Wenn der Browser mit der Startseite der Test Website geöffnet wird, drücken Sie STRG + F5, um die Auswirkungen der CSS-Änderung anzuzeigen.

    ![Auswirkungen der CSS-Änderung](deploying-a-code-update/_static/image11.png)
8. Schließen Sie den Browser.

### <a name="publish-specific-files-from-solution-explorer"></a>Bestimmte Dateien aus Projektmappen-Explorer veröffentlichen

Angenommen, Sie möchten den blauen Hintergrund nicht kennen und möchten zur ursprünglichen Farbe zurückkehren. In diesem Abschnitt stellen Sie die ursprünglichen Einstellungen wieder her, indem Sie eine bestimmte Datei direkt aus **Projektmappen-Explorer**veröffentlichen.

1. Öffnen Sie *Content/Site. CSS* , und stellen Sie die `background-color` Einstellung auf `#fff`wieder her.
2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Datei *Content/Site. CSS* .

    Das Kontextmenü zeigt drei Veröffentlichungs Optionen an.

    ![Veröffentlichungs Optionen aus Projektmappen-Explorer](deploying-a-code-update/_static/image12.png)
3. Klicken Sie **auf Vorschau der Änderungen an Site. CSS**.

    Ein Fenster wird geöffnet, in dem die Unterschiede zwischen der lokalen Datei und der Version von in der Zielumgebung angezeigt werden.

    ![Diff-Content/Site. CSS](deploying-a-code-update/_static/image13.png)
4. Klicken Sie in **Projektmappen-Explorer**erneut mit der rechten Maustaste auf **Site. CSS** , und klicken Sie dann auf **Site. CSS veröffentlichen**.

    Das Fenster **Webveröffentlichungs Aktivität** zeigt an, dass die Datei veröffentlicht wurde.

    ![Webveröffentlichungs-Aktivitäts Fenster](deploying-a-code-update/_static/image14.png)
5. Öffnen Sie einen Browser mit der `http://localhost/contosouniversity`-URL, und drücken Sie STRG + F5, um eine harte Aktualisierung durchführen, um die Auswirkung der CSS-Änderung anzuzeigen.

    ![Startseite mit normalem CSS](deploying-a-code-update/_static/image15.png)
6. Schließen Sie den Browser.

## <a name="summary"></a>Zusammenfassung

Sie haben nun mehrere Möglichkeiten zum Bereitstellen eines Anwendungs Updates kennengelernt, die keine Daten Bank Änderungen beinhalten, und Sie haben gesehen, wie Sie die Änderungen in der Vorschau anzeigen, um zu überprüfen, ob die aktualisierten Elemente den Erwartungen entsprechen. Die Seite Dozenten verfügt jetzt über einen Kurs, der von **Kursen vermittelt** wird.

![Dozenten Seite mit Lehrkursen](deploying-a-code-update/_static/image16.png)

Im nächsten Tutorial wird gezeigt, wie Sie eine Daten Bank Änderung bereitstellen: Sie fügen der-Datenbank und der Dozenten Seite ein BirthDate-Feld hinzu.

> [!div class="step-by-step"]
> [Zurück](deploying-to-production.md)
> [Weiter](deploying-a-database-update.md)
