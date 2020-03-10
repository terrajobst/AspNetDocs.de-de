---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Handbuch zur Problembehandlung für ASP.net Web Pages (Razor) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel werden Probleme beschrieben, die bei der Arbeit mit ASP.net Web Pages (Razor) und einigen empfohlenen Lösungen auftreten können. Software Versionen ASP.net Web-pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: fc03767c16f46c1e282d24ee3a7df2409a7c38bb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473379"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Leitfaden zur Behandlung von Problemen mit ASP.NET Web Pages (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel werden Probleme beschrieben, die bei der Arbeit mit ASP.net Web Pages (Razor) und einigen empfohlenen Lösungen auftreten können.
> 
> ## <a name="software-versions"></a>Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2 und ASP.net Web Pages 1,0.

Dieses Thema enthält folgende Abschnitte:

- [Probleme mit laufenden Seiten](#Issues_Running_.cshtml_Pages)
- [Probleme mit Razor-Code](#IssuesWithRazorCode)
- [Probleme mit der Sicherheit und der Mitgliedschaft](#membership)
- [Probleme beim Senden von e-Mails](#email)
- [Weitere Ressourcen](#AdditionalResources)

Allgemeine Fragen finden Sie in den [FAQ zu ASP.net Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Probleme mit laufenden Seiten

Eine Vielzahl von Problemen kann verhindern, dass *cshtml* -und *vbhtml* -Seiten ordnungsgemäß ausgeführt werden. In diesem Abschnitt werden häufige Fehlermeldungen und wahrscheinliche Gründe aufgeführt.

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP-Fehler 403-verboten: Zugriff verweigert.

*Sie verfügen nicht über die Berechtigung, dieses Verzeichnis oder diese Seite mit den von Ihnen angegebenen Anmelde Informationen anzuzeigen.*

Dieser Fehler kann auftreten, wenn auf dem Server nicht die richtige Version des .NET Framework ausgeführt wird. Stellen Sie sicher, dass auf dem Computer, auf dem der Server (lokal oder Remote) ausgeführt wird, mindestens das .NET Framework 4 installiert ist. Stellen Sie außerdem sicher, dass die Anwendung selbst für die Ausführung der richtigen Version konfiguriert ist.

Wenn dieses Problem bei der Arbeit in webmatrix lokal angezeigt wird, klicken Sie auf den Arbeitsbereich **Website** , und klicken Sie dann in der Strukturansicht auf **Einstellungen**. Wählen Sie in der Liste **.NET Framework Version auswählen** die Option **.NET 4 (integriert)** aus. Wenn diese Version bereits festgelegt ist, versuchen Sie, webmatrix als Administrator auszuführen.

Stellen Sie sicher, dass das Stammverzeichnis Ihrer Website mindestens eine *cshtml* -Datei enthält.

Wenn dieser Fehler angezeigt wird, wenn sich der Webserver auf einem Remote Server befindet, wenden Sie sich an den Server Administrator. Stellen Sie sicher, dass auf dem Server die .NET Framework 4 oder höher installiert ist. Stellen Sie außerdem sicher, dass die Anwendung in einem Anwendungs Pool ausgeführt wird, der für die Verwendung dieser Version von The.NET Framework konfiguriert ist.

Wenn Sie die Kontrolle über den Server haben, stellen Sie sicher, dass die richtige Version des .NET Framework ausgeführt wird. Sie können auch versuchen, die Installation zu reparieren, indem Sie den `aspnet_regiis -iru`-Befehl ausführen. (Wenn Sie z. b. IIS installieren, nachdem Sie den .NET Framework installiert haben, ist IIS nicht ordnungsgemäß für die Ausführung von ASP.NET-Seiten konfiguriert.) Weitere Informationen finden Sie unter [ASP.NET IIS Registration Tool (ASPNET\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>HTTP-Fehler 403,14-verboten

*Der Webserver ist so konfiguriert, dass der Inhalt dieses Verzeichnisses nicht aufgelistet wird.*

Dieser Fehler kann auftreten, wenn Sie eine geschützte Ressource (z. b. die Datei " *Web. config* ") oder in einem geschützten Ordner (z. b. *App\_Daten* oder *App\_Code*) anfordern.

### <a name="http-error-40417---not-found"></a>HTTP-Fehler 404,17-nicht gefunden

*Der angeforderte Inhalt ist anscheinend ein Skript und wird vom statischen Datei Handler nicht bedient.*

Dieser Fehler kann auftreten, wenn der Server nicht ordnungsgemäß für die Verwendung der .NET Framework 4 oder höher konfiguriert ist und den Code in `@{ }` Blöcken daher nicht erkennt. Weitere Informationen finden Sie in der Beschreibung weiter oben für *HTTP-Fehler 403-verboten: Zugriff verweigert*.

### <a name="http-error-4047---not-found"></a>HTTP-Fehler 404,7-nicht gefunden

*Das Anforderungs Filterungs Modul ist so konfiguriert, dass die Dateierweiterung verweigert wird*

Dieser Fehler kann auftreten, wenn *cshtml* -oder *vbhtml* -Erweiterungen explizit auf dem Server blockiert wurden. Ein Symptom für dieses Problem ist, dass URLs funktionieren, wenn Sie die Erweiterung nicht enthalten, aber URLs, die *cshtml* -oder *vbhtml* -Datei enthalten, funktionieren nicht. Eine mögliche Lösung besteht darin, die Erweiterungen in der Datei " *Web. config* " der Website erneut zu aktivieren. Im folgenden Beispiel wird gezeigt, wie die Erweiterung *. cshtml* aktiviert wird.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>HTTP-Fehler 404,8-nicht gefunden

*Das Anforderungs Filterungs Modul ist so konfiguriert, dass ein Pfad in der URL verweigert wird, der einen Abschnitt "hiddensegment" enthält.*

Dieser Fehler kann auftreten, wenn Sie eine geschützte Ressource (z. b. die Datei " *Web. config* ") oder in einem geschützten Ordner (z. b. *App\_Daten* oder *App\_Code*) anfordern.

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Dieser Seitentyp wird nicht verarbeitet (Server Fehler in "/"-Anwendung).

Siehe die Beschreibung weiter oben für HTTP-Fehler 404,17.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Probleme mit Razor-Code

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Der Name '*Class*' ist im aktuellen Kontext nicht vorhanden.

Ein Grund für diesen Fehler ist, dass `class` auf ein Hilfsprogramm verweist, das Hilfsprogramm jedoch nicht installiert ist. Wenn Sie z. b. versuchen, ein Hilfsprogramm zu verwenden, aber wenn Sie das Paket nicht von nuget installiert haben, wird dieser Fehler angezeigt. Verwenden Sie den Katalog in webmatrix, um das Hilfsprogramm zu suchen und zu installieren.

Wenn das Hilfsprogramm installiert ist, die Seite es aber immer noch nicht erkennt, fügen Sie dem Code eine `using`-Anweisung hinzu. Verweisen Sie in der `using`-Anweisung auf den Namespace, der das Hilfsprogramm enthält. Beispielsweise befinden sich die grundlegenden Hilfsprogramme im ASP.net Web-Hilfsprogramme-Paket im `System.Web.Helpers`-Namespace. Fügen Sie oben auf der Seite, auf der Sie das Hilfsprogramm verwenden möchten, folgende Zeile hinzu:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Probleme mit der Sicherheit und der Mitgliedschaft

Wenn Sie das integrierte Sicherheitssystem (Mitgliedschaftssystem) in ASP.net Web Pages (Razor) verwenden, können die folgenden Probleme auftreten.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Um diese Methode aufzurufen, muss die Eigenschaft "Membership. Provider" eine Instanz von "extendedmembership shipprovider" sein.

Dieser Fehler kann darauf hinweisen, dass keine `AspNetSqlMembershipProvider` Klasse konfiguriert ist. (Ein Symptom ist, dass die Site problemlos lokal funktioniert, aber diesen Fehler auslöst, wenn Sie Sie auf dem Server eines Host Anbieters veröffentlichen.) Eine Lösung für dieses Problem besteht darin, die einfache Mitgliedschaft explizit zu aktivieren, indem der Datei " *Web. config* " der Website Folgendes hinzugefügt wird:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Probleme beim Senden von e-Mails

Probleme beim Senden von e-Mails können schwierig zu debuggen sein. Ein erstes Problem kann darin bestehen, dass Sie keine Verbindung mit dem SMTP-Server herstellen können. Wenn die Verbindung erfolgreich hergestellt wurde, übergibt ASP.net die Nachricht an den SMTP-Server. Es können jedoch Probleme mit der Nachricht selbst auftreten, die den SMTP-Server daran hindert, Sie zu senden.

Wenn Ihre Anwendung keine e-Mail erfolgreich sendet, versuchen Sie Folgendes:

- Der Name des SMTP-Servers ist oft wie `smtp.provider.com` oder `smtp.provider.net`. Wenn Sie Ihre Website jedoch in einem Hostinganbieter veröffentlichen, ist der Name des SMTP-Servers zu diesem Zeitpunkt möglicherweise `localhost`. Diese Situation tritt auf, wenn der SMTP-Server nach der Veröffentlichung und der Website, die auf dem Server des Anbieters ausgeführt wird, lokal aus der Perspektive Ihrer Anwendung besteht. Diese Änderung der Servernamen kann bedeuten, dass Sie den SMTP-Servernamen im Rahmen des Veröffentlichungsprozesses ändern müssen.
- Die Portnummer ist in der Regel 25. Bei manchen Anbietern ist es jedoch erforderlich, Port 587 oder einen anderen Port zu verwenden. Überprüfen Sie den Besitzer des SMTP-Servers, welche Portnummer Sie erwarten.
- Stellen Sie sicher, dass Sie die richtigen Anmelde Informationen verwenden. Wenn Sie Ihre Website auf einem Hosting-Anbieter veröffentlicht haben, verwenden Sie die Anmelde Informationen, die der Anbieter speziell für e-Mail angegeben hat. Diese Anmelde Informationen können sich von den Anmelde Informationen unterscheiden, die Sie für die Veröffentlichung verwenden.
- Manchmal benötigen Sie keine Anmelde Informationen. Wenn Sie e-Mails mithilfe Ihres persönlichen ISP senden, kennt Ihr e-Mail-Anbieter ihre Anmelde Informationen möglicherweise bereits. Nach der Veröffentlichung müssen Sie möglicherweise andere Anmelde Informationen als beim Testen auf dem lokalen Computer verwenden.
- Wenn Ihr e-Mail-Anbieter Verschlüsselung verwendet, legen Sie `WebMail.EnableSsl` auf `true`fest.

Wenn beim Senden einer e-Mail ein Fehler auftritt, wird möglicherweise eine ASP.NET-Standard Fehlermeldung angezeigt, die wie folgt aussieht:

![ASP.net-Fehlermeldung bei einem e-Mail-Problem](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Sie können auch Probleme beim Senden von e-Mails mithilfe eines `try-catch` Blocks Debuggen, wie im folgenden Beispiel gezeigt. Wenn Sie einen `try-catch`-Block verwenden, zeigt ASP.net seine Standard Fehlermeldungen nicht an. Stattdessen können Sie den Fehler im `catch` Teil des Blocks erfassen.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Ersetzen Sie die entsprechenden Werte für `your-SMTP-server-name`, usw. Einige der Fehlermeldungen, die möglicherweise auf diese Weise angezeigt werden, umfassen Folgendes:

- *Fehler beim Senden von e-Mail.*

    \- oder -

    *Ein Verbindungsversuch ist fehlgeschlagen, weil die verbundene Partei nach einem bestimmten Zeitraum nicht ordnungsgemäß reagiert hat, oder eine Verbindung konnte nicht hergestellt werden, da der verbundene Host nicht reagiert hat.*

    Dieser Fehler bedeutet in der Regel, dass die Anwendung keine Verbindung mit dem SMTP-Server herstellen konnte. Überprüfen Sie den Servernamen und die Portnummer.
- *Postfach nicht verfügbar. Die Serverantwort lautete: 5.1.0 &lt;someuser@invaliddomain&gt; Absender abgelehnt: Ungültige Absender Domäne*

    Diese Meldung kann darauf hinweisen, dass die `From` Adresse nicht korrekt oder nicht vorhanden ist.
- *Die angegebene Zeichenfolge ist nicht in der für eine e-Mail-Adresse erforderlichen Form angegeben.*

    Dieser Fehler kann darauf hinweisen, dass der Wert der Eigenschaften `To` oder `From` nicht als e-Mail-Adressen erkannt wird. (ASP.net kann nicht prüfen, ob die e-Mail-Adresse gültig ist, sondern nur, dass Sie das richtige Format hat, z. b. *name@domain.com* .)

> [!NOTE]
> Entfernen Sie das Markup, das den Fehler (`@errorMessage`) anzeigt, bevor Sie die Seite auf einer Live Website veröffentlichen. Es ist keine gute Idee, Benutzern Fehlermeldungen anzuzeigen, die Sie von einem Server erhalten.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.NET-Webseiten (Razor) – FAQs](https://go.microsoft.com/fwlink/?LinkId=253000)

[Webmatrix und ASP.net Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) Forum auf der ASP.NET-Website
