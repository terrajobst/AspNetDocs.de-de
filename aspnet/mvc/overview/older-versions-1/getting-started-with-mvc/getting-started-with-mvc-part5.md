---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Zugreifen auf die Daten des Modells von einem Controller | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437367"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Zugreifen auf Modelldaten anhand eines Controllers

von [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt. Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.

In diesem Abschnitt erstellen wir eine neue Klasse "moviescontroller" und schreiben Code, mit dem die Filmdaten abgerufen und mit einer Ansichts Vorlage wieder im Browser angezeigt werden.

Klicken Sie mit der rechten Maustaste auf den Ordner Controllers, und erstellen Sie einen neuen moviescontroller.

[Controller hinzufügen ![](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Dadurch wird eine neue Datei "MoviesController.cs" unterhalb unseres Ordners "\controllers" in unserem Projekt erstellt. Aktualisieren Sie den "" von "", um die Liste der Filme aus unserer neu aufgefüllten Datenbank abzurufen.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Wir führen eine LINQ-Abfrage aus, sodass nur Filme abgerufen werden, die nach dem Sommer von 1984 veröffentlicht wurden. Wir benötigen eine Ansichts Vorlage, um die Liste der Filme wiederherzustellen. Klicken Sie also mit der rechten Maustaste auf die Methode, und wählen Sie Ansicht hinzufügen aus, um Sie zu erstellen.

Im Dialogfeld Ansicht hinzufügen geben wir an, dass wir eine Liste&lt;Filme. Models. Movie&gt; an unsere Ansichts Vorlage übergeben. Im Gegensatz zu den vorherigen Zeiten haben wir das Dialogfeld Ansicht hinzufügen verwendet und eine "leere" Vorlage erstellt. in diesem Fall geben wir an, dass Visual Studio automatisch eine Ansichts Vorlage für uns mit einem Standard Inhalt erstellen soll. Hierzu wählen Sie im Dropdown Menü "Inhalt anzeigen" das Listenelement aus.

Beachten Sie, dass Sie, wenn Sie eine neue Klasse erstellt haben, die Anwendung kompilieren müssen, damit Sie im Dialog Feld "Ansicht hinzufügen" angezeigt wird.

![Ansicht hinzufügen](getting-started-with-mvc-part5/_static/image3.png)

Wenn Sie auf Hinzufügen klicken, generiert das System automatisch den Code für eine Ansicht für uns, in der die Liste der Filme angezeigt wird. Dies ist ein guter Zeitpunkt, die &lt;H2-&gt; Überschrift in "My Movie List" zu ändern, wie dies bereits zuvor in der Hallo Welt Ansicht der Fall war.

[![Filme-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Führen Sie die Anwendung aus, und besuchen Sie/Movies in der Adressleiste. Nun haben wir mit einer einfachen Abfrage innerhalb des Controllers Daten aus der Datenbank abgerufen und die Daten an eine Ansicht zurückgegeben, die über Filme weiß. Diese Ansicht durchläuft dann die Liste der Filme und erstellt eine Tabelle mit Daten für uns.

[![Movie List-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Mit dieser Anwendung werden die Funktionen "Bearbeiten", "Details" und "Löschen" nicht implementiert. Daher benötigen wir nicht die Standard Verknüpfungen, die von der Gerüst Vorlage für uns erstellt wurden. Öffnen Sie die Datei/Movies/Index.aspx, und entfernen Sie Sie.

Im folgenden finden Sie den Quellcode für das, was unsere aktualisierte Ansichts Vorlage aussehen sollte, nachdem wir diese Änderungen vorgenommen haben:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Es erstellt Links, die wir nicht benötigen, daher werden wir Sie für dieses Beispiel löschen. Wir behalten uns jedoch den neuen Link "neu erstellen" vor. Dies ist der nächste Schritt. So sieht unsere App aus, in der diese Spalte entfernt wurde.

[![Movie List-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Wir verfügen jetzt über eine einfache Auflistung unserer Filmdaten. Wenn wir jedoch auf den Link "Create New" (neu erstellen) klicken, wird eine Fehlermeldung angezeigt, da diese nicht verknüpft ist. Wir implementieren eine Create Action-Methode und ermöglichen einem Benutzer die Eingabe neuer Filme in der Datenbank.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part4.md)
> [Weiter](getting-started-with-mvc-part6.md)
