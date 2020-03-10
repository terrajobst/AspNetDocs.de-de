---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Aktivieren von Ursprungs übergreifenden Anforderungen in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt, wie Cross-Origin Resource Sharing (cors) in ASP.net-Web-API unterstützt wird.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447615"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>Aktivieren von Ursprungs übergreifenden Anforderungen in ASP.net-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

> Die Browsersicherheit verhindert, dass eine Webseite AJAX-Anforderungen an eine andere Domäne richtet. Diese Einschränkung wird als *Richtlinie des gleichen Ursprungs* bezeichnet und verhindert, dass eine schädliche Website sensible Daten von einer anderen Website liest. Manchmal sollen andere Websites jedoch unter Umständen Ihre Web-API aufrufen können.
>
> [Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (cors) ist ein W3C-Standard, der es einem Server ermöglicht, die Richtlinie für denselben Ursprung zu lockern. Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen. Cors ist sicherer und flexibler als frühere Techniken wie [JSONP](http://en.wikipedia.org/wiki/JSONP). In diesem Tutorial wird gezeigt, wie Sie cors in Ihrer Web-API-Anwendung aktivieren.
>
> ## <a name="software-used-in-the-tutorial"></a>Im Tutorial verwendete Software
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web-API 2,2

## <a name="introduction"></a>Einführung

In diesem Tutorial wird die cors-Unterstützung in ASP.net-Web-API veranschaulicht. Zunächst erstellen wir zwei ASP.net-Projekte – eine mit dem Namen "Webservice", die einen Web-API-Controller hostet, und die andere mit dem Namen "Webclient", die Webservice aufruft. Da die beiden Anwendungen in verschiedenen Domänen gehostet werden, ist eine AJAX-Anforderung von WebClient an Webservice eine Ursprungs übergreifende Anforderung.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Was ist "derselbe Ursprung"?

Zwei URLs haben denselben Ursprung, wenn Sie identische Schemas, Hosts und Ports aufweisen. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Diese beiden URLs haben denselben Ursprung:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Diese URLs haben andere Ursprünge als die vorherigen beiden:

- `http://example.net`-andere Domäne
- `http://example.com:9000/foo.html`-anderer Port
- `https://example.com/foo.html`-anderes Schema
- `http://www.example.com/foo.html`-Unterdomäne

> [!NOTE]
> Internet Explorer berücksichtigt den Port beim Vergleich von Ursprüngen nicht.

## <a name="create-the-webservice-project"></a>Erstellen des Webservice-Projekts

> [!NOTE]
> In diesem Abschnitt wird davon ausgegangen, dass Sie bereits über das Erstellen von Web-API-Projekten Falls nicht, finden Sie weitere Informationen unter [Getting Started with ASP.net-Web-API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Starten Sie Visual Studio, und erstellen Sie ein neues Projekt der **ASP.NET-Webanwendung (.NET Framework)** .
2. Wählen Sie im Dialogfeld **neue ASP.NET-Webanwendung** die Vorlage **leeres** Projekt aus. Wählen Sie unter **Ordner und Kern Verweise hinzufügen für**das Kontrollkästchen **Web-API** aus.

   ![Dialogfeld "neues ASP.net Projekt" in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Fügen Sie einen Web-API-Controller mit dem Namen `TestController` mit folgendem Code hinzu:

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. Sie können die Anwendung lokal ausführen oder in Azure bereitstellen. (Für die Screenshots in diesem Tutorial wird die APP für Azure App Service Web-Apps bereitgestellt.) Um zu überprüfen, ob die Web-API funktioniert, navigieren Sie zu `http://hostname/api/test/`, wobei *Hostname* die Domäne ist, in der Sie die Anwendung bereitgestellt haben. Der Antworttext sollte angezeigt werden, &quot;Get: Test Message&quot;.

   ![Webbrowser zeigt eine Test Meldung an](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>Erstellen des WebClient-Projekts

1. Erstellen Sie eine weitere **ASP.NET-Webanwendung (.NET Framework)** , und wählen Sie die **MVC** -Projektvorlage aus. Wählen Sie optional **Authentifizierung ändern** > **keine Authentifizierung**aus. Für dieses Tutorial ist keine Authentifizierung erforderlich.

   ![MVC-Vorlage im Dialogfeld "neues ASP.net Projekt" in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. Öffnen Sie in **Projektmappen-Explorer**die Datei *views/Home/Index. cshtml*. Ersetzen Sie den Code in dieser Datei durch Folgendes:

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   Verwenden Sie für die *serviceUrl* -Variable den URI der Webservice-app.

3. Führen Sie die WebClient-App lokal aus, oder veröffentlichen Sie Sie auf einer anderen Website.

Wenn Sie auf die Schaltfläche "ausprobieren" klicken, wird eine AJAX-Anforderung an die Webdienst-App übermittelt, die die im Dropdown Feld (Get, Post, Put) aufgeführte HTTP-Methode verwendet. Auf diese Weise können Sie verschiedene Ursprungs übergreifende Anforderungen untersuchen. Zurzeit unterstützt die Webservice-App keine cors. Wenn Sie also auf die Schaltfläche klicken, wird eine Fehlermeldung angezeigt.

![Fehler "try it" im Browser](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Wenn Sie den HTTP-Datenverkehr in einem Tool wie z. b. " [fddler](https://www.telerik.com/fiddler)" beobachten, werden Sie feststellen, dass der Browser die Get-Anforderung sendet und die Anforderung erfolgreich ist, aber der AJAX-Befehl gibt einen Fehler zurück. Es ist wichtig zu verstehen, dass die Richtlinie für denselben Ursprung nicht verhindert, dass der Browser die Anforderung *sendet* . Stattdessen wird verhindert, dass die Anwendung die *Antwort*sieht.

![Webdebugger für den webdebugger, der Webanforderungen anzeigt](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>Aktivieren von CORS

Nun aktivieren wir cors in der Webdienst-app. Fügen Sie zunächst das cors-nuget-Paket hinzu. Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Mit diesem Befehl wird das neueste Paket installiert, und alle Abhängigkeiten, einschließlich der Web-API-Kernbibliotheken, werden aktualisiert. Verwenden Sie das `-Version` Flag, um eine bestimmte Version als Ziel zu verwenden. Für das cors-Paket ist die Web-API 2,0 oder höher erforderlich.

Öffnen Sie die Datei *-App\_Start/webapiconfig. cs*. Fügen Sie der **webapiconfig. Register** -Methode den folgenden Code hinzu:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Fügen Sie als nächstes das **[enablecors]** -Attribut zur `TestController`-Klasse hinzu:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Verwenden Sie für den *Ursprungs* Parameter den URI, in dem Sie die WebClient Anwendung bereitgestellt haben. Dies ermöglicht Ursprungs übergreifende Anforderungen von WebClient, während alle anderen Domänen übergreifenden Anforderungen immer noch nicht zugelassen werden. Später beschreibe ich die Parameter für **[enablecors]** ausführlicher.

Fügen Sie am Ende der *Ursprungs* -URL keinen Schrägstrich ein.

Stellen Sie die aktualisierte Webservice-Anwendung erneut bereit. Sie müssen WebClient nicht aktualisieren. Die AJAX-Anforderung von WebClient sollte jetzt erfolgreich sein. Die Methoden get, Put und Post sind zulässig.

![Webbrowser zeigt eine erfolgreiche Testnachricht an](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>Funktionsweise von cors

In diesem Abschnitt wird beschrieben, was in einer cors-Anforderung auf der Ebene der HTTP-Nachrichten geschieht. Es ist wichtig, die Funktionsweise von cors zu verstehen, sodass Sie das Attribut **[enablecors]** ordnungsgemäß konfigurieren und beheben können, wenn die Dinge nicht erwartungsgemäß funktionieren.

In der cors-Spezifikation werden mehrere neue HTTP-Header eingeführt, die Ursprungs übergreifende Anforderungen ermöglichen. Wenn ein Browser cors unterstützt, werden diese Header automatisch für Ursprungs übergreifende Anforderungen festgelegt. Sie müssen in Ihrem JavaScript-Code nichts Besonderes tun.

Im folgenden finden Sie ein Beispiel für eine Ursprungs übergreifende Anforderung. Der "Origin"-Header gibt die Domäne der Website an, von der die Anforderung stammt.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Wenn der Server die Anforderung zulässt, wird der Access-Control-Allow-Origin-Header festgelegt. Der Wert dieses Headers entspricht entweder dem Ursprungs Header oder ist der Platzhalter Wert "\*", was bedeutet, dass ein beliebiger Ursprung zulässig ist.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Wenn die Antwort den Access-Control-Allow-Origin-Header nicht einschließt, schlägt die AJAX-Anforderung fehl. Der Browser lässt die Anforderung insbesondere nicht zu. Auch wenn der Server eine erfolgreiche Antwort zurückgibt, stellt der Browser die Antwort nicht für die Client Anwendung zur Verfügung.

**Preflight-Anforderungen**

Bei einigen cors-Anforderungen sendet der Browser eine zusätzliche Anforderung, die als "Preflight-Anforderung" bezeichnet wird, bevor die tatsächliche Anforderung für die Ressource gesendet wird.

Der Browser kann die Preflight-Anforderung überspringen, wenn die folgenden Bedingungen zutreffen:

- Die Anforderungs Methode ist *Get, Head* oder Post.
- Die Anwendung legt keine anderen Anforderungs Header als "Accept", "Accept-Language", "Content-Language", "Content-Type" oder " Last-Event-ID" fest.
- Der Content-Type-Header (falls festgelegt) ist einer der folgenden:

    - application/x-www-form-urlencoded
    - Multipart/Form-Data
    - text/plain

Die Regel für Anforderungs Header gilt für Header, die von der Anwendung festgelegt werden, indem **setrequestheader** für das **XMLHttpRequest** -Objekt aufgerufen wird. (In der cors-Spezifikation werden diese "Autoren Anforderungs Header" aufgerufen.) Die Regel gilt nicht für Header, die vom *Browser* festgelegt werden können, z. b. Benutzer-Agent, Host oder Inhalts Länge.

Im folgenden finden Sie ein Beispiel für eine Preflight-Anforderung:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode. Es enthält zwei spezielle Header:

- Access-Control-Request-Method: die HTTP-Methode, die für die tatsächliche Anforderung verwendet wird.
- Access-Control-Request-Headers: eine Liste mit Anforderungs Headern, die von der *Anwendung* für die tatsächliche Anforderung festgelegt wurden. (Auch hier sind keine Header enthalten, die vom Browser festgelegt werden.)

Hier sehen Sie eine Beispiel Antwort, in der angenommen wird, dass der Server die Anforderung zulässt:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Die Antwort enthält einen Access-Control-Allow-Methods-Header, der die zulässigen Methoden auflistet, und optional einen Access-Control-Allow-Headers-Header, der die zulässigen Header auflistet. Wenn die Preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung, wie zuvor beschrieben.

Tools, die häufig zum Testen von Endpunkten mit Preflight-Options Anforderungen (z. b. " [fddler](https://www.telerik.com/fiddler) " und [Postman](https://www.getpostman.com/)) verwendet werden, senden standardmäßig nicht die erforderlichen Options Header. Vergewissern Sie sich, dass die Header "`Access-Control-Request-Method`" und "`Access-Control-Request-Headers`" mit der Anforderung gesendet werden und dass die Options Header die APP über IIS erreichen.

Um IIS so zu konfigurieren, dass eine ASP.net-App Options Anforderungen empfangen und verarbeiten kann, fügen Sie der Datei *Web. config* der APP im Abschnitt `<system.webServer><handlers>` die folgende Konfiguration hinzu:

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

Durch das Entfernen von `OPTIONSVerbHandler` wird verhindert, dass IIS Options Anforderungen verarbeitet. Durch die Ersetzung von `ExtensionlessUrlHandler-Integrated-4.0` können Options Anforderungen die APP erreichen, da die Standardmodul Registrierung nur Get-, Head-, Post-und Debug-Anforderungen mit Erweiterungs losen URLs zulässt.

## <a name="scope-rules-for-enablecors"></a>Bereichs Regeln für [enablecors]

Sie können cors pro Aktion, pro Controller oder global für alle Web-API-Controller in der Anwendung aktivieren.

**Pro Aktion**

Um cors für eine einzelne Aktion zu aktivieren, legen Sie das **[enablecors]** -Attribut für die Aktionsmethode fest. Im folgenden Beispiel werden cors nur für die `GetItem`-Methode aktiviert.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Pro Controller**

Wenn Sie **[enablecors]** für die Controller Klasse festlegen, gilt dies für alle Aktionen auf dem Controller. Wenn Sie cors für eine Aktion deaktivieren möchten, fügen Sie der Aktion das Attribut **[disablecors]** hinzu. Im folgenden Beispiel werden cors für jede Methode außer `PutItem`aktiviert.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Global**

Wenn Sie cors für alle Web-API-Controller in der Anwendung aktivieren möchten, übergeben Sie eine **enablecorsattribute** -Instanz an die **enablecors** -Methode:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Wenn Sie das Attribut auf mehr als einen Bereich festlegen, ist die Rangfolge wie folgt:

1. Aktion
2. Controller
3. Global

## <a name="set-the-allowed-origins"></a>Festlegen der zulässigen Ursprünge

Der *Ursprungs* Parameter des **[enablecors]** -Attributs gibt an, welche Ursprünge auf die Ressource zugreifen dürfen. Der Wert ist eine durch Trennzeichen getrennte Liste der zulässigen Ursprünge.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Sie können auch den Platzhalter Wert "\*" verwenden, um Anforderungen von beliebigen Ursprüngen zuzulassen.

Berücksichtigen Sie sorgfältig, bevor Sie Anforderungen von einem beliebigen Ursprung zulassen. Dies bedeutet, dass jede Website eine AJAX-Aufrufe an Ihre Web-API durchführen kann.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>Festlegen der zulässigen HTTP-Methoden

Der *Methods* -Parameter des **[enablecors]** -Attributs gibt an, welche HTTP-Methoden für den Zugriff auf die Ressource zulässig sind. Um alle Methoden zuzulassen, verwenden Sie den Platzhalter Wert "\*". Im folgenden Beispiel werden nur Get-und Post-Anforderungen ermöglicht.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>Festlegen der zulässigen Anforderungs Header

In diesem Artikel wurde beschrieben, wie eine Preflight-Anforderung einen Access-Control-Request-Headers-Header enthalten kann, der die von der Anwendung festgelegten HTTP-Header auflistet (die so genannte "Autor Request Headers"). Der *Headers* -Parameter des **[enablecors]** -Attributs gibt an, welche Autoren Anforderungs Header zulässig sind. Um Header zuzulassen, legen Sie *Header* auf "\*" fest. Um bestimmte Header in eine Whitelist aufzunehmen, legen Sie *Header* auf eine durch Trennzeichen getrennte Liste der zulässigen Header fest:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Allerdings sind Browser nicht vollständig konsistent, wenn Sie Access-Control-Request-Headers festlegen. Chrome enthält z. b. aktuell "Origin". Firefox umfasst keine Standard Header wie "Accept", auch wenn Sie von der Anwendung im Skript festgelegt werden.

Wenn Sie *Header* auf einen anderen Wert als "\*" festlegen, sollten Sie mindestens "Accept", "Content-Type" und "Origin" sowie alle benutzerdefinierten Header einschließen, die Sie unterstützen möchten.

## <a name="set-the-allowed-response-headers"></a>Festlegen der zulässigen Antwortheader

Standardmäßig macht der Browser nicht alle Antwortheader für die Anwendung verfügbar. Die standardmäßig verfügbaren Antwortheader lauten wie folgt:

- Cachesteuerung
- Inhaltssprache
- Content-Type
- Läuft ab
- Zuletzt geändert
- Pragma

In der cors-Spezifikation werden diese [einfachen Antwortheader](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)aufgerufen. Um andere Header der Anwendung zur Verfügung zu stellen, legen Sie den *exposedheaders* -Parameter von **[enablecors]** fest.

Im folgenden Beispiel legt die `Get`-Methode des Controllers einen benutzerdefinierten Header mit dem Namen "X-Custom-Header" fest. Standardmäßig macht der Browser diesen Header nicht in einer Ursprungs übergreifenden Anforderung verfügbar. Um den Header verfügbar zu machen, schließen Sie "X-Custom-Header" in *exposedheaders*ein.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Übergeben von Anmelde Informationen in Ursprungs übergreifenden Anforderungen

Anmelde Informationen erfordern eine spezielle Behandlung in einer cors-Anforderung. Standardmäßig sendet der Browser keine Anmelde Informationen mit einer Ursprungs übergreifenden Anforderung. Anmelde Informationen enthalten Cookies und http-Authentifizierungs Schemas. Um Anmelde Informationen mit einer Ursprungs übergreifenden Anforderung zu senden, muss der Client **XMLHttpRequest. withanmelde** Informationen auf "true" festlegen.

Direktes Verwenden von " **XMLHttpRequest** ":

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

In jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Außerdem muss der Server die Anmelde Informationen zulassen. Um Ursprungs übergreifende Anmelde Informationen in der Web-API zuzulassen, legen Sie für das Attribut **[enablecors]** die **supportscredenatyp** -Eigenschaft auf true fest:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Wenn diese Eigenschaft true ist, enthält die HTTP-Antwort einen Access-Control-Allow-Anmelde Header. Dieser Header weist den Browser an, dass der Server Anmelde Informationen für eine Ursprungs übergreifende Anforderung zulässt.

Wenn der Browser Anmelde Informationen sendet, die Antwort jedoch keinen gültigen Header "Access-Control-Allow-Anmelde Informationen" enthält, macht der Browser die Antwort an die Anwendung nicht verfügbar, und die AJAX-Anforderung schlägt fehl.

Beachten Sie, dass **supportscredenatyp** auf true festgelegt wird, da es eine Website in einer anderen Domäne bedeutet, dass die Anmelde Informationen des angemeldeten Benutzers im Auftrag des Benutzers an Ihre Web-API gesendet werden können, ohne dass der Benutzer sich bewusst ist. In der cors-Spezifikation wird auch festgelegt, dass das Festlegen von *Ursprüngen* auf &quot;\*&quot; ungültig ist, wenn **supportscredenseins** true ist.

## <a name="custom-cors-policy-providers"></a>Benutzerdefinierte cors-Richtlinien Anbieter

Das **[enablecors]** -Attribut implementiert die **icorspolicyprovider** -Schnittstelle. Sie können Ihre eigene Implementierung bereitstellen, indem Sie eine Klasse erstellen, die vom- **Attribut** abgeleitet ist und **icorspolicyprovider**implementiert.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Nun können Sie das Attribut an jedem Ort anwenden, den Sie **[enablecors]** einfügen würden.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Ein benutzerdefinierter cors-Richtlinien Anbieter könnte z. b. die Einstellungen aus einer Konfigurationsdatei lesen.

Als Alternative zur Verwendung von Attributen können Sie ein **icorspolicyproviderfactory** -Objekt registrieren, das **icorspolicyprovider** -Objekte erstellt.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Um **icorspolicyproviderfactory**festzulegen, nennen Sie die **setcorspolicyproviderfactory** -Erweiterungsmethode beim Start wie folgt:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Browserunterstützung

Das Web-API-cors-Paket ist eine serverseitige Technologie. Der Browser des Benutzers muss auch cors unterstützen. Glücklicherweise beinhalten die aktuellen Versionen aller wichtigen Browser [Unterstützung für cors](http://caniuse.com/cors).
