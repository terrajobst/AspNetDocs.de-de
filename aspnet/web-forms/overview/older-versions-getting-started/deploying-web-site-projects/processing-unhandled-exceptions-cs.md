---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: Verarbeiten von nicht behandelten AusnahmenC#() | Microsoft-Dokumentation
author: rick-anderson
description: Wenn ein Laufzeitfehler in einer Webanwendung in der Produktion auftritt, ist es wichtig, einen Entwickler zu benachrichtigen und den Fehler zu protokollieren, damit er möglicherweise bei einem La-...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 27d827238d944f86cd913d2b8ecd12729b99f391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510657"
---
# <a name="processing-unhandled-exceptions-c"></a>Verarbeiten von Ausnahmefehlern (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs/samples) ([Vorgehensweise zum Herunterladen](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Wenn ein Laufzeitfehler in einer Webanwendung in der Produktion auftritt, ist es wichtig, einen Entwickler zu benachrichtigen und den Fehler zu protokollieren, damit er zu einem späteren Zeitpunkt diagnostiziert werden kann. Dieses Tutorial bietet einen Überblick darüber, wie ASP.net Laufzeitfehler verarbeitet und eine Möglichkeit bietet, benutzerdefinierten Code immer dann auszuführen, wenn eine nicht behandelte Ausnahme auf die ASP.NET-Laufzeit zugreift.

## <a name="introduction"></a>Einführung

Wenn eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung auftritt, wird die ASP.NET-Laufzeit hochskalieren. Dadurch wird das `Error`-Ereignis ausgelöst, und die entsprechende Fehlerseite wird angezeigt. Es gibt drei verschiedene Arten von Fehlerseiten: den gelben Bildschirm für den Laufzeitfehler (Ysod); Ausnahme Details Ysod; und benutzerdefinierte Fehlerseiten. Im [vorherigen Tutorial](displaying-a-custom-error-page-cs.md) haben wir die Anwendung so konfiguriert, dass Sie eine benutzerdefinierte Fehlerseite für Remote Benutzer verwendet, und die Ausnahme Details Ysod für Benutzer, die lokal besuchen.

Die Verwendung einer benutzerfreundlichen, benutzerfreundlichen Fehlerseite, die dem Aussehen und dem Gefühl der Site entspricht, wird dem Standard-Lauf Zeit Fehler Ysod vorgezogen, aber das Anzeigen einer benutzerdefinierten Fehlerseite ist nur ein Teil einer umfassenden Lösung für die Fehlerbehandlung. Wenn in einer Anwendung in der Produktionsumgebung ein Fehler auftritt, ist es wichtig, dass die Entwickler über den Fehler benachrichtigt werden, damit Sie die Ursache der Ausnahme lösen und beheben können. Außerdem sollten die Fehlerdetails protokolliert werden, damit der Fehler zu einem späteren Zeitpunkt untersucht und diagnostiziert werden kann.

In diesem Tutorial wird gezeigt, wie Sie auf die Details einer nicht behandelten Ausnahme zugreifen, damit Sie protokolliert und von einem Entwickler benachrichtigt werden können. In den beiden folgenden Tutorials werden die Fehler Protokollierungs Bibliotheken untersucht, die Entwicklern nach einiger Konfiguration automatisch über Laufzeitfehler benachrichtigt und deren Details protokolliert werden.

> [!NOTE]
> Die in diesem Tutorial untersuchten Informationen sind besonders nützlich, wenn Sie nicht behandelte Ausnahmen auf eindeutige oder angepasste Weise verarbeiten müssen. In Fällen, in denen Sie die Ausnahme nur protokollieren und einen Entwickler benachrichtigen müssen, ist die Verwendung einer Fehler Protokollierungs Bibliothek der Weg. In den nächsten beiden Tutorials wird eine Übersicht über zwei dieser Bibliotheken bereitgestellt.

## <a name="executing-code-when-theerrorevent-is-raised"></a>Ausführen von Code, wenn das`Error`-Ereignis ausgelöst wird

Ereignisse bieten einen Mechanismus, mit dem signalisiert werden kann, dass etwas interessantes aufgetreten ist, und für ein anderes Objekt das Ausführen von Code als Reaktion. Als ASP.NET-Entwickler sind Sie daran gewöhnt, in Bezug auf Ereignisse zu denken. Wenn Sie Code ausführen möchten, wenn der Besucher auf eine bestimmte Schaltfläche klickt, erstellen Sie einen Ereignishandler für das `Click`-Ereignis der Schaltfläche, und fügen Sie den Code dort ein. Da die ASP.NET-Laufzeit das [`Error`-Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) auslöst, wenn eine nicht behandelte Ausnahme auftritt, folgt der Code für die Protokollierung der Fehlerdetails in einem Ereignishandler. Aber wie erstellen Sie einen Ereignishandler für das `Error` Ereignis?

Das `Error` Ereignis ist eines von vielen Ereignissen in der [`HttpApplication`-Klasse](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) , die während der Lebensdauer einer Anforderung in bestimmten Phasen in der HTTP-Pipeline ausgelöst werden. Beispielsweise wird das [`BeginRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) der `HttpApplication` Klasse zu Beginn jeder Anforderung ausgelöst. Das [`AuthenticateRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) wird ausgelöst, wenn der Anforderer von einem Sicherheitsmodul identifiziert wurde. Diese `HttpApplication` Ereignisse haben der Seiten Entwickler die Möglichkeit, benutzerdefinierte Logik an den verschiedenen Punkten der Lebensdauer einer Anforderung auszuführen.

Ereignishandler für die `HttpApplication` Ereignisse können in eine spezielle Datei namens `Global.asax`eingefügt werden. Um diese Datei auf Ihrer Website zu erstellen, fügen Sie dem Stammverzeichnis Ihrer Website ein neues Element mit der globalen Anwendungs Klassen Vorlage mit dem Namen `Global.asax`hinzu.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen von `Global.asax` zu Ihrer Webanwendung  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](processing-unhandled-exceptions-cs/_static/image3.png))

Der Inhalt und die Struktur der von Visual Studio erstellten `Global.asax` Datei unterscheiden sich geringfügig je nachdem, ob Sie ein Webanwendungs Projekt (WAP) oder ein Website Projekt (WSP) verwenden. Bei einem WAP wird der `Global.asax` als zwei separate Dateien implementiert: `Global.asax` und `Global.asax.cs`. Die `Global.asax` Datei enthält nichts, aber eine `@Application` Direktive, die auf die `.cs` Datei verweist. die Ereignishandler, die von Interesse sind, werden in der `Global.asax.cs`-Datei definiert. Für wsps wird nur eine einzelne Datei erstellt, `Global.asax`und die Ereignishandler werden in einem `<script runat="server">` Block definiert.

Die `Global.asax` Datei, die in einem WAP durch die globale Anwendungs Klassen Vorlage von Visual Studio erstellt wurde, enthält Ereignishandler mit dem Namen `Application_BeginRequest`, `Application_AuthenticateRequest`und `Application_Error`, bei denen es sich um Ereignishandler für die `HttpApplication` Ereignisse `BeginRequest`, `AuthenticateRequest`und `Error`handelt. Es gibt auch Ereignishandler mit dem Namen `Application_Start`, `Session_Start`, `Application_End`und `Session_End`, bei denen es sich um Ereignishandler handelt, die beim Start der Webanwendung ausgelöst werden, wenn eine neue Sitzung gestartet wird, wenn die Anwendung beendet wird, bzw. Wenn eine Sitzung beendet wird. Die `Global.asax` Datei, die in einem WSP von Visual Studio erstellt wurde, enthält nur die Ereignishandler `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`und `Session_End`.

> [!NOTE]
> Wenn Sie die ASP.NET-Anwendung bereitstellen, müssen Sie die `Global.asax` Datei in die Produktionsumgebung kopieren. Die `Global.asax.cs`-Datei, die im WAP erstellt wird, muss nicht in die Produktion kopiert werden, da dieser Code in die Assembly des Projekts kompiliert wird.

Die von der globalen Anwendungs Klassen Vorlage von Visual Studio erstellten Ereignishandler sind nicht vollständig. Sie können einen Ereignishandler für ein beliebiges `HttpApplication` Ereignis hinzufügen, indem Sie den Ereignishandler `Application_EventName`benennen. Beispielsweise können Sie den folgenden Code in der `Global.asax`-Datei hinzufügen, um einen Ereignishandler für das [`AuthorizeRequest`-Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)zu erstellen:

[!code-cs[Main](processing-unhandled-exceptions-cs/samples/sample1.cs)]

Ebenso können Sie alle von der globalen Anwendungs Klassen Vorlage erstellten Ereignishandler entfernen, die nicht benötigt werden. Für dieses Tutorial benötigen wir nur einen Ereignishandler für das `Error`-Ereignis. Sie können die anderen Ereignishandler aus der `Global.asax` Datei entfernen.

> [!NOTE]
> *HTTP-Module* bieten eine weitere Möglichkeit zum Definieren von Ereignis Handlern für `HttpApplication` Ereignisse. HTTP-Module werden als Klassendatei erstellt, die direkt innerhalb des Webanwendungs Projekts platziert oder in eine separate Klassenbibliothek aufgeteilt werden kann. Da Sie in eine Klassenbibliothek aufgeteilt werden können, bieten HTTP-Module ein flexibleres und wiederverwendbares Modell zum Erstellen von `HttpApplication` Ereignis Handlern. Während die `Global.asax` Datei spezifisch für die Webanwendung ist, in der Sie sich befindet, können HTTP-Module in Assemblys kompiliert werden. zu diesem Zeitpunkt ist das Hinzufügen des HTTP-Moduls zu einer Website ganz einfach, wenn die Assembly im Ordner `Bin` gelöscht und das Modul in `Web.config`registriert wird. In diesem Tutorial wird das Erstellen und Verwenden von HTTP-Modulen nicht untersucht. die zwei in den folgenden beiden Tutorials verwendeten Fehler Protokollierungs Bibliotheken werden jedoch als HTTP-Module implementiert. Weitere Hintergrundinformationen zu den Vorteilen von HTTP-Modulen finden Sie unter [Verwenden von HTTP-Modulen und-Handlern zum Erstellen von austauschbaren ASP.NET-Komponenten](https://msdn.microsoft.com/library/aa479332.aspx).

## <a name="retrieving-information-about-the-unhandled-exception"></a>Abrufen von Informationen über die nicht behandelte Ausnahme

An dieser Stelle haben wir eine Global. asax-Datei mit einem `Application_Error`-Ereignishandler. Wenn dieser Ereignishandler ausgeführt wird, müssen wir einen Entwickler über den Fehler benachrichtigen und seine Details protokollieren. Um diese Aufgaben auszuführen, müssen wir zuerst die Details der ausgelöste Ausnahme bestimmen. Verwenden Sie die`GetLastError`- [Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) des Server Objekts zum Abrufen der Details der nicht behandelten Ausnahme, die das Auslösen des `Error` Ereignisses bewirkt hat.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

Die `GetLastError`-Methode gibt ein Objekt vom Typ `Exception`zurück. Hierbei handelt es sich um den Basistyp für alle Ausnahmen in der .NET Framework. Im obigen Code wird jedoch das vom `GetLastError` zurückgegebene Ausnahme Objekt in ein `HttpException` Objekt umgewandelt. Wenn das `Error` Ereignis ausgelöst wird, weil eine Ausnahme während der Verarbeitung einer ASP.NET-Ressource ausgelöst wurde, wird die ausgelöste Ausnahme in eine `HttpException`umgerückt. Um die tatsächliche Ausnahme, die das Fehler Ereignis ausgelöst hat, zu erhalten, verwenden Sie die `InnerException`-Eigenschaft. Wenn das `Error` Ereignis aufgrund einer HTTP-basierten Ausnahme ausgelöst wurde (z. b. eine Anforderung für eine nicht vorhandene Seite), wird eine `HttpException` ausgelöst, aber keine innere Ausnahme.

Der folgende Code verwendet die `GetLastErrormessage`, um Informationen über die Ausnahme abzurufen, die das `Error` Ereignis ausgelöst hat, wobei die `HttpException` in einer Variablen mit dem Namen `lastErrorWrapper`gespeichert wird. Anschließend werden der Typ, die Nachricht und die Stapel Überwachung der Ursprungs Ausnahme in drei Zeichen folgen Variablen gespeichert. dabei wird überprüft, ob es sich bei der `lastErrorWrapper` um die eigentliche Ausnahme handelt, die das `Error` Ereignis ausgelöst hat (im Fall von http-basierten Ausnahmen), oder ob es sich um einen Wrapper für eine Ausnahme handelt, die während der Verarbeitung der Anforderung ausgelöst wurde.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

An diesem Punkt verfügen Sie über alle Informationen, die Sie benötigen, um Code zu schreiben, mit dem die Details der Ausnahme in einer Datenbanktabelle protokolliert werden. Sie können eine Datenbanktabelle mit Spalten für alle Fehlerdetails von Interesse erstellen, z. b. den Typ, die Nachricht, die Stapel Überwachung usw., zusammen mit anderen nützlichen Informationen, wie z. b. die URL der angeforderten Seite und den Namen des aktuell angemeldeten Benutzers. Im `Application_Error`-Ereignishandler stellen Sie dann eine Verbindung mit der Datenbank her und fügen einen Datensatz in die Tabelle ein. Ebenso könnten Sie Code hinzufügen, um einen Entwickler über den Fehler per e-Mail zu benachrichtigen.

Die in den nächsten beiden Tutorials untersuchten Fehler Protokollierungs Bibliotheken stellen eine solche Funktionalität bereit, sodass Sie diese Fehler Protokollierung und Benachrichtigung nicht selbst erstellen müssen. Um jedoch zu veranschaulichen, dass das `Error` Ereignis ausgelöst wird und der `Application_Error`-Ereignishandler zum Protokollieren von Fehlerdetails und zum Benachrichtigen eines Entwicklers verwendet werden kann, fügen Sie Code hinzu, der einen Entwickler benachrichtigt, wenn ein Fehler auftritt.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Benachrichtigen eines Entwicklers, wenn eine nicht behandelte Ausnahme auftritt

Wenn in der Produktionsumgebung eine nicht behandelte Ausnahme auftritt, ist es wichtig, das Entwicklungsteam darauf aufmerksam zu machen, dass Sie den Fehler bewerten und ermitteln können, welche Aktionen ausgeführt werden müssen. Wenn beim Herstellen einer Verbindung mit der Datenbank z. b. ein Fehler auftritt, müssen Sie die Verbindungs Zeichenfolge überprüfen und ggf. ein Support Ticket mit Ihrem Webhostingunternehmen öffnen. Wenn die Ausnahme aufgrund eines Programmierfehlers aufgetreten ist, muss möglicherweise zusätzlicher Code oder eine Validierungs Logik hinzugefügt werden, um solche Fehler in der Zukunft zu verhindern.

Die .NET Framework Klassen im [`System.Net.Mail`-Namespace](https://msdn.microsoft.com/library/system.net.mail.aspx) erleichtern das Senden einer e-Mail. Die [`MailMessage`-Klasse](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) stellt eine e-Mail-Nachricht dar und verfügt über Eigenschaften wie `To`, `From`, `Subject`, `Body`und `Attachments`. Der `SmtpClass` wird verwendet, um ein `MailMessage` Objekt mithilfe eines angegebenen SMTP-Servers zu senden. die SMTP-Servereinstellungen können Programm gesteuert oder deklarativ im`<system.net>`- [Element](https://msdn.microsoft.com/library/6484zdc1.aspx) in der `Web.config file`angegeben werden. Weitere Informationen zum Senden von e-Mail-Nachrichten in einer ASP.NET-Anwendung finden Sie in meinem Artikel, [Senden von e-Mails in ASP.net und in](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)den häufig gestellten Fragen zu [System .net. Mail](http://systemnetmail.com/).

> [!NOTE]
> Das `<system.net>`-Element enthält die SMTP-Servereinstellungen, die von der `SmtpClient`-Klasse zum Senden einer e-Mail verwendet werden. Ihr Webhostingunternehmen verfügt wahrscheinlich über einen SMTP-Server, den Sie zum Senden von e-Mails von Ihrer Anwendung verwenden können. Informationen zu den SMTP-Servereinstellungen, die Sie in Ihrer Webanwendung verwenden sollten, finden Sie im Abschnitt zur Unterstützung des Webhosts.

Fügen Sie dem `Application_Error`-Ereignishandler den folgenden Code hinzu, um dem Entwickler eine e-Mail zu senden, wenn ein Fehler auftritt:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Der obige Code ist zwar recht langwierig, aber der größte Teil des Codes erstellt den HTML-Code, der in der e-Mail angezeigt wird, die an den Entwickler gesendet wird. Der Code beginnt mit dem Verweis auf die `HttpException`, die von der `GetLastError`-Methode (`lastErrorWrapper`) zurückgegeben wurde. Die tatsächliche Ausnahme, die von der Anforderung ausgelöst wurde, wird über `lastErrorWrapper.InnerException` abgerufen und der Variablen `lastError`zugewiesen. Der Typ, die Nachricht und die Stapel Überwachungsinformationen werden aus `lastError` abgerufen und in drei Zeichen folgen Variablen gespeichert.

Als nächstes wird ein `MailMessage` Objekt mit dem Namen "`mm`" erstellt. Der e-Mail-Text ist im HTML-Format formatiert und zeigt die URL der angeforderten Seite, den Namen des aktuell angemeldeten Benutzers und Informationen zur Ausnahme an (Typ, Nachricht und Stapel Überwachung). Eines der coolthings für die `HttpException`-Klasse besteht darin, dass Sie den HTML-Code generieren können, der zum Erstellen der Ausnahme Details gelb Screen of Death (Ysod) durch Aufrufen der [GetHtmlErrorMessage-Methode](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx)verwendet wird. Diese Methode wird hier verwendet, um die Ausnahme Details Ysod-Markup abzurufen und Sie der e-Mail als Anlage hinzuzufügen. Eine Vorsicht: Wenn die Ausnahme, die das `Error` Ereignis ausgelöst hat, eine HTTP-basierte Ausnahme war (z. b. eine Anforderung für eine nicht vorhandene Seite), gibt die `GetHtmlErrorMessage`-Methode `null`zurück.

Der letzte Schritt besteht darin, die `MailMessage`zu senden. Dies erfolgt durch Erstellen einer neuen `SmtpClient`-Methode und Aufrufen der `Send`-Methode.

> [!NOTE]
> Bevor Sie diesen Code in Ihrer Webanwendung verwenden, müssen Sie die Werte in den `ToAddress`-und `FromAddress` Konstanten von support@example.com in eine beliebige e-Mail-Adresse ändern, an die die Fehlerbenachrichtigungs-e-Mail gesendet werden soll. Sie müssen auch die SMTP-Servereinstellungen im Abschnitt `<system.net>` in `Web.config`angeben. Überprüfen Sie die zu verwendenden SMTP-Servereinstellungen über Ihren webhostinstanbieter.

Wenn dieser Code immer vorhanden ist, wird dem Entwickler eine e-Mail-Nachricht gesendet, die den Fehler zusammenfasst und die Ysod enthält. Im vorangehenden Tutorial haben wir einen Laufzeitfehler durch den Besuch von Genre. aspx und das Übergeben eines ungültigen `ID` Werts über die QueryString, wie `Genre.aspx?ID=foo`, veranschaulicht. Wenn Sie die Seite mit der `Global.asax` Datei direkt aufrufen, erhalten Sie dieselbe Benutzerumgebung wie im vorherigen Tutorial: in der Entwicklungsumgebung sehen Sie weiterhin die Ausnahme Details gelbe Bildschirm "Tod", während Sie in der Produktionsumgebung die benutzerdefinierte Fehlerseite sehen. Zusätzlich zu diesem vorhandenen Verhalten wird dem Entwickler eine e-Mail gesendet.

**Abbildung 2** zeigt die beim Besuch `Genre.aspx?ID=foo`empfangene e-Mail. Der e-Mail-Text fasst die Ausnahme Informationen zusammen, während die `YSOD.htm` Anlage den Inhalt anzeigt, der in der Ausnahme Details Ysod angezeigt wird (siehe **Abbildung 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Abbildung 2**: dem Entwickler wird eine e-Mail-Benachrichtigung gesendet, wenn eine nicht behandelte Ausnahme vorliegt.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Abbildung 3**: die e-Mail-Benachrichtigung enthält die Ausnahme Details Ysod als Anlage.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Was ist mit der Verwendung der benutzerdefinierten Fehlerseite?

In diesem Tutorial wurde gezeigt, wie `Global.asax` und der `Application_Error`-Ereignishandler verwendet werden, um Code auszuführen, wenn eine nicht behandelte Ausnahme auftritt. Insbesondere wurde dieser Ereignishandler verwendet, um einen Entwickler über einen Fehler zu benachrichtigen. Wir könnten ihn erweitern, um auch die Fehlerdetails in einer Datenbank zu protokollieren. Das vorhanden sein des `Application_Error` Ereignis Handlers wirkt sich nicht auf die Benutzeroberflächen des Endbenutzers aus. Die konfigurierte Fehlerseite wird weiterhin angezeigt. dabei handelt es sich um die Fehler Details Ysod, den Laufzeitfehler Ysod oder die benutzerdefinierte Fehlerseite.

Wenn eine benutzerdefinierte Fehlerseite verwendet wird, sollten Sie sich natürlich wundern, ob die `Global.asax`-Datei und das `Application_Error` Ereignis notwendig sind. Wenn ein Fehler auftritt, wird dem Benutzer die benutzerdefinierte Fehlerseite angezeigt. Warum kann der Code nicht zum Benachrichtigen des Entwicklers und zum Protokollieren der Fehlerdetails in der Code-Behind-Klasse der benutzerdefinierten Fehlerseite hinzugefügt werden? Obwohl Sie der Code Behind-Klasse der benutzerdefinierten Fehlerseite Code hinzufügen können, haben Sie keinen Zugriff auf die Details der Ausnahme, die das `Error` Ereignis ausgelöst hat, wenn Sie das Verfahren verwenden, das wir im vorherigen Tutorial untersucht haben. Wenn Sie die `GetLastError` Methode von der benutzerdefinierten Fehlerseite aufrufen, wird `Nothing`zurückgegeben.

Der Grund für dieses Verhalten liegt darin, dass die benutzerdefinierte Fehlerseite über eine Umleitung erreicht wird. Wenn eine nicht behandelte Ausnahme die ASP.NET Runtime erreicht, löst die ASP.net-Engine das `Error`-Ereignis aus (das den `Application_Error`-Ereignishandler ausführt) und *leitet* den Benutzer dann zur benutzerdefinierten Fehlerseite um, indem ein `Response.Redirect(customErrorPageUrl)`ausgegeben wird. Die `Response.Redirect`-Methode sendet eine Antwort an den Client mit dem Statuscode HTTP 302, der den Browser anweist, eine neue URL anzufordern, nämlich die benutzerdefinierte Fehlerseite. Der Browser fordert diese neue Seite dann automatisch an. Sie können feststellen, dass die benutzerdefinierte Fehlerseite separat von der Seite angefordert wurde, aus der der Fehler entstanden ist, weil die Adressleiste des Browsers in die URL der benutzerdefinierten Fehlerseite wechselt (siehe **Abbildung 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Abbildung 4**: Wenn ein Fehler auftritt, wird der Browser an die URL der benutzerdefinierten Fehlerseite umgeleitet.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](processing-unhandled-exceptions-cs/_static/image12.png))

Der Nettoeffekt ist, dass die Anforderung, bei der die nicht behandelte Ausnahme aufgetreten ist, beendet wird, wenn der Server mit der HTTP 302-Umleitung antwortet. Bei der nachfolgenden Anforderung an die benutzerdefinierte Fehlerseite handelt es sich um eine Brand neue Anforderung. an diesem Punkt hat das ASP.NET-Modul die Fehlerinformationen verworfen und verfügt darüber hinaus nicht über die Möglichkeit, die nicht behandelte Ausnahme in der vorherigen Anforderung der neuen Anforderung für die benutzerdefinierte Fehlerseite zuzuordnen. Aus diesem Grund gibt `GetLastError` `null` zurück, wenn es von der benutzerdefinierten Fehlerseite aufgerufen wird.

Es ist jedoch möglich, dass die benutzerdefinierte Fehlerseite während der gleichen Anforderung ausgeführt wird, die den Fehler verursacht hat. Mit der [`Server.Transfer(url)`](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) -Methode wird die Ausführung an die angegebene URL übertragen und innerhalb derselben Anforderung verarbeitet. Sie können den Code im `Application_Error`-Ereignishandler in die Code Behind-Klasse der benutzerdefinierten Fehlerseite verschieben und `Global.asax` Sie durch den folgenden Code ersetzen:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

Wenn eine nicht behandelte Ausnahme auftritt, überträgt der `Application_Error` Ereignishandler die Steuerung auf der Grundlage des HTTP-Statuscodes an die entsprechende benutzerdefinierte Fehlerseite. Da das Steuerelement übertragen wurde, kann die benutzerdefinierte Fehlerseite über `Server.GetLastError` auf die Informationen zur Ausnahme Ausnahme zugreifen und den Entwickler über den Fehler informieren und seine Details protokollieren. Der `Server.Transfer`-Befehl beendet die ASP.net-Engine daran, den Benutzer an die benutzerdefinierte Fehlerseite weiterzuleiten. Stattdessen wird der Inhalt der benutzerdefinierten Fehlerseite als Antwort auf die Seite zurückgegeben, die den Fehler generiert hat.

## <a name="summary"></a>Zusammenfassung

Wenn eine nicht behandelte Ausnahme in einer ASP.NET-Webanwendung auftritt, löst die ASP.NET-Laufzeit das `Error`-Ereignis aus und zeigt die konfigurierte Fehlerseite an. Wir können den Entwickler über den Fehler benachrichtigen, seine Details protokollieren oder auf andere Weise verarbeiten, indem Sie einen Ereignishandler für das Fehler Ereignis erstellen. Es gibt zwei Möglichkeiten, einen Ereignishandler für `HttpApplication` Ereignisse wie `Error`zu erstellen: in der `Global.asax`-Datei oder in einem HTTP-Modul. In diesem Tutorial wurde gezeigt, wie Sie einen `Error` Ereignishandler in der `Global.asax`-Datei erstellen, die Entwicklern über einen Fehler über eine e-Mail-Nachricht benachrichtigt.

Das Erstellen eines `Error` Ereignis Handlers ist nützlich, wenn Sie nicht behandelte Ausnahmen auf eindeutige oder angepasste Weise verarbeiten müssen. Das Erstellen eines eigenen `Error` Ereignis Handlers zum Protokollieren der Ausnahme oder zum Benachrichtigen eines Entwicklers ist jedoch nicht die effizienteste Verwendung ihrer Zeit, da bereits eine kostenlose und leicht zu verwendende Fehler Protokollierungs Bibliothek vorhanden ist, die innerhalb weniger Minuten eingerichtet werden kann. In den nächsten beiden Tutorials werden zwei solcher Bibliotheken untersucht.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.net Übersicht über HTTP-Module und HTTP-Handler](https://support.microsoft.com/kb/307985)
- [Ordnungsgemäße Reaktion auf nicht behandelte Ausnahmen: Verarbeitung nicht behandelter Ausnahmen](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`-Klasse und das ASP.NET-Anwendungs Objekt](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP-Handler und HTTP-Module in ASP.net](http://www.15seconds.com/Issue/020417.htm)
- [Senden von e-Mails in ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Grundlegendes zur `Global.asax` Datei](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Verwenden von HTTP-Modulen und-Handlern zum Erstellen von austauschbaren ASP.NET-Komponenten](https://msdn.microsoft.com/library/aa479332.aspx)
- [Arbeiten mit der ASP.net-`Global.asax` Datei](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Arbeiten mit `HttpApplication`-Instanzen](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Zurück](displaying-a-custom-error-page-cs.md)
> [Weiter](logging-error-details-with-asp-net-health-monitoring-cs.md)
