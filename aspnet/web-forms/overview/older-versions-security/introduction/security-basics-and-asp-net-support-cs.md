---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Grundlagen der Sicherheit und ASP.netC#-Unterstützung () | Microsoft-Dokumentation
author: rick-anderson
description: Dies ist das erste Tutorial in einer Reihe von Tutorials, in denen Techniken zum Authentifizieren von Besuchern über ein Webformular untersucht werden, um den Zugriff auf Partic zu autorialisieren...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ccaac101a83d0e28b07b220b8b7b61a9039227e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520071"
---
# <a name="security-basics-and-aspnet-support-c"></a>Grundlagen der Sicherheit und Unterstützung für ASP.NET (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Dies ist das erste Tutorial in einer Reihe von Tutorials, in denen Techniken zum Authentifizieren von Besuchern über ein Webformular, Autorisierung des Zugriffs auf bestimmte Seiten und Funktionen und Verwalten von Benutzerkonten in einer ASP.NET-Anwendung untersucht werden.

## <a name="introduction"></a>Einführung

Was sind die einzelnen Foren, e-Commerce-Websites, Online-e-Mail-Websites, Portal Websites und Websites für soziale Netzwerke, die alle gemeinsam sind? Sie bieten alle *Benutzerkonten*. Websites, die Benutzerkonten anbieten, müssen eine Reihe von Diensten bereitstellen. Neue Besucher müssen zumindest in der Lage sein, ein Konto zu erstellen, und die Rückgabe Besucher müssen sich anmelden können. Solche Webanwendungen können Entscheidungen auf Grundlage des angemeldeten Benutzers treffen: einige Seiten oder Aktionen sind möglicherweise auf angemeldete Benutzer oder auf eine bestimmte Teilmenge von Benutzern beschränkt. auf anderen Seiten werden möglicherweise spezifische Informationen für den angemeldeten Benutzer angezeigt, oder es werden möglicherweise mehr oder weniger Informationen angezeigt, je nachdem, welcher Benutzer die Seite anzeigt.

Dies ist das erste Tutorial in einer Reihe von Tutorials, in denen Techniken zum Authentifizieren von Besuchern über ein Webformular, Autorisierung des Zugriffs auf bestimmte Seiten und Funktionen und Verwalten von Benutzerkonten in einer ASP.NET-Anwendung untersucht werden. Im Verlauf dieser Tutorials untersuchen wir Folgendes:

- Identifizieren und Anmelden von Benutzern bei einer Website
- Verwenden Sie ASP. .Net-Mitgliedschafts Framework zum Verwalten von Benutzerkonten
- Erstellen, aktualisieren und Löschen von Benutzerkonten
- Beschränken des Zugriffs auf eine Webseite, ein Verzeichnis oder eine bestimmte Funktionalität basierend auf dem angemeldeten Benutzer
- Verwenden Sie ASP. .NET-Rollen Framework zum Zuordnen von Benutzerkonten zu Rollen
- Verwalten von Benutzerrollen
- Beschränken des Zugriffs auf eine Webseite, ein Verzeichnis oder eine bestimmte Funktionalität basierend auf der Rolle des angemeldeten Benutzers
- Anpassen und Erweitern von ASP. Websteuer Elemente für die Sicherheit des Netzes

Diese Lernprogramme sind so konzipiert, dass Sie präzise sind, und enthalten Schritt-für-Schritt-Anleitungen mit vielen Screenshots, um Sie durch den Prozess visuell zu führen. Jedes Tutorial ist in C# und Visual Basic Versionen verfügbar und enthält einen Download des gesamten verwendeten Codes. (Dieses erste Tutorial konzentriert sich auf die Sicherheitskonzepte von einem übergeordneten Standpunkt und enthält daher keinen zugeordneten Code.)

In diesem Tutorial werden wichtige Sicherheitskonzepte und die in ASP.NET verfügbaren Funktionen erläutert, um die Implementierung von Formular Authentifizierung, Autorisierung, Benutzerkonten und Rollen zu unterstützen. Erste Schritte

> [!NOTE]
> Sicherheit ist ein wichtiger Aspekt jeder Anwendung, die physische, technologische und Richtlinien relevante Entscheidungen umfasst und ein hohes Maß an Planungs-und Domänen Kenntnissen erfordert. Diese tutorialreihe ist nicht als Leitfaden für die Entwicklung sicherer Webanwendungen gedacht. Stattdessen konzentriert sich der Schwerpunkt auf Formular Authentifizierung, Autorisierung, Benutzerkonten und Rollen. Obwohl einige Sicherheitskonzepte, die diese Probleme umgeben, in dieser Reihe erläutert werden, bleiben andere nicht untersucht.

## <a name="authentication-authorization-user-accounts-and-roles"></a>Authentifizierung, Autorisierung, Benutzerkonten und Rollen

Authentifizierung, Autorisierung, Benutzerkonten und Rollen sind vier Begriffe, die in der tutorialreihe häufig verwendet werden. Daher möchte ich Ihnen einen kurzen Moment entnehmen, um diese Begriffe im Kontext der Websicherheit zu definieren. In einem Client-Server-Modell, z. b. im Internet, gibt es viele Szenarien, in denen der Server den Client identifizieren muss, der die Anforderung sendet. Die *Authentifizierung* ist der Prozess der Ermittlung der Identität des Clients. Ein Client, der erfolgreich identifiziert wurde, wird als *authentifiziert*bezeichnet. Ein nicht identifizierter Client wird als *nicht authentifiziert* oder *Anonym*bezeichnet.

Sichere Authentifizierungssysteme umfassen mindestens eine der folgenden drei Facetten: etwas, das Sie [haben, oder etwas,](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html)das Sie haben. Die meisten Webanwendungen basieren auf dem, was der Client weiß, z. b. ein Kennwort oder eine PIN. Die Informationen, die zum Identifizieren eines Benutzers verwendet werden, z. b. der Benutzername und das Kennwort, werden als *Anmelde*Informationen bezeichnet. Diese tutorialreihe konzentriert sich auf die Formular *Authentifizierung*, bei der es sich um ein Authentifizierungs Modell handelt, bei dem Benutzer sich bei der Website anmelden, indem Sie Ihre Anmelde Informationen in einem Webseiten Formular Wir haben diesen Authentifizierungstyp bereits kennen. Wechseln Sie zu einer beliebigen eCommerce-Website. Wenn Sie bereit sind, sich anzumelden, werden Sie aufgefordert, sich anzumelden, indem Sie Ihren Benutzernamen und Ihr Kennwort in die Textfelder auf einer Webseite eingeben.

Zusätzlich zur Identifizierung von Clients muss ein Server je nach dem Client, der die Anforderung sendet, möglicherweise einschränken, auf welche Ressourcen oder Funktionen zugegriffen werden kann. Bei der *Autorisierung* wird bestimmt, ob ein bestimmter Benutzer über die Berechtigung zum Zugriff auf eine bestimmte Ressource oder Funktionalität verfügt.

Bei einem *Benutzerkonto* handelt es sich um einen Speicher für die Beibehaltung von Informationen zu einem bestimmten Benutzer. Benutzerkonten müssen minimal Informationen enthalten, die den Benutzer eindeutig identifizieren, wie z. b. den Anmelde Namen und das Kennwort des Benutzers. Zusammen mit diesen wichtigen Informationen können Benutzerkonten beispielsweise folgende Elemente enthalten: die e-Mail-Adresse des Benutzers. das Datum und die Uhrzeit der Erstellung des Kontos. das Datum und die Uhrzeit der letzten Anmeldung. Vorname und Nachname; Telefonnummer; und die Postanschrift. Wenn Sie die Formular Authentifizierung verwenden, werden Benutzerkontoinformationen in der Regel in einer relationalen Datenbank wie Microsoft SQL Server gespeichert.

Webanwendungen, die Benutzerkonten unterstützen, können Benutzer optional zu *Rollen*gruppieren. Eine Rolle ist einfach eine Bezeichnung, die auf einen Benutzer angewendet wird und eine Abstraktion zum Definieren von Autorisierungs Regeln und Funktionen auf Seitenebene bereitstellt. Eine Website kann z. b. eine Administrator Rolle mit Autorisierungs Regeln enthalten, die allen Benutzern, aber Administratoren den Zugriff auf eine bestimmte Gruppe von Webseiten erlauben. Darüber hinaus können bei einer Vielzahl von Seiten, auf die alle Benutzer (einschließlich nicht-Administratoren) zugreifen können, zusätzliche Daten angezeigt werden, oder es werden zusätzliche Funktionen geboten, wenn Sie von Benutzern in der Administrator Rolle besucht werden. Mithilfe von Rollen können wir diese Autorisierungs Regeln für Rollen Weise und nicht für Benutzer durch Benutzer definieren.

## <a name="authenticating-users-in-an-aspnet-application"></a>Authentifizieren von Benutzern in einer ASP.NET-Anwendung

Wenn ein Benutzer eine URL in das Adress Fenster des Browsers eingibt oder auf einen Link klickt, sendet der Browser eine HTTP-Anforderung [(Hypertext Transfer Protocol)](http://en.wikipedia.org/wiki/HTTP) an den Webserver für den angegebenen Inhalt, dabei handelt es sich um eine ASP.NET Seite, ein Bild, eine JavaScript-Datei oder einen anderen Inhaltstyp. Der Webserver ist dafür zuständig, den angeforderten Inhalt zurückzugeben. Dabei muss eine Reihe von Dingen über die Anforderung festgelegt werden, einschließlich der Person, die die Anforderung gestellt hat, und der Angabe, ob die Identität zum Abrufen des angeforderten Inhalts autorisiert ist.

Standardmäßig senden Browser HTTP-Anforderungen, bei denen keine Identifikationsinformationen vorhanden sind. Wenn der Browser jedoch Authentifizierungsinformationen enthält, startet der Webserver den Authentifizierungs Workflow, der versucht, den Client zu identifizieren, der die Anforderung sendet. Die Schritte des Authentifizierungs Workflows hängen von der Art der Authentifizierung ab, die von der Webanwendung verwendet wird. ASP.NET unterstützt drei Arten der Authentifizierung: Windows, Passport und Forms. In dieser tutorialreihe liegt der Schwerpunkt auf der Formular Authentifizierung, aber es wird eine Minute dauern, um die Windows-Authentifizierungs Benutzer Stores und den Workflow zu vergleichen

### <a name="authentication-via-windows-authentication"></a>Authentifizierung über die Windows-Authentifizierung

Der Windows-Authentifizierungs Workflow verwendet eine der folgenden Authentifizierungsmethoden:

- Standardauthentifizierung
- Digestauthentifizierung
- Integrierte Windows-Authentifizierung

Alle drei Techniken funktionieren ungefähr auf dieselbe Weise: Wenn eine nicht autorisierte, anonyme Anforderung eingeht, sendet der Webserver eine HTTP-Antwort zurück, die angibt, dass eine Autorisierung erforderlich ist, um den Vorgang fortzusetzen. Der Browser zeigt dann ein modales Dialogfeld an, in dem der Benutzer zur Eingabe des Benutzernamens und des Kennworts aufgefordert wird (siehe Abbildung 1). Diese Informationen werden dann über einen HTTP-Header an den Webserver zurückgesendet.

![Ein modales Dialog Feld fordert den Benutzer zur Eingabe seiner Anmelde Informationen auf.](security-basics-and-asp-net-support-cs/_static/image1.png)

**Abbildung 1**: ein modales Dialog Feld fordert den Benutzer zur Eingabe seiner Anmelde Informationen auf.

Die angegebenen Anmelde Informationen werden im Windows-Benutzerspeicher des Webservers überprüft. Dies bedeutet, dass jeder authentifizierte Benutzer in Ihrer Webanwendung über ein Windows-Konto in Ihrer Organisation verfügen muss. Dies ist in Intranetszenarien üblich. Wenn die integrierte Windows-Authentifizierung in einer intraneteinstellung verwendet wird, stellt der Browser dem Webserver automatisch die Anmelde Informationen zur Verfügung, die für die Anmeldung beim Netzwerk verwendet werden. Dadurch wird das Dialogfeld unterdrückt, das in Abbildung 1 dargestellt wird. Obwohl die Windows-Authentifizierung für Intranetanwendungen hervorragend geeignet ist, ist Sie in der Regel für Internet Anwendungen nicht möglich, da Sie keine Windows-Konten für jeden Benutzer und jeden Benutzer erstellen möchten, der sich an Ihrer Website anmeldet.

### <a name="authentication-via-forms-authentication"></a>Authentifizierung per Formular Authentifizierung

Die Formular Authentifizierung ist hingegen ideal für Internet-Webanwendungen. Denken Sie daran, dass die Formular Authentifizierung den Benutzer identifiziert, indem er Sie zur Eingabe seiner Anmelde Informationen über ein Webformular auffordert. Wenn ein Benutzer versucht, auf eine nicht autorisierte Ressource zuzugreifen, wird er daher automatisch an die Anmeldeseite umgeleitet, auf der Sie seine Anmelde Informationen eingeben können. Die übermittelten Anmelde Informationen werden dann anhand eines benutzerdefinierten Benutzerspeicher überprüft, in der Regel in einer Datenbank.

Nach dem Überprüfen der übermittelten Anmelde Informationen wird ein *Formular Authentifizierungs Ticket* für den Benutzer erstellt. Dieses Ticket gibt an, dass der Benutzer authentifiziert wurde, und enthält identifizierende Informationen, z. b. den Benutzernamen. Das Formular Authentifizierungs Ticket wird (in der Regel) als Cookie auf dem Client Computer gespeichert. Folglich enthalten nachfolgende Besuche der Website das Formular Authentifizierungs Ticket in der HTTP-Anforderung, sodass die Webanwendung den Benutzer nach der Anmeldung identifizieren kann.

Abbildung 2 veranschaulicht den Workflow zur Formular Authentifizierung von einem übergeordneten Standpunkt aus. Beachten Sie, dass die Authentifizierungs-und Autorisierungs Elemente in ASP.net als zwei separate Entitäten fungieren. Das Formular Authentifizierungssystem identifiziert den Benutzer (oder meldet, dass er anonym ist). Das Autorisierungssystem bestimmt, ob der Benutzer Zugriff auf die angeforderte Ressource hat. Wenn der Benutzer nicht autorisiert ist (wie es in Abbildung 2 der Fall ist, wenn Sie versuchen, die Seite "protectedpage. aspx" anonym zu besuchen), meldet das Autorisierungssystem, dass der Benutzer verweigert wird, sodass das Formular Authentifizierungssystem den Benutzer automatisch zur Anmeldeseite umleitet.

Nachdem sich der Benutzer erfolgreich angemeldet hat, enthalten nachfolgende HTTP-Anforderungen das Formular Authentifizierungs Ticket. Das Formular Authentifizierungssystem identifiziert nur den Benutzer, d. h. das Autorisierungssystem, das bestimmt, ob der Benutzer auf die angeforderte Ressource zugreifen kann.

![Der Formular Authentifizierungs Workflow](security-basics-and-asp-net-support-cs/_static/image2.png)

**Abbildung 2**: der Formular Authentifizierungs Workflow

In den nächsten beiden Tutorials wird die Formular Authentifizierung ausführlicher erläutert,[eine Übersicht über die Formular Authentifizierung](an-overview-of-forms-authentication-cs.md) und die Konfiguration der [Formular Authentifizierung sowie erweiterte Themen](forms-authentication-configuration-and-advanced-topics-cs.md). Weitere Informationen zu ASP. Zu den Netzwerk Authentifizierungs Optionen finden Sie unter [ASP.net Authentication](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Einschränken des Zugriffs auf Webseiten, Verzeichnisse und Seitenfunktionen

ASP.net bietet zwei Möglichkeiten, um zu bestimmen, ob ein bestimmter Benutzer über die Berechtigung zum Zugriff auf eine bestimmte Datei oder ein bestimmtes Verzeichnis verfügt:

- **Datei Autorisierung** : da ASP.NET Seiten und Webdienste als Dateien implementiert werden, die sich im Dateisystem des Webservers befinden, kann der Zugriff auf diese Dateien über Access Control Listen (ACLs) angegeben werden. Die Datei Autorisierung wird am häufigsten bei der Windows-Authentifizierung verwendet, da ACLs Berechtigungen sind, die für Windows-Konten gelten. Wenn Sie die Formular Authentifizierung verwenden, werden alle Anforderungen an das Betriebssystem und die Datei auf Systemebene vom gleichen Windows-Konto ausgeführt, unabhängig vom Benutzer, der die Website besucht.
- **URL-Autorisierung**: mit URL-Autorisierung gibt der Seiten Entwickler Autorisierungs Regeln in "Web. config" an. Diese Autorisierungs Regeln geben an, welche Benutzer oder Rollen auf bestimmte Seiten oder Verzeichnisse in der Anwendung zugreifen dürfen oder verweigert werden.

Datei Autorisierung und URL-Autorisierung definieren Sie Autorisierungs Regeln für den Zugriff auf eine bestimmte ASP.NET Seite oder für alle ASP.NET Seiten in einem bestimmten Verzeichnis. Mithilfe dieser Techniken können wir ASP.NET anweisen, Anforderungen an eine bestimmte Seite für einen bestimmten Benutzer zu verweigern oder den Zugriff auf eine Gruppe von Benutzern zuzulassen und den Zugriff auf alle anderen Personen zu verweigern. Wie sieht es mit Szenarien aus, in denen alle Benutzer auf die Seite zugreifen können, aber die Funktionalität der Seite hängt vom Benutzer ab? Beispielsweise verfügen viele Websites, die Benutzerkonten unterstützen, über Seiten, die unterschiedliche Inhalte oder Daten für authentifizierte Benutzer im Vergleich zu anonymen Benutzern anzeigen. Ein anonymer Benutzer könnte einen Link zum Anmelden bei der Website sehen, während ein authentifizierter Benutzer stattdessen eine Meldung wie, Willkommen zurück, *username* und einen Link zum Abmelden sehen würde. Ein weiteres Beispiel: Wenn Sie ein Element auf einer Auktionswebsite anzeigen, sehen Sie unterschiedliche Informationen, je nachdem, ob Sie ein Bieter sind oder der Artikel das Element versteigert.

Solche Anpassungen auf Seitenebene können deklarativ oder Programm gesteuert durchgeführt werden. Um andere Inhalte für anonym als authentifizierte Benutzer anzuzeigen, ziehen Sie einfach ein [LoginView-Steuer](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) Element auf die Seite, und geben Sie den entsprechenden Inhalt in seine "AnonymousTemplate"-und "LoggedInTemplate"-Vorlage ein. Alternativ können Sie Programm gesteuert ermitteln, ob die aktuelle Anforderung authentifiziert ist, wer der Benutzer ist und welche Rollen Sie angehören (sofern vorhanden). Sie können diese Informationen verwenden, um Spalten in einem Raster oder Panels auf der Seite anzuzeigen oder auszublenden.

Diese Serie enthält drei Lernprogramme, die sich auf die Autorisierung konzentrieren. Die ***Benutzerbasierte Autorisierung***untersucht, wie der Zugriff auf eine Seite oder Seiten in einem Verzeichnis für bestimmte Benutzerkonten eingeschränkt wird. Die ***rollenbasierte Autorisierung*** untersucht die Bereitstellung von Autorisierungs Regeln auf Rollen Ebene. Schließlich untersucht der Inhalt, der ***auf dem aktuell angemeldeten Benutzer*** -Tutorial basiert, das Ändern der Inhalte und Funktionen einer bestimmten Seite basierend auf dem Benutzer, der die Seite besucht. Weitere Informationen zu ASP. Zu den Autorisierungs Optionen für NET finden Sie unter [ASP.net Authorization](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Benutzerkonten und-Rollen

ASP. Die Formular Authentifizierung von NET stellt eine Infrastruktur bereit, mit der sich Benutzer bei einer Website anmelden können und deren authentifizierter Zustand über Seitenaufrufe hinweg gespeichert wird. Und die URL-Autorisierung bietet ein Framework zum Einschränken des Zugriffs auf bestimmte Dateien oder Ordner in einer ASP.NET-Anwendung. Keines dieser Features bietet jedoch eine Möglichkeit zum Speichern von Benutzerkontoinformationen oder zum Verwalten von Rollen.

Vor ASP.NET 2,0 waren Entwickler für die Erstellung Ihrer eigenen Benutzer-und Rollen Speicher verantwortlich. Sie waren auch auf dem Hook zum Entwerfen der Benutzeroberflächen und zum Schreiben des Codes für wichtige Benutzerkonten bezogene Seiten wie die Anmeldeseite und die Seite zum Erstellen eines neuen Kontos. Ohne integriertes Benutzerkonto-Framework in ASP.net musste jeder Entwickler, der Benutzerkonten implementiert, seine eigenen Entwurfsentscheidungen zu Fragen wie Gewusst wie Speichern von Kenn Wörtern oder anderen vertraulichen Informationen treffen? und welche Richtlinien sollte ich hinsichtlich der Kenn Wort Länge und-Stärke festlegen?

Heutzutage ist die Implementierung von Benutzerkonten in einer ASP.NET-Anwendung dank des *Mitgliedschafts Frameworks* und der integrierten Anmeldungs-websteuer Elemente viel einfacher. Das Mitgliedschafts Framework besteht aus einer Reihe von Klassen im [System. Web. Security-Namespace](https://msdn.microsoft.com/library/system.web.security.aspx) , die Funktionen zum Ausführen wichtiger Benutzerkonten bezogener Aufgaben bereitstellen. Die Schlüssel Klasse im Mitgliedschafts Framework ist die [Mitgliedschafts Klasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx)mit Methoden wie:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Das Mitgliedschafts Framework verwendet das [Anbieter Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), das die API des Mitgliedschafts Frameworks von der Implementierung getrennt trennt. Dadurch können Entwickler eine gemeinsame API verwenden, Sie können jedoch eine Implementierung verwenden, die die benutzerdefinierten Anforderungen Ihrer Anwendung erfüllt. Kurz gesagt, die Mitgliedschafts Klasse definiert die grundlegende Funktionalität des Frameworks (Methoden, Eigenschaften und Ereignisse), stellt jedoch keine Implementierungsdetails bereit. Stattdessen rufen die Methoden der Mitgliedschafts Klasse den konfigurierten Anbieter auf, der die tatsächliche Arbeit ausführt. Wenn z. b. die "roateuser"-Methode der Mitgliedschafts Klasse aufgerufen wird, kennt die Mitgliedschafts Klasse nicht die Details des Benutzer Stores. Es ist nicht bekannt, ob Benutzer in einer Datenbank oder in einer XML-Datei oder in einem anderen Speicher verwaltet werden. Die Mitgliedschafts Klasse untersucht die Konfiguration der Webanwendung, um zu bestimmen, an welchen Anbieter der Aufruf delegiert werden soll, und diese Anbieter Klasse ist dafür verantwortlich, das neue Benutzerkonto im entsprechenden Benutzerspeicher zu erstellen. Diese Interaktion ist in Abbildung 3 dargestellt.

Microsoft gibt zwei Mitgliedschafts Anbieter Klassen in der .NET Framework aus:

- [Activedirector ymitgliedshipprovider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) : Implementiert die Mitgliedschafts-API in Active Directory und Active Directory Anwendungsmodus (Adam).
- [Sqlmitgliedshipprovider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) : Implementiert die Mitgliedschafts-API in einer SQL Server-Datenbank.

Diese tutorialreihe konzentriert sich ausschließlich auf den sqlmembership shipprovider.

[![das Anbieter Modell ermöglicht, dass verschiedene Implementierungen nahtlos an das Framework&lt;/Strong angeschlossen werden&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Abbildung 03**: mit dem Anbieter Modell können unterschiedliche Implementierungen nahtlos an das Framework angeschlossen werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](security-basics-and-asp-net-support-cs/_static/image5.png))

Der Vorteil des Anbieter Modells besteht darin, dass alternative Implementierungen von Microsoft, Drittanbietern oder einzelnen Entwicklern entwickelt und nahtlos in das Mitgliedschafts Framework eingebunden werden können. Microsoft hat z. b. [einen Mitgliedschafts Anbieter für Microsoft Access-Datenbanken](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi)veröffentlicht. Weitere Informationen zu den Mitgliedschafts Anbietern finden Sie im [Provider Toolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx), das eine exemplarische Vorgehensweise der Mitgliedschafts Anbieter, Beispiele für benutzerdefinierte Anbieter, mehr als 100 Seiten der Dokumentation zum Anbieter Modell sowie den gesamten Quellcode für die integrierten Mitgliedschafts Anbieter (d. a. activedirector ymitgliedshipprovider und sqlmitgliedshipprovider) enthält.

ASP.NET 2,0 hat außerdem das Rollen Framework eingeführt. Wie das Mitgliedschafts Framework basiert das Rollen Framework auf dem Anbieter Modell. Die API wird über die [Klasse "Rollen](https://msdn.microsoft.com/library/system.web.security.roles.aspx) " verfügbar gemacht, und die .NET Framework wird mit drei Anbieter Klassen ausgeliefert:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) : verwaltet Rollen Informationen in einem Autorisierungs-Manager-Richtlinien Speicher, z. b. Active Directory oder Adam.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) : implementiert Rollen in einer SQL Server Datenbank.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) : ordnet Rollen Informationen auf Grundlage der Windows-Gruppe des Besuchers zu. Diese Methode wird in der Regel mit der Windows-Authentifizierung verwendet.

Diese tutorialreihe konzentriert sich ausschließlich auf SqlRoleProvider.

Da das Anbieter Modell eine einzige vorwärts gerichtete API (die Mitgliedschafts-und Rollen Klassen) enthält, ist es möglich, Funktionen um diese API zu erstellen, ohne sich über die Implementierungsdetails Gedanken machen zu müssen, die von den von der Seite ausgewählten Anbietern behandelt werden. Trägers. Diese einheitliche API ermöglicht es Microsoft und Drittanbietern, websteuer Elemente zu erstellen, die mit den Mitgliedschafts-und Rollen Frameworks zusammenarbeiten. ASP.net wird mit einer Reihe von [websteuer Elementen](https://msdn.microsoft.com/library/ms178329.aspx) für die Anmeldung zum Implementieren allgemeiner Benutzeroberflächen für Benutzerkonten ausgeliefert. Beispielsweise fordert das [Anmelde Steuer](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) Element einen Benutzer zur Eingabe seiner Anmelde Informationen auf, überprüft diese und protokolliert Sie anschließend über die Formular Authentifizierung. Das [LoginView-Steuer](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) Element bietet Vorlagen zum Anzeigen verschiedener Markup für anonyme Benutzer im Vergleich zu authentifizierten Benutzern oder für ein anderes Markup, das auf der Rolle des Benutzers basiert. Das Steuerelement " [kreateuserwizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) " bietet eine schrittweise Benutzeroberfläche zum Erstellen eines neuen Benutzerkontos.

Unterhalb der werden die verschiedenen Anmelde Steuerelemente mit den Mitgliedschafts-und Rollen Frameworks interagieren. Die meisten Anmelde Steuerelemente können implementiert werden, ohne dass eine einzige Codezeile geschrieben werden muss. Wir werden diese Steuerelemente in zukünftigen Tutorials ausführlicher untersuchen, einschließlich Techniken zum Erweitern und Anpassen ihrer Funktionalität.

## <a name="summary"></a>Zusammenfassung

Alle Webanwendungen, die Benutzerkonten unterstützen, erfordern ähnliche Features, wie z. b. die Möglichkeit, sich Benutzer anzumelden und deren Anmeldestatus über Seitenbesuche hinweg gespeichert wird. eine Webseite für neue Besucher zum Erstellen eines Kontos. und die Möglichkeit des Seiten Entwicklers anzugeben, welche Ressource, welche Daten und Funktionen für welche Benutzer oder Rollen verfügbar sind. Die Aufgaben der Authentifizierung und Autorisierung von Benutzern und der Verwaltung von Benutzerkonten und Rollen sind in ASP.NET-Anwendungen aufgrund der Formular Authentifizierung, URL-Autorisierung und der Mitgliedschafts-und Rollen Frameworks erstaunlich einfach zu bewerkstelligen.

Im Verlauf der nächsten Tutorials werden diese Aspekte untersucht, indem eine funktionierende Webanwendung von Grund auf schrittweise erstellt wird. Im nächsten zwei Tutorial wird die Formular Authentifizierung ausführlich erläutert. Wir sehen, dass der Formular Authentifizierungs Workflow in Aktion ist, das Formular Authentifizierungs Ticket zerlegt, Sicherheitsaspekte erörtert und das Formular Authentifizierungssystem zu konfigurieren, während eine Webanwendung erstellt wird, die es den Besuchern ermöglicht, sich anzumelden und abzumelden.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.NET 2,0 Mitgliedschaft, Rollen, Formular Authentifizierung und Sicherheitsressourcen](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2,0 Sicherheitsrichtlinien](https://msdn.microsoft.com/library/ms998258.aspx)
- [Authentifizierung in ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [ASP.NET-Autorisierung](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Übersicht über ASP.net-Anmelde Steuerelemente](https://msdn.microsoft.com/library/ms178329.aspx)
- [Überprüfen der Mitgliedschaft, Rollen und Profile von ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Gewusst wie: sichern meiner Website mithilfe von Mitgliedschaften und Rollen](https://asp.net/learn/videos/video-45.aspx) Video
- [Einführung in die Mitgliedschaft](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2,0 Sicherheit, Mitgliedschaft und Rollen Verwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Anbietertoolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial wurde diese tutorialreihe von vielen hilfreichen Reviewern geprüft. Zu den führenden Reviewern für dieses Tutorial zählen Alicja Maziarz, John suru und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Weiter](an-overview-of-forms-authentication-cs.md)
