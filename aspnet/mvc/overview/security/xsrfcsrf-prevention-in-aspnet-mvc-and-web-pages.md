---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: XSRF/CSRF-Verhinderung in ASP.NET MVC und Web Pages | Microsoft-Dokumentation
author: Rick-Anderson
description: Website übergreifende Anforderungs Fälschung (auch als XSRF oder CSRF bezeichnet) ist ein Angriff auf im Internet gehostete Anwendungen, bei denen eine böswillige Website den Zugriff auf die interacti beeinflussen kann...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: fb7e76101cbe6a874ddf5b3429ca2dc6d474334b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595763"
---
# <a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>XSRF/CSRF-Schutz bei ASP.NET MVC und Web Pages

von [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Die Website übergreifende Anforderungs Fälschung (auch als XSRF oder CSRF bezeichnet) ist ein Angriff auf im Internet gehostete Anwendungen, bei dem eine böswillige Website die Interaktion zwischen einem Client Browser und einer Website beeinflussen kann, die von diesem Browser als vertrauenswürdig eingestuft wird. Diese Angriffe können durchgeführt werden, da Webbrowser Authentifizierungs Token automatisch mit jeder Anforderung an eine Website senden. Das kanonische Beispiel ist ein Authentifizierungs Cookie, z. b. ASP. Das Formular Authentifizierungs Ticket von net. Allerdings können Websites, die einen permanenten Authentifizierungsmechanismus (z. b. Windows-Authentifizierung, Basic usw.) verwenden, diese Angriffe als Ziel verwenden.
> 
> Ein XSRF-Angriff unterscheidet sich von einem Phishing-Angriff. Phishingangriffe erfordern eine Interaktion mit dem Opfer. Bei einem Phishingangriff imitiert eine böswillige Website die Zielwebsite, und das Opfer wird getäuscht, um dem Angreifer vertrauliche Informationen bereitzustellen. Bei einem XSRF-Angriff sind häufig keine Interaktionen vom Opfer notwendig. Der Angreifer verlässt sich vielmehr darauf, dass der Browser automatisch alle relevanten Cookies an die Zielwebsite sendet.
> 
> Weitere Informationen finden Sie unter [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).

## <a name="anatomy-of-an-attack"></a>Anatomie eines Angriffs

Um einen XSRF-Angriff zu durchlaufen, sollten Sie einen Benutzer in Erwägung ziehen, der einige Online-Banking-Transaktionen ausführen möchte. Dieser Benutzer besucht zuerst woodgrovebank.com und meldet sich an. zu diesem Zeitpunkt enthält der Antwortheader das Authentifizierungs Cookie:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Da das Authentifizierungs Cookie ein Sitzungs Cookie ist, wird es automatisch vom Browser gelöscht, wenn der Browser Prozess beendet wird. Bis zu diesem Zeitpunkt schließt der Browser das Cookie jedoch automatisch mit jeder Anforderung an woodgrovebank.com ein. Der Benutzer möchte nun $1000 an ein anderes Konto übertragen. daher füllt er ein Formular auf der Bank Website aus, und der Browser sendet diese Anforderung an den Server:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Da dieser Vorgang einen Nebeneffekt hat (er initiiert eine monetäre Transaktion), hat die Bank Site eine HTTP POST-Anforderung angefordert, um diesen Vorgang zu initiieren. Der Server liest das Authentifizierungs Token aus der Anforderung, sucht nach der Kontonummer des aktuellen Benutzers, überprüft, ob genügend Geld vorhanden ist, und initiiert dann die Transaktion in das Zielkonto.

Ihr Online-Banking ist fertig, der Benutzer navigiert von der Bank Website und besucht andere Orte im Web. Eine dieser Sites – fabrikam.com – enthält das folgende Markup auf einer Seite, die in einem &lt;IFRAME-&gt;eingebettet ist:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Dies bewirkt, dass der Browser diese Anforderung vornimmt:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Der Angreifer nutzt die Tatsache, dass der Benutzer möglicherweise noch über ein gültiges Authentifizierungs Token für die Zielwebsite verfügt, und verwendet einen kleinen Code Ausschnitt, damit der Browser automatisch eine HTTP POST-Anforderung an die Zielsite sendet. Wenn das Authentifizierungs Token weiterhin gültig ist, initiiert die bankensite eine Übertragung von $250 in das Konto der Wahl des Angreifers.

### <a name="ineffective-mitigations"></a>Ineffiziente entschärfungen

Es ist interessant zu beachten, dass im obigen Szenario die Tatsache, dass woodgrovebank.com über SSL aufgerufen wurde und ein SSL-Authentifizierungs Cookie enthielt, nicht ausreichen konnte, um den Angriff zu verhindern. Der Angreifer kann das [URI-Schema](http://en.wikipedia.org/wiki/URI_scheme) (HTTPS) in ihrer &lt;Form&gt; Element angeben, und der Browser sendet weiterhin nicht abgelaufene Cookies an die Zielwebsite, solange diese Cookies mit dem URI-Schema des beabsichtigten Ziels konsistent sind.

Man könnte argumentieren, dass der Benutzer einfach nicht vertrauenswürdige Sites besuchen sollte, da der Besuch nur vertrauenswürdiger Websites die Sicherheit online bleibt. Es gibt eine wahre Wahrheit, aber leider ist dieser Ratgeber nicht immer praktikabel. Möglicherweise ist der Benutzer der lokalen News Site konsolidatedmessenger vertraut. ConsolidatedMessenger.com und besuchen Sie stattdessen diese Website, aber diese Site verfügt über ein XSS-Sicherheitsrisiko, mit dem ein Angreifer denselben Code Ausschnitt einfügen kann, der auf fabrikam.com ausgeführt wurde.

Sie können überprüfen, ob eingehende Anforderungen über einen [Referer-Header](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) verfügen, der auf Ihre Domäne verweist. Dadurch werden Anforderungen, die von einer Drittanbieter Domäne übermittelt werden, nicht mehr unterbrochen. Allerdings deaktivieren einige Benutzer den verweigerheader Ihres Browsers aus Datenschutzgründen, und Angreifer können diese Kopfzeile manchmal spoden, wenn das Opfer eine bestimmte unsichere Software installiert hat. Das Überprüfen des [verweigerheaders](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) wird nicht als sicherer Ansatz zum Verhindern von XSRF-Angriffen angesehen.

## <a name="web-stack-runtime-xsrf-mitigations"></a>XSRF-entschärfungen für Web Stack-Laufzeit

Die ASP.net-webstack-Laufzeit verwendet eine Variante des [Synchronisierungs Token-Musters](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#synchronizer-token-pattern) , um gegen XSRF-Angriffe zu schützen. Die allgemeine Form des Synchronisierungs Token-Musters besteht darin, dass zwei Anti-XSRF-Token an den Server übermittelt werden, wobei jede HTTP POST-Anforderung (zusätzlich zum Authentifizierungs Token) verwendet wird: ein Token als Cookie und das andere als Formular Wert. Die von der ASP.NET-Laufzeit generierten Tokenwerte sind von einem Angreifer nicht deterministisch oder vorhersagbar. Wenn die Token übermittelt werden, lässt der Server zu, dass die Anforderung nur fortgesetzt werden kann, wenn beide Token eine Vergleichs Überprüfung bestehen.

Das *Sitzungs Token* für die XSRF-Anforderungs Überprüfung wird als HTTP-Cookie gespeichert und enthält derzeit die folgenden Informationen in der Nutzlast:

- Ein Sicherheits Token, das aus einem zufälligen 128-Bit-Bezeichner besteht.   
 Die folgende Abbildung zeigt das Sitzungs Token für die XSRF-Anforderungs Überprüfung, das mit den Internet Explorer F12-Entwicklertools angezeigt wird: (Beachten Sie, dass es sich hierbei um die aktuelle Implementierung handelt, die sich wahrscheinlich ändern kann.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

Das *Feld Token* wird als `<input type="hidden" />` gespeichert und enthält die folgenden Informationen in seiner Nutzlast:

- Der Benutzername des angemeldeten Benutzers (falls authentifiziert).
- Alle zusätzlichen Daten, die von einem [iantiforgeryadditionaldataprovider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)bereitgestellt werden.

Die Nutzlasten der Anti-XSRF-Token werden verschlüsselt und signiert, sodass Sie den Benutzernamen nicht anzeigen können, wenn Sie Tools verwenden, um die Token zu überprüfen. Wenn die Webanwendung auf ASP.NET 4,0 ausgerichtet ist, werden [Kryptografiedienste von der machineKey. Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) -Routine bereitgestellt. Wenn die Webanwendung auf ASP.NET 4,5 oder höher ausgerichtet ist, werden [Kryptografiedienste von der machineKey. Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) -Routine bereitgestellt, die eine bessere Leistung, Erweiterbarkeit und Sicherheit bietet. Weitere Informationen finden Sie in den folgenden Blogbeiträgen:

- [Kryptografische Verbesserungen in ASP.NET 4,5, PT. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Kryptografische Verbesserungen in ASP.NET 4,5, PT. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Kryptografische Verbesserungen in ASP.NET 4,5, PT. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Erstellen der Token

Um die Anti-XSRF-Token zu generieren, rufen Sie die [@Html.AntiForgeryToken](https://msdn.microsoft.com/library/dd470175.aspx) -Methode aus einer MVC-Ansicht oder @AntiForgery.GetHtml() auf einer Razor Page auf. Die Laufzeit führt dann die folgenden Schritte aus:

1. Wenn die aktuelle HTTP-Anforderung bereits ein Anti-XSRF-Sitzungs Token enthält (das Anti-XSRF-Cookie \_\_requestverificationtoken), wird das Sicherheits Token daraus extrahiert. Wenn die HTTP-Anforderung kein Anti-XSRF-Sitzungs Token enthält oder wenn das Extrahieren des Sicherheits Tokens fehlschlägt, wird ein neues zufälliges Anti-XSRF-Token generiert.
2. Ein Anti-XSRF-Feldtoken wird mit dem Sicherheits Token aus Schritt (1) oben und der Identität des aktuell angemeldeten Benutzers generiert. (Weitere Informationen zum Bestimmen der Benutzeridentität finden Sie unten im Abschnitt **[Szenarios mit besonderem Support](#_Scenarios_with_special)** .) Wenn außerdem ein [iantiforgeryadditionaldataprovider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) konfiguriert ist, ruft die Laufzeit die [getadditionaldata](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) -Methode auf und schließt die zurückgegebene Zeichenfolge in das Feld Token ein. (Weitere Informationen finden Sie im Abschnitt **[Konfiguration und Erweiterbarkeit](#_Configuration_and_extensibility)** .)
3. Wenn ein neues Anti-XSRF-Token in Schritt (1) generiert wurde, wird ein neues Sitzungs Token erstellt, das es enthält und der ausgehenden HTTP-Cookies-Auflistung hinzugefügt wird. Das Feld Token aus Schritt (2) wird in ein `<input type="hidden" />` Element umschließt, und dieses HTML-Markup ist der Rückgabewert von `Html.AntiForgeryToken()` oder `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Validieren der Token

Zum Überprüfen der eingehenden Anti-XSRF-Token enthält der Entwickler ein [validateantiforgerytoken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) -Attribut für die MVC-Aktion oder den MVC-Controller, oder er ruft `@AntiForgery.Validate()` von der Razor Page auf. Die Laufzeit führt die folgenden Schritte aus:

1. Das eingehende Sitzungs Token und das Feld Token werden gelesen, und das Anti-XSRF-Token wird aus jedem extrahiert. Die Anti-XSRF-Token müssen in der Generierungs Routine pro Schritt (2) identisch sein.
2. Wenn der aktuelle Benutzer authentifiziert ist, wird der Benutzername mit dem Benutzernamen verglichen, der im Feld Token gespeichert ist. Die Benutzernamen müssen mit identisch sein.
3. Wenn ein [iantiforgeryadditionaldataprovider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) konfiguriert ist, ruft die Laufzeit die *validateadditionaldata* -Methode auf. Die Methode muss den booleschen Wert " *true*" zurückgeben.

Wenn die Überprüfung erfolgreich ist, kann die Anforderung fortgesetzt werden. Wenn die Validierung fehlschlägt, löst das Framework eine *httpantiforgeryexception*aus.

## <a name="failure-conditions"></a>Fehlerbedingungen

Beginnend mit der ASP.net Web Stack-Laufzeit v2 enthalten alle *httpantiforgeryexception-Ausnahmen* , die während der Validierung ausgelöst werden, ausführliche Informationen über den Fehler. Die aktuell definierten Fehlerbedingungen sind:

- Das Sitzungs Token oder das Formular Token ist in der Anforderung nicht vorhanden.
- Das Sitzungs Token oder das Formular Token ist nicht lesbar. Die wahrscheinlichste Ursache hierfür ist eine Farm mit nicht übereinstimmenden Versionen der ASP.net-webstack-Laufzeit oder einer Farm, in der sich das &lt;machineKey-&gt; Element in Web. config zwischen den Computern unterscheidet. Sie können ein Tool wie z. b. "fddler" verwenden, um diese Ausnahme zu erzwingen, indem Sie entweder das Anti-XSRF-Token manipulieren.
- Das Sitzungs Token und das Feld Token wurden ausgetauscht.
- Das Sitzungs Token und das Feld Token enthalten nicht übereinstimmende Sicherheits Token.
- Der in das Feld Token eingebettete Benutzername stimmt nicht mit dem Benutzernamen des aktuell angemeldeten Benutzers ab.
- Die *[iantiforgeryadditionaldataprovider. validateadditionaldata](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* -Methode hat *false*zurückgegeben.

Die Anti-XSRF-Funktionen können auch während der tokengenerierung oder-Validierung zusätzliche Überprüfungen durchführen, und Fehler während dieser Überprüfungen können dazu führen, dass Ausnahmen ausgelöst werden. Weitere Informationen finden Sie in den Abschnitten [WIF/ACS/Anspruchs basierte Authentifizierung](#_WIF_ACS) und **[Konfiguration und Erweiterbarkeit](#_Configuration_and_extensibility)** .

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Szenarios mit spezieller Unterstützung

### <a name="anonymous-authentication"></a>Anonyme Authentifizierung

Das Anti-XSRF-System enthält spezielle Unterstützung für anonyme Benutzer, wobei "Anonymous" als Benutzer definiert ist, in dem die *IIdentity. IsAuthenticated* -Eigenschaft " *false*" zurückgibt. Zu den Szenarien gehören das Bereitstellen von XSRF-Schutz auf der Anmeldeseite (vor der Authentifizierung des Benutzers) und benutzerdefinierte Authentifizierungs Schemas, in denen die Anwendung einen anderen Mechanismus als *IIdentity* zum Identifizieren von Benutzern verwendet.

Um diese Szenarien zu unterstützen, erinnern Sie sich, dass die Sitzungs-und Feld Token durch ein Sicherheits Token verknüpft werden, bei dem es sich um einen zufällig generierten 128-Bit-Bezeichner handelt. Dieses Sicherheits Token wird verwendet, um die Sitzung eines einzelnen Benutzers zu verfolgen, während Sie die Website navigiert, sodass es praktisch den Zweck eines anonymen Bezeichners erfüllt. Anstelle des Benutzernamens für die oben beschriebenen Generierungs-und Validierungs Routinen wird eine leere Zeichenfolge verwendet.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF/ACS/Anspruchs basierte Authentifizierung

Normalerweise verfügen die in den .NET Framework integrierten *IIdentity* -Klassen über die Eigenschaft, mit der *IIdentity.Name* einen bestimmten Benutzer innerhalb einer bestimmten Anwendung eindeutig identifizieren kann. Beispielsweise gibt *FormsIdentity.Name* den in der Mitgliedschafts Datenbank gespeicherten Benutzernamen zurück (der für alle Anwendungen in Abhängigkeit von der Datenbank eindeutig ist), *WindowsIdentity.Name* gibt die Domänen qualifizierte Identität des Benutzers zurück usw. Diese Systeme bieten nicht nur die Authentifizierung, sondern Außerdem *identifizieren* Sie Benutzer für eine Anwendung.

Bei der Anspruchs basierten Authentifizierung ist hingegen nicht unbedingt die Identifizierung eines bestimmten Benutzers erforderlich. Stattdessen sind die Typen " *ClaimsPrincipal* " und " *ClaimsIdentity* " mit einem Satz von *Anspruchs* Instanzen verknüpft, wobei die einzelnen Ansprüche "ist 18 + Jahre des Alters" oder "ist ein Administrator". Da der Benutzer nicht unbedingt identifiziert wurde, kann die Common Language Runtime die *ClaimsIdentity.Name* -Eigenschaft nicht als eindeutigen Bezeichner für diesen bestimmten Benutzer verwenden. Das Team hat praktische Beispiele gesehen, in denen *ClaimsIdentity.Name* *null*zurückgibt, einen anzeigen Amen zurückgibt oder andernfalls eine Zeichenfolge zurückgibt, die nicht für die Verwendung als eindeutiger Bezeichner für den Benutzer geeignet ist.

Viele bereit Stellungen, die die Anspruchs basierte Authentifizierung verwenden, verwenden insbesondere [Azure-Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS). ACS ermöglicht es dem Entwickler, individuelle *Identitäts Anbieter* (z. b. AD FS, den Microsoft-Konto Anbieter, OpenID-Anbieter wie Yahoo! usw.) zu konfigurieren, und die Identitäts Anbieter geben *namens*Bezeichner zurück. Diese namens Bezeichner können personenbezogene Informationen (PII) wie eine e-Mail-Adresse enthalten, oder Sie können anonymisiert werden, wie eine private persönliche ID (PPID). Unabhängig davon, dass das Tupel (Identitäts Anbieter, namens Bezeichner) ausreichend als ein geeignetes Überwachungs Token für einen bestimmten Benutzer fungiert, während Sie die Website durchsucht, kann die ASP.net Web Stack-Laufzeit das Tupel anstelle des Benutzernamens verwenden, wenn Sie erstellen. die Anti-XSRF-Feldtoken werden überprüft. Die spezifischen URIs für den Identitäts Anbieter und den namens Bezeichner lauten:

- `https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(Weitere Informationen finden Sie auf dieser [ACS-Dokumentseite](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) .)

Beim Erstellen oder Validieren eines Tokens versucht die ASP.net Web Stack-Laufzeit, eine Bindung an die Typen zu erstellen:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (für das WIF-SDK)
- `System.Security.Claims.ClaimsIdentity` (für .NET 4,5).

Wenn diese Typen vorhanden sind und die *iiiidentity* des aktuellen Benutzers einen dieser Typen implementiert oder Unterklassen implementiert, verwendet die Anti-XSRF-Funktion beim erzeugen und Überprüfen der Token das Tupel (Identity Provider, Name Identifier) anstelle des Benutzernamens. Wenn kein Tupel vorhanden ist, schlägt die Anforderung fehl, und es wird ein Fehler angezeigt, der den Entwicklern beschreibt, wie das Anti-XSRF-System konfiguriert werden soll, um den spezifischen Anspruchs basierten Authentifizierungsmechanismus zu verstehen. Weitere Informationen finden Sie im Abschnitt **[Konfiguration und Erweiterbarkeit](#_Configuration_and_extensibility)** .

### <a name="oauth--openid-authentication"></a>OAuth-/OpenID-Authentifizierung

Schließlich verfügt die Anti-XSRF-Funktion über besondere Unterstützung für Anwendungen, die OAuth oder OpenID-Authentifizierung verwenden. Diese Unterstützung ist heuristisch-basiert: Wenn die aktuelle *IIdentity.Name* mit http://oder https://beginnt, werden Benutzernamen Vergleiche mithilfe eines ordinalen Vergleichs anstelle des Standard Vergleichs der OrdinalIgnoreCase durchgeführt.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Konfiguration und Erweiterbarkeit

Gelegentlich benötigen Entwickler eine strengere Kontrolle über das Anti-XSRF-Generierungs-und Validierungs Verhalten. Beispielsweise ist das Standardverhalten von MVC-und Web Pages-Hilfsprogrammen zum automatischen Hinzufügen von HTTP-Cookies zur Antwort nicht erwünscht, und der Entwickler möchte die Token an anderer Stelle aufbewahren. Hierfür gibt es zwei APIs:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

Die *getTokens* -Methode übernimmt als Eingabe ein vorhandenes XSRF-Anforderungs Überprüfungs-Sitzungs Token (das möglicherweise NULL ist) und erzeugt als Ausgabe ein neues Sitzungs Token für die XSRF-Anforderungs Überprüfung und ein Feld Token. Die Token sind einfach nicht transparente Zeichen folgen ohne Dekoration. der *formtoken* -Wert wird beispielsweise nicht in eine &lt;Eingabe&gt; Tag umschließt. Der *newcookietoken* -Wert kann NULL sein. Wenn dies der Fall ist, ist der Wert *oldcookietoken* weiterhin gültig, und es muss kein neues Antwort Cookie festgelegt werden. Der Aufrufer von *getTokens* ist dafür verantwortlich, alle notwendigen Antwort Cookies beizubehalten oder das erforderliche Markup zu erzeugen. die *getTokens* -Methode selbst ändert die Antwort nicht als Nebeneffekt. Die *Validate* -Methode nimmt die eingehenden Sitzungs-und Feld Token an und führt die oben genannte Validierungs Logik aus.

### <a name="antiforgeryconfig"></a>Antiforgeryconfig

Der Entwickler kann das Anti-XSRF-System vom Anwendungs\_aus konfigurieren. Die Konfiguration ist Programm gesteuert. Die Eigenschaften des statischen *antiforgeryconfig* -Typs werden unten beschrieben. Die meisten Benutzer, die Ansprüche verwenden, möchten die uniqueclaimtypeidentifier-Eigenschaft festlegen.

| **Property** | **Beschreibung** |
| --- | --- |
| **Additionaldataprovider** | Ein [iantiforgeryadditionaldataprovider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) , der während der tokengenerierung zusätzliche Daten bereitstellt und während der tokenüberprüfung zusätzliche Daten verbraucht. Der Standardwert ist *null*. Weitere Informationen finden Sie im Abschnitt [iantiforgeryadditionaldataprovider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) . |
| **CookieName** | Eine Zeichenfolge, die den Namen des HTTP-Cookies bereitstellt, das verwendet wird, um das Anti-XSRF-Sitzungs Token zu speichern. Wenn dieser Wert nicht festgelegt ist, wird basierend auf dem bereitgestellten virtuellen Pfad der Anwendung automatisch ein Name generiert. Der Standardwert ist *null*. |
| **RequireSsl** | Ein boolescher Wert, der festlegt, ob die Anti-XSRF-Token über einen SSL-gesicherten Kanal übermittelt werden müssen. Wenn dieser Wert *true*ist, wird für alle automatisch generierten Cookies das Flag "Secure" festgelegt, und die Anti-XSRF-APIs lösen Sie aus, wenn Sie aus einer Anforderung aufgerufen werden, die nicht über SSL übermittelt wird. Der Standardwert ist *FALSE*. |
| **Suppressidentityheuristicchecks** | Ein boolescher Wert, der festlegt, ob das Anti-XSRF-System seine Unterstützung für Anspruchs basierte Identitäten deaktivieren soll. Wenn dieser Wert *true*ist, geht das System davon aus, dass *IIdentity.Name* für die Verwendung als eindeutiger Benutzer spezifischer Bezeichner geeignet ist, und versucht nicht, wie in WIF/ACS/beschrieben eine sonderschreibung für " *IClaimsIdentity* " oder " *clclaimsidentity* " zu verwenden. [ Abschnitt zur Anspruchs basierten Authentifizierung](#_WIF_ACS) . Der Standardwert ist `false`sein. |
| **Uniqueclaimtypeidentifier** | Eine Zeichenfolge, die angibt, welcher Anspruchstyp für die Verwendung als eindeutiger Bezeichner pro Benutzer geeignet ist. Wenn dieser Wert festgelegt wird und die aktuelle *IIdentity* Anspruchs basiert ist, versucht das System, einen Anspruch mit dem durch *uniqueclaimtypeidentifier*angegebenen Typ zu extrahieren, und der entsprechende Wert wird anstelle des Benutzernamens des Benutzers verwendet, wenn das Feld Token wird erzeugt. Wenn der Anspruchstyp nicht gefunden wird, wird die Anforderung vom System nicht bestanden. Der Standardwert ist *null*. Dies bedeutet, dass das System das Tupel (Identitäts Anbieter, namens Bezeichner) wie zuvor beschrieben anstelle des Benutzernamens des Benutzers verwenden soll. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>Iantiforgeryadditionaldataprovider

Der *[iantiforgeryadditionaldataprovider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* -Typ ermöglicht es Entwicklern, das Verhalten des Anti-XSRF-Systems zu erweitern, indem zusätzliche Daten in jedem Token abgerundet werden. Die *getadditionaldata* -Methode wird jedes Mal aufgerufen, wenn ein Feld Token generiert wird, und der Rückgabewert wird in das generierte Token eingebettet. Ein Implementierer könnte einen Zeitstempel, eine Nonce oder einen beliebigen anderen Wert zurückgeben, den Sie von dieser Methode wünscht.

Entsprechend wird die *validateadditionaldata* -Methode jedes Mal aufgerufen, wenn ein Feld Token überprüft wird, und die "zusätzliche Daten"-Zeichenfolge, die in das Token eingebettet wurde, wird an die-Methode weitergegeben. Die Validierungs Routine kann ein Timeout implementieren (durch Überprüfen der aktuellen Zeit mit dem Zeitpunkt, an dem das Token erstellt wurde), eine Nonce-Überprüfungs Routine oder eine beliebige andere gewünschte Logik.

## <a name="design-decisions-and-security-considerations"></a>Entwurfsentscheidungen und Sicherheitsüberlegungen

Das Sicherheits Token, das die Sitzungs-und Feld Token verknüpft, ist technisch nur erforderlich, wenn Sie versuchen, anonyme/nicht authentifizierte Benutzer vor XSRF-Angriffen zu schützen. Wenn der Benutzer authentifiziert ist, kann das Authentifizierungs Token selbst (vermutlich in Form eines Cookies gesendet) als eine Hälfte eines Synchronisierungs Token-Paars verwendet werden. Es gibt jedoch gültige Szenarios für den Schutz von Anmelde Seiten, die von nicht authentifizierten Benutzern betroffen sind, und die Anti-XSRF-Logik wurde vereinfacht, indem das Sicherheits Token stets erstellt und überprüft wurde, auch für authentifizierte Benutzer. Außerdem bietet es zusätzlichen Schutz für den Fall, dass ein Feld Token jemals von einem Angreifer kompromittiert wird. das Festlegen oder erraten des Sitzungs Tokens wäre eine andere Hürde, die der Angreifer überwinden könnte.

Entwickler sollten Vorsicht walten lassen, wenn mehrere Anwendungen in einer einzelnen Domäne gehostet werden. Obwohl es sich bei *example1.cloudapp.net* und *example2.cloudapp.net* um unterschiedliche Hosts handelt, besteht beispielsweise eine implizite Vertrauensstellung zwischen allen Hosts in der *\*. cloudapp.net* -Domäne. Diese implizite Vertrauensstellung [ermöglicht möglicherweise nicht vertrauenswürdigen Hosts, sich gegenseitig zu beeinflussen](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (die gleichen Ursprungs Richtlinien, die AJAX-Anforderungen Regeln, werden nicht notwendigerweise auf http-Cookies angewendet). Die ASP.net-webstack-Laufzeit stellt eine gewisse Entschärfung dar, da der Benutzername in das Feld Token eingebettet ist, sodass auch dann, wenn eine böswillige Unterdomäne ein Sitzungs Token überschreiben kann, kein gültiges Feld Token für den Benutzer generiert werden kann. Beim Hosten in einer solchen Umgebung können die integrierten Anti-XSRF-Routinen jedoch immer noch nicht gegen Session Hijacking oder Login-XSRF schützen.

Die Anti-XSRF-Routinen haben derzeit keine Schutz vor [Clickjacking](https://www.owasp.org/index.php/Clickjacking). Anwendungen, die sich gegen Clickjacking schützen möchten, können dies problemlos erreichen, indem Sie einen X-Frame-Options: sameorigin-Header mit jeder Antwort senden. Dieser Header wird von allen neueren Browsern unterstützt. Weitere Informationen finden Sie im [IE-Blog](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), im [SDL-Blog](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)und im [OWASP](https://www.owasp.org/index.php/Clickjacking). Die ASP.net-Web Stack-Laufzeit kann in einer zukünftigen Version die MVC-und Web Pages-Anti-XSRF-Hilfsprogramme automatisch festlegen, sodass Anwendungen automatisch gegen diesen Angriff geschützt werden.

Webentwickler sollten weiterhin sicherstellen, dass Ihre Website nicht anfällig für XSS-Angriffe ist. XSS-Angriffe sind sehr leistungsstark, und eine erfolgreiche Ausnutzung würde auch die ASP.net-Web Stack-Lauf Zeit Verteidigung gegen XSRF-Angriffe unterbrechen.

## <a name="acknowledgment"></a>Bestätigung

[@LeviBroderick](https://twitter.com/LeviBroderick), die einen Großteil der ASP.net-Sicherheitscodes geschrieben hat, den Großteil dieser Informationen.
