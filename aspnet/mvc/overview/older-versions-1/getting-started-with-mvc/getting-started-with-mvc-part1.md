---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Einführung in ASP.NET MVC | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469797"
---
# <a name="intro-to-aspnet-mvc"></a>Einführung in ASP.NET MVC

von [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Eine aktualisierte Version, wenn dieses [Tutorial mithilfe von](../../getting-started/introduction/getting-started.md) [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)verfügbar ist. Im neuen Tutorial wird ASP.NET MVC 5 verwendet, das in diesem Tutorial viele Verbesserungen bietet.
>
>
> Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt. Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.

Machen wir unsere erste ASP.NET MVC-Webanwendung mit [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Wir erstellen eine kleine Movie List-Anwendung, mit der Filme erstellt und aufgelistet werden können.

## <a name="what-youll-build"></a>Sie lernen Folgendes

Im folgenden finden Sie zwei Screenshots der Anwendung, die Sie erstellen. Sie verfügen über eine einfache Tabelle mit Filmen mit verschiedenen Spalten.

[![Movie List-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Und Sie haben ein Formular erstellen, damit wir der Liste Filme hinzufügen können.

[![Erstellen eines Films: Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Erlernte Fertigkeiten

Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Visual Studio. Sie lernen Folgendes:

- Erstellen eines neuen ASP.NET MVC-Projekts
- Erstellen einer neuen Datenbank mit SQL Server
- Erstellen von ASP.NET-MVC-Controllern und-Ansichten
- Abrufen und Anzeigen von Daten
- Vorgehensweise beim Bearbeiten von Daten und Aktivieren der Datenvalidierung
- Aktualisieren des Datenbankschemas

## <a name="get-started"></a>Erste Schritte

Beginnen Sie mit der Ausführung von Visual Web Developer 2010 Express (ich nenne Sie jetzt "VWD"), und wählen Sie "Neues Projekt" auf dem Start Bildschirm aus.

Visual Web Developer ist eine IDE oder integrierte Entwicklerumgebung. Ebenso wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE zum Erstellen von Anwendungen. Es gibt eine Symbolleiste am oberen Rand der verschiedenen Optionen, die Ihnen zur Verfügung stehen, sowie das Menü, das Sie auch zum Auswählen der Datei verwenden können. Neues Projekt.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Anwendungen können mit Visual Basic oder Visual C#erstellt werden. Wählen Sie jetzt auf der C# linken Seite Visualisierung aus, und wählen Sie dann "ASP.NET MVC 2-Webanwendung" aus. Benennen Sie Ihr Projekt "Movies", und klicken Sie auf OK.

[Neues Projekt ![](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Auf der rechten Seite befindet sich die Projektmappen-Explorer, in der alle Dateien und Ordner in der Anwendung angezeigt werden. Im großen Fenster in der Mitte bearbeiten Sie den Code und verbringen die meiste Zeit. Visual Studio hat eine Standardvorlage für das ASP.NET MVC-Projekt verwendet, das Sie gerade erstellt haben, sodass Sie momentan über eine funktionierende Anwendung verfügen, ohne etwas zu tun! Dies ist eine einfache "Hallo Welt! Das Projekt ist ein guter Ausgangspunkt für die Anwendung.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Wählen Sie die Schaltfläche "Wiedergabe" auf der Symbolleiste aus.

![Debuggen](getting-started-with-mvc-part1/_static/image11.png)

Es handelt sich um einen grünen Pfeil, der auf das Recht zeigt, mit dem das Programm kompiliert und die Anwendung in einem Webbrowser gestartet wird.

*Hinweis: Sie können die Taste F5 auf der Tastatur drücken oder Debuggen&gt;Debuggen starten im Menü Debuggen auswählen.*

Dies bewirkt, dass Visual Web Developer einen Webserver für die Entwicklung startet und die Webanwendung ausgeführt wird (es sind keine Konfigurationsschritte oder manuelle Schritte erforderlich, um dies zu aktivieren). Anschließend wird ein Browser gestartet und zum Durchsuchen der Startseite der Anwendung konfiguriert. Beachten Sie, dass die Adressleiste des Browsers "localhost" und nicht wie "example.com" lautet. Der Grund hierfür ist, dass "localhost" immer auf Ihren eigenen lokalen Computer verweist. in diesem Fall wird die Anwendung ausgeführt, die wir soeben erstellt haben.

[![Startseite](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Diese Standardvorlage enthält standardmäßig zwei zu besuchende Seiten und eine einfache Anmeldeseite. Ändern Sie die Funktionsweise dieser Anwendung, und erfahren Sie etwas über ASP.NET MVC im Prozess. Schließen Sie Ihren Browser, und lassen Sie Code ändern.

> [!div class="step-by-step"]
> [Weiter](getting-started-with-mvc-part2.md)
