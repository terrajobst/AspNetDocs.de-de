---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Teil 1: Datei > Neues Projekt | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 1 umfasst Übersicht und Datei/Neues Projekt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516537"
---
# <a name="part-1-file--new-project"></a>Teil 1: Datei > Neues Projekt

von [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen. Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 1 umfasst Übersicht und Datei/Neues Projekt.

## <a id="_Toc260221666"></a>Übersicht über

Dieses Tutorial ist eine Einführung in ASP.net WebForms. Wir fangen langsam an, sodass die Webentwicklung für Einsteiger in Ordnung ist.

Die Anwendung, die wir aufbauen werden, ist ein einfacher Online-Speicher.

![](tailspin-spyworks-part-1/_static/image1.jpg)

Besucher können Produkte nach Kategorie durchsuchen:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Sie können ein einzelnes Produktanzeigen und zum Warenkorb hinzufügen:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Sie können Ihren Warenkorb überprüfen und alle Elemente entfernen, die Sie nicht mehr benötigen:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Beim Auschecken werden Sie zur Eingabe aufgefordert.

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Nach der Bestellung wird ein einfacher Bestätigungsbildschirm angezeigt:

![](tailspin-spyworks-part-1/_static/image7.jpg)

Wir beginnen mit der Erstellung eines neuen ASP.net WebForms-Projekts in Visual Studio 2010, und wir fügen inkrementell Features hinzu, um eine komplette funktionsfähige Anwendung zu erstellen. Dabei werden die Datenbankzugriffs-, Listen-und Raster Ansichten, Daten Aktualisierungs Seiten, Datenvalidierung, die Verwendung von Masterseiten für konsistentes Seitenlayout, AJAX, Validierung, Benutzer Mitgliedschaft und mehr behandelt.

Sie können Schritt für Schritt ausführen, oder Sie können die abgeschlossene Anwendung von [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/) herunterladen.

Sie können entweder Visual Studio 2010 oder den kostenlosen Visual Web Developer 2010 aus [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/)verwenden. Zum Erstellen der Anwendung können Sie entweder SQL Server oder die kostenlose SQL Server Express verwenden, um die Datenbank zu hosten.

## <a id="_Toc260221667"></a>Datei/Neues Projekt

Zunächst wählen wir das neue Projekt aus dem Menü "Datei" in Visual Studio aus. Dadurch wird das Dialogfeld "Neues Projekt" geöffnet.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Wählen Sie die Gruppe " C# Visual/Web-Vorlagen" auf der linken Seite aus, und wählen Sie dann die Vorlage "ASP.NET Webanwendung" in der mittleren Spalte aus. Benennen Sie Ihr Projekt tailspinspyworks, und klicken Sie auf die Schaltfläche OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Dadurch wird das Projekt erstellt. Werfen wir einen Blick auf die Ordner, die in der Anwendung in der Projektmappen-Explorer auf der rechten Seite enthalten sind.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Die leere Projekt Mappe ist nicht vollständig leer – Sie fügt eine grundlegende Ordnerstruktur hinzu:

![](tailspin-spyworks-part-1/_static/image1.png)

Beachten Sie die Konventionen, die von der Standard Projektvorlage ASP.NET 4 implementiert werden.

- Der Ordner "Account" implementiert eine einfache Benutzeroberfläche für ASP. .Net-Mitgliedschafts Subsystem.
- Der Ordner "Scripts" dient als Repository für Client seitige JavaScript-Dateien, und die jQuery. js-Kerndateien werden standardmäßig zur Verfügung gestellt.
- Der Ordner "Styles" wird verwendet, um die Visualisierungen unserer Websites (CSS-Stylesheets) zu organisieren.

Wenn Sie F5 drücken, um die Anwendung auszuführen und die Seite "default. aspx" zu reneln, wird Folgendes angezeigt:

![](tailspin-spyworks-part-1/_static/image11.jpg)

Unsere erste Anwendungs Erweiterung besteht darin, die Datei "Style. CSS" aus der standardmäßigen WebForms-Vorlage durch die CSS-Klassen und die zugehörigen Bilddateien zu ersetzen, die die visuelle Asthetik für unsere Tailspin SpyWorks-Anwendung darstellen.

Danach wird unsere default. aspx-Seite wie folgt gerendert.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Beachten Sie die Bild links oben rechts auf der Seite und die Menü Elemente, die der Master Seite hinzugefügt wurden. Nur die Links "Anmelden" und "Konto" verweisen auf Seiten, die vorhanden (von der Standardvorlage generiert) und auf die restlichen Seiten, die wir beim Erstellen der Anwendung implementieren werden.

Außerdem verschieben wir die Master Seite in das Verzeichnis "Stile". Obwohl dies nur eine bevorzugte Einstellung ist, ist es möglicherweise etwas einfacher, wenn wir die Anwendung in Zukunft "Skinable" machen.

Danach müssen wir die Verweise der Master Seite in allen ASPX-Dateien ändern, die von den standardmäßigen ASP.net WebForms-Seiten generiert werden.

> [!div class="step-by-step"]
> [Weiter](tailspin-spyworks-part-2.md)
