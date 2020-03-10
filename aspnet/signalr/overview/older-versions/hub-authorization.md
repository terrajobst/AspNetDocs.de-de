---
uid: signalr/overview/older-versions/hub-authorization
title: Authentifizierung und Autorisierung für signalr-Hubs (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie einschränken, welche Benutzer oder Rollen auf hubmethoden zugreifen können.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8182677c8931f060d98d17008b16ad545bee4e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450039"
---
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Authentifizierung und Autorisierung für SignalR-Hubs (SignalR 1.x)

von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Thema wird beschrieben, wie Sie einschränken, welche Benutzer oder Rollen auf hubmethoden zugreifen können.

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

Signalr stellt das [Autorisierungs](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut bereit, um anzugeben, welche Benutzer oder Rollen Zugriff auf einen Hub oder eine Methode haben. Dieses Attribut befindet sich im `Microsoft.AspNet.SignalR`-Namespace. Sie wenden das `Authorize`-Attribut entweder auf einen Hub oder bestimmte Methoden in einem Hub an. Wenn Sie das `Authorize`-Attribut auf eine hubklasse anwenden, wird die angegebene Autorisierungs Anforderung auf alle Methoden im Hub angewendet. Die verschiedenen Typen von Autorisierungs Anforderungen, die Sie anwenden können, sind unten dargestellt. Ohne das `Authorize`-Attribut sind alle öffentlichen Methoden auf dem Hub für einen Client verfügbar, der mit dem Hub verbunden ist.

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

Sie können eine Authentifizierung für alle Hubs und hubmethoden in Ihrer Anwendung anfordern, indem Sie die Methode "Requirements [Authentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) " aufrufen, wenn die Anwendung gestartet wird. Sie können diese Methode verwenden, wenn Sie über mehrere Hubs verfügen und eine Authentifizierungsanforderung für alle erzwingen möchten. Mit dieser Methode können Sie keine Rollen-, Benutzer-oder ausgehende Autorisierung angeben. Sie können nur angeben, dass der Zugriff auf die hubmethoden auf authentifizierte Benutzer beschränkt ist. Allerdings können Sie das Autorisierungs Attribut weiterhin auf Hubs oder Methoden anwenden, um zusätzliche Anforderungen anzugeben. Alle Anforderungen, die Sie in Attributen angeben, werden zusätzlich zu den grundlegenden Authentifizierungsanforderungen angewendet.

Das folgende Beispiel zeigt eine Global. asax-Datei, die alle hubmethoden auf authentifizierte Benutzer beschränkt.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Wenn Sie die `RequireAuthentication()`-Methode nach der Verarbeitung einer signalr-Anforderung aufzurufen, löst signalr eine `InvalidOperationException`-Ausnahme aus. Diese Ausnahme wird ausgelöst, weil Sie der hubpipeline kein Modul hinzufügen können, nachdem die Pipeline aufgerufen wurde. Das vorherige Beispiel zeigt, wie Sie die `RequireAuthentication`-Methode in der `Application_Start`-Methode aufrufen, die einmal vor der Verarbeitung der ersten Anforderung ausgeführt wird.

<a id="custom"></a>

## <a name="customized-authorization"></a>Angepasste Autorisierung

Wenn Sie die Art der Autorisierung anpassen müssen, können Sie eine Klasse erstellen, die von `AuthorizeAttribute` abgeleitet ist, und die [userauthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) -Methode überschreiben. Diese Methode wird für jede Anforderung aufgerufen, um zu bestimmen, ob der Benutzer autorisiert ist, die Anforderung abzuschließen. In der überschriebenen Methode stellen Sie die erforderliche Logik für Ihr Autorisierungs Szenario bereit. Im folgenden Beispiel wird gezeigt, wie Sie die Autorisierung über eine Anspruchs basierte Identität erzwingen.

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

Wenn der .NET-Client mit einem Hub interagiert, der die ASP.NET-Formular Authentifizierung verwendet, müssen Sie das Authentifizierungs Cookie manuell auf der Verbindung festlegen. Das Cookie wird der `CookieContainer`-Eigenschaft des [hubconnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) -Objekts hinzugefügt. Das folgende Beispiel zeigt eine Konsolen-APP, die ein Authentifizierungs Cookie von einer Webseite abruft und dieses Cookie der Verbindung hinzufügt. Die im Beispiel `https://www.contoso.com/RemoteLogin` URL verweist auf eine Webseite, die Sie erstellen müssen. Auf der Seite werden der veröffentlichte Benutzername und das Kennwort abgerufen, und es wird versucht, den Benutzer mit den Anmelde Informationen anzumelden.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Die Konsolen-App stellt die Anmelde Informationen an www.contoso.com/RemoteLogin, die auf eine leere Seite verweisen können, die die folgende Code-Behind-Datei enthält.

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
