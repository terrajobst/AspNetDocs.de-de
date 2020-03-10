---
uid: web-api/overview/security/authentication-filters
title: Authentifizierungs Filter in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Ein Authentifizierungs Filter ist eine Komponente, die eine HTTP-Anforderung authentifiziert. Die Web-API 2 und MVC 5 unterstützen Authentifizierungs Filter, unterscheiden sich jedoch geringfügig...
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 2ef9e62a6c634237e920b6d7aba2127b835f959d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447771"
---
# <a name="authentication-filters-in-aspnet-web-api-2"></a>Authentifizierungs Filter in ASP.net-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

> Ein Authentifizierungs Filter ist eine Komponente, die eine HTTP-Anforderung authentifiziert. Die Web-API 2 und MVC 5 unterstützen Authentifizierungs Filter. Sie unterscheiden sich jedoch geringfügig, größtenteils in den Benennungs Konventionen für die Filter Schnittstelle. In diesem Thema werden Web-API-Authentifizierungs Filter beschrieben.

Mit Authentifizierungs filtern können Sie ein Authentifizierungsschema für einzelne Controller oder Aktionen festlegen. Auf diese Weise kann Ihre APP verschiedene Authentifizierungsmechanismen für verschiedene HTTP-Ressourcen unterstützen.

In diesem Artikel zeige ich Code aus dem Beispiel zur Standard [Authentifizierung](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) auf [https://github.com/aspnet/samples](https://github.com/aspnet/samples). Das Beispiel zeigt einen Authentifizierungs Filter, der das HTTP-Basis Zugriffs Authentifizierungsschema (RFC 2617) implementiert. Der Filter wird in einer Klasse mit dem Namen `IdentityBasicAuthenticationAttribute`implementiert. Ich zeige nicht den gesamten Code aus dem Beispiel, sondern nur die Teile, die veranschaulichen, wie ein Authentifizierungs Filter geschrieben wird.

## <a name="setting-an-authentication-filter"></a>Festlegen eines Authentifizierungs Filters

Wie bei anderen Filtern können Authentifizierungs Filter pro Controller, pro Aktion oder global auf alle Web-API-Controller angewendet werden.

Zum Anwenden eines Authentifizierungs Filters auf einen Controller müssen Sie die Controller Klasse mit dem Filter-Attribut ergänzen. Mit dem folgenden Code wird der `[IdentityBasicAuthentication]` Filter für eine Controller Klasse festgelegt, die die Standard Authentifizierung für alle Aktionen des Controllers ermöglicht.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Um den Filter auf eine Aktion anzuwenden, ergänzen Sie die Aktion mit dem Filter. Der folgende Code legt den `[IdentityBasicAuthentication]` Filter für die `Post`-Methode des Controllers fest.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Wenn Sie den Filter auf alle Web-API-Controller anwenden möchten, fügen Sie ihn zu " **globalconfiguration. Filters**" hinzu.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementieren eines Web-API-Authentifizierungs Filters

In der Web-API implementieren Authentifizierungs Filter die [System. Web. http. Filters. iauthenticationfilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) -Schnittstelle. Sie sollten auch von " **System. Attribute**" erben, damit Sie als Attribute angewendet werden können.

Die **iauthenticationfilter** -Schnittstelle verfügt über zwei Methoden:

- **Authenti-easync** authentifiziert die Anforderung, indem die Anmelde Informationen in der Anforderung überprüft werden, falls vorhanden.
- " **Herausforderer** " fügt bei Bedarf der HTTP-Antwort eine Authentifizierungs Aufforderung hinzu.

Diese Methoden entsprechen dem Authentifizierungs Fluss, der in [RFC 2612](http://tools.ietf.org/html/rfc2616) und [RFC 2617](http://tools.ietf.org/html/rfc2617)definiert ist:

1. Der Client sendet Anmelde Informationen im Autorisierungs Header. Dies geschieht normalerweise, nachdem der Client eine Antwort vom Typ 401 (nicht autorisiert) vom Server empfangen hat. Ein Client kann jedoch Anmelde Informationen mit einer beliebigen Anforderung senden, nicht nur nach dem erhalten eines 401.
2. Wenn der Server die Anmelde Informationen nicht akzeptiert, wird die Antwort 401 (nicht autorisiert) zurückgegeben. Die Antwort enthält einen WWW-Authenticate-Header, der eine oder mehrere Herausforderungen enthält. Jede Abfrage gibt ein Authentifizierungsschema an, das vom Server erkannt wird.

Der Server kann 401 auch aus einer anonymen Anforderung zurückgeben. Tatsächlich wird der Authentifizierungsprozess in der Regel initiiert:

1. Der Client sendet eine anonyme Anforderung.
2. Der Server gibt 401 zurück.
3. Der Client sendet die Anforderung mit den Anmelde Informationen erneut.

Dieser Flow umfasst sowohl *Authentifizierungs* -als auch *Autorisierungs* Schritte.

- Bei der Authentifizierung wird die Identität des Clients bestätigt.
- Die Autorisierung bestimmt, ob der Client auf eine bestimmte Ressource zugreifen kann.

In der Web-API behandeln Authentifizierungs Filter die Authentifizierung, aber nicht die Autorisierung. Die Autorisierung sollte durch einen Autorisierungs Filter oder innerhalb der Controller Aktion durchgeführt werden.

Dies ist der Flow in der Web-API 2-Pipeline:

1. Bevor eine Aktion aufgerufen wird, erstellt die Web-API eine Liste der Authentifizierungs Filter für diese Aktion. Dies schließt Filter mit Aktionsbereich, Controllerbereich und globalem Gültigkeitsbereich ein.
2. Die Web-API ruft bei jedem Filter in der Liste **Authenti-easync** auf. Jeder Filter kann die Anmelde Informationen in der Anforderung überprüfen. Wenn ein Filter erfolgreich Anmelde Informationen überprüft, erstellt der Filter einen **IPrincipal** und fügt ihn an die Anforderung an. Ein Filter kann an dieser Stelle auch einen Fehler auslöst. Wenn dies der Fall ist, wird der Rest der Pipeline nicht ausgeführt.
3. Wenn kein Fehler auftritt, fließt die Anforderung durch den Rest der Pipeline.
4. **Zum Schluss** Ruft die Web-API die Methode "-Methode" für jeden Authentifizierungs Filter auf. Filter verwenden diese Methode, um bei Bedarf der Antwort eine Aufforderung hinzuzufügen. In der Regel (aber nicht immer), die als Reaktion auf einen 401-Fehler auftreten würden.

Die folgenden Diagramme zeigen zwei mögliche Fälle. Im ersten authentifiziert der Authentifizierungs Filter die Anforderung erfolgreich, ein Autorisierungs Filter autorisiert die Anforderung, und die Controller Aktion gibt 200 (OK) zurück.

![](authentication-filters/_static/image1.png)

Im zweiten Beispiel authentifiziert der Authentifizierungs Filter die Anforderung, aber der Autorisierungs Filter gibt 401 (nicht autorisiert) zurück. In diesem Fall wird die Controller Aktion nicht aufgerufen. Der Authentifizierungs Filter fügt der Antwort einen WWW-Authenticate-Header hinzu.

![](authentication-filters/_static/image2.png)

Andere Kombinationen sind möglich&mdash;z. b. wenn die Controller Aktion anonyme Anforderungen zulässt, verfügen Sie möglicherweise über einen Authentifizierungs Filter, aber keine Autorisierung.

## <a name="implementing-the-authenticateasync-method"></a>Implementieren der Authenti-easync-Methode

Die **authentikateasync** -Methode versucht, die Anforderung zu authentifizieren. Hier die Signatur der Methode:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

Die **Authenti-easync** -Methode muss eine der folgenden Aktionen ausführen:

1. Nothing (No-OP).
2. Erstellen Sie einen **IPrincipal** , und legen Sie ihn für die Anforderung fest.
3. Legen Sie ein Fehler Ergebnis fest.

Option (1) bedeutet, dass die Anforderung keine Anmelde Informationen enthielt, die der Filter versteht. Option (2) bedeutet, dass der Filter die Anforderung erfolgreich authentifiziert hat. Die Option (3) bedeutet, dass die Anforderung ungültige Anmelde Informationen enthielt (z. b. das falsche Kennwort), die eine Fehler Antwort auslösen.

Im folgenden finden Sie eine allgemeine Übersicht über die Implementierung von **Authenti-easync**.

1. Suchen Sie in der Anforderung nach Anmelde Informationen.
2. Wenn keine Anmelde Informationen vorhanden sind, führen Sie nichts aus, und geben Sie (No-OP) zurück.
3. Wenn Anmelde Informationen vorhanden sind, der Filter das Authentifizierungsschema aber nicht erkennt, führen Sie nichts aus, und geben Sie (No-OP) zurück. Ein anderer Filter in der Pipeline kann das Schema verstehen.
4. Wenn Anmelde Informationen vorhanden sind, die der Filter versteht, versuchen Sie, diese zu authentifizieren.
5. Wenn die Anmelde Informationen falsch sind, wird 401 zurückgegeben, indem `context.ErrorResult`festgelegt wird.
6. Wenn die Anmelde Informationen gültig sind, erstellen Sie einen **IPrincipal** , und legen Sie `context.Principal`fest.

Der folgende Code zeigt die **Authenti-easync** -Methode aus dem Beispiel zur Standard [Authentifizierung](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) . In den Kommentaren werden die einzelnen Schritte angegeben. Der Code zeigt verschiedene Arten von Fehlern: einen Autorisierungs Header ohne Anmelde Informationen, falsch formatierte Anmelde Informationen und ungültigen Benutzernamen/Kennwort.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Festlegen eines Fehler Ergebnisses

Wenn die Anmelde Informationen ungültig sind, muss der Filter `context.ErrorResult` auf ein **ihttpactionresult** festlegen, das eine Fehler Antwort erstellt. Weitere Informationen zu **ihttpactionresult**finden Sie unter [Aktions Ergebnisse in der Web-API 2](../getting-started-with-aspnet-web-api/action-results.md).

Das Beispiel zur Standard Authentifizierung enthält eine `AuthenticationFailureResult`-Klasse, die für diesen Zweck geeignet ist.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementieren von "".

Der Zweck der Methode "Delegat **Async** " besteht darin, der Antwort bei Bedarf Authentifizierungs Herausforderungen hinzuzufügen. Hier die Signatur der Methode:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Die-Methode wird für jeden Authentifizierungs Filter in der Anforderungs Pipeline aufgerufen.

Es ist wichtig zu verstehen, **dass "** " "" " " "" "" ". Wenn **"** " "" "" "" "" "" **"`context.Result`,** " "" " **Wenn Sie** also "" "" "" "" ". Die Methode " **delegalasync** " sollte den ursprünglichen Wert `context.Result` durch ein neues **ihttpactionresult**ersetzen. Dieses **ihttpactionresult** muss die ursprüngliche `context.Result`einschließen.

![](authentication-filters/_static/image3.png)

Ich rufe das ursprüngliche **ihttpactionresult** -Ergebnis im *inneren Ergebnis*und das neue **ihttpactionresult** -Ergebnis das *äußere Ergebnis*auf. Das äußere Ergebnis muss folgende Aktionen ausführen:

1. Rufen Sie das innere Ergebnis auf, um die HTTP-Antwort zu erstellen.
2. Überprüfen Sie die Antwort.
3. Fügen Sie bei Bedarf der Antwort eine Authentifizierungs Aufforderung hinzu.

Das folgende Beispiel stammt aus dem Beispiel zur Standard Authentifizierung. Dabei wird ein **ihttpactionresult** für das äußere Ergebnis definiert.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

Die `InnerResult`-Eigenschaft enthält das innere **ihttpactionresult**. Die `Challenge`-Eigenschaft stellt einen www-Authentifizierungs Header dar. Beachten Sie, dass von **ExecuteAsync** zuerst `InnerResult.ExecuteAsync` aufgerufen wird, um die HTTP-Antwort zu erstellen. Anschließend wird die Abfrage bei Bedarf hinzugefügt.

Überprüfen Sie den Antwort Code, bevor Sie die Abfrage hinzufügen. Die meisten Authentifizierungs Schemas fügen nur dann eine Herausforderung hinzu, wenn die Antwort 401 ist, wie hier gezeigt. Einige Authentifizierungs Schemas stellen jedoch eine Herausforderung für eine Erfolgs Antwort dar. Informationen hierzu finden Sie beispielsweise unter [Aushandlung](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Wenn die `AddChallengeOnUnauthorizedResult`-Klasse angegeben wird, ist der tatsächliche **Code in "** " "" Sie erstellen das Ergebnis einfach und fügen es an `context.Result`an.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Hinweis: das Beispiel zur Standard Authentifizierung abstrahiert diese Logik ein wenig, indem es in eine Erweiterungsmethode platziert wird.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Kombinieren von Authentifizierungs Filtern mit Authentifizierung auf Hostebene

"Authentifizierung auf Hostebene" ist die Authentifizierung, die vom Host (z. b. IIS) ausgeführt wird, bevor die Anforderung das Web-API-Framework erreicht.

Häufig empfiehlt es sich, die Authentifizierung auf Hostebene für den Rest der Anwendung zu aktivieren, diese aber für Ihre Web-API-Controller zu deaktivieren. Ein typisches Szenario ist beispielsweise die Aktivierung der Formular Authentifizierung auf Hostebene, aber die tokenbasierte Authentifizierung für die Web-API.

Um die Authentifizierung auf Hostebene in der Web-API-Pipeline zu deaktivieren, wenden Sie `config.SuppressHostPrincipal()` in Ihrer Konfiguration an. Dies bewirkt, dass die Web-API den **IPrincipal** aus allen Anforderungen entfernt, die in die Web-API-Pipeline eingegeben werden. Effektiv &quot;&quot; der Anforderung nicht authentifiziert werden.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.net-Web-API Sicherheitsfilter](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
