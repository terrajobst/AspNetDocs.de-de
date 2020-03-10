---
uid: web-api/overview/security/basic-authentication
title: Standard Authentifizierung in ASP.net-Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der Standard Authentifizierung in ASP.net-Web-API.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447633"
---
# <a name="basic-authentication-in-aspnet-web-api"></a>Standard Authentifizierung in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

Die Standard Authentifizierung ist in [RFC 2617, http-Authentifizierung: Standard-und Digest-Zugriffs Authentifizierung](http://www.ietf.org/rfc/rfc2617.txt)definiert.

Nachteile

- Benutzer Anmelde Informationen werden in der Anforderung gesendet.
- Anmelde Informationen werden als Klartext gesendet.
- Anmelde Informationen werden bei jeder Anforderung gesendet.
- Es gibt keine Möglichkeit, sich abzumelden, es sei denn, Sie beenden die Browsersitzung.
- Anfällig für Website übergreifende Anforderungs Fälschung (CSRF); erfordert Anti-CSRF-Measures.

Vorteile

- Internet Standard.
- Wird von allen wichtigen Browsern unterstützt.
- Relativ einfaches Protokoll.

Die Standard Authentifizierung funktioniert wie folgt:

1. Wenn eine Anforderung eine Authentifizierung erfordert, gibt der Server 401 (nicht autorisiert) zurück. Die Antwort enthält einen WWW-Authenticate-Header, der angibt, dass der Server die Standard Authentifizierung unterstützt.
2. Der Client sendet eine weitere Anforderung mit den Client Anmelde Informationen im Autorisierungs Header. Die Anmelde Informationen werden als Zeichenfolge "Name: Password", Base64-codiert formatiert. Die Anmelde Informationen werden nicht verschlüsselt.

Die Standard Authentifizierung erfolgt im Kontext eines Bereichs. Der Server enthält den Namen des Bereichs im WWW-Authenticate-Header. Die Anmelde Informationen des Benutzers sind innerhalb dieses Bereichs gültig. Der genaue Bereich eines Bereichs wird vom Server definiert. Beispielsweise können Sie mehrere Bereiche definieren, um Ressourcen zu partitionieren.

![](basic-authentication/_static/image1.png)

Da die Anmelde Informationen unverschlüsselt gesendet werden, ist die Standard Authentifizierung nur über HTTPS sicher. Siehe [Arbeiten mit SSL in der Web-API](working-with-ssl-in-web-api.md).

Die Standard Authentifizierung ist auch anfällig für CSRF-Angriffe. Nachdem der Benutzer Anmelde Informationen eingegeben hat, sendet der Browser diese automatisch für die Dauer der Sitzung bei nachfolgenden Anforderungen an dieselbe Domäne. Dies schließt AJAX-Anforderungen ein. Weitere Informationen finden Sie unter [verhindern von Angriffen für Website übergreifende Anforderungs Fälschung (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Standard Authentifizierung mit IIS

IIS unterstützt die Standard Authentifizierung, aber es liegt ein Nachteil vor: der Benutzer wird anhand seiner Windows-Anmelde Informationen authentifiziert. Dies bedeutet, dass der Benutzer über ein Konto in der Domäne des Servers verfügen muss. Für eine öffentliche Website möchten Sie sich in der Regel bei einem ASP.net-Mitgliedschafts Anbieter authentifizieren.

Legen Sie den Authentifizierungsmodus in der Datei "Web. config" Ihres ASP.NET-Projekts auf "Windows" fest, um die Standard Authentifizierung mit IIS zu aktivieren:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

In diesem Modus verwendet IIS Windows-Anmelde Informationen, um sich zu authentifizieren. Außerdem müssen Sie die Standard Authentifizierung in IIS aktivieren. Wechseln Sie im IIS-Manager zur Ansicht Features, wählen Sie Authentifizierung aus, und aktivieren Sie die Standard Authentifizierung.

![](basic-authentication/_static/image2.png)

Fügen Sie in Ihrem Web-API-Projekt das `[Authorize]`-Attribut für alle Controller Aktionen hinzu, die eine Authentifizierung erfordern.

Ein Client authentifiziert sich selbst, indem der Autorisierungs Header in der Anforderung festgelegt wird. Dieser Schritt wird automatisch von Browser Clients durchgeführt. Nicht-Browser Clients müssen den-Header festlegen.

## <a name="basic-authentication-with-custom-membership"></a>Standard Authentifizierung mit benutzerdefinierter Mitgliedschaft

Wie bereits erwähnt, verwendet die in IIS integrierte Standard Authentifizierung Windows-Anmelde Informationen. Dies bedeutet, dass Sie Konten für Ihre Benutzer auf dem Host Server erstellen müssen. Allerdings werden Benutzerkonten bei einer Internetanwendung in der Regel in einer externen Datenbank gespeichert.

Der folgende Code zeigt, wie ein HTTP-Modul, das die Standard Authentifizierung ausführt. Sie können problemlos einen ASP.net-Mitgliedschafts Anbieter einbinden, indem Sie die `CheckPassword`-Methode ersetzen, die in diesem Beispiel eine Dummy-Methode ist.

In der Web-API 2 sollten Sie anstelle eines HTTP-Moduls einen [Authentifizierungs Filter](authentication-filters.md) oder eine [owin-Middleware](../../../aspnet/overview/owin-and-katana/index.md)schreiben.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Fügen Sie der Datei "Web. config" im Abschnitt " **System. Webserver** " Folgendes hinzu, um das HTTP-Modul zu aktivieren:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Ersetzen Sie "yourassemblyname" durch den Namen der Assembly (ohne die DLL-Erweiterung).

Deaktivieren Sie andere Authentifizierungs Schemas, z. b. Formulare oder Windows-Authentifizierung.
