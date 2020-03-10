---
uid: signalr/overview/security/introduction-to-security
title: Einführung in die signalr-Sicherheit | Microsoft-Dokumentation
author: bradygaster
description: Beschreibt die Sicherheitsprobleme, die beim Entwickeln einer signalr-Anwendung berücksichtigt werden müssen.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 24ce20b45543468de28ad017ba62d2f6e5a00f3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450075"
---
# <a name="introduction-to-signalr-security"></a>Einführung in die Sicherheit von SignalR-Anwendungen

von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Artikel werden die Sicherheitsprobleme beschrieben, die beim Entwickeln einer signalr-Anwendung berücksichtigt werden müssen.
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

Dieses Dokument enthält folgende Abschnitte:

- [Sicherheitskonzepte von signalr](#concepts)

    - [Authentifizierung und Autorisierung](#authentication)
    - [Verbindungs Token](#connectiontoken)
    - [Gruppen werden beim erneuten Herstellen der Verbindung erneut beitreten](#rejoingroup)
- [So verhindert signalr die Website übergreifende Anforderungs Fälschung](#csrf)
- [Signalr-Sicherheitsempfehlungen](#recommendations)

    - [SSL-Protokoll (Secure Socket Layer)](#ssl)
    - [Verwenden Sie keine Gruppen als Sicherheitsmechanismus](#groupsecurity)
    - [Sichere Handhabung von Eingaben von Clients](#input)
    - [Abstimmen einer Änderung des Benutzer Status mit einer aktiven Verbindung](#reconcile)
    - [Automatisch generierte JavaScript-Proxy Dateien](#autogen)
    - [Ausnahmen](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Sicherheitskonzepte von signalr

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Signalr stellt keine Features zum Authentifizieren von Benutzern bereit. Stattdessen integrieren Sie die signalr-Funktionen in die vorhandene Authentifizierungs Struktur für eine Anwendung. Sie authentifizieren Benutzer wie gewohnt in der Anwendung und arbeiten mit den Ergebnissen der Authentifizierung in Ihrem signalr-Code. Beispielsweise können Sie Ihre Benutzer mit der ASP.NET-Formular Authentifizierung authentifizieren und dann in ihrem Hub erzwingen, welche Benutzer oder Rollen autorisiert sind, eine Methode aufzurufen. In ihrem Hub können Sie auch Authentifizierungsinformationen, wie z. b. den Benutzernamen oder den Benutzer, der zu einer Rolle gehört, an den Client übergeben.

Signalr stellt das [Autorisierungs](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut bereit, um anzugeben, welche Benutzer Zugriff auf einen Hub oder eine Methode haben. Sie wenden das Attribut autorisieren entweder auf einen Hub oder bestimmte Methoden in einem Hub an. Ohne das Attribut "autorisieren" sind alle öffentlichen Methoden auf dem Hub für einen Client verfügbar, der mit dem Hub verbunden ist. Weitere Informationen zu Hubs finden Sie unter [Authentifizierung und Autorisierung für signalr-Hubs](hub-authorization.md).

Sie wenden das `Authorize`-Attribut auf Hubs, jedoch nicht auf persistente Verbindungen an. Zum Erzwingen von Autorisierungs Regeln bei Verwendung eines `PersistentConnection` müssen Sie die `AuthorizeRequest`-Methode überschreiben. Weitere Informationen zu permanenten Verbindungen finden Sie unter [Authentifizierung und Autorisierung für permanente signalr-Verbindungen](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Verbindungs Token

Signalr verringert das Risiko, dass schädliche Befehle ausgeführt werden, indem die Identität des Absenders überprüft wird. Der Client und der Server übergeben für jede Anforderung ein Verbindungs Token, das die Verbindungs-ID und den Benutzernamen für authentifizierte Benutzer enthält. Die Verbindungs-ID identifiziert jeden verbundenen Client eindeutig. Der Server generiert nach dem Zufallsprinzip die Verbindungs-ID, wenn eine neue Verbindung erstellt wird, und behält diese ID für die Dauer der Verbindung bei. Der Authentifizierungsmechanismus für die Webanwendung stellt den Benutzernamen bereit. Signalr verwendet Verschlüsselung und eine digitale Signatur, um das Verbindungs Token zu schützen.

![](introduction-to-security/_static/image2.png)

Für jede Anforderung überprüft der Server den Inhalt des Tokens, um sicherzustellen, dass die Anforderung vom angegebenen Benutzer stammt. Der Benutzername muss der Verbindungs-ID entsprechen. Durch die Überprüfung der Verbindungs-ID und des Benutzernamens verhindert signalr, dass ein böswilliger Benutzer leicht die Identität eines anderen Benutzers annimmt. Wenn der Server das Verbindungs Token nicht validieren kann, schlägt die Anforderung fehl.

![](introduction-to-security/_static/image4.png)

Da die Verbindungs-ID Teil des Überprüfungs Vorgangs ist, sollten Sie die Verbindungs-ID eines Benutzers nicht für andere Benutzer offenlegen oder den Wert auf dem Client (z. b. in einem Cookie) speichern.

#### <a name="connection-tokens-vs-other-token-types"></a>Verbindungs Token und andere Tokentypen

Verbindungs Token werden von Sicherheitstools gelegentlich gekennzeichnet, da es sich um Sitzungs Token oder Authentifizierungs Token handelt, die ein Risiko darstellen, wenn Sie verfügbar gemacht werden.

Das Verbindungs Token von signalr ist kein Authentifizierungs Token. Es wird verwendet, um zu bestätigen, dass der Benutzer, der diese Anforderung vornimmt, derselbe ist, der die Verbindung erstellt hat. Das Verbindungs Token ist erforderlich, da ASP.net signalr Verbindungen zwischen Servern zulässt. Das Token ordnet die Verbindung einem bestimmten Benutzer zu, bestätigt aber nicht die Identität des Benutzers, der die Anforderung sendet. Damit eine signalr-Anforderung ordnungsgemäß authentifiziert werden kann, muss Sie über ein anderes Token verfügen, das die Identität des Benutzers bestätigt, z. b. ein Cookie oder ein bearertoken. Das Verbindungs Token selbst erhebt jedoch keinen Anspruch darauf, dass die Anforderung von diesem Benutzer hergestellt wurde, sondern nur, dass die im Token enthaltene Verbindungs-ID diesem Benutzer zugeordnet ist.

Da das Verbindungs Token keinen eigenen Authentifizierungs Anspruch bereitstellt, gilt es nicht als Sitzungs-oder Authentifizierungs Token. Wenn Sie das Verbindungs Token eines bestimmten Benutzers übernehmen und in einer Anforderung wiedergeben, die als ein anderer Benutzer (oder eine nicht authentifizierte Anforderung) authentifiziert ist, tritt ein Fehler auf, da die Benutzeridentität der Anforderung und der Identität, die im Token gespeichert ist, nicht entspricht.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Gruppen werden beim erneuten Herstellen der Verbindung erneut beitreten

Standardmäßig wird ein Benutzer von der signalr-Anwendung automatisch den entsprechenden Gruppen zugewiesen, wenn eine Verbindung mit einer vorübergehenden Unterbrechung wieder hergestellt wird, z. b. Wenn eine Verbindung getrennt und wieder hergestellt wird, bevor ein Timeout für die Verbindung auftritt. Beim erneuten Herstellen der Verbindung übergibt der Client ein Gruppen Token, das die Verbindungs-ID und die zugewiesenen Gruppen enthält. Das Gruppen Token ist digital signiert und verschlüsselt. Der Client behält die gleiche Verbindungs-ID nach einer erneuten Verbindung bei. Daher muss die von dem Wiederherstellen des Clients übergebenen Verbindungs-ID mit der vorherigen Verbindungs-ID, die vom Client verwendet wird, identisch sein. Durch diese Überprüfung wird verhindert, dass ein böswilliger Benutzer bei erneutem Verbindungsaufbau Anforderungen an nicht autorisierte Gruppen übergibt.

Es ist jedoch wichtig zu beachten, dass das Gruppen Token nicht abläuft. Wenn ein Benutzer zu einer Gruppe in der Vergangenheit gehört, aber von dieser Gruppe gesperrt wurde, kann dieser Benutzer ein Gruppen Token imitieren, das die unzulässige Gruppe enthält. Wenn Sie sicher verwalten müssen, welche Benutzer zu welchen Gruppen gehören, müssen Sie diese Daten auf dem Server speichern, z. b. in einer Datenbank. Fügen Sie dann der Anwendung Logik hinzu, mit der auf dem Server überprüft wird, ob ein Benutzer zu einer Gruppe gehört. Ein Beispiel für die Überprüfung der Gruppenmitgliedschaft finden Sie unter [Arbeiten mit Gruppen](../guide-to-the-api/working-with-groups.md).

Das automatische erneute Verbinden von Gruppen gilt nur, wenn eine Verbindung nach einer vorübergehenden Unterbrechung wieder hergestellt wird. Wenn ein Benutzer die Verbindung trennt, indem er von der Anwendung navigiert oder die Anwendung neu gestartet wird, muss die Anwendung behandeln, wie dieser Benutzer den richtigen Gruppen hinzugefügt wird. Weitere Informationen finden Sie unter [Arbeiten mit Gruppen](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>So verhindert signalr die Website übergreifende Anforderungs Fälschung

Website übergreifende Anforderungs Fälschung (Cross-Site Request Fälschung, CSRF) ist ein Angriff, bei dem ein böswilliger Standort eine Anforderung an einen anfälligen Standort sendet, an dem der Benutzer zurzeit angemeldet ist Signalr verhindert csrf, da es für eine böswillige Website äußerst unwahrscheinlich ist, dass eine gültige Anforderung für Ihre signalr-Anwendung erstellt wird.

### <a name="description-of-csrf-attack"></a>Beschreibung von CSRF-Angriffen

Im folgenden finden Sie ein Beispiel für einen CSRF-Angriff:

1. Ein Benutzer meldet sich mit der Formular Authentifizierung bei www.example.com an.
2. Der-Server authentifiziert den Benutzer. Die Antwort vom Server enthält ein Authentifizierungs Cookie.
3. Ohne Abmeldung greift der Benutzer auf eine böswillige Website zu. Diese böswillige Site enthält das folgende HTML-Formular:

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Beachten Sie, dass das Formular Action an die anfällige Site und nicht an die böswillige Site sendet. Dies ist der "Cross-Site"-Teil von CSRF.
4. Der Benutzer klickt auf die Schaltfläche Senden. Der Browser enthält das Authentifizierungs Cookie mit der Anforderung.
5. Die Anforderung wird auf dem example.com-Server mit dem Authentifizierungs Kontext des Benutzers ausgeführt und kann alle Aktionen ausführen, die ein authentifizierter Benutzer ausführen darf.

Obwohl es für dieses Beispiel erforderlich ist, dass der Benutzer auf die Schaltfläche "Formular" klickt, kann die bösartige Seite genauso einfach ein Skript ausführen, das eine AJAX-Anforderung an die signalr-Anwendung sendet. Außerdem wird durch die Verwendung von SSL kein CSRF-Angriff verhindert, da die böswillige Website eine "https://"-Anforderung senden kann.

In der Regel sind CSRF-Angriffe auf Websites möglich, die Cookies für die Authentifizierung verwenden, da Browser alle relevanten Cookies an die Zielwebsite senden. Allerdings sind CSRF-Angriffe nicht auf das Ausnutzen von Cookies beschränkt. Beispielsweise sind die Standard-und Digestauthentifizierung auch anfällig. Nachdem sich ein Benutzer mit der Standard-oder Digestauthentifizierung anmeldet, sendet der Browser die Anmelde Informationen automatisch, bis die Sitzung beendet wird.

### <a name="csrf-mitigations-taken-by-signalr"></a>Von signalr abgenommene CSRF-entschärfungen

Signalr führt die folgenden Schritte aus, um zu verhindern, dass eine böswillige Site gültige Anforderungen an Ihre Anwendung erstellt. Diese Schritte werden von signalr standardmäßig ausgeführt. Sie müssen keine Aktionen in Ihrem Code durchführen.

- **Domänen übergreifende Anforderungen deaktivieren** Signalr deaktiviert Domänen übergreifende Anforderungen, um zu verhindern, dass Benutzer einen signalr-Endpunkt von einer externen Domäne aus aufrufen. Signalr betrachtet jede Anforderung von einer externen Domäne als ungültig und blockiert die Anforderung. Es wird empfohlen, dass Sie dieses Standardverhalten beibehalten. Andernfalls könnte ein böswilliger Standort Benutzer dazu verleiten, Befehle an Ihre Website zu senden. Wenn Sie Domänen übergreifende Anforderungen verwenden müssen, finden Sie weitere Informationen unter [Einrichten einer Domänen übergreifenden Verbindung](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Verbindungs Token in Abfrage Zeichenfolge übergeben, nicht als Cookie** Signalr übergibt das Verbindungs Token als Abfrage Zeichen folgen Wert anstelle eines Cookies. Das Speichern des Verbindungs Tokens in einem Cookie ist unsicher, da der Browser das Verbindungs Token versehentlich weiterleiten kann, wenn bösartiger Code auftritt. Außerdem verhindert das Übergeben des Verbindungs Tokens in der Abfrage Zeichenfolge, dass das Verbindungs Token nicht länger als die aktuelle Verbindung beibehalten wird. Daher kann ein böswilliger Benutzer eine Anforderung nicht unter den Authentifizierungs Anmelde Informationen eines anderen Benutzers tätigen.
- **Verbindungs Token überprüfen** Wie im Abschnitt " [Verbindungs Token](#connectiontoken) " beschrieben, weiß der Server, welche Verbindungs-ID den einzelnen authentifizierten Benutzern zugeordnet ist. Der Server verarbeitet keine Anforderung von einer Verbindungs-ID, die nicht mit dem Benutzernamen identisch ist. Es ist unwahrscheinlich, dass ein böswilliger Benutzer eine gültige Anforderung erraten könnte, da der böswillige Benutzer den Benutzernamen und die aktuelle zufällig generierte Verbindungs-ID kennen muss. Diese Verbindungs-ID wird ungültig, sobald die Verbindung beendet wird. Anonyme Benutzer sollten keinen Zugriff auf vertrauliche Informationen haben.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Signalr-Sicherheitsempfehlungen

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>SSL-Protokoll (Secure Socket Layer)

Das SSL-Protokoll verwendet Verschlüsselung, um den Datentransport zwischen einem Client und einem Server zu sichern. Wenn die signalr-Anwendung vertrauliche Informationen zwischen Client und Server überträgt, verwenden Sie SSL für den Transport. Weitere Informationen zum Einrichten von SSL finden Sie unter [Einrichten von SSL auf IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Verwenden Sie keine Gruppen als Sicherheitsmechanismus

Gruppen sind eine bequeme Möglichkeit, Verwandte Benutzer zu erfassen, sind aber kein sicherer Mechanismus zum Einschränken des Zugriffs auf vertrauliche Informationen. Dies trifft vor allem dann zu, wenn Benutzer bei einer erneuten Verbindungs Herstellung automatisch Gruppen beitreten können. Fügen Sie stattdessen privilegierte Benutzer zu einer Rolle hinzu, und beschränken Sie den Zugriff auf eine Hub-Methode nur auf Mitglieder dieser Rolle. Ein Beispiel für das Einschränken des Zugriffs basierend auf einer Rolle finden Sie unter [Authentifizierung und Autorisierung für signalr-Hubs](hub-authorization.md). Ein Beispiel für das Überprüfen des Benutzer Zugriffs auf Gruppen beim erneuten Herstellen der Verbindung finden Sie unter [Arbeiten mit Gruppen](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Sichere Handhabung von Eingaben von Clients

Um sicherzustellen, dass ein böswilliger Benutzer kein Skript an andere Benutzer sendet, müssen Sie alle Eingaben von Clients codieren, die für die Übertragung an andere Clients vorgesehen sind. Sie sollten Nachrichten auf den empfangenden Clients anstelle des Servers codieren, da die signalr-Anwendung viele verschiedene Arten von Clients aufweisen kann. Daher funktioniert die HTML-Codierung für einen Webclient, jedoch nicht für andere Arten von Clients. Beispielsweise würde eine Webclient Methode, mit der eine Chat Nachricht angezeigt wird, den Benutzernamen und die Nachricht sicher behandeln, indem Sie die `html()`-Funktion aufrufen.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Abstimmen einer Änderung des Benutzer Status mit einer aktiven Verbindung

Wenn sich der Authentifizierungs Status eines Benutzers ändert, während eine aktive Verbindung besteht, wird dem Benutzer eine Fehlermeldung angezeigt, die besagt, dass die Benutzeridentität während einer aktiven signalr-Verbindung nicht geändert werden kann. In diesem Fall sollte die Anwendung erneut eine Verbindung mit dem Server herstellen, um sicherzustellen, dass die Verbindungs-ID und der Benutzername koordiniert werden. Wenn die Anwendung beispielsweise dem Benutzer ermöglicht, sich abzumelden, während eine aktive Verbindung besteht, entspricht der Benutzername für die Verbindung nicht mehr dem Namen, der für die nächste Anforderung weitergegeben wurde. Sie müssen die Verbindung vor dem Abmelden des Benutzers abbrechen und dann neu starten.

Es ist jedoch wichtig zu beachten, dass die meisten Anwendungen die Verbindung nicht manuell beendet und gestartet werden müssen. Wenn Ihre Anwendung Benutzer nach der Abmeldung an eine separate Seite umleitet (z. b. das Standardverhalten in einer Web Forms Anwendung oder MVC-Anwendung) oder die aktuelle Seite nach der Abmeldung aktualisiert, wird die aktive Verbindung automatisch getrennt. erfordert zusätzliche Aktionen.

Das folgende Beispiel zeigt, wie Sie eine Verbindung beenden und starten, wenn sich der Benutzer Status geändert hat.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Oder der Authentifizierungs Status des Benutzers kann sich ändern, wenn Ihre Website den gleitenden Ablauf mit der Formular Authentifizierung verwendet und keine Aktivität vorhanden ist, mit der das Authentifizierungs Cookie gültig bleibt. In diesem Fall wird der Benutzer abgemeldet, und der Benutzername stimmt nicht mehr mit dem Benutzernamen im Verbindungs Token ab. Sie können dieses Problem beheben, indem Sie ein Skript hinzufügen, das regelmäßig eine Ressource auf dem Webserver anfordert, damit das Authentifizierungs Cookie gültig bleibt. Im folgenden Beispiel wird gezeigt, wie Sie alle 30 Minuten eine Ressource anfordern.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Automatisch generierte JavaScript-Proxy Dateien

Wenn Sie nicht alle Hubs und Methoden in die JavaScript-Proxy Datei für jeden Benutzer einschließen möchten, können Sie die automatische Generierung der Datei deaktivieren. Sie können diese Option wählen, wenn Sie über mehrere Hubs und Methoden verfügen, aber nicht möchten, dass jeder Benutzer alle Methoden kennt. Sie deaktivieren die automatische Generierung, indem Sie **enablejavascriptproxies** auf **false**festlegen.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Weitere Informationen zu den JavaScript-Proxy Dateien finden [Sie unter der generierte Proxy und dessen](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)Funktionsweise. <a id="exceptions"></a>

### <a name="exceptions"></a>Ausnahmen

Vermeiden Sie das Übergeben von Ausnahme Objekten an Clients, da die Objekte möglicherweise vertrauliche Informationen für die Clients verfügbar machen. Stattdessen wird eine Methode auf dem Client aufgerufen, die die relevante Fehlermeldung anzeigt.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
