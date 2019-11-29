---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Eine Übersicht über die Formular Authentifizierung (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird von der reinen Diskussion zur Implementierung geführt. insbesondere wird die Implementierung der Formular Authentifizierung untersucht. Die Webanwendung...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: d8ceb6b5290300992e52199caa9314c573de1942
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626738"
---
# <a name="an-overview-of-forms-authentication-vb"></a>Eine Übersicht über die Formular Authentifizierung (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> In diesem Tutorial wird von der reinen Diskussion zur Implementierung geführt. insbesondere wird die Implementierung der Formular Authentifizierung untersucht. Die Webanwendung, die wir in diesem Tutorial erstellen, wird in nachfolgenden Tutorials weiterhin erstellt, da wir von der einfachen Formular Authentifizierung zu Mitgliedschaft und Rollen wechseln.
> 
> Weitere Informationen zu diesem Thema finden Sie in diesem Video: [Verwenden der grundlegenden Formular Authentifizierung in ASP.net](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md).

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](security-basics-and-asp-net-support-vb.md) wurden die verschiedenen Authentifizierungs-, Autorisierungs-und Benutzerkonto Optionen erläutert, die von ASP.NET bereitgestellt werden. In diesem Tutorial wird von der reinen Diskussion zur Implementierung geführt. insbesondere wird die Implementierung der Formular Authentifizierung untersucht. Die Webanwendung, die wir in diesem Tutorial erstellen, wird in nachfolgenden Tutorials weiterhin erstellt, da wir von der einfachen Formular Authentifizierung zu Mitgliedschaft und Rollen wechseln.

Dieses Tutorial beginnt mit einem detaillierten Einblick in den Formular Authentifizierungs Workflow, ein Thema, auf das wir im vorherigen Tutorial eingegangen sind. Danach erstellen wir eine ASP.NET-Website, über die die Konzepte der Formular Authentifizierung erläutert werden. Als Nächstes konfigurieren Sie die Website für die Verwendung der Formular Authentifizierung, erstellen eine einfache Anmeldeseite und erfahren, wie Sie im Code bestimmen, ob ein Benutzer authentifiziert ist, und wenn ja, mit dem Benutzernamen, mit dem Sie sich angemeldet haben.

Das Verständnis des Formular Authentifizierungs Workflows, die Aktivierung in einer Webanwendung und das Erstellen der Anmelde-und Abmelde Seiten sind die wesentlichen Schritte beim Erstellen einer ASP.NET-Anwendung, die Benutzerkonten unterstützt und Benutzer über eine Webseite authentifiziert. Aus diesem Grund und da diese Tutorials aufeinander aufbauen, empfehlen wir Ihnen, dieses Tutorial vollständig durchzuarbeiten, bevor Sie mit dem nächsten Schritt fortfahren, auch wenn Sie bereits über Erfahrungen mit der Konfiguration der Formular Authentifizierung in früheren Projekten verfügen.

## <a name="understanding-the-forms-authentication-workflow"></a>Grundlegendes zum Formular Authentifizierungs Workflow

Wenn die ASP.NET-Laufzeit eine Anforderung für eine ASP.NET-Ressource verarbeitet, z. b. eine ASP.NET-Seite oder einen ASP.NET-Webdienst, löst die Anforderung während des Lebenszyklus eine Reihe von Ereignissen aus. Ereignisse werden am Anfang und am Ende der Anforderung ausgelöst. Diese werden ausgelöst, wenn die Anforderung authentifiziert und autorisiert wird, ein Ereignis, das im Fall einer nicht behandelten Ausnahme ausgelöst wird, usw. Eine umfassende Liste der Ereignisse finden Sie in den [Ereignissen des HttpApplication-Objekts](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*HTTP-Module* sind verwaltete Klassen, deren Code als Reaktion auf ein bestimmtes Ereignis im Anforderungs Lebenszyklus ausgeführt wird. ASP.net wird mit einer Reihe von HTTP-Modulen ausgeliefert, die wichtige Aufgaben im Hintergrund ausführen. Zwei integrierte HTTP-Module, die für unsere Diskussion besonders relevant sind:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** : authentifiziert den Benutzer, indem er das Formular Authentifizierungs Ticket überprüft, das in der Regel in der Cookies-Auflistung des Benutzers enthalten ist. Wenn kein Formular Authentifizierungs Ticket vorhanden ist, ist der Benutzer anonym.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** : bestimmt, ob der aktuelle Benutzer für den Zugriff auf die angeforderte URL autorisiert ist. Dieses Modul bestimmt die Autorität, indem die Autorisierungs Regeln, die in den Konfigurationsdateien der Anwendung angegeben sind, konsultiert werden. ASP.NET enthält auch das [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) , das die Autorität bestimmt, indem die angeforderten Datei-ACLs konsultiert werden.

Das FormsAuthenticationModule-Modul versucht, den Benutzer zu authentifizieren, bevor das "UrlAuthorizationModule" (und das FileAuthorizationModule) ausgeführt wird. Wenn der Benutzer, der die Anforderung sendet, nicht autorisiert ist, auf die angeforderte Ressource zuzugreifen, beendet das Autorisierungs Modul die Anforderung und gibt den Status " [HTTP 401 nicht autorisiert](http://www.checkupdown.com/status/E401.html) " zurück. In Windows-Authentifizierungs Szenarien wird der HTTP 401-Status an den Browser zurückgegeben. Dieser Statuscode bewirkt, dass der Browser den Benutzer über ein modales Dialogfeld zur Eingabe seiner Anmelde Informationen auffordert. Bei der Formular Authentifizierung wird der HTTP 401-Status "nicht autorisiert" jedoch nie an den Browser gesendet, weil das FormsAuthenticationModule diesen Status erkennt und ihn so ändert, dass der Benutzer stattdessen an die Anmeldeseite umgeleitet wird (über einen [HTTP 302-Umleitungs](http://www.checkupdown.com/status/E302.html) Status).

Auf der Anmeldeseite muss festgestellt werden, ob die Anmelde Informationen des Benutzers gültig sind. wenn dies der Fall ist, wird ein Formular Authentifizierungs Ticket erstellt und der Benutzer zurück an die Seite umgeleitet, die er besuchen wollte. Das Authentifizierungs Ticket ist in nachfolgenden Anforderungen an die Seiten auf der Website enthalten, die von FormsAuthenticationModule verwendet werden, um den Benutzer zu identifizieren.

[![des Formular Authentifizierungs Workflows](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Abbildung 01**: der Workflow zur Formular Authentifizierung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image3.png))

### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Erinnerung an das Authentifizierungs Ticket über Seitenbesuche

Nach der Anmeldung muss das Formular Authentifizierungs Ticket bei jeder Anforderung an den Webserver zurückgesendet werden, damit der Benutzer beim Durchsuchen der Website angemeldet bleibt. Dies wird in der Regel erreicht, indem das Authentifizierungs Ticket in der Cookies-Sammlung des Benutzers abgelegt wird. [Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) sind kleine Textdateien, die sich auf dem Computer des Benutzers befinden und in den HTTP-Headern für jede Anforderung an die Website übertragen werden, von der das Cookie erstellt wurde. Nachdem das Formular Authentifizierungs Ticket erstellt und in den Cookies des Browsers gespeichert wurde, sendet jeder nachfolgende Besuch dieser Website das Authentifizierungs Ticket zusammen mit der Anforderung und identifiziert so den Benutzer.

> [!NOTE]
> Die in jedem Tutorial verwendete Demo-Webanwendung steht als Download zur Verfügung. Diese herunterladbare Anwendung wurde mit Visual Web Developer 2008 erstellt, das auf die .NET Framework Version 3,5 ausgerichtet ist. Da die Anwendung für .NET 3,5 konzipiert ist, enthält die Datei Web. config zusätzliche, 3,5-spezifische Konfigurationselemente. Lange Geschichte kurz: Wenn Sie .NET 3,5 noch nicht auf Ihrem Computer installieren müssen, funktioniert die herunterladbare Webanwendung nicht, ohne zuvor das 3,5-spezifische Markup aus "Web. config" zu entfernen.

Ein Aspekt der Cookies ist der Ablauf, bei dem es sich um das Datum und die Uhrzeit handelt, zu denen der Browser das Cookie verwirft. Wenn das Formular Authentifizierungs Cookie abläuft, kann der Benutzer nicht mehr authentifiziert und daher anonym werden. Wenn ein Benutzer von einem öffentlichen Terminal aus besucht, ist es wahrscheinlich, dass das Authentifizierungs Ticket abläuft, wenn er seinen Browser schließt. Beim Besuch von Home ist es jedoch möglicherweise erforderlich, dass das Authentifizierungs Ticket über Browser Neustarts hinweg gespeichert wird, damit Sie sich nicht bei jedem Besuch der Website erneut anmelden müssen. Diese Entscheidung wird häufig vom Benutzer in Form eines Kontrollkästchens für die Anmeldung auf der Anmeldeseite getroffen. In Schritt 3 wird erläutert, wie Sie das Kontrollkästchen "Speichern" auf der Anmeldeseite implementieren. Im folgenden Tutorial werden die Timeout Einstellungen für das Authentifizierungs Ticket ausführlich erläutert.

> [!NOTE]
> Es ist möglich, dass der Benutzer-Agent, der für die Anmeldung bei der Website verwendet wird, Cookies möglicherweise nicht unterstützt. In einem solchen Fall kann ASP.net cookilose Formular Authentifizierungs Tickets verwenden. In diesem Modus wird das Authentifizierungs Ticket in die URL codiert. Wir sehen uns an, wann cookilose Authentifizierungs Tickets verwendet werden und wie Sie im nächsten Tutorial erstellt und verwaltet werden.

### <a name="the-scope-of-forms-authentication"></a>Der Bereich der Formular Authentifizierung.

Bei FormsAuthenticationModule handelt es sich um verwalteten Code, der Teil der ASP.NET-Laufzeit ist. Vor Version 7 des Microsoft- [Internetinformationsdienste (IIS)](https://www.iis.net/) -Webservers gab es eine eindeutige Barriere zwischen der HTTP-Pipeline von IIS und der ASP.net-Lauf Zeit Pipeline. Kurz gesagt: in IIS 6 und früheren Versionen wird das FormsAuthenticationModule nur dann ausgeführt, wenn eine Anforderung von IIS an die ASP.NET-Laufzeit delegiert wird. Standardmäßig verarbeitet IIS statische Inhalte selbst, z. b. HTML-Seiten und CSS-und Bilddateien, und übergibt nur Anforderungen an die ASP.NET-Laufzeit, wenn eine Seite mit der Erweiterung ASPX, asmx oder ashx angefordert wird.

IIS 7 ermöglicht jedoch integrierte IIS-und ASP.net-Pipelines. Mit einigen Konfigurationseinstellungen können Sie IIS 7 einrichten, um das FormsAuthenticationModule für *alle* Anforderungen aufzurufen. Darüber hinaus können Sie mit IIS 7 URL-Autorisierungs Regeln für Dateien eines beliebigen Typs definieren. Weitere Informationen finden Sie unter [Änderungen zwischen IIS6-und IIS7-Sicherheit](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [ihrer Webplattform-Sicherheit](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)und Grundlegendes zur [IIS7-URL-Autorisierung](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Lange Geschichte kurz: in Versionen vor IIS 7 können Sie nur die Formular Authentifizierung verwenden, um Ressourcen zu schützen, die von der ASP.NET-Laufzeit behandelt werden. Ebenso gelten URL-Autorisierungs Regeln nur für Ressourcen, die von der ASP.NET-Laufzeit behandelt werden. Aber mit IIS 7 ist es möglich, FormsAuthenticationModule und cdthorizationmodule in die HTTP-Pipeline von IIS zu integrieren, um diese Funktionalität auf alle Anforderungen auszuweiten.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Schritt 1: Erstellen einer ASP.NET-Website für diese tutorialreihe

Um die größtmögliche Zielgruppe zu erreichen, wird die ASP.NET-Website, die wir in dieser Reihe erstellen werden, mit der kostenlosen Version von Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)von Microsoft erstellt. Der sqlmitgliedshipprovider-Benutzerspeicher wird in einer [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) -Datenbank implementiert. Wenn Sie Visual Studio 2005 oder eine andere Edition von Visual Studio 2008 oder SQL Server verwenden, machen Sie sich keine Sorgen, da die Schritte nahezu identisch sind und alle nicht trivialen Unterschiede aufgezeigt werden.

Bevor wir die Formular Authentifizierung konfigurieren können, benötigen wir zuerst eine ASP.NET-Website. Beginnen Sie mit dem Erstellen einer neuen Dateisystem basierten ASP.NET-Website. Um dies zu erreichen, starten Sie Visual Web Developer. wechseln Sie dann zum Menü Datei, und wählen Sie neue Website aus, auf der das Dialogfeld neue Website angezeigt wird. Wählen Sie die Vorlage ASP.NET-Website aus, legen Sie die Dropdown Liste Speicherort auf Datei System fest, wählen Sie einen Ordner zum Platzieren der Website aus, und legen Sie die Sprache auf VB fest. Dadurch wird eine neue Website mit der ASP.NET-Seite "default. aspx", einer APP\_Daten Ordners und einer "Web. config"-Datei erstellt.

> [!NOTE]
> Visual Studio unterstützt zwei Modi der Projektverwaltung: Website Projekte und Webanwendungs Projekte. Bei Website Projekten fehlt eine Projektdatei, während Webanwendungs Projekte die Projekt Architektur in Visual Studio .NET 2002/2003 imitieren. Sie enthalten eine Projektdatei und kompilieren den Quellcode des Projekts in eine einzelne Assembly, die sich im Ordner "/bin" befindet. Visual Studio 2005 ursprünglich nur unterstützte Website Projekte, obwohl das Webanwendungs Projekt Modell mit Service Pack 1 neu eingeführt wurde. Visual Studio 2008 bietet beide Projekt Modelle. Die Editionen "Visual Web Developer 2005" und "2008" unterstützen jedoch nur Website Projekte. Ich verwende das Website Projekt Modell. Wenn Sie eine nicht-Express-Edition verwenden und stattdessen das [Webanwendungs Projekt Modell](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) verwenden möchten, können Sie dies tun, aber beachten Sie, dass es möglicherweise einige Abweichungen zwischen den auf Ihrem Bildschirm angezeigten Schritten und den erforderlichen Schritten und den in diesen Tutorials dargestellten Bildschirmfotos gibt.

[![Erstellen einer neuen Datei System basierten Website](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Abbildung 02**: Erstellen einer neuen Datei System basierten Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image6.png))

### <a name="adding-a-master-page"></a>Hinzufügen einer Master Seite

Fügen Sie als nächstes der Website eine neue Master Seite im Stammverzeichnis mit dem Namen Site. Master hinzu. [Master Seiten](https://msdn.microsoft.com/library/wtxbf3hh.aspx) ermöglichen es einem Seiten Entwickler, eine Website weite Vorlage zu definieren, die auf ASP.NET-Seiten angewendet werden kann. Der Hauptvorteil von Masterseiten besteht darin, dass die Gesamtdarstellung der Site an einem einzigen Ort definiert werden kann, sodass das Layout der Website einfach aktualisiert oder angepasst werden kann.

[![der Website eine Master Seite mit dem Namen "Site. Master" hinzufügen](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Abbildung 03**: Hinzufügen einer Master Seite mit dem Namen "Site. Master" zur Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image9.png))

Definieren Sie das standortweite Seitenlayout hier auf der Master Seite. Sie können den Designansicht verwenden und alle benötigten Layout-oder websteuer Elemente hinzufügen, oder Sie können das Markup manuell in der Quell Ansicht hinzufügen. Ich strukturierte das Layout meiner Master Seite so, dass es das Layout für meine *[Arbeiten mit Daten in ASP.NET 2,0](../../data-access/index.md)* -tutorialreihe imitiert (siehe Abbildung 4). Auf der Master Seite werden [Cascading Stylesheets](http://www.w3schools.com/css/default.asp) für die Positionierung und Stile mit den CSS-Einstellungen verwendet, die in der Datei "Style. CSS" definiert sind (die im zugehörigen Download dieses Tutorials enthalten ist). Obwohl Sie nicht im unten gezeigten Markup erkennen können, werden die CSS-Regeln so definiert, dass der Inhalt der Navigation &lt;div&gt;absolut so positioniert ist, dass er auf der linken Seite angezeigt wird und eine festgelegte Breite von 200 Pixeln aufweist.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Eine Master Seite definiert sowohl das statische Seitenlayout als auch die Bereiche, die von den ASP.NET-Seiten bearbeitet werden können, die die Master Seite verwenden. Diese bearbeitbaren Inhaltsbereiche werden durch das contentplachalter-Steuerelement angegeben, das im Content &lt;div&gt;angezeigt werden kann. Unsere Master Seite verfügt über einen einzelnen contentplachalter (mainContent), die Master Seite kann jedoch mehrere Inhalts Platzhalter enthalten.

Wenn Sie das obige Markup eingegeben haben, zeigt der Wechsel zum Designansicht das Layout der Master Seite an. Alle ASP.NET Seiten, auf denen diese Master Seite verwendet wird, verfügen über dieses einheitliche Layout, mit der Möglichkeit, das Markup für den mainContent-Bereich anzugeben.

[![der Master Seite, wenn Sie in der Entwurfs Ansicht angezeigt wird](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Abbildung 04**: die Master Seite, wenn Sie in der Entwurfs Ansicht angezeigt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image12.png))

### <a name="creating-content-pages"></a>Erstellen von Inhaltsseiten

An dieser Stelle haben wir eine default. aspx-Seite auf unserer Website, aber die soeben erstellte Master Seite wird nicht verwendet. Obwohl es möglich ist, das deklarative Markup einer Webseite so zu ändern, dass eine Master Seite verwendet wird, wenn die Seite keinen Inhalt enthält, ist es einfacher, die Seite zu löschen und Sie erneut dem Projekt hinzuzufügen, und die zu verwendende Master Seite anzugeben. Daher sollten Sie zunächst "default. aspx" aus dem Projekt löschen.

Klicken Sie anschließend im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie ein neues Webformular mit dem Namen default. aspx aus. Aktivieren Sie dieses Mal das Kontrollkästchen Master Seite auswählen, und wählen Sie die Master Seite Site. Master aus der Liste aus.

[![eine neue Seite "default. aspx" hinzufügen, um eine Master Seite auszuwählen.](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Abbildung 05**: Hinzufügen einer neuen Seite "default. aspx" Auswählen einer Master Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image15.png))

[![verwenden der Master Seite "Site. Master"](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Abbildung 06**: Verwenden der Master Seite "Site. Master" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image18.png))

> [!NOTE]
> Wenn Sie das Webanwendungs Projekt Modell verwenden, enthält das Dialogfeld Neues Element hinzufügen das Kontrollkästchen Master Seite auswählen nicht. Stattdessen müssen Sie ein Element des Typs Webinhalts Formular hinzufügen. Nachdem Sie die Option Webinhalts Formular ausgewählt und auf Hinzufügen geklickt haben, wird in Visual Studio das gleiche in Abbildung 6 dargestellte Dialogfeld "Master auswählen" angezeigt.

Das deklarative Markup der neuen default. aspx-Seite enthält nur eine @Page-Direktive, die den Pfad zur Masterseiten Datei und ein Inhalts Steuerelement für den mainContent-contentplachalter der Master Seite angibt.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Belassen Sie vorerst "default. aspx" leer. Wir kehren später in diesem Tutorial zu diesem Tutorial zurück, um Inhalte hinzuzufügen.

> [!NOTE]
> Unsere Master Seite enthält einen Abschnitt für ein Menü oder eine andere Navigationsschnittstelle. Wir werden eine solche Schnittstelle in einem zukünftigen Tutorial erstellen.

## <a name="step-2-enabling-forms-authentication"></a>Schritt 2: Aktivieren der Formular Authentifizierung

Nachdem die ASP.NET-Website erstellt wurde, besteht die nächste Aufgabe darin, die Formular Authentifizierung zu aktivieren. Die Authentifizierungs Konfiguration der Anwendung wird über das [&lt;Authentication&gt;-Element](https://msdn.microsoft.com/library/532aee0e.aspx) in Web. config angegeben. Das &lt;Authentication&gt;-Element enthält ein einzelnes Attribut mit dem Namen Mode, das das von der Anwendung verwendete Authentifizierungs Modell angibt. Dieses Attribut kann einen der folgenden vier Werte aufweisen:

- **Windows** : Wenn eine Anwendung die Windows-Authentifizierung verwendet, ist es in der Verantwortung des Webservers, den Besucher zu authentifizieren, und dies erfolgt in der Regel über die Basis-, Digest-oder integrierte Windows-Authentifizierung.
- **Formulare**: Benutzer werden über ein Formular auf einer Webseite authentifiziert.
- **Passport**: Benutzer werden über das Passport-Netzwerk von Microsoft authentifiziert.
- **Keine**: Es wird kein Authentifizierungs Modell verwendet. alle Besucher sind anonym.

Standardmäßig verwenden ASP.NET-Anwendungen die Windows-Authentifizierung. Um den Authentifizierungstyp in die Formular Authentifizierung zu ändern, müssen wir das Mode-Attribut des &lt;Authentication-&gt; Elements in Forms ändern.

Wenn das Projekt noch keine Web. config-Datei enthält, fügen Sie es jetzt ein, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen klicken, neues Element hinzufügen auswählen und dann eine Webkonfigurationsdatei hinzufügen.

[![, wenn das Projekt noch keine Web. config-Datei enthält, fügen Sie es jetzt hinzu.](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Abbildung 07**: Wenn das Projekt noch keine Web. config-Datei enthält, fügen Sie es jetzt hinzu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image21.png)).

Suchen Sie als nächstes das &lt;Authentication&gt;-Element, und aktualisieren Sie es für die Verwendung der Formular Authentifizierung. Nachdem Sie diese Änderung vorgenommen haben, sollte das Markup Ihrer Web. config-Datei in etwa wie folgt aussehen:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Da Web. config eine XML-Datei ist, ist die Groß-/Kleinschreibung wichtig. Stellen Sie sicher, dass Sie das Mode-Attribut mit einem Großbuchstaben (F) auf Forms festlegen. Wenn Sie eine andere Schreibweise wie Formulare verwenden, erhalten Sie einen Konfigurationsfehler, wenn Sie die Website über einen Browser besuchen.

Das &lt;Authentication&gt;-Element kann optional ein &lt;Forms&gt; untergeordnetes Element enthalten, das Formular Authentifizierungs spezifische Einstellungen enthält. Nun verwenden wir einfach die Standardeinstellungen für die Formular Authentifizierung. Im nächsten Tutorial untersuchen wir das &lt;Forms-&gt; untergeordnete Element ausführlicher.

## <a name="step-3-building-the-login-page"></a>Schritt 3: Aufbau der Anmeldeseite

Um die Formular Authentifizierung zu unterstützen, benötigt unsere Website eine Anmeldeseite. Wie im Abschnitt Grundlegendes zum Formular Authentifizierungs Workflow erläutert, leitet das FormsAuthenticationModule den Benutzer automatisch an die Anmeldeseite weiter, wenn er versucht, auf eine Seite zuzugreifen, für die er nicht autorisiert ist. Es gibt auch ASP.net-websteuer Elemente, die einen Link zur Anmeldeseite für anonyme Benutzer anzeigen. Dadurch wird die Frage angezeigt, wie lautet die URL der Anmeldeseite?

Standardmäßig erwartet das Formular Authentifizierungssystem, dass die Anmeldeseite "Login. aspx" heißt und im Stammverzeichnis der Webanwendung abgelegt wird. Wenn Sie eine andere URL für die Anmeldeseite verwenden möchten, können Sie dies in der Datei "Web. config" angeben. Dies wird im nachfolgenden Tutorial erläutert.

Die Anmeldeseite besteht aus drei Aufgaben:

1. Stellen Sie eine Schnittstelle bereit, die es dem Besucher ermöglicht, seine Anmelde Informationen einzugeben.
2. Bestimmen Sie, ob die übermittelten Anmelde Informationen gültig sind.
3. Melden Sie sich beim Benutzer an, indem Sie das Formular Authentifizierungs Ticket erstellen.

### <a name="creating-the-login-pages-user-interface"></a>Erstellen der Benutzeroberfläche der Anmeldeseite

Beginnen wir mit der ersten Aufgabe. Fügen Sie dem Stammverzeichnis der Website eine neue ASP.NET-Seite mit dem Namen Login. aspx hinzu, und ordnen Sie Sie der Master Seite Site. Master zu.

[![eine neue ASP.NET Seite mit dem Namen "Login. aspx" hinzufügen](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Abbildung 08**: Hinzufügen einer neuen ASP.NET-Seite namens "Login. aspx" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image24.png))

Die typische Anmelde Seiten Schnittstelle besteht aus zwei Textfeldern: einer für den Namen des Benutzers, einer für das Kennwort und eine Schaltfläche zum Senden des Formulars. Die Website enthält ein Kontrollkästchen, das bei aktivierter Aktivierung das resultierende Authentifizierungs Ticket über Browser Neustarts hinweg beibehält.

Fügen Sie "Login. aspx" zwei Textfelder hinzu, und legen Sie Ihre ID-Eigenschaften auf Benutzername bzw. Kennwort fest. Legen Sie außerdem die TextMode-Eigenschaft des Kennworts auf Password fest. Fügen Sie als nächstes ein CheckBox-Steuerelement hinzu, und legen Sie seine ID-Eigenschaft auf Erinnerung und deren Text-Eigenschaft fest, Fügen Sie danach eine Schaltfläche mit dem Namen loginbutton hinzu, deren Text-Eigenschaft auf Login festgelegt ist. Fügen Sie abschließend ein Label-websteuer Element hinzu, und legen Sie die ID-Eigenschaft auf invalidkredentialsmess Age fest, deren Text-Eigenschaft auf den Benutzernamen oder das Kennwort ungültig ist. Versuchen Sie es erneut., seine ForeColor-Eigenschaft in rot und die sichtbare Eigenschaft auf "false".

An diesem Punkt sollte der Bildschirm dem Screenshot in Abbildung 9 ähneln, und die deklarative Syntax Ihrer Seite sollte wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]

[![die Anmeldeseite zwei Textfelder enthält, ein Kontrollkästchen, eine Schaltfläche und eine Bezeichnung.](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Abbildung 09**: die Anmeldeseite enthält zwei Textfelder, ein Kontrollkästchen, eine Schaltfläche und eine Bezeichnung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image27.png))

Erstellen Sie abschließend einen Ereignishandler für das Click-Ereignis von loginbutton. Doppelklicken Sie im Designer einfach auf das Schaltflächen-Steuerelement, um diesen Ereignishandler zu erstellen.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Ermitteln, ob die angegebenen Anmelde Informationen gültig sind

Wir müssen jetzt Aufgabe 2 im Click-Ereignishandler der Schaltfläche implementieren, um zu bestimmen, ob die angegebenen Anmelde Informationen gültig sind. Zu diesem Zweck muss ein Benutzerspeicher vorhanden sein, der alle Anmelde Informationen des Benutzers enthält, damit wir ermitteln können, ob die angegebenen Anmelde Informationen mit bekannten Anmelde Informationen identisch sind.

Vor ASP.NET 2,0 waren Entwickler dafür verantwortlich, sowohl ihre eigenen Benutzerspeicher zu implementieren als auch den Code zu schreiben, um die angegebenen Anmelde Informationen für den Speicher zu überprüfen. Die meisten Entwickler würden den Benutzerspeicher in einer Datenbank implementieren und eine Tabelle mit dem Namen "Benutzer" mit Spalten wie "username", "Password", "Email", "LastLoginDate" usw. erstellen. Diese Tabelle hat dann einen Datensatz pro Benutzerkonto. Wenn Sie die angegebenen Anmelde Informationen eines Benutzers überprüfen, müssen Sie die Datenbank auf einen passenden Benutzernamen Abfragen und dann sicherstellen, dass das Kennwort in der Datenbank dem angegebenen Kennwort entspricht.

Mit ASP.NET 2,0 sollten Entwickler einen der Mitgliedschafts Anbieter verwenden, um den Benutzerspeicher zu verwalten. In dieser tutorialreihe wird der sqlmembership shipprovider verwendet, der eine SQL Server Datenbank für den Benutzerspeicher verwendet. Bei Verwendung von sqlmembership shipprovider muss ein bestimmtes Datenbankschema implementiert werden, das die vom Anbieter erwarteten Tabellen, Sichten und gespeicherten Prozeduren enthält. Wir untersuchen, wie dieses Schema im Tutorial *[Erstellen des Mitgliedschafts Schemas in SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* implementiert wird. Mit dem Mitgliedschafts Anbieter ist das Überprüfen der Anmelde Informationen des Benutzers so einfach wie das Aufrufen der [ValidateUser-Methode (*username*, *Password*)](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)der [Mitgliedschafts Klasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx), die einen booleschen Wert zurückgibt, der angibt, ob die Gültigkeit der Kombination aus *Benutzername* und *Kennwort* gültig ist. Wenn Sie sehen, dass der Benutzerspeicher von sqlmitgliedshipprovider noch nicht implementiert wurde, ist es nicht möglich, die ValidateUser-Methode der Mitgliedschafts Klasse zu diesem Zeitpunkt zu verwenden.

Anstatt die Zeit zum Erstellen einer eigenen benutzerdefinierten Benutzerdaten Bank Tabelle zu erstellen (die nach der Implementierung von sqlmembership shipprovider veraltet wäre), sollten Sie stattdessen die gültigen Anmelde Informationen auf der Anmeldeseite selbst hart codieren. Fügen Sie im Click-Ereignishandler von loginbutton den folgenden Code hinzu:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Wie Sie sehen können, gibt es drei gültige Benutzerkonten: Scott, jisun und Sam, und alle drei haben das gleiche Kennwort (Kennwort). Der Code durchläuft die Arrays von Benutzern und Kenn Wörtern, die nach einem gültigen Benutzernamen und einem Kennwort suchen. Wenn sowohl der Benutzername als auch das Kennwort gültig sind, müssen wir den Benutzer anmelden und ihn dann an die entsprechende Seite umleiten. Wenn die Anmelde Informationen ungültig sind, zeigen wir die Bezeichnung invalidkredentialsmess Age an.

Wenn ein Benutzer gültige Anmelde Informationen eingibt, habe ich erwähnt, dass diese an die entsprechende Seite umgeleitet werden. Was ist die geeignete Seite, aber? Wenn ein Benutzer eine Seite besucht, die nicht zum Anzeigen autorisiert ist, wird er von FormsAuthenticationModule automatisch an die Anmeldeseite umgeleitet. Dabei enthält Sie die angeforderte URL in der Abfrage Zeichenfolge über den Parameter "rückkehrnurl". Das heißt, wenn ein Benutzer versucht hat, protectedpage. aspx zu besuchen, und er nicht dazu autorisiert war, leitet das FormsAuthenticationModule Sie an:

Login. aspx? Rückkehrnurl = protectedpage. aspx

Nach erfolgreicher Anmeldung sollte der Benutzer zu protectedpage. aspx zurück umgeleitet werden. Alternativ können Benutzer die Anmeldeseite in ihrer eigenen Volition besuchen. In diesem Fall sollten Sie den Benutzer nach der Anmeldung an die Seite "default. aspx" des Stamm Ordners senden.

### <a name="logging-in-the-user"></a>Anmelden beim Benutzer

Wenn Sie davon ausgehen, dass die angegebenen Anmelde Informationen gültig sind, müssen wir ein Formular Authentifizierungs Ticket erstellen und damit den Benutzer an der Website anmelden. Die [FormsAuthentication-Klasse](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) im [System. Web. Security-Namespace](https://msdn.microsoft.com/library/system.web.security.aspx) bietet verschiedene Methoden zum Anmelden und Abmelden von Benutzern über das Formular Authentifizierungs System. Obwohl es in der FormsAuthentication-Klasse mehrere Methoden gibt, sind wir an diesem Punkt an den drei folgenden Themen interessiert:

- [GetAuthCookie (*username*, *persistcookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) : erstellt ein Formular Authentifizierungs Ticket für den angegebenen Namen *Benutzername*. Anschließend wird von dieser Methode ein HttpCookie-Objekt erstellt und zurückgegeben, das den Inhalt des Authentifizierungs Tickets enthält. Wenn *persistcookie* den Wert true hat, wird ein dauerhaftes Cookie erstellt.
- [SetAuthCookie (*username*, *persistcookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) : Ruft die GetAuthCookie (*username*, *persistcookie*)-Methode auf, um das Formular Authentifizierungs Cookie zu generieren. Diese Methode fügt dann das von GetAuthCookie zurückgegebene Cookie der Cookies-Auflistung hinzu (vorausgesetzt, dass Cookies-basierte Formular Authentifizierung verwendet wird), andernfalls ruft diese Methode eine interne Klasse auf, die die cookielose Ticket Logik behandelt.
- [RedirectFromLoginPage (*username*, *persistcookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) : Diese Methode ruft SetAuthCookie (*username*, *persistcookie*) auf und leitet den Benutzer dann an die entsprechende Seite um.

GetAuthCookie ist praktisch, wenn Sie das Authentifizierungs Ticket ändern müssen, bevor Sie das Cookie in die Cookies-Sammlung schreiben. SetAuthCookie ist nützlich, wenn Sie das Formular Authentifizierungs Ticket erstellen und der Cookies-Sammlung hinzufügen möchten, aber den Benutzer nicht an die entsprechende Seite weiterleiten möchten. Vielleicht möchten Sie Sie auf der Anmeldeseite aufbewahren oder an eine alternative Seite senden.

Da wir den Benutzer anmelden und ihn an die entsprechende Seite umleiten möchten, verwenden wir RedirectFromLoginPage. Aktualisieren Sie den Click-Ereignishandler von loginbutton, und ersetzen Sie die beiden kommentierten TODO-Zeilen durch die folgende Codezeile:

FormsAuthentication. RedirectFromLoginPage (username. Text, Erinnerungen. aktiviert)

Beim Erstellen des Formular Authentifizierungs Tickets verwenden wir die Text-Eigenschaft des username-Textfelds für den Parameter " *username* " des Formular Authentifizierungs Tickets und den aktivierten Zustand des Kontrollkästchens "Erinnerung" für den *persistcookie* -Parameter.

Um die Anmeldeseite zu testen, besuchen Sie Sie in einem Browser. Beginnen Sie, indem Sie ungültige Anmelde Informationen eingeben, z. b. den Benutzernamen Nope und das Kennwort falsch. Wenn Sie auf die Anmelde Schaltfläche klicken, wird ein Postback ausgeführt, und die Bezeichnung invalidkredentialsmmeage wird angezeigt.

[![die Bezeichnung "invalidkredentialsmess Age" angezeigt, wenn Sie ungültige Anmelde Informationen eingeben.](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Abbildung 10**: die Bezeichnung "invalidkredentialsmess Age" wird angezeigt, wenn ungültige Anmelde Informationen eingegeben werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image30.png)).

Geben Sie als nächstes gültige Anmelde Informationen ein, und klicken Sie auf die Schaltfläche Diesmal wird ein Formular Authentifizierungs Ticket erstellt, und Sie werden automatisch zurück an default. aspx umgeleitet. An dieser Stelle haben Sie sich bei der Website angemeldet, aber es gibt keine visuellen Hinweise, die darauf hinweisen, dass Sie zurzeit angemeldet sind. In Schritt 4 sehen Sie, wie Sie Programm gesteuert ermitteln, ob ein Benutzer angemeldet ist oder nicht, und wie Sie den Benutzer identifizieren, der die Seite besucht.

In Schritt 5 werden die Verfahren zum Protokollieren eines Benutzers aus der Website überprüft.

### <a name="securing-the-login-page"></a>Sichern der Anmeldeseite

Wenn der Benutzer die Anmelde Informationen eingibt und das Formular der Anmeldeseite sendet, werden die Anmelde Informationen (einschließlich des Kennworts) über das Internet als nur- *Text*an den Webserver übertragen. Dies bedeutet, dass der Benutzername und das Kennwort von Hacker-Ermittlung des Netzwerk Datenverkehrs angezeigt werden. Um dies zu verhindern, ist es von entscheidender Bedeutung, den Netzwerk Datenverkehr mithilfe von [SSL (Secure Socket Layer)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)zu verschlüsseln. Dadurch wird sichergestellt, dass die Anmelde Informationen (und das HTML-Markup der gesamten Seite) ab dem Zeitpunkt, an dem Sie den Browser verlassen, verschlüsselt werden, bis Sie vom Webserver empfangen werden.

Wenn Ihre Website keine vertraulichen Informationen enthält, müssen Sie nur SSL auf der Anmeldeseite und auf anderen Seiten verwenden, auf denen das Kennwort des Benutzers andernfalls per Klartext über das Netzwerk gesendet wird. Sie müssen sich keine Gedanken über das Sichern des Formular Authentifizierungs Tickets machen, da es standardmäßig sowohl verschlüsselt als auch digital signiert ist (um Manipulationen zu verhindern). Im folgenden Tutorial finden Sie eine ausführlichere Erläuterung der Authentifizierung von Formular Authentifizierungs Tickets.

> [!NOTE]
> Viele Finanz-und medizinische Websites sind für die Verwendung von SSL auf *allen* Seiten konfiguriert, auf die authentifizierte Benutzer zugreifen können. Wenn Sie eine solche Website entwickeln, können Sie das Formular Authentifizierungssystem so konfigurieren, dass das Formular Authentifizierungs Ticket nur über eine sichere Verbindung übertragen wird. Im nächsten Tutorial werden die verschiedenen Konfigurationsoptionen zur Formular Authentifizierung, die Konfiguration der *[Formular Authentifizierung und erweiterte Themen behandelt](../membership/creating-the-membership-schema-in-sql-server-vb.md)* .

## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Schritt 4: ermitteln authentifizierter Besucher und ermitteln ihrer Identität

An dieser Stelle haben wir die Formular Authentifizierung aktiviert und eine rudimentäre Anmeldeseite erstellt, aber wir haben noch untersucht, wie wir bestimmen können, ob ein Benutzer authentifiziert oder anonym ist. In bestimmten Szenarien möchten wir möglicherweise unterschiedliche Daten oder Informationen anzeigen, je nachdem, ob ein authentifizierter oder anonymer Benutzer die Seite besucht. Außerdem müssen wir oft die Identität des authentifizierten Benutzers kennen.

Wir erweitern die vorhandene default. aspx-Seite, um diese Techniken zu veranschaulichen. Fügen Sie in "default. aspx" zwei Panel-Steuerelemente hinzu, eine mit dem Namen "Authenti" und "anonymousmessagepanel". Fügen Sie im ersten Bereich ein Label-Steuerelement namens welcomebackmessage hinzu. Fügen Sie im zweiten Bereich ein Hyperlink-Steuerelement hinzu, legen Sie dessen Text-Eigenschaft auf anmelden und deren NavigateUrl-Eigenschaft auf ~/Login.aspx. An diesem Punkt sollte das deklarative Markup für "default. aspx" in etwa wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Wie Sie wahrscheinlich schon erraten haben, besteht die Idee darin, nur das Authenti-edmessagepanel-Element für authentifizierte Besucher und nur das anonymousmessagepanel-Element für anonyme Besucher anzuzeigen. Um dies zu erreichen, müssen wir die sichtbaren Eigenschaften dieser Panels abhängig davon festlegen, ob der Benutzer angemeldet ist oder nicht.

Die [Request. IsAuthenticated-Eigenschaft](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) gibt einen booleschen Wert zurück, der angibt, ob die Anforderung authentifiziert wurde. Geben Sie den folgenden Code in die Seite\_Laden des Ereignishandlercodes ein:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Wenn dieser Code vorhanden ist, besuchen Sie "default. aspx" über einen Browser. Wenn Sie sich noch anmelden müssen, wird ein Link zur Anmeldeseite angezeigt (siehe Abbildung 11). Klicken Sie auf diesen Link, und melden Sie sich bei der Website an. Wie wir in Schritt 3 gesehen haben, werden Sie nach der Eingabe Ihrer Anmelde Informationen an default. aspx zurückgegeben, aber dieses Mal zeigt die Seite die Willkommensseite an. Message (siehe Abbildung 12).

[![beim anonymen Besuch wird ein Link "Anmelden" angezeigt.](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Abbildung 11**: beim anonymen Besuch wird ein Link zum Anmelden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image33.png))

[![authentifizierten Benutzern wird die Willkommensseite angezeigt. Nachricht](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Abbildung 12**: authentifizierte Benutzer werden die Willkommensseite angezeigt! Meldung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image36.png))

Die Identität des aktuell angemeldeten Benutzers kann anhand der [User-Eigenschaft](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)des [HttpContext-Objekts](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)ermittelt werden. Das HttpContext-Objekt stellt Informationen über die aktuelle Anforderung dar und ist das Zuhause für solche allgemeinen ASP.NET-Objekte als Antwort, Anforderung und Sitzung. Die User-Eigenschaft stellt den Sicherheitskontext der aktuellen HTTP-Anforderung dar und implementiert die [IPrincipal-Schnittstelle](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Die User-Eigenschaft wird von FormsAuthenticationModule festgelegt. Insbesondere, wenn das FormsAuthenticationModule-Objekt in der eingehenden Anforderung ein Formular Authentifizierungs Ticket findet, erstellt es ein neues GenericPrincipal-Objekt und weist es der User-Eigenschaft zu.

Prinzipal Objekte (z. b. GenericPrincipal) stellen Informationen über die Identität des Benutzers und die Rollen bereit, zu denen Sie gehören. Die IPrincipal-Schnittstelle definiert zwei Member:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) : eine Methode, die einen booleschen Wert zurückgibt, der angibt, ob der Prinzipal zur angegebenen Rolle gehört.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) : eine Eigenschaft, die ein Objekt zurückgibt, das die [IIdentity-Schnittstelle](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx)implementiert. Die IIdentity-Schnittstelle definiert drei Eigenschaften: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)und [Name](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Der Name des aktuellen Besuchers kann mithilfe des folgenden Codes bestimmt werden:

Currentusersname als String = User.Identity.Name

Wenn Sie die Formular Authentifizierung verwenden, wird für die Identity-Eigenschaft des GenericPrincipal ein [FormsIdentity-Objekt](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) erstellt. Die FormsIdentity-Klasse gibt immer die Zeichen folgen Formulare für Ihre AuthenticationType-Eigenschaft und true für Ihre IsAuthenticated-Eigenschaft zurück. Die Name-Eigenschaft gibt den Benutzernamen zurück, der beim Erstellen des Formular Authentifizierungs Tickets angegeben wurde. Zusätzlich zu diesen drei Eigenschaften schließt FormsIdentity den Zugriff auf das zugrunde liegende Authentifizierungs Ticket über die [Ticket-Eigenschaft](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx)ein. Die Ticket-Eigenschaft gibt ein Objekt vom Typ [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)zurück, das Eigenschaften wie "Ablauf", "isPersistent", "IssueDate", "Name" usw. hat.

Wichtig ist hierbei, dass der *username* -Parameter, der in den Methoden FormsAuthentication. GetAuthCookie (*username*, *persistcookie*), FormsAuthentication. SetAuthCookie (*username*, *persistcookie*) und FormsAuthentication. RedirectFromLoginPage (*username*, *persistcookie*) angegeben ist, der gleiche Wert ist, der von User.Identity.Name zurückgegeben wird. Außerdem ist das Authentifizierungs Ticket, das von diesen Methoden erstellt wird, verfügbar, indem User. Identity in ein FormsIdentity-Objekt umgewandelt und dann auf die Ticket-Eigenschaft zugegriffen wird:

Dim Ident as FormsIdentity = CType (User. Identity, FormsIdentity)

Dim authTicket als FormsAuthenticationTicket = Ident. Preis

Wir geben in "default. aspx" eine personalisierte Nachricht an. Aktualisieren Sie die Seite\_Lade Ereignishandler, sodass der Text-Eigenschaft der "welcomebackmessage"-Bezeichnung die Zeichenfolge Willkommen zurück, *username*!

Welcomebackmessage. Text = "Willkommen zurück," &amp; User.Identity.Name &amp; "!"

Abbildung 13 zeigt die Auswirkung dieser Änderung (bei der Anmeldung als Benutzer Scott).

[![die Begrüßungsnachricht den Namen des aktuell angemeldeten Benutzers enthält.](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Abbildung 13**: die Willkommensnachricht enthält den Namen des aktuell angemeldeten Benutzers ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image39.png))

### <a name="using-the-loginview-and-loginname-controls"></a>Verwenden der LoginView-und LoginName-Steuerelemente

Die Anzeige verschiedener Inhalte für authentifizierte und anonyme Benutzer ist eine häufige Anforderung. zeigt also den Namen des aktuell angemeldeten Benutzers an. Aus diesem Grund enthält ASP.net zwei websteuer Elemente, die die gleiche Funktionalität wie in Abbildung 13 dargestellt, aber ohne eine einzige Codezeile schreiben zu müssen.

Das [LoginView-Steuer](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) Element ist ein auf Vorlagen basierendes websteuer Element, mit dem unterschiedliche Daten für authentifizierte und anonyme Benutzer leicht angezeigt werden können. Die LoginView enthält zwei vordefinierte Vorlagen:

- AnonymousTemplate-jedes dieser Vorlage hinzugefügte Markup wird nur für anonyme Besucher angezeigt.
- LoggedInTemplate: das Markup dieser Vorlage wird nur authentifizierten Benutzern angezeigt.

Fügen wir das LoginView-Steuerelement zur Master Seite der Website, Site. Master, hinzu. Anstatt nur das LoginView-Steuerelement hinzuzufügen, fügen wir jedoch ein neues contentplachalter-Steuerelement hinzu und platzieren dann das LoginView-Steuerelement in diesem neuen contentplachalter-Steuerelement. Die Begründung für diese Entscheidung wird in Kürze offensichtlich.

> [!NOTE]
> Neben "AnonymousTemplate" und "LoggedInTemplate" kann das LoginView-Steuerelement Rollen spezifische Vorlagen enthalten. Rollen spezifische Vorlagen zeigen Markup nur für die Benutzer an, die zu einer bestimmten Rolle gehören. Wir werden die rollenbasierten Features des LoginView-Steuer Elements in einem zukünftigen Tutorial untersuchen.

Fügen Sie zunächst einen contentplachalter namens logincontent in der Master Seite im Navigations &lt;div&gt;-Element hinzu. Sie können einfach ein contentplachalter-Steuerelement aus der Toolbox in die Quell Ansicht ziehen, indem Sie das resultierende Markup direkt oberhalb des TODO:-Menüs platzieren.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Fügen Sie als nächstes ein LoginView-Steuerelement in den logincontent-contentplachalter ein. Inhalt, der in die contentplachalter-Steuerelemente der Master Seite eingefügt wird, gilt als *Standard Inhalt* für den contentplachalter. Das heißt, ASP.NET Seiten, die diese Master Seite verwenden, können Ihren eigenen Inhalt für jeden contentplachalter angeben oder den Standard Inhalt der Master Seite verwenden.

Die LoginView-und anderen Anmelde bezogenen Steuerelemente befinden sich auf der Registerkarte Anmeldung der Toolbox.

[![Sie das LoginView-Steuerelement in der Toolbox.](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Abbildung 14**: das LoginView-Steuerelement in der Toolbox ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image42.png))

Fügen Sie als nächstes zwei &lt;BR/&gt;-Elemente direkt nach dem LoginView-Steuerelement hinzu, aber noch innerhalb des contentplachalter. An diesem Punkt sollte das Markup des Navigations &lt;div&gt; Elements wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Die LoginView-Vorlagen können im Designer oder im deklarativen Markup definiert werden. Erweitern Sie im Visual Studio-Designer das Smarttag von LoginView, das die konfigurierten Vorlagen in einer Dropdown Liste auflistet. Geben Sie den Text Hello, Stranger in das AnonymousTemplate-Zeichen ein. Fügen Sie als nächstes ein Hyperlink-Steuerelement hinzu, und legen Sie dessen Text-und NavigateUrl-Eigenschaften auf anmelden bzw. ~/Login.aspx fest.

Wechseln Sie nach dem Konfigurieren von AnonymousTemplate zu LoggedInTemplate, und geben Sie den Text "Welcome Back" ein. Ziehen Sie dann ein LoginName-Steuerelement aus der Toolbox in die LoggedInTemplate, und platzieren Sie es unmittelbar nach dem Text "Willkommen zurück". Das [LoginName-Steuer](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx)Element, wie der Name schon sagt, zeigt den Namen des aktuell angemeldeten Benutzers an. Intern gibt das LoginName-Steuerelement einfach die User.Identity.Name-Eigenschaft aus.

Nachdem diese Ergänzungen den Vorlagen von LoginView hinzugefügt wurden, sollte das Markup in etwa wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Auf der Seite "Site. Master Master" wird auf jeder Seite der Website eine andere Meldung angezeigt, je nachdem, ob der Benutzer authentifiziert ist. Abbildung 15 zeigt die Seite "default. aspx", wenn Sie über einen Browser von User jisun besucht wird. Die "Willkommen"-Meldung "jisun" wird zweimal wiederholt: einmal im Navigationsbereich der Master Seite auf der linken Seite (über das soeben hinzugefügte LoginView-Steuerelement) und einmal im Inhalts Bereich von "default. aspx" (über Panel-Steuerelemente und programmgesteuerte Logik).

[![dem LoginView-Steuerelement wird Willkommen zurück, jisun angezeigt.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Abbildung 15**: das LoginView-Steuerelement zeigt Willkommen zurück, jisun. ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image45.png))

Da wir der Master Seite die LoginView hinzugefügt haben, kann Sie auf jeder Seite auf unserer Website angezeigt werden. Es gibt jedoch möglicherweise Webseiten, auf denen diese Meldung nicht angezeigt werden soll. Eine solche Seite ist die Anmeldeseite, da dort ein Link zur Anmeldeseite angezeigt wird. Da wir das LoginView-Steuerelement in einem contentplachalter-Element auf der Master Seite abgelegt haben, können wir dieses Standard Markup auf unserer Inhaltsseite überschreiben. Öffnen Sie Login. aspx, und wechseln Sie zum Designer. Da wir in Login. aspx nicht explizit ein Inhalts Steuerelement für den logincontent-contentplachalter auf der Master Seite definiert haben, wird auf der Anmeldeseite das Standard Markup der Master Seite für diesen contentplachalter angezeigt. Dies kann über den Designer angezeigt werden. der logincontent-contentplachalter zeigt das Standard Markup (das LoginView-Steuerelement).

[![die Anmeldeseite den Standard Inhalt für den logincontent-contentplachalter der Master Seite anzeigt.](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Abbildung 16**: die Anmeldeseite zeigt den Standard Inhalt für den logincontent-contentplachalter der Master Seite[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image48.png))

Um das Standard Markup für den logincontent-contentplachalter zu überschreiben, klicken Sie einfach mit der rechten Maustaste auf den Bereich im Designer, und wählen Sie im Kontextmenü die Option benutzerdefinierten Inhalt erstellen aus. (Bei Verwendung von Visual Studio 2008 enthält der contentplachalter ein Smarttag, das die gleiche Option bietet, wenn diese Option ausgewählt ist.) Dadurch wird dem Markup der Seite ein neues Inhalts Steuerelement hinzugefügt, sodass wir benutzerdefinierten Inhalt für diese Seite definieren können. Hier können Sie eine benutzerdefinierte Meldung hinzufügen, z. b. Melden Sie sich an, aber lassen Sie das Feld leer.

> [!NOTE]
> In Visual Studio 2005 wird durch das Erstellen von benutzerdefiniertem Inhalt ein leeres Inhalts Steuerelement auf der ASP.NET-Seite erstellt. In Visual Studio 2008 wird durch das Erstellen von benutzerdefiniertem Inhalt jedoch der Standard Inhalt der Master Seite in das neu erstellte Inhalts Steuerelement kopiert. Wenn Sie Visual Studio 2008 verwenden, stellen Sie nach dem Erstellen des neuen Inhalts Steuer Elements sicher, dass Sie den Inhalt löschen, der von der Master Seite kopiert wurde.

In Abbildung 17 wird die Seite Login. aspx angezeigt, wenn Sie von einem Browser aus nach dieser Änderung besucht werden. Beachten Sie, dass es im linken Navigations &lt;div&gt; wie beim Besuch von "default. aspx" *keine Meldung "* Hello, Stranger" oder "Willkommen" angezeigt wird.

[auf der Anmeldeseite wird das standardmäßige logincontent contentplachalter-Markup ausgeblendet. ![](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Abbildung 17**: die Anmeldeseite blendet das standardmäßige logincontent contentplacmage-Markup aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image51.png))

## <a name="step-5-logging-out"></a>Schritt 5: abmelden

In Schritt 3 haben wir uns mit dem Aufbau einer Anmeldeseite zum Anmelden eines Benutzers bei der Website befasst, aber wir haben noch erfahren, wie Sie einen Benutzer abmelden. Zusätzlich zu den Methoden für die Protokollierung eines Benutzers in stellt die FormsAuthentication-Klasse auch eine [SignOut-Methode](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)bereit. Mit der SignOut-Methode wird einfach das Formular Authentifizierungs Ticket zerstört, sodass der Benutzer von der Website abgemeldet wird.

Das anbieten eines Abmelde Links ist ein typisches Feature, das ASP.net ein Steuerelement enthält, das speziell für die Protokollierung eines Benutzers entworfen wurde. Das [LoginStatus-Steuer](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) Element zeigt entweder eine Anmeldelink Schaltfläche oder eine Logout-Link Schaltfläche an, je nach Authentifizierungs Status des Benutzers. Eine Anmeldelink Schaltfläche wird für anonyme Benutzer gerendert, während authentifizierten Benutzern eine Abmeldelink Schaltfläche angezeigt wird. Der Text für die Link Schaltflächen Login und Logout kann über die Eigenschaften LoginText und LogoutText von LoginStatus konfiguriert werden.

Wenn Sie auf den Link Button Login klicken, wird ein Postback ausgelöst, von dem aus eine Umleitung an die Anmeldeseite ausgegeben wird. Wenn Sie auf die Schaltfläche Logout (Abmelden) klicken, ruft das LoginStatus-Steuerelement die FormsAuthentication. Signoff-Methode auf und leitet den Benutzer dann zu einer Seite um. Die Seite, an die der angemeldete Benutzer umgeleitet wird, hängt von der LogoutAction-Eigenschaft ab, die einem der folgenden drei Werte zugewiesen werden kann:

- Refresh (Standard); leitet den Benutzer an die Seite um, die Sie gerade besucht haben. Wenn die Seite, die Sie gerade besucht haben, keine anonymen Benutzer zulässt, leitet das FormsAuthenticationModule den Benutzer automatisch an die Anmeldeseite weiter.

Sie sind möglicherweise neugierig, warum hier eine Umleitung erfolgt. Warum ist die explizite Umleitung erforderlich, wenn der Benutzer auf der gleichen Seite bleiben möchte? Der Grund hierfür ist, dass der Benutzer nach dem Klicken auf die Abmeldelink Schaltfläche weiterhin über das Formular Authentifizierungs Ticket in der Cookies-Sammlung verfügt. Folglich ist die Post Back Anforderung eine authentifizierte Anforderung. Das LoginStatus-Steuerelement ruft die SignOut-Methode auf, aber dies geschieht, nachdem das FormsAuthenticationModule den Benutzer authentifiziert hat. Aus diesem Grund bewirkt eine explizite Umleitung, dass der Browser die Seite erneut anfordert. Wenn der Browser die Seite erneut anfordert, wurde das Formular Authentifizierungs Ticket entfernt, und daher ist die eingehende Anforderung anonym.

- Umleitung: der Benutzer wird an die URL umgeleitet, die von der LogoutPageUrl-Eigenschaft von LoginStatus angegeben wird.
- RedirectToLoginPage: der Benutzer wird zur Anmeldeseite umgeleitet.

Fügen Sie der Master Seite ein LoginStatus-Steuerelement hinzu, und konfigurieren Sie es so, dass der Benutzer mithilfe der Umleitungs Option an eine Seite gesendet wird, auf der eine Meldung mit dem Hinweis angezeigt wird, dass Sie abgemeldet wurden. Erstellen Sie zunächst eine Seite im Stammverzeichnis mit dem Namen Logout. aspx. Vergessen Sie nicht, diese Seite der Master Seite Site. Master zuzuordnen. Geben Sie als nächstes eine Meldung in das Markup der Seite ein, die dem Benutzer erklärt, dass diese abgemeldet wurden.

Kehren Sie als nächstes zur Seite "Site. Master Master" zurück, und fügen Sie ein LoginStatus-Steuerelement unterhalb von "LoginView" im Inhaltsort "logincontent" hinzu. Legen Sie die LogoutAction-Eigenschaft des LoginStatus-Steuer Elements auf Redirect und deren LogoutPageUrl-Eigenschaft auf ~/Logout.aspx fest.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Da der LoginStatus außerhalb des LoginView-Steuer Elements liegt, wird er sowohl für anonyme als auch für authentifizierte Benutzer angezeigt, aber das ist in Ordnung, da der LoginStatus eine Anmelde-oder Abmeldelink Schaltfläche korrekt anzeigt. Wenn das LoginStatus-Steuerelement hinzugefügt wurde, ist der Link "Anmelden" in der "AnonymousTemplate"-Tabelle überflüssig. entfernen Sie ihn daher.

Abbildung 18 zeigt "default. aspx" beim Besuch von jisun. Beachten Sie, dass in der linken Spalte die Meldung, Willkommen zurück, jisun und ein Link zum Abmelden angezeigt werden. Wenn Sie auf die Link Schaltfläche "Abmelden" klicken, wird ein Postback ausgelöst, der jisun wird vom System signiert und dann an "Logout. aspx" umgeleitet. Die Abbildung 19 zeigt, dass jisun ab dem Zeitpunkt der Abmeldung abgemeldet wird. aspx ist bereits abgemeldet und daher anonym. Folglich werden in der linken Spalte der Text willkommen, ein unbekannter und ein Link zur Anmeldeseite angezeigt.

[![default. aspx zeigt Willkommen zurück, jisun und eine Abmeldelink Schaltfläche an.](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Abbildung 18**: default. aspx zeigt Willkommen zurück, jisun und einen Abmeldelink Button ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image54.png))

[![Logout. aspx zeigt willkommen, Stranger und einen Link Button für die Anmeldung an.](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Abbildung 19**: Logout. aspx zeigt willkommen, Stranger und eine Link[Schaltfläche für die Anmeldung (Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-forms-authentication-vb/_static/image57.png))

> [!NOTE]
> Ich empfehle Ihnen, die Seite Logout. aspx so anzupassen, dass der logincontent-contentplachalter der Master Seite ausgeblendet wird (wie in Schritt 4 in "Login. aspx"). Der Grund hierfür ist, dass der vom LoginStatus-Steuerelement (der unter Hello, Stranger) gerenderte Link Button den Benutzer an die Anmeldeseite sendet, wobei die aktuelle URL im Parameter "QueryString" von "Rückkehrer" übergeben wird. Kurz gesagt, wenn ein Benutzer, der sich abgemeldet hat, auf den Anmeldelink für LoginStatus klickt und sich dann anmeldet, wird er zurück zu Logout. aspx umgeleitet, was den Benutzer leicht verwirren könnte.

## <a name="summary"></a>Summary

In diesem Tutorial haben wir mit einer Untersuchung des Workflows zur Formular Authentifizierung begonnen und dann die Formular Authentifizierung in einer ASP.NET-Anwendung implementiert. Die Formular Authentifizierung basiert auf dem FormsAuthenticationModule, das zwei Zuständigkeiten hat: die Identifizierung von Benutzern auf der Grundlage ihres Formular Authentifizierungs Tickets und die Umleitung nicht autorisierter Benutzer auf die Anmeldeseite.

Die FormsAuthentication-Klasse des .NET Framework enthält Methoden zum Erstellen, überprüfen und Entfernen von Formular Authentifizierungs Tickets. Die Eigenschaft "Request. IsAuthenticated" und das Benutzerobjekt bieten zusätzliche programmgesteuerte Unterstützung für die Bestimmung, ob eine Anforderung authentifiziert ist, sowie Informationen zur Identität des Benutzers. Außerdem gibt es die websteuer Elemente LoginView, LoginStatus und LoginName, die Entwicklern eine schnelle, Code freie Methode zum Ausführen zahlreicher allgemeiner Anmelde bezogener Aufgaben zur Verfügung stellen. In zukünftigen Tutorials werden diese und andere websteuer Elemente im Zusammenhang mit der Anmeldung ausführlicher erläutert.

In diesem Tutorial wurde eine kurze Übersicht über die Formular Authentifizierung bereitgestellt. Wir haben die Optionen für die verschiedenen Konfigurationen nicht untersucht, sehen uns an, wie cookischlose Formular Authentifizierungs Tickets funktionieren, oder Sie untersuchen, wie ASP.NET den Inhalt des Formular Authentifizierungs Tickets schützt. Diese Themen und weitere Informationen finden Sie im [nächsten Tutorial](forms-authentication-configuration-and-advanced-topics-vb.md).

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Änderungen zwischen IIS6-und IIS7-Sicherheit](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Login ASP.NET Steuerelemente](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2,0 Sicherheit, Mitgliedschaft und Rollen Verwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Das &lt;Authentication&gt;-Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Das &lt;Forms&gt; Element für &lt;Authentifizierung&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video Schulung zu Themen in diesem Tutorial

- [Verwenden der grundlegenden Formularauthentifizierung in ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Zu den führenden Reviewern für dieses Tutorial zählen Alicja Maziarz, John suru und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](security-basics-and-asp-net-support-vb.md)
> [Weiter](forms-authentication-configuration-and-advanced-topics-vb.md)
