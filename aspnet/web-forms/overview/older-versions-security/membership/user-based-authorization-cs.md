---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Benutzerbasierte Autorisierung (C#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erfahren Sie, wie Sie den Zugriff auf Seiten einschränken und Funktionen auf Seitenebene durch eine Vielzahl von Verfahren einschränken.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 059dbf42956268884dcfdade696491ac39e32da9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463587"
---
# <a name="user-based-authorization-c"></a>Benutzerbasierte Autorisierung (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> In diesem Tutorial erfahren Sie, wie Sie den Zugriff auf Seiten einschränken und Funktionen auf Seitenebene durch eine Vielzahl von Verfahren einschränken.

## <a name="introduction"></a>Einführung

Die meisten Webanwendungen, die Benutzerkonten anbieten, sind so Teil, um bestimmte Besucher daran zu hindern, auf bestimmte Seiten innerhalb der Site zuzugreifen. In den meisten Online-Messageboard-Sites können z. b. alle Benutzer anonym und authentifiziert die Beiträge des Messageboards anzeigen, aber nur authentifizierte Benutzer können die Webseite besuchen, um einen neuen Beitrag zu erstellen. Es können auch Verwaltungs Seiten vorhanden sein, auf die nur ein bestimmter Benutzer (oder eine bestimmte Gruppe von Benutzern) zugreifen kann. Darüber hinaus können Funktionen auf Seitenebene von Benutzer zu Benutzer-Basis abweichen. Beim Anzeigen einer Liste von Beiträgen wird den authentifizierten Benutzern eine Schnittstelle für die Bewertung der einzelnen Beiträge angezeigt, während diese Schnittstelle für anonyme Besucher nicht verfügbar ist.

ASP.NET vereinfacht die Definition Benutzer basierter Autorisierungs Regeln. Mit nur einem Markup in `Web.config`können bestimmte Webseiten oder gesamte Verzeichnisse gesperrt werden, sodass nur eine bestimmte Teilmenge von Benutzern zugänglich ist. Funktionen auf Seitenebene können basierend auf dem aktuell angemeldeten Benutzer durch programmgesteuerte und deklarative Mittel aktiviert oder deaktiviert werden.

In diesem Tutorial erfahren Sie, wie Sie den Zugriff auf Seiten einschränken und Funktionen auf Seitenebene durch eine Vielzahl von Verfahren einschränken. Erste Schritte

## <a name="a-look-at-the-url-authorization-workflow"></a>Ein Blick auf den URL-Autorisierungs Workflow

Wie in der Übersicht über das Tutorial zur [*Formular Authentifizierung*](../introduction/an-overview-of-forms-authentication-cs.md) erläutert, werden bei der Verarbeitung einer Anforderung für eine ASP.NET-Ressource die Anforderung eine Reihe von Ereignissen während des Lebenszyklus auslöst. *HTTP-Module* sind verwaltete Klassen, deren Code als Reaktion auf ein bestimmtes Ereignis im Anforderungs Lebenszyklus ausgeführt wird. ASP.net wird mit einer Reihe von HTTP-Modulen ausgeliefert, die wichtige Aufgaben im Hintergrund ausführen.

Ein solches HTTP-Modul ist [`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Wie in den vorherigen Tutorials erläutert, besteht die primäre Funktion des `FormsAuthenticationModule` darin, die Identität der aktuellen Anforderung zu bestimmen. Dies wird erreicht, indem das Formular Authentifizierungs Ticket überprüft wird, das sich entweder in einem Cookie befindet oder in die URL eingebettet ist. Diese Identifikation findet während des [`AuthenticateRequest` Ereignisses](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)statt.

Ein weiteres wichtiges HTTP-Modul ist die [`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), die als Reaktion auf das [`AuthorizeRequest`-Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) ausgelöst wird (das nach dem `AuthenticateRequest`-Ereignis erfolgt). Der `UrlAuthorizationModule` überprüft das Konfigurations Markup in `Web.config`, um zu bestimmen, ob die aktuelle Identität über die Berechtigung verfügt, die angegebene Seite zu besuchen. Dieser Prozess wird als URL- *Autorisierung*bezeichnet.

Wir untersuchen die Syntax für die URL-Autorisierungs Regeln in Schritt 1, aber zunächst sehen wir uns an, was die `UrlAuthorizationModule` hat, je nachdem, ob die Anforderung autorisiert ist. Wenn die `UrlAuthorizationModule` feststellt, dass die Anforderung autorisiert ist, wird keine Aktion durchgeführt, und die Anforderung wird durch den Lebenszyklus der Anforderung fortgesetzt. Wenn die Anforderung jedoch *nicht* autorisiert ist, bricht der `UrlAuthorizationModule` den Lebenszyklus ab und weist das `Response` Objekt an, den Status " [HTTP 401 nicht autorisiert](http://www.checkupdown.com/status/E401.html) " zurückzugeben. Wenn Sie die Formular Authentifizierung verwenden, wird dieser HTTP 401-Status niemals an den Client zurückgegeben, denn wenn die `FormsAuthenticationModule` einen 401 HTTP-Status erkennt, wird der HTTP-Status in [HTTP 302](http://www.checkupdown.com/status/E302.html) an die Anmeldeseite umgeleitet.

Abbildung 1 veranschaulicht den Workflow der ASP.NET-Pipeline, die `FormsAuthenticationModule`und die `UrlAuthorizationModule`, wenn eine nicht autorisierte Anforderung eingeht. In Abbildung 1 wird insbesondere eine Anforderung eines anonymen Besuchers für `ProtectedPage.aspx`angezeigt. dabei handelt es sich um eine Seite, die den Zugriff auf anonyme Benutzer verweigert. Da der Besucher anonym ist, wird die Anforderung vom `UrlAuthorizationModule` abgebrochen, und es wird der Status "http 401 nicht autorisiert" zurückgegeben. Anschließend konvertiert der `FormsAuthenticationModule` den Status 401 in eine 302-Umleitung zur Anmeldeseite. Nachdem der Benutzer über die Anmeldeseite authentifiziert wurde, wird er zu `ProtectedPage.aspx`umgeleitet. Dieses Mal identifiziert der `FormsAuthenticationModule` den Benutzer basierend auf seinem Authentifizierungs Ticket. Nachdem der Besucher authentifiziert wurde, ermöglicht das `UrlAuthorizationModule` den Zugriff auf die Seite.

[![des Workflows zur Formular Authentifizierung und URL-Autorisierung](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Abbildung 1**: der Workflow zur Formular Authentifizierung und URL-Autorisierung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image3.png))

Abbildung 1 zeigt die Interaktion, die auftritt, wenn ein anonymer Besucher versucht, auf eine Ressource zuzugreifen, die für anonyme Benutzer nicht verfügbar ist. In einem solchen Fall wird der anonyme Besucher an die Anmeldeseite umgeleitet, auf der die von ihm besuchte Seite in der Abfrage Zeichenfolge angegeben wurde. Nachdem sich der Benutzer erfolgreich angemeldet hat, wird er automatisch an die Ressource umgeleitet, die er anfänglich versucht hat.

Wenn die nicht autorisierte Anforderung von einem anonymen Benutzer durchgeführt wird, ist dieser Workflow unkompliziert und für den Besucher leicht zu verstehen, was passiert ist und warum. Beachten Sie jedoch, dass der `FormsAuthenticationModule` *alle nicht* autorisierten Benutzer auf die Anmeldeseite umleitet, selbst wenn die Anforderung von einem authentifizierten Benutzer gestellt wird. Dies kann zu einer verwirrenden Benutzer Darstellung führen, wenn ein authentifizierter Benutzer versucht, eine Seite zu besuchen, für die er nicht über die Berechtigung verfügt.

Stellen Sie sich vor, dass die URL-Autorisierungs Regeln für unsere Website so konfiguriert wurden, dass die ASP.NET-Seite `OnlyTito.aspx` nur in Tito zugänglich war. Stellen Sie sich nun vor, dass Sam die Website besucht, sich anmeldet und dann versucht, `OnlyTito.aspx`zu besuchen. Der `UrlAuthorizationModule` hält den Anforderungs Lebenszyklus an und gibt den Status "http 401 nicht autorisiert" zurück, der vom `FormsAuthenticationModule` erkannt und dann Sam an die Anmeldeseite umgeleitet wird. Da Sam sich bereits angemeldet hat, kann er sich jedoch Fragen, warum er zurück an die Anmeldeseite gesendet wurde. Dies könnte daran liegen, dass Ihre Anmelde Informationen irgendwie verloren gegangen sind oder dass Sie ungültige Anmelde Informationen eingegeben hat. Wenn Sam Ihre Anmelde Informationen von der Anmeldeseite wieder eingibt, wird Sie angemeldet (erneut) und an `OnlyTito.aspx`umgeleitet. Die `UrlAuthorizationModule` erkennt, dass Sam diese Seite nicht besuchen kann, und Sie wird zur Anmeldeseite zurückgegeben.

Abbildung 2 zeigt diesen verwirrenden Workflow.

[![der Standard Workflow zu einem verwirrenden Durchlauf führen kann](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Abbildung 2**: der Standard Workflow kann zu einem verwirrenden Durchlauf führen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image6.png))

Der in Abbildung 2 dargestellte Workflow kann den meisten Computer versierten Besucher schnell verwirren. Wir sehen uns die Möglichkeiten an, diesen verwirrenden Zeitraum in Schritt 2 zu vermeiden.

> [!NOTE]
> ASP.NET verwendet zwei Mechanismen, um zu bestimmen, ob der aktuelle Benutzer auf eine bestimmte Webseite zugreifen kann: URL-Autorisierung und Datei Autorisierung. Die Datei Autorisierung wird durch das [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx)implementiert, das die Autorität bestimmt, indem die angeforderten Datei-ACLs konsultiert werden. Die Datei Autorisierung wird am häufigsten bei der Windows-Authentifizierung verwendet, da ACLs Berechtigungen sind, die für Windows-Konten gelten. Wenn Sie die Formular Authentifizierung verwenden, werden alle Anforderungen an das Betriebssystem und die Datei auf Systemebene vom gleichen Windows-Konto ausgeführt, unabhängig vom Benutzer, der die Website besucht. Da sich diese tutorialreihe auf die Formular Authentifizierung konzentriert, wird die Datei Autorisierung nicht erörtert.

### <a name="the-scope-of-url-authorization"></a>Gültigkeitsbereich der URL-Autorisierung

Der `UrlAuthorizationModule` ist verwalteter Code, der Teil der ASP.NET-Laufzeit ist. Vor Version 7 des Microsoft- [Internetinformationsdienste (IIS)](https://www.iis.net/) -Webservers gab es eine eindeutige Barriere zwischen der HTTP-Pipeline von IIS und der ASP.net-Lauf Zeit Pipeline. Kurz gesagt: in IIS 6 und früher ASP. Die `UrlAuthorizationModule` des Netzes wird nur ausgeführt, wenn eine Anforderung von IIS an die ASP.NET-Laufzeit delegiert wird. Standardmäßig verarbeitet IIS statische Inhalte selbst, z. b. HTML-Seiten und CSS-, JavaScript-und Bilddateien und übergibt nur Anforderungen an die ASP.NET-Laufzeit, wenn eine Seite mit einer Erweiterung von `.aspx`, `.asmx`oder `.ashx` angefordert wird.

IIS 7 ermöglicht jedoch integrierte IIS-und ASP.net-Pipelines. Mit einigen Konfigurationseinstellungen können Sie IIS 7 einrichten, um die `UrlAuthorizationModule` für *alle* Anforderungen aufzurufen. Dies bedeutet, dass URL-Autorisierungs Regeln für Dateien eines beliebigen Typs definiert werden können. Darüber hinaus enthält IIS 7 eine eigene URL-Autorisierungs-Engine. Weitere Informationen zur ASP.NET-Integration und zur nativen URL-Autorisierungs Funktion von IIS 7 finden Sie unter [Understanding IIS7 URL Authorization](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Eine ausführlichere Betrachtung der ASP.net-und IIS 7-Integration erhalten Sie, wenn Sie eine Kopie des Buchs von Shahram Khosravi, *Professional IIS 7 und ASP.NET Integrated Programming* (ISBN: 978-0470152539), abrufen.

Kurz gesagt: in Versionen vor IIS 7 gelten URL-Autorisierungs Regeln nur für Ressourcen, die von der ASP.NET-Laufzeit behandelt werden. Aber mit IIS 7 ist es möglich, das Native URL-Autorisierungs Feature von IIS zu verwenden oder ASP zu integrieren. NET ist `UrlAuthorizationModule` in die HTTP-Pipeline von IIS, um diese Funktionalität auf alle Anforderungen auszuweiten.

> [!NOTE]
> Es gibt einige feine, aber wichtige Unterschiede bei der Verwendung von ASP. Der `UrlAuthorizationModule` von .net und die URL-Autorisierungs Funktion von IIS 7 verarbeiten die Autorisierungs Regeln. In diesem Tutorial werden die URL-Autorisierungs Funktionen von IIS 7 oder die Unterschiede beim Analysieren von Autorisierungs Regeln im Vergleich zum `UrlAuthorizationModule`nicht untersucht. Weitere Informationen zu diesen Themen finden Sie in der IIS 7-Dokumentation auf MSDN oder unter [www.IIS.net](https://www.iis.net/).

## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Schritt 1: Definieren von URL-Autorisierungs Regeln in`Web.config`

Der `UrlAuthorizationModule` bestimmt, ob der Zugriff auf eine angeforderte Ressource für eine bestimmte Identität basierend auf den in der Konfiguration der Anwendung definierten URL-Autorisierungs Regeln gewährt oder verweigert wird. Die Autorisierungs Regeln werden im`<authorization>`- [Element](https://msdn.microsoft.com/library/8d82143t.aspx) in Form von `<allow>` und `<deny>` untergeordneten Elementen ausgeschrieben. Jedes `<allow>` und `<deny>` untergeordnete Element kann Folgendes angeben:

- Ein bestimmter Benutzer
- Eine durch Trennzeichen getrennte Liste von Benutzern
- Alle anonymen Benutzer, die durch ein Fragezeichen (?) gekennzeichnet sind
- Alle Benutzer, angegeben durch ein Sternchen (\*)

Das folgende Markup veranschaulicht, wie die URL-Autorisierungs Regeln verwendet werden, um Benutzern Tito und Scott zu gestatten und alle anderen abzulehnen:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

Das `<allow>`-Element definiert, welche Benutzer zulässig sind (Tito und Scott), während das `<deny>`-Element anweist, dass *alle* Benutzer verweigert werden.

> [!NOTE]
> Mit den `<allow>`-und `<deny>` Elementen können auch Autorisierungs Regeln für Rollen angegeben werden. Wir werden die rollenbasierte Autorisierung in einem zukünftigen Tutorial untersuchen.

Die folgende Einstellung gewährt allen anderen Benutzern Zugriff auf Sam (einschließlich anonymer Besucher):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Um nur authentifizierte Benutzer zuzulassen, verwenden Sie die folgende Konfiguration, die den Zugriff auf alle anonymen Benutzer verweigert:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Die Autorisierungs Regeln werden innerhalb des `<system.web>`-Elements in `Web.config` definiert und gelten für alle ASP.NET-Ressourcen in der Webanwendung. Oft hat eine Anwendung verschiedene Autorisierungs Regeln für verschiedene Abschnitte. Beispielsweise können alle Besucher an einer e-Commerce-Website die Produkte nutzen, Informationen zu Produkten anzeigen, den Katalog durchsuchen usw. Allerdings können nur authentifizierte Benutzer das Auschecken oder die Seiten erreichen, um den Versand Verlauf eines einzelnen zu verwalten. Darüber hinaus gibt es möglicherweise Teile der Website, auf die nur ausgewählte Benutzer zugreifen können, z. b. Website Administratoren.

ASP.NET erleichtert das Definieren verschiedener Autorisierungs Regeln für verschiedene Dateien und Ordner auf der Website. Die in der `Web.config` Datei des Stamm Ordners angegebenen Autorisierungs Regeln gelten für alle ASP.NET-Ressourcen des Standorts. Diese Standard Autorisierungs Einstellungen können jedoch für einen bestimmten Ordner überschrieben werden, indem ein `Web.config` mit einem `<authorization>` Abschnitt hinzugefügt wird.

Wir aktualisieren unsere Website so, dass nur authentifizierte Benutzer die ASP.NET-Seiten im Ordner "`Membership`" besuchen können. Um dies zu erreichen, müssen wir dem Ordner "`Membership`" eine `Web.config`-Datei hinzufügen und die Autorisierungs Einstellungen so festlegen, dass anonyme Benutzer verweigert werden. Klicken Sie mit der rechten Maustaste auf den Ordner `Membership` im Projektmappen-Explorer, wählen Sie im Kontextmenü das Menü neues Element hinzufügen aus, und fügen Sie eine neue Webkonfigurationsdatei mit dem Namen `Web.config`hinzu.

[![dem Mitgliedschafts Ordner eine Web. config-Datei hinzufügen](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Abbildung 3**: Hinzufügen einer `Web.config` Datei zum Ordner "`Membership`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image9.png))

An diesem Punkt sollte das Projekt zwei `Web.config` Dateien enthalten: eine im Stammverzeichnis und eine im Ordner "`Membership`".

[![Ihre Anwendung nun zwei Web. config-Dateien enthalten soll.](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Abbildung 4**: Ihre Anwendung sollte nun zwei `Web.config` Dateien enthalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image12.png))

Aktualisieren Sie die Konfigurationsdatei im Ordner "`Membership`", damit Sie den Zugriff auf anonyme Benutzer untersagt.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

Das ist schon alles!

Um diese Änderung zu testen, besuchen Sie die Homepage in einem Browser, und stellen Sie sicher, dass Sie sich abgemeldet haben. Da das Standardverhalten einer ASP.NET-Anwendung darin besteht, alle Besucher zuzulassen, und da wir keine Autorisierungs Änderungen an der `Web.config` Datei des Stamm Verzeichnisses vorgenommen haben, können wir die Dateien im Stammverzeichnis als anonymer Besucher besuchen.

Klicken Sie in der linken Spalte auf den Link Benutzerkonten erstellen. Dadurch gelangen Sie zum `~/Membership/CreatingUserAccounts.aspx`. Da die `Web.config` Datei im Ordner "`Membership`" Autorisierungs Regeln definiert, um den anonymen Zugriff zu verhindern, bricht der `UrlAuthorizationModule` die Anforderung ab und gibt den Status "http 401 nicht autorisiert" zurück. Der `FormsAuthenticationModule` ändert diesen in einen 302-Umleitungs Status, der uns an die Anmeldeseite sendet. Beachten Sie, dass die Seite, auf die wir zugreifen wollten (`CreatingUserAccounts.aspx`), über den `ReturnUrl` QueryString-Parameter an die Anmeldeseite übergeben wird.

[![da die URL-Autorisierungs Regeln den anonymen Zugriff verbieten, werden wir zur Anmeldeseite umgeleitet.](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Abbildung 5**: da die URL-Autorisierungs Regeln den anonymen Zugriff verbieten, werden wir zur Anmeldeseite umgeleitet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image15.png)).

Nach erfolgreicher Anmeldung werden wir auf die Seite "`CreatingUserAccounts.aspx`" umgeleitet. Dieses Mal ermöglicht das `UrlAuthorizationModule` den Zugriff auf die Seite, da wir nicht mehr anonym sind.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Anwenden von URL-Autorisierungs Regeln auf einen bestimmten Speicherort

Die im `<system.web>` Abschnitt von `Web.config` definierten Autorisierungs Einstellungen gelten für alle ASP.NET-Ressourcen in diesem Verzeichnis und seinen Unterverzeichnissen (bis anderweitig von einer anderen `Web.config` Datei überschrieben). In einigen Fällen kann es jedoch sein, dass alle ASP.NET-Ressourcen in einem bestimmten Verzeichnis über eine bestimmte Autorisierungs Konfiguration verfügen, außer für eine oder zwei bestimmte Seiten. Dies kann erreicht werden, indem Sie in `Web.config`ein `<location>` Element hinzufügen, auf die Datei verweisen, deren Autorisierungs Regeln voneinander abweichen, und die darin enthaltenen eindeutigen Autorisierungs Regeln definieren.

Um die Verwendung des `<location>`-Elements zum Überschreiben der Konfigurationseinstellungen für eine bestimmte Ressource zu veranschaulichen, passen Sie die Autorisierungs Einstellungen so an, dass nur Tito `CreatingUserAccounts.aspx`besuchen kann. Fügen Sie hierzu der `Web.config` Datei des `Membership` Ordners ein `<location>`-Element hinzu, und aktualisieren Sie das zugehörige Markup so, dass es wie folgt aussieht:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

Das `<authorization>`-Element in `<system.web>` definiert die Standard-URL-Autorisierungs Regeln für ASP.NET-Ressourcen im `Membership`-Ordner und seinen Unterordnern. Mit dem `<location>`-Element können diese Regeln für eine bestimmte Ressource überschrieben werden. Im obigen Markup verweist das `<location>` Element auf die `CreatingUserAccounts.aspx` Seite und gibt seine Autorisierungs Regeln an, z. b., um Tito zuzulassen, aber alle anderen abzulehnen.

Um diese Autorisierungs Änderung zu testen, besuchen Sie zunächst die Website als anonymer Benutzer. Wenn Sie versuchen, eine beliebige Seite im `Membership` Ordner (z. b. `UserBasedAuthorization.aspx`) aufzurufen, wird die Anforderung vom `UrlAuthorizationModule` verweigert, und Sie werden zur Anmeldeseite umgeleitet. Wenn Sie sich als, z.b. Scott, anmelden, können Sie eine beliebige Seite im Ordner "`Membership`" aufrufen, *mit Ausnahme* von `CreatingUserAccounts.aspx`. Wenn Sie versuchen, `CreatingUserAccounts.aspx`, die als Person, aber Tito angemeldet sind, zu besuchen, führt dies zu einem nicht autorisierten Zugriffs Versuch, der Sie zur Anmeldeseite umleitet.

> [!NOTE]
> Das `<location>` Element muss außerhalb des `<system.web>` Elements der Konfiguration angezeigt werden. Sie müssen für jede Ressource, deren Autorisierungs Einstellungen Sie außer Kraft setzen möchten, ein separates `<location>`-Element verwenden.

### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Es wird erläutert, wie das`UrlAuthorizationModule`die Autorisierungs Regeln verwendet, um den Zugriff zu gewähren oder zu verweigern.

Der `UrlAuthorizationModule` bestimmt, ob eine bestimmte Identität für eine bestimmte URL autorisiert werden soll, indem die URL-Autorisierungs Regeln nacheinander analysiert werden, beginnend mit dem ersten und durchlaufen. Sobald eine Entsprechung gefunden wird, wird dem Benutzer der Zugriff gewährt oder verweigert, abhängig davon, ob die Entsprechung in einem `<allow>` oder `<deny>` Element gefunden wurde. <strong>Wenn keine Entsprechung gefunden wird, wird dem Benutzer der Zugriff gewährt.</strong> Wenn Sie den Zugriff einschränken möchten, müssen Sie daher unbedingt ein `<deny>`-Element als letztes Element in der URL-Autorisierungs Konfiguration verwenden. <strong>Wenn Sie ein</strong> <strong>`<deny>`</strong>-Element weglassen <strong>, wird allen Benutzern der Zugriff gewährt.</strong>

Zum besseren Verständnis des Prozesses, der vom `UrlAuthorizationModule` zur Bestimmung der Zertifizierungsstelle verwendet wird, sollten Sie sich die Beispiel-URL-Autorisierungs Regeln ansehen, die wir zuvor in diesem Schritt Die erste Regel ist ein `<allow>` Element, das den Zugriff auf Tito und Scott ermöglicht. Die zweite Regel ist ein `<deny>` Element, das allen Benutzern den Zugriff verweigert. Wenn ein anonymer Benutzer besucht, wird die `UrlAuthorizationModule` mit der Frage, ob anonym entweder Scott oder Tito ist? Die Antwort ist offensichtlich Nein, sodass Sie mit der zweiten Regel fort fährt. Ist im Satz aller anonym? Da die Antwort hier "Ja" lautet, wird die `<deny>` Regel wirksam, und der Besucher wird an die Anmeldeseite umgeleitet. Wenn jisun beispielsweise besucht wird, wird der `UrlAuthorizationModule` zunächst gefragt, ob jisun entweder Scott oder Tito ist? Da Sie nicht ist, geht der `UrlAuthorizationModule` zur zweiten Frage, ist jisun im Satz von allen? Sie ist, dass Ihnen auch der Zugriff verweigert wird. Schließlich ist die erste Frage, die von der `UrlAuthorizationModule` gestellt wird, eine positive Antwort, damit Tito Zugriff erhält.

Da der `UrlAuthorizationModule` die Autorisierungs Regeln von oben nach unten verarbeitet und bei jeder beliebigen Entsprechung anhält, ist es wichtig, dass die spezifischeren Regeln vor den weniger spezifischen Regeln stehen. Das heißt, um Autorisierungs Regeln zu definieren, die jisun und anonyme Benutzer verbieten, aber alle anderen authentifizierten Benutzer zulassen, beginnen Sie mit der spezifischsten Regel, die sich auf jisun auswirkt, und fahren Sie dann mit den weniger spezifischen Regeln fort, die alle anderen authentifizierte Benutzer, verweigern aller anonymen Benutzer. Die folgenden URL-Autorisierungs Regeln implementieren diese Richtlinie, indem Sie zuerst jisun verweigern und dann allen anonymen Benutzern verweigern. Alle anderen authentifizierten Benutzer als jisun erhalten Zugriff, da keine dieser `<deny>` Anweisungen abgleichen wird.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Schritt 2: Reparieren des Workflows für nicht autorisierte authentifizierte Benutzer

Wie bereits weiter oben in diesem Tutorial im Abschnitt "betrachten des URL-Autorisierungs Workflows" erläutert, wird die Anforderung vom `UrlAuthorizationModule` abgebrochen und der Status "http 401 nicht autorisiert" zurückgegeben, wenn eine nicht autorisierte Anforderung auftritt. Dieser 401-Status wird vom `FormsAuthenticationModule` in einen 302-Umleitungs Status geändert, der den Benutzer an die Anmeldeseite sendet. Dieser Workflow tritt bei allen nicht autorisierten Anforderungen auf, auch wenn der Benutzer authentifiziert ist.

Wenn Sie einen authentifizierten Benutzer an die Anmeldeseite zurückgeben, werden diese wahrscheinlich verwechselt, da Sie sich bereits beim System angemeldet haben. Mit etwas Arbeit können wir diesen Workflow verbessern, indem Sie authentifizierte Benutzer, die nicht autorisierte Anforderungen ausführen, an eine Seite weiterleiten, die darauf angreift, dass Sie versucht haben, auf eine eingeschränkte Seite zuzugreifen.

Beginnen Sie, indem Sie im Stamm Ordner der Webanwendung mit dem Namen `UnauthorizedAccess.aspx`eine neue ASP.NET-Seite erstellen. vergessen Sie nicht, diese Seite der `Site.master` Master Seite zuzuordnen. Nachdem Sie diese Seite erstellt haben, entfernen Sie das Inhalts Steuerelement, das auf die `LoginContent` contentplachalter verweist, sodass der Standard Inhalt der Master Seite angezeigt wird. Fügen Sie als nächstes eine Meldung hinzu, in der die Situation erläutert wird, nämlich, dass der Benutzer versucht hat, auf eine geschützte Ressource zuzugreifen. Nach dem Hinzufügen einer solchen Nachricht sollte das deklarative Markup der `UnauthorizedAccess.aspx` Seite in etwa wie folgt aussehen:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Wir müssen den Workflow jetzt ändern, damit eine nicht autorisierte Anforderung von einem authentifizierten Benutzer an die `UnauthorizedAccess.aspx` Seite anstatt an die Anmeldeseite gesendet wird. Die Logik, die nicht autorisierte Anforderungen an die Anmeldeseite umleitet, wird in einer privaten Methode der `FormsAuthenticationModule`-Klasse verborgen, sodass dieses Verhalten nicht angepasst werden kann. Wir können jedoch unserer Anmeldeseite unsere eigene Logik hinzufügen, die den Benutzer bei Bedarf an `UnauthorizedAccess.aspx`umleitet.

Wenn der `FormsAuthenticationModule` einen nicht autorisierten Besucher an die Anmeldeseite umleitet, fügt er die angeforderte, nicht autorisierte URL an die QueryString mit dem Namen `ReturnUrl`an. Wenn z. b. ein nicht autorisierter Benutzer versucht hat, `OnlyTito.aspx`zu besuchen, werden die `FormsAuthenticationModule` Sie zu `Login.aspx?ReturnUrl=OnlyTito.aspx`umleiten. Wenn die Anmeldeseite durch einen authentifizierten Benutzer mit einer QueryString erreicht wird, die den `ReturnUrl`-Parameter enthält, dann wissen wir, dass dieser nicht authentifizierte Benutzer nur versucht hat, eine Seite zu besuchen, die nicht zum Anzeigen autorisiert ist. In diesem Fall möchten wir Sie an `UnauthorizedAccess.aspx`umleiten.

Fügen Sie hierzu den folgenden Code zum `Page_Load` Ereignishandler der Anmeldeseite hinzu:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Der obige Code leitet authentifizierte, nicht autorisierte Benutzer an die `UnauthorizedAccess.aspx` Seite um. Um diese Logik in Aktion zu sehen, besuchen Sie die Website als anonymer Besucher, und klicken Sie in der linken Spalte auf den Link Erstellen von Benutzerkonten. Dadurch gelangen Sie zu der Seite "`~/Membership/CreatingUserAccounts.aspx`", die Sie in Schritt 1 so konfiguriert haben, dass nur der Zugriff auf Tito zugelassen wird. Da anonyme Benutzer nicht zulässig sind, leitet der `FormsAuthenticationModule` zurück zur Anmeldeseite.

An diesem Punkt sind wir anonym, sodass `Request.IsAuthenticated` `false` zurückgibt und wir nicht an `UnauthorizedAccess.aspx`umgeleitet werden. Stattdessen wird die Anmeldeseite angezeigt. Melden Sie sich als ein anderer Benutzer als Tito an, z. b. "Bruce". Nachdem Sie die entsprechenden Anmelde Informationen eingegeben haben, leitet Sie die Anmeldeseite zurück an `~/Membership/CreatingUserAccounts.aspx`. Da diese Seite jedoch nur für Tito zugänglich ist, sind wir nicht autorisiert, Sie anzuzeigen und werden umgehend zur Anmeldeseite zurückgegeben. Diesmal gibt `Request.IsAuthenticated` jedoch `true` zurück (und der `ReturnUrl` QueryString-Parameter ist vorhanden), sodass wir zur `UnauthorizedAccess.aspx` Seite umgeleitet werden.

[![authentifiziert werden nicht autorisierte Benutzer an "unauthorizedaccess. aspx" umgeleitet.](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Abbildung 6**: authentifizierte, nicht autorisierte Benutzer werden zu `UnauthorizedAccess.aspx` umgeleitet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image18.png))

Dieser angepasste Workflow bietet eine sinnvolle und unkomplizierte Benutzer Darstellung, indem der in Abbildung 2 dargestellte Prozess kurz umschließt wird.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Schritt 3: Einschränken der Funktionalität basierend auf dem aktuell angemeldeten Benutzer

Mit der URL-Autorisierung können Sie ganz einfach grobe Autorisierungs Regeln angeben. Wie in Schritt 1 gezeigt, können wir mit der URL-Autorisierung kurz angeben, welche Identitäten zulässig sind und welche verweigert werden, wenn eine bestimmte Seite oder alle Seiten in einem Ordner angezeigt werden. In bestimmten Szenarien empfiehlt es sich jedoch, allen Benutzern zu gestatten, eine Seite zu besuchen, aber die Funktionalität der Seite auf der Grundlage des von Ihnen besuchten Benutzers einzuschränken.

Betrachten Sie den Fall einer e-Commerce-Website, mit der authentifizierte Besucher Ihre Produkte überprüfen können. Wenn ein anonymer Benutzer auf die Seite eines Produkts zugreift, werden ihm nur die Produktinformationen angezeigt, und es wird nicht die Gelegenheit gegeben, eine Überprüfung zu hinterlassen. Ein authentifizierter Benutzer, der dieselbe Seite besucht, würde jedoch die Überprüfungs Schnittstelle sehen. Wenn der authentifizierte Benutzer dieses Produkt noch nicht überprüft hat, würde die Schnittstelle es Ihnen ermöglichen, einen Review zu übermitteln. Andernfalls würden Sie Ihnen den zuvor übermittelten Review zeigen. Um dieses Szenario noch einen Schritt weiter zu machen, werden auf der Produktseite möglicherweise zusätzliche Informationen angezeigt, und es werden erweiterte Features für die Benutzer angeboten, die für das eCommerce-Unternehmen funktionieren Beispielsweise kann auf der Produktseite die Inventur in der Aktienliste angezeigt werden, um den Preis und die Beschreibung des Produkts zu bearbeiten, wenn Sie von einem Mitarbeiter besucht werden.

Solche differenzierten Autorisierungs Regeln können entweder deklarativ oder Programm gesteuert (oder mithilfe einer Kombination der beiden) implementiert werden. Im nächsten Abschnitt erfahren Sie, wie Sie eine feine granulare Autorisierung über das LoginView-Steuerelement implementieren. Danach werden die programmgesteuerten Verfahren erläutert. Bevor wir uns mit dem Anwenden von Regeln für die differenzierte Autorisierung befassen, müssen wir zunächst eine Seite erstellen, deren Funktionalität vom Benutzer abhängt.

Wir erstellen eine Seite, auf der die Dateien in einem bestimmten Verzeichnis innerhalb einer GridView aufgeführt sind. Zusammen mit der Auflistung von Namen, Größe und anderen Informationen für jede Datei enthält die GridView zwei Spalten von LinkButtons: eine mit der Bezeichnung "View" und eine mit dem Titel "Delete". Wenn auf den Link Button anzeigen geklickt wird, wird der Inhalt der ausgewählten Datei angezeigt. Wenn auf die Schaltfläche "Link löschen" geklickt wird, wird die Datei gelöscht. Zunächst erstellen wir diese Seite so, dass Ihre Ansicht-und Löschfunktionen für alle Benutzer verfügbar sind. Im Abschnitt Verwenden des LoginView-Steuer Elements und des programmgesteuerten Einschränkungs Limits wird beschrieben, wie Sie diese Features basierend auf den Benutzern, die die Seite besuchen, aktivieren oder deaktivieren.

> [!NOTE]
> Auf der ASP.NET Seite, die wir gerade erstellen, wird ein GridView-Steuerelement verwendet, um eine Liste mit Dateien anzuzeigen. Da sich diese tutorialreihe auf Formular Authentifizierung, Autorisierung, Benutzerkonten und Rollen konzentriert, möchte ich nicht zu viel Zeit aufwenden, um die interne Funktionsweise des GridView-Steuer Elements zu erörtern. In diesem Tutorial finden Sie eine ausführliche Anleitung zum Einrichten dieser Seite. es wird jedoch nicht ausführlich erläutert, warum bestimmte Optionen getroffen wurden oder welche Auswirkungen bestimmte Eigenschaften auf die gerenderte Ausgabe haben. Eine gründliche Untersuchung des GridView-Steuer Elements finden Sie in der tutorialreihe *[Arbeiten mit Daten in ASP.NET 2,0](../../data-access/index.md)* .

Öffnen Sie zunächst die Datei `UserBasedAuthorization.aspx` im Ordner `Membership`, und fügen Sie der Seite mit dem Namen `FilesGrid`ein GridView-Steuerelement hinzu. Klicken Sie in der GridView-Smarttag auf den Link Spalten bearbeiten, um das Dialogfeld Felder zu öffnen. Deaktivieren Sie in der linken unteren Ecke das Kontrollkästchen Felder automatisch generieren. Fügen Sie als nächstes eine Schaltfläche auswählen, eine Schaltfläche Löschen und zwei boundfields aus der oberen linken Ecke hinzu (die Schaltflächen auswählen und löschen finden Sie unter dem CommandField-Typ). Legen Sie die `SelectText`-Eigenschaft der Select-Schaltfläche auf View und die Eigenschaften `HeaderText` und `DataField` des ersten BoundField auf Name fest. Legen Sie die `HeaderText`-Eigenschaft des zweiten BoundField auf size in Bytes, die `DataField`-Eigenschaft auf length, die `DataFormatString`-Eigenschaft auf {0:N0} und deren `HtmlEncode`-Eigenschaft auf false fest.

Nachdem Sie die Spalten der GridView konfiguriert haben, klicken Sie auf OK, um das Dialogfeld Felder zu schließen. Legen Sie im Eigenschaftenfenster die `DataKeyNames`-Eigenschaft der GridView auf `FullName`fest. An diesem Punkt sollte das deklarative Markup der GridView wie folgt aussehen:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Wenn das Markup der GridView erstellt wurde, können wir den Code schreiben, der die Dateien in einem bestimmten Verzeichnis abruft und an die GridView bindet. Fügen Sie den folgenden Code zum `Page_Load`-Ereignishandler der Seite hinzu:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Der obige Code verwendet die [`DirectoryInfo`-Klasse](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) , um eine Liste der Dateien im Stamm Ordner der Anwendung abzurufen. Die [`GetFiles()`-Methode](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) gibt alle Dateien im Verzeichnis als Array von [`FileInfo` Objekten](https://msdn.microsoft.com/library/system.io.fileinfo.aspx)zurück, die dann an die GridView gebunden werden. Das `FileInfo`-Objekt verfügt unter anderem über eine Reihe von Eigenschaften, z. b. `Name`, `Length`und `IsReadOnly`. Wie Sie aus dem deklarativen Markup erkennen können, werden in der GridView nur die Eigenschaften "`Name`" und "`Length`" angezeigt.

> [!NOTE]
> Die Klassen `DirectoryInfo` und `FileInfo` befinden sich im [`System.IO`-Namespace](https://msdn.microsoft.com/library/system.io.aspx). Daher müssen Sie diese Klassennamen entweder Ihren Namespace Namen voranstellen oder den Namespace in die Klassendatei importieren lassen (über `using System.IO`).

Nehmen Sie sich einen Moment Zeit, um diese Seite über einen Browser zu besuchen. Dadurch wird die Liste der Dateien angezeigt, die sich im Stammverzeichnis der Anwendung befinden. Wenn Sie auf eine der Schaltflächen "anzeigen" oder "Löschen" klicken, wird ein Postback ausgelöst, aber es erfolgt keine Aktion, da wir noch die erforderlichen Ereignishandler erstellen müssen.

[![die GridView die Dateien im Stammverzeichnis der Webanwendung auflistet.](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Abbildung 7**: die GridView listet die Dateien im Stammverzeichnis der Webanwendung auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image21.png))

Wir benötigen eine Möglichkeit, den Inhalt der ausgewählten Datei anzuzeigen. Kehren Sie zu Visual Studio zurück, und fügen Sie ein Textfeld mit dem Namen `FileContents` oberhalb der GridView hinzu. Legen Sie die `TextMode`-Eigenschaft auf `MultiLine` und deren `Columns` und `Rows` Eigenschaften auf 95% bzw. 10 fest.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Erstellen Sie als nächstes einen Ereignishandler für das [`SelectedIndexChanged` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) der GridView, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

In diesem Code wird die `SelectedValue`-Eigenschaft der GridView verwendet, um den vollständigen Dateinamen der ausgewählten Datei zu bestimmen. Intern wird auf die `DataKeys` Auflistung verwiesen, um die `SelectedValue`abzurufen. Daher ist es zwingend erforderlich, dass Sie die `DataKeyNames`-Eigenschaft der GridView auf "Name" festlegen, wie zuvor in diesem Schritt beschrieben. Die [`File`-Klasse](https://msdn.microsoft.com/library/system.io.file.aspx) wird verwendet, um den Inhalt der ausgewählten Datei in eine Zeichenfolge zu lesen, die dann der `Text`-Eigenschaft des `FileContents`-Textfelds zugewiesen wird. auf diese Weise wird der Inhalt der ausgewählten Datei auf der Seite angezeigt.

[![der Inhalt der ausgewählten Datei im Textfeld angezeigt wird.](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Abbildung 8**: der Inhalt der ausgewählten Datei wird im Textfeld angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image24.png))

> [!NOTE]
> Wenn Sie den Inhalt einer Datei anzeigen, die HTML-Markup enthält, und dann versuchen, eine Datei anzuzeigen oder zu löschen, erhalten Sie einen `HttpRequestValidationException` Fehler. Dies liegt daran, dass beim Postback der Inhalt des Textfelds zurück an den Webserver gesendet wird. Standardmäßig löst ASP.net immer dann einen `HttpRequestValidationException` Fehler aus, wenn potenziell gefährliche Post Back Inhalte erkannt werden, z. b. HTML-Markup. Deaktivieren Sie die Anforderungs Überprüfung für die Seite, indem Sie `ValidateRequest="false"` der `@Page`-Direktive hinzufügen, um diesen Fehler zu deaktivieren. Weitere Informationen zu den Vorteilen der Anforderungs Überprüfung sowie zu den Vorsichtsmaßnahmen, die Sie bei der Deaktivierung treffen sollten, finden Sie unter über [Prüfung der Anforderung-verhindern von Skript Angriffen](https://asp.net/learn/whitepapers/request-validation/).

Fügen Sie abschließend einen Ereignishandler mit dem folgenden Code für das [`RowDeleting`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)der GridView hinzu:

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Der Code zeigt einfach den vollständigen Namen der zu löschenden Datei im Textfeld `FileContents` an, *ohne* die Datei tatsächlich zu löschen.

[durch ![klicken auf die Schaltfläche "Löschen" wird die Datei nicht gelöscht.](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Abbildung 9**: durch Klicken auf die Schaltfläche "Löschen" wird die Datei nicht gelöscht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image27.png))

In Schritt 1 haben wir die URL-Autorisierungs Regeln so konfiguriert, dass anonyme Benutzer die Seiten im `Membership` Ordner nicht anzeigen können. Um die fein Körnung der Granularität zu verbessern, gestatten wir anonymen Benutzern das Besuchen der Seite "`UserBasedAuthorization.aspx`", aber mit eingeschränkter Funktionalität. Um diese Seite zu öffnen, auf der alle Benutzer zugreifen können, fügen Sie der `Web.config`-Datei im `Membership` Ordner Folgendes `<location>`-Element hinzu:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Nachdem Sie dieses `<location>` Element hinzugefügt haben, testen Sie die neuen URL-Autorisierungs Regeln, indem Sie sich von der Website abmelden. Als anonymer Benutzer empfiehlt es sich, die `UserBasedAuthorization.aspx` Seite zu besuchen.

Derzeit können alle authentifizierten oder anonymen Benutzer die `UserBasedAuthorization.aspx` Seite besuchen und Dateien anzeigen oder löschen. Wir legen dies so fest, dass nur authentifizierte Benutzer den Inhalt einer Datei anzeigen können, sodass nur eine Datei von Tito gelöscht werden kann. Solche differenzierten Autorisierungs Regeln können deklarativ, Programm gesteuert oder über eine Kombination beider Methoden angewendet werden. Wir verwenden den deklarativen Ansatz, um einzuschränken, wer den Inhalt einer Datei anzeigen kann. Wir verwenden den programmatischen Ansatz, um einzuschränken, wer eine Datei löschen kann.

### <a name="using-the-loginview-control"></a>Verwenden des LoginView-Steuer Elements

Wie bereits in den vorherigen Tutorials gezeigt, eignet sich das LoginView-Steuerelement zum Anzeigen verschiedener Schnittstellen für authentifizierte und anonyme Benutzer und bietet eine einfache Möglichkeit, die Funktionalität auszublenden, die für anonyme Benutzer nicht zugänglich ist. Da anonyme Benutzer Dateien nicht anzeigen oder löschen können, muss nur das `FileContents` Textfeld angezeigt werden, wenn die Seite von einem authentifizierten Benutzer besucht wird. Um dies zu erreichen, fügen Sie der Seite ein LoginView-Steuerelement hinzu, benennen Sie `LoginViewForFileContentsTextBox`, und verschieben Sie das deklarative Markup des `FileContents` Textfelds in die `LoggedInTemplate`des LoginView-Steuer Elements.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Auf die websteuer Elemente in den Vorlagen von LoginView kann nicht mehr direkt aus der Code Behind-Klasse zugegriffen werden. Beispielsweise verweisen die `SelectedIndexChanged`-und `RowDeleting` Ereignishandler der `FilesGrid` GridView aktuell auf das `FileContents` TextBox-Steuerelement mit Code wie:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Dieser Code ist jedoch nicht mehr gültig. Wenn Sie das Textfeld `FileContents` in den `LoggedInTemplate` verschieben, kann auf das Textfeld nicht direkt zugegriffen werden. Stattdessen müssen wir die `FindControl("controlId")`-Methode verwenden, um Programm gesteuert auf das Steuerelement zu verweisen. Aktualisieren Sie die `FilesGrid` Ereignishandler so, dass Sie wie folgt auf das Textfeld verweisen:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Nachdem Sie das Textfeld in den `LoggedInTemplate` von LoginView verschoben und den Code der Seite aktualisiert haben, um mithilfe des `FindControl("controlId")` Musters auf das Textfeld zu verweisen, besuchen Sie die Seite als anonymer Benutzer. Wie Abbildung 10 zeigt, wird das Textfeld `FileContents` nicht angezeigt. Der Link Button anzeigen wird jedoch weiterhin angezeigt.

[![das LoginView-Steuerelement nur das FileContent-Textfeld für authentifizierte Benutzer rendert.](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Abbildung 10**: das LoginView-Steuerelement rendert nur das Textfeld "`FileContents`" für authentifizierte Benutzer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image30.png))

Eine Möglichkeit zum Ausblenden der Ansichts Schaltfläche für anonyme Benutzer besteht darin, das GridView-Feld in ein TemplateField-Element zu konvertieren. Dadurch wird eine Vorlage generiert, die das deklarative Markup für die Link Schaltfläche "View" enthält. Anschließend können Sie ein LoginView-Steuerelement zum TemplateField hinzufügen und den LinkButton innerhalb der `LoggedInTemplate`von LoginView platzieren. Dadurch wird die Schaltfläche Ansicht von anonymen Besuchern ausgeblendet. Um dies zu erreichen, klicken Sie auf den Link Spalten bearbeiten des Smarttags von GridView, um das Dialogfeld Felder zu öffnen. Wählen Sie anschließend in der Liste in der unteren linken Ecke die Schaltfläche auswählen aus, und klicken Sie dann auf den Link dieses Feld in einen TemplateField-Link konvertieren. Dadurch wird das deklarative Markup des Felds geändert von:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 Nach: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

An diesem Punkt können wir eine LoginView zum TemplateField hinzufügen. Das folgende Markup zeigt den Link Button anzeigen nur für authentifizierte Benutzer an.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Wie in Abbildung 11 dargestellt, ist das Endergebnis nicht so schön, dass die Spalte "View" weiterhin angezeigt wird, auch wenn die Ansicht "LinkButtons" in der Spalte ausgeblendet ist. Im nächsten Abschnitt erfahren Sie, wie Sie die gesamte GridView-Spalte (und nicht nur die LinkButton-Spalte) ausblenden.

[![das LoginView-Steuerelement die Linkschaltflächen für anonyme Besucher ausblenden](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Abbildung 11**: das LoginView-Steuerelement blendet die Linkschaltflächen für anonyme Besucher aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Programmgesteuerte Einschränkung der Funktionalität

In einigen Fällen reichen die deklarativen Techniken nicht aus, um die Funktionalität auf eine Seite zu beschränken. Beispielsweise kann es sein, dass die Verfügbarkeit bestimmter Seitenfunktionen von den Kriterien abhängig ist, über die der Benutzer, der die Seite besucht, anonym oder authentifiziert ist. In solchen Fällen können die verschiedenen Elemente der Benutzeroberfläche durch programmgesteuerte Mittel angezeigt oder ausgeblendet werden.

Um die Funktionalität Programm gesteuert einzuschränken, müssen wir zwei Aufgaben ausführen:

1. Bestimmen Sie, ob der Benutzer, der die Seite besucht, auf die Funktionen zugreifen kann.
2. Programm gesteuertes Ändern der Benutzeroberfläche basierend darauf, ob der Benutzer Zugriff auf die betreffende Funktionalität hat.

Um die Anwendung dieser beiden Aufgaben zu veranschaulichen, dürfen Sie nur die Dateien aus der GridView löschen. Die erste Aufgabe besteht darin, zu bestimmen, ob es Tito ist, die Seite zu besuchen. Nachdem dies festgelegt wurde, müssen wir die Delete-Spalte der GridView ausblenden (oder anzeigen). Der Zugriff auf die Spalten der GridView erfolgt über Ihre `Columns`-Eigenschaft. eine Spalte wird nur gerendert, wenn die `Visible`-Eigenschaft auf `true` (Standard) festgelegt ist.

Fügen Sie den folgenden Code in den `Page_Load`-Ereignishandler ein, bevor Sie die Daten an das GridView-Ereignis binden:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Wie in der Übersicht über das Tutorial zur [*Formular Authentifizierung*](../introduction/an-overview-of-forms-authentication-cs.md) erläutert, gibt `User.Identity.Name` den Namen der Identität zurück. Dies entspricht dem Benutzernamen, der im Login-Steuerelement eingegeben wurde. Wenn es sich um einen Besuch der Seite handelt, wird die `Visible`-Eigenschaft der zweiten Spalte der GridView auf `true`festgelegt. Andernfalls wird Sie auf `false`festgelegt. Das Ergebnis ist, dass die Spalte löschen nicht gerendert wird (siehe Abbildung 12), wenn jemand anderes als Tito die Seite besucht, entweder einen anderen authentifizierten Benutzer oder einen anonymen Benutzer. Wenn Tito jedoch die Seite besucht, ist die Spalte löschen vorhanden (siehe Abbildung 13).

[![die Spalte löschen nicht gerendert wird, wenn Sie von einem anderen Benutzer als Tito (z. b. Bruce) besucht wird](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Abbildung 12**: die Spalte löschen wird nicht gerendert, wenn Sie von einem anderen Benutzer als Tito (z. b. Bruce) besucht wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image36.png)

[![die Delete-Spalte für Tito gerendert wird.](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Abbildung 13**: die Spalte löschen wird für Tito gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image39.png))

## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Schritt 4: Anwenden von Autorisierungs Regeln auf Klassen und Methoden

In Schritt 3 haben wir den anonymen Benutzern untersagt, den Inhalt einer Datei anzuzeigen, und alle Benutzer, die das Löschen von Dateien untersagt haben, sind nicht zulässig. Dies wurde erreicht, indem die zugehörigen Benutzeroberflächen Elemente für nicht autorisierte Besucher durch deklarative und programmgesteuerte Techniken ausgeblendet wurden. In unserem einfachen Beispiel war das ordnungsgemäße Ausblenden der Benutzeroberflächen Elemente unkompliziert, aber was ist mit komplexeren Websites, bei denen es möglicherweise viele verschiedene Möglichkeiten gibt, dieselbe Funktionalität auszuführen? Was geschieht, wenn Sie diese Funktionalität auf nicht autorisierte Benutzer beschränken, wenn Sie vergessen, alle anwendbaren Elemente der Benutzeroberfläche auszublenden oder zu deaktivieren?

Eine einfache Möglichkeit, um sicherzustellen, dass ein bestimmtes Element nicht von einem nicht autorisierten Benutzer aufgerufen werden kann, besteht darin, diese Klasse oder Methode mit dem [`PrincipalPermission`-Attribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)zu ergänzen. Wenn die .NET-Runtime eine-Klasse verwendet oder eine ihrer Methoden ausführt, wird überprüft, ob der aktuelle Sicherheitskontext über die Berechtigung zum Verwenden der-Klasse oder die Ausführung der-Methode verfügt. Das `PrincipalPermission`-Attribut stellt einen Mechanismus bereit, mit dem diese Regeln definiert werden können.

Im folgenden wird veranschaulicht, wie Sie das `PrincipalPermission`-Attribut für die `SelectedIndexChanged` von GridView und `RowDeleting` Ereignishandler verwenden, um die Ausführung durch anonyme Benutzer und andere Benutzer als Tito zu verhindern. Wir müssen lediglich das entsprechende Attribut für jede Funktionsdefinition hinzufügen:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

Das-Attribut für den `SelectedIndexChanged`-Ereignishandler legt fest, dass nur authentifizierte Benutzer den Ereignishandler ausführen können, wobei das-Attribut des `RowDeleting`-Ereignis Handlers die Ausführung auf Tito einschränkt.

Wenn ein anderer Benutzer als Tito versucht, den `RowDeleting` Ereignishandler auszuführen, oder wenn ein nicht authentifizierter Benutzer versucht, den `SelectedIndexChanged`-Ereignishandler auszuführen, gibt die .NET-Laufzeit eine `SecurityException`aus.

[![wenn der Sicherheitskontext nicht zum Ausführen der Methode autorisiert ist, wird eine SecurityException ausgelöst.](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Abbildung 14**: Wenn der Sicherheitskontext nicht zum Ausführen der Methode autorisiert ist, wird ein `SecurityException` ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](user-based-authorization-cs/_static/image42.png)).

> [!NOTE]
> Um mehreren Sicherheits Kontexten den Zugriff auf eine Klasse oder Methode zu gestatten, ergänzen Sie die Klasse oder Methode mit einem `PrincipalPermission`-Attribut für jeden Sicherheitskontext. Das heißt, damit sowohl Tito als auch Bruce den `RowDeleting` Ereignishandler ausführen kann, fügen Sie *zwei* `PrincipalPermission` Attribute hinzu:

[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Neben ASP.NET Seiten verfügen viele Anwendungen auch über eine Architektur, die verschiedene Ebenen umfasst, z. b. Geschäftslogik und Datenzugriffsebenen. Diese Ebenen werden in der Regel als Klassenbibliotheken implementiert und bieten Klassen und Methoden zum Ausführen von Geschäftslogik-und datenbezogenen Funktionen. Das `PrincipalPermission`-Attribut eignet sich zum Anwenden von Autorisierungs Regeln auf diese Ebenen.

Weitere Informationen zur Verwendung des `PrincipalPermission`-Attributs zum Definieren von Autorisierungs Regeln für Klassen und Methoden finden Sie im Blogbeitrag von [Scott Guthrie](https://weblogs.asp.net/scottgu/)( [Hinzufügen von Autorisierungs Regeln zu Geschäfts-und Daten Ebenen mit `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)).

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben wir uns mit dem Anwenden von benutzerbasierten Autorisierungs Regeln beschäftigt. Wir haben mit einem Blick auf ASP begonnen. Das URL-Autorisierungs Framework von net. Bei jeder Anforderung überprüft das `UrlAuthorizationModule` der ASP.net-Engine die URL-Autorisierungs Regeln, die in der Konfiguration der Anwendung definiert sind, um zu bestimmen, ob die Identität für den Zugriff auf die angeforderte Ressource autorisiert ist. Kurz gesagt: die URL-Autorisierung vereinfacht das Angeben von Autorisierungs Regeln für eine bestimmte Seite oder für alle Seiten in einem bestimmten Verzeichnis.

Das URL-Autorisierungs Framework wendet Autorisierungs Regeln Seite für Seite an. Bei der URL-Autorisierung ist entweder die Anforderungs Identität für den Zugriff auf eine bestimmte Ressource autorisiert. Viele Szenarien erfordern jedoch eine präzisere Autorisierungs Regel. Anstatt zu definieren, wer auf eine Seite zugreifen darf, müssen wir möglicherweise allen Benutzern den Zugriff auf eine Seite erlauben, um unterschiedliche Daten anzuzeigen oder andere Funktionen anzubieten, je nachdem, welche Benutzer die Seite besuchen. Die Autorisierung auf Seitenebene umfasst in der Regel das Ausblenden bestimmter Benutzeroberflächen Elemente, um zu verhindern, dass nicht autorisierte Benutzer auf unzulässige Funktionen zugreifen Außerdem ist es möglich, Attribute zu verwenden, um den Zugriff auf Klassen und die Ausführung ihrer Methoden für bestimmte Benutzer einzuschränken.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Hinzufügen von Autorisierungs Regeln zu Geschäfts-und Daten Ebenen mithilfe von `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET-Autorisierung](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Änderungen zwischen IIS6-und IIS7-Sicherheit](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Konfigurieren bestimmter Dateien und Unterverzeichnisse](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Einschränken der Daten Änderungs Funktionalität basierend auf dem Benutzer](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Schnellstarts für LoginView-Steuerelemente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Grundlegendes zum IIS7 URL Authorization](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` technische Dokumentation](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Arbeiten mit Daten in ASP.NET 2,0](../../data-access/index.md)

### <a name="about-the-author"></a>Zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](validating-user-credentials-against-the-membership-user-store-cs.md)
> [Weiter](storing-additional-user-information-cs.md)
