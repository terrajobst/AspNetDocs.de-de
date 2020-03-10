---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: 'ASP.net signalr Hubs-API-Handbuch: .NET-Client (signalr 1. x) | Microsoft-Dokumentation'
author: bradygaster
description: Dieses Dokument bietet eine Einführung in die Verwendung der Hubs-API für signalr Version 2 in .NET-Clients, wie z. b. Windows Store (WinRT), WPF, Silverlight und Nachteile...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 2b22b53c405a865f91b04e677f60b82dd46dbf9b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505971"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>ASP.net signalr Hubs-API-Handbuch: .NET-Client (signalr 1. x)

von [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Dieses Dokument bietet eine Einführung in die Verwendung der Hubs-API für signalr Version 2 in .NET-Clients, wie z. b. Windows Store (WinRT), WPF, Silverlight und Konsolen Anwendungen.
> 
> Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server. In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden. Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden. Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.
> 
> Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird. Eine Einführung in signalr, Hubs und persistente Verbindungen oder ein Tutorial, das zeigt, wie Sie eine komplette signalr-Anwendung erstellen, finden Sie unter [signalr-Getting Started](../getting-started/index.md).

## <a name="overview"></a>Übersicht

Dieses Dokument enthält folgende Abschnitte:

- [Client Einrichtung](#clientsetup)
- [Einrichten einer Verbindung](#establishconnection)

    - [Domänen übergreifende Verbindungen von Silverlight-Clients](#slcrossdomain)
- [Konfigurieren der Verbindung](#configureconnection)

    - [Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-Clients](#maxconnections)
    - [Angeben von Abfrage Zeichen folgen Parametern](#querystring)
    - [Angeben der Transportmethode](#transport)
    - [Angeben von HTTP-Headern](#httpheaders)
    - [Angeben von Client Zertifikaten](#clientcertificate)
- [Erstellen des Hub-Proxys](#proxy)
- [Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann](#callclient)

    - [Methoden ohne Parameter](#clientmethodswithoutparms)
    - [Methoden mit Parametern, angeben von Parametertypen](#clientmethodswithparmtypes)
    - [Methoden mit Parametern, die dynamische Objekte für die Parameter angeben](#clientmethodswithdynamparms)
    - [Vorgehensweise beim Entfernen eines Handlers](#removehandler)
- [Vorgehensweise beim Abrufen von Server Methoden vom Client](#callserver)
- [Behandeln von Ereignissen der Verbindungs Lebensdauer](#connectionlifetime)
- [Behandeln von Fehlern](#handleerrors)
- [Aktivieren der Client seitigen Protokollierung](#logging)
- [WPF-, Silverlight-und Konsolen Anwendungscode Beispiele für Client Methoden, die vom Server aufgerufen werden können](#wpfsl)

Ein Beispiel für .NET-Client Projekte finden Sie in den folgenden Ressourcen:

- [Gustavo-Armenta/signalr-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, Konsolen-App-Beispiele).
- [Damianedwards/signalr-muveshapeer Demo/muveshape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF-Beispiel).
- [Signalr/Microsoft. Aspnet. signalr. Client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) auf GitHub.com (Konsolen-App-Beispiel).

Dokumentation zum Programmieren der Server-oder JavaScript-Clients finden Sie in den folgenden Ressourcen:

- [Leitfaden für signalr-Hubs-API-Server](../guide-to-the-api/hubs-api-guide-server.md)
- [Leitfaden für signalr-Hubs-API-JavaScript-Client](../guide-to-the-api/hubs-api-guide-javascript-client.md)

Links zu API-Referenz Themen beziehen sich auf die .NET 4,5-Version der API. Wenn Sie .NET 4 verwenden, finden Sie weitere Informationen unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Clientsetup

Installieren Sie das nuget-Paket [Microsoft. Aspnet. signalr. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (nicht das Paket [Microsoft. Aspnet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ). Dieses Paket unterstützt WinRT-, Silverlight-, WPF-, Konsolen Anwendungs-und Windows Phone Clients sowohl für .NET 4 als auch für .NET 4,5.

Wenn sich die Version von signalr, die Sie auf dem Client haben, von der auf dem Server fest zufügende Version unterscheidet, kann signalr häufig an den Unterschied angepasst werden. Wenn z. b. signalr, Version 2,0, veröffentlicht wird und Sie diese auf dem Server installieren, unterstützt der Server Clients, auf denen 1.1. x installiert ist, sowie Clients, auf denen 2,0 installiert ist. Wenn der Unterschied zwischen der Version auf dem Server und der Version auf dem Client zu groß ist, löst signalr eine `InvalidOperationException` Ausnahme aus, wenn der Client versucht, eine Verbindung herzustellen. Die Fehlermeldung lautet "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Einrichten einer Verbindung

Bevor Sie eine Verbindung herstellen können, müssen Sie ein `HubConnection` Objekt erstellen und einen Proxy erstellen. Um die Verbindung herzustellen, wenden Sie die `Start`-Methode für das `HubConnection`-Objekt an.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Bei JavaScript-Clients müssen Sie mindestens einen Ereignishandler registrieren, bevor Sie die `Start`-Methode aufrufen, um die Verbindung herzustellen. Dies ist für .NET-Clients nicht erforderlich. Bei JavaScript-Clients erstellt der generierte Proxy Code automatisch Proxys für alle auf dem Server vorhandenen Hubs, und bei der Registrierung eines Handlers wird angegeben, welche Hubs der Client verwenden soll. Für einen .NET-Client erstellen Sie aber manuell Hub-Proxys, daher geht signalr davon aus, dass Sie einen beliebigen Hub verwenden, für den Sie einen Proxy erstellen.

Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen. Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

Die `Start`-Methode wird asynchron ausgeführt. Um sicherzustellen, dass nachfolgende Codezeilen erst ausgeführt werden, nachdem die Verbindung hergestellt wurde, verwenden Sie `await` in einer asynchronen ASP.NET 4,5-Methode oder in einer synchronen Methode `.Wait()`. Verwenden Sie `.Wait()` nicht in einem WinRT-Client.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

Die `HubConnection`-Klasse ist threadsicher.

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Domänen übergreifende Verbindungen von Silverlight-Clients

Informationen zum Aktivieren von Domänen übergreifenden Verbindungen von Silverlight-Clients finden Sie unter [Bereitstellen eines Dienstanbieter übergreifenden Bereichs](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Konfigurieren der Verbindung

Bevor Sie eine Verbindung herstellen, können Sie eine der folgenden Optionen angeben:

- Limit für gleichzeitige Verbindungen.
- Abfrage Zeichen folgen Parameter.
- Die Transportmethode.
- HTTP-Header.
- Client Zertifikate.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-Clients

In WPF-Clients müssen Sie möglicherweise die maximale Anzahl gleichzeitiger Verbindungen mit dem Standardwert 2 erhöhen. Der empfohlene Wert ist 10.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Weitere Informationen finden Sie unter [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Angeben von Abfrage Zeichen folgen Parametern

Wenn Sie Daten an den Server senden möchten, wenn der Client eine Verbindung herstellt, können Sie dem Verbindungs Objekt Abfrage Zeichenfolgen-Parameter hinzufügen. Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Client Code festgelegt wird.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Servercode gelesen wird.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Angeben der Transportmethode

Im Rahmen der Verbindungs Herstellung aushandiert ein signalr-Client normalerweise mit dem Server, um den optimalen Transport zu ermitteln, der von Server und Client unterstützt wird. Wenn Sie bereits wissen, welcher Transport Sie verwenden möchten, können Sie diesen Aushandlungs Prozess umgehen. Um die Transportmethode anzugeben, übergeben Sie ein Transport Objekt an die Start Methode. Im folgenden Beispiel wird gezeigt, wie die Transportmethode im Client Code angegeben wird.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

Der [Microsoft. Aspnet. signalr. Client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) -Namespace enthält die folgenden Klassen, die Sie zum Angeben des Transports verwenden können.

- [Longpollingtransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [Serversenteventstransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [Websockettransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (nur verfügbar, wenn sowohl der Server als auch der Client .NET 4,5 verwenden.)
- [Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (wählt automatisch den optimalen Transport aus, der sowohl vom Client als auch vom Server unterstützt wird. Dies ist der Standard Transport. Wenn Sie diese Angabe an die `Start`-Methode vornehmen, hat dies denselben Effekt wie das Übergeben von nichts.)

Der foreverframe-Transport ist nicht in dieser Liste enthalten, da er nur von Browsern verwendet wird.

Informationen zum Überprüfen der Transportmethode im Servercode finden Sie unter [ASP.net signalr Hubs API Guide-Server-How to Get Information about the Client from the context Property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Weitere Informationen zu Transporten und Fallbacks finden Sie unter [Einführung in signalr-Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Angeben von HTTP-Headern

Um HTTP-Header festzulegen, verwenden Sie die `Headers`-Eigenschaft für das Verbindungs Objekt. Im folgenden Beispiel wird gezeigt, wie Sie einen HTTP-Header hinzufügen.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Angeben von Client Zertifikaten

Verwenden Sie zum Hinzufügen von Client Zertifikaten die `AddClientCertificate`-Methode für das Verbindungs Objekt.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Erstellen des Hub-Proxys

Um Methoden auf dem Client zu definieren, die ein Hub vom Server aufrufen kann, und um Methoden für einen Hub auf dem Server aufzurufen, erstellen Sie einen Proxy für den Hub, indem Sie `CreateHubProxy` für das Verbindungs Objekt aufrufen. Die Zeichenfolge, die Sie an `CreateHubProxy` übergeben, ist der Name der Hub-Klasse oder der Name, der vom `HubName`-Attribut angegeben wird, wenn auf dem Server eine verwendet wurde. Beim Namensvergleich wird die Groß-/Kleinschreibung nicht beachtet

**Hub-Klasse auf dem Server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Erstellen eines Client Proxys für die Hub-Klasse**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Wenn Sie Ihre Hub-Klasse mit einem `HubName`-Attribut versehen, verwenden Sie diesen Namen.

**Hub-Klasse auf dem Server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**Erstellen eines Client Proxys für die Hub-Klasse**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Das Proxy Objekt ist Thread sicher. Wenn Sie `HubConnection.CreateHubProxy` mehrmals mit demselben `hubName`aufzurufen, erhalten Sie tatsächlich dasselbe zwischengespeicherte `IHubProxy` Objekt.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann

Zum Definieren einer Methode, die vom Server aufgerufen werden kann, verwenden Sie die `On`-Methode des Proxys, um einen Ereignishandler zu registrieren.

Beim Methodennamen wird die Groß-/Kleinschreibung beachtet. Beispielsweise werden auf dem-Server `Clients.All.UpdateStockPrice` auf dem-Server `updateStockPrice`, `updatestockprice`oder `UpdateStockPrice` auf dem Client ausgeführt.

Für verschiedene Client Plattformen gelten andere Anforderungen, wie Sie Methoden Code schreiben, um die Benutzeroberfläche zu aktualisieren. Die gezeigten Beispiele sind für WinRT-Clients (Windows Store .net). Beispiele für WPF, Silverlight und Konsolen Anwendungen werden in [einem separaten Abschnitt weiter unten in diesem Thema](#wpfsl)bereitgestellt.

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Methoden ohne Parameter

Wenn die Methode, die Sie behandeln, keine Parameter enthält, verwenden Sie die nicht generische Überladung der `On`-Methode:

**Server Code Aufruf der Client Methode ohne Parameter**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**WinRT-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird ([siehe WPF-und Silverlight-Beispiele weiter unten in diesem Thema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Methoden mit Parametern, die Parametertypen angeben

Wenn die Methode, die Sie behandeln, über Parameter verfügt, geben Sie die Typen der Parameter als generische Typen der `On` Methode an. Es gibt generische über Ladungen der `On`-Methode, die es Ihnen ermöglichen, bis zu 8 Parameter (4 auf Windows Phone 7) anzugeben. Im folgenden Beispiel wird ein Parameter an die `UpdateStockPrice`-Methode gesendet.

**Server Code zum Aufrufen der Client Methode mit einem Parameter**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Die für den Parameter verwendete Aktienklasse**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**WinRT-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird ([Siehe Beispiele zu WPF und Silverlight weiter unten in diesem Thema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Methoden mit Parametern, die dynamische Objekte für die Parameter angeben

Als Alternative zum Angeben von Parametern als generische Typen der `On`-Methode können Sie Parameter als dynamische Objekte angeben:

**Server Code zum Aufrufen der Client Methode mit einem Parameter**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Die für den Parameter verwendete Aktienklasse**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**WinRT-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird ([siehe WPF-und Silverlight-Beispiele weiter unten in diesem Thema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Vorgehensweise beim Entfernen eines Handlers

Um einen Handler zu entfernen, müssen Sie dessen `Dispose`-Methode aufzurufen.

**Client Code für eine vom Server aufgerufene Methode**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Client Code zum Entfernen des Handlers**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Vorgehensweise beim Abrufen von Server Methoden vom Client

Um eine Methode auf dem Server aufzurufen, verwenden Sie die `Invoke`-Methode des Hub-Proxys.

Wenn die Server Methode über keinen Rückgabewert verfügt, verwenden Sie die nicht generische Überladung der `Invoke`-Methode.

**Server Code für eine Methode, die über keinen Rückgabewert verfügt**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Client Code zum Aufrufen einer Methode ohne Rückgabewert**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Wenn die Server Methode über einen Rückgabewert verfügt, geben Sie den Rückgabetyp als generischen Typ der `Invoke` Methode an.

**Server Code für eine Methode, die über einen Rückgabewert verfügt und einen komplexen Typparameter annimmt**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Die für den Parameter und den Rückgabewert verwendete Aktienklasse**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**Client Code, der eine Methode mit einem Rückgabewert und einem komplexen Typparameter in einer ASP.NET 4,5 Async-Methode aufrufen**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Client Code, der eine Methode mit einem Rückgabewert und einem komplexen Typparameter in einer synchronen Methode aufrufen**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

Die `Invoke`-Methode wird asynchron ausgeführt und gibt ein `Task`-Objekt zurück. Wenn Sie `await` oder `.Wait()`nicht angeben, wird die nächste Codezeile ausgeführt, bevor die von Ihnen aufgerufene Methode die Ausführung abgeschlossen hat.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Behandeln von Ereignissen der Verbindungs Lebensdauer

Signalr stellt die folgenden Ereignisse für die Verbindungs Lebensdauer bereit, die Sie behandeln können:

- `Received`: wird ausgelöst, wenn Daten über die Verbindung empfangen werden. Stellt die empfangenen Daten bereit.
- `ConnectionSlow`: wird ausgelöst, wenn der Client eine langsame oder häufig gelöschter Verbindung erkennt.
- `Reconnecting`: wird ausgelöst, wenn der zugrunde liegende Transport mit dem erneuten Verbinden beginnt.
- `Reconnected`: wird ausgelöst, wenn der zugrunde liegende Transport die Verbindung wieder hergestellt hat.
- `StateChanged`: wird ausgelöst, wenn sich der Verbindungsstatus ändert. Stellt den alten und den neuen Zustand bereit. Weitere Informationen zu Verbindungs Zustands Werten finden Sie unter [ConnectionState-Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: wird ausgelöst, wenn die Verbindung getrennt wurde.

Wenn Sie z. b. Warnmeldungen für Fehler anzeigen möchten, die nicht schwerwiegend sind, aber zeitweilig auftretende Verbindungsprobleme verursachen, z. b. verlangsamtheit oder häufiges Löschen der Verbindung, behandeln Sie das `ConnectionSlow`-Ereignis.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](../guide-to-the-api/handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Behandeln von Fehlern

Wenn Sie auf dem Server nicht explizit detaillierte Fehlermeldungen aktivieren, enthält das Ausnahme Objekt, das signalr nach einem Fehler zurückgibt, nur minimale Informationen über den Fehler. Wenn beispielsweise ein `newContosoChatMessage` Fehler auftritt, enthält die Fehlermeldung im Fehler Objekt den Wert "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`", dass detaillierte Fehlermeldungen an Clients in der Produktionsumgebung nicht gesendet werden sollen. Wenn Sie jedoch ausführliche Fehlermeldungen zu Problem Behandlungszwecken aktivieren möchten, verwenden Sie den folgenden Code auf dem Server.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Um Fehler zu behandeln, die von signalr ausgelöst werden, können Sie einen Handler für das `Error`-Ereignis für das Verbindungs Objekt hinzufügen.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

Um Fehler von Methoden aufrufen zu behandeln, wrappen Sie den Code in einem try-catch-Block.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Aktivieren der Client seitigen Protokollierung

Um die Client seitige Protokollierung zu aktivieren, legen Sie die Eigenschaften `TraceLevel` und `TraceWriter` für das Verbindungs Objekt fest.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF-, Silverlight-und Konsolen Anwendungscode Beispiele für Client Methoden, die vom Server aufgerufen werden können

Die zuvor gezeigten Codebeispiele zum Definieren von Client Methoden, die vom Server aufgerufen werden können, gelten für WinRT-Clients. In den folgenden Beispielen wird der entsprechende Code für WPF-, Silverlight-und Konsolen Anwendungs Clients angezeigt.

### <a name="methods-without-parameters"></a>Methoden ohne Parameter

**WPF-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Silverlight-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Client Code der Konsolenanwendung für die Methode, die vom Server ohne Parameter aufgerufen wird**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Methoden mit Parametern, die Parametertypen angeben

**WPF-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Silverlight-Client Code für eine Methode, die von einem Server mit einem Parameter aufgerufen wird**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Client Code der Konsolenanwendung für eine Methode, die von einem Server mit einem Parameter aufgerufen wird**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Methoden mit Parametern, die dynamische Objekte für die Parameter angeben

**WPF-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Silverlight-Client Code für eine Methode, die von Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Client Code für Konsolen Anwendungen für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
