---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Server Übertragung mit signalr 2 | Microsoft-Dokumentation'
author: tdykstra
description: In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um Server Broadcast Funktionen bereitzustellen.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431241"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: Server Übertragung mit signalr 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um Server Broadcast Funktionen bereitzustellen. Server Broadcast bedeutet, dass der Server die an Clients gesendeten Mitteilungen startet.

Die Anwendung, die Sie in diesem Tutorial erstellen, simuliert einen Börsen Ticker, ein typisches Szenario für die Server Broadcast-Funktionalität. Der Server aktualisiert die Aktienpreise in regelmäßigen Abständen nach dem Zufallsprinzip und überträgt die Updates an alle verbundenen Clients. Im Browser ändern sich die Zahlen und Symbole in den **Änderungs** -und **%** Spalten dynamisch in Reaktion auf Benachrichtigungen vom Server. Wenn Sie zusätzliche Browser in derselben URL öffnen, zeigen Sie alle dieselben Daten und die gleichen Änderungen an den Daten gleichzeitig an.

![Web erstellen](tutorial-server-broadcast-with-signalr/_static/image1.png)

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen eines Projekts
> * Einrichten des Servercodes
> * Überprüfen des Server Codes
> * Einrichten des Client Codes
> * Überprüfen des Client Codes
> * Testen der Anwendung
> * Aktivieren der Protokollierung

> [!IMPORTANT]
> Wenn Sie die Schritte zum Entwickeln der Anwendung nicht durcharbeiten möchten, können Sie das signalr. Sample-Paket in einem neuen, leeren ASP.NET-Webanwendungs Projekt installieren. Wenn Sie das nuget-Paket installieren, ohne die Schritte in diesem Tutorial auszuführen, müssen Sie die Anweisungen in der Datei "Infodatei *. txt* " befolgen. Zum Ausführen des Pakets müssen Sie eine owin-Startklasse hinzufügen, die die `ConfigureSignalR`-Methode im installierten Paket aufruft. Wenn Sie die owin-Startklasse nicht hinzufügen, erhalten Sie eine Fehlermeldung. Weitere Informationen finden Sie im Abschnitt [Installieren des stocktickbeispiels](#install-the-stockticker-sample) in diesem Artikel.

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**

## <a name="create-the-project"></a>Erstellen eines Projekts

In diesem Abschnitt wird gezeigt, wie Sie mit Visual Studio 2017 eine leere ASP.NET-Webanwendung erstellen.

1. Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.

    ![Web erstellen](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. Lassen Sie im Fenster **neue ASP.NET Webanwendung-signalr. StockTicker** den Eintrag **leer** , und wählen Sie **OK**aus.

## <a name="set-up-the-server-code"></a>Einrichten des Servercodes

In diesem Abschnitt richten Sie den Code ein, der auf dem Server ausgeführt wird.

### <a name="create-the-stock-class"></a>Erstellen der Aktienklasse

Beginnen Sie mit dem Erstellen *der Kurs* Modell Klasse, die Sie zum Speichern und übertragen von Informationen zu einem Kurs verwenden.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Klasse** **Hinzufügen** .

1. Benennen Sie die Klasse *Stock* , und fügen Sie Sie dem Projekt hinzu.

1. Ersetzen Sie den Code in der Datei *Stock.cs* durch den folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Die zwei Eigenschaften, die Sie beim Erstellen von Beständen festlegen, werden `Symbol` (z. b. msft für Microsoft) und `Price`. Die anderen Eigenschaften sind davon abhängig, wie und wann Sie `Price`festlegen. Wenn Sie `Price`zum ersten Mal festlegen, wird der Wert an `DayOpen`weitergegeben. Nachdem Sie `Price`festgelegt haben, berechnet die APP die `Change`-und `PercentChange` Eigenschaftswerte basierend auf dem Unterschied zwischen `Price` und `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Erstellen der Stock tickerhub-und StockTicker-Klassen

Sie verwenden die signalr Hub-API, um die Server-zu-Client-Interaktion zu verarbeiten. Eine `StockTickerHub` Klasse, die von der signalr-`Hub` Klasse abgeleitet wird, verarbeitet den Empfang von Verbindungen und Methoden Aufrufen von Clients. Außerdem müssen Sie Aktiendaten aufbewahren und ein `Timer` Objekt ausführen. Das `Timer`-Objekt löst regelmäßig Preis Aktualisierungen unabhängig von Clientverbindungen aus. Diese Funktionen können nicht in einer `Hub` Klasse abgelegt werden, da Hubs flüchtig sind. Die App erstellt eine Instanz einer `Hub`-Klasse für jede Aufgabe auf dem Hub, wie z. b. Verbindungen und Aufrufe vom Client zum Server. Der Mechanismus, der Aktiendaten beibehält, Preise aktualisiert und übermittelt, muss in einer separaten Klasse ausgeführt werden. Benennen Sie die Klasse `StockTicker`.

![Senden aus StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Sie möchten nur, dass eine Instanz der `StockTicker`-Klasse auf dem Server ausgeführt werden kann. Daher müssen Sie einen Verweis von jeder `StockTickerHub`-Instanz auf die Singleton `StockTicker`-Instanz einrichten. Die `StockTicker` Klasse muss an Clients übertragen werden, da Sie über die Aktiendaten und die triggerupdates verfügt, `StockTicker` aber keine `Hub` Klasse ist. Die `StockTicker` Klasse muss einen Verweis auf das signalr-Hub-Verbindungs Kontext Objekt erhalten. Anschließend kann das signalr-Verbindungs Kontext Objekt für die Übertragung an Clients verwendet werden.

#### <a name="create-stocktickerhubcs"></a>StockTickerHub.cs erstellen

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.

1. Wählen Sie unter **Neues Element hinzufügen-signalr. StockTicker**die Option **installiert** > **Visual C#**  > **Web** > **signalr** aus, und wählen Sie dann **signalr Hub-Klasse (v2)** aus.

1. Benennen Sie die Klasse *stocktickerhub* , und fügen Sie Sie dem Projekt hinzu.

    In diesem Schritt wird die *StockTickerHub.cs* -Klassendatei erstellt. Gleichzeitig werden dem Projekt ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die signalr unterstützen.

1. Ersetzen Sie den Code in der Datei *StockTickerHub.cs* durch den folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Speichern Sie die Datei .

Die APP verwendet die [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) -Klasse, um Methoden zu definieren, die von Clients auf dem Server aufgerufen werden können. Sie definieren eine Methode: `GetAllStocks()`. Wenn ein Client zum ersten Mal eine Verbindung mit dem Server herstellt, wird diese Methode aufgerufen, um eine Liste aller Aktien mit ihren aktuellen Preisen zu erhalten. Die-Methode kann synchron ausgeführt werden und `IEnumerable<Stock>` zurückgeben, da Sie Daten aus dem Arbeitsspeicher zurückgibt.

Wenn die Methode die Daten abrufen musste, indem Sie eine bestimmte Aktion durchführen, wie z. b. eine Datenbanksuche oder einen Webdienst-Befehl, geben Sie `Task<IEnumerable<Stock>>` als Rückgabewert an, um die asynchrone Verarbeitung zu aktivieren. Weitere Informationen finden Sie unter [ASP.net signalr Hubs API Guide-Server-when for asynchron Execute](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

Das `HubName`-Attribut gibt an, wie die APP auf den Hub im JavaScript-Code auf dem Client verweist. Wenn Sie dieses Attribut nicht verwenden, ist der Standardname auf dem Client eine "CamelCase"-Version des Klassen namens, die in diesem Fall `stockTickerHub`ist.

Wie Sie später sehen werden, wenn Sie die `StockTicker`-Klasse erstellen, erstellt die APP eine Singleton Instanz dieser Klasse in der statischen `Instance`-Eigenschaft. Diese Singleton Instanz `StockTicker` befindet sich im Arbeitsspeicher, unabhängig davon, wie viele Clients eine Verbindung herstellen oder trennen. Diese Instanz wird von der `GetAllStocks()`-Methode verwendet, um aktuelle Aktieninformationen zurückzugeben.

#### <a name="create-stocktickercs"></a>StockTicker.cs erstellen

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Klasse** **Hinzufügen** .

1. Benennen Sie die Klasse *StockTicker* , und fügen Sie Sie dem Projekt hinzu.

1. Ersetzen Sie den Code in der Datei *StockTicker.cs* durch den folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Da alle Threads dieselbe Instanz von stocktickcode ausführen, muss die StockTicker-Klasse Thread sicher sein.

### <a name="examine-the-server-code"></a>Überprüfen des Server Codes

Wenn Sie den Servercode untersuchen, hilft er Ihnen, die Funktionsweise der APP zu verstehen.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Speichern der Singleton-Instanz in einem statischen Feld

Der Code initialisiert das statische `_instance` Feld, das die `Instance`-Eigenschaft mit einer Instanz der-Klasse unterstützt. Da der Konstruktor privat ist, ist dies die einzige Instanz der-Klasse, die von der App erstellt werden kann. Die APP verwendet die verzögerte [Initialisierung](/dotnet/framework/performance/lazy-initialization) für das `_instance` Feld. Dies ist aus Leistungsgründen nicht möglich. Es muss sichergestellt werden, dass die Instanzerstellung Thread sicher ist.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Jedes Mal, wenn ein Client eine Verbindung mit dem Server herstellt, ruft eine neue Instanz der stocktickerhub-Klasse, die in einem separaten Thread ausgeführt wird, die StockTicker Singleton-Instanz aus der `StockTicker.Instance` static-Eigenschaft ab, wie Sie zuvor in der `StockTickerHub`-Klasse gesehen haben.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Speichern von Aktiendaten in einem ConcurrentDictionary

Der Konstruktor initialisiert die `_stocks` Auflistung mit einigen Beispiel Daten für die Aktien, und `GetAllStocks` gibt die Bestände zurück. Wie Sie bereits gesehen haben, wird diese Auflistung von Beständen von `StockTickerHub.GetAllStocks`zurückgegeben. dabei handelt es sich um eine Server Methode in der `Hub` Klasse, die von Clients aufgerufen werden kann.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

Die Bestands Sammlung ist als [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) -Typ für Thread Sicherheit definiert. Als Alternative können Sie ein [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) -Objekt verwenden und das Wörterbuch explizit sperren, wenn Sie Änderungen daran vornehmen.

Bei dieser Beispielanwendung ist es in Ordnung, Anwendungsdaten im Arbeitsspeicher zu speichern und die Daten zu verlieren, wenn die APP die `StockTicker` Instanz freigibt. In einer echten Anwendung würden Sie mit einem Back-End-Datenspeicher wie einer Datenbank arbeiten.

#### <a name="periodically-updating-stock-prices"></a>Regelmäßiges Aktualisieren von Aktienkursen

Der-Konstruktor startet ein `Timer` Objekt, das regelmäßig Methoden aufruft, die Aktienkurse nach dem Zufallsprinzip aktualisieren.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` ruft `UpdateStockPrices`auf, der im State-Parameter NULL übergibt. Vor dem Aktualisieren der Preise nimmt die APP eine Sperre für das `_updateStockPricesLock` Objekt an. Der Code prüft, ob die Preise bereits von einem anderen Thread aktualisiert werden, und ruft dann `TryUpdateStockPrice` für jeden bestand in der Liste auf. Die `TryUpdateStockPrice`-Methode entscheidet, ob der Aktienkurs geändert und wie viel geändert werden muss. Wenn sich der Aktienkurs ändert, ruft die APP `BroadcastStockPrice` auf, um die Aktienkurs Änderung an alle verbundenen Clients zu senden.

Das `_updatingStockPrices` Flag, das als [flüchtig](https://msdn.microsoft.com/library/x13ttww7.aspx) gekennzeichnet ist, um sicherzustellen, dass es Thread sicher ist

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

In einer echten Anwendung ruft die `TryUpdateStockPrice`-Methode einen Webdienst auf, um den Preis zu suchen. In diesem Code verwendet die APP einen Zufallszahlengenerator, um Änderungen nach dem Zufallsprinzip vorzunehmen.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>So erhalten Sie den signalr-Kontext, sodass die StockTicker-Klasse an Clients gesendet werden kann

Da die Preisänderungen hier in das `StockTicker` Objekt kommen, ist das Objekt, das eine `updateStockPrice` Methode auf allen verbundenen Clients abrufen muss. In einer `Hub`-Klasse verfügen Sie über eine API zum Aufrufen von Client Methoden, `StockTicker` jedoch nicht von der `Hub`-Klasse abgeleitet ist und keinen Verweis auf ein `Hub` Objekt hat. Zum Übertragen an verbundene Clients muss die `StockTicker` Klasse die signalr-Kontext Instanz für die `StockTickerHub`-Klasse abrufen und diese zum Abrufen von Methoden auf Clients verwenden.

Der Code Ruft einen Verweis auf den signalr-Kontext ab, wenn er die Singleton-Klasseninstanz erstellt, übergibt diesen Verweis an den Konstruktor, und der Konstruktor legt ihn in der `Clients`-Eigenschaft ab.

Es gibt zwei Gründe dafür, warum Sie den Kontext nur einmal erhalten möchten: das erhalten des Kontexts ist eine aufwändige Aufgabe und der einmalige Abschluss stellt sicher, dass die APP die beabsichtigte Reihenfolge der an die Clients gesendeten Nachrichten beibehält.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Wenn Sie die `Clients`-Eigenschaft des Kontexts abrufen und in der `StockTickerClient`-Eigenschaft platzieren, können Sie Code schreiben, um Client Methoden aufzurufen, die wie in einer `Hub`-Klasse aussehen. Zum Senden von Daten an alle Clients können Sie beispielsweise `Clients.All.updateStockPrice(stock)`schreiben.

Die `updateStockPrice` Methode, die Sie in `BroadcastStockPrice` aufrufen, ist noch nicht vorhanden. Sie fügen Sie später hinzu, wenn Sie Code schreiben, der auf dem Client ausgeführt wird. Sie können hier auf `updateStockPrice` verweisen, da `Clients.All` dynamisch ist, was bedeutet, dass die APP den Ausdruck zur Laufzeit evaluiert. Wenn dieser Methodenaufrufe ausgeführt wird, sendet signalr den Methodennamen und den Parameterwert an den Client. wenn der Client über eine Methode mit dem Namen `updateStockPrice`verfügt, ruft die APP diese Methode auf und übergibt den Parameterwert an den Client.

`Clients.All` bedeutet, dass an alle Clients gesendet wird. Signalr bietet Ihnen andere Optionen, um anzugeben, an welche Clients oder Client Gruppen die Clients gesendet werden sollen. Weitere Informationen finden Sie unter [hubconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrieren der signalr-Route

Der Server muss wissen, welche URL abgefangen und an signalr weitergeleitet werden soll. Fügen Sie hierzu eine owin-Startklasse hinzu:

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.

1. Wählen Sie unter **Neues Element hinzufügen-signalr. StockTicker** die Option **installiert** > **Visual C#**  > **Web** aus, und wählen Sie dann **owin-Startklasse**aus.

1. Benennen Sie die Klasse *Startup* , und wählen Sie **OK**aus.

1. Ersetzen Sie den Standard Code in der Datei *Startup.cs* durch den folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Die Einrichtung des Server Codes ist nun abgeschlossen. Im nächsten Abschnitt richten Sie den-Client ein.

## <a name="set-up-the-client-code"></a>Einrichten des Client Codes

In diesem Abschnitt richten Sie den Code ein, der auf dem Client ausgeführt wird.

### <a name="create-the-html-page-and-javascript-file"></a>Erstellen der HTML-Seite und der JavaScript-Datei

Auf der HTML-Seite werden die Daten angezeigt, und die JavaScript-Datei organisiert die Daten.

#### <a name="create-stocktickerhtml"></a>Erstellen von StockTicker. html

Zuerst fügen Sie den HTML-Client hinzu.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **HTML-Seite** **Hinzufügen** .

1. Nennen Sie die Datei *StockTicker* , und wählen Sie **OK**aus.

1. Ersetzen Sie den Standard Code in der Datei " *StockTicker. html* " durch den folgenden Code:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    Der HTML-Code erstellt eine Tabelle mit fünf Spalten, einer Kopfzeile und einer Daten Zeile mit einer einzelnen Zelle, die alle fünf Spalten umfasst. In der Daten Zeile wird "laden..." angezeigt. vorübergehend, wenn die APP gestartet wird. Der JavaScript-Code entfernt diese Zeile und fügt Ihre Platz Zeilen mit den vom Server abgerufenen Aktiendaten hinzu.

    Die Skript Tags geben Folgendes an:

    * Die jQuery-Skriptdatei.

    * Die signalr Core-Skriptdatei.

    * Die Skriptdatei für signalr-Proxys.

    * Eine StockTicker-Skriptdatei, die Sie später erstellen.

    Die APP generiert dynamisch die Skriptdatei für signalr-Proxys. Sie gibt die URL "/signalr/Hubs" an und definiert Proxy Methoden für die Methoden in der hubklasse, in diesem Fall für die `StockTickerHub.GetAllStocks`. Wenn Sie möchten, können Sie diese JavaScript-Datei mithilfe von [signalr-Hilfsprogramme](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)manuell generieren. Vergessen Sie nicht, die Erstellung dynamischer Dateien im `MapHubs` Methoden aufzurufen.

1. Erweitern Sie in **Projektmappen-Explorer**den Eintrag **Skripts**.

    Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.

    > [!IMPORTANT]
    > Der Paket-Manager installiert eine spätere Version der signalr-Skripts.

1. Aktualisieren Sie die Skript Verweise im Codeblock, damit Sie den Versionen der Skriptdateien im Projekt entsprechen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *StockTicker. html*, und wählen Sie dann **als Start Seite festlegen**aus.

#### <a name="create-stocktickerjs"></a>Erstellen von "StockTicker. js"

Erstellen Sie jetzt die JavaScript-Datei.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **JavaScript-Datei** **Hinzufügen** .

1. Nennen Sie die Datei *StockTicker* , und wählen Sie **OK**aus.

1. Fügen Sie den folgenden Code in die Datei " *StockTicker. js* " ein:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Überprüfen des Client Codes

Wenn Sie den Client Code überprüfen, erfahren Sie, wie der Client Code mit dem Servercode interagiert, damit die APP funktioniert.

#### <a name="starting-the-connection"></a>Verbindung wird gestartet

`$.connection` verweist auf die signalr-Proxys. Der Code Ruft einen Verweis auf den Proxy für die `StockTickerHub`-Klasse ab und legt ihn in der `ticker`-Variablen ab. Der Proxy Name ist der Name, der vom `HubName`-Attribut festgelegt wurde:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Nachdem Sie alle Variablen und Funktionen definiert haben, initialisiert die letzte Codezeile in der Datei die signalr-Verbindung durch Aufrufen der signalr-`start` Funktion. Die `start`-Funktion wird asynchron ausgeführt und gibt ein [Verzögertes jQuery-Objekt](http://api.jquery.com/category/deferred-object/)zurück. Sie können die done-Funktion aufzurufen, um die Funktion anzugeben, die aufgerufen werden soll, wenn die APP die asynchrone Aktion beendet.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Alle Bestände erhalten

Die `init`-Funktion Ruft die `getAllStocks`-Funktion auf dem Server auf und verwendet die vom Server zurückgegebenen Informationen, um die Aktien Tabelle zu aktualisieren. Beachten Sie, dass Sie auf dem Client standardmäßig die "camelschreibweise" verwenden müssen, obwohl der Methodenname auf dem Server Pascal-Schreibweise ist. Die Regel für die Groß-/Kleinschreibung gilt nur für Methoden, nicht für Objekte. Beispielsweise verweisen Sie auf `stock.Symbol` und `stock.Price`, nicht auf `stock.symbol` oder `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

In der `init`-Methode erstellt die APP HTML für eine Tabellenzeile für jedes vom Server empfangene Lager Objekt, indem `formatStock` zum Formatieren von Eigenschaften des `stock` Objekts aufgerufen wird. Anschließend wird `supplant` aufgerufen, um die Platzhalter in der `rowTemplate` Variablen durch die Eigenschaftswerte `stock` Objekts zu ersetzen. Der resultierende HTML-Code wird dann an die Aktien Tabelle angehängt.

> [!NOTE]
> Sie werden `init` aufgerufen, indem Sie Sie als `callback` Funktion übergeben, die ausgeführt wird, nachdem die asynchrone `start` Funktion abgeschlossen wurde. Wenn Sie nach dem Aufrufen von `start``init` als separate JavaScript-Anweisung aufgerufen haben, schlägt die Funktion fehl, da Sie sofort ausgeführt werden würde, ohne zu warten, bis die Start Funktion die Verbindung hergestellt hat. In diesem Fall versucht die `init` Funktion, die `getAllStocks`-Funktion aufzurufen, bevor die APP eine Server Verbindung herstellt.

#### <a name="getting-updated-stock-prices"></a>Erhalten von aktualisierten Aktienkursen

Wenn der Server den Kurs eines Aktien ändert, wird der `updateStockPrice` auf verbundenen Clients aufgerufen. Die APP fügt die Funktion der Client-Eigenschaft des `stockTicker` Proxys hinzu, um Sie für Aufrufe vom Server verfügbar zu machen.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

Die `updateStockPrice`-Funktion formatiert ein vom Server empfangenes Aktien Objekt auf die gleiche Weise wie in der `init`-Funktion in eine Tabellenzeile. Anstatt die Zeile an die Tabelle zu anhängen, wird die aktuelle Zeile der Aktien Tabelle in der Tabelle gefunden und diese Zeile durch die neue Zeile ersetzt.

## <a name="test-the-application"></a>Testen der Anwendung

Sie können die APP testen, um sicherzustellen, dass Sie funktioniert. Sie sehen, dass in allen Browserfenstern die Live Aktien Tabelle angezeigt wird, bei der Aktienkurse schwanken.

1. Aktivieren Sie auf der Symbolleiste das **Skript Debugging** , und wählen Sie dann die Schaltfläche wiedergeben aus, um die APP im Debugmodus auszuführen.

    ![Screenshot des Benutzers zum Aktivieren des Debugmodus und zum Auswählen von Play.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Ein Browserfenster wird geöffnet, in dem die **Live Aktien Tabelle**angezeigt wird. Die Aktien Tabelle zeigt anfänglich das "laden..." Line, dann zeigt die APP nach kurzer Zeit die anfänglichen Aktiendaten an, und die Aktienpreise werden geändert.

1. Kopieren Sie die URL aus dem Browser, öffnen Sie zwei andere Browser, und fügen Sie die URLs in die Adressleiste ein.

    Die anfängliche Aktien Anzeige ist identisch mit dem ersten Browser, und die Änderungen werden gleichzeitig durchgeführt.

1. Schließen Sie alle Browser, öffnen Sie einen neuen Browser, und wechseln Sie zur gleichen URL.

    Das StockTicker-Singleton-Objekt wird weiterhin auf dem Server ausgeführt. Die **Live Aktien Tabelle** zeigt an, dass sich die Bestände weiterhin geändert haben. Die Anfangs Tabelle mit 0 (null) Änderungs Zahlen wird nicht angezeigt.

1. Schließen Sie den Browser.

## <a name="enable-logging"></a>Aktivieren der Protokollierung

Signalr verfügt über eine integrierte Protokollierungsfunktion, die Sie auf dem Client aktivieren können, um die Problembehandlung zu unterstützen. In diesem Abschnitt aktivieren Sie die Protokollierung und sehen anhand von Beispielen, wie Protokolle anzeigen, welche der folgenden Transportmethoden von signalr verwendet wird:

* [Websockets](http://en.wikipedia.org/wiki/WebSocket), die von IIS 8 und aktuellen Browsern unterstützt werden.

* Vom [Server gesendete Ereignisse](http://en.wikipedia.org/wiki/Server-sent_events), die von anderen Browsern als Internet Explorer unterstützt werden.

* " [Forever Frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)", unterstützt von Internet Explorer.

* [AJAX Long](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)-Abruf, von allen Browsern unterstützt.

Für eine beliebige Verbindung wählt signalr die beste Transportmethode aus, die sowohl vom Server als auch vom Client unterstützt wird.

1. Öffnen Sie *StockTicker. js*.

1. Fügen Sie diese hervorgehobene Codezeile hinzu, um die Protokollierung direkt vor dem Code zu aktivieren, der die Verbindung am Ende der Datei initialisiert:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Drücken Sie **F5**, um das Projekt auszuführen.

1. Öffnen Sie das Fenster "Entwicklertools" des Browsers, und wählen Sie die Konsole aus, um die Protokolle anzuzeigen. Möglicherweise müssen Sie die Seite aktualisieren, um die Protokolle von signalr zum Aushandeln der Transportmethode für eine neue Verbindung anzuzeigen.

    * Wenn Sie Internet Explorer 10 unter Windows 8 (IIS 8) ausführen, ist die Transportmethode **websockets**.

    * Wenn Sie Internet Explorer 10 unter Windows 7 (IIS 7,5) ausführen, ist die Transportmethode **iframe**.

    * Wenn Sie Firefox 19 auf Windows 8 (IIS 8) ausführen, ist die Transportmethode **websockets**.

        > [!TIP]
        > Installieren Sie in Firefox das Firebug-Add-in, um ein Konsolenfenster zu erhalten.

    * Wenn Sie Firefox 19 unter Windows 7 (IIS 7,5) ausführen, handelt es sich bei der Transportmethode um vom **Server gesendete** Ereignisse.

## <a name="install-the-stockticker-sample"></a>Installieren des StockTicker-Beispiels

[Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installiert die StockTicker-Anwendung. Das nuget-Paket enthält mehr Features als die vereinfachte Version, die Sie von Grund auf neu erstellt haben. In diesem Abschnitt des Tutorials installieren Sie das nuget-Paket und überprüfen die neuen Features und den Code, der diese implementiert.

> [!IMPORTANT]
> Wenn Sie das Paket installieren, ohne die vorherigen Schritte dieses Tutorials auszuführen, müssen Sie dem Projekt eine owin-Startklasse hinzufügen. Dieser Schritt wird in dieser Datei "Infodatei. txt" für das nuget-Paket erläutert.

### <a name="install-the-signalrsample-nuget-package"></a>Installieren des "signalr. Sample"-nuget-Pakets

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

1. Wählen Sie im **nuget-Paket-Manager: signalr. StockTicker**die Option **Durchsuchen**aus.

1. Wählen Sie aus **Paketquelle**die Option **nuget.org**aus.

1. Geben Sie *signalr. Sample* in das Suchfeld ein, und wählen Sie " **Microsoft. Aspnet. signalr. Sample** > **install**" aus.

1. Erweitern Sie in **Projektmappen-Explorer**den Ordner *signalr. Sample* .

    Bei der Installation des Pakets "signalr. Sample" wurden der Ordner und dessen Inhalt erstellt.

1. Klicken Sie im Ordner *signalr. Sample* mit der rechten Maustaste auf *StockTicker. html*, und wählen Sie dann **als Start Seite festlegen**aus.

    > [!NOTE]
    > Durch die Installation des nuget-Pakets "signalr. Sample" kann die Version von jQuery geändert werden, die Sie im Ordner " *Scripts* " haben. Die neue Datei " *StockTicker. html* ", die das Paket im Ordner " *signalr. Sample* " installiert, ist synchron mit der jQuery-Version, die vom Paket installiert wird. Wenn Sie jedoch die ursprüngliche Datei " *StockTicker. html* " erneut ausführen möchten, müssen Sie möglicherweise zuerst den jQuery-Verweis im Skript-Tag aktualisieren.

### <a name="run-the-application"></a>Ausführen der Anwendung

 Die Tabelle, die Sie in der ersten APP gesehen haben, hatte nützliche Features. Die vollständige Börsen Ticker-Anwendung zeigt neue Features an: ein horizontales Bild Lauf Fenster, in dem die Aktiendaten und-Bestände angezeigt werden, die die Farbe bei steigender und steigender Farbe ändern

1. Drücken Sie **F5**, um die App auszuführen.

     Wenn Sie die APP zum ersten Mal ausführen, wird der "Markt" als "geschlossen" angezeigt, und Sie sehen eine statische Tabelle und ein Tickerfenster, das keinen Bildlauf durchführt.

1. Wählen Sie **Open Market**aus.

    ![Screenshot des Live-Tickers.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * Im Feld " **Live Stock Ticker** " wird ein horizontaler Bildlauf durchlaufen, und der Server beginnt, in regelmäßigen Abständen Aktienkurs Änderungen zu übertragen.

    * Jedes Mal, wenn sich ein Aktienkurs ändert, aktualisiert die APP sowohl die **Live Aktien Tabelle** als auch den **Live Stock-Ticker**.

    * Wenn die Preisänderung eines Bestands positiv ist, zeigt die APP die Aktien mit einem grünen Hintergrund an.

    * Wenn die Änderung negativ ist, wird die Aktie in der APP mit einem roten Hintergrund angezeigt.

1. Wählen Sie **Markt beenden**aus.

    * Die Tabellen Updates werden beendet.

    * Der Ticker beendet den Bildlauf.

1. Klicken Sie auf **Zurücksetzen**.

    * Alle Aktiendaten werden zurückgesetzt.

    * Die APP stellt den Anfangszustand vor dem Start der Preisänderungen wieder her.

1. Kopieren Sie die URL aus dem Browser, öffnen Sie zwei andere Browser, und fügen Sie die URLs in die Adressleiste ein.

1. Sie sehen, dass die gleichen Daten in jedem Browser dynamisch gleichzeitig aktualisiert werden.

1. Wenn Sie eines der Steuerelemente auswählen, reagieren alle Browser gleichzeitig auf dieselbe Weise.

### <a name="live-stock-ticker-display"></a>Live Stock Ticker-Anzeige

Die **Live Stock Ticker** -Anzeige ist eine ungeordnete Liste in einem `<div>`-Element, das in einer einzelnen Zeile von CSS-Stilen formatiert ist. Die APP initialisiert und aktualisiert den Ticker auf dieselbe Weise wie die Tabelle: durch Ersetzen von Platzhaltern in einer `<li>` Vorlagen Zeichenfolge und dynamisches Hinzufügen der `<li>` Elemente zum `<ul>` Element. Die APP umfasst den Bildlauf mithilfe der jQuery-`animate` Funktion, um den Rand Links von der ungeordneten Liste innerhalb der `<div>`zu variieren.

#### <a name="signalrsample-stocktickerhtml"></a>Signalr. Sample StockTicker. html

Der HTML-Code des Aktien Tickers:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>Signalr. Sample StockTicker. CSS

Der Aktien-Ticker-CSS-Code:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>Signalr. Sample signalr. StockTicker. js

Der jQuery-Code, der den Bildlauf durchführt:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Zusätzliche Methoden auf dem Server, die vom Client aufgerufen werden können

Um der APP Flexibilität zu verleihen, gibt es zusätzliche Methoden, die die App abrufen kann.

#### <a name="signalrsample-stocktickerhubcs"></a>Signalr. Sample StockTickerHub.cs

Die `StockTickerHub`-Klasse definiert vier zusätzliche Methoden, die vom Client aufgerufen werden können:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

Die App Ruft `OpenMarket`, `CloseMarket`und `Reset` als Antwort auf die Schaltflächen oben auf der Seite auf. Sie veranschaulichen das Muster eines Clients, der eine Änderung des Zustands auslöst, der sofort an alle Clients weitergegeben wird. Jede dieser Methoden Ruft eine Methode in der `StockTicker` Klasse auf, die die Markt Zustandsänderung verursacht und dann den neuen Status überträgt.

#### <a name="signalrsample-stocktickercs"></a>Signalr. Sample StockTicker.cs

In der `StockTicker`-Klasse behält die APP den Marktstatus mit einer `MarketState`-Eigenschaft bei, die einen `MarketState` Enumerationswert zurückgibt:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Jede Methode, die den Marktstatus ändert, wird in einem Sperrblock angezeigt, da die `StockTicker` Klasse Thread sicher sein muss:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Um sicherzustellen, dass dieser Code Thread sicher ist, wird das `_marketState` Feld, das die `MarketState` Eigenschaft unterstützt, die `volatile`festgelegt ist:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Die Methoden `BroadcastMarketStateChange` und `BroadcastMarketReset` ähneln der von Ihnen bereits erkannten broadcaststockprice-Methode, mit der Ausnahme, dass Sie verschiedene Methoden aufruft, die auf dem Client definiert sind:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Zusätzliche Funktionen auf dem Client, die vom Server aufgerufen werden können

Die `updateStockPrice`-Funktion verarbeitet nun sowohl die Tabelle als auch die tickeranzeige und verwendet `jQuery.Color`, um rote und grüne Farben zu blinken.

Neue Funktionen in *signalr. StockTicker. js* aktivieren und deaktivieren die Schaltflächen auf der Grundlage des Markt Zustands. Außerdem wird der horizontale **Bildlauf** für den Live Kurs-Ticker beendet oder gestartet. Da viele Funktionen `ticker.client`hinzugefügt werden, verwendet die APP die [Funktion "jQuery Extend](http://api.jquery.com/jQuery.extend/) ", um Sie hinzuzufügen.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Zusätzliche Client Einrichtung nach dem Herstellen der Verbindung

Nachdem der Client die Verbindung hergestellt hat, müssen zusätzliche Aufgaben ausgeführt werden:

* Stellen Sie fest, ob der Markt geöffnet oder geschlossen ist, um die entsprechende `marketOpened` oder `marketClosed` Funktion aufzurufen.

* Fügen Sie die Server Methodenaufrufe an die Schaltflächen an.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Die Server Methoden sind erst dann mit den Schaltflächen verknüpft, wenn die APP die Verbindung herstellt. Dies ist der Fall, wenn der Code die Server Methoden nicht abrufen kann, bevor diese verfügbar sind.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

In diesem Tutorial haben Sie gelernt, wie Sie eine signalr-Anwendung programmieren, die Nachrichten vom Server an alle verbundenen Clients überträgt. Nun können Sie Nachrichten in regelmäßigen Abständen und als Antwort auf Benachrichtigungen von einem beliebigen Client übertragen. Sie können das Konzept der Multithread-Singleton-Instanz verwenden, um den Serverstatus in Online-Spielszenarien mit mehreren spielen beizubehalten. Ein Beispiel finden Sie [auf der Grundlage von signalr im shootr-Spiel](https://github.com/NTaylorMullen/ShootR).

Lernprogramme, in denen Peer-zu-Peer-Kommunikations Szenarios angezeigt werden, finden Sie unter [Getting Started with signalr](introduction-to-signalr.md) und [Echt Zeit Aktualisierung mit signalr](tutorial-high-frequency-realtime-with-signalr.md).

Weitere Informationen zu signalr finden Sie in den folgenden Ressourcen:

* [ASP.NET SignalR](../../index.md)
* [Signalr-Projekt](http://signalr.net/)
* [Signalr GitHub und Beispiele](https://github.com/SignalR/SignalR)
* [Signalr-wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Das Projekt wurde erstellt.
> * Einrichten des Servercodes
> * Untersuchen des Server Codes
> * Einrichten des Client Codes
> * Der Client Code wurde untersucht.
> * Testen der Anwendung
> * Aktivierte Protokollierung

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie eine echt Zeit Webanwendung erstellen, die ASP.net signalr 2 verwendet.
> [!div class="nextstepaction"]
> [Erstellen einer echt Zeit-Web-App mit signalr](real-time-web-applications-with-signalr.md)
