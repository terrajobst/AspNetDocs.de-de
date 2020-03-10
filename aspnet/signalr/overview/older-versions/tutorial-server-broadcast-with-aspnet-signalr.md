---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Tutorial: Server Übertragung mit ASP.net signalr 1. x | Microsoft-Dokumentation'
author: bradygaster
description: In diesem Tutorial wird gezeigt, wie eine Webanwendung erstellt wird, die ASP.net signalr zum Bereitstellen von Server Broadcast Funktionen verwendet. Server Broadcast bedeutet, dass Communic...
ms.author: bradyg
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 68908be34f6b010e512677fe5f5e31bfdefab592
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467931"
---
# <a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Tutorial: Server Übertragung mit ASP.net signalr 1. x

von [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Tutorial wird gezeigt, wie eine Webanwendung erstellt wird, die ASP.net signalr zum Bereitstellen von Server Broadcast Funktionen verwendet. Server Broadcast bedeutet, dass die an Clients gesendeten Mitteilungen vom Server initiiert werden. Dieses Szenario erfordert einen anderen Programmier Ansatz als Peer-to-Peer-Szenarios, wie z. b. Chat-Anwendungen, bei denen die an Clients gesendete Kommunikation von mindestens einem Client initiiert wird.
> 
> Die Anwendung, die Sie in diesem Tutorial erstellen, simuliert einen Börsen Ticker, ein typisches Szenario für die Server Broadcast-Funktionalität.
> 
> Kommentare zu diesem Tutorial sind willkommen. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com)veröffentlichen.

## <a name="overview"></a>Übersicht

Das nuget-Paket [Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installiert eine simulierte Beispiel-tickeranwendung in einem Visual Studio-Projekt. Im ersten Teil dieses Tutorials erstellen Sie eine vereinfachte Version der Anwendung von Grund auf neu. Im weiteren Verlauf des Tutorials installieren Sie das nuget-Paket und überprüfen die zusätzlichen Features und Code, die es erstellt.

Die Stock Ticker-Anwendung ist ein Vertreter einer Art von Echtzeitanwendung, in der Sie in regelmäßigen Abständen Benachrichtigungen vom Server an alle verbundenen Clients senden oder übertragen möchten.

Die Anwendung, die Sie im ersten Teil dieses Tutorials erstellen, zeigt ein Raster mit Aktiendaten an.

![StockTicker-anfangs Version](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Der Server aktualisiert die Aktienpreise in regelmäßigen Abständen nach dem Zufallsprinzip und überträgt die Updates an alle verbundenen Clients. Im Browser ändern sich die Zahlen und Symbole in den **Änderungs** -und **%** Spalten dynamisch in Reaktion auf Benachrichtigungen vom Server. Wenn Sie zusätzliche Browser in derselben URL öffnen, zeigen Sie alle dieselben Daten und die gleichen Änderungen an den Daten gleichzeitig an.

Dieses Tutorial enthält die folgenden Abschnitte:

- [Erforderliche Komponenten](#prerequisites)
- [Erstellen des Projekts](#createproject)
- [Hinzufügen der signalr-nuget-Pakete](#nugetpackages)
- [Einrichten des Server Codes](#server)
- [Einrichten des Client Codes](#client)
- [Testen der Anwendung](#test)
- [Aktivieren der Protokollierung](#enablelogging)
- [Vollständiges StockTicker-Beispiel installieren und überprüfen](#fullsample)
- [Nächste Schritte](#nextsteps)

> [!NOTE]
> Wenn Sie die Schritte zum Entwickeln der Anwendung nicht durcharbeiten möchten, können Sie das signalr. Sample-Paket in einem neuen, **leeren ASP.NET-Webanwendungs** Projekt installieren und diese Schritte lesen, um Erläuterungen zum Code zu erhalten. Der erste Teil des Tutorials behandelt eine Teilmenge des signalr. Sample-Codes, und der zweite Teil erläutert die wichtigsten Features der zusätzlichen Funktionalität im signalr. Sample-Paket.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Visual Studio 2012 oder 2010 SP1 auf dem Computer installiert ist. Wenn Sie nicht über Visual Studio verfügen, finden Sie weitere Informationen unter [ASP.net Downloads](https://www.asp.net/downloads) , um das kostenlose Visual Studio 2012 Express für Web zu erhalten.

Wenn Sie über Visual Studio 2010 verfügen, stellen Sie sicher, dass [nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert ist.

<a id="createproject"></a>

## <a name="create-the-project"></a>Erstellen eines Projekts

1. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
2. Erweitern **C#** Sie im Dialogfeld **Neues Projekt** unter **Vorlagen** , und wählen Sie **Web**aus.
3. Wählen Sie die Vorlage **ASP.net Empty Web Application** aus, benennen Sie das Projekt *signalr. StockTicker*, und klicken Sie auf **OK**.

    ![Dialogfeld "Neues Projekt"](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Hinzufügen der signalr-nuget-Pakete

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Hinzufügen der signalr-und jQuery-nuget-Pakete

Sie können einem Projekt signalr-Funktionen hinzufügen, indem Sie ein nuget-Paket installieren.

1. Klicken Sie auf **Tools | Nuget-Paket-Manager | Paket-Manager-Konsole**.
2. Geben Sie den folgenden Befehl in den Paket-Manager ein.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    Das signalr-Paket installiert eine Reihe von anderen nuget-Paketen als Abhängigkeiten. Wenn die Installation abgeschlossen ist, verfügen Sie über alle Server-und Client Komponenten, die für die Verwendung von signalr in einer ASP.NET-Anwendung erforderlich sind.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Einrichten des Servercodes

In diesem Abschnitt richten Sie den Code ein, der auf dem Server ausgeführt wird.

### <a name="create-the-stock-class"></a>Erstellen der Aktienklasse

Beginnen Sie mit dem Erstellen der kursmodell Klasse, die Sie zum Speichern und übertragen von Informationen zu einem Kurs verwenden.

1. Erstellen Sie eine neue Klassendatei im Projektordner, benennen Sie Sie *Stock.cs*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    Die zwei Eigenschaften, die Sie beim Erstellen von Beständen festlegen, sind das Symbol (z. b. msft für Microsoft) und der Preis. Welche anderen Eigenschaften Sie benötigen, hängt davon ab, wie und wann Sie den Preis festlegen. Wenn Sie das erste Mal einen Preis festlegen, wird der Wert an dayopen weitergegeben. Nachfolgende Zeiten, in denen Sie den Preis festlegen, werden die Eigenschaftswerte für Änderung und prozentualänderung basierend auf der Differenz zwischen Preis und dayopen berechnet.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Erstellen der Stock Ticker-und stocktickerhub-Klassen

Sie verwenden die signalr Hub-API, um die Server-zu-Client-Interaktion zu verarbeiten. Eine stocktickerhub-Klasse, die von der signalr-Hub-Klasse abgeleitet wird, verarbeitet den Empfang von Verbindungen und Methoden Aufrufen von Clients. Außerdem müssen Sie Aktiendaten aufbewahren und ein Zeit Geber Objekt ausführen, um Preis Aktualisierungen unabhängig von Clientverbindungen regelmäßig zu initiieren. Diese Funktionen können nicht in einer Hub-Klasse abgelegt werden, da die Hub-Instanzen flüchtig sind. Für jeden Vorgang auf dem Hub wird eine Hub-Klasseninstanz erstellt, z. b. Verbindungen und Aufrufe vom Client an den Server. Der Mechanismus, der die Aktiendaten beibehält, die Preise aktualisiert und die Preis Aktualisierungen überträgt, muss in einer separaten Klasse ausgeführt werden, die Sie StockTicker nennen.

![Senden aus StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Sie möchten nur, dass eine Instanz der Stock Ticker-Klasse auf dem Server ausgeführt wird. Daher müssen Sie einen Verweis von jeder stocktickerhub-Instanz auf die Singleton Stock Ticker-Instanz einrichten. Die StockTicker-Klasse muss an Clients übertragen werden können, da Sie über die Aktiendaten und die triggerupdates verfügt, StockTicker jedoch keine Hub-Klasse ist. Daher muss die StockTicker-Klasse einen Verweis auf das signalr Hub-Verbindungs Kontext Objekt erhalten. Anschließend kann das signalr-Verbindungs Kontext Objekt für die Übertragung an Clients verwendet werden.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Add New Item**.
2. Wenn Sie Visual Studio 2012 mit dem [ASP.net and Web Tools 2012,2-Update](https://go.microsoft.com/fwlink/?LinkId=279941)haben, klicken Sie unter  **C# Visual** auf **Web** , und wählen Sie die Element Vorlage **signalr-Hub-Klasse** aus. Wählen Sie andernfalls die **Klassen** Vorlage aus.
3. Nennen Sie die neue Klasse *StockTickerHub.cs*, und klicken Sie dann auf **Hinzufügen**.

    ![StockTickerHub.cs Hinzufügen](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Ersetzen Sie den Vorlagencode durch den folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    Die [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) -Klasse wird verwendet, um Methoden zu definieren, die von Clients auf dem Server aufgerufen werden können. Sie definieren eine Methode: `GetAllStocks()`. Wenn ein Client zum ersten Mal eine Verbindung mit dem Server herstellt, wird diese Methode aufgerufen, um eine Liste aller Aktien mit ihren aktuellen Preisen zu erhalten. Die-Methode kann synchron ausgeführt werden und `IEnumerable<Stock>` zurückgeben, da Sie Daten aus dem Arbeitsspeicher zurückgibt. Wenn die Methode die Daten abrufen musste, indem Sie eine bestimmte Aktion durchführen, wie z. b. eine Datenbanksuche oder einen Webdienst-Befehl, geben Sie `Task<IEnumerable<Stock>>` als Rückgabewert an, um die asynchrone Verarbeitung zu aktivieren. Weitere Informationen finden Sie unter [ASP.net signalr Hubs API Guide-Server-when for asynchron Execute](index.md).

    Das Attribut hubname gibt an, wie auf den Hub im JavaScript-Code auf dem Client verwiesen wird. Wenn Sie dieses Attribut nicht verwenden, ist der Standardname auf dem Client eine Version des Klassen namens, die in Kamel Buchstaben steht, was in diesem Fall stocktickerhub wäre.

    Wie Sie später sehen werden, wenn Sie die Stock Ticker-Klasse erstellen, wird in der statischen Instance-Eigenschaft eine Singleton Instanz dieser Klasse erstellt. Diese Singleton-Instanz von StockTicker verbleibt im Arbeitsspeicher, unabhängig davon, wie viele Clients eine Verbindung herstellen oder trennen, und diese Instanz verwendet die getallstocks-Methode, um aktuelle Aktieninformationen zurückzugeben.
5. Erstellen Sie eine neue Klassendatei im Projektordner, benennen Sie Sie *StockTicker.cs*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Da mehrere Threads dieselbe Instanz von stocktickcode ausführen, muss die StockTicker-Klasse Thread sicher sein.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Speichern der Singleton-Instanz in einem statischen Feld

    Der Code initialisiert das statische \_Instanzfeld, das die Instanzeigenschaft mit einer Instanz der-Klasse unterstützt. Dies ist die einzige Instanz der-Klasse, die erstellt werden kann, da der Konstruktor als privat markiert ist. Die verzögerte [Initialisierung](https://msdn.microsoft.com/library/dd997286.aspx) wird für das \_Instanzfeld verwendet, nicht aus Leistungsgründen, sondern um sicherzustellen, dass die Instanzerstellung Thread sicher ist.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Jedes Mal, wenn ein Client eine Verbindung mit dem Server herstellt, ruft eine neue Instanz der stocktickerhub-Klasse, die in einem separaten Thread ausgeführt wird, die stockticksingleton-Instanz aus der statischen StockTicker. Instance-Eigenschaft ab, wie Sie zuvor in der Stock tickerhub-Klasse gesehen haben.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Speichern von Aktiendaten in einem ConcurrentDictionary

    Der Konstruktor initialisiert die \_Bestands Auflistung mit einigen Beispiel Daten für die Aktien und getallstocks gibt die Bestände zurück. Wie Sie bereits gesehen haben, wird diese Sammlung von Beständen wiederum von stocktickerhub. getallstocks zurückgegeben, bei der es sich um eine Server Methode in der Hub-Klasse handelt, die von Clients aufgerufen werden kann.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    Die Bestands Sammlung ist als [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) -Typ für Thread Sicherheit definiert. Als Alternative können Sie ein [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) -Objekt verwenden und das Wörterbuch explizit sperren, wenn Sie Änderungen daran vornehmen.

    Bei dieser Beispielanwendung ist es in Ordnung, Anwendungsdaten im Arbeitsspeicher zu speichern und die Daten zu verlieren, wenn die StockTicker-Instanz verworfen wird. In einer realen Anwendung arbeiten Sie mit einem Back-End-Datenspeicher, z. b. einer-Datenbank.

    ### <a name="periodically-updating-stock-prices"></a>Regelmäßiges Aktualisieren von Aktienkursen

    Der-Konstruktor startet ein Timer-Objekt, das regelmäßig Methoden aufruft, die die Aktienpreise nach dem Zufallsprinzip aktualisieren.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    Updatestockprices wird vom Timer aufgerufen, der im State-Parameter NULL übergibt. Vor dem Aktualisieren der Preise wird eine Sperre für das \_updatestockpriceslock-Objekt übernommen. Der Code überprüft, ob die Preise bereits von einem anderen Thread aktualisiert werden, und ruft dann tryupdatestockprice für jede Aktie in der Liste auf. Die tryupdatestockprice-Methode entscheidet, ob der Aktienkurs geändert und wie viel geändert werden muss. Wenn der Aktienkurs geändert wird, wird broadcaststockprice aufgerufen, um die Aktienkurs Änderung an alle verbundenen Clients zu senden.

    Das \_updatingstockprices-Flag ist als [flüchtig](https://msdn.microsoft.com/library/x13ttww7.aspx) gekennzeichnet, um sicherzustellen, dass der Zugriff auf das Flag Thread sicher ist.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    In einer realen Anwendung würde die tryupdatestockprice-Methode einen Webdienst aufrufen, um den Preis zu suchen. in diesem Code wird ein Zufallszahlengenerator verwendet, um Änderungen nach dem Zufallsprinzip vorzunehmen.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>So erhalten Sie den signalr-Kontext, sodass die StockTicker-Klasse an Clients gesendet werden kann

    Da sich die Preisänderungen hier im StockTicker-Objekt befinden, ist dies das Objekt, das eine updatestockprice-Methode für alle verbundenen Clients aufrufen muss. In einer Hub-Klasse verfügen Sie über eine API zum Aufrufen von Client Methoden, aber StockTicker ist nicht von der Hub-Klasse abgeleitet und verfügt über keinen Verweis auf ein Hub-Objekt. Daher muss die StockTicker-Klasse die signalr-Kontext Instanz für die stocktickerhub-Klasse abrufen und diese zum Abrufen von Methoden auf Clients verwenden, damit Sie an verbundene Clients gesendet werden kann.

    Der Code Ruft beim Erstellen der Singleton-Klasseninstanz einen Verweis auf den signalr-Kontext ab, übergibt diesen Verweis an den-Konstruktor, und der-Konstruktor legt ihn in der Clients-Eigenschaft ab.

    Es gibt zwei Gründe, warum Sie den Kontext nur einmal abrufen möchten: das Abrufen des Kontexts ist ein kostspieliger Vorgang, und er stellt sicher, dass die beabsichtigte Reihenfolge der Nachrichten, die an Clients gesendet werden, erhalten bleibt.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Wenn Sie die Clients-Eigenschaft des Kontexts abrufen und in der stocktickerclient-Eigenschaft platzieren, können Sie Code schreiben, um Client Methoden aufzurufen, die wie in einer Hub-Klasse aussehen. Wenn Sie beispielsweise an alle Clients übertragen möchten, können Sie Clients schreiben. alle. updatestockprice (Stock).

    Die updatestockprice-Methode, die Sie in broadcaststockprice aufrufen, ist noch nicht vorhanden. Sie fügen Sie später hinzu, wenn Sie Code schreiben, der auf dem Client ausgeführt wird. Sie können hier auf "updatestockprice" verweisen, da "Clients. all" dynamisch ist, was bedeutet, dass der Ausdruck zur Laufzeit ausgewertet wird. Wenn dieser Methodenaufruf ausgeführt wird, sendet signalr den Methodennamen und den Parameterwert an den Client. wenn der Client über eine Methode mit dem Namen "updatestockprice" verfügt, wird diese Methode aufgerufen, und der Parameterwert wird an ihn übergeben.

    Clients. All bedeutet, dass an alle Clients gesendet wird. Signalr bietet Ihnen andere Optionen, um anzugeben, an welche Clients oder Client Gruppen die Clients gesendet werden sollen. Weitere Informationen finden Sie unter [hubconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrieren der signalr-Route

Der Server muss wissen, welche URL abgefangen und an signalr weitergeleitet werden soll. Zu diesem Zweck fügen Sie der Datei " *Global. asax* " Code hinzu.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Neues Element hinzufügen**.
2. Wählen Sie die Element Vorlage **globale Anwendungsklasse** aus, und klicken Sie dann auf **Hinzufügen**.

    !["Global. asax" hinzufügen](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Fügen Sie den signalr-Routen Registrierungscode der Anwendung\_Start-Methode hinzu:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    Standardmäßig ist die Basis-URL für den gesamten signalr-Datenverkehr "/signalr", und "/signalr/Hubs" wird verwendet, um eine dynamisch generierte JavaScript-Datei abzurufen, die Proxys für alle in der Anwendung verwendeten Hubs definiert. Die maphubs-Methode enthält über Ladungen, mit denen Sie eine andere Basis-URL und bestimmte signalr-Optionen in einer Instanz der [hubconfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) -Klasse angeben können.
4. Fügen Sie am Anfang der Datei eine using-Anweisung hinzu:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Speichern und schließen Sie die Datei *Global. asax* , und erstellen Sie das Projekt.

Sie haben nun das Einrichten des Server Codes abgeschlossen. Im nächsten Abschnitt richten Sie den-Client ein.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Einrichten des Client Codes

1. Erstellen Sie eine neue HTML-Datei im Projektordner, und nennen Sie Sie *StockTicker. html*.
2. Ersetzen Sie den Vorlagencode durch den folgenden Code:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    Der HTML-Code erstellt eine Tabelle mit 5 Spalten, einer Kopfzeile und einer Daten Zeile mit einer einzelnen Zelle, die alle 5 Spalten umfasst. In der Daten Zeile wird "laden..." angezeigt. und werden nur vorübergehend angezeigt, wenn die Anwendung gestartet wird. Der JavaScript-Code entfernt diese Zeile und fügt Ihre Platz Zeilen mit den vom Server abgerufenen Aktiendaten hinzu.

    Die Skript Tags geben die jQuery-Skriptdatei, die signalr-Kern Skriptdatei, die Skriptdatei für signalr-Proxys und eine StockTicker-Skriptdatei an, die Sie später erstellen. Die Skriptdatei für signalr-Proxys, die die URL "/signalr/Hubs" angibt, wird dynamisch generiert und definiert Proxy Methoden für die Methoden in der hubklasse, in diesem Fall für stocktickerhub. getallstocks. Wenn Sie möchten, können Sie diese JavaScript-Datei mithilfe von [signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) -Hilfsprogrammen manuell generieren und die Erstellung dynamischer Dateien im maphubs-Methodenaufrufe deaktivieren.
3. > [!IMPORTANT]
   > Stellen Sie sicher, dass die JavaScript-Datei Verweise in *StockTicker. html* korrekt sind. Stellen Sie also sicher, dass die jQuery-Version in Ihrem Skripttag (1.8.2 im Beispiel) mit der jQuery-Version im Skript Ordner Ihres Projekts identisch ist, und stellen Sie sicher, dass die signalr-Version in Ihrem Skripttag mit der signalr *-Version* im *Skript Ordner Ihres* Projekts identisch ist. Ändern Sie ggf. die Dateinamen in den Skript Tags.
4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *StockTicker. html*, und klicken Sie dann auf **als Start Seite festlegen**.
5. Erstellen Sie eine neue JavaScript-Datei im Projektordner, und nennen Sie Sie *StockTicker. js*.
6. Ersetzen Sie den Vorlagencode durch den folgenden Code:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $. Connection verweist auf die signalr-Proxys. Der Code Ruft einen Verweis auf den Proxy für die stocktickerhub-Klasse ab und legt ihn in der tickervariablen ab. Der Proxy Name ist der Name, der vom Attribut [hubname] festgelegt wurde:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Nachdem alle Variablen und Funktionen definiert wurden, initialisiert die letzte Codezeile in der Datei die signalr-Verbindung, indem die signalr-Start Funktion aufgerufen wird. Die Start-Funktion führt asynchron aus und gibt ein [Verzögertes jQuery-Objekt](http://api.jquery.com/category/deferred-object/)zurück. Dies bedeutet, dass Sie die done-Funktion zum Angeben der Funktion aufruft, die aufgerufen wird, wenn der asynchrone Vorgang abgeschlossen ist.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    Die Init-Funktion Ruft die getallstocks-Funktion auf dem Server auf und verwendet die vom Server zurückgegebenen Informationen, um die Aktien Tabelle zu aktualisieren. Beachten Sie, dass Sie die Kamel-Schreibweise auf dem Client standardmäßig verwenden müssen, obwohl der Methodenname auf dem Server Pascal-Schreibweise ist. Die Regel für die Kamel Schreibweise gilt nur für Methoden, nicht für Objekte. Beispielsweise verweisen Sie auf Aktien. Symbol und Lager. Price, nicht Stock. Symbol oder Stock. Price.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Wenn Sie die Pascal-Schreibweise auf dem Client verwenden oder einen vollständig anderen Methodennamen verwenden möchten, können Sie die hubmethode mit dem Attribut "hubmethodname" auf die gleiche Weise wie die Hub-Klasse mit dem Attribut "hubname" ergänzen.

    In der Init-Methode wird HTML für eine Tabellenzeile für jedes vom Server empfangene Lager Objekt erstellt, indem formatstock aufgerufen wird, um die Eigenschaften des Aktien Objekts zu formatieren, und dann durch Aufrufen von verdrängen (das oben in *StockTicker. js*definiert ist), dass Platzhalter in der RowTemplate-Variablen durch die Eigenschaften Werte der Aktien Objekte ersetzt werden. Der resultierende HTML-Code wird dann an die Aktien Tabelle angehängt.

    Sie rufen init auf, indem Sie Sie als Rückruffunktion übergeben, die ausgeführt wird, nachdem die asynchrone Start Funktion abgeschlossen wurde. Wenn Sie "init" nach dem Aufruf von "Start" als separate JavaScript-Anweisung aufrufen, schlägt die Funktion fehl, da Sie sofort ausgeführt werden würde, ohne zu warten, bis die Start Funktion die Verbindung hergestellt hat. In diesem Fall versucht die Init-Funktion, die getallstocks-Funktion aufzurufen, bevor die Server Verbindung hergestellt wird.

    Wenn der Server den Preis einer Aktien Person ändert, wird updatestockprice auf verbundenen Clients aufgerufen. Die-Funktion wird der Client-Eigenschaft des StockTicker-Proxys hinzugefügt, um Sie für Aufrufe vom Server verfügbar zu machen.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    Die Funktion updatestockprice formatiert ein vom Server empfangenes Aktien Objekt auf die gleiche Weise wie in der Init-Funktion in eine Tabellenzeile. Anstatt die Zeile an die Tabelle zu anhängen, wird die aktuelle Zeile der Aktien Tabelle in der Tabelle gefunden und durch die neue Zeile ersetzt.

<a id="test"></a>

## <a name="test-the-application"></a>Testen der Anwendung

1. Drücken Sie F5, um die Anwendung im Debugmodus auszuführen.

    Die Aktien Tabelle zeigt anfänglich das "laden..." und nach einer kurzen Verzögerung werden die anfänglichen Aktiendaten angezeigt, und die Aktienpreise werden geändert.

    ![Laden](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Anfängliche Aktien Tabelle](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Aktien Tabelle empfängt Änderungen vom Server](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Kopieren Sie die URL aus der Adressleiste des Browsers, und fügen Sie Sie in ein oder mehrere neue Browserfenster ein.

    Die anfängliche Aktien Anzeige ist identisch mit dem ersten Browser, und die Änderungen werden gleichzeitig durchgeführt.
3. Schließen Sie alle Browser, und öffnen Sie einen neuen Browser, und gehen Sie dann zur gleichen URL.

    Das StockTicker-Singleton-Objekt wird weiterhin auf dem Server ausgeführt, sodass die Aktien Tabelle anzeigt, dass sich die Bestände unverändert geändert haben. (Die anfängliche Tabelle mit 0 (null) Änderungs Zahlen wird nicht angezeigt.)
4. Schließen Sie den Browser.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Aktivieren der Protokollierung

Signalr verfügt über eine integrierte Protokollierungsfunktion, die Sie auf dem Client aktivieren können, um die Problembehandlung zu unterstützen. In diesem Abschnitt aktivieren Sie die Protokollierung und sehen anhand von Beispielen, wie Protokolle anzeigen, welche der folgenden Transportmethoden von signalr verwendet wird:

- [Websockets](http://en.wikipedia.org/wiki/WebSocket), die von IIS 8 und aktuellen Browsern unterstützt werden.
- Vom [Server gesendete Ereignisse](http://en.wikipedia.org/wiki/Server-sent_events), die von anderen Browsern als Internet Explorer unterstützt werden.
- " [Forever Frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)", unterstützt von Internet Explorer.
- [AJAX Long](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)-Abruf, von allen Browsern unterstützt.

Für eine beliebige Verbindung wählt signalr die beste Transportmethode aus, die sowohl vom Server als auch vom Client unterstützt wird.

1. Öffnen Sie *StockTicker. js* , und fügen Sie eine Codezeile hinzu, um die Protokollierung direkt vor dem Code zu aktivieren, der die Verbindung am Ende der Datei initialisiert:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Drücken Sie F5, um das Projekt auszuführen.
3. Öffnen Sie das Fenster "Entwicklertools" des Browsers, und wählen Sie die Konsole aus, um die Protokolle anzuzeigen. Möglicherweise müssen Sie die Seite aktualisieren, um die Protokolle von signalr zum Aushandeln der Transportmethode für eine neue Verbindung anzuzeigen.

    Wenn Sie Internet Explorer 10 unter Windows 8 (IIS 8) ausführen, ist die Transportmethode websockets.

    ![Internet Explorer 10 IIS 8-Konsole](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Wenn Sie Internet Explorer 10 unter Windows 7 (IIS 7,5) ausführen, ist die Transportmethode iframe.

    ![IE 10-Konsole, IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    Installieren Sie in Firefox das Firebug-Add-in, um ein Konsolenfenster zu erhalten. Wenn Sie Firefox 19 auf Windows 8 (IIS 8) ausführen, ist die Transportmethode websockets.

    ![Firefox 19 IIS 8-websockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    Wenn Sie Firefox 19 unter Windows 7 (IIS 7,5) ausführen, handelt es sich bei der Transportmethode um vom Server gesendete Ereignisse.

    ![Firefox 19 IIS 7,5-Konsole](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Vollständiges StockTicker-Beispiel installieren und überprüfen

Die StockTicker-Anwendung, die vom nuget-Paket [Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installiert wird, umfasst mehr Features als die vereinfachte Version, die Sie soeben erstellt haben. In diesem Abschnitt des Tutorials installieren Sie das nuget-Paket und überprüfen die neuen Features und den Code, der diese implementiert.

### <a name="install-the-signalrsample-nuget-package"></a>Installieren des "signalr. Sample"-nuget-Pakets

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **nuget-Pakete verwalten**.
2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Online**, geben Sie im Feld **Online suchen** den Wert *signalr. Sample* ein, und klicken Sie dann im Paket **signalr. Sample** auf **Installieren** .

    ![Installieren des signalr. Sample-Pakets](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. Kommentieren Sie in der Datei " *Global. asax* " RouteTable. routes. maphubs (); aus. Zeile, die Sie zuvor in der Anwendung\_Start-Methode hinzugefügt haben.

    Der Code in " *Global. asax* " ist nicht mehr erforderlich, da das signalr. Sample-Paket die signalr-Route in der *App\_"Start/registerhubs. cs"-* Datei registriert:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    Die webactivator-Klasse, auf die durch das Assembly-Attribut verwiesen wird, ist im webactivatorex-nuget-Paket enthalten, das als Abhängigkeit des signalr. Sample-Pakets installiert wird.
4. Erweitern Sie in **Projektmappen-Explorer**den Ordner *signalr. Sample* , der durch die Installation des Pakets signalr. Sample erstellt wurde.
5. Klicken Sie im Ordner *signalr. Sample* mit der rechten Maustaste auf *StockTicker. html*, und klicken Sie dann auf **als Start Seite festlegen**.

    > [!NOTE]
    > Durch die Installation des nuget-Pakets "signalr. Sample" kann die Version von jQuery geändert werden, die Sie im Ordner " *Scripts* " haben. Die neue Datei " *StockTicker. html* ", die das Paket im Ordner " *signalr. Sample* " installiert, ist synchron mit der jQuery-Version, die vom Paket installiert wird. Wenn Sie jedoch die ursprüngliche Datei " *StockTicker. html* " erneut ausführen möchten, müssen Sie möglicherweise zuerst den jQuery-Verweis im Skript-Tag aktualisieren.

### <a name="run-the-application"></a>Ausführen der Anwendung

1. Drücken Sie F5, um die Anwendung auszuführen.

    Zusätzlich zu dem Raster, das Sie zuvor gesehen haben, zeigt die vollständige Börsen Ticker-Anwendung ein Fenster mit horizontaler Bildlauf an, in dem dieselben Aktiendaten angezeigt werden. Wenn Sie die Anwendung zum ersten Mal ausführen, wird der "Markt" als "geschlossen" angezeigt, und Sie sehen ein statisches Raster und ein Tickerfenster, das keinen Bildlauf durchführt.

    ![StockTicker-Bildschirm Start](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Wenn Sie auf " **Markt öffnen**" klicken, wird für das **Live Stock-Ticker** -Feld ein horizontaler Bildlauf durchlaufen, und der Server beginnt, in regelmäßigen Abständen Aktienkurs Änderungen zu übertragen Jedes Mal, wenn sich ein Aktienkurs ändert, werden sowohl das **Live Stock Table** -Raster als auch das **Live Stock-tickfeld** aktualisiert. Wenn die Preisänderung eines Bestands positiv ist, wird der Kurs mit einem grünen Hintergrund angezeigt, und wenn die Änderung negativ ist, wird die Aktie mit einem roten Hintergrund angezeigt.

    ![StockTicker-APP, Markt offen](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    Die Schaltfläche " **Markt schließen** " hält die Änderungen an und beendet den Bildlauf, und die Schaltfläche " **Zurücksetzen** " setzt alle Aktiendaten auf den ursprünglichen Zustand zurück, bevor die Preisänderungen begonnen wurden Wenn Sie mehr Browserfenster öffnen und zur gleichen URL wechseln, sehen Sie, dass die gleichen Daten in jedem Browser dynamisch gleichzeitig aktualisiert werden. Wenn Sie auf eine der Schaltflächen klicken, reagieren alle Browser gleichzeitig auf dieselbe Weise.

### <a name="live-stock-ticker-display"></a>Live Stock Ticker-Anzeige

Die **Live Stock Ticker** -Anzeige ist eine unsortierter Liste in einem div-Element, das von CSS-Stilen in eine einzelne Zeile formatiert wird. Der Ticker wird auf die gleiche Weise wie die Tabelle initialisiert und aktualisiert: durch Ersetzen von Platzhaltern in einer &lt;Li&gt; Vorlagen Zeichenfolge und dynamisches Hinzufügen der &lt;Li&gt; Elemente zum &lt;Element&gt; UL. Der Bildlauf wird mithilfe der jQuery-Funktion "animieren" durchgeführt, um den Rand Links von der ungeordneten Liste innerhalb des div-Vorgangs zu verändern.

Der Stock Ticker-HTML:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

Das Aktien-Ticker-CSS:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Der jQuery-Code, der den Bildlauf durchführt:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Zusätzliche Methoden auf dem Server, die vom Client aufgerufen werden können

Die stocktickerhub-Klasse definiert vier zusätzliche Methoden, die vom Client aufgerufen werden können:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

Openmarket, closemarket und Reset werden als Antwort auf die Schaltflächen oben auf der Seite aufgerufen. Sie veranschaulichen das Muster eines Clients, der eine Zustandsänderung auslöst, die sofort an alle Clients weitergegeben wird. Jede dieser Methoden Ruft eine Methode in der StockTicker-Klasse auf, die sich auf die Markt Zustandsänderung auswirkt und dann den neuen Status überträgt.

In der Stock Ticker-Klasse wird der Marktstatus durch eine marketstate-Eigenschaft verwaltet, die einen marketstate-Enumerationswert zurückgibt:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Jede Methode, die den Marktstatus ändert, wird in einem Sperrblock angezeigt, da die StockTicker-Klasse threadsafe sein muss:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Um sicherzustellen, dass dieser Code Thread sicher ist, wird das Feld \_marketstate, das die Eigenschaft "marketstate" sichert, als flüchtig markiert.

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

Die Methoden "broadcastmarketstatechange" und "broadcastmarketreset" ähneln der von Ihnen bereits erkannten Methode "broadcaststockprice", mit dem Unterschied, dass Sie verschiedene Methoden aufruft, die auf dem Client definiert sind:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Zusätzliche Funktionen auf dem Client, die vom Server aufgerufen werden können

Die Funktion "updatestockprice" verarbeitet nun sowohl das Raster als auch das tickerdisplay und verwendet "jQuery. Color", um rote und grüne Farben zu blinken.

Neue Funktionen in *signalr. StockTicker. js* aktivieren und deaktivieren die Schaltflächen auf der Grundlage des Markt Zustands, und Sie können den horizontalen Bildlauf des tickfensters abbrechen oder starten. Da dem Ticker. Client mehrere Funktionen hinzugefügt werden, werden diese mithilfe der Erweiterungs [Funktion "jQuery](http://api.jquery.com/jQuery.extend/) " hinzugefügt.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Zusätzliche Client Einrichtung nach dem Herstellen der Verbindung

Nachdem der Client die Verbindung hergestellt hat, gibt es einige zusätzliche Aufgaben: ermitteln Sie, ob der Markt geöffnet oder geschlossen ist, um die entsprechende marketopup-oder marketclosed-Funktion aufzurufen, und fügen Sie die Server Methodenaufrufe an die Schaltflächen an.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

Die Server Methoden werden erst nach dem Herstellen der Verbindung mit den Schaltflächen verknüpft, sodass der Code nicht versuchen kann, die Server Methoden aufzurufen, bevor diese verfügbar sind.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine signalr-Anwendung programmieren, die Nachrichten von dem Server sowohl regelmäßig als auch als Reaktion auf Benachrichtigungen von einem beliebigen Client an alle verbundenen Clients überträgt. Das Muster der Verwendung einer Multithread-Singleton-Instanz zur Wartung des Server Zustands kann auch in Online-Spielszenarien mit mehreren Spielen verwendet werden. Ein Beispiel finden Sie [unter dem shootr-Spiel, das auf signalr basiert](https://github.com/NTaylorMullen/ShootR).

Lernprogramme, in denen Peer-zu-Peer-Kommunikations Szenarios angezeigt werden, finden Sie unter [Getting Started with signalr](index.md) und [Echt Zeit Aktualisierung mit signalr](index.md).

Weitere Informationen zu den erweiterten signalr-Entwicklungskonzepten finden Sie auf den folgenden Websites für signalr-Quellcode und-Ressourcen:

- [ASP.NET SignalR](https://asp.net/signalr/)
- [Signalr-Projekt](http://signalr.net/)
- [Signalr GitHub und Beispiele](https://github.com/SignalR/SignalR)
- [Signalr-wiki](https://github.com/SignalR/SignalR/wiki)
