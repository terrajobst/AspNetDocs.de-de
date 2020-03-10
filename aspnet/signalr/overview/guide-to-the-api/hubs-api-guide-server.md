---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: ASP.net signalr Hubs-API-Handbuch-C#Server () | Microsoft-Dokumentation
author: bradygaster
description: Dieses Dokument enthält eine Einführung in die Programmierung der Serverseite der ASP.net signalr Hubs-API für signalr, Version 2, mit Codebeispielen, die zeigen...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431367"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>ASP.net signalr Hubs-API-Handbuch-C#Server ()

von [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Dieses Dokument enthält eine Einführung in die Programmierung der Serverseite der ASP.net signalr Hubs-API für signalr, Version 2, mit Codebeispielen, die allgemeine Optionen veranschaulichen.
> 
> Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server. In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden. Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden. Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.
> 
> Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird. Eine Einführung in signalr, Hubs und persistente Verbindungen finden Sie unter [Einführung in signalr 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendete Software Versionen
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Signalr Version 2
>   
> 
> 
> ## <a name="topic-versions"></a>Themen Versionen
> 
> Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

## <a name="overview"></a>Übersicht

Dieses Dokument enthält folgende Abschnitte:

- [Registrieren von signalr-Middleware](#route)

    - [Die/signalr-URL](#signalrurl)
    - [Konfigurieren von signalr-Optionen](#options)
- [Erstellen und Verwenden von Hub-Klassen](#hubclass)

    - [Hub-Objekt Lebensdauer](#transience)
    - [Kamel Schreibweise von Hub-Namen in JavaScript-Clients](#hubnames)
    - [Mehrere Hubs](#multiplehubs)
    - [Stark typisierte Hubs](#stronglytypedhubs)
- [Definieren von Methoden in der hubklasse, die von Clients aufgerufen werden können](#hubmethods)

    - [Kamel Schreibweise von Methodennamen in JavaScript-Clients](#methodnames)
    - [Zeitpunkt der asynchronen Ausführung](#asyncmethods)
    - [Definieren von über Ladungen](#overloads)
    - [Berichten des Fortschritts von hubmethoden aufrufen](#progress)
- [So werden Client Methoden aus der Hub-Klasse aufgerufen](#callfromhub)

    - [Auswählen, welche Clients den RPC erhalten](#selectingclients)
    - [Keine Validierung der Kompilierzeit für Methodennamen](#dynamicmethodnames)
    - [Nicht Groß-/Kleinschreibung nicht beachtet.](#caseinsensitive)
    - [Asynchrone Ausführung](#asyncclient)
- [Verwalten der Gruppenmitgliedschaft mit der Hub-Klasse](#groupsfromhub)

    - [Asynchrone Ausführung von Add-und Remove-Methoden](#asyncgroupmethods)
    - [Persistenz der Gruppenmitgliedschaft](#grouppersistence)
    - [Einzelbenutzer Gruppen](#singleusergroups)
- [Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse](#connectionlifetime)

    - [Wenn "onconnected", "ongetrennte" und "onreconnected" aufgerufen werden](#onreconnected)
    - [Aufruferstatus nicht aufgefüllt](#nocallerstate)
- [So erhalten Sie Informationen über den Client aus der Kontext Eigenschaft](#contextproperty)
- [Übergeben des Zustands zwischen Clients und der Hub-Klasse](#passstate)
- [Behandeln von Fehlern in der Hub-Klasse](#handleErrors)
- [Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der hubklasse](#callfromoutsidehub)

    - [Aufrufen von Client Methoden](#callingclientsoutsidehub)
    - [Verwalten der Gruppenmitgliedschaft](#managinggroupsoutsidehub)
- [Aktivieren der Ablauf Verfolgung](#tracing)
- [Vorgehensweise beim Anpassen der Hubs-Pipeline](#hubpipeline)

Dokumentation zum Programmieren von Clients finden Sie in den folgenden Ressourcen:

- [Leitfaden für signalr-Hubs-API-JavaScript-Client](hubs-api-guide-javascript-client.md)
- [Leitfaden für signalr-Hubs-API: .NET-Client](hubs-api-guide-net-client.md)

Die Serverkomponenten für signalr 2 sind nur in .NET 4,5 verfügbar. Server, auf denen .NET 4,0 ausgeführt wird, müssen signalr v1. x verwenden.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Registrieren von signalr-Middleware

Um die Route zu definieren, die von Clients zum Herstellen einer Verbindung mit dem Hub verwendet wird, müssen Sie beim Starten der Anwendung die `MapSignalR`-Methode abrufen. `MapSignalR` ist eine [Erweiterungsmethode](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) für die `OwinExtensions`-Klasse. Im folgenden Beispiel wird gezeigt, wie die signalr Hubs-Route mithilfe einer owin-Startklasse definiert wird.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Wenn Sie signalr-Funktionen zu einer ASP.NET MVC-Anwendung hinzufügen, stellen Sie sicher, dass die signalr-Route vor den anderen Routen hinzugefügt wird. Weitere Informationen finden Sie unter [Tutorial: ersten Schritte mit signalr 2 und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>Die/signalr-URL

Standardmäßig ist die Routen-URL, die von Clients zum Herstellen einer Verbindung mit ihrem Hub verwendet wird, "/signalr". (Verwechseln Sie diese URL nicht mit der URL "/signalr/Hubs", die für die automatisch generierte JavaScript-Datei gilt. Weitere Informationen zum generierten Proxy finden [Sie im signalr Hubs-API-Handbuch-JavaScript-Client-der generierte Proxy und dessen](hubs-api-guide-javascript-client.md#genproxy)Funktionsweise.)

Möglicherweise gibt es außergewöhnliche Umstände, in denen diese Basis-URL für signalr nicht verwendbar ist. Beispielsweise verfügen Sie über einen Ordner in Ihrem Projekt mit dem Namen *signalr* , und Sie möchten den Namen nicht ändern. In diesem Fall können Sie die Basis-URL ändern, wie in den folgenden Beispielen gezeigt (ersetzen Sie "/signalr" im Beispielcode durch die gewünschte URL).

**Server Code, der die URL angibt**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**JavaScript-Client Code, der die URL (mit dem generierten Proxy) angibt**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**JavaScript-Client Code, der die URL (ohne den generierten Proxy) angibt**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**.NET-Client Code, der die URL angibt**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Konfigurieren von signalr-Optionen

Über Ladungen der `MapSignalR`-Methode ermöglichen es Ihnen, eine benutzerdefinierte URL, einen benutzerdefinierten Abhängigkeits Konflikt Löser und die folgenden Optionen anzugeben:

- Aktivieren Sie Domänen übergreifende Aufrufe mithilfe von cors oder JSONP von Browser Clients.

    Wenn der Browser eine Seite aus `http://contoso.com`lädt, befindet sich die signalr-Verbindung in der Regel in der gleichen Domäne `http://contoso.com/signalr`. Wenn die Seite aus `http://contoso.com` eine Verbindung mit `http://fabrikam.com/signalr`herstellt, handelt es sich um eine Domänen übergreifende Verbindung. Aus Sicherheitsgründen sind Domänen übergreifende Verbindungen standardmäßig deaktiviert. Weitere Informationen finden Sie unter [ASP.net signalr Hubs API Guide-JavaScript Client (Einrichten einer Domänen übergreifenden Verbindung](hubs-api-guide-javascript-client.md#crossdomain)).
- Aktivieren Sie detaillierte Fehlermeldungen.

    Wenn Fehler auftreten, besteht das Standardverhalten von signalr darin, eine Benachrichtigung an Clients zu senden, ohne Details dazu zu erhalten, was passiert ist. Das Senden ausführlicher Fehlerinformationen an Clients wird in der Produktion nicht empfohlen, da böswillige Benutzer möglicherweise die Informationen in Angriffen gegen Ihre Anwendung verwenden können. Zur Problembehandlung können Sie diese Option verwenden, um eine informative Fehlerberichterstattung temporär zu aktivieren.
- Automatisch generierte JavaScript-Proxy Dateien deaktivieren.

    Standardmäßig wird eine JavaScript-Datei mit Proxys für Ihre Hub-Klassen als Antwort auf die URL "/signalr/Hubs" generiert. Wenn Sie die JavaScript-Proxys nicht verwenden möchten oder wenn Sie diese Datei manuell generieren und auf eine physische Datei in ihren Clients verweisen möchten, können Sie diese Option verwenden, um die Proxy Generierung zu deaktivieren. Weitere Informationen finden Sie unter [signalr Hubs-API-Handbuch-JavaScript-Client-Erstellen einer physischen Datei für den von signalr generierten Proxy](hubs-api-guide-javascript-client.md#manualproxy).

Im folgenden Beispiel wird gezeigt, wie die signalr-Verbindungs-URL und diese Optionen in einem aufzurufenden `MapSignalR`-Methode angegeben werden. Um eine benutzerdefinierte URL anzugeben, ersetzen Sie "/signalr" im Beispiel durch die URL, die Sie verwenden möchten.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Erstellen und Verwenden von Hub-Klassen

Um einen Hub zu erstellen, erstellen Sie eine Klasse, die von [Microsoft. Aspnet. signalr. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)abgeleitet wird. Das folgende Beispiel zeigt eine einfache Hub-Klasse für eine Chat-Anwendung.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

In diesem Beispiel kann ein verbundener Client die `NewContosoChatMessage`-Methode aufzurufen. wenn dies der Fall ist, werden die empfangenen Daten an alle verbundenen Clients übermittelt.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Hub-Objekt Lebensdauer

Die hubklasse wird nicht instanziiert, oder es werden keine Methoden aus dem eigenen Code auf dem Server aufgerufen. Das ist alles, was Sie von der signalr Hubs-Pipeline für Sie erledigt haben. Signalr erstellt immer dann eine neue Instanz der Hub-Klasse, wenn Sie einen Hub-Vorgang verarbeiten muss, z. b. Wenn ein Client eine Verbindung herstellt, die Verbindung trennt oder einen Methodenaufruf an den Server ausführt.

Da Instanzen der Hub-Klasse flüchtig sind, können Sie Sie nicht verwenden, um den Zustand von einem Methodenaufrufe zum nächsten beizubehalten. Jedes Mal, wenn der Server einen Methodenaufruf von einem Client empfängt, wird die Nachricht von einer neuen Instanz der Hub-Klasse verarbeitet. Um den Zustand über mehrere Verbindungen und Methodenaufrufe beizubehalten, verwenden Sie eine andere Methode, z. b. eine Datenbank oder eine statische Variable für die Hub-Klasse, oder eine andere Klasse, die nicht von `Hub`abgeleitet ist. Wenn Sie Daten im Arbeitsspeicher beibehalten, werden die Daten bei Verwendung einer Methode, z. b. einer statischen Variablen in der Hub-Klasse, verloren gehen, wenn die APP-Domäne wieder verwendet wird.

Wenn Sie Nachrichten aus Ihrem eigenen Code, der außerhalb der Hub-Klasse ausgeführt wird, an Clients senden möchten, können Sie dies nicht tun, indem Sie eine Hub-Klasseninstanz instanziieren. Sie können Sie jedoch durch einen Verweis auf das signalr-Kontext Objekt für die Hub-Klasse erstellen. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der Hub-Klasse](#callfromoutsidehub) .

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Kamel Schreibweise von Hub-Namen in JavaScript-Clients

Standardmäßig verweisen JavaScript-Clients auf Hubs, indem Sie eine Version des Klassen namens mit Kamel Schreibung verwenden. Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann. Das vorherige Beispiel wird im JavaScript-Code als `contosoChatHub` bezeichnet.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**JavaScript-Client mit generiertem Proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Wenn Sie einen anderen Namen angeben möchten, der von Clients verwendet werden soll, fügen Sie das `HubName`-Attribut hinzu. Wenn Sie ein `HubName` Attribut verwenden, gibt es keine Namensänderung in der Camel-Schreibweise auf JavaScript-Clients.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**JavaScript-Client mit generiertem Proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Mehrere Hubs

Sie können mehrere Hub-Klassen in einer Anwendung definieren. Wenn Sie dies tun, wird die Verbindung freigegeben, die Gruppen sind jedoch getrennt:

- Alle Clients verwenden dieselbe URL, um eine signalr-Verbindung mit Ihrem Dienst herzustellen ("/signalr" oder Ihre benutzerdefinierte URL, wenn Sie eine solche URL angegeben haben), und diese Verbindung wird für alle vom Dienst definierten Hubs verwendet.

    Es gibt keinen Leistungsunterschied für mehrere Hubs im Vergleich zum Definieren aller Hubfunktionen in einer einzelnen Klasse.
- Alle Hubs erhalten die gleichen HTTP-Anforderungs Informationen.

    Da alle Hubs die gleiche Verbindung gemeinsam nutzen, werden die HTTP-Anforderungs Informationen, die der Server erhält, in der ursprünglichen HTTP-Anforderung angezeigt, die die signalr-Verbindung herstellt. Wenn Sie die Verbindungsanforderung verwenden, um Informationen vom Client an den Server zu übergeben, indem Sie eine Abfrage Zeichenfolge angeben, können Sie verschiedene Abfrage Zeichenfolgen nicht für verschiedene Hubs bereitstellen. Alle Hubs erhalten die gleichen Informationen.
- Die generierte JavaScript-Proxydatei enthält Proxys für alle Hubs in einer Datei.

    Weitere Informationen zu JavaScript [-Proxys finden Sie im signalr Hubs-API-Handbuch-JavaScript-Client-der generierte Proxy und dessen](hubs-api-guide-javascript-client.md#genproxy)Funktionsweise.
- Gruppen werden innerhalb von Hubs definiert.

    In signalr können Sie benannte Gruppen für die Übertragung an Teilmengen verbundener Clients definieren. Gruppen werden für jeden Hub separat verwaltet. Beispielsweise würde eine Gruppe mit dem Namen "Administratoren" eine Reihe von Clients für Ihre `ContosoChatHub`-Klasse enthalten, und der gleiche Gruppenname bezieht sich auf einen anderen Satz von Clients für die `StockTickerHub` Klasse.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Stark typisierte Hubs

Um eine Schnittstelle für Ihre hubmethoden zu definieren, auf die der Client verweisen kann (und um IntelliSense für Ihre hubmethoden zu aktivieren), leiten Sie Ihren Hub von `Hub<T>` (eingeführt in signalr 2,1) anstatt `Hub`ab:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Definieren von Methoden in der hubklasse, die von Clients aufgerufen werden können

Wenn Sie eine Methode auf dem Hub verfügbar machen möchten, die vom Client aufgerufen werden soll, deklarieren Sie eine öffentliche Methode, wie in den folgenden Beispielen gezeigt.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Sie können einen Rückgabetyp und Parameter, einschließlich komplexer Typen und Arrays, wie in jeder beliebigen C# Methode angeben. Alle Daten, die Sie in Parametern empfangen oder an den Aufrufer zurückgeben, werden mithilfe von JSON zwischen dem Client und dem Server übermittelt, und signalr übernimmt automatisch die Bindung komplexer Objekte und Arrays von Objekten.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Kamel Schreibweise von Methodennamen in JavaScript-Clients

Standardmäßig verweisen JavaScript-Clients auf hubmethoden, indem Sie eine Version des Methoden namens mit Kamel Schreibung verwenden. Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**JavaScript-Client mit generiertem Proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Wenn Sie einen anderen Namen angeben möchten, der von Clients verwendet werden soll, fügen Sie das `HubMethodName`-Attribut hinzu.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**JavaScript-Client mit generiertem Proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Zeitpunkt der asynchronen Ausführung

Wenn die Methode eine lange Ausführungszeit erfordert oder Aufgaben ausführen müssen, die warten würden, wie z. b. eine Datenbanksuche oder ein Webdienst-aufrufbedarf, machen Sie die Hub-Methode asynchron, indem Sie eine [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (anstelle von `void` Rückgabe) oder [Aufgabe&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) Objekt (anstelle `T` Rückgabe Typs) zurückgeben. Wenn Sie ein `Task` Objekt von der-Methode zurückgeben, wartet signalr darauf, dass die `Task` abgeschlossen ist, und sendet dann das entpackte Ergebnis an den Client zurück. es gibt also keinen Unterschied in der Art und Weise, wie Sie den Methodenaufrufe im Client codieren.

Durch die asynchrone Erstellung einer hubmethode wird verhindert, dass die Verbindung blockiert wird, wenn der WebSocket-Transport verwendet wird. Wenn eine hubmethode synchron ausgeführt wird und der Transport WebSocket ist, werden nachfolgende Aufrufe von Methoden auf dem Hub desselben Clients blockiert, bis die hubmethode abgeschlossen ist.

Im folgenden Beispiel wird die gleiche Methode gezeigt, die für die synchrone oder asynchrone Ausführung programmiert ist, gefolgt von JavaScript-Client Code, der zum Aufrufen einer beliebigen Version funktioniert.

**Synchrone**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asynchronen**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**JavaScript-Client mit generiertem Proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Weitere Informationen zum Verwenden von asynchronen Methoden in ASP.NET 4,5 finden [Sie unter Verwenden von asynchronen Methoden in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definieren von über Ladungen

Wenn Sie über Ladungen für eine Methode definieren möchten, muss die Anzahl von Parametern in den einzelnen über Ladungen abweichen. Wenn Sie eine Überladung nur durch Angabe verschiedener Parametertypen unterscheiden, wird die hubklasse kompiliert, aber der signalr-Dienst löst zur Laufzeit eine Ausnahme aus, wenn Clients versuchen, eine der über Ladungen aufzurufen.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Berichten des Fortschritts von hubmethoden aufrufen

Signalr 2,1 fügt Unterstützung für das in .NET 4,5 eingeführte [Progress Reporting-Muster](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) hinzu. Um die Status Berichterstattung zu implementieren, definieren Sie einen `IProgress<T>` Parameter für die Hub-Methode, auf den der Client zugreifen kann:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Beim Schreiben einer Server Methode mit langer Ausführungszeit ist es wichtig, ein asynchrones Programmier Muster wie Async/warten zu verwenden, anstatt den Hub-Thread zu blockieren.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>So werden Client Methoden aus der Hub-Klasse aufgerufen

Um Client Methoden vom Server aufzurufen, verwenden Sie die `Clients`-Eigenschaft in einer Methode in ihrer Hub-Klasse. Das folgende Beispiel zeigt Servercode, der `addNewMessageToPage` auf allen verbundenen Clients aufruft, und Client Code, der die-Methode in einem JavaScript-Client definiert.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

Das Aufrufen einer Client Methode ist ein asynchroner Vorgang und gibt einen `Task`zurück. Verwenden Sie `await`:

* Um sicherzustellen, dass die Nachricht ohne Fehler gesendet wird. 
* Um das Abfangen und behandeln von Fehlern in einem try-catch-Block zu aktivieren.

**JavaScript-Client mit generiertem Proxy**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Sie können keinen Rückgabewert von einer Client Methode erhalten. Syntax, wie z. b. `int x = Clients.All.add(1,1)`, funktioniert nicht.

Sie können komplexe Typen und Arrays für die Parameter angeben. Im folgenden Beispiel wird ein komplexer Typ in einem Methoden Parameter an den Client übergeben.

**Server Code, der eine Client Methode mithilfe eines komplexen Objekts aufruft**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Server Code, der das komplexe Objekt definiert**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**JavaScript-Client mit generiertem Proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Auswählen, welche Clients den RPC erhalten

Die Clients-Eigenschaft gibt ein [hubconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) -Objekt zurück, das mehrere Optionen zum Angeben von Clients bereitstellt, die den RPC empfangen:

- Alle verbundenen Clients.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Nur der aufrufenden Client.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Alle Clients mit Ausnahme des aufrufenden Clients.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Ein spezifischer Client, der durch die Verbindungs-ID identifiziert wird

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    In diesem Beispiel wird `addContosoChatMessageToPage` auf dem aufrufenden Client aufgerufen und hat dieselbe Auswirkung wie die Verwendung von `Clients.Caller`.
- Alle verbundenen Clients mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Alle verbundenen Clients in einer bestimmten Gruppe.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme des aufrufenden Clients.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Ein bestimmter Benutzer, der durch UserID identifiziert wird.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Standardmäßig ist dies `IPrincipal.Identity.Name`. Dies kann jedoch geändert werden, indem [eine Implementierung von iuseridprovider beim globalen Host registriert](mapping-users-to-connections.md#IUserIdProvider)wird.
- Alle Clients und Gruppen in einer Liste mit Verbindungs-IDs.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Eine Liste von Gruppen.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Ein Benutzer anhand des Namens.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Eine Liste von Benutzernamen (eingeführt in signalr 2,1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Keine Validierung der Kompilierzeit für Methodennamen

Der von Ihnen angegebene Methodenname wird als dynamisches Objekt interpretiert, was bedeutet, dass keine IntelliSense-oder Kompilierzeit Validierung dafür vorliegt. Der Ausdruck wird zur Laufzeit ausgewertet. Wenn der Methodenaufruf ausgeführt wird, sendet signalr den Methodennamen und die Parameterwerte an den Client. wenn der Client über eine Methode verfügt, die mit dem Namen übereinstimmt, wird diese Methode aufgerufen und die Parameterwerte an Sie übergeben. Wenn keine übereinstimmende Methode auf dem Client gefunden wird, wird kein Fehler ausgelöst. Informationen über das Format der Daten, die von signalr im Hintergrund an den Client übermittelt werden, wenn Sie eine Client Methode aufruft, finden Sie unter [Introduction to signalr (Einführung in signalr](../getting-started/introduction-to-signalr.md)).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Nicht Groß-/Kleinschreibung nicht beachtet.

Beim Methodennamen wird die Groß-/Kleinschreibung beachtet. Beispielsweise werden auf dem-Server `Clients.All.addContosoChatMessageToPage` auf dem-Server `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`oder `addContosoChatMessageToPage` auf dem Client ausgeführt.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Asynchrone Ausführung

Die von Ihnen aufzurufende Methode wird asynchron ausgeführt. Jeglicher Code, der nach einem Methodenaufrufe an einen Client gesendet wird, wird sofort ausgeführt, ohne darauf zu warten, dass signalr die Übertragung von Daten an Clients abgeschlossen hat, es sei denn, Sie geben an, dass die nachfolgenden Codezeilen auf die Im folgenden Codebeispiel wird gezeigt, wie zwei Client Methoden sequenziell ausgeführt werden.

**Verwenden von "warten" (.NET 4,5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Wenn Sie `await` verwenden, um zu warten, bis eine Client Methode abgeschlossen ist, bevor die nächste Codezeile ausgeführt wird, bedeutet dies nicht, dass Clients die Nachricht tatsächlich empfangen, bevor die nächste Codezeile ausgeführt wird. Die Vervollständigung eines Client Methoden Aufrufes bedeutet nur, dass signalr alle notwendigen Schritte zum Senden der Nachricht ausgeführt hat. Wenn Sie überprüfen müssen, ob die Nachricht von Clients empfangen wurde, müssen Sie diesen Mechanismus selbst programmieren. Beispielsweise können Sie eine `MessageReceived` Methode auf dem Hub programmieren, und in der `addContosoChatMessageToPage`-Methode auf dem Client können Sie `MessageReceived` abrufen, nachdem Sie die Aufgaben ausgeführt haben, die Sie auf dem Client ausführen müssen. In `MessageReceived` im Hub können Sie jede beliebige Arbeit von tatsächlichem Client Empfang und Verarbeitung des ursprünglichen Methoden Aufrufes abhängig machen.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Verwenden einer Zeichen folgen Variablen als Methodenname

Wenn Sie eine Client Methode aufrufen möchten, indem Sie eine Zeichen folgen Variable als Methodenname verwenden, wandeln Sie `Clients.All` (oder `Clients.Others`, `Clients.Caller`usw.) in `IClientProxy` und rufen Sie dann aufrufen [(MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx)auf.

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Verwalten der Gruppenmitgliedschaft mit der Hub-Klasse

Gruppen in signalr bieten eine Methode zum Senden von Nachrichten an bestimmte Teilmengen verbundener Clients. Eine Gruppe kann über eine beliebige Anzahl von Clients verfügen, und ein Client kann Mitglied einer beliebigen Anzahl von Gruppen sein.

Verwenden Sie zum Verwalten der Gruppenmitgliedschaft die Methoden zum [Hinzufügen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) und [Entfernen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) , die von der `Groups`-Eigenschaft der Hub-Klasse bereitgestellt werden. Das folgende Beispiel zeigt die `Groups.Add`-und `Groups.Remove` Methoden, die in hubmethoden verwendet werden, die vom Client Code aufgerufen werden, gefolgt von JavaScript-Client Code, der Sie aufruft.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**JavaScript-Client mit generiertem Proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Sie müssen nicht explizit Gruppen erstellen. Tatsächlich wird eine Gruppe automatisch erstellt, wenn Sie Ihren Namen zum ersten Mal in einem Aufruf von `Groups.Add`angeben. Sie wird gelöscht, wenn Sie die letzte Verbindung aus der Mitgliedschaft entfernen.

Es ist keine API zum erhalten einer Gruppen Mitgliedschafts Liste oder einer Liste von Gruppen vorhanden. Signalr sendet Nachrichten auf der Grundlage eines [Pub/Sub-Modells](http://en.wikipedia.org/wiki/Publish/subscribe)an Clients und Gruppen, und der Server verwaltet keine Listen mit Gruppen oder Gruppenmitgliedschaften. Dadurch wird die Skalierbarkeit maximiert, denn wenn Sie einer Webfarm einen Knoten hinzufügen, muss jeder Status, den signalr beibehält, an den neuen Knoten weitergegeben werden.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Asynchrone Ausführung von Add-und Remove-Methoden

Die Methoden `Groups.Add` und `Groups.Remove` werden asynchron ausgeführt. Wenn Sie einer Gruppe einen Client hinzufügen und sofort mithilfe der-Gruppe eine Nachricht an den Client senden möchten, müssen Sie sicherstellen, dass die `Groups.Add`-Methode zuerst abgeschlossen wird. Das folgende Codebeispiel zeigt, wie Sie dies tun.

**Hinzufügen eines Clients zu einer Gruppe und anschließendes Messaging dieses Clients**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistenz der Gruppenmitgliedschaft

Signalr verfolgt Verbindungen und keine Benutzer nach. Wenn Sie also möchten, dass sich ein Benutzer jedes Mal, wenn der Benutzer eine Verbindung herstellt, in derselben Gruppe befindet, müssen Sie `Groups.Add` jedes Mal anrufen, wenn der Benutzer eine neue Verbindung herstellt.

Nach einem vorübergehenden Verbindungsverlust kann die Verbindung manchmal von signalr automatisch wieder hergestellt werden. In diesem Fall wird die gleiche Verbindung von signalr wieder hergestellt, und es wird keine neue Verbindung hergestellt, sodass die Gruppenmitgliedschaft des Clients automatisch wieder hergestellt wird. Dies ist auch dann möglich, wenn die temporäre Unterbrechung das Ergebnis eines Serverneustarts oder eines Fehlers ist, da der Verbindungsstatus für jeden Client, einschließlich der Gruppenmitgliedschaften, auf den Client gerundet wird. Wenn ein Server ausfällt und vor dem Verbindungs Timeout durch einen neuen Server ersetzt wird, kann ein Client automatisch eine Verbindung mit dem neuen Server herstellen und sich erneut bei Gruppen anmelden, bei denen er Mitglied ist.

Wenn eine Verbindung nach einem Verbindungsverlust oder einem Verbindungs Timeout nicht automatisch wieder hergestellt werden kann, oder wenn der Client die Verbindung trennt (z. b. Wenn ein Browser zu einer neuen Seite navigiert), gehen Gruppenmitgliedschaften verloren. Wenn der Benutzer das nächste Mal eine Verbindung herstellt, wird eine neue Verbindung hergestellt. Um die Gruppenmitgliedschaften beizubehalten, wenn derselbe Benutzer eine neue Verbindung herstellt, muss Ihre Anwendung die Zuordnungen zwischen Benutzern und Gruppen nachverfolgen und Gruppenmitgliedschaften jedes Mal wiederherstellen, wenn ein Benutzer eine neue Verbindung herstellt.

Weitere Informationen zu Verbindungen und erneuten Verbindungen finden Sie unter [Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse](#connectionlifetime) weiter unten in diesem Thema.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Einzelbenutzer Gruppen

Anwendungen, die signalr verwenden, müssen in der Regel die Zuordnungen zwischen Benutzern und Verbindungen nachverfolgen, um zu ermitteln, welcher Benutzer eine Nachricht gesendet hat und welche Benutzer eine Nachricht erhalten sollen. Gruppen werden in einem der beiden häufig verwendeten Muster verwendet.

- Einzelbenutzer Gruppen.

    Sie können den Benutzernamen als Gruppennamen angeben und die aktuelle Verbindungs-ID der Gruppe jedes Mal hinzufügen, wenn der Benutzer eine Verbindung herstellt oder die Verbindung wiederherstellt. Zum Senden von Nachrichten an den Benutzer, den Sie an die Gruppe senden. Ein Nachteil dieser Methode besteht darin, dass die Gruppe Ihnen keine Möglichkeit bietet, herauszufinden, ob der Benutzer online oder offline ist.
- Nachverfolgen von Zuordnungen zwischen Benutzernamen und Verbindungs-IDs.

    Sie können eine Zuordnung zwischen den einzelnen Benutzernamen und mindestens einer Verbindungs-ID in einem Wörterbuch oder einer Datenbank speichern und die gespeicherten Daten jedes Mal aktualisieren, wenn der Benutzer eine Verbindung herstellt oder die Verbindung trennt. Um Nachrichten an den Benutzer zu senden, geben Sie die Verbindungs-IDs an. Ein Nachteil dieser Methode besteht darin, dass Sie mehr Arbeitsspeicher benötigt.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse

Typische Gründe für die Behandlung von Verbindungs Lebensdauer-Ereignissen besteht darin, zu verfolgen, ob ein Benutzer verbunden ist oder nicht, und die Zuordnung zwischen Benutzernamen und Verbindungs-IDs nachzuverfolgen. Um eigenen Code auszuführen, wenn Clients eine Verbindung herstellen oder trennen, überschreiben Sie die virtuellen Methoden `OnConnected`, `OnDisconnected`und `OnReconnected` der Hub-Klasse, wie im folgenden Beispiel gezeigt.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Wenn "onconnected", "ongetrennte" und "onreconnected" aufgerufen werden

Jedes Mal, wenn ein Browser zu einer neuen Seite navigiert, muss eine neue Verbindung eingerichtet werden. Dies bedeutet, dass signalr die `OnDisconnected`-Methode gefolgt von der `OnConnected`-Methode ausführt. Signalr erstellt immer eine neue Verbindungs-ID, wenn eine neue Verbindung hergestellt wird.

Die `OnReconnected`-Methode wird aufgerufen, wenn eine temporäre Verbindungsunterbrechung vorliegt, von der signalr automatisch wieder hergestellt werden kann, z. b. Wenn ein Kabel vorübergehend getrennt wird und die Verbindung wieder hergestellt wird, bevor ein Timeout der Verbindung auftritt. Die `OnDisconnected`-Methode wird aufgerufen, wenn der Client getrennt wird und signalr nicht automatisch wieder verbunden werden kann, z. b. Wenn ein Browser zu einer neuen Seite navigiert. Daher ist eine mögliche Sequenz von Ereignissen für einen bestimmten Client `OnConnected`, `OnReconnected``OnDisconnected`; oder `OnConnected``OnDisconnected`. Die Sequenz `OnConnected`, `OnDisconnected``OnReconnected` für eine bestimmte Verbindung wird nicht angezeigt.

Die `OnDisconnected`-Methode wird in einigen Szenarios nicht aufgerufen, z. b. Wenn ein Server ausfällt oder die APP-Domäne wieder verwendet wird. Wenn ein anderer Server Online ist oder die APP-Domäne die Wiederverwendung beendet, können einige Clients möglicherweise erneut eine Verbindung herstellen und das `OnReconnected` Ereignis auslösen.

Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Aufruferstatus nicht aufgefüllt

Die Ereignishandlermethoden der Verbindungs Lebensdauer werden vom Server aufgerufen, d. h. jeder Zustand, den Sie im `state` Objekt auf dem Client ablegen, wird nicht in der `Caller`-Eigenschaft auf dem Server aufgefüllt. Weitere Informationen über das `state`-Objekt und die `Caller`-Eigenschaft finden Sie weiter unten in diesem Thema unter [übergeben des Zustands zwischen Clients und der Hub-Klasse](#passstate) .

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>So erhalten Sie Informationen über den Client aus der Kontext Eigenschaft

Um Informationen über den Client zu erhalten, verwenden Sie die `Context`-Eigenschaft der Hub-Klasse. Die `Context`-Eigenschaft gibt ein [hubcallercontext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) -Objekt zurück, das den Zugriff auf die folgenden Informationen ermöglicht:

- Die Verbindungs-ID des aufrufenden Clients.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    Die Verbindungs-ID ist eine GUID, die von signalr zugewiesen wird (Sie können den Wert nicht in Ihrem eigenen Code angeben). Es gibt eine Verbindungs-ID für jede Verbindung, und die gleiche Verbindungs-ID wird von allen Hubs verwendet, wenn Sie über mehrere Hubs in Ihrer Anwendung verfügen.
- HTTP-Header Daten.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Sie können auch HTTP-Header aus `Context.Headers`erhalten. Der Grund für mehrere Verweise auf dasselbe Ergebnis ist, dass `Context.Headers` zuerst erstellt wurde, die `Context.Request`-Eigenschaft später hinzugefügt wurde und `Context.Headers` aus Gründen der Abwärtskompatibilität beibehalten wurde.
- Abfrage Zeichen folgen Daten.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    Sie können auch Abfrage Zeichenfolgen-Daten aus `Context.QueryString`erhalten.

    Die Abfrage Zeichenfolge, die Sie in dieser Eigenschaft erhalten, ist die Abfrage Zeichenfolge, die mit der HTTP-Anforderung verwendet wurde, die die signalr-Verbindung Sie können dem Client Abfrage Zeichenfolgen-Parameter hinzufügen, indem Sie die Verbindung konfigurieren. Dies ist eine bequeme Möglichkeit, Daten über den Client vom Client an den Server zu übergeben. Das folgende Beispiel zeigt eine Möglichkeit, eine Abfrage Zeichenfolge in einem JavaScript-Client hinzuzufügen, wenn Sie den generierten Proxy verwenden.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Weitere Informationen zum Festlegen von Abfrage Zeichen folgen Parametern finden Sie in den API-Handbüchern für die [JavaScript](hubs-api-guide-javascript-client.md) -und [.net](hubs-api-guide-net-client.md) -Clients.

    Sie finden die Transportmethode, die für die Verbindung verwendet wird, in den Abfrage Zeichenfolgen-Daten zusammen mit einigen anderen Werten, die intern von signalr verwendet werden:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Der Wert von `transportMethod` wird "websockets", "serversentevents", "foreverframe" oder "longabruf" lauten. Beachten Sie Folgendes: Wenn Sie diesen Wert in der `OnConnected`-Ereignishandlermethode überprüfen, erhalten Sie in einigen Szenarien anfänglich möglicherweise einen Transport Wert, der nicht die endgültige aushandelte Transportmethode für die Verbindung ist. In diesem Fall löst die Methode eine Ausnahme aus und wird später erneut aufgerufen, wenn die letzte Transportmethode eingerichtet wird.
- KS.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    Sie können auch Cookies aus `Context.RequestCookies`erhalten.
- Benutzerinformationen.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- Das HttpContext-Objekt für die Anforderung:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Verwenden Sie diese Methode, anstatt `HttpContext.Current`, um das `HttpContext`-Objekt für die signalr-Verbindung zu erhalten.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Übergeben des Zustands zwischen Clients und der Hub-Klasse

Der-Client Proxy stellt ein `state` Objekt bereit, in dem Sie Daten speichern können, die mit jedem Methoden aufrufan den Server übertragen werden sollen. Auf dem-Server können Sie auf diese Daten in der `Clients.Caller`-Eigenschaft in hubmethoden zugreifen, die von-Clients aufgerufen werden. Die `Clients.Caller`-Eigenschaft wird für die Verbindungs Lebensdauer-Ereignishandlermethoden `OnConnected`, `OnDisconnected`und `OnReconnected`nicht aufgefüllt.

Das Erstellen oder Aktualisieren von Daten im `state` Objekt und in der `Clients.Caller`-Eigenschaft funktioniert in beide Richtungen. Sie können Werte auf dem Server aktualisieren, die an den Client zurückgegeben werden.

Das folgende Beispiel zeigt JavaScript-Client Code, der den Status für die Übertragung an den Server mit jedem Methoden Aufrufwert speichert.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Das folgende Beispiel zeigt den entsprechenden Code in einem .NET-Client.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

In ihrer Hub-Klasse können Sie auf diese Daten in der `Clients.Caller`-Eigenschaft zugreifen. Das folgende Beispiel zeigt Code, der den Zustand abruft, auf den im vorherigen Beispiel verwiesen wird.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Dieser Mechanismus für das Beibehalten des Zustands ist nicht für große Datenmengen gedacht, da alles, was Sie in der `state`-oder `Clients.Caller`-Eigenschaft ablegen, bei jedem Methodenaufruf Roundtrip umfasst. Dies ist nützlich für kleinere Elemente, wie z. b. Benutzernamen oder Leistungsindikatoren.

In VB.net oder in einem stark typisierten Hub kann nicht über `Clients.Caller`auf das aufruferstatusobjekt zugegriffen werden. Verwenden Sie stattdessen `Clients.CallerState` (eingeführt in signalr 2,1):

**Verwenden von callerstate inC#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Verwenden von callerstate in Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Behandeln von Fehlern in der Hub-Klasse

Um Fehler zu behandeln, die in den hubklassen Methoden auftreten, stellen Sie zunächst sicher, dass Sie alle Ausnahmen von asynchronen Vorgängen (z. b. das Aufrufen von Client Methoden) mithilfe von `await`"beobachten. Verwenden Sie dann mindestens eine der folgenden Methoden:

- Packen Sie den Methoden Code in try-catch-Blöcken, und protokollieren Sie das Ausnahme Objekt. Zu Debuggingzwecken können Sie die Ausnahme an den Client senden, aber aus Sicherheitsgründen wird das Senden detaillierter Informationen an Clients in der Produktionsumgebung nicht empfohlen.
- Erstellen Sie ein Hubs Pipeline Modul, das die [onincomingerror](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) -Methode behandelt. Das folgende Beispiel zeigt ein Pipeline Modul, das Fehler protokolliert, gefolgt von Code in Startup.cs, der das Modul in die Hubs-Pipeline einfügt.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Verwenden Sie die `HubException`-Klasse (eingeführt in signalr 2). Dieser Fehler kann von jedem hubaufruf ausgelöst werden. Der `HubError`-Konstruktor nimmt eine Zeichen folgen Nachricht und ein Objekt zum Speichern zusätzlicher Fehler Daten an. Signalr serialisiert die Ausnahme automatisch und sendet Sie an den Client, wo Sie verwendet wird, um den Aufruf der hubmethode abzulehnen oder fehlschlagen zu lassen.

    Die folgenden Codebeispiele veranschaulichen, wie eine `HubException` während eines hubaufzurufenden ausgelöst wird und wie die Ausnahme auf JavaScript-und .NET-Clients behandelt wird.

    **Der Server Code demonstriert die hubexception-Klasse.**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **JavaScript-Client Code, der die Antwort auf das Auslösen einer hubexception in einem Hub demonstriert**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **.NET-Client Code, der die Antwort auf das Auslösen einer hubexception in einem Hub demonstriert**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Weitere Informationen zu Hub-Pipeline Modulen finden Sie weiter unten in diesem Thema unter [Anpassen der Hubs-Pipeline](#hubpipeline) .

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Aktivieren der Ablauf Verfolgung

Um die serverseitige Ablauf Verfolgung zu aktivieren, fügen Sie der Datei "Web. config" ein System. Diagnostics-Element hinzu, wie im folgenden Beispiel gezeigt:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Wenn Sie die Anwendung in Visual Studio ausführen, können Sie die Protokolle im **Ausgabe** Fenster anzeigen.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der hubklasse

Um Client Methoden aus einer anderen Klasse als der hubklasse aufzurufen, erhalten Sie einen Verweis auf das signalr-Kontext Objekt für den Hub und verwenden dieses, um Methoden auf dem Client aufzurufen oder Gruppen zu verwalten.

Das folgende Beispiel `StockTicker` Klasse ruft das Kontext Objekt ab, speichert es in einer Instanz der-Klasse, speichert die-Klasseninstanz in einer statischen-Eigenschaft und verwendet den Kontext aus der Singleton-Klasseninstanz, um die `updateStockPrice`-Methode auf Clients aufzurufen, die mit einem Hub mit dem Namen `StockTickerHub`verbunden sind.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Wenn Sie den Kontext mehrmals in einem langlebiges Objekt verwenden müssen, sollten Sie den Verweis einmal erhalten und speichern, anstatt ihn jedes Mal erneut zu erhalten. Wenn Sie den Kontext einmal erhalten, stellen Sie sicher, dass signalr Nachrichten in derselben Reihenfolge an Clients sendet, in der die hubmethoden Aufrufe der Client Methode ausführen. Ein Tutorial, das zeigt, wie der signalr-Kontext für einen Hub verwendet wird, finden Sie unter [Server Broadcast mit ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Aufrufen von Client Methoden

Sie können angeben, welche Clients den RPC erhalten sollen, aber Sie haben weniger Optionen als beim Aufruf von einer Hub-Klasse. Der Grund hierfür ist, dass der Kontext keinem bestimmten Client von einem Client zugeordnet ist, sodass alle Methoden, die Kenntnisse der aktuellen Verbindungs-ID erfordern (z. b. `Clients.Others`oder `Clients.Caller`oder `Clients.OthersInGroup`), nicht verfügbar sind. Die folgenden Optionen sind verfügbar:

- Alle verbundenen Clients.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Ein spezifischer Client, der durch die Verbindungs-ID identifiziert wird

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Alle verbundenen Clients mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Alle verbundenen Clients in einer bestimmten Gruppe.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients, die durch die Verbindungs-ID identifiziert werden.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Wenn Sie die nicht-Hub-Klasse von Methoden in ihrer Hub-Klasse aufrufen, können Sie die aktuelle Verbindungs-ID übergeben und diese mit `Clients.Client`, `Clients.AllExcept`oder `Clients.Group` verwenden, um `Clients.Caller`, `Clients.Others`oder `Clients.OthersInGroup`zu simulieren. Im folgenden Beispiel übergibt die `MoveShapeHub`-Klasse die Verbindungs-ID an die `Broadcaster`-Klasse, damit die `Broadcaster`-Klasse `Clients.Others`simulieren kann.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Verwalten der Gruppenmitgliedschaft

Zum Verwalten von Gruppen haben Sie dieselben Optionen wie in einer Hub-Klasse.

- Hinzufügen eines Clients zu einer Gruppe

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Entfernen eines Clients aus einer Gruppe

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Vorgehensweise beim Anpassen der Hubs-Pipeline

Mit signalr können Sie Ihren eigenen Code in die Hub-Pipeline einfügen. Das folgende Beispiel zeigt ein benutzerdefiniertes Hub Pipeline Modul, das jeden eingehenden Methodenaufruf protokolliert, der vom Client empfangen wurde, und den Aufruf der ausgehenden Methode, der auf dem Client

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Der folgende Code in der Datei *Startup.cs* registriert das Modul, das in der Hub-Pipeline ausgeführt werden soll:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Es gibt viele verschiedene Methoden, die Sie überschreiben können. Eine umfassende Liste finden Sie unter [hubpipelinemodule-Methoden](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
