---
uid: signalr/overview/older-versions/troubleshooting
title: Problembehandlung bei signalr (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: In diesem Artikel werden häufige Probleme beim Entwickeln von signalr-Anwendungen beschrieben.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 3dbf091ac2daa4da477b405852bb4d1316584fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468099"
---
# <a name="signalr-troubleshooting-signalr-1x"></a>Problembehandlung für SignalR (SignalR 1.x)

von [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Dokument werden häufige Probleme bei der Problembehandlung in signalr beschrieben.

Dieses Dokument enthält die folgenden Abschnitte.

- [Aufrufen von Methoden zwischen Client und Server im Hintergrund fehlgeschlagen](#connection)
- [Andere Verbindungsprobleme](#other)
- [Kompilierung und serverseitige Fehler](#server)
- [Visual Studio-Probleme](#vs)
- [Internetinformationsdienste Probleme](#iis)
- [Azure-Probleme](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Aufrufen von Methoden zwischen Client und Server im Hintergrund fehlgeschlagen

In diesem Abschnitt werden mögliche Ursachen für den Fehler eines Methoden Aufrufes zwischen Client und Server beschrieben, ohne dass eine sinnvolle Fehlermeldung vorliegt. In einer signalr-Anwendung enthält der Server keine Informationen zu den Methoden, die vom Client implementiert werden. Wenn der Server eine Client Methode aufruft, werden der Methodenname und die Parameterdaten an den Client gesendet, und die Methode wird nur ausgeführt, wenn Sie in dem vom Server angegebenen Format vorhanden ist. Wenn keine übereinstimmende Methode auf dem Client gefunden wird, geschieht nichts, und auf dem Server wird keine Fehlermeldung ausgelöst.

Zur weiteren Untersuchung von Client Methoden, die nicht aufgerufen werden, können Sie die Protokollierung aktivieren, bevor Sie die Start-Methode auf dem Hub aufrufen, um festzustellen, welche Aufrufe vom Server stammen. Informationen zum Aktivieren der Protokollierung in einer JavaScript-Anwendung finden Sie unter [Aktivieren der Client seitigen Protokollierung (JavaScript-Client Version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Informationen zum Aktivieren der Protokollierung in einer .NET-Client Anwendung finden Sie unter [Aktivieren der Client seitigen Protokollierung (.NET-Client Version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Falsch geschriebene Methode, falsche Methoden Signatur oder falscher Hub-Name.

Wenn der Name oder die Signatur einer aufgerufenen Methode nicht genau mit einer entsprechenden Methode auf dem Client übereinstimmt, schlägt der Aufruf fehl. Vergewissern Sie sich, dass der vom Server aufgerufene Methodenname mit dem Namen der Methode auf dem Client übereinstimmt. Außerdem erstellt signalr den hubproxy mithilfe von Kamel Schreib Methoden, wie es in JavaScript angebracht ist, sodass eine Methode namens "`SendMessage`" auf dem Server im Client Proxy `sendMessage` aufgerufen wird. Wenn Sie das `HubName`-Attribut im serverseitigen Code verwenden, überprüfen Sie, ob der verwendete Name mit dem Namen übereinstimmt, der zum Erstellen des Hubs auf dem Client verwendet wurde. Wenn Sie das `HubName`-Attribut nicht verwenden, überprüfen Sie, ob der Name des Hubs in einem JavaScript-Client Kamel-Schreibweise ist, z. b. chathub anstelle von chathub.

### <a name="duplicate-method-name-on-client"></a>Doppelter Methodenname auf Client

Vergewissern Sie sich, dass auf dem Client keine doppelte Methode vorhanden ist Wenn Ihre Client Anwendung über eine Methode namens `sendMessage`verfügt, stellen Sie sicher, dass nicht auch eine Methode namens `SendMessage` vorhanden ist.

### <a name="missing-json-parser-on-the-client"></a>Fehlender JSON-Parser auf dem Client

Für signalr muss ein JSON-Parser vorhanden sein, um Aufrufe zwischen dem Server und dem Client zu serialisieren. Wenn Ihr Client nicht über einen integrierten JSON-Parser (z. b. Internet Explorer 7) verfügt, müssen Sie einen in Ihre Anwendung einschließen. Sie können den JSON-Parser [hier](http://nuget.org/packages/json2)herunterladen.

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Mischung aus Hub und persistentconnection-Syntax

Signalr verwendet zwei Kommunikationsmodelle: Hubs und persistentconnections. Die Syntax zum Aufrufen dieser beiden Kommunikationsmodelle unterscheidet sich im Client Code. Wenn Sie einen Hub in Ihrem Servercode hinzugefügt haben, überprüfen Sie, ob der gesamte Client Code die richtige Hub-Syntax verwendet.

**JavaScript-Client Code, der eine persistentconnection in einem JavaScript-Client erstellt**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**JavaScript-Client Code, der einen Hub-Proxy in einem JavaScript-Client erstellt**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C#Servercode, der eine Route einer persistentconnection zuordnet**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C#Servercode, der eine Route zu einem Hub oder mehreren Hubs zuordnet, wenn mehrere Anwendungen vorhanden sind**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Verbindung gestartet, bevor Abonnements hinzugefügt werden

Wenn die Verbindung des Hubs gestartet wird, bevor Methoden, die vom Server aufgerufen werden können, dem Proxy hinzugefügt werden, werden keine Nachrichten empfangen. Mit dem folgenden JavaScript-Code wird der Hub nicht ordnungsgemäß gestartet:

**Falscher JavaScript-Client Code, der nicht zulässt, dass Hubs-Nachrichten empfangen werden.**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Fügen Sie stattdessen die-Methoden Abonnements vor dem Aufruf von Start ein:

**JavaScript-Client Code, mit dem einem Hub ordnungsgemäß Abonnements hinzugefügt werden**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Fehlender Methodenname auf dem Hub-Proxy

Überprüfen Sie, ob die auf dem Server definierte Methode auf dem Client abonniert ist. Obwohl der Server die Methode definiert, muss er dem Client Proxy trotzdem hinzugefügt werden. Methoden können dem Client Proxy auf folgende Weise hinzugefügt werden (Beachten Sie, dass die-Methode dem `client`-Member des Hubs hinzugefügt wird, nicht direkt dem Hub):

**JavaScript-Client Code zum Hinzufügen von Methoden zu einem hubproxy**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Hub-oder hubmethoden, die nicht als öffentlich deklariert sind

Um auf dem Client sichtbar zu sein, müssen die hubimplementierung und-Methoden als `public`deklariert werden.

### <a name="accessing-hub-from-a-different-application"></a>Zugreifen auf den Hub aus einer anderen Anwendung

Auf signalr Hubs kann nur über Anwendungen zugegriffen werden, die signalr-Clients implementieren. Signalr kann nicht mit anderen Kommunikations Bibliotheken (wie SOAP oder WCF-Webdiensten) interagieren. Wenn kein signalr-Client für die Zielplattform verfügbar ist, können Sie nicht direkt auf den Endpunkt des Servers zugreifen.

### <a name="manually-serializing-data"></a>Manuelles Serialisieren von Daten

Signalr verwendet automatisch JSON, um die Methoden Parameter zu serialisieren. es ist nicht nötig, dies selbst zu tun.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Remotehub Methode wird nicht auf dem Client in der ongetrennte-Funktion ausgeführt

Dieses Verhalten ist beabsichtigt. Wenn `OnDisconnected` aufgerufen wird, wurde der Hub bereits in den `Disconnected`-Zustand versetzt, sodass keine weiteren hubmethoden aufgerufen werden können.

**C#Servercode, der Code im ongetrennte Ereignis ordnungsgemäß ausführt**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Verbindungs Limit erreicht

Wenn Sie die Vollversion von IIS auf einem Client Betriebssystem wie Windows 7 verwenden, wird ein Limit von 10 Verbindungen erzwungen. Wenn Sie ein Client Betriebssystem verwenden, verwenden Sie stattdessen IIS Express, um dieses Limit zu vermeiden.

### <a name="cross-domain-connection-not-set-up-properly"></a>Die Domänen übergreifende Verbindung wurde nicht ordnungsgemäß eingerichtet.

Wenn eine Domänen übergreifende Verbindung (eine Verbindung, für die sich die signalr-URL nicht in derselben Domäne wie die Hostingseite befindet) nicht ordnungsgemäß eingerichtet ist, kann die Verbindung ohne eine Fehlermeldung fehlschlagen. Informationen dazu, wie Sie die Domänen übergreifende Kommunikation aktivieren, finden Sie unter [Einrichten einer Domänen übergreifenden Verbindung](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Verbindung mit NTLM (Active Directory) funktioniert nicht im .NET-Client

Eine Verbindung in einer .NET-Client Anwendung, die Domänen Sicherheit verwendet, kann fehlschlagen, wenn die Verbindung nicht ordnungsgemäß konfiguriert ist. Wenn Sie signalr in einer Domänen Umgebung verwenden möchten, legen Sie die erforderliche Verbindungs Eigenschaft wie folgt fest:

**C#Client Code, der Verbindungs Anmelde Informationen implementiert**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Andere Verbindungsprobleme

In diesem Abschnitt werden die Gründe und Lösungen für bestimmte Symptome oder Fehlermeldungen beschrieben, die während einer Verbindung auftreten.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Fehler "Start muss aufgerufen werden, bevor Daten gesendet werden können"

Dieser Fehler tritt häufig auf, wenn der Code auf signalr-Objekte verweist, bevor die Verbindung gestartet wird. Das wireup für Handler und die Like-Methode, die Methoden aufruft, die auf dem Server definiert sind, müssen hinzugefügt werden, nachdem die Verbindung abgeschlossen wurde. Beachten Sie, dass der-`Start` asynchron ist, sodass Code nach dem-Vorgang möglicherweise ausgeführt wird, bevor er abgeschlossen wird. Die beste Möglichkeit zum Hinzufügen von Handlern, nachdem eine Verbindung vollständig gestartet wurde, besteht darin, Sie in eine Rückruffunktion einzufügen, die als Parameter an die Start Methode übergeben wird:

**JavaScript-Client Code, der ordnungsgemäß Ereignishandler hinzufügt, die auf signalr-Objekte verweisen**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Dieser Fehler wird auch angezeigt, wenn eine Verbindung beendet wird, während auf signalr-Objekte noch verwiesen wird.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>Fehler "301 wurde verschoben" oder "302 verschoben temporär"

Dieser Fehler wird möglicherweise angezeigt, wenn das Projekt einen Ordner namens signalr enthält, der den automatisch erstellten Proxy beeinträchtigt. Um diesen Fehler zu vermeiden, verwenden Sie keinen Ordner mit dem Namen `SignalR` in Ihrer Anwendung, oder schalten Sie die automatische Proxy Generierung aus. Weitere Informationen finden [Sie im generierten Proxy und in den Ausführungen zu diesem](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) Vorgang.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Fehler "403 verboten" im .net-oder Silverlight-Client

Dieser Fehler kann in Domänen übergreifenden Umgebungen auftreten, in denen die Domänen übergreifende Kommunikation nicht ordnungsgemäß aktiviert ist. Informationen dazu, wie Sie die Domänen übergreifende Kommunikation aktivieren, finden Sie unter [Einrichten einer Domänen übergreifenden Verbindung](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Informationen zum Herstellen einer Domänen übergreifenden Verbindung in einem Silverlight-Client finden Sie unter [Domänen übergreifende Verbindungen von Silverlight-Clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Fehler "404 nicht gefunden"

Für dieses Problem gibt es mehrere Gründe. Überprüfen Sie Folgendes:

- Der **Hub-Proxy Adress Verweis ist nicht korrekt formatiert:** Dieser Fehler tritt häufig auf, wenn der Verweis auf die generierte Hub-Proxy Adresse nicht ordnungsgemäß formatiert ist. Überprüfen Sie, ob der Verweis auf die Hub-Adresse ordnungsgemäß erstellt wurde. Weitere Informationen finden Sie unter Gewusst [wie: verweisen auf den dynamisch generierten Proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) .
- **Hinzufügen von Routen zur Anwendung vor dem Hinzufügen der Hub-Route:** Wenn Ihre Anwendung andere Routen verwendet, vergewissern Sie sich, dass die erste hinzugefügte Route der `MapHubs`aufgerufen wird.

### <a name="500-internal-server-error"></a>"interner Server Fehler 500"

Dies ist ein sehr allgemeiner Fehler, der eine Vielzahl von Gründen aufweisen könnte. Die Details des Fehlers sollten im Ereignisprotokoll des Servers angezeigt werden oder durch Debuggen des Servers gefunden werden. Ausführlichere Fehlerinformationen können Sie durch das Aktivieren detaillierter Fehler auf dem Server erhalten. Weitere Informationen finden Sie unter [Behandeln von Fehlern in der Hub-Klasse](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Fehler "TypeError: &lt;hubtype&gt; ist nicht definiert"

Dieser Fehler wird zurückgegeben, wenn der `MapHubs` nicht ordnungsgemäß ausgeführt wird. Weitere Informationen finden [Sie unter Registrieren der signalr-Route und Konfigurieren von signalr-Optionen](../guide-to-the-api/hubs-api-guide-server.md#route) .

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>Jsonserializationexception wurde nicht von Benutzercode behandelt.

Überprüfen Sie, ob die Parameter, die Sie an ihre Methoden senden, nicht serialisierbare Typen enthalten (z. b. Datei Handles oder Datenbankverbindungen). Wenn Sie Member für ein serverseitiges Objekt verwenden müssen, das nicht an den Client gesendet werden soll (aus Sicherheitsgründen oder aus Gründen der Serialisierung), verwenden Sie das `JSONIgnore`-Attribut.

### <a name="protocol-error-unknown-transport-error"></a>Fehler "Protokollfehler: Unbekannter Transport"

Dieser Fehler kann auftreten, wenn der Client die von signalr verwendeten Transporte nicht unterstützt. Informationen dazu, welche Browser mit signalr verwendet werden können, finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports) .

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Die Erstellung des JavaScript-Hub-Proxys wurde deaktiviert."

Dieser Fehler tritt auf, wenn `DisableJavaScriptProxies` festgelegt wird, während Sie auch einen Verweis auf den dynamisch generierten Proxy bei `signalr/hubs`einschließen. Weitere Informationen zum manuellen Erstellen des Proxys finden [Sie unter der generierte Proxy und dessen](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)Funktionsweise.

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"Die Verbindungs-ID weist das falsche Format auf, oder die Benutzeridentität kann während einer aktiven signalr-Verbindung nicht geändert werden."

Dieser Fehler wird möglicherweise angezeigt, wenn die Authentifizierung verwendet wird und der Client vor dem Beenden der Verbindung abgemeldet wird. Die Lösung besteht darin, die signalr-Verbindung anzuhalten, bevor der Client protokolliert wird.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Nicht abgefangener Fehler: signalr: jQuery nicht gefunden. Stellen Sie sicher, dass auf "jQuery" vor der Datei "signalr. js" verwiesen wird.

Für den signalr-JavaScript-Client muss jQuery ausgeführt werden. Vergewissern Sie sich, dass der Verweis auf jQuery korrekt ist, dass der verwendete Pfad gültig ist und dass der Verweis auf jQuery vor dem Verweis auf signalr liegt.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Nicht abgefangene TypeError: die Eigenschaft"&lt;-Eigenschaft&gt;"nicht definierter Fehler" kann nicht gelesen werden.

Dieser Fehler führt dazu, dass jQuery oder der hubproxy nicht ordnungsgemäß referenziert ist. Vergewissern Sie sich, dass der Verweis auf jQuery und der Hubs-Proxy richtig sind, dass der verwendete Pfad gültig ist und der Verweis auf jQuery vor dem Verweis auf den Hubs-Proxy steht. Der Standard Verweis auf den Hubs-Proxy sollte wie folgt aussehen:

**Client seitiger HTML-Code, der ordnungsgemäß auf den Hubs-Proxy verweist**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Fehler "RuntimeBinderException wurde vom Benutzercode nicht behandelt"

Dieser Fehler kann auftreten, wenn die falsche Überladung von `Hub.On` verwendet wird. Wenn die Methode über einen Rückgabewert verfügt, muss der Rückgabetyp als generischer Typparameter angegeben werden:

**Auf dem Client definierte Methode (ohne generierten Proxy)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Verbindungs-ID ist inkonsistent oder Verbindungsunterbrechungen zwischen Seiten Ladevorgängen

Dieses Verhalten ist beabsichtigt. Da das Hub-Objekt im Seiten Objekt gehostet wird, wird der Hub zerstört, wenn die Seite aktualisiert wird. Eine Anwendung mit mehreren Seiten muss die Zuordnung zwischen Benutzern und Verbindungs-IDs aufrechterhalten, damit Sie zwischen den Seiten Ladevorgängen konsistent werden. Die Verbindungs-IDs können auf dem Server entweder in einem `ConcurrentDictionary` Objekt oder in einer Datenbank gespeichert werden.

### <a name="value-cannot-be-null-error"></a>Fehler "der Wert darf nicht NULL sein"

Server seitige Methoden mit optionalen Parametern werden zurzeit nicht unterstützt. Wenn der optionale Parameter weggelassen wird, schlägt die Methode fehl. Weitere Informationen finden Sie unter [optionale Parameter](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox kann keine Verbindung mit dem Server bei &lt;Adresse herstellen&gt;" Fehler in Firebug

Diese Fehlermeldung kann in Firebug angezeigt werden, wenn die Aushandlung des WebSocket-Transports fehlschlägt und stattdessen ein anderer Transport verwendet wird. Dieses Verhalten ist beabsichtigt.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Fehler "das Remote Zertifikat ist gemäß der Überprüfungs Prozedur ungültig" in der .NET-Client Anwendung.

Wenn für Ihren Server benutzerdefinierte Client Zertifikate erforderlich sind, können Sie der Verbindung ein X509Certificate hinzufügen, bevor die Anforderung erfolgt. Fügen Sie das Zertifikat mit `Connection.AddClientCertificate`der Verbindung hinzu.

### <a name="connection-drops-after-authentication-times-out"></a>Verbindung wird nach dem Timeout der Authentifizierung getrennt

Dieses Verhalten ist beabsichtigt. Anmelde Informationen für die Authentifizierung können nicht geändert werden, während eine Verbindung aktiv ist. zum Aktualisieren von Anmelde Informationen muss die Verbindung beendet und neu gestartet werden.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>"Onconnected" wird bei Verwendung von jQuery Mobile zweimal aufgerufen.

die `initializePage` Funktion von jQuery Mobile erzwingt die erneute Ausführung der Skripts auf jeder Seite und somit eine zweite Verbindung. Lösungen für dieses Problem sind:

- Fügen Sie den Verweis auf "jQuery Mobile" vor der JavaScript-Datei ein.
- Deaktivieren Sie die `initializePage` Funktion durch Festlegen von `$.mobile.autoInitializePage = false`.
- Warten Sie, bis die Seite vor dem Starten der Verbindung initialisiert wurde.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Nachrichten werden in Silverlight-Anwendungen verzögert, die Server gesendete Ereignisse verwenden

Nachrichten werden verzögert, wenn die Server gesendeten Ereignisse in Silverlight verwendet werden. Wenn Sie stattdessen die Verwendung eines langen Abrufs erzwingen möchten, verwenden Sie beim Starten der Verbindung Folgendes:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"Berechtigung verweigert" mithilfe des Forever-Frame Protokolls

Dies ist ein bekanntes Problem, das [hier](https://github.com/SignalR/SignalR/issues/1963)beschrieben wird. Dieses Symptom kann mit der neuesten jQuery-Bibliothek verwendet werden. das Problem kann umgangen werden, indem Sie die Anwendung auf jQuery 1.8.2 herabstufen.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Kompilierung und serverseitige Fehler

 Der folgende Abschnitt enthält mögliche Lösungen für Compiler-und serverseitige Laufzeitfehler. 

### <a name="reference-to-hub-instance-is-null"></a>Der Verweis auf die Hub-Instanz ist NULL.

Da für jede Verbindung eine Hub-Instanz erstellt wird, können Sie selbst keine Instanz eines Hubs im Code erstellen. Weitere Informationen zum Abrufen eines Verweises auf den hubkontext finden Sie unter So rufen Sie Methoden auf einem Hub von außerhalb der Hub [-Klasse](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) auf.

### <a name="httpcontextcurrentsession-is-null"></a>HttpContext. Current. Session ist NULL

Dieses Verhalten ist beabsichtigt. Signalr unterstützt den ASP.NET-Sitzungs Status nicht, da das Aktivieren des Sitzungs Zustands Duplex Nachrichten unterbrechen würde.

### <a name="no-suitable-method-to-override"></a>Keine geeignete Methode zum Überschreiben

Dieser Fehler wird möglicherweise angezeigt, wenn Sie Code aus älteren Dokumentationen oder Blogs verwenden. Vergewissern Sie sich, dass Sie nicht auf Namen von Methoden verweisen, die geändert oder veraltet sind (z. b. `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>"Hostcontextextensions. WebSocketServerUrl" ist NULL.

Dieses Verhalten ist beabsichtigt. Dieser Member ist veraltet und sollte nicht verwendet werden.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"Eine Route mit dem Namen ' signalr. Hubs ' ist bereits in der Routen Sammlung vorhanden".

Dieser Fehler wird angezeigt, wenn `MapHubs` von Ihrer Anwendung zweimal aufgerufen wird. Einige Beispielanwendungen nennen `MapHubs` direkt in der globalen Anwendungsdatei. andere nehmen den-Befehl in einer Wrapper Klasse auf. Stellen Sie sicher, dass Ihre Anwendung nicht beides tut.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio-Probleme

In diesem Abschnitt werden die in Visual Studio auftretenden Probleme beschrieben.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Der Knoten "Skript Dokumente" wird nicht in Projektmappen-Explorer angezeigt.

Einige unserer Tutorials leiten Sie beim Debuggen auf den Knoten "Skript Dokumente" in Projektmappen-Explorer. Dieser Knoten wird vom JavaScript-Debugger erstellt und wird nur beim Debuggen von Browser Clients in Internet Explorer angezeigt. der Knoten wird nicht angezeigt, wenn Chrome oder Firefox verwendet werden. Der JavaScript-Debugger wird auch dann nicht ausgeführt, wenn ein anderer Client Debugger ausgeführt wird, z. b. der Silverlight-Debugger.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>Signalr funktioniert nicht in Visual Studio 2008 oder früher.

Dieses Verhalten ist beabsichtigt. Für signalr ist .NET Framework 4 oder höher erforderlich. Dies erfordert, dass signalr-Anwendungen in Visual Studio 2010 oder höher entwickelt werden.

<a id="iis"></a>

## <a name="iis-issues"></a>IIS-Probleme

Dieser Abschnitt enthält Probleme mit Internetinformationsdienste.

### <a name="web-site-crashes-after-maphubs-call"></a>Website Abstürze nach maphubs-Rückruf

Dieses Problem wurde in der aktuellen Version von signalr behoben. Vergewissern Sie sich, dass Sie die neueste veröffentlichte Version von signalr verwenden, indem Sie Ihre Installation mithilfe von nuget aktualisieren.

<a id="azure"></a>

## <a name="azure-issues"></a>Azure-Probleme

Dieser Abschnitt enthält Probleme mit Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Nach dem Ändern von Themen Namen werden keine Nachrichten über die Azure-Rückwand empfangen.

Die von der Azure-Rückwand verwendeten Themen sollen nicht vom Benutzer konfiguriert werden können.
