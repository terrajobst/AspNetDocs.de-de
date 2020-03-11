---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Verhindern von Angriffen mit offenerC#Umleitung () | Microsoft-Dokumentation
author: jongalloway
description: In diesem Tutorial wird erläutert, wie Sie offene Umleitungs Angriffe in Ihren ASP.NET-MVC-Anwendungen verhindern können. In diesem Tutorial werden die Änderungen erläutert, die vorgenommen wurden...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: cfa635d4fd14d031993c5b452325cbe334f82dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432585"
---
# <a name="preventing-open-redirection-attacks-c"></a>Verhindern von Angriffen durch offene Umleitungen (C#)

von [Jon Galloway](https://github.com/jongalloway)

> In diesem Tutorial wird erläutert, wie Sie offene Umleitungs Angriffe in Ihren ASP.NET-MVC-Anwendungen verhindern können. In diesem Tutorial werden die Änderungen erläutert, die in AccountController in ASP.NET MVC 3 vorgenommen wurden, und es wird veranschaulicht, wie Sie diese Änderungen in Ihren vorhandenen ASP.NET MVC 1,0-und 2-Anwendungen anwenden können.

## <a name="what-is-an-open-redirection-attack"></a>Was ist ein offener Umleitungs Angriff?

Jede Webanwendung, die an eine URL umgeleitet wird, die über die Anforderung angegeben wird, z. b. die QueryString-oder Formulardaten, kann möglicherweise manipuliert werden, um Benutzer an eine externe, böswillige URL umzuleiten. Diese Manipulation wird als offener Umleitungs Angriff bezeichnet.

Wenn Ihre Anwendungslogik eine Umleitung an eine angegebene URL durchführt, müssen Sie überprüfen, ob die Umleitungs-URL nicht manipuliert wurde. Der Anmelde Name, der im standardmäßigen AccountController für ASP.NET MVC 1,0 und ASP.NET MVC 2 verwendet wird, ist anfällig für Open Redirect-Angriffe. Glücklicherweise ist es einfach, vorhandene Anwendungen so zu aktualisieren, dass die Korrekturen aus ASP.NET MVC 3 Preview verwendet werden.

Um das Sicherheitsrisiko zu verstehen, sehen wir uns an, wie die Anmelde Umleitung in einem standardmäßigen ASP.NET MVC 2-Webanwendungs Projekt funktioniert. Wenn Sie versuchen, eine Controller Aktion zu besuchen, die über das Attribut [autorisieren] verfügt, werden nicht autorisierte Benutzer in die/Account/Logon-Ansicht umgeleitet. Diese Umleitung zu/Account/Logon enthält einen "rückkehrnurl"-QueryString-Parameter, sodass der Benutzer an die ursprünglich angeforderte URL zurückgegeben werden kann, nachdem Sie sich erfolgreich angemeldet haben.

Im folgenden Screenshot sehen Sie, dass der Versuch, auf die/Account/ChangePassword-Ansicht zuzugreifen, wenn Sie nicht angemeldet ist, eine Umleitung an/Account/Logon führt. Rückgab =% 2fakcount% 2F ChangePassword% 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Abbildung 01**: Anmeldeseite mit offener Umleitung

Da der Parameter "QueryString" von "Rückkehrer" nicht überprüft wird, kann er von einem Angreifer geändert werden, um eine beliebige URL-Adresse in den Parameter einzufügen, um einen offenen Umleitungs Angriff auszuführen. Um dies zu veranschaulichen, können wir den Parameter "der rückkehrnurl" in " [http://bing.com](http://bing.com)" ändern, sodass die resultierende Anmelde-URL "/Account/LOGON" lautet? Rückkehrnurl =<http://www.bing.com/>. Nach erfolgreicher Anmeldung bei der Site werden wir zu [http://bing.com](http://bing.com)umgeleitet. Da diese Umleitung nicht überprüft wird, könnte Sie stattdessen auf eine böswillige Site verweisen, die versucht, den Benutzer zu täuschen.

### <a name="a-more-complex-open-redirection-attack"></a>Ein komplexerer offener Umleitungs Angriff

Angriffe mit offener Umleitung sind besonders gefährlich, weil ein Angreifer weiß, dass wir versuchen, sich bei einer bestimmten Website anzumelden, was uns anfällig [phishing attack](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)für einen Phishingangriff ist. Ein Angreifer könnte z. b. böswillige e-Mails an Website Benutzer senden, um die Kenn Wörter zu erfassen. Sehen wir uns an, wie dies auf der Seite "nerddinner" funktioniert. (Beachten Sie, dass die Live-Seite "nerddinner" aktualisiert wurde, um Schutz vor offenen Umleitungen zu bieten.)

Zuerst sendet ein Angreifer einen Link zur Anmeldeseite in "nerddinner", die eine Umleitung an die zugehörige gefälschte Seite enthält:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Beachten Sie, dass die Rückgabe-URL auf nerddiner.com zeigt, dem ein "n" aus dem Word Dinner fehlt. In diesem Beispiel ist dies eine Domäne, die der Angreifer steuert. Wenn wir auf den obigen Link zugreifen, werden wir zur legitimen NerdDinner.com-Anmeldeseite weitergeleitet.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Abbildung 02**: Nerddinner Login-Seite mit offener Umleitung

Wenn wir uns ordnungsgemäß anmelden, werden wir durch die Anmelde Aktion ASP.NET MVC AccountController zur URL umgeleitet, die im Parameter "QueryString" von "wiederholnurl" angegeben ist. In diesem Fall ist dies die URL, die der Angreifer eingegeben hat, was [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn)ist. Sofern wir nicht sehr wachsam sind, ist es sehr wahrscheinlich, dass dies nicht der Fall ist, insbesondere weil der Angreifer sicher ist, dass die gefälschte Seite genau wie die berechtigte Anmeldeseite aussieht. Diese Anmeldeseite enthält eine Fehlermeldung, die anfordert, dass Sie sich erneut anmelden. Ungeschickt, wir müssen unser Kennwort falsch formatierten.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Abbildung 03**: Gefälschter nerddinner Login-Bildschirm

Wenn wir den Benutzernamen und das Kennwort erneut eingeben, speichert die gefälschte Anmeldeseite die Informationen und sendet Sie zurück an die berechtigte NerdDinner.com-Website. An diesem Punkt hat die Website "NerdDinner.com" bereits authentifiziert, sodass die gefälschte Anmeldeseite direkt zu dieser Seite umgeleitet werden kann. Das Endergebnis ist, dass der Angreifer über den Benutzernamen und das Kennwort verfügt, und wir wissen, dass wir ihn Ihnen nicht zur Verfügung gestellt haben.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Überprüfen des anfälligen Codes in der AccountController-Anmelde Aktion

Der Code für die Anmelde Aktion in einer ASP.NET MVC 2-Anwendung ist unten dargestellt. Beachten Sie, dass der Controller bei erfolgreicher Anmeldung eine Umleitung an die Rücksende Liste zurückgibt. Sie können sehen, dass keine Überprüfung für den Parameter "Rückkehrer" ausgeführt wird.

**Codebeispiel 1 – ASP.NET MVC 2-Anmelde Aktion in `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Sehen wir uns nun die Änderungen an der Anmelde Aktion ASP.NET MVC 3 an. Dieser Code wurde geändert, um den "Rückkehrer"-Parameter zu validieren, indem er eine neue Methode in der Hilfsprogrammklasse "System. Web. MVC. URL" namens "`IsLocalUrl()`" aufruft.

**Codebeispiel 2 – ASP.NET MVC 3-Anmelde Aktion in `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Dies wurde geändert, um den Rückgabe-URL-Parameter zu validieren, indem eine neue Methode in der Hilfsklasse "System. Web. MVC. URL" aufgerufen wird, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Schützen Ihrer ASP.NET MVC 1,0-und MVC 2-Anwendungen

Wir können die ASP.NET MVC 3-Änderungen in unseren vorhandenen ASP.NET MVC 1,0-und 2-Anwendungen nutzen, indem Sie die Hilfsmethode "islocalurl ()" hinzufügen und die Anmelde Aktion aktualisieren, um den Parameter "Rückkehrer" zu validieren.

Die Urlhelper islocalurl ()-Methode ruft tatsächlich nur eine Methode in System. Web. Webseiten auf, da diese Validierung auch von ASP.net Web Pages Anwendungen verwendet wird.

**Codebeispiel 3 – die islocalurl ()-Methode aus dem ASP.NET MVC 3 Urlhelper-`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Die isurllocaltohost-Methode enthält die tatsächliche Validierungs Logik, wie in der Abbildung 4 gezeigt.

**Codebeispiel 4 – isurllocaltohost ()-Methode aus der System. Web. Webseiten requestextensions-Klasse**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

In unserer ASP.NET MVC 1,0-oder 2-Anwendung fügen wir dem AccountController eine islocalurl ()-Methode hinzu. es wird jedoch empfohlen, Sie nach Möglichkeit zu einer separaten Hilfsklasse hinzuzufügen. Wir nehmen zwei kleine Änderungen an der ASP.NET MVC 3-Version von islocalurl () vor, damit Sie in AccountController verwendet werden kann. Zunächst ändern wir die Methode von einer öffentlichen Methode in eine private Methode, da auf öffentliche Methoden in Controllern als Controller Aktionen zugegriffen werden kann. Zweitens ändern wir den-Befehl, mit dem der URL-Host anhand des Anwendungs Hosts überprüft wird. Dieser Befehl nutzt ein lokales RequestContext-Feld in der Urlhelper-Klasse. Anstatt diese zu verwenden. RequestContext. HttpContext. Request. URL. Host verwenden wir dies. Request. URL. Host. Der folgende Code zeigt die geänderte islocalurl ()-Methode für die Verwendung mit einer Controller Klasse in ASP.NET MVC 1,0-und 2-Anwendungen.

**Listing 5 – islocalurl ()-Methode, die für die Verwendung mit einer MVC-Controller Klasse geändert wird**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Nun, da die islocalurl ()-Methode vorhanden ist, können wir Sie aus der Anmelde Aktion aufrufen, um den Parameter "Rückkehrer" zu validieren, wie im folgenden Code gezeigt.

**Codebeispiel 6 – aktualisierte Anmelde Methode, die den Parameter "kehrnurl" überprüft**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Nun können Sie einen offenen Umleitungs Angriff testen, indem Sie versuchen, sich mit einer externen Rückgabe-URL anzumelden. Verwenden wir/Account/LOGON? Wieder kehrnurl =<http://www.bing.com/>.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Abbildung 04**: Testen der aktualisierten Anmelde Aktion

Nach der erfolgreichen Anmeldung werden wir zur Aktion "Home/Index Controller" anstatt zur externen URL umgeleitet.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Abbildung 05**: Offener Umleitungs Angriff besiegt

## <a name="summary"></a>Zusammenfassung

Open Redirect-Angriffe können auftreten, wenn Umleitungs-URLs als Parameter in der URL für eine Anwendung weitergegeben werden. Die ASP.NET MVC 3-Vorlage enthält Code zum Schutz vor offenen Umleitungs Angriffen. Sie können diesen Code mit einigen Änderungen an ASP.NET MVC 1,0 und 2-Anwendungen hinzufügen. Fügen Sie zum Schutz vor offenen Umleitungs Angriffen bei der Anmeldung bei ASP.NET 1,0-und 2-Anwendungen eine islocalurl ()-Methode hinzu, und überprüfen Sie den Parameter "Rückkehrer" in der Anmelde Aktion.
