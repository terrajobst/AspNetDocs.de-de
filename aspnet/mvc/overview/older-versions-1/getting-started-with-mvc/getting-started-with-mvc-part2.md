---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Hinzufügen eines Controllers | Microsoft-Dokumentation
author: shanselman
description: Eine aktualisierte Version, wenn dieses Tutorial mithilfe von Visual Studio 2013 verfügbar ist. Im neuen Tutorial wird ASP.NET MVC 5 verwendet, das viele Verbesserungen gegenüber t bietet...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: e2a298584473f57c2b14edf507f0f6886d906ea3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437565"
---
# <a name="adding-a-controller"></a>Hinzufügen eines Controllers

von [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Eine aktualisierte Version, wenn dieses [Tutorial mithilfe von](../../getting-started/introduction/getting-started.md) [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)verfügbar ist. Im neuen Tutorial wird ASP.NET MVC 5 verwendet, das in diesem Tutorial viele Verbesserungen bietet.
>
>
> Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt. Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.

MVC steht für das Modell, die Ansicht und den Controller. MVC ist ein Muster für die Entwicklung von Anwendungen, bei denen jeder Teil eine andere Verantwortung hat als eine andere.

- Model: die Daten Ihrer Anwendung
- Sichten: die Vorlagen Dateien, die von Ihrer Anwendung verwendet werden, um HTML-Antworten dynamisch zu generieren.
- Controller: Klassen, die eingehende URL-Anforderungen an die Anwendung verarbeiten, Modelldaten abrufen und dann Ansichts Vorlagen angeben, die eine Antwort an den Client zurückgeben

Wir werden alle diese Konzepte in diesem Tutorial abdecken und Ihnen zeigen, wie Sie Sie zum Erstellen einer Anwendung verwenden können.

Erstellen Sie einen neuen Controller, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controllers klicken und Controller hinzufügen auswählen.

[![addcontrollerrightclick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Benennen Sie den neuen Controller "helloworldcontroller", und klicken Sie auf hinzufügen.

[Dialog ![Hinzufügen eines Controllers](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Beachten Sie in der Projektmappen-Explorer auf der rechten Seite, dass eine neue Datei für Sie erstellt wurde HelloWorldController.cs und diese Datei nun in der **IDE**geöffnet ist.

[![helloworldcontrollercode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Erstellen Sie zwei neue Methoden, die in der neuen öffentlichen Klasse helloworldcontroller wie folgt aussehen. Eine HTML-HTML-Zeichenfolge wird als Beispiel direkt aus dem Controller zurückgegeben.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Der Controller hat den Namen "helloworldcontroller", und die neue Methode wird als "index" bezeichnet. Führen Sie die Anwendung wie zuvor erneut aus (Klicken Sie auf die Wiedergabe Schaltfläche, oder drücken Sie F5, um dies zu tun). Nachdem der Browser gestartet wurde, ändern Sie den Pfad in der Adressleiste in `http://localhost:xx/HelloWorld`, wobei xx eine beliebige Nummer ist, die Ihr Computer ausgewählt hat. Der Browser sollte nun wie im folgenden Screenshot aussehen. In unserer obigen Methode haben wir eine Zeichenfolge zurückgegeben, die an eine Methode namens "Content" weitergegeben wurde. Wir haben gesagt, dass das System nur HTML zurückgibt.

ASP.NET MVC ruft in Abhängigkeit von der eingehenden URL verschiedene Controller Klassen (und verschiedene Aktionsmethoden) auf. Die von ASP.NET MVC verwendete standardmäßige Zuordnungslogik verwendet ein Format wie dieses, um zu steuern, welcher Code ausgeführt wird:

/[Controller]/[Aktionsname]/[Parameter]

Der erste Teil der URL bestimmt die auszuführende Controller Klasse. Daher wird/HelloWorld der helloworldcontroller-Klasse zugeordnet. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse, die ausgeführt werden soll. /HelloWorld/Index würde also bewirken, dass die Index ()-Methode der helloworldcontroller-Klasse ausgeführt wird. Beachten Sie, dass wir nur/HelloWorld aufrufen mussten und der Methoden Index impliziert wurde. Dies liegt daran, dass eine Methode namens "index" die Standardmethode ist, die auf einem Controller aufgerufen wird, wenn eine nicht explizit angegeben ist.

[![Dies ist meine Standardaktion.](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Sehen wir uns jetzt an `http://localhost:xx/HelloWorld/Welcome.` jetzt hat unsere Willkommens Methode ausgeführt und Ihre HTML-Zeichenfolge zurückgegeben.

Auch hier:/[Controller]/[Aktionsname]/[Parameter], sodass Controller HelloWorld ist, und willkommen ist die Methode in diesem Fall. Wir haben noch keine Parameter durchgeführt.

[![Dies ist die Willkommens Aktionsmethode.](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Ändern wir nun das Beispiel, damit wir einige Informationen aus der URL an den Controller übergeben können, z. b. wie folgt:/HelloWorld/Welcome? Name = Scott&amp;numtimes = 4. Ändern Sie die Willkommens Methode, sodass Sie zwei Parameter enthält, und aktualisieren Sie Sie wie unten beschrieben. Beachten Sie, dass die C# optionale Parameter Funktion verwendet wurde, um anzugeben, dass der Parameter "numtimes" den Standardwert "1" hat, wenn er nicht übergeben wird.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Führen Sie die Anwendung aus, und besuchen Sie `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` ändern Sie den Wert von Name und numtimes wie gewünscht. Das System hat automatisch die benannten Parameter aus der Abfrage Zeichenfolge in der Adressleiste den Parametern in der Methode zugeordnet.

In diesen Beispielen hat der Controller alle Aufgaben durchgeführt und HTML direkt zurückgegeben. Normalerweise möchten wir nicht, dass unsere Controller den HTML-Code direkt zurückgeben, da dies für Code sehr mühsam ist. Stattdessen verwenden wir in der Regel eine separate Ansichts Vorlagen Datei, um die HTML-Antwort zu generieren. Sehen wir uns an, wie wir dies tun können. Schließen Sie Ihren Browser, und kehren Sie zur IDE zurück.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part1.md)
> [Weiter](getting-started-with-mvc-part3.md)
