---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Hinzufügen der Validierung zum Modell | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437277"
---
# <a name="adding-validation-to-the-model"></a>Hinzufügen der Überprüfung zum Modell

von [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt. Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.

In diesem Abschnitt implementieren wir die erforderliche Unterstützung zum Aktivieren der Eingabevalidierung in unserer Anwendung. Wir stellen sicher, dass der Daten Bank Inhalt immer korrekt ist, und stellen hilfreiche Fehlermeldungen für Endbenutzer bereit, wenn Sie versuchen, die Film Daten einzugeben, die ungültig sind. Wir beginnen mit dem Hinzufügen einer kleinen Validierungs Logik zur Movie-Klasse.

Klicken Sie mit der rechten Maustaste auf den Modell Ordner und wählen Sie Klasse hinzufügen Benennen Sie Ihre Class Movie.

Als wir das Movie-Entitäts Modell zuvor erstellt haben, hat die IDE eine Movie-Klasse erstellt. Tatsächlich kann sich ein Teil der Movie-Klasse in einer Datei befinden und Teil eines anderen sein. Dies wird als partielle Klasse bezeichnet. Wir erweitern die Movie-Klasse aus einer anderen Datei.

Wir erstellen eine partielle Movie-Klasse, die auf eine "Buddy-Klasse" zeigt, mit einigen Attributen, die Validierungs Hinweise an das System übergeben. Wir markieren den Titel und den Preis als erforderlich und setzen auch darauf, dass sich der Preis innerhalb eines bestimmten Bereichs liegt. Klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie Klasse Benennen Sie die Klasse Movie, und klicken Sie auf die Schaltfläche OK. So sieht unsere partielle Movie-Klasse aus.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Führen Sie die Anwendung erneut aus, und versuchen Sie, einen Film mit einem Preis über 100 einzugeben. Nachdem Sie das Formular übermittelt haben, erhalten Sie einen Fehler. Der Fehler wird auf Serverseite abgefangen und tritt auf, nachdem das Formular gepostet wurde. Beachten Sie, dass die integrierten HTML-Hilfsprogramme von ASP.NET MVC intelligent genug waren, um die Fehlermeldung anzuzeigen und die Werte für uns innerhalb der Textfeld-Elemente beizubehalten:

[!["kreatemoviewithvalidation"](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Dies funktioniert hervorragend, aber es wäre schön, wenn wir den Benutzer sofort über die Clientseite informieren könnten, bevor der Server eingebunden wird.

Wir ermöglichen eine Client seitige Validierung mit JavaScript.

## <a name="adding-client-side-validation"></a>Hinzufügen der Client seitigen Validierung

Da unsere Movie-Klasse bereits über einige Validierungs Attribute verfügt, müssen wir nur einige JavaScript-Dateien zur CREATE. aspx-Ansichts Vorlage hinzufügen und eine Codezeile hinzufügen, um die Client seitige Validierung zu aktivieren.

Wechseln Sie in VWD in den Ordner views/Movie, und öffnen Sie Create. aspx.

Öffnen Sie den Ordner Scripts in der Projektmappen-Explorer, und ziehen Sie die folgenden drei Skripts in das &lt;Head&gt;-Tags.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Sie möchten, dass diese Skriptdateien in dieser Reihenfolge angezeigt werden.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Fügen Sie außerdem diese einzelne Zeile oberhalb von HTML. BeginForm hinzu:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Hier sehen Sie den Code, der in der IDE angezeigt wird.

[![Filme-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Führen Sie die Anwendung aus, und klicken Sie auf/Movies/Create, und klicken Sie auf erstellen, ohne Daten einzugeben. Die Fehlermeldungen werden sofort ohne den seitenflash angezeigt, den wir mit dem Senden von Daten an den Server verknüpfen. Dies liegt daran, dass ASP.NET MVC nun die Eingabe sowohl auf dem Client (mit JavaScript) als auch auf dem Server überprüft.

[![erstellen-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Das sieht gut aus! Nun fügen wir der Datenbank eine zusätzliche Spalte hinzu.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part6.md)
> [Weiter](getting-started-with-mvc-part8.md)
