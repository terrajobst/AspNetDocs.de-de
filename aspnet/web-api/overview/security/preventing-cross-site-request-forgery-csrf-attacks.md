---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Verhindern von CSRF-Angriffen (Cross-Site Request Fälschung) in ASP.NET MVC
author: MikeWasson
description: Beschreibt den CSRF-Angriff (Cross-Site Request Fälschung) und die Implementierung von Anti-CSRF-Measures in ASP.net Web MVC.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5fb0f8bcc9e587ba4fbbf2b857d3bf7adcaafb94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447111"
---
# <a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Verhindern von CSRF-Angriffen (Cross-Site Request Fälschung) in ASP.NET MVC-Anwendungen

von [Mike Wasson](https://github.com/MikeWasson)

Website übergreifende Anforderungs Fälschung (Cross-Site Request Fälschung, CSRF) ist ein Angriff, bei dem ein böswilliger Standort eine Anforderung an einen anfälligen Standort sendet, an dem der Benutzer zurzeit angemeldet

Im folgenden finden Sie ein Beispiel für einen CSRF-Angriff:

1. Ein Benutzer meldet sich mit der Formular Authentifizierung bei `www.example.com` an.
2. Der-Server authentifiziert den Benutzer. Die Antwort vom Server enthält ein Authentifizierungs Cookie.
3. Ohne Abmeldung greift der Benutzer auf eine böswillige Website zu. Diese böswillige Site enthält das folgende HTML-Formular: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Beachten Sie, dass das Formular Action an die anfällige Site und nicht an die böswillige Site sendet. Dies ist der "Cross-Site"-Teil von CSRF.
4. Der Benutzer klickt auf die Schaltfläche Senden. Der Browser enthält das Authentifizierungs Cookie mit der Anforderung.
5. Die Anforderung wird auf dem Server mit dem Authentifizierungs Kontext des Benutzers ausgeführt und kann alle Aktionen ausführen, die ein authentifizierter Benutzer ausführen darf.

Obwohl es für dieses Beispiel erforderlich ist, dass der Benutzer auf die Schaltfläche "Formular" klickt, kann die bösartige Seite ein Skript, das das Formular automatisch übermittelt, genauso einfach ausführen. Außerdem wird durch die Verwendung von SSL kein CSRF-Angriff verhindert, da die böswillige Website eine "https://"-Anforderung senden kann.

In der Regel sind CSRF-Angriffe auf Websites möglich, die Cookies für die Authentifizierung verwenden, da Browser alle relevanten Cookies an die Zielwebsite senden. Allerdings sind CSRF-Angriffe nicht auf das Ausnutzen von Cookies beschränkt. Beispielsweise sind die Standard-und Digestauthentifizierung auch anfällig. Nachdem sich ein Benutzer mit der Standard-oder Digestauthentifizierung anmeldet hat. der Browser sendet die Anmelde Informationen automatisch, bis die Sitzung beendet wird.

## <a name="anti-forgery-tokens"></a>Fälschungs Token

Um CSRF-Angriffe zu verhindern, verwendet ASP.NET MVC antifälschungstokentoken, auch als *Anforderungs Überprüfungs Token*bezeichnet.

1. Der Client fordert eine HTML-Seite an, die ein Formular enthält.
2. Der Server enthält zwei Token in der Antwort. Ein Token wird als Cookie gesendet. Der andere wird in einem ausgeblendeten Formularfeld platziert. Die Token werden nach dem Zufallsprinzip generiert, damit ein Angreifer die Werte nicht erraten kann.
3. Wenn der Client das Formular übermittelt, muss er beide Token an den Server zurücksenden. Der Client sendet das Cookie-Token als Cookie und sendet das Formular Token innerhalb der Formulardaten. (Ein Browser Client führt dies automatisch aus, wenn der Benutzer das Formular sendet.)
4. Wenn eine Anforderung nicht beide Token enthält, wird die Anforderung vom Server nicht zugelassen.

Im folgenden finden Sie ein Beispiel für ein HTML-Formular mit einem verborgenen Formular Token:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Anti-fälschungstoken funktionieren, weil die böswillige Seite die Token des Benutzers aufgrund von Richtlinien für denselben Ursprung nicht lesen kann. (Die[gleichen Ursprungs Richtlinien](http://www.w3.org/Security/wiki/Same_Origin_Policy) verhindern, dass Dokumente, die an zwei unterschiedlichen Standorten gehostet werden, auf die Inhalte der anderen zugreifen. Im vorherigen Beispiel kann die böswillige Seite Anforderungen an example.com senden, aber Sie kann die Antwort nicht lesen.)

Um CSRF-Angriffe zu verhindern, verwenden Sie antifälschungs Token mit einem beliebigen Authentifizierungsprotokoll, bei dem der Browser nach der Anmeldung des Benutzers automatisch Anmelde Informationen sendet. Dies schließt cookiebasierte Authentifizierungsprotokolle ein, z. b. Formular Authentifizierung, sowie Protokolle wie die Basis-und Digestauthentifizierung.

Sie sollten antifälschungstokentoken für alle nicht sicheren Methoden (Post, Put, DELETE) benötigen. Stellen Sie außerdem sicher, dass sichere Methoden (Get, Head) keine Nebeneffekte haben. Wenn Sie die Domänen übergreifende Unterstützung (z. b. cors oder JSONP) aktivieren, sind sogar sichere Methoden wie Get potenziell anfällig für CSRF-Angriffe, sodass der Angreifer potenziell sensible Daten lesen kann.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Anti-fälschungstokentoken in ASP.NET MVC

Verwenden Sie die Hilfsmethode **htmlhelper. antiforgerytoken** , um die antifälschungstoken zu einer Razor Page hinzuzufügen:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Diese Methode fügt das ausgeblendete Formularfeld hinzu und legt auch das Cookie-Token fest.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF und AJAX

Das Formulartoken kann für AJAX-Anforderungen ein Problem darstellen, da eine AJAX-Anforderung unter Umständen JSON-Daten und keine HTML-Formulardaten sendet. Eine Lösung besteht darin, die Token in einem benutzerdefinierten HTTP-Header zu senden. Im folgenden Code wird Razor-Syntax zum Generieren der Token verwendet, und anschließend werden die Token einer AJAX-Anforderung hinzugefügt. Die Token werden auf dem Server generiert, indem **Antifälschung. getTokens**aufgerufen wird.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Extrahieren Sie die Token beim Verarbeiten der Anforderung aus dem Anforderungsheader. Anschließend wird die **Antifälschung. Validate** -Methode aufgerufen, um die Token zu validieren. Die **Validate** -Methode löst eine Ausnahme aus, wenn die Token nicht gültig sind.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
