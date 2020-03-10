---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Einführung in das Debuggen von ASP.net Web Pages Websites (Razor) | Microsoft-Dokumentation
author: Rick-Anderson
description: Beim Debuggen werden Fehler in ihren Codepages gefunden und behoben. In diesem Kapitel werden einige Tools und Techniken vorgestellt, mit denen Sie Debuggen und Analysen durchführen können...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: ae7d871e56326610c043dc20fe6e0919e1b4ac89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506457"
---
# <a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Einführung in das Debuggen von ASP.net Web Pages Websites (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel werden verschiedene Möglichkeiten zum Debuggen von Seiten auf einer ASP.net Web Pages-Website (Razor) erläutert. Beim Debuggen werden Fehler in ihren Codepages gefunden und behoben.
>
> **Lernen Sie Folgendes:**
>
> - Vorgehensweise beim Anzeigen von Informationen zum Analysieren und Debuggen von Seiten.
> - Gewusst wie: Verwenden von Debugtools in Visual Studio
>
>
> Dies sind die ASP.NET-Funktionen, die im Artikel eingeführt wurden:
>
> - Das `ServerInfo`-Hilfsprogramm.
> - `ObjectInfo`-Hilfsprogramm.
>
>
> ## <a name="software-versions"></a>Software Versionen
>
>
> - ASP.net Web Pages (Razor) 3
> - Visual Studio 2013
>
>
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2. Sie können webmatrix 3 verwenden, der integrierte Debugger wird jedoch nicht unterstützt.

Ein wichtiger Aspekt bei der Behebung von Fehlern und Problemen in Ihrem Code besteht darin, Sie überhaupt zu vermeiden. Dies können Sie tun, indem Sie Abschnitte Ihres Codes einfügen, die wahrscheinlich Fehler in `try/catch` Blöcken verursachen. Weitere Informationen finden Sie im Abschnitt Behandeln von Fehlern in der [Einführung in die ASP.net-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890).

Das `ServerInfo`-Hilfsprogramm ist ein Diagnosetool, das Ihnen einen Überblick über die Webserver Umgebung bietet, die Ihre Seite hostet. Außerdem werden Informationen zu HTTP-Anforderungen angezeigt, die gesendet werden, wenn ein Browser die Seite anfordert. Das `ServerInfo`-Hilfsprogramm zeigt die aktuelle Benutzeridentität, den Typ des Browsers, von dem die Anforderung stammt, usw. an. Diese Art von Informationen kann Ihnen helfen, häufige Probleme zu beheben.

1. Erstellen Sie eine neue Webseite mit dem Namen *ServerInfo. cshtml*.
2. Fügen Sie am Ende der Seite direkt vor dem schließenden `</body>` Tag `@ServerInfo.GetHtml()`hinzu:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Sie können den `ServerInfo` Code an beliebiger Stelle auf der Seite hinzufügen. Wenn die Ausgabe jedoch am Ende hinzugefügt wird, wird Sie von Ihrem anderen Seiten Inhalt getrennt, wodurch Sie leichter lesbar ist.

    > [!NOTE]
    >
    > **Wichtig** Sie sollten jeglichen Diagnosecode von ihren Webseiten entfernen, bevor Sie Webseiten auf einen Produktionsserver verschieben. Dies gilt für die `ServerInfo`-Hilfsprogramm sowie für die anderen Diagnoseverfahren in diesem Artikel, die das Hinzufügen von Code zu einer Seite einschließen. Sie möchten nicht, dass Ihre Website Besucher Informationen über Ihren Servernamen, Benutzernamen, Pfade auf dem Server und ähnliche Details anzeigen, da diese Art von Informationen für Personen mit böswilliger Absicht nützlich sein kann.
3. Speichern Sie die Seite, und führen Sie Sie in einem Browser aus.

    ![Debuggen-1](introduction-to-debugging/_static/image1.jpg)

    Das `ServerInfo`-Hilfsprogramm zeigt vier Tabellen mit Informationen auf der Seite an:

   - Server Konfiguration. Dieser Abschnitt enthält Informationen zum hostingwebserver, einschließlich des Computer namens, der Version von ASP.net, die Sie ausführen, dem Domänen Namen und der Serverzeit.
   - ASP.NET-Server Variablen. Dieser Abschnitt enthält Details zu vielen http-Protokoll Details (http-Variablen genannt) und Werten, die Teil jeder Webseiten Anforderung sind.
   - Informationen zur HTTP-Laufzeit. Dieser Abschnitt enthält ausführliche Informationen dazu, wie die Version des Microsoft .NET Frameworks, unter dem Ihre Webseite ausgeführt wird, den Pfad, die Details zum Cache usw. enthält. (Wie Sie unter [Einführung in die ASP.net-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890)gelernt haben, werden ASP.net Web Pages mithilfe der Razor-Syntax auf der ASP.NET-Webserver Technologie von Microsoft erstellt, die selbst auf einer umfassenden Software Entwicklungsbibliothek erstellt wurde, die als .NET Framework bezeichnet wird.)
   - Umgebungsvariablen. Dieser Abschnitt enthält eine Liste aller lokalen Umgebungsvariablen und ihrer Werte auf dem Webserver.

     Eine vollständige Beschreibung aller Server-und Anforderungs Informationen würde den Rahmen dieses Artikels sprengen. Sie können jedoch sehen, dass das `ServerInfo`-Hilfsprogramm viele Diagnoseinformationen zurückgibt. Weitere Informationen zu den Werten, die `ServerInfo` zurückgibt, finden Sie auf der MSDN-Website unter [erkannte Umgebungsvariablen](https://technet.microsoft.com/library/dd560744(WS.10).aspx) auf der Microsoft TechNet-Website und in den [IIS-Server Variablen](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) .

## <a name="embedding-output-expressions-to-display-page-values"></a>Einbetten von Ausgabe Ausdrücken zum Anzeigen von Seiten Werten

Eine andere Möglichkeit, um zu sehen, was in Ihrem Code passiert, besteht darin, Ausgabe Ausdrücke in die Seite einzubetten. Wie Sie wissen, können Sie den Wert einer Variablen direkt ausgeben, indem Sie der Seite etwas wie `@myVariable` oder `@(subTotal * 12)` hinzufügen. Zum Debuggen können Sie diese Ausgabe Ausdrücke an strategischen Punkten in Ihrem Code platzieren. Dies ermöglicht es Ihnen, den Wert von Schlüsselvariablen oder das Ergebnis von Berechnungen anzuzeigen, wenn die Seite ausgeführt wird. Wenn Sie mit dem Debuggen fertig sind, können Sie die Ausdrücke entfernen oder Sie auskommentieren. Diese Prozedur veranschaulicht eine typische Methode, mit der eingebettete Ausdrücke zum Debuggen einer Seite verwendet werden.

1. Erstellen Sie eine neue webmatrix-Seite mit dem Namen *outputexpression. cshtml*.
2. Ersetzen Sie den Seiten Inhalt durch Folgendes:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    Im Beispiel wird eine `switch`-Anweisung verwendet, um den Wert der `weekday` Variablen zu überprüfen und dann eine andere Ausgabe Meldung anzuzeigen, je nachdem, an welchem Wochentag Sie sich befindet. Im Beispiel ändert sich der `if`-Block im ersten Codeblock willkürlich durch Hinzufügen eines Tags zum aktuellen Wochentagswert. Dies ist ein Fehler, der zu Illustrations Zwecken eingeführt wurde.
3. Speichern Sie die Seite, und führen Sie Sie in einem Browser aus.

    Die Seite zeigt die Meldung für den falschen Wochentag an. Der Tag der Woche, an dem es sich tatsächlich handelt, wird die Meldung für einen Tag später angezeigt. Obwohl Sie in diesem Fall wissen, warum die Nachricht deaktiviert ist (weil der Code absichtlich den falschen Tageswert festlegt), ist es in Wirklichkeit oft schwierig, herauszufinden, wo die Dinge im Code schief gehen. Zum Debuggen müssen Sie herausfinden, was mit dem Wert von schlüsselobjekten und Variablen, wie z. b. `weekday`, geschieht.
4. Fügen Sie Ausgabe Ausdrücke hinzu, indem Sie `@weekday` wie in den Kommentaren im Code angezeigt werden, einfügen. Diese Ausgabe Ausdrücke zeigen die Werte der Variablen an dieser Stelle in der Codeausführung an.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Speichern Sie die Seite in einem Browser, und führen Sie Sie aus.

    Die Seite zeigt den aktuellen Wochentag zuerst an, dann den aktualisierten Wochentag, der sich aus dem Hinzufügen eines Tages ergibt, und dann die resultierende Meldung aus der `switch`-Anweisung. Die Ausgabe der beiden Variablen Ausdrücke (`@weekday`) enthält keine Leerzeichen zwischen den Tagen, weil Sie der Ausgabe keine HTML-`<p>` Tags hinzugefügt haben. die Ausdrücke dienen nur zu Testzwecken.

    ![Debuggen-2](introduction-to-debugging/_static/image2.jpg)

    Nun können Sie sehen, wo der Fehler liegt. Wenn Sie die `weekday` Variable im Code zum ersten Mal anzeigen, wird der richtige Tag angezeigt. Wenn Sie die Datei zum zweiten Mal anzeigen, wird der Tag nach dem `if`-Block im Code um 1 deaktiviert. Sie wissen, dass zwischen der ersten und zweiten Darstellung der Wochentag-Variablen etwas passiert ist. Wenn dies ein echter Fehler wäre, könnte diese Art von Herangehensweise Ihnen helfen, den Speicherort des Codes einzugrenzen, der das Problem verursacht.
6. Korrigieren Sie den Code auf der Seite, indem Sie die beiden hinzugefügten Ausgabe Ausdrücke entfernen und den Code entfernen, der den Tag der Woche ändert. Der verbleibende, komplette Codeblock sieht wie im folgenden Beispiel aus:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Führen Sie die Seite in einem Browser aus. Dieses Mal sehen Sie, dass die richtige Meldung für den tatsächlichen Wochentag angezeigt wird.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Verwenden des objectinfo-Hilfsprogramms zum Anzeigen von Objekt Werten

Das `ObjectInfo`-Hilfsprogramm zeigt den Typ und den Wert der einzelnen Objekte an, die Sie an ihn übergeben. Sie können Sie verwenden, um den Wert von Variablen und Objekten in Ihrem Code anzuzeigen (wie im vorherigen Beispiel mit Ausgabe Ausdrücken), und Sie können die Datentyp Informationen über das Objekt sehen.

1. Öffnen Sie die Datei mit dem Namen *outputexpression. cshtml* , die Sie zuvor erstellt haben.
2. Ersetzen Sie den gesamten Code in der Seite durch den folgenden Codeblock:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Speichern Sie die Seite in einem Browser, und führen Sie Sie aus.

    ![Debuggen-4](introduction-to-debugging/_static/image3.jpg)

    In diesem Beispiel zeigt das `ObjectInfo`-Hilfsprogramm zwei Elemente an:

   - Der Typ. Für die erste Variable ist der Typ `DayOfWeek`. Für die zweite Variable ist der Typ `String`.
   - Der Wert. Da in diesem Fall der Wert der Begrüßungs Variablen bereits auf der Seite angezeigt wird, wird der Wert erneut angezeigt, wenn Sie die Variable an `ObjectInfo`übergeben.

     Bei komplexeren Objekten kann das `ObjectInfo`-Hilfsprogramm Weitere Informationen &#8212; anzeigen, um die Typen und Werte aller Eigenschaften eines Objekts anzuzeigen.

## <a name="using-debugging-tools-in-visual-studio"></a>Verwenden von Debugtools in Visual Studio

Verwenden Sie [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017), um eine umfassendere Debuggingfunktion zu erhalten. Mit Visual Studio können Sie in Ihrem Code einen Haltepunkt in der Zeile festlegen, die Sie überprüfen möchten.

![Haltepunkt festlegen](introduction-to-debugging/_static/image1.png)

Wenn Sie die Website testen, wird der ausführende Code am Breakpoint angehalten.

![Haltepunkt erreichen](introduction-to-debugging/_static/image2.png)

Sie können die aktuellen Werte der Variablen untersuchen und den Codezeilen Weise durchlaufen.

![siehe Werte](introduction-to-debugging/_static/image3.png)

Informationen zur Verwendung des integrierten Debuggers in Visual Studio zum Debuggen von ASP.net Razor Pages finden [Sie unter Programmieren von ASP.net Web Pages (Razor) mithilfe von Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Programmieren von ASP.net Web Pages (Razor) mithilfe von Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS-Server Variablen](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Erkannte Umgebungsvariablen](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
