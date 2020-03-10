---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Aktivieren der Windows-Authentifizierung in Katana | Microsoft-Dokumentation
author: MikeWasson
description: 'In diesem Artikel wird gezeigt, wie Sie die Windows-Authentifizierung in Katana aktivieren. Es umfasst zwei Szenarien: die Verwendung von IIS zum Hosten von Katana und das Verwenden von HttpListener für das Self-Host-Zertifikat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500295"
---
# <a name="enabling-windows-authentication-in-katana"></a>Aktivieren der Windows-Authentifizierung in Katana

von [Mike Wasson](https://github.com/MikeWasson)

> In diesem Artikel wird gezeigt, wie Sie die Windows-Authentifizierung in Katana aktivieren. Es umfasst zwei Szenarien: die Verwendung von IIS zum Hosten von Katana und das Verwenden von HttpListener für das selbst Hosting von Katana in einem benutzerdefinierten Prozess. Vielen Dank an Barry dorrane, David Matson und Chris Ross zum Überprüfen dieses Artikels.

Katana ist die Microsoft-Implementierung von [owin](http://owin.org/), dem Open Web Interface für .net. Eine Einführung in owin und Katana finden Sie [hier](an-overview-of-project-katana.md). Die owin-Architektur verfügt über mehrere Ebenen:

- Host: verwaltet den Prozess, in dem die owin-Pipeline ausgeführt wird.
- Server: öffnet einen Netzwerksocket und lauscht auf Anforderungen.
- Middleware: verarbeitet die HTTP-Anforderung und-Antwort.

Katana verfügt zurzeit über zwei Server, von denen beide die integrierte Windows-Authentifizierung unterstützen:

- **Microsoft. owin. Host. systemWeb**. Verwendet IIS mit der ASP.NET-Pipeline.
- **Microsoft. owin. Host. HttpListener**. Verwendet [System .net. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Bei diesem Server handelt es sich zurzeit um die Standardoption für das Selbsthosting von Katana.

> [!NOTE]
> Katana stellt derzeit keine owin-Middleware für die Windows-Authentifizierung bereit, da diese Funktionalität bereits auf den Servern verfügbar ist.

## <a name="windows-authentication-in-iis"></a>Windows-Authentifizierung in IIS

Mithilfe von "Microsoft. owin. Host. systemWeb" können Sie die Windows-Authentifizierung in IIS einfach aktivieren.

Beginnen Sie mit der Erstellung einer neuen ASP.NET-Anwendung, indem Sie die Projektvorlage "ASP.net Empty Webanwendung" verwenden.

![](enabling-windows-authentication-in-katana/_static/image1.png)

Fügen Sie als nächstes nuget-Pakete hinzu. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Fügen Sie nun eine Klasse mit dem Namen `Startup` mit folgendem Code hinzu:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Das ist alles, was Sie benötigen, um eine "Hello World"-Anwendung für owin zu erstellen, die auf IIS ausgeführt wird. Drücken Sie F5, um die Anwendung zu debuggen. „Hello World!“ muss auf im Browserfenster.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Als nächstes aktivieren wir die Windows-Authentifizierung in IIS Express. Wählen Sie im Menü **Ansicht** die Option **Eigenschaften**aus. Klicken Sie in Projektmappen-Explorer auf den Projektnamen, um die Projekteigenschaften anzuzeigen.

Legen Sie im **Eigenschaften** Fenster die **anonyme Authentifizierung** auf **deaktiviert** fest, und legen Sie die **Windows-Authentifizierung** auf **aktiviert**fest.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Wenn Sie die Anwendung in Visual Studio ausführen, müssen IIS Express die Windows-Anmelde Informationen des Benutzers anfordern. Dies können Sie mithilfe von " [fddler](http://fiddler2.com/home) " oder einem anderen HTTP-Debugtool sehen. Im folgenden finden Sie ein Beispiel für eine HTTP-Antwort:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Die WWW-Authenticate-Header in dieser Antwort zeigen an, dass der Server das [Aushandlungs](http://www.ietf.org/rfc/rfc4559.txt) Protokoll unterstützt, das entweder Kerberos oder NTLM verwendet.

Wenn Sie die Anwendung später auf einem Server bereitstellen, führen Sie die folgenden [Schritte](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) aus, um die Windows-Authentifizierung in IIS auf diesem Server zu aktivieren.

## <a name="windows-authentication-in-httplistener"></a>Windows-Authentifizierung in HttpListener

Wenn Sie Microsoft. owin. Host. HttpListener für die selbst gehosteter Katana verwenden, können Sie die Windows-Authentifizierung direkt auf der **HttpListener** -Instanz aktivieren.

Erstellen Sie zunächst eine neue Konsolenanwendung. Fügen Sie als nächstes nuget-Pakete hinzu. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Fügen Sie nun eine Klasse mit dem Namen `Startup` mit folgendem Code hinzu:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Diese Klasse implementiert das gleiche "Hello World"-Beispiel von vor, aber Sie legt auch die Windows-Authentifizierung als Authentifizierungsschema fest.

Starten Sie in der `Main`-Funktion die owin-Pipeline:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Sie können eine Anforderung in "fddler" senden, um zu bestätigen, dass die Anwendung die Windows-Authentifizierung verwendet:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Verwandte Themen

[Übersicht über das Katana-Projekt](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Grundlegendes zur owin-Formular Authentifizierung in MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
