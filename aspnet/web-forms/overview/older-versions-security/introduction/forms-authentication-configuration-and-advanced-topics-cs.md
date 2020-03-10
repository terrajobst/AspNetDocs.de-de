---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Konfiguration der Formular Authentifizierung und erweiterte ThemenC#() | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden die verschiedenen Formular Authentifizierungs Einstellungen untersucht, und es wird erläutert, wie Sie mit dem Forms-Element geändert werden. Dies führt zu einem detaillierten...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: b296f31da1c73df97175d94402b4d618df425d8d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521373"
---
# <a name="forms-authentication-configuration-and-advanced-topics-c"></a>Konfiguration der Formularauthentifizierung und weiterführende Themen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> In diesem Tutorial werden die verschiedenen Formular Authentifizierungs Einstellungen untersucht, und es wird erläutert, wie Sie mit dem Forms-Element geändert werden. Dies führt zu einem detaillierten Blick darauf, wie der Timeout Wert des Formular Authentifizierungs Tickets angepasst wird, indem eine Anmeldeseite mit einer benutzerdefinierten URL (z. b. "SignIn. aspx" anstelle von "Login. aspx") und cookielose Formular Authentifizierungs Tickets verwendet werden.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](an-overview-of-forms-authentication-cs.md) haben wir die erforderlichen Schritte zum Implementieren der Formular Authentifizierung in einer ASP.NET-Anwendung erläutert, von der Angabe von Konfigurationseinstellungen in der Datei Web. config bis zum Erstellen einer Anmeldeseite zum Anzeigen verschiedener Inhalte für authentifizierte und anonyme Benutzer. Beachten Sie, dass wir die Website für die Verwendung der Formular Authentifizierung konfiguriert haben, indem Sie das Mode-Attribut des &lt;Authentication&gt;-Elements auf Forms festlegen. Das &lt;Authentication&gt;-Element kann optional ein &lt;Forms&gt; untergeordnetes Element enthalten, über das eine Reihe von Formular Authentifizierungs Einstellungen angegeben werden kann.

In diesem Tutorial werden die verschiedenen Formular Authentifizierungs Einstellungen untersucht, und es wird erläutert, wie Sie diese über das &lt;Forms-&gt; Element ändern können. Dies führt zu einem detaillierten Blick darauf, wie der Timeout Wert des Formular Authentifizierungs Tickets angepasst wird, indem eine Anmeldeseite mit einer benutzerdefinierten URL (z. b. "SignIn. aspx" anstelle von "Login. aspx") und cookielose Formular Authentifizierungs Tickets verwendet werden. Außerdem wird die Zusammensetzung des Formular Authentifizierungs Tickets genauer untersucht, und es werden die Vorsichtsmaßnahmen ASP.net, um sicherzustellen, dass die Daten des Tickets vor Prüfung und Manipulation sicher sind. Abschließend erfahren Sie, wie Sie zusätzliche Benutzerdaten im Formular Authentifizierungs Ticket speichern und wie Sie diese Daten über ein benutzerdefiniertes Prinzipal Objekt modellieren.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Schritt 1: Untersuchen der &lt;Formularen&gt; Konfigurationseinstellungen

Das Formular Authentifizierungssystem in ASP.net bietet eine Reihe von Konfigurationseinstellungen, die je nach Anwendung angepasst werden können. Dies schließt folgende Einstellungen ein: die Lebensdauer des Formular Authentifizierungs Tickets. welche Art von Schutz wird auf das Ticket angewendet? unter welchen Bedingungen werden cookilose Authentifizierungs Tickets verwendet? der Pfad zur Anmeldeseite. und weitere Informationen. Um die Standardwerte zu ändern, fügen Sie ein [&lt;Forms&gt;-Element](https://msdn.microsoft.com/library/1d3t3c61.aspx) als untergeordnetes Element des [&lt;Authentication&gt;-Elements](https://msdn.microsoft.com/library/532aee0e.aspx)hinzu, wobei Sie die Eigenschaftswerte angeben, die Sie als XML-Attribute wie folgt anpassen möchten:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

In Tabelle 1 werden die Eigenschaften zusammengefasst, die über das&gt;-Element &lt;Forms angepasst werden können. Da "Web. config" eine XML-Datei ist, wird bei den Attributnamen in der linken Spalte die Groß-/Kleinschreibung beachtet.

| <strong>Attribut</strong> |                                                                                                                                                                                                                                     <strong>Beschreibung</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         ohne Cookies         |                                                                                                                Dieses Attribut gibt an, unter welchen Bedingungen das Authentifizierungs Ticket in einem Cookie gespeichert wird und in die URL eingebettet ist. Zulässige Werte sind: UseCookies; UseUri; Auto Ermittlung und UseDeviceProfile (Standard). In Schritt 2 wird diese Einstellung ausführlicher geprüft.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Gibt die URL an, an die Benutzer nach der Anmeldung von der Anmeldeseite umgeleitet werden, wenn kein RedirectURL-Wert in der Abfrage Zeichenfolge angegeben ist. Der Standardwert ist default.aspx.                                                                                                                                                         |
|           Domäne           | Bei Verwendung von cookiebasierten Authentifizierungs Tickets gibt diese Einstellung den Domänen Wert des Cookies an. Der Standardwert ist eine leere Zeichenfolge, die bewirkt, dass der Browser die Domäne verwendet, von der er ausgestellt wurde (z. b. www.yourdomain.com). In diesem Fall wird das Cookie <strong>nicht</strong> gesendet, wenn Anforderungen an Unterdomänen, z. b. admin.yourdomain.com, gesendet werden. Wenn Sie möchten, dass das Cookie an alle Unterdomänen übermittelt wird, müssen Sie das Domänen Attribut auf yourdomain.com festlegen. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Ein boolescher Wert, der angibt, ob authentifizierte Benutzer gespeichert werden, wenn Sie an URLs in anderen Webanwendungen auf demselben Server umgeleitet werden. Die Standardeinstellung ist „false“.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Die URL der Anmeldeseite. Der Standardwert ist login.aspx.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Bei Verwendung von cookiebasierten Authentifizierungs Tickets der Name des Cookies. Der Standardwert ist. ASPXAUTH.                                                                                                                                                                                                   |
|            path            |                                                                             Bei Verwendung von cookiebasierten Authentifizierungs Tickets gibt diese Einstellung das Pfad Attribut des Cookies an. Mithilfe des Path-Attributs kann ein Entwickler den Gültigkeitsbereich eines Cookies auf eine bestimmte Verzeichnishierarchie beschränken. Der Standardwert ist/. Hiermit wird der Browser angewiesen, das Authentifizierungs Ticket Cookie an eine beliebige Anforderung an die Domäne zu senden.                                                                              |
|         Schutz         |                                                                                                                                            Gibt an, welche Techniken verwendet werden, um das Formular Authentifizierungs Ticket zu schützen. Folgende Werte sind zulässig: alle (Standard); Verschlüsselungs Gar und Validierung. Diese Einstellungen werden in Schritt 3 ausführlich erläutert.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Ein boolescher Wert, der angibt, ob eine SSL-Verbindung erforderlich ist, um das Authentifizierungs Cookie zu übermitteln. Der Standardwert ist false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Ein boolescher Wert, der angibt, ob das Timeout des Authentifizierungs Cookies jedes Mal zurückgesetzt wird, wenn der Benutzer die Website während einer einzigen Sitzung besucht. Der Standardwert lautet „true“. Die Richtlinie für das Authentifizierungs Ticket-Timeout wird im Abschnitt angeben des Timeout Werts für das Ticket ausführlicher erläutert.                                                                                                 |
|          timeout           |                                                                                                                               Gibt die Zeit in Minuten an, nach der das Authentifizierungs Ticket Cookie abläuft. Der Standardwert ist 30. Die Richtlinie für das Authentifizierungs Ticket-Timeout wird im Abschnitt angeben des Timeout Werts für das Ticket ausführlicher erläutert.                                                                                                                               |

**Tabelle 1**: eine Zusammenfassung der Attribute des &lt;Forms-&gt; Elements

In ASP.NET 2,0 und höher sind die standardmäßigen Formular Authentifizierungs Werte in der FormsAuthenticationConfiguration-Klasse in der .NET Framework hart codiert. Alle Änderungen müssen in der Datei "Web. config" auf Anwendungs Basis angewendet werden. Dies unterscheidet sich von ASP.NET 1. x, wobei die standardmäßigen Formular Authentifizierungs Werte in der Datei Machine. config gespeichert wurden (und daher über die Bearbeitung von Machine. config geändert werden können). Im Thema ASP.NET 1. x ist es jedoch sinnvoll, zu erwähnen, dass eine Reihe von Formular Authentifizierungs Systemeinstellungen unterschiedliche Standardwerte in ASP.NET 2,0 und höher als in ASP.NET 1. x aufweisen. Wenn Sie Ihre Anwendung aus einer ASP.NET 1. x-Umgebung migrieren, ist es wichtig, dass Sie diese Unterschiede kennen. Eine Liste der Unterschiede finden Sie in [der technischen Dokumentation &lt;Forms&gt;-Elements](https://msdn.microsoft.com/library/1d3t3c61.aspx) .

> [!NOTE]
> Einige Einstellungen für die Formular Authentifizierung, z. b. Timeout, Domäne und Pfad, geben Details für das resultierende Formular Authentifizierungs Ticket-Cookie an. Weitere Informationen zu Cookies, ihrer Funktionsweise und ihren verschiedenen Eigenschaften finden Sie in [diesem Cookie-Tutorial](http://www.quirksmode.org/js/cookies.html).

### <a name="specifying-the-tickets-timeout-value"></a>Angeben des Timeout Werts für das Ticket

Das Formular Authentifizierungs Ticket ist ein Token, das eine Identität darstellt. Bei cookiebasierten Authentifizierungs Tickets wird dieses Token in Form eines Cookies gespeichert und bei jeder Anforderung an den Webserver gesendet. Der Besitz des Tokens deklariert im Grunde, ich bin *username*, ich bin bereits angemeldet und wird verwendet, damit die Identität eines Benutzers über Seitenbesuche hinweg gespeichert werden kann.

Das Formular Authentifizierungs Ticket umfasst nicht nur die Identität des Benutzers, sondern enthält auch Informationen, mit denen Sie die Integrität und Sicherheit des Tokens sicherstellen können. Schließlich möchten wir nicht, dass ein Benutzer, der keine Benutzer ist, ein gefälschtes Token erstellen oder ein legit-Token in einer anderen Weise ändern kann.

Ein solches Informations Paar im Ticket ist ein *Ablauf*Datum, bei dem es sich um das Datum und die Uhrzeit handelt, zu denen das Ticket nicht mehr gültig ist. Jedes Mal, wenn das FormsAuthenticationModule ein Authentifizierungs Ticket überprüft, wird sichergestellt, dass der Ablauf des Tickets noch nicht abgelaufen ist. Wenn dies der Fall ist, wird das Ticket ignoriert, und der Benutzer wird als anonym bezeichnet. Diese Schutzmaßnahme schützt vor Replay-Angriffen. Ohne Ablauf, wenn ein Hacker ein Benutzer gültiges Authentifizierungs Ticket erhalten konnte, indem er möglicherweise physischen Zugriff auf Ihren Computer erlangt und seine Cookies durchläuft, könnte er eine Anforderung an den Server mit diesem gestohlenen Authentifizierungs Ticket senden und gewinnen Sie einen Eintrag. Während der Ablauf dieses Szenarios nicht verhindert, wird das Fenster, in dem ein solcher Angriff erfolgreich ausgeführt werden kann, eingeschränkt.

> [!NOTE]
> In Schritt 3 werden zusätzliche Verfahren erläutert, die vom Formular Authentifizierungssystem zum Schutz des Authentifizierungs Tickets verwendet werden.

Beim Erstellen des Authentifizierungs Tickets bestimmt das Formular Authentifizierungssystem seinen Ablauf, indem er die Timeout Einstellung einrichtet. Wie in Tabelle 1 erwähnt, wird die Timeout Einstellung standardmäßig auf 30 Minuten festgelegt. Dies bedeutet, dass beim Erstellen des Formular Authentifizierungs Tickets der Ablauf in der Zukunft auf ein Datum und eine Uhrzeit von 30 Minuten festgelegt wird.

Der Ablauf definiert eine absolute Zeit in der Zukunft, wenn das Formular Authentifizierungs Ticket abläuft. Doch in der Regel möchten Entwickler eine gleitende Ablaufzeit implementieren, die bei jedem erneuten Besuchen der Website zurückgesetzt wird. Dieses Verhalten wird durch die slidingExpiration-Einstellungen festgelegt. Wenn der Wert auf true (Standardeinstellung) festgelegt ist, wird jedes Mal, wenn das FormsAuthenticationModule einen Benutzer authentifiziert, der Ablauf des Tickets aktualisiert. Wenn der Wert auf false festgelegt ist, wird der Ablauf bei jeder Anforderung nicht aktualisiert, was dazu führt, dass das Ticket genau in Minuten nach der ersten Erstellung des Tickets abläuft.

> [!NOTE]
> Der im Authentifizierungs Ticket gespeicherte Ablauf ist ein absoluter Datums-und Uhrzeitwert, wie z. b. den 2. August, 2008 11:34 Uhr. Außerdem sind Datum und Uhrzeit relativ zur lokalen Zeit des Webservers. Diese Entwurfs Entscheidung kann einige interessante Nebeneffekte in Bezug auf die Sommerzeit (Sommerzeit, DST) aufweisen, bei der Uhren im USA eine Stunde nach vorn verschoben werden (vorausgesetzt, der Webserver wird in einem Gebiets Schema gehostet, in dem die Sommerzeit beobachtet wird). Sehen Sie sich an, was für eine ASP.NET-Website mit einem Ablauf von 30 Minuten in der Zeit passiert, in der der DST beginnt (2:00 Uhr). Stellen Sie sich vor, dass sich ein Besucher am 11. März 2008 um 1:55 Uhr bei der Website anmeldet. Dadurch wird ein Formular Authentifizierungs Ticket generiert, das am 11. März 2008 um 2:25 Uhr (in der Zukunft 30 Minuten) abläuft. Wenn jedoch 2:00 ein Rollback durchführt, springt die Uhr aufgrund von DST auf 3:00 Uhr. Wenn der Benutzer eine neue Seite sechs Minuten nach der Anmeldung lädt (um 3:01 Uhr), stellt das FormsAuthenticationModule fest, dass das Ticket abgelaufen ist, und leitet den Benutzer zur Anmeldeseite um. Eine ausführlichere Erläuterung zu diesem und anderen Authentifizierungs Ticket-Timeout-Oddities sowie Problem Umgehungen erhalten Sie, wenn Sie eine Kopie von Stefan Schackow *Professional ASP.NET 2,0 Security, Membership und Role Management* (ISBN: 978-0-7645-9698-8) übernehmen.

Abbildung 1 veranschaulicht den Workflow, wenn slidingExpiration auf false festgelegt ist und Timeout auf 30 festgelegt ist. Beachten Sie, dass das bei der Anmeldung generierte Authentifizierungs Ticket das Ablaufdatum enthält. dieser Wert wird bei nachfolgenden Anforderungen nicht aktualisiert. Wenn das FormsAuthenticationModule feststellt, dass das Ticket abgelaufen ist, wird es verworfen und die Anforderung als anonym behandelt.

[![eine grafische Darstellung des Ablaufs des Formular Authentifizierungs Tickets, wenn slidingExpiration den Wert false hat.](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Abbildung 01**: eine grafische Darstellung des Ablaufs des Formular Authentifizierungs Tickets, wenn "slidingExpiration" den Wert "false" hat ([Klicken Sie, um das Bild in voller Größe anzuzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))

Abbildung 2 zeigt den Workflow, wenn slidingExpiration auf true festgelegt ist und Timeout auf 30 festgelegt ist. Wenn eine authentifizierte Anforderung empfangen wird (mit einem nicht abgelaufenen Ticket), wird der Ablauf in der Zukunft auf die Timeout Anzahl von Minuten aktualisiert.

[![eine grafische Darstellung der Formular Authentifizierungs Tickets, wenn "slidingExpiration" den Wert "true" hat.](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Abbildung 02**: eine grafische Darstellung der Formular Authentifizierungs Tickets, wenn slidingExpiration true ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))

Bei der Verwendung von cookiebasierten Authentifizierungs Tickets (Standardeinstellung) wird diese Erörterung etwas verwirrend, da für Cookies auch eigene Expiries angegeben werden können. Der Ablauf eines Cookies weist den Browser an, wenn das Cookie zerstört werden soll. Wenn das Cookie keinen Ablauf hat, wird es zerstört, wenn der Browser heruntergefahren wird. Wenn eine Ablaufzeit vorhanden ist, bleibt das Cookie jedoch so lange auf dem Computer des Benutzers gespeichert, bis das Datum und die Uhrzeit des Ablaufs abgelaufen sind. Wenn ein Cookie vom Browser zerstört wird, wird es nicht mehr an den Webserver gesendet. Daher ist die Zerstörung eines Cookies analog zum Benutzer, der sich von der Website abgemeldet hat.

> [!NOTE]
> Natürlich kann ein Benutzer proaktiv alle Cookies entfernen, die auf dem Computer gespeichert sind. In Internet Explorer 7 wechseln Sie zu Extras, Optionen, und klicken Sie im Abschnitt Browserverlauf auf die Schaltfläche Löschen. Klicken Sie dort auf die Schaltfläche Cookies löschen.

Das Formular Authentifizierungssystem erstellt Sitzungs basierte oder Ablauf basierte Cookies in Abhängigkeit von dem Wert, der an den *persistcookie* -Parameter übergeben wird. Beachten Sie, dass die Methoden GetAuthCookie, SetAuthCookie und RedirectFromLoginPage der FormsAuthentication-Klasse zwei Eingabeparameter akzeptieren: *username* und *persistcookie*. Die Anmeldeseite, die wir im vorherigen Tutorial erstellt haben, enthielt das Kontrollkästchen "Erinnerung", mit dem bestimmt wird, ob ein dauerhaftes Cookie erstellt wurde. Persistente Cookies sind Ablauf basiert. nicht persistente Cookies sind Sitzungs basiert.

Das Timeout und die slidingExpiration-Konzepte, die bereits erläutert wurden, gelten auch für Sitzungs basierte und Ablauf basierte Cookies. Es gibt nur einen geringfügigen Unterschied bei der Ausführung: bei Verwendung von Ablauf basierten Cookies mit auf true festgelegtem slidingtimeout wird der Ablauf des Cookies nur aktualisiert, wenn mehr als die Hälfte der angegebenen Zeit abgelaufen ist.

Wir aktualisieren die Timeout Richtlinien für das Authentifizierungs Ticket unserer Website so, dass Tickets nach einer Stunde (60 Minuten) mit einem gleitenden Ablaufzeit Limit überschritten werden. Um diese Änderung zu beeinflussen, aktualisieren Sie die Datei Web. config, und fügen Sie dem &lt;Authentication&gt;-Element mit dem folgenden Markup ein &lt;Forms&gt;-Element hinzu:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Verwenden einer anderen Anmelde Seiten-URL als "Login. aspx"

Da FormsAuthenticationModule nicht autorisierte Benutzer automatisch an die Anmeldeseite umleitet, muss Sie die URL der Anmeldeseite kennen. Diese URL wird durch das loginUrl-Attribut im &lt;Forms&gt;-Element angegeben und ist standardmäßig Login. aspx. Wenn Sie über eine vorhandene Website portieren, verfügen Sie möglicherweise bereits über eine Anmeldeseite mit einer anderen URL, die bereits mit einem Lesezeichen versehen und von Suchmaschinen indiziert wurde. Anstatt Ihre vorhandene Anmeldeseite in Login. aspx und unterbrechende Links und Benutzer Lesezeichen umzubenennen, können Sie stattdessen das loginUrl-Attribut so ändern, dass es auf Ihre Anmeldeseite verweist.

Wenn die Anmeldeseite z. b. den Namen "SignIn. aspx" hat und sich im Verzeichnis "Users" befindet, können Sie die loginUrl-Konfigurationseinstellung auf ~/users/SignIn.aspx wie folgt festlegen:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Da unsere aktuelle Anwendung bereits über eine Anmeldeseite mit dem Namen Login. aspx verfügt, ist es nicht erforderlich, einen benutzerdefinierten Wert im &lt;Forms-&gt; Element anzugeben.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Schritt 2: Verwenden von Authentifizierungs Tickets für cookielose Formulare

Standardmäßig legt das Formular Authentifizierungssystem fest, ob die Authentifizierungs Tickets in der Cookies-Sammlung gespeichert oder in die URL eingebettet werden, basierend auf dem Benutzer-Agent, der die Website besucht. Alle gängigen Desktop Browser wie Internet Explorer, Firefox, Opera und Safari unterstützen Cookies, aber nicht alle mobilen Geräte.

Die vom Formular Authentifizierungssystem verwendete cookierichtlinie hängt von der Cookieeinstellung in der &lt;Forms-&gt; Element ab, der einer von vier Werten zugewiesen werden kann:

- UseCookies: gibt an, dass cookiebasierte Authentifizierungs Tickets immer verwendet werden.
- UseUri: gibt an, dass cookiebasierte Authentifizierungs Tickets nie verwendet werden.
- Automatische Erkennung: Wenn das Geräte Profil Cookies nicht unterstützt, werden cookiebasierte Authentifizierungs Tickets nicht verwendet. Wenn das Geräte Profil Cookies unterstützt, wird ein Überprüfungsmechanismus verwendet, um zu bestimmen, ob Cookies aktiviert sind.
- UseDeviceProfile-der Standardwert; verwendet cookiebasierte Authentifizierungs Tickets nur dann, wenn das Geräte Profil Cookies unterstützt. Es wird kein Überprüfungsmechanismus verwendet.

Die Einstellungen Autodetect und UseDeviceProfile basieren auf einem *Geräte Profil* bei der Ermittlung, ob cookiebasierte oder cookilose Authentifizierungs Tickets verwendet werden sollen. ASP.NET verwaltet eine Datenbank mit verschiedenen Geräten und ihren Funktionen, z. b. ob Cookies unterstützt werden, welche JavaScript-Version unterstützt wird, und so weiter. Jedes Mal, wenn ein Gerät von einem Webserver eine Webseite anfordert, sendet es einen *Benutzer-Agent-HTTP-* Header, der den Gerätetyp identifiziert. ASP.NET entspricht automatisch der angegebenen Benutzeragentzeichenfolge, wobei das entsprechende Profil in der Datenbank angegeben ist.

> [!NOTE]
> Diese Datenbank von Gerätefunktionen wird in einer Reihe von XML-Dateien gespeichert, die das [Schema der Browser Definitionsdatei](https://msdn.microsoft.com/library/ms228122.aspx)einhalten. Die standardmäßigen Geräte Profil Dateien befinden sich unter%windir%\Microsoft.NET\Framework\v2.0.50727\CONFIG\Browsers. Sie können benutzerdefinierte Dateien auch der APP\_Browser-Ordner Ihrer Anwendung hinzufügen. Weitere Informationen finden Sie unter Gewusst [wie: Erkennen von Browser Typen in ASP.net Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).

Da die Standardeinstellung UseDeviceProfile ist, werden cookiless-Formular Authentifizierungs Tickets verwendet, wenn der Standort von einem Gerät besucht wird, dessen Profil meldet, dass Cookies nicht unterstützt werden.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Codieren des Authentifizierungs Tickets in der URL

Cookies sind ein natürliches Medium zum Einschließen von Informationen aus dem Browser in jeder Anforderung an eine bestimmte Website. Daher verwenden die standardmäßigen Formular Authentifizierungs Einstellungen Cookies, wenn Sie vom besuchenden Gerät unterstützt werden. Wenn Cookies nicht unterstützt werden, muss ein alternatives Mittel zum Übergeben des Authentifizierungs Tickets vom Client an den Server verwendet werden. Eine häufige Problem Umgehung in cookichtlosen Umgebungen besteht darin, die Cookiedaten in der URL zu codieren.

Die beste Möglichkeit, um zu sehen, wie diese Informationen in die URL eingebettet werden können, besteht darin, die Website für die Verwendung von cookilosen Authentifizierungs Tickets zu erzwingen. Dies kann erreicht werden, indem die Konfigurationseinstellung cookiless auf UseUri festgelegt wird:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Nachdem Sie diese Änderung vorgenommen haben, besuchen Sie die Website über einen Browser. Wenn Sie als anonymer Benutzer besuchen, sehen die URLs genau wie zuvor aus. Wenn z. b. die Adressleiste des Browsers auf der Seite "default. aspx" angezeigt wird, wird die folgende URL angezeigt:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Bei der Anmeldung wird das Formular Authentifizierungs Ticket jedoch in die URL eingebettet. Wenn Sie z. b. die Anmeldeseite besuchen und sich als Sam anmelden, wird die Seite "default. aspx" zurückgegeben, aber die URL lautet:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Das Formular Authentifizierungs Ticket wurde in die URL eingebettet. Die Zeichenfolge (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-fymalxjfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2) stellt die hexadezimal codierten Authentifizierungs Ticketinformationen dar. dabei handelt es sich um dieselben Daten, die normalerweise in einem Cookie gespeichert werden.

Damit die Authentifizierungs Tickets ohne Cookies funktionieren, muss das System alle URLs auf der Seite codieren, um die Authentifizierungs Ticketdaten einzubeziehen. Andernfalls geht das Authentifizierungs Ticket verloren, wenn der Benutzer auf einen Link klickt. Glücklicherweise wird diese Einbett Ende Logik automatisch ausgeführt. Um diese Funktionalität zu veranschaulichen, öffnen Sie die Seite "default. aspx", und fügen Sie ein Hyperlink-Steuerelement hinzu, indem Sie die Eigenschaften "Text" und "NavigateUrl" auf "Test Link" bzw. "somepage Es spielt keine Rolle, dass es in unserem Projekt wirklich keine Seite mit dem Namen "somepage. aspx" gibt.

Speichern Sie die Änderungen an default. aspx, und besuchen Sie Sie dann über einen Browser. Melden Sie sich bei der Website an, sodass das Formular Authentifizierungs Ticket in die URL eingebettet ist. Klicken Sie als nächstes in "default. aspx" auf den Link "Link testen". Was ist passiert? Wenn keine Seite mit dem Namen "somepage. aspx" vorhanden ist, ist ein Fehler 404 aufgetreten, aber das ist hier nicht wichtig. Konzentrieren Sie sich stattdessen auf die Adressleiste in Ihrem Browser. Beachten Sie, dass das Formular Authentifizierungs Ticket in der URL enthalten ist.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

Die URL "somepage. aspx" in der Verknüpfung wurde automatisch in eine URL konvertiert, die das Authentifizierungs Ticket enthielt. wir mussten keinen Code mit Code schreiben! Das Formular Authentifizierungs Ticket wird automatisch in die URL für alle Hyperlinks eingebettet, die nicht mit `http://` oder `/`beginnen. Es spielt keine Rolle, ob der Hyperlink in einem Rückruf von Response. Redirect, in einem Hyperlink-Steuerelement oder in einem Anchor-HTML-Element (d. h. `<a href="...">...</a>`) angezeigt wird. Solange die URL nicht `http://www.someserver.com/SomePage.aspx` oder `/SomePage.aspx`ist, wird das Formular Authentifizierungs Ticket für uns eingebettet.

> [!NOTE]
> Cookiless-Formular Authentifizierungs Tickets entsprechen denselben Timeout Richtlinien wie cookiebasierte Authentifizierungs Tickets. Allerdings sind cookilose Authentifizierungs Tickets anfälliger für Wiedergabe Angriffe, da das Authentifizierungs Ticket direkt in die URL eingebettet ist. Stellen Sie sich vor, dass ein Benutzer, der eine Website besucht, sich anmeldet und dann die URL in eine e-Mail an einen Kollegen einfügt. Wenn der Kollege auf diesen Link klickt, bevor der Ablauf erreicht ist, wird er als der Benutzer angemeldet, der die e-Mail gesendet hat!

## <a name="step-3-securing-the-authentication-ticket"></a>Schritt 3: Sichern des Authentifizierungs Tickets

Das Formular Authentifizierungs Ticket wird entweder in einem Cookie übertragen oder direkt in die URL eingebettet. Zusätzlich zu den Identitätsinformationen kann das Authentifizierungs Ticket auch Benutzerdaten einschließen (wie in Schritt 4 zu sehen). Folglich ist es wichtig, dass die Daten des Tickets aus den Augenblicken und (noch wichtiger) verschlüsselt werden, dass das Formular Authentifizierungssystem garantieren kann, dass das Ticket nicht manipuliert wurde.

Um den Datenschutz der Ticketdaten sicherzustellen, kann das Formular Authentifizierungssystem die Ticketdaten verschlüsseln. Wenn die Ticketdaten nicht verschlüsselt werden, werden potenziell vertrauliche Informationen über das Netzwerk im Klartext gesendet.

Um die Authentizität eines Tickets zu gewährleisten *, muss das* Formular Authentifizierungssystem das Ticket überprüfen. Bei der Validierung wird sichergestellt, dass ein bestimmtes Datenelement nicht geändert wurde, und es wird über einen *[Nachrichten Authentifizierungscode (Message Authentication Code, Mac)](http://en.wikipedia.org/wiki/Message_authentication_code)* erreicht. Kurz gesagt, ist der Mac ein kleines Informationselement, das die zu validierenden Daten identifiziert (in diesem Fall das Ticket). Wenn die durch den Mac dargestellten Daten geändert werden, Stimmen der Mac und die Daten nicht ab. Außerdem ist es für einen Hacker Rechen intensiv, sowohl die Daten zu ändern als auch einen eigenen Mac zu generieren, der den geänderten Daten entspricht.

Beim Erstellen (oder ändern) eines Tickets erstellt das Formular Authentifizierungssystem einen Mac und fügt ihn an die Ticketdaten an. Wenn eine nachfolgende Anforderung eingeht, vergleicht das Formular Authentifizierungssystem die Mac-und Ticketdaten, um die Authentizität der Ticketdaten zu überprüfen. In Abbildung 3 wird dieser Workflow grafisch veranschaulicht.

[![die Authentizität des Tickets durch einen Mac sichergestellt wird](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Abbildung 03**: die Authentizität des Tickets wird durch einen Mac sichergestellt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))

Welche Sicherheitsmaßnahmen auf das Authentifizierungs Ticket angewendet werden, hängt von der Schutz Einstellung im &lt;Forms&gt;-Element ab. Die Schutz Einstellung kann einem der folgenden drei Werte zugewiesen werden:

- Alle: das Ticket ist verschlüsselt und digital signiert (Standard).
- Verschlüsselung nur Verschlüsselung wird angewendet-es wird kein Mac generiert.
- Keine: das Ticket ist weder verschlüsselt noch digital signiert.
- Validierung: ein Mac wird generiert, aber die Ticketdaten werden im Klartext über das Netzwerk gesendet.

Microsoft empfiehlt dringend die Verwendung der Einstellung "alle".

### <a name="setting-the-validation-and-decryption-keys"></a>Festlegen der Validierungs-und Entschlüsselungsschlüssel

Die Verschlüsselungs-und Hash Algorithmen, die vom Formular Authentifizierungssystem zum Verschlüsseln und Überprüfen des Authentifizierungs Tickets verwendet werden, können über das [&lt;machineKey&gt;-Element](https://msdn.microsoft.com/library/w8h3skw9.aspx) in der Datei "Web. config" angepasst werden. In Tabelle 2 werden die Attribute des &lt;machineKey&gt; Elements und ihre möglichen Werte beschrieben.

| **Attribut** | **Beschreibung** |
| --- | --- |
| Entschlüsselung | Gibt den Algorithmus an, der für die Verschlüsselung verwendet wird. Dieses Attribut kann einen der folgenden vier Werte aufweisen:-Auto-the default; bestimmt den Algorithmus basierend auf der Länge des Attributs decryptionKey. -AES: verwendet den [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) -Algorithmus. -Des-verwendet den [Daten Verschlüsselungs Standard (Data Encryption Standard, des)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) . dieser Algorithmus wird als Rechen schwach angesehen und sollte nicht verwendet werden. -3DES: verwendet den [Triple des](http://en.wikipedia.org/wiki/Triple_DES) -Algorithmus, der den des-Algorithmus dreimal anwendet. |
| decryptionKey | Der geheime Schlüssel, der vom Verschlüsselungsalgorithmus verwendet wird. Bei diesem Wert muss es sich entweder um eine hexadezimale Zeichenfolge der entsprechenden Länge (basierend auf dem Entschlüsselungs Wert), die automatische Generierung oder einen der Werte, die mit IsolateApps angefügt werden. Das Hinzufügen von IsolateApps weist ASP.net an, einen eindeutigen Wert für jede Anwendung zu verwenden. Der Standardwert lautet AutoGenerate, IsolateApps. |
| Überprüfung | Gibt den für die Validierung verwendeten Algorithmus an. Dieses Attribut kann einen der folgenden vier Werte aufweisen:-AES-verwendet den Advanced Encryption Standard (AES)-Algorithmus. -MD5-verwendet den [Message Digest 5-Algorithmus (MD5)](http://en.wikipedia.org/wiki/MD5) . -SHA1-verwendet den [SHA1](http://en.wikipedia.org/wiki/Sha1) -Algorithmus (Standard). -3DES-verwendet den Triple des-Algorithmus. |
| validationKey | Der geheime Schlüssel, der vom Validierungs Algorithmus verwendet wird. Bei diesem Wert muss es sich entweder um eine hexadezimale Zeichenfolge der entsprechenden Länge (basierend auf dem Wert in der Validierung), der automatischen Generierung oder eines der beiden Werte handeln, die mit IsolateApps angefügt werden. Das Hinzufügen von IsolateApps weist ASP.net an, einen eindeutigen Wert für jede Anwendung zu verwenden. Der Standardwert lautet AutoGenerate, IsolateApps. |

**Tabelle 2**: die &lt;machineKey-&gt; Element Attribute

Eine ausführliche Erläuterung dieser Verschlüsselungs-und Validierungs Optionen sowie der vor-und Nachteile der verschiedenen Algorithmen geht über den Rahmen dieses Tutorials hinaus. Ausführliche Informationen zu diesen Problemen, einschließlich Anleitungen dazu, welche Verschlüsselungs-und Validierungs Algorithmen verwendet werden müssen, welche Schlüssellängen verwendet werden und wie diese Schlüssel am besten generiert werden, finden Sie unter *Professional ASP.NET 2,0 Sicherheit, Mitgliedschaft und Rollen Verwaltung*.

Standardmäßig werden die Schlüssel, die für die Verschlüsselung und Validierung verwendet werden, automatisch für jede Anwendung generiert. diese Schlüssel werden in der lokalen Sicherheits Autorität (Local Security Authority, LSA) gespeichert. Kurz gesagt: die Standardeinstellungen garantieren eindeutige Schlüssel auf einem Webserver-by-Webserver-Server und auf Anwendungs Basis. Folglich funktioniert dieses Standardverhalten nicht für die beiden folgenden Szenarien:

- **Webfarmen** : in einem [Webfarm](http://en.wikipedia.org/wiki/Web_farm) Szenario wird eine einzelne Webanwendung zum Zweck der Skalierbarkeit und Redundanz auf mehreren Webservern gehostet. Jede eingehende Anforderung wird an einen Server in der Farm gesendet. Dies bedeutet, dass während der Lebensdauer der Sitzung eines Benutzers verschiedene Server verwendet werden können, um die verschiedenen Anforderungen zu verarbeiten. Folglich muss jeder Server dieselben Verschlüsselungs-und Validierungs Schlüssel verwenden, damit das Formular Authentifizierungs Ticket, das auf einem Server erstellt, verschlüsselt und überprüft wird, auf einem anderen Server in der Farm entschlüsselt und überprüft werden kann.
- **Anwendungsübergreifende Ticket Freigabe** : ein einzelner Webserver kann mehrere ASP.NET-Anwendungen hosten. Wenn Sie für diese verschiedenen Anwendungen die Verwendung eines einzelnen Formular Authentifizierungs Tickets benötigen, ist es zwingend erforderlich, dass die Verschlüsselungs-und Validierungs Schlüssel einander entsprechen.

Wenn Sie in einer Webfarm Einstellung arbeiten oder Authentifizierungs Tickets für mehrere Anwendungen auf demselben Server freigeben, müssen Sie das &lt;machineKey-&gt; Element in den betroffenen Anwendungen konfigurieren, damit die Werte für "decryptionKey" und "validationKey" übereinstimmen.

Obwohl keines der obigen Szenarien für unsere Beispielanwendung gilt, können wir weiterhin explizite decryptionKey-und validationKey-Werte angeben und die zu verwendenden Algorithmen definieren. Fügen Sie der Datei "Web. config" eine &lt;machineKey-&gt; Einstellung hinzu:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Weitere Informationen finden Sie [unter Vorgehensweise: Konfigurieren von machineKey in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Die Werte "decryptionKey" und "validationKey" wurden von der [Webseite "perfekte Kenn Wörter](https://www.grc.com/passwords.htm)" von [Steve Gibson](http://www.grc.com/stevegibson.htm)entnommen, die auf jedem Seitenbesuch 64 zufällige hexadezimale Zeichen generiert. Um die Wahrscheinlichkeit zu verringern, dass diese Schlüssel in ihren Produktionsanwendungen umgesetzt werden, wird empfohlen, die oben genannten Schlüssel durch zufällig generierte Schlüssel von der Seite "perfekte Kenn Wörter" zu ersetzen.

## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Schritt 4: Speichern zusätzlicher Benutzerdaten im Ticket

Viele Webanwendungen zeigen Informationen zu oder basieren auf der Anzeige der Seite auf dem aktuell angemeldeten Benutzer. Beispielsweise kann eine Webseite den Namen des Benutzers und das Datum, an dem sich der letzte angemeldete, in der oberen Ecke jeder Seite anzeigt. Das Formular Authentifizierungs Ticket speichert den Benutzernamen des aktuell angemeldeten Benutzers. Wenn jedoch weitere Informationen erforderlich sind, muss die Seite in den Benutzerspeicher (in der Regel eine Datenbank) gelangen, um die Informationen zu suchen, die nicht im Authentifizierungs Ticket gespeichert sind.

Mit etwas Code können wir zusätzliche Benutzerinformationen im Formular Authentifizierungs Ticket speichern. Diese Daten können über die [UserData-Eigenschaft](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)der [FormsAuthenticationTicket-Klasse](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)ausgedrückt werden. Dies ist ein nützlicher Ort, um kleine Mengen von Informationen über den Benutzer zu platzieren, der häufig benötigt wird. Der in der UserData-Eigenschaft angegebene Wert ist als Teil des Authentifizierungs Ticket Cookies enthalten und wird, wie die anderen Ticket Felder, basierend auf der Konfiguration des Formular Authentifizierungssystems verschlüsselt und überprüft. Standardmäßig ist "UserData" eine leere Zeichenfolge.

Um Benutzerdaten im Authentifizierungs Ticket speichern zu können, müssen Sie auf der Anmeldeseite ein wenig Code schreiben, das die benutzerspezifischen Informationen erfasst und im Ticket speichert. Da "UserData" eine Eigenschaft vom Typ "String" ist, müssen die darin gespeicherten Daten ordnungsgemäß als Zeichenfolge serialisiert werden. Stellen Sie sich beispielsweise vor, dass der Benutzerspeicher das Geburtsdatum des Benutzers und den Namen seines Arbeitgebers enthalten hat und wir diese beiden Eigenschaftswerte im Authentifizierungs Ticket speichern wollten. Diese Werte könnten in eine Zeichenfolge serialisiert werden, indem das Datum der Geburts Zeichenfolge des Benutzers mit einer Pipe (|), gefolgt vom Namen des Arbeitgebers, verkettet wird. Für einen Benutzer, der am 15. August 1974, der für Northwind Traders, geboren wurde, weisen wir der UserData-Eigenschaft die Zeichenfolge: 1974-08-15 | Northwind Traders.

Wenn Sie auf die im Ticket gespeicherten Daten zugreifen müssen, können wir dies tun, indem wir das FormsAuthenticationTicket der aktuellen Anforderung erfassen und die UserData-Eigenschaft deserialisieren. Im Fall des Beispiels für das Geburtsdatum und den Namen des Arbeitgeber namens würden wir die UserData-Zeichenfolge basierend auf dem Trennzeichen (|) in zwei Teil Zeichenfolgen aufteilen.

[![zusätzlichen Benutzerinformationen können im Authentifizierungs Ticket gespeichert werden.](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Abbildung 04**: zusätzliche Benutzerinformationen können im Authentifizierungs Ticket gespeichert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))

### <a name="writing-information-to-userdata"></a>Schreiben von Informationen in UserData

Leider ist das Hinzufügen von benutzerspezifischen Informationen zu einem Formular Authentifizierungs Ticket nicht so einfach wie erwartet. Die UserData-Eigenschaft der FormsAuthenticationTicket-Klasse ist schreibgeschützt und kann nur über den FormsAuthenticationTicket-Klassenkonstruktor angegeben werden. Wenn die UserData-Eigenschaft im Konstruktor angegeben wird, müssen auch die anderen Werte des Tickets angegeben werden: Benutzername, Ausgabedatum, Ablauf usw. Als wir die Anmeldeseite im vorherigen Tutorial erstellt haben, wurde dies von der FormsAuthentication-Klasse für uns erledigt. Beim Hinzufügen von UserData zu FormsAuthenticationTicket müssen wir Code schreiben, um einen Großteil der Funktionen zu replizieren, die bereits von der FormsAuthentication-Klasse bereitgestellt werden.

Sehen wir uns den erforderlichen Code für die Arbeit mit UserData an, indem wir die Seite "Login. aspx" aktualisieren, um zusätzliche Informationen über den Benutzer für das Authentifizierungs Ticket aufzuzeichnen. Angenommen, der Benutzerspeicher enthält Informationen über das Unternehmen, für den der Benutzer arbeitet, und seinen Titel, und wir möchten diese Informationen im Authentifizierungs Ticket erfassen. Aktualisieren Sie den loginbutton-Ereignishandler der Login. aspx-Seite, sodass der Code wie folgt aussieht:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Wir können diesen Code schrittweise durchlaufen. Die Methode beginnt mit der Definition von vier Zeichen folgen Arrays: Benutzer, Kenn Wörter, Unternehmens Name und titleatcompany. Diese Arrays enthalten die Benutzernamen, Kenn Wörter, Firmennamen und Titel für die Benutzerkonten im System, von denen es drei gibt: Scott, jisun und Sam. In einer echten Anwendung werden diese Werte aus dem Benutzerspeicher abgefragt, nicht im Quellcode der Seite hart codiert.

Wenn die angegebenen Anmelde Informationen gültig sind, haben wir im vorherigen Tutorial einfach FormsAuthentication. RedirectFromLoginPage (username. Text, Erinnerungs Me. Check) aufgerufen, die die folgenden Schritte ausgeführt hat:

1. Das Formular Authentifizierungs Ticket wurde erstellt.
2. Das Ticket wurde in den entsprechenden Speicher geschrieben. Bei Cookies-basierten Authentifizierungs Tickets wird die cookiesammlung des Browsers verwendet. bei cookielosen Authentifizierungs Tickets werden die Ticketdaten in die URL serialisiert.
3. Der Benutzer wurde an die entsprechende Seite umgeleitet.

Diese Schritte werden im obigen Code repliziert. Zuerst wird die Zeichenfolge, die letztendlich in der UserData-Eigenschaft gespeichert wird, durch Kombinieren des Firmennamens und des Titels gebildet, wobei die beiden Werte durch ein senkrechter Strich (|) getrennt werden.

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

Als nächstes wird die FormsAuthentication. GetAuthCookie-Methode aufgerufen, die das Authentifizierungs Ticket erstellt, diese gemäß den Konfigurationseinstellungen verschlüsselt und überprüft und in einem HttpCookie-Objekt platziert.

HttpCookie authCookie = FormsAuthentication. GetAuthCookie (username. Text, Erinnerungen. aktiviert);

Um mit dem im Cookie eingebetteten formauthenticationticket-Ereignis arbeiten zu können, müssen wir die [Entschlüsselungsmethode](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)der formauthentication-Klasse aufzurufen und dabei den Cookiewert übergeben.

FormsAuthenticationTicket Ticket = FormsAuthentication. entschlüsseln (authCookie. Value);

Anschließend erstellen wir eine *neue* FormsAuthenticationTicket-Instanz basierend auf den vorhandenen Werten von FormsAuthenticationTicket. Dieses neue Ticket enthält jedoch die benutzerspezifischen Informationen (userdatastring).

FormsAuthenticationTicket newTicket = New FormsAuthenticationTicket (Ticket. Version, Ticket. Name, Ticket. IssueDate, Ticket. Ablauf, Ticket. IsPersistent, userdatastring);

Anschließend verschlüsseln (und validieren) Sie die neue FormsAuthenticationTicket-Instanz, indem Sie die [Methode verschlüsseln](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)aufrufen und diese verschlüsselten (und validierten) Daten zurück in authCookie ablegen.

authCookie. Value = FormsAuthentication. verschlüsseln (newTicket);

Zum Schluss wird authCookie der Response. Cookies-Auflistung hinzugefügt, und die GetRedirectUrl-Methode wird aufgerufen, um die entsprechende Seite zum Senden des Benutzers zu bestimmen.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Der gesamte Code ist erforderlich, da die UserData-Eigenschaft schreibgeschützt ist und die FormsAuthentication-Klasse keine Methoden zum Angeben von UserData-Informationen in ihren GetAuthCookie-, SetAuthCookie-oder RedirectFromLoginPage-Methoden bereitstellt.

> [!NOTE]
> Der soeben untersuchte Code speichert benutzerspezifische Informationen in einem Cookie-basierten Authentifizierungs Ticket. Die Klassen, die für die Serialisierung des Formular Authentifizierungs Tickets zur URL zuständig sind, sind intern für die .NET Framework. Lange Geschichte kurz: Sie können Benutzerdaten nicht in einem cookiless-Formular Authentifizierungs Ticket speichern.

### <a name="accessing-the-userdata-information"></a>Zugreifen auf die UserData-Informationen

An diesem Punkt werden die Firmennamen und der Titel jedes Benutzers in der UserData-Eigenschaft des Formular Authentifizierungs Tickets gespeichert, wenn Sie sich anmelden. Auf diese Informationen kann über das Authentifizierungs Ticket auf einer beliebigen Seite zugegriffen werden, ohne dass eine Fahrt zum Benutzerspeicher erforderlich ist. Um zu veranschaulichen, wie diese Informationen aus der UserData-Eigenschaft abgerufen werden können, aktualisieren Sie "default. aspx", sodass die Willkommensnachricht nicht nur den Namen des Benutzers enthält, sondern auch das Unternehmen, für das Sie arbeiten, und ihren Titel.

"Default. aspx" enthält derzeit ein "authentipeedmessagepanel"-Panel mit einem Label-Steuerelement namens "welcomebackmessage". Dieser Bereich wird nur authentifizierten Benutzern angezeigt. Aktualisieren Sie den Code auf der Seite "default. aspx"\_Load-Ereignishandler, sodass er wie folgt aussieht:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Wenn Request. IsAuthenticated auf true festgelegt ist, wird die Text-Eigenschaft von welcomebackmessage zuerst auf Willkommen zurück, *Benutzername*, festgelegt. Anschließend wird die User. Identity-Eigenschaft in ein FormsIdentity-Objekt umgewandelt, sodass wir auf das zugrunde liegende FormsAuthenticationTicket zugreifen können. Sobald wir über das FormsAuthenticationTicket verfügen, deserialisieren wir die UserData-Eigenschaft in den Firmennamen und-Titel. Dies erfolgt durch Aufteilen der Zeichenfolge auf das senkrechter Strich. Der Name und der Titel des Unternehmens werden dann in der Bezeichnung "welcomebackmessage" angezeigt.

Abbildung 5 zeigt einen Screenshot dieser Anzeige in Aktion. Bei der Anmeldung als Scott wird eine Willkommensnachricht angezeigt, die Scott das Unternehmen und den Titel enthält.

[![das aktuell angemeldete Unternehmen und der Titel des angemeldeten Benutzers angezeigt werden.](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Abbildung 05**: das aktuell angemeldete Unternehmen und der Titel des angemeldeten Benutzers werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))

> [!NOTE]
> Die UserData-Eigenschaft des Authentifizierungs Tickets dient als Cache für den Benutzerspeicher. Wie bei jedem Cache muss er aktualisiert werden, wenn die zugrunde liegenden Daten geändert werden. Wenn beispielsweise eine Webseite vorhanden ist, von der Benutzer ihr Profil aktualisieren können, müssen die in der UserData-Eigenschaft zwischengespeicherten Felder aktualisiert werden, um die vom Benutzer vorgenommenen Änderungen widerzuspiegeln.

## <a name="step-5-using-a-custom-principal"></a>Schritt 5: Verwenden eines benutzerdefinierten Prinzipals

Bei jeder eingehenden Anforderung versucht FormsAuthenticationModule, den Benutzer zu authentifizieren. Wenn ein nicht abgelaufenes Authentifizierungs Ticket vorhanden ist, weist das FormsAuthenticationModule die HttpContext. User-Eigenschaft einem neuen GenericPrincipal-Objekt zu. Dieses GenericPrincipal-Objekt verfügt über eine Identität vom Typ FormsIdentity, die einen Verweis auf das Formular Authentifizierungs Ticket enthält. Die GenericPrincipal-Klasse enthält die erforderliche Mindestfunktionalität, die von einer Klasse benötigt wird, die IPrincipal implementiert. Sie verfügt nur über eine Identity-Eigenschaft und eine IsInRole-Methode.

Das Prinzipal Objekt hat zwei Aufgaben: um anzugeben, welche Rollen der Benutzer angehört, und um Identitätsinformationen bereitzustellen. Dies erfolgt über die IsInRole (*roleName*)-Methode bzw. die Identity-Eigenschaft der IPrincipal-Schnittstelle. Die GenericPrincipal-Klasse ermöglicht, dass ein Zeichen folgen Array mit Rollennamen über den zugehörigen Konstruktor angegeben wird. die IsInRole (*roleName*)-Methode überprüft lediglich, ob der übergebenen *roleName* im Zeichen folgen Array vorhanden ist. Wenn das FormsAuthenticationModule den GenericPrincipal erstellt, übergibt es ein leeres Zeichen folgen Array an den Konstruktor von GenericPrincipal. Folglich geben alle Aufrufe von IsInRole immer false zurück.

Die GenericPrincipal-Klasse erfüllt die Anforderungen für die meisten Formular basierten Authentifizierungs Szenarios, in denen keine Rollen verwendet werden. In Situationen, in denen die standardmäßige Rollen Behandlung unzureichend ist, oder wenn Sie dem Benutzer ein benutzerdefiniertes IIdentity-Objekt zuordnen müssen, können Sie ein benutzerdefiniertes IPrincipal-Objekt während des Authentifizierungs Workflows erstellen und es der HttpContext. User-Eigenschaft zuweisen.

> [!NOTE]
> Wie wir in zukünftigen Tutorials sehen werden, wenn ASP. Das Rollen Framework von NET ist aktiviert und erstellt ein benutzerdefiniertes Prinzipal Objekt vom Typ [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) und überschreibt das von der Formular Authentifizierung erstellte GenericPrincipal-Objekt. Dies geschieht, um die IsInRole-Methode des Prinzipals mit der API des Rollen-Frameworks zu verbinden.

Da wir uns noch nicht mit Rollen befassen, besteht der einzige Grund, den wir für die Erstellung eines benutzerdefinierten Prinzipals zu diesem Zeitpunkt hätten, darin, dem Prinzipal ein benutzerdefiniertes IIdentity-Objekt zuzuordnen. In Schritt 4 haben wir uns mit dem Speichern zusätzlicher Benutzerinformationen in der UserData-Eigenschaft des Authentifizierungs Tickets beschäftigt. Dies bezieht sich insbesondere auf den Firmennamen und den Titel des Benutzers. Die UserData-Informationen sind jedoch nur über das Authentifizierungs Ticket und nur dann als serialisierte Zeichenfolge zugänglich. Dies bedeutet, dass Sie immer dann, wenn wir die im Ticket gespeicherten Benutzerinformationen anzeigen möchten, die UserData-Eigenschaft analysieren müssen.

Wir können die Entwickler Leistung verbessern, indem wir eine Klasse erstellen, die IIdentity implementiert und die Eigenschaften "CompanyName" und "Title" enthält. Auf diese Weise kann ein Entwickler direkt über die Eigenschaften "Unternehmenname" und "Title" auf den Namen und den Titel des angemeldeten Benutzers zugreifen, ohne zu wissen, wie die UserData-Eigenschaft analysiert werden muss.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Erstellen der benutzerdefinierten Identitäts-und Prinzipal Klassen

In diesem Tutorial erstellen wir die benutzerdefinierten Prinzipal-und Identitäts Objekte im Ordner App\_Code. Fügen Sie zunächst dem Projekt eine APP\_Code Ordner hinzu. Klicken Sie mit der rechten Maustaste auf den Projektnamen in Projektmappen-Explorer, wählen Sie die Option ASP.NET Ordner hinzufügen aus, und wählen Sie App\_Code aus. Der APP-\_Code Ordner ist ein spezieller ASP.NET-Ordner, der für die Website spezifische Klassendateien enthält.

> [!NOTE]
> Der APP-\_Code Ordner sollte nur verwendet werden, wenn Sie Ihr Projekt über das Website Projekt Modell verwalten. Wenn Sie das [Webanwendungs Projekt Modell](https://msdn.microsoft.com/asp.net/Aa336618.aspx)verwenden, erstellen Sie einen Standardordner, und fügen Sie die-Klassen hinzu. Beispielsweise können Sie einen neuen Ordner namens Classes hinzufügen und den Code dort platzieren.

Fügen Sie als nächstes dem App-\_Code Ordner zwei neue Klassendateien hinzu, eine namens CustomIdentity.cs und eine mit dem Namen CustomPrincipal.cs.

[![dem Projekt die customidentity-Klasse und die CustomPrincipal-Klasse hinzufügen](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Abbildung 06**: Hinzufügen der Klassen "customidentity" und "CustomPrincipal" zu Ihrem Projekt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))

Die customidentity-Klasse ist für die Implementierung der IIdentity-Schnittstelle zuständig, die die Eigenschaften "AuthenticationType", "IsAuthenticated" und "Name" definiert. Zusätzlich zu den erforderlichen Eigenschaften möchten wir sowohl das zugrunde liegende Formular Authentifizierungs Ticket als auch die Eigenschaften für den Firmennamen und den Titel des Benutzers verfügbar machen. Geben Sie den folgenden Code in die customidentity-Klasse ein.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Beachten Sie, dass die-Klasse eine FormsAuthenticationTicket-Member-Variable (\_Ticket) enthält und diese Ticketinformationen über den-Konstruktor angegeben werden müssen. Diese Ticketdaten werden bei der Rückgabe des Identitäts namens verwendet. die UserData-Eigenschaft wird so analysiert, dass die Werte für die Eigenschaften "CompanyName" und "Title" zurückgegeben werden.

Erstellen Sie als nächstes die CustomPrincipal-Klasse. Da wir zu diesem Zeitpunkt keine Rollen betreffen, akzeptiert der Konstruktor der CustomPrincipal-Klasse nur ein customidentity-Objekt. die IsInRole-Methode gibt immer false zurück.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Zuweisen eines CustomPrincipal-Objekts zum Sicherheitskontext der eingehenden Anforderung

Wir verfügen jetzt über eine Klasse, die die IIdentity-Standardspezifikation erweitert, sodass Sie die Eigenschaften "CompanyName" und "Title" sowie eine benutzerdefinierte Prinzipal Klasse enthält, die die benutzerdefinierte Identität verwendet. Wir können die ASP.NET-Pipeline schrittweise ausführen und das benutzerdefinierte Prinzipal Objekt dem Sicherheitskontext der eingehenden Anforderung zuweisen.

Die ASP.NET-Pipeline nimmt eine eingehende Anforderung an und verarbeitet sie durch eine Reihe von Schritten. Bei jedem Schritt wird ein bestimmtes Ereignis ausgelöst, sodass Entwickler in die ASP.NET-Pipeline tippen und die Anforderung an bestimmten Punkten im Lebenszyklus ändern können. Das FormsAuthenticationModule-Modul wartet beispielsweise darauf, dass ASP.NET das [AuthenticateRequest-Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)aufhebt. an diesem Punkt wird die eingehende Anforderung für ein Authentifizierungs Ticket überprüft. Wenn ein Authentifizierungs Ticket gefunden wird, wird ein GenericPrincipal-Objekt erstellt und der HttpContext. User-Eigenschaft zugewiesen.

Nach dem AuthenticateRequest-Ereignis löst die ASP.NET-Pipeline das [PostAuthenticateRequest-Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)aus, in dem wir das von FormsAuthenticationModule erstellte GenericPrincipal-Objekt durch eine Instanz des CustomPrincipal-Objekts ersetzen können. In Abbildung 7 ist dieser Workflow dargestellt.

[![GenericPrincipal im postauthenticationrequest-Ereignis durch einen CustomPrincipal ersetzt](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Abbildung 07**: GenericPrincipal wird im postauthenticationrequest-Ereignis durch einen CustomPrincipal ersetzt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))

Um Code als Reaktion auf ein ASP.NET-Pipeline Ereignis auszuführen, können wir entweder den entsprechenden Ereignishandler in "Global. asax" erstellen oder ein eigenes HTTP-Modul erstellen. In diesem Tutorial erstellen wir den Ereignishandler in "Global. asax". Fügen Sie zunächst Global. asax zu Ihrer Website hinzu. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und fügen Sie ein Element des Typs globale Anwendungsklasse mit dem Namen Global. asax hinzu.

[![Ihrer Website eine Global. asax-Datei hinzufügen](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Abbildung 08**: Hinzufügen einer Global. asax-Datei zu Ihrer Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))

Die Standardvorlage "Global. asax" umfasst Ereignishandler für eine Reihe von ASP.NET-Pipeline Ereignissen, einschließlich Start-, End-und [Fehler Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx). Sie können diese Ereignishandler auch entfernen, da Sie für diese Anwendung nicht benötigt werden. Das Ereignis, an dem wir interessiert sind, ist PostAuthenticateRequest. Aktualisieren Sie Ihre Global. asax-Datei, damit das Markup in etwa wie folgt aussieht:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

Die Anwendung\_onpostauthenticaterequest-Methode wird jedes Mal ausgeführt, wenn die ASP.NET-Laufzeit das PostAuthenticateRequest-Ereignis auslöst, das einmal bei jeder eingehenden Seiten Anforderung auftritt. Der Ereignishandler prüft zunächst, ob der Benutzer authentifiziert ist und über die Formular Authentifizierung authentifiziert wurde. Wenn dies der Fall ist, wird ein neues customidentity-Objekt erstellt, und das Authentifizierungs Ticket der aktuellen Anforderung wird in seinem Konstruktor übergeben. Danach wird ein CustomPrincipal-Objekt erstellt und an das soeben erstellte customidentity-Objekt im Konstruktor übergeben. Schließlich wird der Sicherheitskontext der aktuellen Anforderung dem neu erstellten CustomPrincipal-Objekt zugewiesen.

Beachten Sie, dass der letzte Schritt, der das CustomPrincipal-Objekt mit dem Sicherheitskontext der Anforderung verknüpft, dem Prinzipal zwei Eigenschaften zugewiesen wird: HttpContext. User und Thread. CurrentPrincipal. Diese beiden Zuweisungen sind aufgrund der Art und Weise erforderlich, wie Sicherheits Kontexte in ASP.NET behandelt werden. Der .NET Framework ordnet jedem laufenden Thread einen Sicherheitskontext zu. Diese Informationen stehen als IPrincipal-Objekt über die [CurrentPrincipal-Eigenschaft](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)des [Thread Objekts](https://msdn.microsoft.com/library/system.threading.thread.aspx)zur Verfügung. Was verwirrend ist, besteht darin, dass ASP.net über eigene Sicherheitskontext Informationen (HttpContext. User) verfügt.

In bestimmten Szenarien wird die Thread. CurrentPrincipal-Eigenschaft bei der Ermittlung des Sicherheits Kontexts untersucht. in anderen Szenarien wird HttpContext. User verwendet. Beispielsweise gibt es in .NET Sicherheitsfunktionen, mit denen Entwickler deklarativ angeben können, welche Benutzer oder Rollen eine Klasse instanziieren oder bestimmte Methoden aufrufen können (Weitere Informationen finden [Sie unter Hinzufügen von Autorisierungs Regeln zu Geschäfts-und Daten Ebenen mithilfe von "PrincipalPermissionAttribute](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)"). Im Hintergrund bestimmen diese deklarativen Techniken den Sicherheitskontext über die Thread. CurrentPrincipal-Eigenschaft.

In anderen Szenarien wird die HttpContext. User-Eigenschaft verwendet. Im vorherigen Tutorial haben wir beispielsweise diese Eigenschaft verwendet, um den Benutzernamen des aktuell angemeldeten Benutzers anzuzeigen. Natürlich ist es zwingend erforderlich, dass die Sicherheitskontext Informationen in den Eigenschaften "Thread. CurrentPrincipal" und "HttpContext. User" übereinstimmen.

Die ASP.NET-Laufzeit synchronisiert diese Eigenschaftswerte automatisch für uns. Diese Synchronisierung erfolgt jedoch nach dem AuthenticateRequest-Ereignis, jedoch *vor* dem PostAuthenticateRequest-Ereignis. Folglich muss beim Hinzufügen eines benutzerdefinierten Prinzipals im PostAuthenticateRequest-Ereignis sicher sein, dass der Thread. CurrentPrincipal oder else Thread. CurrentPrincipal und HttpContext. User nicht synchronisiert werden. Eine ausführlichere Erläuterung zu diesem Problem finden Sie unter [context. User im Vergleich zu Thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) .

### <a name="accessing-the-companyname-and-title-properties"></a>Zugreifen auf die CompanyName-und Title-Eigenschaften

Wenn eine Anforderung eingeht und an die ASP.net-Engine weitergeleitet wird, wird die Anwendung\_onpostauthenticaterequest-Ereignishandler in "Global. asax" ausgelöst. Wenn die Anforderung erfolgreich durch das FormsAuthenticationModule authentifiziert wurde, erstellt der Ereignishandler ein neues CustomPrincipal-Objekt mit einem customidentity-Objekt auf Grundlage des Formular Authentifizierungs Tickets. Wenn diese Logik eingerichtet ist, ist der Zugriff auf Informationen über den Firmennamen und den Titel des aktuell angemeldeten Benutzers unglaublich unkompliziert.

Kehren Sie zur Seite\_Load-Ereignishandler in Default. aspx zurück. in Schritt 4 haben wir Code geschrieben, um das Formular Authentifizierungs Ticket abzurufen und die UserData-Eigenschaft zu analysieren, um den Firmennamen und den Titel des Benutzers anzuzeigen. Wenn die CustomPrincipal-und customidentity-Objekte jetzt verwendet werden, ist es nicht erforderlich, die Werte aus der UserData-Eigenschaft des Tickets zu analysieren. Verwenden Sie stattdessen einfach einen Verweis auf das customidentity-Objekt, und verwenden Sie die Eigenschaften "CompanyName" und "Title":

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie die Einstellungen des Formular Authentifizierungssystems über "Web. config" angepasst werden. Wir haben uns angesehen, wie das Ablaufdatum des Authentifizierungs Tickets behandelt wird und wie die Verschlüsselungs-und Validierungs Schutzvorrichtungen verwendet werden, um das Ticket vor Prüfung und Änderungen zu schützen. Schließlich haben wir die Verwendung der UserData-Eigenschaft des Authentifizierungs Tickets erläutert, um zusätzliche Benutzerinformationen im Ticket selbst zu speichern, und wie benutzerdefinierte Prinzipal-und Identitäts Objekte verwendet werden, um diese Informationen auf Entwickler freundlichere Weise verfügbar zu machen.

In diesem Tutorial wird die Untersuchung der Formular Authentifizierung in ASP.net abgeschlossen. Im nächsten Tutorial wird unsere Tour in das Mitgliedschafts Framework gestartet.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Dissecting der Formular Authentifizierung](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Erläutert: Formular Authentifizierung in ASP.NET 2,0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Vorgehensweise: Schützen der Formular Authentifizierung in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2,0 Sicherheit, Mitgliedschaft und Rollen Verwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Sichern von Anmelde Steuerelementen](https://msdn.microsoft.com/library/ms178346.aspx)
- [Das &lt;Authentication&gt;-Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Das &lt;Forms&gt; Element für &lt;Authentifizierung&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [Das &lt;machineKey-&gt; Element](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Verständnis des Formular Authentifizierungs Tickets und Cookies](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video Schulung zu Themen in diesem Tutorial

- [Vorgehensweise beim Ändern der Eigenschaften der Formular Authentifizierung](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Einrichten und Verwenden der Authentifizierung ohne Cookies in einer ASP.NET-Anwendung](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Verschieben der ASP-Formularanmeldung](../../../videos/authentication/asp-forms-login-relocation.md)
- [Benutzerdefinierte Schlüsselkonfiguration für die Formularanmeldung](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Hinzufügen von benutzerdefinierten Daten zur Authentifizierungsmethode](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Verwenden von benutzerdefinierten Prinzipalobjekten](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Alicja Maziarz. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](an-overview-of-forms-authentication-cs.md)
> [Weiter](security-basics-and-asp-net-support-vb.md)
