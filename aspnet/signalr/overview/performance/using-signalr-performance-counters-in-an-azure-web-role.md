---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Verwenden von signalr-Leistungsindikatoren in einer Azure-webrolle | Microsoft-Dokumentation
author: guardrex
description: Installieren und Verwenden von signalr-Leistungsindikatoren in einer Azure-webrolle.
keywords: ASP. net, signalr, Leistungs Counter, Azure-webrolle
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467565"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Verwenden von signalr-Leistungsindikatoren in einer Azure-webrolle

Von [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Signalr-Leistungsindikatoren werden verwendet, um die Leistung Ihrer APP in einer Azure-webrolle zu überwachen. Die Leistungsindikatoren werden von Microsoft Azure-Diagnose aufgezeichnet. Sie installieren signalr-Leistungsindikatoren in Azure mit *signalr. exe*, dem gleichen Tool, das auch für eigenständige oder lokale Apps verwendet wird. Da Azure-Rollen flüchtig sind, konfigurieren Sie eine APP so, dass Sie signalr-Leistungsindikatoren beim Start installieren und registrieren.

## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio 2015 oder [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Microsoft Azure SDK für Visual Studio](https://azure.microsoft.com/downloads/) **Hinweis: Starten Sie den Computer neu, nachdem Sie das SDK installiert haben.**
* Microsoft Azure Abonnement: Informationen zur Registrierung für ein kostenloses Azure-Testkonto finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Erstellen einer Azure-Webrollen Anwendung, die signalr-Leistungsindikatoren verfügbar macht

1. Öffnen Sie Visual Studio.

2. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.

3. Wählen Sie im Dialogfeld **Neues Projekt** auf der linken Seite die Kategorie **Visual C#**  > **Cloud** aus, und wählen Sie dann die Vorlage **Azure-clouddienst** aus. Benennen Sie die APP **signalrperfcounters** , und wählen Sie **OK**aus.

   ![Neue cloudanwendung](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Wenn die **Cloud** -Vorlagen Kategorie oder die Azure- **clouddienstvorlage** nicht angezeigt wird, müssen Sie die Arbeitsauslastung für die **Azure-Entwicklung** für Visual Studio 2017 installieren. Wählen Sie unten links im Dialogfeld **Neues Projekt** den Link **Öffnen Visual Studio-Installer** aus, um Visual Studio-Installer zu öffnen. Wählen Sie die Arbeitsauslastung für die **Azure-Entwicklung** , und wählen Sie dann **ändern** aus, um mit der Installation der
   >
   > ![Azure-Entwicklungs Arbeitsauslastung in Visual Studio-Installer](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. Wählen Sie im Dialogfeld **neuer Microsoft Azure Cloud-Dienst** die Option **ASP.net Web Role** aus, und klicken Sie auf die Schaltfläche >, um die Rolle dem Projekt hinzuzufügen. Klicken Sie auf **OK**.

   ![ASP.net-webrolle hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. Wählen Sie im Dialogfeld **neue ASP.NET Webanwendung WebRole1** die **MVC** -Vorlage aus, und klicken Sie dann auf **OK**.

   ![Hinzufügen von MVC und Web-API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. Öffnen Sie in **Projektmappen-Explorer**die Datei *Diagnostics. wadcfgx* unter **WebRole1**.

   ![Projektmappen-Explorer Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Ersetzen Sie den Inhalt der Datei durch die folgende Konfiguration, und speichern Sie die Datei:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Öffnen Sie die **Paket-Manager-Konsole** über die **Tools** > **nuget-Paket-Manager**. Geben Sie die folgenden Befehle ein, um die neueste Version von signalr und des signalr-Hilfsprogramme-Pakets zu installieren:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Konfigurieren Sie die APP, um die signalr-Leistungsindikatoren in der Rollen Instanz zu installieren, wenn Sie gestartet oder wieder verwendet wird. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **WebRole1** , und wählen Sie > **neuen Ordner** **Hinzufügen** aus. Nennen Sie den neuen Ordner *Startup*.

   ![Startordner hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Kopieren Sie die Datei *signalr. exe* (die mit dem Paket **Microsoft. Aspnet. signalr. utils** hinzugefügt wurde) aus \<Projektordner >/SignalRPerfCounters/Packages/Microsoft.Aspnet.SignalR.utils.\<Version >/Tools auf den *Start* Ordner, den Sie im vorherigen Schritt erstellt haben.

11. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Start* , und wählen Sie > **Vorhandenes Element** **Hinzufügen** . Wählen Sie im angezeigten Dialogfeld die Option *signalr. exe* aus, und klicken Sie auf **Hinzufügen**.

    !["Signalr. exe" zu Projekt hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Klicken Sie mit der rechten Maustaste auf den erstellten *Start* Ordner. Klicken Sie dann auf **Hinzufügen** > **Neues Element**. Wählen Sie den Knoten **Allgemein** aus, wählen Sie **Textdatei**aus, und benennen Sie das neue Element *signalrperfcounterinstall. cmd*. Mit dieser Befehlsdatei werden die signalr-Leistungsindikatoren in der webrolle installiert.

    ![Erstellen einer Batchdatei für die Installation von signalr](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Wenn die Datei " *signalrperfcounterinstall. cmd* " von Visual Studio erstellt wird, wird Sie automatisch im Hauptfenster geöffnet. Ersetzen Sie den Inhalt der Datei durch das folgende Skript, speichern Sie die Datei, und schließen Sie Sie. Mit diesem Skript wird die *signalr. exe*-Anweisung ausgeführt, mit der die signalr-Leistungsindikatoren der Rollen Instanz hinzugefügt werden.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Wählen Sie in **Projektmappen-Explorer**die Datei *signalr. exe* aus. Legen Sie in den **Eigenschaften**der Datei in **Ausgabeverzeichnis kopieren** auf **immer kopieren**fest.

    ![In Ausgabeverzeichnis kopieren auf immer kopieren festlegen](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. Wiederholen Sie den vorherigen Schritt für die Datei " *signalrperfcounterinstall. cmd* ".

16. Klicken Sie mit der rechten Maustaste auf die Datei *signalrperfcounterinstall. cmd* , und wählen Sie **Öffnen mit**aus. Wählen Sie im angezeigten Dialogfeld die Option **Binär-Editor** aus, und klicken Sie auf **OK**.

    ![Mit binärem Editor öffnen](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. Wählen Sie im Binär-Editor alle führenden Bytes in der Datei aus, und löschen Sie Sie. Speichern und schließen Sie die Datei.

    ![Führende Bytes löschen](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Öffnen Sie *Service Definition. csdef* , und fügen Sie einen Starttask hinzu, der die Datei *signalrperfcounterinstall. cmd* ausführt, wenn der Dienst gestartet wird:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Öffnen Sie `Views/Shared/_Layout.cshtml`, und entfernen Sie das jQuery-Bundle-Skript am Ende der Datei.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Fügen Sie einen JavaScript-Client hinzu, der fortlaufend die `increment`-Methode auf dem Server aufruft. Öffnen Sie `Views/Home/Index.cshtml`, und ersetzen Sie den Inhalt durch den folgenden Code:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Erstellen Sie im **WebRole1** -Projekt einen neuen Ordner namens *Hubs*. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Hubs* , und wählen Sie > **Neues Element** **Hinzufügen** . Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Kategorie **Web** > **signalr** aus, und wählen Sie dann die Element Vorlage **signalr Hub-Klasse (v2)** aus. Benennen Sie den neuen Hub *MyHub.cs* , und wählen Sie **Hinzufügen**aus.

    ![Hinzufügen der signalr Hub-Klasse zum Ordner Hubs im Dialogfeld Neues Element hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* wird automatisch im Hauptfenster geöffnet. Ersetzen Sie den Inhalt durch den folgenden Code, und speichern und schließen Sie dann die Datei:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. " *[Crank. exe](signalr-connection-density-testing-with-crank.md)* " ist ein Tool zur Verbindungs Dichte Überprüfung, das mit der signalr-CodeBase bereitgestellt wird Da für die Kurbel eine persistente Verbindung erforderlich ist, fügen Sie Ihrer Website für die Verwendung beim Testen einen hinzu. Fügen Sie dem **WebRole1** -Projekt einen neuen Ordner mit dem Namen *persistentconnections*hinzu. Klicken Sie mit der rechten Maustaste auf diesen Ordner, und wählen Sie **Hinzufügen** >  Benennen Sie die neue Klassendatei *MyPersistentConnections.cs* , und wählen Sie **Hinzufügen**aus.

24. Die Datei *MyPersistentConnections.cs* wird im Hauptfenster von Visual Studio geöffnet. Ersetzen Sie den Inhalt durch den folgenden Code, und speichern und schließen Sie dann die Datei:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. Mit der `Startup`-Klasse beginnen die signalr-Objekte, wenn owin gestartet wird. Öffnen oder erstellen Sie *Startup.cs* , und ersetzen Sie den Inhalt durch den folgenden Code:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    Im obigen Code markiert das `OwinStartup`-Attribut diese Klasse zum Starten von owin. Die `Configuration`-Methode startet signalr.

26. Testen Sie die Anwendung im Microsoft Azure-Emulator, indem Sie **F5**drücken.

    > [!NOTE]
    > Wenn bei **mapsignalr**eine **FileLoadException-Ausnahme** auftritt, ändern Sie die Bindungs Umleitungen in der Datei " *Web. config* " wie folgt:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Warten Sie ungefähr eine Minute. Öffnen Sie das Cloud-Explorer Tool Fenster in Visual Studio ( > **Cloud-Explorer** **anzeigen** ), und erweitern Sie den Pfad `(Local)/Storage Accounts/(Development)/Tables`. Doppelklicken Sie auf **wadperformancecounterstable**. In den Tabellendaten sollten signalr-Leistungsindikatoren angezeigt werden. Wenn die Tabelle nicht angezeigt wird, müssen Sie möglicherweise Ihre Azure Storage Anmelde Informationen erneut eingeben. Möglicherweise müssen Sie auf die Schaltfläche " **Aktualisieren** " klicken, um die Tabelle in **Cloud-Explorer** anzuzeigen, oder die Schaltfläche " **Aktualisieren** " im geöffneten Tabellenfenster auswählen, um die Daten in der Tabelle anzuzeigen.

    ![Auswählen der Tabelle "wad-Leistungsindikatoren" in Visual Studio Cloud-Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Die in der Tabelle "wad-Leistungsindikatoren" gesammelten Indikatoren werden angezeigt.](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Um Ihre Anwendung in der Cloud zu testen, aktualisieren Sie die Datei **serviceconfiguration. Cloud. cscfg** , und legen Sie die `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` auf eine gültige Verbindungs Zeichenfolge für Azure Storage Konto fest.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Stellen Sie die Anwendung in Ihrem Azure-Abonnement bereit. Ausführliche Informationen zum Bereitstellen einer Anwendung in Azure finden Sie unter [Erstellen und Bereitstellen eines clouddiensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Warten Sie ein paar Minuten. Suchen Sie in **Cloud-Explorer**das zuvor konfigurierte Speicherkonto, und suchen Sie die `WADPerformanceCountersTable`-Tabelle darin. In den Tabellendaten sollten signalr-Leistungsindikatoren angezeigt werden. Wenn die Tabelle nicht angezeigt wird, müssen Sie möglicherweise Ihre Azure Storage Anmelde Informationen erneut eingeben. Möglicherweise müssen Sie auf die Schaltfläche " **Aktualisieren** " klicken, um die Tabelle in **Cloud-Explorer** anzuzeigen, oder die Schaltfläche " **Aktualisieren** " im geöffneten Tabellenfenster auswählen, um die Daten in der Tabelle anzuzeigen.

Besonders vielen Dank an [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) für den ursprünglichen Inhalt, der in diesem Tutorial verwendet wird.
