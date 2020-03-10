---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: Grundlegende Unterschiede zwischen IIS und demC#ASP.NET Development Server () | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie eine ASP.NET-Anwendung lokal testen, verwenden Sie die Wahrscheinlichkeit, dass Sie den ASP.NET Development Web Server verwenden. Die Produktions Website ist jedoch höchstwahrscheinlich ein pow...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fea3939bd910a052340215c207013e2269f32a2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78522081"
---
# <a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>Wichtige Unterschiede zwischen den IIS und dem ASP.NET Development Server (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> Wenn Sie eine ASP.NET-Anwendung lokal testen, verwenden Sie die Wahrscheinlichkeit, dass Sie den ASP.NET Development Web Server verwenden. Die Produktions Website ist jedoch höchstwahrscheinlich mit IIS ausgestattet. Es gibt einige Unterschiede zwischen der Verarbeitung von Anforderungen durch diese Webserver, und diese Unterschiede können wichtige Konsequenzen haben. In diesem Tutorial werden einige der weiteren Unterschiede erläutert.

## <a name="introduction"></a>Einführung

Wenn ein Benutzer eine ASP.NET-Anwendung besucht, sendet der Browser eine Anforderung an die Website. Diese Anforderung wird von der Webserver Software übernommen, die mit der ASP.NET-Laufzeit koordiniert, um den Inhalt für die angeforderte Ressource zu generieren und zurückzugeben. Bei den[**i** internetbasierter **i** nformation **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) handelt es sich um eine Suite von Diensten, die allgemeine Internet basierte Funktionen für Windows-Server bereitstellen. IIS ist der am häufigsten verwendete Webserver für ASP.NET-Anwendungen in Produktionsumgebungen. Es ist höchstwahrscheinlich die Webserver Software, die von Ihrem webhostinstanbieter verwendet wird, um Ihre ASP.NET-Anwendung zu bedienen. IIS kann auch als Webserver Software in der Entwicklungsumgebung verwendet werden, obwohl dies die Installation von IIS und eine ordnungsgemäße Konfiguration erfordert.

Der ASP.NET Development Server ist eine Alternative Webserver Option für die Entwicklungsumgebung. Es ist mit ausgeliefert und ist in Visual Studio integriert. Wenn die Webanwendung nicht für die Verwendung von IIS konfiguriert wurde, wird die ASP.NET Development Server automatisch gestartet und als Webserver verwendet, wenn Sie zum ersten Mal eine Webseite innerhalb von Visual Studio besuchen. Die Demo-Webanwendungen, die wir im Lernprogramm zur Bestimmung der zu verwendenden [*Dateien*](determining-what-files-need-to-be-deployed-cs.md) erstellt haben, waren beide Dateisystem basierte Webanwendungen, die nicht für die Verwendung von IIS konfiguriert wurden. Daher wird beim Besuch einer dieser Websites innerhalb von Visual Studio die ASP.NET Development Server verwendet.

In einer perfekten Welt wären die Entwicklungs-und Produktionsumgebungen identisch. Wie bereits im vorherigen Tutorial erläutert, ist es jedoch nicht ungewöhnlich, dass die Umgebungen über unterschiedliche Konfigurationseinstellungen verfügen. Wenn Sie unterschiedliche Webserver Software in Umgebungen verwenden, wird eine weitere Variable hinzugefügt, die beim Bereitstellen einer Anwendung berücksichtigt werden muss. Dieses Tutorial behandelt die wichtigsten Unterschiede zwischen IIS und dem ASP.NET Development Server. Aufgrund dieser Unterschiede gibt es Szenarien, in denen Code, der in der Entwicklungsumgebung einwandfrei ausgeführt wird, eine Ausnahme auslöst oder sich bei der Ausführung in der Produktion anders verhält.

## <a name="security-context-differences"></a>Unterschiede im Sicherheitskontext

Wenn die Webserver Software eine eingehende Anforderung verarbeitet, ordnet Sie diese Anforderung einem bestimmten Sicherheitskontext zu. Diese Sicherheitskontext Informationen werden vom Betriebssystem verwendet, um zu bestimmen, welche Aktionen von der Anforderung zulässig sind. Beispielsweise kann eine ASP.NET-Seite Code enthalten, der eine Nachricht in einer Datei auf dem Datenträger protokolliert. Damit diese ASP.NET Seite ohne Fehler ausgeführt werden kann, muss der Sicherheitskontext über die entsprechenden Berechtigungen auf Dateisystem Ebene verfügen, nämlich über Schreibberechtigungen für diese Datei.

Der ASP.NET Development Server ordnet eingehende Anforderungen dem Sicherheitskontext des aktuell angemeldeten Benutzers zu. Wenn Sie als Administrator bei Ihrem Desktop angemeldet sind, haben die ASP.NET-Seiten, die vom ASP.NET Development Server bereitgestellt werden, die gleichen Zugriffsrechte wie ein Administrator. ASP.NET-Anforderungen, die von IIS verarbeitet werden, sind jedoch mit einem bestimmten Computer Konto verknüpft. Standardmäßig wird das Netzwerkdienst-Computer Konto von IIS Version 6 und 7 verwendet, obwohl ihr webhostinstanbieter möglicherweise ein eindeutiges Konto für jeden Kunden konfiguriert hat. Außerdem hat ihr webhostinstanbieter wahrscheinlich eingeschränkte Berechtigungen für dieses Computer Konto erteilt. Das Ergebnis ist, dass Sie Webseiten haben können, die in der Entwicklungsumgebung ohne Fehler ausgeführt werden, aber Autorisierungs bezogene Ausnahmen generieren, wenn Sie in der Produktionsumgebung gehostet werden.

Um diese Art von Fehler in Aktion anzuzeigen, habe ich eine Seite auf der Book Reviews-Website erstellt, die eine Datei auf dem Datenträger erstellt, auf der das aktuelle Datum und die Uhrzeit gespeichert werden, an denen jemand den Review *yourself ASP.NET 3,5 in 24 Stunden* gelesen hat. Öffnen Sie hierzu die Seite `~/Tech/TYASP35.aspx`, und fügen Sie den folgenden Code zum `Page_Load`-Ereignishandler hinzu:

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> Mit der [`File.WriteAllText`-Methode](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) wird eine neue Datei erstellt, wenn Sie nicht vorhanden ist, und dann wird der angegebene Inhalt in diese Datei geschrieben. Wenn die Datei bereits vorhanden ist, wird Ihr vorhandener Inhalt überschrieben.

Besuchen Sie als nächstes die Seite unter Verfassen von *ASP.NET 3,5 in 24 Stunden* der Buch Prüfung in der Entwicklungsumgebung mithilfe der ASP.NET Development Server. Wenn Sie auf Ihrem Computer mit einem Konto angemeldet sind, das über ausreichende Berechtigungen zum Erstellen und Ändern einer Textdatei im Stammverzeichnis der Webanwendung verfügt, wird der Buchreview wie zuvor angezeigt, aber jedes Mal, wenn die Seite besucht wird, wird das Datum und die Uhrzeit und die IP-Adresse des Benutzers in der `LastTYASP35Access.txt`-Datei gespeichert. Zeigen Sie Ihren Browser auf diese Datei. Es sollte eine Meldung wie in Abbildung 1 angezeigt werden.

[![die Textdatei das Datum und die Uhrzeit des letzten besuchtes der Buch Überprüfung enthält.](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**Abbildung 1**: die Textdatei enthält das letzte Datum und die Uhrzeit, zu der der Buchreview besucht wurde ([Klicken Sie, um das Bild in voller Größe anzuzeigen](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png)).

Stellen Sie die Webanwendung in der Produktionsumgebung bereit, und besuchen Sie dann die Seite Hosted *Teach Yourself ASP.NET 3,5 auf* der Seite Book Review. An diesem Punkt sollten Sie entweder die Seite "Book Review" als normal oder die in Abbildung 2 gezeigte Fehlermeldung sehen. Einige Webhostinganbieter erteilen Schreibberechtigungen für das anonyme ASP.net-Computer Konto. in diesem Fall funktioniert die Seite ohne Fehler. Wenn der Webhost Anbieter jedoch Schreibzugriff für das anonyme Konto untersagt, wird eine [`UnauthorizedAccessException` Ausnahme](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) ausgelöst, wenn die `TYASP35.aspx` Seite versucht, das aktuelle Datum und die aktuelle Uhrzeit in die `LastTYASP35Access.txt` Datei zu schreiben.

[![das von IIS verwendete Standard Computer Konto nicht über die Berechtigung zum Schreiben in das Datei System verfügt.](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**Abbildung 2**: das von IIS verwendete Standard Computer Konto verfügt nicht über die Berechtigung zum Schreiben in das Datei System ([Klicken Sie, um das Bild in voller Größe anzuzeigen](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))

Die gute Nachricht ist, dass die meisten Webhostinganbieter über eine Art von Berechtigungs Tool verfügen, mit dem Sie Dateisystem Berechtigungen auf Ihrer Website angeben können. Erteilen Sie dem anonymen ASP.NET-Konto Schreibzugriff auf das Stammverzeichnis, und besuchen Sie dann die Seite Book Review. (Falls erforderlich, wenden Sie sich an Ihren webhostinbieter, um Unterstützung beim Erteilen von Schreibberechtigungen für das standardmäßige ASP.NET-Konto zu erhalten.) Dieses Mal sollte die Seite ohne Fehler geladen werden, und die `LastTYASP35Access.txt` Datei sollte erfolgreich erstellt werden.

Der Grund dafür ist, dass die ASP.NET Development Server in einem anderen Sicherheitskontext als IIS ausgeführt wird. es ist möglich, dass Ihre ASP.NET Seiten, die auf das Dateisystem schreiben oder schreiben, in das Windows-Ereignisprotokoll schreiben oder in das Windows-Ereignisprotokoll schreiben oder in das Fenster schreiben oder schreiben.  die Registrierung funktioniert bei der Entwicklung erwartungsgemäß, generiert jedoch Ausnahmen in der Produktionsumgebung. Wenn Sie eine Webanwendung erstellen, die in einer freigegebenen Webhostingumgebung bereitgestellt wird, lesen oder schreiben Sie das Ereignisprotokoll oder die Windows-Registrierung nicht. Beachten Sie auch alle ASP.NET Seiten, die aus dem Dateisystem lesen oder in das Dateisystem schreiben, da Sie möglicherweise Lese-und Schreibberechtigungen für die entsprechenden Ordner in der Produktionsumgebung erteilen müssen.

## <a name="differences-on-serving-static-content"></a>Unterschiede beim Reservieren statischer Inhalte

Ein weiterer Hauptunterschied zwischen IIS und dem ASP.NET Development Server ist die Art und Weise, wie Sie Anforderungen für statischen Inhalt verarbeiten. Jede Anforderung, die in der ASP.NET Development Server erfolgt, unabhängig davon, ob für eine ASP.NET Seite, ein Bild oder eine JavaScript-Datei, wird von der ASP.NET-Laufzeit verarbeitet. Standardmäßig ruft IIS nur die ASP.NET-Laufzeit auf, wenn eine Anforderung für eine ASP.NET-Ressource (z. b. eine ASP.NET-Webseite, ein Webdienst usw.) ankommt. Anforderungen an statische Inhalte: Bilder, CSS-Dateien, JavaScript-Dateien, PDF-Dateien, ZIP-Dateien und die like-werden von IIS ohne Beteiligung der ASP.NET-Laufzeit abgerufen. (Es ist möglich, IIS anzuweisen, beim Verarbeiten statischer Inhalte mit der ASP.NET-Laufzeit zu arbeiten. Weitere Informationen finden Sie im Abschnitt "durchführen der Formular basierten Authentifizierung und URL-Authentifizierung bei statischen Dateien mit IIS 7".)

Die ASP.NET-Laufzeit führt eine Reihe von Schritten aus, um den angeforderten Inhalt zu generieren, einschließlich der Authentifizierung (Identifizierung des Anforderer) und der Autorisierung (ermitteln, ob der Anforderer über die Berechtigung zum Anzeigen des angeforderten Inhalts verfügt). Eine beliebte Form der Authentifizierung ist die Formular *basierte Authentifizierung*, bei der ein Benutzer durch Eingabe seiner Anmelde Informationen (normalerweise eines Benutzernamens und eines Kennworts in Textfelder auf einer Webseite) identifiziert wird. Wenn Sie Ihre Anmelde Informationen überprüfen, speichert die Website ein *Authentifizierungs Ticket* Cookie im Browser des Benutzers, das bei jeder nachfolgenden Anforderung an die Website gesendet wird und zum Authentifizieren des Benutzers verwendet wird. Darüber hinaus ist es möglich, *URL-Autorisierungs* Regeln anzugeben, mit denen festgelegt wird, welche Benutzer auf einen bestimmten Ordner zugreifen können. Viele ASP.NET Websites verwenden die Formular basierte Authentifizierung und URL-Autorisierung, um Benutzerkonten zu unterstützen und Teile der Website zu definieren, auf die nur authentifizierte Benutzer oder Benutzer zugegriffen werden kann, die zu einer bestimmten Rolle gehören.

> [!NOTE]
> Für eine gründliche Untersuchung von ASP. Die Formular basierte Authentifizierung von NET, URL-Autorisierung und andere Features im Zusammenhang mit dem Benutzerkonto finden Sie unter meine [Website-Sicherheits](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)Lernprogramme.

Angenommen, eine Website unterstützt Benutzerkonten mithilfe der Formular basierten Autorisierung und verfügt über einen Ordner, der mithilfe der URL-Autorisierung so konfiguriert ist, dass nur authentifizierte Benutzer zugelassen werden. Stellen Sie sich vor, dass dieser Ordner ASP.net Pages und PDF-Dateien enthält und dass die Absicht darin besteht, dass nur authentifizierte Benutzer diese PDF-Dateien anzeigen können.

Was geschieht, wenn ein Besucher versucht, eine dieser PDF-Dateien anzuzeigen, indem er die URL direkt in die Adressleiste des Browsers eingegeben hat? Um dies herauszufinden, erstellen wir einen neuen Ordner auf der Website "Book Reviews", fügen einige PDF-Dateien hinzu und konfigurieren die Website für die Verwendung der URL-Autorisierung, um anonymen Benutzern den Besuch dieses Ordners zu gestatten. Wenn Sie die Demoanwendung herunterladen, sehen Sie, dass ich einen Ordner mit dem Namen "`PrivateDocs`" erstellt und eine PDF-Datei aus meinen [Website](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) -Lernprogrammen hinzugefügt habe. Der `PrivateDocs` Ordner enthält außerdem eine `Web.config` Datei, in der die URL-Autorisierungs Regeln zum verweigern anonymer Benutzer angegeben werden:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

Schließlich habe ich die Webanwendung so konfiguriert, dass Sie die Formular basierte Authentifizierung verwendet, indem ich die `Web.config` Datei im Stammverzeichnis aktualisiere und dabei Folgendes ersetzt:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

Durch:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

Verwenden Sie die ASP.NET Development Server, besuchen Sie die Website, und geben Sie die direkte URL zu einer der PDF-Dateien in der Adressleiste Ihres Browsers ein. Wenn Sie die diesem Tutorial zugeordnete Website heruntergeladen haben, sollte die URL in etwa wie folgt aussehen: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Wenn Sie diese URL in die Adressleiste eingeben, sendet der Browser eine Anforderung an den ASP.NET Development Server für die Datei. Der ASP.NET Development Server übergibt die Anforderung zur Verarbeitung an die ASP.NET-Laufzeit. Da wir noch nicht angemeldet sind und die `Web.config` im `PrivateDocs` Ordner so konfiguriert ist, dass der anonyme Zugriff verweigert wird, leitet die ASP.NET-Laufzeit den Benutzer automatisch an die Anmeldeseite weiter, `Login.aspx` (siehe Abbildung 3). Beim Umleiten des Benutzers an die Anmeldeseite enthält ASP.net einen `ReturnUrl` QueryString-Parameter, der die Seite angibt, die der Benutzer anzeigen wollte. Nachdem die Anmeldung erfolgreich abgeschlossen wurde, kann der Benutzer zu dieser Seite zurückkehren.

[![nicht autorisierte Benutzer werden automatisch zur Anmeldeseite umgeleitet.](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**Abbildung 3**: nicht autorisierte Benutzer werden automatisch zur Anmeldeseite umgeleitet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))

Sehen wir uns nun an, wie sich dies auf die Produktion verhält. Stellen Sie die Anwendung bereit, und geben Sie die direkte URL zu einem der PDFs im `PrivateDocs` Ordner in der Produktionsumgebung ein. Dadurch wird Ihr Browser aufgefordert, eine IIS-Anforderung für die Datei zu senden. Da eine statische Datei angefordert wird, wird die Datei von IIS abgerufen und zurückgegeben, ohne dass die ASP.NET-Laufzeit aufgerufen wird. Daher wurde keine URL-Autorisierungs Überprüfung durchgeführt. Jeder Benutzer, der die direkte URL zu der Datei kennt, kann auf den Inhalt der angeblich privaten PDF zugreifen.

[![anonyme Benutzer können die privaten PDF-Dateien herunterladen, indem Sie die direkte URL zur Datei eingeben.](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**Abbildung 4**: anonyme Benutzer können die privaten PDF-Dateien herunterladen, indem Sie die direkte URL zur Datei eingeben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png)).

### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Ausführen von Formular basierter Authentifizierung und URL-Authentifizierung für statische Dateien mit IIS 7

Es gibt eine Reihe von Techniken, die Sie verwenden können, um statischen Inhalt vor unbefugten Benutzern zu schützen. In IIS 7 wurde die *integrierte Pipeline*eingeführt, die den IIS-Workflow mit dem Workflow der ASP.NET-Laufzeit Marshalling. Kurz gesagt, Sie können IIS anweisen, die Authentifizierungs-und Autorisierungs Module der ASP.NET-Laufzeit für alle eingehenden Anforderungen (einschließlich statischer Inhalte wie PDF-Dateien) aufzurufen. Wenden Sie sich an Ihren webhostinstanbieter, um zu erfahren, wie Sie Ihre Website für die Verwendung der integrierten Pipeline konfigurieren.

Nachdem IIS für die Verwendung der integrierten Pipeline konfiguriert wurde, fügen Sie das folgende Markup zur `Web.config`-Datei im Stammverzeichnis hinzu:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

Dieses Markup weist IIS 7 an, die ASP.NET-basierten Authentifizierungs-und Autorisierungs Module zu verwenden. Stellen Sie die Anwendung erneut bereit, und besuchen Sie die PDF-Datei erneut. Dieses Mal, wenn die Anforderung von IIS verarbeitet wird, wird der Authentifizierungs-und Autorisierungs Logik der ASP.NET-Laufzeit eine Gelegenheit gegeben, die Anforderung zu überprüfen. Da nur authentifizierte Benutzer autorisiert sind, den Inhalt im `PrivateDocs` Ordner anzuzeigen, wird der anonyme Besucher automatisch zur Anmeldeseite umgeleitet (siehe Abbildung 3).

> [!NOTE]
> Wenn der webhostinstanbieter weiterhin IIS 6 verwendet, können Sie die integrierte Pipeline Funktion nicht verwenden. Eine Problem Umgehung besteht darin, private Dokumente in einem Ordner zu platzieren, der den HTTP-Zugriff untersagt (z. b. `App_Data`), und anschließend eine Seite zu erstellen, die diese Dokumente verarbeitet. Diese Seite wird möglicherweise `GetPDF.aspx`aufgerufen, und der Name der PDF-Datei wird über einen QueryString-Parameter übergeben. Auf der Seite `GetPDF.aspx` wird zuerst überprüft, ob der Benutzer über die Berechtigung zum Anzeigen der Datei verfügt. wenn dies der Fall ist, wird die [`Response.WriteFile(filePath)`](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) -Methode verwendet, um den Inhalt der angeforderten PDF-Datei an den anfordernden Client zurückzusenden. Diese Technik funktioniert auch für IIS 7, wenn Sie die integrierte Pipeline nicht aktivieren möchten.

## <a name="summary"></a>Zusammenfassung

Webanwendungen in einer Produktionsumgebung werden mithilfe der IIS-Webserver Software von Microsoft gehostet. In der Entwicklungsumgebung kann die Anwendung jedoch mit IIS oder dem ASP.NET Development Server gehostet werden. Im Idealfall sollte dieselbe Webserver Software in beiden Umgebungen verwendet werden, da bei der Verwendung unterschiedlicher Software eine weitere Variable in der Mischung hinzugefügt wird. Die einfache Verwendung des ASP.NET Development Server macht es jedoch zu einer attraktiven Wahl in der Entwicklungsumgebung. Die gute Nachricht ist, dass es nur einige grundlegende Unterschiede zwischen IIS und dem ASP.NET Development Server gibt. Wenn Sie diese Unterschiede kennen, können Sie Maßnahmen ergreifen, um sicherzustellen, dass die Anwendung unabhängig von der Umgebung.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.NET-Integration mit IIS 7,0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Verwenden der ASP.net-forenauthentifizierung mit allen Arten von Inhalten in IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Webserver in Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Zurück](common-configuration-differences-between-development-and-production-cs.md)
> [Weiter](deploying-a-database-cs.md)
