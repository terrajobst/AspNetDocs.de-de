---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Protokollieren von Fehler Details mit ELMAH (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Fehler Protokollierungs Module und-Handler (ELMAH) bieten einen weiteren Ansatz zum Protokollieren von Laufzeitfehlern in einer Produktionsumgebung. ELMAH ist ein kostenloser Open Source-Fehler...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: 46b7fc22807c8cb9f47ff035639815d7b6104735
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421905"
---
# <a name="logging-error-details-with-elmah-vb"></a>Protokollieren von Fehlerdetails mit ELMAH (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Fehler Protokollierungs Module und-Handler (ELMAH) bieten einen weiteren Ansatz zum Protokollieren von Laufzeitfehlern in einer Produktionsumgebung. ELMAH ist eine kostenlose Open Source-Fehler Protokollierungs Bibliothek, die Features wie Fehler Filterung und die Möglichkeit zum Anzeigen des Fehler Protokolls auf einer Webseite, als RSS-Feed oder zum Herunterladen der Datei als durch Trennzeichen getrennte Datei enthält. Dieses Tutorial führt Sie durch das herunterladen und Konfigurieren von ELMAH.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](logging-error-details-with-asp-net-health-monitoring-vb.md) wurde ASP untersucht. Das System Überwachungssystem von NET, das eine Out-of-Box-Bibliothek für die Aufzeichnung eines breiten Arrays von Webanwendungen bietet. Viele Entwickler verwenden die Integritäts Überwachung, um Details von nicht behandelten Ausnahmen zu protokollieren und per e-Mail zu senden. Es gibt jedoch einige Probleme mit diesem System. Der erste und wichtigste besteht darin, dass keine Benutzeroberfläche für die Anzeige von Informationen zu den protokollierten Ereignissen fehlt. Wenn Sie eine Zusammenfassung der 10 letzten Fehler anzeigen oder die Details eines Fehlers anzeigen möchten, der in der letzten Woche aufgetreten ist, müssen Sie entweder die Datenbank durchsuchen, Ihren e-Mail-Posteingang durchsuchen oder eine Webseite erstellen, auf der Informationen aus der `aspnet_WebEvent_Events` Tabelle angezeigt werden.

Ein weiterer Punkt liegt in der Komplexität der Integritäts Überwachung. Da die Integritäts Überwachung verwendet werden kann, um eine Vielzahl verschiedener Ereignisse aufzuzeichnen, und da es eine Vielzahl von Optionen gibt, um anzuweisen, wie und wann Ereignisse protokolliert werden, kann das ordnungsgemäße Konfigurieren des System Überwachungssystems eine schwierige Aufgabe sein. Schließlich sind Kompatibilitätsprobleme aufgetreten. Da die Systemüberwachung zuerst der .NET Framework in Version 2,0 hinzugefügt wurde, ist Sie für ältere Webanwendungen, die mit ASP.NET Version 1. x erstellt wurden, nicht verfügbar. Außerdem funktioniert die `SqlWebEventProvider`-Klasse, die wir im vorherigen Tutorial verwendet haben, um Fehlerdetails in einer-Datenbank zu protokollieren, nur mit Microsoft SQL Server-Datenbanken. Sie müssen eine benutzerdefinierte Protokoll Anbieter Klasse erstellen, wenn Sie Fehler in einem alternativen Datenspeicher, z. b. einer XML-Datei oder einer Oracle-Datenbank, protokollieren müssen.

Eine Alternative zum System Überwachungssystem ist Fehler Protokollierung Module und Handler (ELMAH), ein kostenloses Open-Source-Fehler Protokollierungs System, das von [Atif Aziz](http://www.raboof.com/)erstellt wurde. Der wichtigste Unterschied zwischen den beiden Systemen besteht darin, dass elamh eine Liste von Fehlern und Details zu einem bestimmten Fehler von einer Webseite und als RSS-Feed anzeigen kann. ELMAH ist einfacher zu konfigurieren als die Integritäts Überwachung, da nur Fehler protokolliert werden. Außerdem bietet ELMAH Unterstützung für ASP.NET 1. x, ASP.NET 2,0 und ASP.NET 3,5-Anwendungen und wird mit einer Vielzahl von Protokoll Quellen Anbietern ausgeliefert.

Dieses Tutorial führt Sie durch die Schritte zum Hinzufügen von ELMAH zu einer ASP.NET-Anwendung. Erste Schritte

> [!NOTE]
> Das System Überwachungssystem und ELMAH verfügen jeweils über eigene Sätze von vor-und Nachteile. Ich empfehle Ihnen, beide Systeme auszuprobieren und zu entscheiden, was für Ihre Anforderungen am besten geeignet ist.

## <a name="adding-elmah-to-an-aspnet-web-application"></a>Hinzufügen von ELMAH zu einer ASP.NET-Webanwendung

Das Integrieren von ELMAH in eine neue oder vorhandene ASP.NET-Anwendung ist ein einfacher und einfacher Prozess, der weniger als fünf Minuten dauert. Kurz gesagt, sind vier einfache Schritte erforderlich:

1. Herunterladen von ELMAH und Hinzufügen der `Elmah.dll`-Assembly zu Ihrer Webanwendung
2. Registrieren Sie die HTTP-Module und den Handler von ELMAH in `Web.config`.
3. Geben Sie die Konfigurationsoptionen von ELMAH an, und
4. Erstellen Sie bei Bedarf die Infrastruktur für Fehlerprotokoll Quellen.

Sehen wir uns die einzelnen vier Schritte nacheinander einmal an.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Schritt 1: Herunterladen der ELMAH-Projektdateien und Hinzufügen von`Elmah.dll`zu Ihrer Webanwendung

ELMAH 1,0 Beta 3 (Build 10617), die aktuellste Version zum Zeitpunkt des Schreibens, ist in dem Download enthalten, der in diesem Tutorial verfügbar ist. Alternativ können Sie die [ELMAH-Website](https://code.google.com/p/elmah/) besuchen, um die neueste Version zu erhalten oder den Quellcode herunterzuladen. Extrahieren Sie den ELMAH-Download in einen Ordner auf Ihrem Desktop, und suchen Sie die ELMAH-Assemblydatei (`Elmah.dll`).

> [!NOTE]
> Die `Elmah.dll` Datei befindet sich im `Bin` Ordner des Downloads, der Unterordner für verschiedene .NET Framework Versionen und für Release-und Debugbuilds enthält. Verwenden Sie den Releasebuild für die entsprechende Frameworkversion. Wenn Sie beispielsweise eine ASP.NET 3,5-Webanwendung aufbauen, kopieren Sie die `Elmah.dll` Datei aus dem Ordner `Bin\net-3.5\Release`.

Öffnen Sie als nächstes Visual Studio, und fügen Sie dem Projekt die Assembly hinzu, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Namen der Website klicken und im Kontextmenü Verweis hinzufügen auswählen. Dadurch wird das Dialogfeld Verweis hinzufügen geöffnet. Navigieren Sie zur Registerkarte Durchsuchen, und wählen Sie die `Elmah.dll` Datei aus. Durch diese Aktion wird die `Elmah.dll` Datei dem Ordner `Bin` der Webanwendung hinzugefügt.

> [!NOTE]
> Der Typ des Webanwendungs Projekts (WAP) zeigt den `Bin` Ordner in der Projektmappen-Explorer nicht an. Stattdessen werden diese Elemente unter dem Ordner Verweise aufgelistet.

Die `Elmah.dll`-Assembly enthält die Klassen, die vom ELMAH-System verwendet werden. Diese Klassen sind in eine von drei Kategorien unterteilt:

- **HTTP-Module** : ein HTTP-Modul ist eine Klasse, die Ereignishandler für `HttpApplication` Ereignisse definiert, z. b. das `Error`-Ereignis. ELMAH umfasst mehrere HTTP-Module, die jeweils die drei wichtigsten sind: 

    - in `ErrorLogModule` werden nicht behandelte Ausnahmen in einer Protokoll Quelle protokolliert.
    - `ErrorMailModule`: sendet die Details einer nicht behandelten Ausnahme in einer e-Mail-Nachricht.
    - `ErrorFilterModule` wendet vom Entwickler angegebene Filter an, um zu bestimmen, welche Ausnahmen protokolliert und welche ignoriert werden.
- **Http** -Handler: ein HTTP-Handler ist eine Klasse, die für das Erstellen des Markups für einen bestimmten Anforderungstyp zuständig ist. ELMAH enthält HTTP-Handler, die Fehlerdetails als Webseite, als RSS-Feed oder als durch Trennzeichen getrennte Datei (CSV) darstellen.
- **Fehlerprotokoll Quellen** : Out of the Box ELMAH kann Fehler im Arbeitsspeicher, in einer Microsoft SQL Server Datenbank, in einer Microsoft Access-Datenbank, in eine Oracle-Datenbank, in eine XML-Datei, in eine SQLite-Datenbank oder in eine Vista DB-Datenbank protokollieren. Ebenso wie das System Überwachungssystem wurde die Architektur von ELMAH mit dem Anbieter Modell erstellt, was bedeutet, dass Sie bei Bedarf Ihre eigenen benutzerdefinierten Protokoll Quellen Anbieter erstellen und nahtlos integrieren können.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Schritt 2: Registrieren des HTTP-Moduls und-Handlers von ELMAH

Die `Elmah.dll`-Datei enthält die HTTP-Module und den Handler, die erforderlich sind, um nicht behandelte Ausnahmen automatisch zu protokollieren und Fehlerdetails von einer Webseite anzuzeigen. diese müssen explizit in der Konfiguration der Webanwendung registriert werden. Das `ErrorLogModule` HTTP-Modul abonniert nach der Registrierung das `Error` Ereignis des `HttpApplication`. Jedes Mal, wenn dieses Ereignis ausgelöst wird, protokolliert der `ErrorLogModule` die Details der Ausnahme an eine angegebene Protokoll Quelle. Wir sehen uns an, wie der Protokoll Quellen Anbieter im nächsten Abschnitt "Konfigurieren von ELMAH" definiert wird. Die `ErrorLogPageFactory` HTTP-Handlerfactory ist dafür verantwortlich, das Markup zu erzeugen, wenn das Fehlerprotokoll von einer Webseite angezeigt wird.

Die spezifische Syntax zum Registrieren von HTTP-Modulen und-Handlern hängt von dem Webserver ab, der die Website verwendet. Für den ASP.NET Development Server und die IIS-Version 6,0 und früher von Microsoft werden HTTP-Module und-Handler in den Abschnitten `<httpModules>` und `<httpHandlers>` registriert, die im `<system.web>`-Element angezeigt werden. Wenn Sie IIS 7,0 verwenden, müssen Sie in den Abschnitten `<modules>` und `<handlers>` des `<system.webServer>` Elements registriert werden. Glücklicherweise können Sie die HTTP-Module und-Handler an *beiden* stellen definieren, unabhängig davon, welcher Webserver verwendet wird. Diese Option ist die aktuellste Methode, da Sie die gleiche Konfiguration unabhängig vom verwendeten Webserver in der Entwicklungs-und Produktionsumgebung verwenden kann.

Beginnen Sie, indem Sie das `ErrorLogModule` HTTP-Modul und den `ErrorLogPageFactory` HTTP-Handler im `<httpModules>` und `<httpHandlers>` Abschnitt in `<system.web>`registrieren. Wenn Ihre Konfiguration diese beiden Elemente bereits definiert, fügen Sie einfach das `<add>`-Element für das HTTP-Modul und den Handler von ELMAH ein.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

Registrieren Sie als nächstes das HTTP-Modul und den Handler von ELMAH im `<system.webServer>` Element. Wenn dieses Element nicht bereits in der Konfiguration vorhanden ist, fügen Sie es wie zuvor hinzu.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

Standardmäßig meldet IIS 7, ob HTTP-Module und-Handler im `<system.web>` Abschnitt registriert sind. Das `validateIntegratedModeConfiguration`-Attribut im `<validation>`-Element weist IIS 7 an, solche Fehlermeldungen zu unterdrücken.

Beachten Sie, dass die Syntax zum Registrieren des `ErrorLogPageFactory` HTTP-Handlers ein `path`-Attribut enthält, das auf `elmah.axd`festgelegt ist. Mit diesem Attribut wird der Webanwendung mitgeteilt, dass die Anforderung vom `ErrorLogPageFactory` HTTP-Handler verarbeitet werden soll, wenn eine Anforderung für eine Seite mit dem Namen `elmah.axd` eingeht. Der `ErrorLogPageFactory` HTTP-Handler wird später in diesem Tutorial in Aktion angezeigt.

### <a name="step-3-configuring-elmah"></a>Schritt 3: Konfigurieren von ELMAH

ELMAH sucht in der `Web.config` Datei der Website in einem benutzerdefinierten Konfigurations Abschnitt mit dem Namen `<elmah>`nach seinen Konfigurationsoptionen. Um einen benutzerdefinierten Abschnitt in `Web.config` verwenden zu können, muss er zuerst im `<configSections>` Element definiert werden. Öffnen Sie die Datei `Web.config`, und fügen Sie dem `<configSections>`das folgende Markup hinzu:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Wenn Sie ELMAH für eine ASP.NET 1. x-Anwendung konfigurieren, entfernen Sie das `requirePermission="false"`-Attribut aus den oben aufgeführten `<section>` Elementen.

Die obige Syntax registriert den benutzerdefinierten `<elmah>` Abschnitt und seine Unterabschnitte: `<security>`, `<errorLog>`, `<errorMail>`und `<errorFilter>`.

Fügen Sie als nächstes den Abschnitt `<elmah>` `Web.config`hinzu. Dieser Abschnitt sollte auf der gleichen Ebene wie das `<system.web>`-Element angezeigt werden. Fügen Sie im Abschnitt `<elmah>` die `<security>` und `<errorLog>` Abschnitte wie folgt hinzu:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

Das `allowRemoteAccess`-Attribut des `<security>` Abschnitts gibt an, ob der Remote Zugriff zulässig ist. Wenn dieser Wert auf 0 festgelegt ist, kann die Fehlerprotokoll-Webseite nur lokal angezeigt werden. Wenn dieses Attribut auf 1 festgelegt ist, wird die Fehlerprotokoll-Webseite sowohl für Remote-als auch für lokale Besucher aktiviert. Deaktivieren Sie vorerst die Fehlerprotokoll-Webseite für Remote Besucher. Wir gestatten später den Remote Zugriff, wenn wir die Gelegenheit haben, die Sicherheitsbedenken zu erörtern.

Der `<errorLog>` Abschnitt definiert die Fehlerprotokoll Quelle, die festlegt, wo die Fehlerdetails aufgezeichnet werden. Dies ähnelt dem Abschnitt `<providers>` im System Überwachungssystem. Die obige Syntax gibt die `SqlErrorLog` Klasse als Fehlerprotokoll Quelle an, die die Fehler in einer Microsoft SQL Server Datenbank protokolliert, die durch den `connectionStringName`-Attribut Wert angegeben wird.

> [!NOTE]
> ELMAH wird mit zusätzlichen Fehlerprotokoll Anbietern ausgeliefert, die zum Protokollieren von Fehlern in einer XML-Datei, einer Microsoft Access-Datenbank, einer Oracle-Datenbank und anderen Daten speichern verwendet werden können. Informationen zur Verwendung dieser alternativen Fehlerprotokoll Anbieter finden Sie in der `Web.config`-Beispieldatei, die im ELMAH-Download enthalten ist.

### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Schritt 4: Erstellen der Infrastruktur für Fehlerprotokoll Quellen

Der `SqlErrorLog` Anbieter von ELMAH protokolliert Fehlerdetails für eine angegebene Microsoft SQL Server Datenbank. Der `SqlErrorLog`-Anbieter erwartet, dass diese Datenbank eine Tabelle mit dem Namen `ELMAH_Error` und drei gespeicherte Prozeduren hat: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`und `ELMAH_LogError`. Der ELMAH-Download enthält eine Datei mit dem Namen `SQLServer.sql` im Ordner `db`, der t-SQL zum Erstellen dieser Tabelle und dieser gespeicherten Prozeduren enthält. Sie müssen diese Anweisungen für die-Datenbank ausführen, um den `SqlErrorLog`-Anbieter zu verwenden.

Die **Abbildungen 1** und **2** zeigen die Datenbank-Explorer in Visual Studio, nachdem die Datenbankobjekte, die vom `SqlErrorLog` Anbieter benötigt werden, hinzugefügt wurden.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Abbildung 1**: der `SqlErrorLog` Anbieter protokolliert Fehler in der `ELMAH_Error` Tabelle

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Abbildung 2**: der `SqlErrorLog` Anbieter verwendet drei gespeicherte Prozeduren

## <a name="elmah-in-action"></a>ELMAH in Aktion

An dieser Stelle haben wir ELMAH zur Webanwendung hinzugefügt, das `ErrorLogModule` HTTP-Modul und den `ErrorLogPageFactory` HTTP-Handler registriert, die Konfigurationsoptionen von ELMAH in `Web.config`angegeben und die benötigten Datenbankobjekte für den `SqlErrorLog` Fehlerprotokoll Anbieter hinzugefügt. Wir sind nun bereit, ELMAH in Aktion zu sehen! Besuchen Sie die Book Reviews-Website, und besuchen Sie eine Seite, die einen Laufzeitfehler, z. b. `Genre.aspx?ID=foo`, oder eine nicht vorhandene Seite (z. b. `NoSuchPage.aspx`) generiert. Was Sie beim Besuchen dieser Seiten sehen, hängt von der `<customErrors>` Konfiguration und davon ab, ob Sie lokal oder Remote besuchen. (Weitere Informationen zu diesem Thema finden Sie im [Tutorial *Anzeigen einer benutzerdefinierten Fehlerseite* ](displaying-a-custom-error-page-vb.md) .)

ELMAH wirkt sich nicht auf den Inhalt aus, der dem Benutzer angezeigt wird, wenn eine nicht behandelte Ausnahme auftritt. die Details werden lediglich protokolliert. Auf dieses Fehlerprotokoll kann von der Webseite `elmah.axd` aus dem Stammverzeichnis Ihrer Website zugegriffen werden, z. b. `http://localhost/BookReviews/elmah.axd`. (Diese Datei ist nicht physisch in Ihrem Projekt vorhanden, aber wenn eine Anforderung für `elmah.axd`, sendet die Laufzeit Sie an den `ErrorLogPageFactory` HTTP-Handler, der das an den Browser zurück gesendete Markup generiert.)

> [!NOTE]
> Sie können auch die Seite "`elmah.axd`" verwenden, um ELMAH anzuweisen, einen Test Fehler zu generieren. Wenn Sie `elmah.axd/test` aufrufen (wie in `http://localhost/BookReviews/elmah.axd/test`), bewirkt ELMAH, dass eine Ausnahme vom Typ "`Elmah.TestException`" ausgelöst wird, die die folgende Fehlermeldung enthält: "Dies ist eine Test Ausnahme, die sicher ignoriert werden kann."

**Abbildung 3** zeigt das Fehlerprotokoll, wenn Sie `elmah.axd` aus der Entwicklungsumgebung besuchen.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Abbildung 3**: `Elmah.axd` zeigt das Fehlerprotokoll von einer Webseite an  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](logging-error-details-with-elmah-vb/_static/image7.png))

Das Fehlerprotokoll in **Abbildung 3** enthält sechs Fehler Einträge. Jeder Eintrag enthält den HTTP-Statuscode (404 oder 500, für diese Fehler), den Typ, die Beschreibung, den Namen des angemeldeten Benutzers, wenn der Fehler aufgetreten ist, und das Datum und die Uhrzeit. Wenn Sie auf den Link Details klicken, wird eine Seite mit der gleichen Fehlermeldung angezeigt, die im gelben Bildschirm Fehler Details (siehe **Abbildung 4**) zusammen mit den Werten der Server Variablen zum Zeitpunkt des Fehlers angezeigt wird (siehe **Abbildung 5**). Sie können auch das unformatierte XML anzeigen, in dem die Fehlerdetails gespeichert werden. Dies schließt zusätzliche Informationen ein, z. b. die Werte im HTTP Post-Header.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Abbildung 4**: Anzeigen der Fehler Details Ysod  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Abbildung 5**: Untersuchen der Werte der Auflistung der Server Variablen zum Zeitpunkt des Fehlers  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](logging-error-details-with-elmah-vb/_static/image13.png))

Die Bereitstellung von ELMAH für die Produktions Website umfasst Folgendes:

- Kopieren der `Elmah.dll` Datei in den Ordner "`Bin`" in der Produktionsumgebung
- Kopieren der ELMAH-spezifischen Konfigurationseinstellungen in die `Web.config` Datei, die in der Produktionsumgebung verwendet wird, und
- Fügen Sie der Produktionsdatenbank die Infrastruktur der Fehlerprotokoll Quelle hinzu.

Wir haben Verfahren zum Kopieren von Dateien aus der Entwicklung in die Produktion in vorherigen Tutorials untersucht. Die einfachste Möglichkeit, die Infrastruktur für Fehlerprotokoll Quellen in der Produktionsdatenbank zu erhalten, besteht darin, SQL Server Management Studio zum Herstellen einer Verbindung mit der Produktionsdatenbank zu verwenden und dann die `SqlServer.sql` Skriptdatei auszuführen, die die benötigte Tabelle und die erforderlichen gespeicherten Prozeduren erstellt.

### <a name="viewing-the-error-details-page-on-production"></a>Anzeigen der Seite "Fehler Details" in der Produktion

Nachdem Sie Ihren Standort in der Produktionsumgebung bereitgestellt haben, besuchen Sie die Produktions Website und generieren eine nicht behandelte Ausnahme. Wie in der Entwicklungsumgebung hat ELMAH keine Auswirkung auf die Fehlerseite, die angezeigt wird, wenn eine nicht behandelte Ausnahme auftritt. Stattdessen wird der Fehler lediglich protokolliert. Wenn Sie versuchen, die Seite "Fehlerprotokoll" (`elmah.axd`) aus der Produktionsumgebung aufzurufen, werden Sie in **Abbildung 6**mit der Seite "verboten" angezeigt.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Abbildung 6**: Standardmäßig können Remote Besucher die Fehlerprotokoll-Webseite nicht anzeigen.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](logging-error-details-with-elmah-vb/_static/image16.png))

Beachten Sie, dass im `<security>` Abschnitt der ELMAH-Konfiguration das `allowRemoteAccess`-Attribut auf 0 festgelegt wird. Dadurch wird verhindert, dass Remote Benutzer das Fehlerprotokoll anzeigen. Es ist wichtig, anonymen Besuchern das Anzeigen des Fehler Protokolls zu verbieten, da die Fehlerdetails Sicherheitsrisiken oder andere vertrauliche Informationen offenlegen können. Wenn Sie dieses Attribut auf 1 festlegen und den Remote Zugriff auf das Fehlerprotokoll aktivieren, ist es wichtig, den `elmah.axd` Pfad zu sperren, damit nur autorisierte Besucher darauf zugreifen können. Dies kann erreicht werden, indem der `Web.config` Datei ein `<location>` Element hinzugefügt wird.

Mit der folgenden Konfiguration können nur Benutzer in der Administrator Rolle auf die Fehlerprotokoll-Webseite zugreifen:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> Die Administrator Rolle und die drei Benutzer im System-Scott, jisun und Alice wurden im [Tutorial *Konfigurieren einer Website, die Anwendungsdienste verwendet,* ](configuring-a-website-that-uses-application-services-vb.md)hinzugefügt. Benutzer Scott und jisun sind Mitglieder der Administrator Rolle. Weitere Informationen zur Authentifizierung und Autorisierung finden Sie unter meine [Website-Sicherheits](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)Lernprogramme.

Das Fehlerprotokoll in der Produktionsumgebung kann nun von Remote Benutzern angezeigt werden. Weitere Informationen finden Sie in den **Abbildungen 3**, **4**und **5** für Screenshots der Webseite mit dem Fehlerprotokoll. Wenn jedoch ein anonymer oder nicht Administrator Benutzer versucht, die Fehlerprotokoll Seite anzuzeigen, wird er automatisch an die Anmeldeseite (`Login.aspx`) umgeleitet, wie in **Abbildung 7** gezeigt.

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Abbildung 7**: nicht autorisierte Benutzer werden automatisch zur Anmeldeseite umgeleitet.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Programm gesteuertes Protokollieren von Fehlern

Das `ErrorLogModule` HTTP-Modul von ELMAH protokolliert automatisch nicht behandelte Ausnahmen in der angegebenen Protokoll Quelle. Alternativ können Sie einen Fehler protokollieren, ohne dass eine nicht behandelte Ausnahme ausgelöst werden muss, indem Sie die `ErrorSignal`-Klasse und ihre `Raise`-Methode verwenden. Der `Raise`-Methode wird ein `Exception` Objekt übergebenen und protokolliert, als ob diese Ausnahme ausgelöst wurde und die ASP.NET-Laufzeit erreicht hätte, ohne verarbeitet zu werden. Der Unterschied besteht jedoch darin, dass die Anforderung nach dem Aufrufen der `Raise`-Methode weiterhin ordnungsgemäß ausgeführt wird, während eine ausgelöste, nicht behandelte Ausnahme die normale Ausführung der Anforderung unterbricht und bewirkt, dass die ASP.NET-Laufzeit die konfigurierte Fehlerseite anzeigt.

Die `ErrorSignal`-Klasse ist in Situationen nützlich, in denen möglicherweise Fehler auftreten, aber der Fehler ist für den gesamten ausgeführten Vorgang nicht schwerwiegend. Beispielsweise kann eine Website ein Formular enthalten, das die Eingabe des Benutzers annimmt, Sie in einer Datenbank speichert und dann dem Benutzer eine e-Mail sendet, in der er darüber informiert wird, dass diese Informationen verarbeitet wurden. Was geschieht, wenn die Informationen erfolgreich in der Datenbank gespeichert werden, beim Senden der e-Mail-Nachricht tritt jedoch ein Fehler auf? Eine Möglichkeit besteht darin, eine Ausnahme auszulösen und den Benutzer an die Fehlerseite zu senden. Dies könnte jedoch den Benutzer in die Meinung bringen, dass die eingegebenen Informationen nicht gespeichert wurden. Ein anderer Ansatz besteht darin, den e-Mail-bezogenen Fehler zu protokollieren, die Benutzer Darstellung jedoch in keiner Weise zu ändern. Dies ist der Ort, an dem die `ErrorSignal`-Klasse nützlich ist.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Fehler Benachrichtigung per e-Mail

Zusammen mit der Protokollierung von Fehlern in einer Datenbank kann ELMAH auch für das Senden von Fehlerdetails an einen angegebenen Empfänger konfiguriert werden. Diese Funktionalität wird durch das `ErrorMailModule` HTTP-Modul bereitgestellt. Daher müssen Sie dieses http-Modul in `Web.config` registrieren, um Fehlerdetails per e-Mail zu senden.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

Geben Sie als nächstes Informationen über die Fehler-e-Mail im Abschnitt `<errorMail>` des `<elmah>` Elements an, und geben Sie dabei den Absender und Empfänger der e-Mail sowie den Betreff und die Angabe an, ob die e-Mail asynchron gesendet wird.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Wenn die oben genannten Einstellungen vorhanden sind, sendet ELMAH immer dann, wenn ein Laufzeitfehler auftritt, eine e-Mail an support@example.com mit den Fehlerdetails. Die Fehler-e-Mail von ELMAH enthält die gleichen Informationen, die auf der Webseite mit den Fehlerdetails angezeigt werden, nämlich die Fehlermeldung, die Stapel Überwachung und die Server Variablen (siehe **Abbildung 4** und **5**). Die Fehler-e-Mail enthält auch die Ausnahme Details gelber Bildschirm Inhalt des Inhalts als Anlage (`YSOD.html`).

**Abbildung 8** zeigt die Fehler-e-Mail von ELMAH, die durch den Besuch `Genre.aspx?ID=foo`generiert Obwohl in **Abbildung 8** nur die Fehlermeldung und die Stapel Überwachung angezeigt werden, werden die Server Variablen weiter unten im e-Mail-Text angezeigt.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Abbildung 8**: Sie können ELMAH so konfigurieren, dass Fehler Details per e-Mail gesendet werden.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Nur Protokollierungs Fehler von Interesse

Standardmäßig protokolliert ELMAH die Details aller nicht behandelten Ausnahmen, einschließlich 404 und anderer HTTP-Fehler. Sie können ELMAH anweisen, diese oder andere Fehlertypen mithilfe der Fehler Filterung zu ignorieren. Die Filter Logik wird von ELMAH `ErrorFilterModule` HTTP-Modul ausgeführt, das Sie in `Web.config` registrieren müssen, um die Filter Logik zu verwenden. Die Regeln für das Filtern werden im `<errorFilter>` Abschnitt angegeben.

Das folgende Markup weist ELMAH an, 404-Fehler nicht zu protokollieren.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> Vergessen Sie nicht, dass Sie das `ErrorFilterModule` HTTP-Modul registrieren müssen, um die Fehler Filterung zu verwenden.

Das `<equal>`-Element innerhalb des `<test>` Abschnitts wird als-Assertion bezeichnet. Wenn die-Assertion als true ausgewertet wird, wird der Fehler aus dem Protokoll von ELMAH gefiltert. Es sind noch weitere Assertionen verfügbar, einschließlich: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`usw. Sie können Assertionen auch mit den `<and>`-und `<or>` booleschen Operatoren kombinieren. Darüber hinaus können Sie sogar einen einfachen JavaScript-Ausdruck als eine-Assertion einschließen oder eigene Assertionen in C# oder Visual Basic schreiben.

Weitere Informationen zu den Fehler Filterungs Funktionen von ELMAH finden Sie im [Abschnitt Fehler Filterung](https://code.google.com/p/elmah/wiki/ErrorFiltering) des [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Zusammenfassung

ELMAH bietet einen einfachen, aber leistungsfähigen Mechanismus zum Protokollieren von Fehlern in einer ASP.NET-Webanwendung. Wie das System Überwachungssystem von Microsoft kann ELMAH Fehler in einer Datenbank protokollieren und die Fehlerdetails per e-Mail an einen Entwickler senden. Im Gegensatz zum System Überwachungssystem umfasst ELMAH standardmäßig die Unterstützung für eine größere Anzahl von Fehlerprotokoll-Daten speichern, wie z. b. Microsoft SQL Server, Microsoft Access, Oracle, XML-Dateien und verschiedene andere. Außerdem bietet ELMAH einen integrierten Mechanismus zum Anzeigen des Fehler Protokolls und Details zu einem bestimmten Fehler auf einer Webseite, `elmah.axd`. Auf der Seite `elmah.axd` können auch Fehlerinformationen als RSS-Feed oder als CSV-Datei (Comma-Separated Value), die Sie mit Microsoft Excel lesen können, ausgegeben werden. Sie können ELMAH auch anweisen, Fehler aus dem Protokoll mithilfe von deklarativen oder programmatischen Assertionen zu filtern. Und ELMAH können mit ASP.NET Version 1. x-Anwendungen verwendet werden.

Jede bereitgestellte Anwendung sollte über einen Mechanismus zum automatischen protokollieren nicht behandelter Ausnahmen und zum Senden von Benachrichtigungen an das Entwicklungsteam verfügen. Ob dies mithilfe der Systemüberwachung erreicht wird, oder ELMAH ist sekundär. Anders ausgedrückt: Es spielt keine Rolle, ob Sie die Systemüberwachung oder ELMAH verwenden. Evaluieren Sie beide Systeme, und wählen Sie dann den, der Ihren Anforderungen am besten entspricht. Grundlegend wichtig ist, dass ein Mechanismus zum Protokollieren von nicht behandelten Ausnahmen in der Produktionsumgebung eingerichtet wird.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ELMAH-Fehler Protokollierung von Modulen und Handlern](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH-Projektseite](https://code.google.com/p/elmah/) (Quellcode, Beispiele, wiki)
- [Plugging von ELMAH in eine Webanwendung zum Abfangen von nicht behandelten Ausnahmen](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (Video)
- [Sicherheitsfehler Protokoll Seiten](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Verwenden von HTTP-Modulen und-Handlern zum Erstellen von austauschbaren ASP.NET-Komponenten](https://msdn.microsoft.com/library/aa479332.aspx)
- [Tutorials zur Website Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Zurück](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [Weiter](precompiling-your-website-vb.md)
