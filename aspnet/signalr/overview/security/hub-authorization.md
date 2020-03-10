---
uid: signalr/overview/security/hub-authorization
title: Authentifizierung und Autorisierung für signalr-Hubs | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie einschränken, welche Benutzer oder Rollen auf hubmethoden zugreifen können. Die in diesem Thema verwendeten Software Versionen Visual Studio 2013 .NET 4,5 signalr ve...
ms.author: bradyg
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 5006af5e623da6958a6d59949c6f2cf776c77fc3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467511"
---
# <a name="authentication-and-authorization-for-signalr-hubs"></a>Authentifizierung und Autorisierung für SignalR-Hubs

von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Thema wird beschrieben, wie Sie einschränken, welche Benutzer oder Rollen auf hubmethoden zugreifen können.
>
> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr Version 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Vorherige Versionen dieses Themas
>
> Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
>
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

## <a name="overview"></a>Übersicht

Dieses Thema enthält folgende Abschnitte:

- [Attribut autorisieren](#authorizeattribute)
- [Authentifizierung für alle Hubs erforderlich](#requireauth)
- [Angepasste Autorisierung](#custom)
- [Übergeben von Authentifizierungsinformationen an Clients](#passauth)
- [Authentifizierungs Optionen für .NET-Clients](#authoptions)

    - [Cookie mit Formular Authentifizierung](#cookie)
    - [Windows-Authentifizierung](#windows)
    - [Verbindungs Header](#header)
    - [Certificate](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Attribut autorisieren

Signalr stellt das [Autorisierungs](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut bereit, um anzugeben, welche Benutzer oder Rollen Zugriff auf einen Hub oder eine Methode haben. Dieses Attribut befindet sich im `Microsoft.AspNet.SignalR`-Namespace. Sie wenden das `Authorize`-Attribut entweder auf einen Hub oder bestimmte Methoden in einem Hub an. Wenn Sie das `Authorize`-Attribut auf eine hubklasse anwenden, wird die angegebene Autorisierungs Anforderung auf alle Methoden im Hub angewendet. Dieses Thema enthält Beispiele für die verschiedenen Typen von Autorisierungs Anforderungen, die Sie anwenden können. Ohne das `Authorize`-Attribut kann ein verbundener Client auf jede öffentliche Methode auf dem Hub zugreifen.

Wenn Sie in Ihrer Webanwendung eine Rolle mit dem Namen "admin" definiert haben, können Sie angeben, dass nur Benutzer in dieser Rolle mit folgendem Code auf einen Hub zugreifen können.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Oder Sie können angeben, dass ein Hub eine Methode enthält, die für alle Benutzer verfügbar ist, und eine zweite Methode, die nur authentifizierten Benutzern zur Verfügung steht, wie unten gezeigt.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

In den folgenden Beispielen werden verschiedene Autorisierungs Szenarien behandelt:

- `[Authorize]` – nur authentifizierte Benutzer
- `[Authorize(Roles = "Admin,Manager")]` – nur authentifizierte Benutzer in den angegebenen Rollen
- `[Authorize(Users = "user1,user2")]` – nur authentifizierte Benutzer mit den angegebenen Benutzernamen
- `[Authorize(RequireOutgoing=false)]` – nur authentifizierte Benutzer können den Hub aufrufen, aber Aufrufe vom Server zurück an Clients werden nicht durch Autorisierung eingeschränkt, wie z. b., wenn nur bestimmte Benutzer eine Nachricht senden können, aber alle anderen Personen die Nachricht empfangen können. Die Eigenschaft "Requirements Outgoing" kann nur auf den gesamten Hub angewendet werden, nicht auf die Einzelpersonen Methoden innerhalb des Hubs. Wenn "Requirements Outgoing" nicht auf "false" festgelegt ist, werden nur Benutzer aufgerufen, die die Autorisierungs Anforderung erfüllen.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Authentifizierung für alle Hubs erforderlich

Sie können eine Authentifizierung für alle Hubs und hubmethoden in Ihrer Anwendung anfordern, indem Sie die Methode "Requirements [Authentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) " aufrufen, wenn die Anwendung gestartet wird. Sie können diese Methode verwenden, wenn Sie über mehrere Hubs verfügen und eine Authentifizierungsanforderung für alle erzwingen möchten. Mit dieser Methode können Sie keine Anforderungen für die Rollen-, Benutzer-oder ausgehende Autorisierung angeben. Sie können nur angeben, dass der Zugriff auf die hubmethoden auf authentifizierte Benutzer beschränkt ist. Allerdings können Sie das Autorisierungs Attribut weiterhin auf Hubs oder Methoden anwenden, um zusätzliche Anforderungen anzugeben. Jede Anforderung, die Sie in einem Attribut angeben, wird der grundlegenden Anforderung der Authentifizierung hinzugefügt.

Das folgende Beispiel zeigt eine Startdatei, die alle hubmethoden auf authentifizierte Benutzer beschränkt.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Wenn Sie die `RequireAuthentication()`-Methode nach der Verarbeitung einer signalr-Anforderung aufzurufen, löst signalr eine `InvalidOperationException`-Ausnahme aus. Signalr löst diese Ausnahme aus, da Sie der hubpipeline kein Modul hinzufügen können, nachdem die Pipeline aufgerufen wurde. Das vorherige Beispiel zeigt, wie Sie die `RequireAuthentication`-Methode in der `Configuration`-Methode aufrufen, die einmal vor der Verarbeitung der ersten Anforderung ausgeführt wird.

<a id="custom"></a>

## <a name="customized-authorization"></a>Angepasste Autorisierung

Wenn Sie die Art der Autorisierung anpassen müssen, können Sie eine Klasse erstellen, die von `AuthorizeAttribute` abgeleitet ist, und die [userauthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) -Methode überschreiben. Für jede Anforderung ruft signalr diese Methode auf, um zu bestimmen, ob der Benutzer autorisiert ist, die Anforderung abzuschließen. In der überschriebenen Methode stellen Sie die erforderliche Logik für Ihr Autorisierungs Szenario bereit. Im folgenden Beispiel wird gezeigt, wie Sie die Autorisierung über eine Anspruchs basierte Identität erzwingen.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Übergeben von Authentifizierungsinformationen an Clients

Möglicherweise müssen Sie Authentifizierungsinformationen in dem Code verwenden, der auf dem Client ausgeführt wird. Sie übergeben die erforderlichen Informationen, wenn Sie die Methoden auf dem Client aufrufen. Eine Chat-Anwendungsmethode könnte z. b. den Benutzernamen der Person, die eine Meldung bereitstellt, als Parameter übergeben, wie unten gezeigt.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Oder Sie können ein Objekt erstellen, um die Authentifizierungsinformationen darzustellen, und dieses Objekt als Parameter übergeben, wie unten gezeigt.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Sie sollten die Verbindungs-ID eines Clients niemals an andere Clients übergeben, da Sie von einem böswilligen Benutzer verwendet werden kann, um eine Anforderung von diesem Client zu imitieren.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Authentifizierungs Optionen für .NET-Clients

Wenn Sie über einen .NET-Client verfügen, z. b. eine Konsolen-APP, die mit einem Hub interagiert, der auf authentifizierte Benutzer beschränkt ist, können Sie die Authentifizierungs Anmelde Informationen in einem Cookie, dem Verbindungs Header oder einem Zertifikat übergeben. In den Beispielen in diesem Abschnitt wird gezeigt, wie diese verschiedenen Methoden zum Authentifizieren eines Benutzers verwendet werden. Dabei handelt es sich nicht um voll funktionsfähige signalr-apps. Weitere Informationen zu .NET-Clients mit signalr finden Sie im Leitfaden für die [Hubs-API-.NET-Client](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Wenn der .NET-Client mit einem Hub interagiert, der die ASP.NET-Formular Authentifizierung verwendet, müssen Sie das Authentifizierungs Cookie manuell auf der Verbindung festlegen. Das Cookie wird der `CookieContainer`-Eigenschaft des [hubconnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) -Objekts hinzugefügt. Das folgende Beispiel zeigt eine Konsolen-APP, die ein Authentifizierungs Cookie von einer Webseite abruft und dieses Cookie der Verbindung hinzufügt.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Die Konsolen-App stellt die Anmelde Informationen an <strong>www.contoso.com/RemoteLogin</strong> , die auf eine leere Seite verweisen können, die die folgende Code-Behind-Datei enthält.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows-Authentifizierung

Wenn Sie die Windows-Authentifizierung verwenden, können Sie die Anmelde Informationen des aktuellen Benutzers mithilfe der [default-Anmelde](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) Informationen-Eigenschaft übergeben. Sie legen die Anmelde Informationen für die Verbindung mit dem Wert von Default-Anmelde Informationen fest.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Verbindungs Header

Wenn Ihre Anwendung keine Cookies verwendet, können Sie Benutzerinformationen im Verbindungs Header übergeben. Beispielsweise können Sie ein Token im Verbindungs Header übergeben.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Dann überprüfen Sie im Hub das Token des Benutzers.

<a id="certificate"></a>

### <a name="certificate"></a>Zertifikat

Sie können ein Client Zertifikat übergeben, um den Benutzer zu überprüfen. Sie fügen das Zertifikat hinzu, wenn Sie die Verbindung erstellen. Das folgende Beispiel zeigt nur, wie Sie der Verbindung ein Client Zertifikat hinzufügen. die vollständige Konsolen-APP wird nicht angezeigt. Dabei wird die [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) -Klasse verwendet, die verschiedene Methoden zum Erstellen des Zertifikats bereitstellt.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
