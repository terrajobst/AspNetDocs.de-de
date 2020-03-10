---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.net-Fehlerbehandlung | Microsoft-Dokumentation
author: Erikre
description: Diese tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 9514142ca50b33470a3f4c033e4f8e319a9ee09b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457023"
---
# <a name="aspnet-error-handling"></a>ASP.NET – Fehlerbehandlung

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für das Web. Für diese tutorialreihe steht ein Visual Studio 2013- [Projekt mit C# Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) zur Verfügung.

In diesem Tutorial ändern Sie die Wingtip Toys-Beispielanwendung, um Fehlerbehandlung und Fehler Protokollierung einzubeziehen. Die Fehlerbehandlung ermöglicht es der Anwendung, Fehler ordnungsgemäß zu behandeln und Fehlermeldungen entsprechend anzuzeigen. Die Fehler Protokollierung ermöglicht es Ihnen, aufgetretene Fehler zu finden und zu beheben. Dieses Tutorial baut auf dem vorherigen Tutorial "URL-Routing" auf und ist Teil der Wingtip Toys-tutorialreihe.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- So fügen Sie der Konfiguration der Anwendung eine globale Fehlerbehandlung hinzu.
- Gewusst wie: Hinzufügen der Fehlerbehandlung auf Anwendungs-, Seiten-und Codeebene.
- Protokollieren von Fehlern für die spätere Überprüfung.
- Anzeigen von Fehlermeldungen, die die Sicherheit nicht beeinträchtigen.
- Gewusst wie: Implementieren von Fehler Protokollierungs Modulen und-Handlern (ELMAH)-Fehler Protokollierung.

## <a name="overview"></a>Übersicht

ASP.NET-Anwendungen müssen in der Lage sein, Fehler zu behandeln, die während der Ausführung konsistent auftreten. ASP.NET verwendet die Common Language Runtime (CLR), die eine Möglichkeit bietet, Anwendungen von Fehlern auf einheitliche Weise zu benachrichtigen. Wenn ein Fehler auftritt, wird eine Ausnahme ausgelöst. Eine Ausnahme ist ein beliebiger Fehler, eine Bedingung oder ein unerwartetes Verhalten, das von einer Anwendung erkannt wird.

In .NET Framework stellt eine Ausnahme ein Objekt dar, das von der `System.Exception`-Klasse erbt. Eine Ausnahme wird in einem Codebereich ausgelöst, in dem ein Fehler aufgetreten ist. Die Ausnahme wird in der-aufrufsstapel an eine Stelle, an der die Anwendung Code zur Behandlung der Ausnahme bereitstellt, übermittelt. Wenn die Anwendung die Ausnahme nicht behandelt, wird der Browser gezwungen, die Fehlerdetails anzuzeigen.

Als bewährte Vorgehensweise sollten Sie Fehler in auf der Codeebene in `Try`/`Catch`/`Finally`-Blöcke innerhalb Ihres Codes behandeln. Versuchen Sie, diese Blöcke zu platzieren, damit der Benutzer Probleme im Kontext beheben kann, in dem Sie auftreten. Wenn die Fehler Behandlungs Blöcke zu weit entfernt sind, wo der Fehler aufgetreten ist, wird es schwieriger, Benutzern die Informationen zur Verfügung zu stellen, die Sie zum Beheben des Problems benötigen.

### <a name="exception-class"></a>Exception-Klasse

Die Exception-Klasse ist die Basisklasse, von der Ausnahmen erben. Die meisten Ausnahme Objekte sind Instanzen einer abgeleiteten Klasse der Ausnahme Klasse, z. b. die `SystemException` Klasse, die `IndexOutOfRangeException`-Klasse oder die `ArgumentNullException`-Klasse. Die Exception-Klasse verfügt über Eigenschaften, z. b. die `StackTrace`-Eigenschaft, die `InnerException`-Eigenschaft und die `Message`-Eigenschaft, die spezifische Informationen über den aufgetretenen Fehler bereitstellen.

### <a name="exception-inheritance-hierarchy"></a>Ausnahme Vererbungs Hierarchie

Die Laufzeit verfügt über einen Basissatz von Ausnahmen, die von der `SystemException`-Klasse abgeleitet werden, die von der Laufzeit ausgelöst wird, wenn eine Ausnahme auftritt. Die meisten der Klassen, die von der Exception-Klasse erben, wie z. b. die `IndexOutOfRangeException`-Klasse und die `ArgumentNullException`-Klasse, implementieren keine weiteren Member. Daher finden sich die wichtigsten Informationen für eine Ausnahme in der Hierarchie von Ausnahmen, dem Namen der Ausnahme und den in der Ausnahme enthaltenen Informationen.

### <a name="exception-handling-hierarchy"></a>Ausnahme Behandlungs Hierarchie

In einer ASP.net-Web Forms Anwendung können Ausnahmen auf der Grundlage einer bestimmten Behandlungs Hierarchie behandelt werden. Eine Ausnahme kann auf den folgenden Ebenen behandelt werden:

- Anwendungsschicht
- Seitenebene
- Codeebene

Wenn eine Anwendung Ausnahmen behandelt, können zusätzliche Informationen über die Ausnahme, die von der Ausnahme Klasse geerbt wird, häufig abgerufen und dem Benutzer angezeigt werden. Zusätzlich zu Anwendungs-, Seiten-und Codeebene können Sie Ausnahmen auch auf der http-Modulebene und mithilfe eines benutzerdefinierten IIS-Handlers behandeln.

### <a name="application-level-error-handling"></a>Fehlerbehandlung auf Anwendungsebene

Sie können Standardfehler auf Anwendungsebene behandeln, indem Sie die Konfiguration Ihrer Anwendung ändern oder indem Sie in der Datei " *Global. asax* " Ihrer Anwendung einen `Application_Error`-Handler hinzufügen.

Sie können Standardfehler und HTTP-Fehler behandeln, indem Sie der Datei " *Web. config* " einen `customErrors` Abschnitt hinzufügen. Der `customErrors` Abschnitt ermöglicht Ihnen, eine Standardseite anzugeben, zu der Benutzer bei Auftreten eines Fehlers umgeleitet werden. Außerdem können Sie einzelne Seiten für bestimmte Statuscode Fehler angeben.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Wenn Sie die Konfiguration verwenden, um den Benutzer zu einer anderen Seite umzuleiten, haben Sie leider keine Details zu dem aufgetretenen Fehler.

Sie können jedoch Fehler abfangen, die an einer beliebigen Stelle in der Anwendung auftreten, indem Sie dem `Application_Error`-Handler in der Datei *Global. asax* Code hinzufügen.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Behandlung von Fehlerereignissen auf Seitenebene

Ein Handler auf Seitenebene gibt den Benutzer an die Seite zurück, auf der der Fehler aufgetreten ist, aber da Instanzen von Steuerelementen nicht beibehalten werden, wird auf der Seite nichts mehr angezeigt. Um dem Benutzer der Anwendung Fehlerdetails bereitzustellen, müssen Sie die Fehlerdetails explizit auf die Seite schreiben.

In der Regel verwenden Sie einen Fehlerhandler auf Seitenebene, um nicht behandelte Fehler zu protokollieren oder den Benutzer auf eine Seite zu übernehmen, auf der hilfreiche Informationen angezeigt werden können.

Dieses Codebeispiel zeigt einen Handler für das Fehler Ereignis auf einer ASP.NET-Webseite. Dieser Handler fängt alle Ausnahmen ab, die nicht bereits in `try`/`catch`-Blöcken auf der Seite behandelt werden.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Nachdem Sie einen Fehler behandelt haben, müssen Sie ihn löschen, indem Sie die `ClearError`-Methode des Server Objekts (`HttpServerUtility`-Klasse) aufrufen. andernfalls wird ein zuvor aufgetretene Fehler angezeigt.

### <a name="code-level-error-handling"></a>Fehlerbehandlung auf Codeebene

Die try-catch-Anweisung besteht aus einem try-Block gefolgt von einer oder mehreren catch-Klauseln, die Handler für verschiedene Ausnahmen angeben. Wenn eine Ausnahme ausgelöst wird, sucht der Common Language Runtime (CLR) nach der catch-Anweisung, die diese Ausnahme behandelt. Wenn die derzeit ausgeführte Methode keinen catch-Block enthält, untersucht die CLR die Methode, die die aktuelle Methode aufgerufen hat, usw., die Aufruf Stapel. Wenn kein Catch-Block gefunden wird, zeigt die CLR dem Benutzer eine Meldung über eine nicht behandelte Ausnahme an und beendet die Ausführung des Programms.

Das folgende Codebeispiel zeigt eine gängige Methode zum Verwenden von `try`/`catch`/`finally`, um Fehler zu behandeln.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Im obigen Code enthält der try-Block den Code, der vor einer möglichen Ausnahme geschützt werden muss. Der-Block wird ausgeführt, bis entweder eine Ausnahme ausgelöst oder der-Block erfolgreich abgeschlossen wurde. Wenn entweder eine `FileNotFoundException` Ausnahme oder eine `IOException` Ausnahme auftritt, wird die Ausführung auf eine andere Seite übertragen. Anschließend wird der Code, der im schließlich-Block enthalten ist, ausgeführt, und zwar unabhängig davon, ob ein Fehler aufgetreten ist.

## <a name="adding-error-logging-support"></a>Hinzufügen der Fehler Protokollierungs Unterstützung

Vor dem Hinzufügen der Fehlerbehandlung zur Wingtip Toys-Beispielanwendung fügen Sie die Unterstützung für die Fehler Protokollierung hinzu, indem Sie dem *Logik* Ordner eine `ExceptionUtility` Klasse hinzufügen. Auf diese Weise werden die Fehlerdetails der Fehlerprotokoll Datei hinzugefügt, wenn die Anwendung einen Fehler behandelt.

1. Klicken Sie mit der rechten Maustaste auf den Ordner *Logic* , und wählen Sie -&gt; **Neues Element** **Hinzufügen** aus.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie auf der linken Seite die Gruppe **Visual C#**  -&gt; **Code** Templates aus. Wählen Sie dann in der mittleren Liste **Klasse**aus, und nennen Sie Sie **ExceptionUtility.cs**.
3. Wählen Sie **Hinzufügen** aus. Die neue Klassendatei wird angezeigt.
4. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Wenn eine Ausnahme auftritt, kann die Ausnahme in eine Ausnahme Protokolldatei geschrieben werden, indem die `LogException`-Methode aufgerufen wird. Diese Methode übernimmt zwei Parameter: Das Exception-Objekt und eine Zeichenfolge, die Details über die Quelle der Ausnahme enthält. Das Ausnahme Protokoll wird in die Datei *ErrorLog. txt* im Ordner *App\_Data* geschrieben.

### <a name="adding-an-error-page"></a>Hinzufügen einer Fehlerseite

In der Wingtip Toys-Beispielanwendung wird eine Seite verwendet, um Fehler anzuzeigen. Auf der Seite Fehler wird eine sichere Fehlermeldung an die Benutzer der Website angezeigt. Wenn der Benutzer jedoch ein Entwickler ist, der eine HTTP-Anforderung sendet, die lokal auf dem Computer bereitgestellt wird, auf dem sich der Code befindet, werden auf der Fehlerseite zusätzliche Fehlerdetails angezeigt.

1. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen (**Wingtip Toys**), und wählen Sie -&gt; **Neues Element** **Hinzufügen** aus.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie auf der linken Seite die Gruppe **Visual C#**  -&gt; **Web** Templates aus. Wählen Sie in der mittleren Liste **Web Form mit Master Seite**aus, und nennen Sie es **ErrorPage. aspx**.
3. Klicken Sie auf **Hinzufügen**.
4. Wählen Sie die Datei *Site. Master* als Master Seite aus, und klicken Sie dann auf **OK**.
5. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Ersetzen Sie den vorhandenen Code des Code Behind (*ErrorPage.aspx.cs*), sodass er wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Wenn die Fehlerseite angezeigt wird, wird der `Page_Load` Ereignishandler ausgeführt. Im `Page_Load` Handler wird der Speicherort bestimmt, an dem der Fehler zuerst behandelt wurde. Anschließend wird der zuletzt aufgetretene Fehler durch den Rückruf der `GetLastError`-Methode des Server Objekts festgelegt. Wenn die Ausnahme nicht mehr vorhanden ist, wird eine generische Ausnahme erstellt. Wenn die HTTP-Anforderung lokal erstellt wurde, werden alle Fehlerdetails angezeigt. In diesem Fall werden nur auf dem lokalen Computer, auf dem die Webanwendung ausgeführt wird, diese Fehlerdetails angezeigt. Nachdem die Fehlerinformationen angezeigt wurden, wird der Fehler der Protokolldatei hinzugefügt, und der Fehler wird auf dem Server gelöscht.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Anzeigen nicht behandelter Fehlermeldungen für die Anwendung

Indem Sie der Datei " *Web. config* " einen `customErrors` Abschnitt hinzufügen, können Sie einfache Fehler, die in der gesamten Anwendung auftreten, schnell verarbeiten. Sie können auch angeben, wie Fehler auf Grundlage ihres Statuscode Werts behandelt werden sollen, z. b. 404-Datei wurde nicht gefunden.

#### <a name="update-the-configuration"></a>Aktualisieren der Konfiguration

Aktualisieren Sie die Konfiguration, indem Sie der Datei " *Web. config* " einen `customErrors` Abschnitt hinzufügen.

1. Suchen Sie in **Projektmappen-Explorer**nach der Datei *Web. config* im Stammverzeichnis der Wingtip Toys-Beispielanwendung, und öffnen Sie Sie.
2. Fügen Sie den `customErrors` Abschnitt der Datei " *Web. config* " im `<system.web>` Knoten wie folgt hinzu:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Speichern Sie die Datei *Web.config* .

Der `customErrors` Abschnitt gibt den Modus an, der auf "on" festgelegt ist. Außerdem wird die `defaultRedirect`angegeben, die der Anwendung mitteilt, zu welcher Seite bei Auftreten eines Fehlers navigiert werden soll. Außerdem haben Sie ein bestimmtes Error-Element hinzugefügt, das angibt, wie ein 404-Fehler behandelt wird, wenn eine Seite nicht gefunden wird. Später in diesem Tutorial fügen Sie zusätzliche Fehlerbehandlung hinzu, mit der die Details eines Fehlers auf Anwendungsebene erfasst werden.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt ausführen, um die aktualisierten Routen anzuzeigen.

1. Drücken Sie **F5** , um die Beispielanwendung Wingtip Toys auszuführen.  
 Der Browser wird geöffnet und zeigt die Seite *default. aspx* an.
2. Geben Sie die folgende URL in den Browser ein (stellen Sie sicher, dass **Sie Ihre** Portnummer verwenden):  
    `https://localhost:44300/NoPage.aspx`
3. Überprüfen Sie die im Browser angezeigte *ErrorPage. aspx* . 

    ![ASP.net-Fehlerbehandlung-Fehler bei Seite nicht gefunden](aspnet-error-handling/_static/image1.png)

Wenn Sie die Seite " *NOPAGE. aspx* " anfordern, die nicht vorhanden ist, werden auf der Fehlerseite die einfache Fehlermeldung und die detaillierten Fehlerinformationen angezeigt, wenn weitere Details verfügbar sind. Wenn der Benutzer jedoch eine nicht vorhandene Seite an einem Remote Speicherort angefordert hat, wird auf der Fehlerseite nur die Fehlermeldung rot angezeigt.

### <a name="including-an-exception-for-testing-purposes"></a>Einschließen einer Ausnahme zu Testzwecken

Um zu überprüfen, wie Ihre Anwendung bei Auftreten eines Fehlers funktioniert, können Sie absichtlich Fehlerbedingungen in ASP.NET erstellen. In der Wingtip Toys-Beispielanwendung lösen Sie eine Test Ausnahme aus, wenn die Standardseite geladen wird, um zu sehen, was passiert.

1. Öffnen Sie die Code Behind-Seite der *default. aspx* -Seite in Visual Studio.   
   Die *default.aspx.cs* -Code Behind-Seite wird angezeigt.
2. Fügen Sie im `Page_Load` Handler Code hinzu, damit der Handler wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Es ist möglich, verschiedene Typen von Ausnahmen zu erstellen. Im obigen Code erstellen Sie eine `InvalidOperationException`, wenn die Seite " *default. aspx* " geladen wird.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung ausführen, um zu sehen, wie die Anwendung die Ausnahme behandelt.

1. Drücken Sie **STRG + F5** , um die Beispielanwendung Wingtip Toys auszuführen.  
 Die Anwendung löst die InvalidOperationException aus. 

    > [!NOTE] 
    > 
    > Sie müssen **STRG + F5** drücken, um die Seite anzuzeigen, ohne in den Code zu unterbrechen, um die Quelle des Fehlers in Visual Studio anzuzeigen.
2. Überprüfen Sie die im Browser angezeigte *ErrorPage. aspx* . 

    ![ASP.net-Fehlerbehandlung-Fehlerseite](aspnet-error-handling/_static/image2.png)

Wie Sie in den Fehlerdetails sehen können, wurde die Ausnahme vom `customError` Abschnitt in der Datei " *Web. config* " aufgefangen.

### <a name="adding-application-level-error-handling"></a>Hinzufügen der Fehlerbehandlung auf Anwendungsebene

Anstatt die Ausnahme mithilfe des `customErrors` Abschnitts in der Datei " *Web. config* " abzufangen, bei der Sie nur wenige Informationen über die Ausnahme erhalten, können Sie den Fehler auf Anwendungsebene abfangen und Fehlerdetails abrufen.

1. Suchen und öffnen Sie in **Projektmappen-Explorer**die Datei *Global.asax.cs* .
2. Fügen Sie einen **Anwendungs\_Fehler** Handler hinzu, sodass er wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Wenn in der Anwendung ein Fehler auftritt, wird der `Application_Error`-Handler aufgerufen. In diesem Handler wird die letzte Ausnahme abgerufen und überprüft. Wenn die Ausnahme nicht behandelt wurde und die Ausnahme Details zu inneren Ausnahmen enthält (d. h. `InnerException` nicht NULL ist), überträgt die Anwendung die Ausführung an die Fehlerseite, auf der die Ausnahme Details angezeigt werden.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung ausführen, um die zusätzlichen Fehlerdetails anzuzeigen, indem Sie die Ausnahme auf Anwendungsebene behandeln.

1. Drücken Sie **STRG + F5** , um die Beispielanwendung Wingtip Toys auszuführen.  
 Die Anwendung löst die `InvalidOperationException` aus.
2. Überprüfen Sie die im Browser angezeigte *ErrorPage. aspx* . 

    ![ASP.net-Fehlerbehandlung-Fehler auf Anwendungsebene](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Hinzufügen der Fehlerbehandlung auf Seitenebene

Sie können eine Fehlerbehandlung auf Seitenebene zu einer Seite hinzufügen, indem Sie der `@Page`-Anweisung der Seite ein `ErrorPage` Attribut hinzufügen oder indem Sie dem Code-Behind einer Seite einen `Page_Error` Ereignishandler hinzufügen. In diesem Abschnitt fügen Sie einen `Page_Error` Ereignishandler hinzu, der die Ausführung auf die Seite *ErrorPage. aspx* überträgt.

1. Suchen und öffnen Sie in **Projektmappen-Explorer**die Datei *default.aspx.cs* .
2. Fügen Sie einen `Page_Error` Handler hinzu, damit Code Behind wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Wenn auf der Seite ein Fehler auftritt, wird der `Page_Error` Ereignishandler aufgerufen. In diesem Handler wird die letzte Ausnahme abgerufen und überprüft. Wenn eine `InvalidOperationException` auftritt, überträgt der `Page_Error`-Ereignishandler die Ausführung an die Fehlerseite, auf der die Ausnahme Details angezeigt werden.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt ausführen, um die aktualisierten Routen anzuzeigen.

1. Drücken Sie **STRG + F5** , um die Beispielanwendung Wingtip Toys auszuführen.  
 Die Anwendung löst die `InvalidOperationException` aus.
2. Überprüfen Sie die im Browser angezeigte *ErrorPage. aspx* . 

    ![ASP.net-Fehlerbehandlung-Fehler auf Seitenebene](aspnet-error-handling/_static/image4.png)
3. Schließen Sie das Browserfenster.

### <a name="removing-the-exception-used-for-testing"></a>Entfernen der zum Testen verwendeten Ausnahme

Damit die Wingtip Toys-Beispielanwendung funktioniert, ohne die Ausnahme auszulösen, die Sie zuvor in diesem Tutorial hinzugefügt haben, entfernen Sie die Ausnahme.

1. Öffnen Sie die Code Behind-Datei der Seite " *default. aspx* ".
2. Entfernen Sie im `Page_Load` Handler den Code, der die Ausnahme auslöst, sodass der Handler wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Hinzufügen der Fehler Protokollierung auf Code Ebene

Wie bereits weiter oben in diesem Tutorial erwähnt, können Sie try/catch-Anweisungen hinzufügen, um zu versuchen, einen Code Abschnitt auszuführen und den ersten Fehler zu behandeln, der auftritt. In diesem Beispiel werden die Fehlerdetails nur in die Fehlerprotokoll Datei geschrieben, damit der Fehler später überprüft werden kann.

1. Suchen und öffnen Sie in **Projektmappen-Explorer**im Ordner " *Logic* " die Datei *PayPalFunctions.cs* .
2. Aktualisieren Sie die `HttpCall`-Methode, sodass der Code wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Der obige Code Ruft die `LogException`-Methode auf, die in der `ExceptionUtility`-Klasse enthalten ist. Sie haben die *ExceptionUtility.cs* -Klassendatei zuvor in diesem Tutorial dem *Logik* Ordner hinzugefügt. Die `LogException`-Methode nimmt zwei Parameter an. Der erste Parameter ist das Ausnahme Objekt. Der zweite Parameter ist eine Zeichenfolge, die verwendet wird, um die Fehlerquelle zu erkennen.

### <a name="inspecting-the-error-logging-information"></a>Überprüfen der Fehler Protokollierungs Informationen

Wie bereits erwähnt, können Sie das-Fehlerprotokoll verwenden, um zu bestimmen, welche Fehler in der Anwendung zuerst korrigiert werden sollten. Natürlich werden nur Fehler aufgezeichnet, die in das Fehlerprotokoll eingeschlossen und geschrieben wurden.

1. Suchen Sie in **Projektmappen-Explorer**die Datei *ErrorLog. txt* im Ordner *App\_Data* , und öffnen Sie Sie.   
 Möglicherweise müssen Sie die Option "**alle Dateien anzeigen**" oder die Option "**Aktualisieren**" oben in **Projektmappen-Explorer** auswählen, um die Datei *ErrorLog. txt* anzuzeigen.
2. Überprüfen Sie das in Visual Studio angezeigte Fehlerprotokoll: 

    ![ASP.net-Fehlerbehandlung: ErrorLog. txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Sichere Fehlermeldungen

Beachten Sie, dass bei der Anzeige **von** Fehlermeldungen in der Anwendung keine Informationen ausgegeben werden sollten, die ein böswilliger Benutzer möglicherweise beim Angriff auf Ihre Anwendung hilfreich finden kann. Wenn Ihre Anwendung z. b. erfolglos versucht, in eine Datenbank zu schreiben, sollte keine Fehlermeldung angezeigt werden, die den verwendeten Benutzernamen enthält. Aus diesem Grund wird dem Benutzer eine generische Fehlermeldung in roter Farbe angezeigt. Alle zusätzlichen Fehlerdetails werden nur dem Entwickler auf dem lokalen Computer angezeigt.

## <a name="using-elmah"></a>Verwenden von ELMAH

ELMAH (Fehler Protokollierungs Module und Handler) ist eine Fehler Protokollierungsfunktion, die Sie als nuget-Paket in Ihre ASP.NET-Anwendung einbinden. ELMAH bietet die folgenden Funktionen:

- Protokollierung von nicht behandelten Ausnahmen.
- Eine Webseite, um das gesamte Protokoll der nicht behandelten Ausnahmen anzuzeigen.
- Eine Webseite zum Anzeigen der vollständigen Details der einzelnen protokollierten Ausnahmen.
- Eine e-Mail-Benachrichtigung zu den einzelnen Fehlern zu dem Zeitpunkt, zu dem dieser auftritt
- Ein RSS-Feed der letzten 15 Fehler aus dem Protokoll.

Bevor Sie mit dem ELMAH arbeiten können, müssen Sie es installieren. Dies ist einfach mit dem *nuget* -Paket-Installer. Wie bereits erwähnt, ist nuget eine Visual Studio-Erweiterung, die die Installation und Aktualisierung von Open Source-Bibliotheken und-Tools in Visual Studio erleichtert.

1. Wählen Sie in Visual Studio im **Menü Extras** die Option **nuget-Paket-Manager** > **nuget-Pakete für**Projekt Mappe verwalten aus. 

    ![ASP.net-Fehlerbehandlung-nuget-Pakete für Projekt Mappe verwalten](aspnet-error-handling/_static/image6.png)
2. Das Dialogfeld **nuget-Pakete verwalten** wird in Visual Studio angezeigt.
3. Erweitern Sie im Dialogfeld **nuget-Pakete verwalten** auf der linken Seite die Option **Online** , und wählen Sie dann **nuget.org**aus. Suchen und installieren Sie dann das **ELMAH** -Paket aus der Liste der verfügbaren Pakete online. 

    ![ASP.net-Fehlerbehandlung: Elma nuget-Paket](aspnet-error-handling/_static/image7.png)
4. Sie benötigen eine Internetverbindung, um das Paket herunterzuladen.
5. Stellen Sie im Dialogfeld **Projekte auswählen** sicher, dass die Auswahl **wingtiptoys** ausgewählt ist, und klicken Sie dann auf **OK**. 

    ![ASP.net-Fehlerbehandlung: Dialog Feld "Projekte auswählen"](aspnet-error-handling/_static/image8.png)
6. Klicken Sie bei Bedarf im Dialogfeld **nuget-Pakete verwalten** auf **Schließen** .
7. Wenn Visual Studio anfordert, dass Sie geöffnete Dateien erneut laden, wählen Sie "**Ja für alle**" aus.
8. Das ELMAH-Paket fügt Einträge für sich selbst in der Datei " *Web. config* " im Stammverzeichnis Ihres Projekts hinzu. Wenn Sie von Visual Studio gefragt werden, ob Sie die geänderte Datei *Web. config* erneut laden möchten, klicken Sie auf **Ja**.

ELMAH ist nun bereit, alle auftretenden Fehler zu speichern.

### <a name="viewing-the-elmah-log"></a>Anzeigen des ELMAH-Protokolls

Das Anzeigen des ELMAH-Protokolls ist einfach, aber zunächst wird eine nicht behandelte Ausnahme erstellt, die im ELMAH-Protokoll aufgezeichnet wird.

1. Drücken Sie **STRG + F5** , um die Beispielanwendung Wingtip Toys auszuführen.
2. Um eine unbehandelte Ausnahme in das ELMAH-Protokoll zu schreiben, navigieren Sie in Ihrem Browser zur folgenden URL (unter Verwendung ihrer Portnummer):  
    `https://localhost:44300/NoPage.aspx` wird die Fehlerseite angezeigt.
3. Um das ELMAH-Protokoll anzuzeigen, navigieren Sie in Ihrem Browser zur folgenden URL (unter Verwendung ihrer Portnummer):  
    `https://localhost:44300/elmah.axd`

    ![ASP.net-Fehlerbehandlung-ELMAH-Fehlerprotokoll](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie Fehler auf Anwendungsebene, auf Seitenebene und auf Codeebene behandeln. Sie haben auch gelernt, wie Sie behandelte und nicht behandelte Fehler zur späteren Überprüfung protokollieren. Sie haben das Dienstprogramm "ELMAH" hinzugefügt, um eine Ausnahme Protokollierung und Benachrichtigung an Ihre Anwendung mithilfe von nuget zu Darüber hinaus haben Sie die Wichtigkeit sicherer Fehlermeldungen kennengelernt.

## <a name="tutorial-series-conclusion"></a>Abschluss des Tutorials

Vielen Dank für die folgenden Punkte. Ich hoffe, dass diese Reihe von Tutorials Ihnen helfen, sich mit ASP.net Web Forms vertraut zu machen. Weitere Informationen zu Web Forms Features, die in ASP.NET 4,5 und Visual Studio 2013 verfügbar sind, finden Sie unter [ASP.net and Web Tools Visual Studio 2013 Anmerkungen zu dieser Version](../../../../visual-studio/overview/2013/release-notes.md). Sehen Sie sich auch das Tutorial an, das im Abschnitt " **Nächste Schritte** " erwähnt wurde, und testen Sie die [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

![Vielen Dank, Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Bereitstellen Ihrer Webanwendung in Microsoft Azure finden Sie unter Bereitstellen [einer Secure ASP.net Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)Website.

## <a name="free-trial"></a>Kostenlose Testversion

[Kostenlose Testversion Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)  
 Wenn Sie Ihre Website auf Microsoft Azure veröffentlichen, sparen Sie Zeit, Wartung und Kosten. Es ist ein schneller Vorgang zum Bereitstellen Ihrer Web-App in Azure. Wenn Sie Ihre Web-App verwalten und überwachen müssen, bietet Azure eine Reihe von Tools und Diensten. Verwalten von Daten, Datenverkehr, Identität, Sicherungen, Messaging, Medien und Leistung in Azure. Und all dies wird in einem sehr kostengünstigen Ansatz bereitgestellt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Protokollieren von Fehler Details mit der ASP.NET Health Monitoring](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Danksagung

Ich möchte den folgenden Personen danken, die bedeutende Beiträge zum Inhalt dieser tutorialreihe gemacht haben:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Niederlande](http://blog.alexthissen.nl/) (Twitter: [@alexthissen](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slowenien](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasilien](http://msmvps.com/blogs/bsonnino) (Twitter: [@bsonnino](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brasilien](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (Twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (Twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Michael Sharps, USA](http://www.930solutions.com/) (Twitter: [@mrsharps](http://twitter.com/mrsharps))
- Mike Papst
- [Mitchel Verkäufer, USA](http://www.mitchelsellers.com/) (Twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))
- [Paul cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Beiträge der Community

- Graham mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012-bezogenes Codebeispiel auf MSDN: [Navigation Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 Verwandte Codebeispiele auf MSDN: [ASP.NET 4,5 Web Forms-tutorialreihe in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo-Mitwirkender von Microsoft Technical Audience (Twitter: @driazevedo)  
  Visual Studio 2012-Übersetzung: [iniciando com ASP.net Web Forms 4,5-parte 1-Introduction ção e visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Previous](url-routing.md)
