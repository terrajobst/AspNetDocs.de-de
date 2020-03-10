---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Sichern einer Web-API mit einzelnen Konten und lokalen Anmelde Informationen in ASP.net-Web-API 2,2 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Thema wird gezeigt, wie Sie eine Web-API mit OAuth2 für die Authentifizierung bei einer Mitgliedschafts Datenbank sichern. Im Tutorial Visual Studio 201 verwendete Software Versionen...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7492c4aa4c2a0a8aeed64c3462bda8fc51f35a6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447177"
---
# <a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Sichern einer Web-API mit einzelnen Konten und lokalen Anmelde Informationen in ASP.net-Web-API 2,2

von [Mike Wasson](https://github.com/MikeWasson)

[Beispiel-app herunterladen](https://github.com/MikeWasson/LocalAccountsApp)

> In diesem Thema wird gezeigt, wie Sie eine Web-API mit OAuth2 für die Authentifizierung bei einer Mitgliedschafts Datenbank sichern.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web-API 2,2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.net Identity 2,1](../../../identity/index.md)

In Visual Studio 2013 bietet die Web-API-Projektvorlage drei Authentifizierungs Optionen:

- **Einzelne Konten.** Die APP verwendet eine Mitgliedschafts Datenbank.
- **Organisations Konten.** Benutzer melden sich mit ihren Azure Active Directory, Office 365 oder lokalen Active Directory Anmelde Informationen an.
- **Windows-Authentifizierung.** Diese Option ist für Intranetanwendungen vorgesehen und verwendet das IIS-Modul für die Windows-Authentifizierung.

Weitere Informationen zu diesen Optionen finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Einzelne Konten bieten zwei Möglichkeiten für einen Benutzer, sich anzumelden:

- **Lokale Anmeldung**. Der Benutzer registriert sich am Standort und gibt einen Benutzernamen und ein Kennwort ein. Die APP speichert den Kenn Wort Hash in der Mitgliedschafts Datenbank. Wenn sich der Benutzer anmeldet, überprüft das ASP.net Identity System das Kennwort.
- **Anmeldung für soziale**Netzwerke. Der Benutzer meldet sich mit einem externen Dienst an, z. b. Facebook, Microsoft oder Google. Die App erstellt weiterhin einen Eintrag für den Benutzer in der Mitgliedschafts Datenbank, speichert aber keine Anmelde Informationen. Der Benutzer authentifiziert sich, indem er sich beim externen Dienst anmeldet.

Dieser Artikel befasst sich mit dem lokalen Anmelde Szenario. Sowohl für die lokale als auch für die soziale Anmeldung verwendet die Web-API OAuth2, um Anforderungen zu authentifizieren. Allerdings unterscheiden sich die Anmelde Informationen für die lokale Anmeldung und die Anmeldung für soziale Netzwerke.

In diesem Artikel zeige ich eine einfache APP, die es dem Benutzer ermöglicht, authentifizierte AJAX-Aufrufe an eine Web-API zu senden. Sie können den Beispielcode [hier](https://github.com/MikeWasson/LocalAccountsApp)herunterladen. In der Info Datei wird beschrieben, wie das Beispiel in Visual Studio von Grund auf neu erstellt wird.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Die Beispiel-App verwendet Knockout. js für die Datenbindung und jQuery zum Senden von AJAX-Anforderungen. Ich konzentriere mich auf die AJAX-Aufrufe, damit Sie "Knockout. js" für diesen Artikel nicht kennen müssen.

Dabei beschreibe ich Folgendes:

- Was die APP auf der Clientseite tut.
- Was geschieht auf dem Server?
- Der HTTP-Datenverkehr in der Mitte.

Zuerst müssen wir eine OAuth2-Terminologie definieren.

- *Ressource*. Einige Daten, die geschützt werden können.
- *Ressourcen Server*. Der Server, der die Ressource hostet.
- *Ressourcen Besitzer*. Die Entität, die die Berechtigung für den Zugriff auf eine Ressource erteilen kann. (Normalerweise der Benutzer.)
- *Client*: die APP, die auf die Ressource zugreifen möchte. In diesem Artikel ist der Client ein Webbrowser.
- *Zugriffs Token*. Ein Token, das den Zugriff auf eine Ressource gewährt.
- *Bearertoken*. Ein bestimmter Typ von Zugriffs Token mit der-Eigenschaft, die von jedem verwendet werden kann. Anders ausgedrückt: ein Client benötigt keinen kryptografischen Schlüssel oder anderen geheimen Schlüssel, um ein bearertoken zu verwenden. Aus diesem Grund sollten bearertoken nur über HTTPS verwendet werden, und es sollten relativ kurze Ablaufzeiten vorhanden sein.
- *Autorisierungsserver*. Ein Server, der Zugriffs Token zuweist.

Eine Anwendung kann sowohl als autorisierungsserver als auch als Ressourcen Server fungieren. Die Web-API-Projektvorlage folgt diesem Muster.

## <a name="local-login-credential-flow"></a>Anmelde Informationsfluss für lokale Anmeldung

Bei der lokalen Anmeldung verwendet die Web-API den in OAuth2 definierten Kenn [Wort Fluss des Ressourcen Besitzers](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) .

1. Der Benutzer gibt einen Namen und ein Kennwort in den Client ein.
2. Der Client sendet diese Anmelde Informationen an den autorisierungsserver.
3. Der autorisierungsserver authentifiziert die Anmelde Informationen und gibt ein Zugriffs Token zurück.
4. Für den Zugriff auf eine geschützte Ressource schließt der Client das Zugriffs Token im Autorisierungs Header der HTTP-Anforderung ein.

![](individual-accounts-in-web-api/_static/image3.png)

Wenn Sie **einzelne Konten** in der Web-API-Projektvorlage auswählen, enthält das Projekt einen autorisierungsserver, der die Benutzer Anmelde Informationen überprüft und Token ausgibt. Das folgende Diagramm zeigt den gleichen Anmelde Informationsfluss in Bezug auf Web-API-Komponenten.

![](individual-accounts-in-web-api/_static/image4.png)

In diesem Szenario fungieren Web-API-Controller als Ressourcen Server. Ein Authentifizierungs Filter überprüft Zugriffs Token, und das **[autorisieren]** -Attribut wird verwendet, um eine Ressource zu schützen. Wenn ein Controller oder eine Aktion über das Attribut **[autorisieren]** verfügt, müssen alle Anforderungen an diesen Controller oder diese Aktion authentifiziert werden. Andernfalls wird die Autorisierung verweigert, und die Web-API gibt einen Fehler 401 (nicht autorisiert) zurück.

Der autorisierungsserver und der Authentifizierungs Filter filtern beide eine [owin-Middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) -Komponente, die die Details von OAuth2 behandelt. Der Entwurf wird später in diesem Tutorial ausführlicher beschrieben.

## <a name="sending-an-unauthorized-request"></a>Senden einer nicht autorisierten Anforderung

Führen Sie die APP aus, und klicken Sie auf die Schaltfläche **API Anrufen** , um loszulegen. Wenn die Anforderung abgeschlossen ist, sollte im **Ergebnis** Feld eine Fehlermeldung angezeigt werden. Der Grund hierfür ist, dass die Anforderung kein Zugriffs Token enthält, sodass die Anforderung nicht autorisiert ist.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Die Schaltfläche **API aufrufen** sendet eine AJAX-Anforderung an ~/API/Values, die eine Web-API-Controller Aktion aufruft. Dies ist der Abschnitt von JavaScript-Code, der die AJAX-Anforderung sendet. In der Beispiel-App befindet sich der gesamte JavaScript-app-Code in der Datei scripung\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Bis der Benutzer sich anmeldet, gibt es kein bearertoken und daher keinen Autorisierungs Header in der Anforderung. Dies bewirkt, dass die Anforderung einen 401-Fehler zurückgibt.

Hier ist die HTTP-Anforderung. (Ich habe " [fddler](http://www.telerik.com/fiddler) " zum Erfassen des HTTP-Datenverkehrs verwendet.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Beachten Sie, dass die Antwort einen WWW-Authenticate-Header enthält, bei dem die Challenge auf bearfest gelegt ist. Dies gibt an, dass der Server ein bearertoken erwartet.

## <a name="register-a-user"></a>Registrieren eines Benutzers

Geben Sie im Abschnitt **registrieren** der App eine e-Mail und ein Kennwort ein, und klicken Sie auf die Schaltfläche **registrieren** .

Sie müssen für dieses Beispiel keine gültige e-Mail-Adresse verwenden, aber eine echte App bestätigt die Adresse. (Siehe [Erstellen einer Secure ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-Bestätigung und Kenn Wort](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)Zurücksetzung.) Verwenden Sie für das Kennwort etwa "Password1!" mit einem Großbuchstaben, Kleinbuchstaben, Zahlen und nicht alphanumerischen Zeichen. Um die APP auf dem neuesten Stand zu halten, habe ich die Client seitige Validierung ausgelassen. Wenn also ein Problem mit dem Kenn Wort Format vorliegt, erhalten Sie den Fehler 400 (ungültige Anforderung).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Die **Register** Schaltfläche sendet eine Post-Anforderung an ~/API/Account/Register/. Der Anforderungs Text ist ein JSON-Objekt, das den Namen und das Kennwort enthält. Dies ist der JavaScript-Code, der die Anforderung sendet:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Diese Anforderung wird von der `AccountController`-Klasse verarbeitet. Intern verwendet `AccountController` ASP.net Identity zum Verwalten der Mitgliedschafts Datenbank.

Wenn Sie die APP lokal aus Visual Studio ausführen, werden Benutzerkonten in der Tabelle "aspnettusers" in "localdb" gespeichert. Um die Tabellen in Visual Studio anzuzeigen, klicken Sie auf das Menü **Ansicht** , wählen Sie **Server-Explorer**und dann **Datenverbindungen**aus.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Abrufen eines Zugriffs Tokens

Bisher haben wir keine OAuth abgeschlossen, aber jetzt wird der OAuth-autorisierungsserver in Aktion angezeigt, wenn wir ein Zugriffs Token anfordern. Geben Sie im Bereich **Log in** der Beispiel-APP die e-Mail und das Kennwort ein, und klicken Sie auf **Anmelden**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Die Schaltfläche **Anmelden** sendet eine Anforderung an den tokenendpunkt. Der Anforderungs Text enthält die folgenden Formular-URL-codierten Daten:

- Grant\_Type: "Password"
- Benutzername: &lt;die e-Mail des Benutzers an&gt;
- Kennwort: &lt;Kennwort&gt;

Hier sehen Sie den JavaScript-Code, der die AJAX-Anforderung sendet:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Wenn die Anforderung erfolgreich ist, gibt der autorisierungsserver ein Zugriffs Token im Antworttext zurück. Beachten Sie, dass das Token im Sitzungs Speicher gespeichert wird, um es später beim Senden von Anforderungen an die API zu verwenden. Im Gegensatz zu einigen Authentifizierungs Formen (z. b. cookiebasierte Authentifizierung) fügt der Browser das Zugriffs Token nicht automatisch in nachfolgende Anforderungen ein. Die Anwendung muss dies explizit tun. Das ist eine gute Sache, da Sie [CSRF-Sicherheits](preventing-cross-site-request-forgery-csrf-attacks.md)Risiken einschränkt.

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Sie können sehen, dass die Anforderung die Anmelde Informationen des Benutzers enthält. Sie *müssen* HTTPS verwenden, um die Transportschicht Sicherheit bereitzustellen.

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Zur besseren Lesbarkeit habe ich den JSON-Code eingerückt und das Zugriffs Token gekürzt, was recht lang ist.

Die Eigenschaften `access_token`, `token_type`und `expires_in` werden von der OAuth2-Spezifikation definiert. Die anderen Eigenschaften (`userName`, `.issued`und `.expires`) dienen nur zu Informationszwecken. Sie finden den Code, mit dem diese zusätzlichen Eigenschaften in der `TokenEndpoint`-Methode in der/Providers/ApplicationOAuthProvider.cs-Datei hinzugefügt werden.

## <a name="send-an-authenticated-request"></a>Authentifizierte Anforderung senden

Nachdem wir nun über ein bearertoken verfügen, können wir eine authentifizierte Anforderung an die API senden. Dies geschieht, indem der Autorisierungs Header in der Anforderung festgelegt wird. Klicken Sie erneut auf die Schaltfläche **API abrufen** , um dies anzuzeigen.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Abmelden

Da der Browser die Anmelde Informationen oder das Zugriffs Token nicht zwischenspeichert, ist es einfach, das Token zu "vergessen", indem es aus dem Sitzungs Speicher entfernt wird:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Grundlegendes zur Projektvorlage für einzelne Konten

Wenn Sie **einzelne Konten** in der Projektvorlage ASP.NET-Webanwendung auswählen, enthält das Projekt Folgendes:

- Ein OAuth2-autorisierungsserver.
- Ein Web-API-Endpunkt zum Verwalten von Benutzerkonten
- Ein EF-Modell zum Speichern von Benutzerkonten.

Im folgenden finden Sie die wichtigsten Anwendungs Klassen, die diese Features implementieren:

- [https://login.microsoftonline.com/consumers/](`AccountController`). Stellt einen Web-API-Endpunkt zum Verwalten von Benutzerkonten bereit. Die `Register` Aktion ist die einzige Aktion, die wir in diesem Tutorial verwendet haben. Andere Methoden der-Klasse unterstützen die Kenn Wort Zurücksetzung, soziale Anmeldungen und andere Funktionen.
- in/Models/IdentityModels.cs. definierte `ApplicationUser` Diese Klasse ist das EF-Modell für Benutzerkonten in der Mitgliedschafts Datenbank.
- `ApplicationUserManager`, die in/App\_Start/identityconfig. cs definiert ist, wird diese Klasse von [usermanager](https://msdn.microsoft.com/library/dn613290.aspx) abgeleitet und führt Vorgänge für Benutzerkonten aus, z. b. das Erstellen eines neuen Benutzers, das Überprüfen von Kenn Wörtern usw. und speichert automatisch Änderungen an der Datenbank.
- [https://login.microsoftonline.com/consumers/](`ApplicationOAuthProvider`). Dieses Objekt wird in die owin-Middleware eingebunden und verarbeitet Ereignisse, die von der Middleware ausgelöst werden. Sie wird von [oauthauthorizationserverprovider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)abgeleitet.

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Konfigurieren des Autorisierungs Servers

Der folgende Code konfiguriert in StartupAuth.cs den OAuth2-autorisierungsserver.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

Die `TokenEndpointPath`-Eigenschaft ist der URL-Pfad zum autorisierungsserver Endpunkt. Das ist die URL, die die APP verwendet, um die bearertoken zu erhalten.

Die `Provider`-Eigenschaft gibt einen Anbieter an, der in die owin-Middleware einfügt und Ereignisse verarbeitet, die von der Middleware ausgelöst werden.

Hier ist der grundlegende Ablauf, wenn die APP ein Token erhalten soll:

1. Um ein Zugriffs Token zu erhalten, sendet die APP eine Anforderung an ~/Token.
2. Die OAuth-Middleware ruft `GrantResourceOwnerCredentials` für den Anbieter auf.
3. Der Anbieter Ruft die `ApplicationUserManager` auf, um die Anmelde Informationen zu überprüfen und eine Anspruchs Identität zu erstellen.
4. Wenn dies erfolgreich ist, erstellt der Anbieter ein Authentifizierungs Ticket, das verwendet wird, um das Token zu generieren.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Die OAuth-Middleware weiß nichts über die Benutzerkonten. Der Anbieter kommuniziert zwischen der Middleware und ASP.net Identity. Weitere Informationen zum Implementieren des Autorisierungs Servers finden Sie unter [owin OAuth 2,0 Authorization Server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Konfigurieren der Web-API für die Verwendung von bearertoken

In der `WebApiConfig.Register`-Methode richtet der folgende Code die Authentifizierung für die Web-API-Pipeline ein:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

Die **hostauthenticationfilter** -Klasse aktiviert die Authentifizierung mithilfe von bearertoken.

Die **suppressdefaulthostauthentication** -Methode weist die Web-API an, jegliche Authentifizierung zu ignorieren, bevor die Anforderung die Web-API-Pipeline erreicht, entweder von IIS oder von owin-Middleware. Auf diese Weise können Sie die Authentifizierung der Web-API ausschließlich auf Bearertoken einschränken.

> [!NOTE]
> Der MVC-Teil Ihrer APP kann insbesondere eine Formular Authentifizierung verwenden, die Anmelde Informationen in einem Cookie speichert. Die cookiebasierte Authentifizierung erfordert die Verwendung von Fälschungs Token, um CSRF-Angriffe zu verhindern. Das ist ein Problem für Web-APIs, da die Web-API keine bequeme Möglichkeit hat, das antifälschungstoken an den Client zu senden. (Weitere Hintergrundinformationen zu diesem Problem finden Sie unter [verhindern von CSRF-Angriffen in der Web-API](preventing-cross-site-request-forgery-csrf-attacks.md).) Der Aufruf von **suppressdefaulthostauthentication** stellt sicher, dass die Web-API nicht anfällig für CSRF-Angriffe von Anmelde Informationen ist, die in Cookies gespeichert

Wenn der Client eine geschützte Ressource anfordert, geschieht Folgendes in der Web-API-Pipeline:

1. Der **hostauthentication** -Filter Ruft die OAuth-Middleware auf, um das Token zu validieren.
2. Die Middleware konvertiert das Token in eine Anspruchs Identität.
3. An diesem Punkt wird die Anforderung *authentifiziert* , aber nicht *autorisiert*.
4. Der Autorisierungs Filter untersucht die Anspruchs Identität. Wenn die Ansprüche den Benutzer für diese Ressource autorisieren, wird die Anforderung autorisiert. Standardmäßig werden alle authentifizierten authentifizierten Anforderungen vom Attribut **[autorisieren]** autorisiert. Allerdings können Sie Sie nach Rolle oder anderen Ansprüchen autorisieren. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in der Web-API](authentication-and-authorization-in-aspnet-web-api.md).
5. Wenn die vorherigen Schritte erfolgreich ausgeführt wurden, gibt der Controller die geschützte Ressource zurück. Andernfalls empfängt der Client den Fehler 401 (nicht autorisiert).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET Identity](../../../identity/index.md)
- Grundlegendes [zu Sicherheits Features in der Spa-Vorlage für VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN-Blogbeitrag von hongye sun.
- [Dissecting the Web API Individual Accounts Template – Part 2: local Accounts](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Blog Beitrag von Dominick Baier.
- [Host Authentifizierung und Web-API mit owin](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Eine gute Erläuterung der `SuppressDefaultHostAuthentication` und `HostAuthenticationFilter` von Brock allen.
- [Anpassen von Profilinformationen in ASP.net Identity in vs 2013-Vorlagen](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). MSDN-Blogbeitrag von Pranav Rastogi.
- [Pro Anforderungs Lebensdauer-Verwaltung für die usermanager-Klasse in ASP.net Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). MSDN-Blogbeitrag von Suhas Joshi, mit einer guten Erläuterung der `UserManager`-Klasse.
