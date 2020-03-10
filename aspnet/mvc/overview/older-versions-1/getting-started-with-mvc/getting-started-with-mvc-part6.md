---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Hinzufügen einer Create-Methode und CREATE VIEW | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 05a281720f76b107fe8d902ef60d5d2e72af3ef5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437289"
---
# <a name="adding-a-create-method-and-create-view"></a>Hinzufügen einer Create-Methode und -Ansicht

von [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt. Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.

In diesem Abschnitt implementieren wir die erforderliche Unterstützung, damit Benutzer neue Filme in unserer Datenbank erstellen können. Dies wird durch Implementieren der/Movies/Create-URL-Aktion durchführen.

Das Implementieren der/Movies/Create-URL ist ein zweistufiger Prozess. Wenn ein Benutzer die/Movies/Create-URL zum ersten Mal besucht, möchten wir Ihnen ein HTML-Formular anzeigen, das Sie ausfüllen können, um einen neuen Film einzugeben. Wenn der Benutzer dann das Formular sendet und die Daten an den Server zurücksendet, möchten wir die übermittelten Inhalte abrufen und in der Datenbank speichern.

Wir implementieren diese beiden Schritte innerhalb von zwei Create ()-Methoden innerhalb der Klasse "moviescontroller". Eine Methode zeigt das &lt;Formular&gt; an, das der Benutzer ausfüllen sollte, um einen neuen Film zu erstellen. Die zweite Methode verarbeitet die Verarbeitung der gesendeten Daten, wenn der Benutzer das &lt;Formular&gt; an den Server zurücksendet und einen neuen Film in der Datenbank speichert.

Im folgenden finden Sie den Code, den wir zur Klasse "moviescontroller" hinzufügen, um dies zu implementieren:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Der obige Code enthält den gesamten Code, den wir in unserem Controller benötigen.

Nun implementieren wir die CREATE VIEW-Vorlage, mit der Sie dem Benutzer ein Formular anzeigen. Klicken Sie mit der rechten Maustaste auf die erste Create-Methode, und wählen Sie "Add View" (Ansicht hinzufügen) aus, um die Ansichts Vorlage für das

Wir legen fest, dass die Ansichts Vorlage "Movie" als Ansichts Datenklasse übergeben wird, und geben an, dass wir eine "Create"-Vorlage erstellen möchten.

[![Ansicht hinzufügen](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Nachdem Sie auf die Schaltfläche Hinzufügen geklickt haben, wird die Vorlage \movies\ansichts Vorlage für Sie erstellt. Da wir in der Dropdown Liste "Inhalt anzeigen" die Option "erstellen" ausgewählt haben, wird im Dialogfeld "Ansicht hinzufügen" automatisch ein Standard Inhalt für uns "gerügt" angezeigt. Der Gerüstbau hat ein HTML-&lt;Formular&gt;erstellt, einen Speicherort für Validierungs Fehlermeldungen, und da der Gerüstbau über Filme weiß, hat er Bezeichnung und Felder für jede Eigenschaft der Klasse erstellt.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Da unsere Datenbank automatisch eine ID erhält, entfernen wir nun die Felder, die auf das Modell verweisen. ID aus unserer Create-Sicht. Entfernen Sie die 7 Zeilen nach &lt;Legenden&gt;Felder&lt;/Legend&gt;, da Sie das gewünschte ID-Feld anzeigen.

Nun erstellen wir einen neuen Film und fügen ihn der Datenbank hinzu. Dies geschieht, indem Sie die Anwendung erneut ausführen und die URL "/Movies" aufrufen und auf den Link "erstellen" klicken, um einen neuen Film hinzuzufügen.

[![erstellen-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Wenn Sie auf die Schaltfläche "erstellen" klicken, werden die Daten in diesem Formular (über HTTP Post) an die soeben erstellte/Movies/Create-Methode zurückgesendet. Ebenso wie wenn das System automatisch den Parameter "numtimes" und "Name" aus der URL herausgenommen hat und diese den Parametern einer Methode zuvor zugeordnet hat, übernimmt das System automatisch die Formularfelder aus einem Post und ordnet diese einem Objekt zu. In diesem Fall werden Werte aus Feldern in HTML, wie z. b. "ReleaseDate" und "Title", automatisch in die richtigen Eigenschaften einer neuen Instanz eines Films eingefügt.

Sehen wir uns erneut die zweite Create-Methode aus unserem "moviescontroller" an. Beachten Sie, wie es ein "Movie"-Objekt als Argument annimmt:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Dieses Movie-Objekt wurde dann an die [HttpPost]-Version der Create Action-Methode übergeben, und wir haben es in der Datenbank gespeichert und dann wieder an die Index ()-Aktionsmethode umgeleitet, die das gespeicherte Ergebnis in der Filmliste anzeigt:

[![Movie List-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Wir überprüfen nicht, ob die Filme richtig sind, und die Datenbank lässt nicht zu, dass wir einen Film ohne Titel speichern. Es wäre schön, wenn wir dem Benutzer mitteilen könnten, dass ein Fehler aufgetreten ist, bevor die Datenbank einen Fehler ausgelöst hat. Wir werden dies als nächstes durchführen, indem wir der Anwendung Validierungs Unterstützung hinzufügen.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part5.md)
> [Weiter](getting-started-with-mvc-part7.md)
