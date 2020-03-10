---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Verwenden von AJAX zum übermitteln dynamischer Updates | Microsoft-Dokumentation
author: microsoft
description: Schritt 10 implementiert die Unterstützung für angemeldete Benutzer, um Ihr Interesse an der Teilnahme an einem Dinner mit einem AJAX-basierten Ansatz zu übernehmen, der in das Dinner-Detail integriert ist...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486309"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Bereitstellen von dynamischen Updates mithilfe von AJAX

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 10 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> Schritt 10 implementiert die Unterstützung für angemeldete Benutzer, um Ihr Interesse an der Teilnahme an einem Dinner mit einem AJAX-basierten Ansatz zu übernehmen, der auf der Seite Dinner Details integriert ist.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>Nerddinner Step 10: AJAX-Aktivierung von RSVPs akzeptiert

Nun implementieren wir die Unterstützung für angemeldete Benutzer, um Ihr Interesse an der Teilnahme an Dinner zu übernehmen. Wir aktivieren dies mithilfe eines AJAX-basierten Ansatzes, der in die Dinner Details-Seite integriert ist.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Angeben, ob der Benutzer RSVP

Benutzer können die URL */Dinners/Details/[ID*] besuchen, um Details zu einem bestimmten Dinner anzuzeigen:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Die "Details ()"-Aktionsmethode wird wie folgt implementiert:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Der erste Schritt bei der Implementierung der RSVP-Unterstützung besteht darin, dem Dinner-Objekt (innerhalb der zuvor erstellten partiellen Dinner.cs partiellen Klasse) eine "isuserregistrierte (username)"-Hilfsmethode hinzuzufügen. Diese Hilfsmethode gibt "true" oder "false" zurück, je nachdem, ob der Benutzer zurzeit ein RSVP für das Dinner hat:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Wir können dann der Ansicht "Details. aspx" den folgenden Code hinzufügen, um eine entsprechende Meldung anzuzeigen, die angibt, ob der Benutzer für das Ereignis registriert ist oder nicht:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Und wenn ein Benutzer nun ein Dinner besucht, wird die Meldung angezeigt:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Und wenn Sie ein Dinner besuchen, werden Sie nicht für Sie registriert. die folgende Meldung wird angezeigt:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementieren der Register Action-Methode

Nun fügen wir die Funktionalität hinzu, die erforderlich ist, um Benutzern die RSVP für ein Dinner auf der Seite "Details" zu ermöglichen.

Um dies zu implementieren, erstellen Sie eine neue Klasse "rsvpcontroller", indem Sie mit der rechten Maustaste auf das Verzeichnis "\controllers" klicken und den Menübefehl Add-&gt;Controller auswählen.

Wir implementieren in der neuen rsvpcontroller-Klasse eine "Register"-Aktionsmethode, die eine ID für ein Dinner als Argument annimmt, das entsprechende Dinner-Objekt abruft, prüft, ob der angemeldete Benutzer aktuell in der Liste der Benutzer enthalten ist, die sich registriert haben, und wenn Fügt kein RSVP-Objekt für Sie hinzu:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Beachten Sie, dass eine einfache Zeichenfolge als Ausgabe der Aktionsmethode zurückgegeben wird. Wir könnten diese Nachricht in eine Ansichts Vorlage eingebettet haben – aber da Sie so klein ist, verwenden wir einfach die Content ()-Hilfsmethode für die Controller-Basisklasse und geben eine Zeichen folgen Nachricht wie oben zurück.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Aufrufen der rsvpforevent-Aktionsmethode mithilfe von AJAX

Wir verwenden AJAX zum Aufrufen der Register Action-Methode aus der Detailansicht. Diese Implementierung ist ziemlich einfach. Zuerst fügen wir zwei Skript Bibliotheks Verweise hinzu:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

Die erste Bibliothek verweist auf die Client seitige Core ASP.NET AJAX-Skript Bibliothek. Diese Datei ist ungefähr rund um die Uhr groß (komprimiert) und enthält die Client seitigen Kernfunktionen von AJAX. Die zweite Bibliothek enthält Hilfsprogrammfunktionen, die in die integrierten AJAX-Hilfsmethoden von ASP.NET MVC integriert werden (die wir in Kürze verwenden werden).

Wir können dann den zuvor hinzugefügten Ansichts Vorlagen Code aktualisieren, sodass anstelle der Meldung "Sie sind nicht für dieses Ereignis registriert" einen Link zu erstellen, der bei einem Pushvorgang einen AJAX-Aufruf ausführt, der unsere rsvpforevent-Aktionsmethode auf unserem RSVP-Controller aufruft. und RSVPs:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Die oben verwendete AJAX. Action Link ()-Hilfsmethode ist in ASP.NET MVC integriert und ähnelt der "HTML. Action Link ()"-Hilfsmethode, mit der Ausnahme, dass anstelle einer Standard Navigation ein AJAX-Aufrufe an die Aktionsmethode erfolgt, wenn auf den Link geklickt wird. Oben rufen wir die "Register"-Aktionsmethode auf dem "RSVP"-Controller auf und übergeben die dinnerid als "ID"-Parameter an ihn. Der endgültige Parameter "ajaxoptions", den wir übergeben, gibt an, dass der von der Aktionsmethode zurückgegebene Inhalt übernommen und das HTML-&lt;div&gt;-Element auf der Seite aktualisiert werden soll, deren ID "rsvpmsg" lautet.

Wenn ein Benutzer nun zu einem Dinner sucht, ist er noch nicht registriert, und es wird ein Link zur RSVP dafür angezeigt:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Wenn Sie auf den Link "RSVP für dieses Ereignis" klicken, wird ein AJAX-Rückruf der Register Action-Methode auf dem RSVP-Controller ausgeführt, und wenn Sie abgeschlossen ist, wird eine aktualisierte Meldung wie die folgende angezeigt:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Die Netzwerkbandbreite und der Datenverkehr bei der Erstellung dieses AJAX-Aufrufes ist wirklich einfach. Wenn der Benutzer auf den Link "RSVP für dieses Ereignis" klickt, wird eine kleine HTTP Post-Netzwerk Anforderung an die */Dinners/Register/1* -URL gesendet, die wie unten gezeigt aussieht:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Die Antwort von der Register Aktionsmethode ist einfach:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Dieser Lightweight-Rückruf ist schnell und funktioniert sogar über ein langsames Netzwerk.

### <a name="adding-a-jquery-animation"></a>Hinzufügen einer jQuery-Animation

Die von uns implementierte AJAX-Funktionalität funktioniert gut und schnell. Manchmal kann es so schnell vorkommen, dass ein Benutzer möglicherweise nicht bemerkt, dass der RSVP-Link durch neuen Text ersetzt wurde. Um das Ergebnis etwas deutlicher zu machen, können wir eine einfache Animation hinzufügen, um auf die Aktualisierungs Nachricht aufmerksam zu machen.

Die standardmäßige ASP.NET-MVC-Projektvorlage enthält jQuery – eine ausgezeichnete (und sehr beliebte) Open Source-JavaScript-Bibliothek, die auch von Microsoft unterstützt wird. jQuery bietet eine Reihe von Features, einschließlich einer netten HTML-DOM-Auswahl-und-Effekte-Bibliothek.

Zur Verwendung von jQuery fügen wir zuerst einen Skript Verweis hinzu. Da wir jQuery innerhalb verschiedener Orte innerhalb unserer Website verwenden werden, fügen wir den Skript Verweis innerhalb unserer Website-Masterseiten Datei hinzu, damit Sie von allen Seiten verwendet werden kann.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Tipp: Stellen Sie sicher, dass Sie den JavaScript IntelliSense-Hotfix für VS 2008 SP1 installiert haben, der eine umfassendere IntelliSense-Unterstützung für JavaScript-Dateien (einschließlich jQuery) ermöglicht. Sie können Sie von: http://tinyurl.com/vs2008javascripthotfix*

Mit jQuery geschriebener Code verwendet häufig eine globale "$ ()"-JavaScript-Methode, die ein oder mehrere HTML-Elemente mithilfe eines CSS-Selektor abruft. Beispielsweise wählt *$ ("#rsvpmsg")* alle HTML-Elemente mit der ID rsvpmsg aus, während *$ (". Something")* alle Elemente mit dem CSS-Klassennamen "etwas" auswählen würde. Sie können auch erweiterte Abfragen wie "alle aktivierten Options Felder zurückgeben" mit einer Auswahl Abfrage schreiben, z. b.: *$ ("input [@type= Radio] [@checked]")* .

Nachdem Sie Elemente ausgewählt haben, können Sie Methoden für diese Methoden zum Ausführen von Aktionen (z. b. ausblenden) aufzurufen: *$ ("#rsvpmsg"). Hide ();*

In unserem RSVP-Szenario definieren wir eine einfache JavaScript-Funktion mit dem Namen "animatersvpmessage", die den &lt;div-&gt; "rsvpmsg" auswählt und die Größe des Text Inhalts animiert. Der folgende Code startet den Text "Small" und bewirkt dann, dass er sich über einen Zeitraum von 400 Millisekunden vergrößert:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Anschließend können Sie diese JavaScript-Funktion nach erfolgreichem Abschluss des AJAX-Aufrufs verbinden, indem Sie Ihren Namen an unsere AJAX. Action Link ()-Hilfsmethode übergeben (über die "onSuccess"-Ereignis Eigenschaft "ajaxoptions"):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Wenn Sie nun auf den Link "RSVP für diesen Ereignis" klicken und der AJAX-Befehl erfolgreich abgeschlossen wird, wird die zurück gesendete Inhalts Nachricht animiert und vergrößert:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Zusätzlich zur Bereitstellung eines onSuccess-Ereignisses macht das ajaxoptions-Objekt OnBegin-, OnFailure-und OnComplete-Ereignisse verfügbar, die Sie behandeln können (zusammen mit einer Reihe von anderen Eigenschaften und nützlichen Optionen).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Bereinigen: umgestalten einer RSVP-Teilansicht

Die Vorlage für die Detailansicht beginnt mit einem gewissen Zeitraum, und die Überstunden machen es etwas schwerer zu verstehen. Um die Lesbarkeit des Codes zu verbessern, erstellen Sie zunächst eine Teilansicht – rsvpstatus. ascx –, die den gesamten RSVP-Ansichts Code für unsere Detailseite Kapseln.

Klicken Sie hierzu mit der rechten Maustaste auf den Ordner "\views\dinners", und wählen Sie dann den Menübefehl Add-&gt;View aus. Wir haben ein Dinner-Objekt als stark typisiertes ViewModel. Anschließend können wir den RSVP-Inhalt aus der Ansicht "Details. aspx" Kopieren und einfügen.

Anschließend erstellen wir eine weitere Teilansicht – editanddelta etelinert. ascx, die den Code für den Link zum Bearbeiten und Löschen der Verknüpfung kapselt. Außerdem wird ein Dinner-Objekt als stark typisiertes ViewModel übernommen, und die Bearbeitungs-und Lösch Logik wird aus der Ansicht "Details. aspx" kopiert und eingefügt.

Die Vorlage für die Detailansicht kann dann einfach zwei HTML. renderpartial ()-Methodenaufrufe im unteren Bereich einschließen:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Dadurch wird der Code sauberer zu lesen und zu warten.

### <a name="next-step"></a>Nächster Schritt

Sehen wir uns nun an, wie wir Ajax noch weiter verwenden und der Anwendung interaktive zuordnungsunterstützung hinzufügen können.

> [!div class="step-by-step"]
> [Zurück](secure-applications-using-authentication-and-authorization.md)
> [Weiter](use-ajax-to-implement-mapping-scenarios.md)
