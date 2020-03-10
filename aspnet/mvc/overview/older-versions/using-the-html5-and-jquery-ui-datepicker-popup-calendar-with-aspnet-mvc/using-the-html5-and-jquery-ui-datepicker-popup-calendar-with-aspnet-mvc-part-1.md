---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 1 | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.net MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: c1c2380f24c72f6aabaaacaf975e95288a384ff1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433287"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 1

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.NET MVC-Webanwendung.

Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery [UI DatePicker-Popup Kalenders](http://plugins.jquery.com/project/datepicker) in einer ASP.NET MVC-Webanwendung. In diesem Tutorial können Sie Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;) verwenden, bei dem es sich um eine kostenlose Version von Microsoft Visual Studio handelt, oder Sie können Visual Studio 2010 SP1 verwenden, wenn Sie das bereits haben.

Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderliche Software einzeln mithilfe der folgenden Links installieren:

- [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)

Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

In diesem Tutorial wird davon ausgegangen, dass Sie das Tutorial für die ersten Schritte [mit MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) abgeschlossen haben oder dass Sie mit der ASP.NET MVC-Entwicklung vertraut sind. Dieses Tutorial beginnt mit dem abgeschlossenen Projekt aus dem Tutorial zu den ersten Schritten [mit MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) .

In diesem Tutorial wird Code C#in gezeigt. Das Starter- [Projekt](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) und das abgeschlossene Projekt sind aber auch in Visual Basic verfügbar.

Ein Visual Studio-Projekt C# mit und Visual Basic Quellcode ist für dieses Thema verfügbar: [Download](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Sie lernen Folgendes

Sie fügen Vorlagen (speziell zum Bearbeiten und Anzeigen von Vorlagen) der einfachen Movie-Listing-Anwendung hinzu, die im Tutorial " [Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " erstellt wurde. Außerdem fügen Sie einen [jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) -Popup Kalender hinzu, um den Prozess der Eingabe von Datumsangaben zu vereinfachen. Der folgende Screenshot zeigt die geänderte Anwendung mit dem angezeigten Popup Kalender jQuery UI DatePicker.

![jQuery abgeschlossen](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Erlernte Fertigkeiten

Folgendes können Sie lernen:

- Verwenden von Attributen aus dem [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace, um das Format der Daten zu steuern, wenn Sie angezeigt werden, und wenn Sie sich im Bearbeitungsmodus befinden.
- Erstellen von Vorlagen (Bearbeiten und Anzeigen von Vorlagen) zum Steuern der Formatierung von Daten
- Vorgehensweise beim Hinzufügen von [jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) als Möglichkeit zum Eingeben von Datumsfeldern.

### <a name="getting-started"></a>Erste Schritte

Wenn Sie die Movie-Listing-Anwendung nicht bereits aus dem Starter-Projekt haben, laden Sie Sie herunter: 

* [Herunterladen](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).
* Klicken Sie in Windows-Explorer mit der rechten Maustaste auf die Datei *mvcmovie. zip* , und wählen Sie **Eigenschaften**aus. 
* Wählen Sie im Dialogfeld **Eigenschaften von mvcmovie. zip** die Option **Sperre**Entsperren aus. (Durch diese Option wird eine Sicherheitswarnung verhindert, die angezeigt wird, wenn Sie versuchen, eine *.zip* -Datei zu verwenden, die Sie aus dem Internet heruntergeladen haben.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Klicken Sie mit der rechten Maustaste auf die Datei *mvcmovie. zip* , und wählen Sie **Alle extrahieren** , um die Datei zu entpacken. Öffnen Sie in Visual Web Developer oder Visual Studio 2010 die Datei *mvcmoviecs\_TU. sln* .

Doppelklicken Sie in **Projektmappen-Explorer**auf die Datei *views\shared\\_Layout. cshtml* , um Sie zu öffnen. Ändern Sie den `H1`-Header von **MVC Movie App** in **Movie jQuery**. Drücken Sie STRG + F5, um die Anwendung auszuführen, und klicken Sie auf die Registerkarte **Start** , auf der Sie zur `Index`-Methode des Movie-Controllers gelangen. Wenn Sie die Anwendung testen möchten, wählen Sie den Link **Bearbeiten** und den Link **Details** für einen der Filme aus. Beachten Sie, dass in den Ansichten "index", "Bearbeiten" und "Details" das Veröffentlichungsdatum und der Preis gut formatiert sind:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Die Formatierung für das Datum und den Preis ist das Ergebnis der Verwendung des [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attributs für Eigenschaften der `Movie`-Klasse.

Öffnen Sie die Datei *Movie.cs* , und kommentieren Sie das `DisplayFormat`-Attribut in den Eigenschaften `ReleaseDate` und `Price` aus. Die resultierende `Movie` Klasse sieht wie folgt aus:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Drücken Sie erneut STRG + F5, um die Anwendung auszuführen, und klicken Sie auf die Registerkarte **Start** , um die Liste der Filme anzuzeigen. Dieses Mal zeigt das Veröffentlichungsdatum das Datum und die Uhrzeit an, und im Feld "Price" wird das Währungssymbol nicht mehr angezeigt. Ihre Änderung in der `Movie` Klasse hat die schöne Formatierung rückgängig gemacht, die Sie zuvor gesehen haben. Sie werden dies jedoch in einem Moment beheben.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Verwenden des DataType-Attributs "DataAnnotations" zum Angeben des Datentyps

Ersetzen Sie das auskommentiertes `DisplayFormat` Attribut für die `ReleaseDate`-Eigenschaft durch das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut, indem Sie die `Date`-Enumeration verwenden. Ersetzen Sie das `DisplayFormat`-Attribut für die `Price`-Eigenschaft erneut durch das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut, diesmal mithilfe der `Currency`-Enumeration. Der abgeschlossene Code sieht wie folgt aus:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Führen Sie die Anwendung aus. Jetzt sind das Veröffentlichungsdatum und die Preis Eigenschaften ordnungsgemäß formatiert (d. h., es werden die entsprechenden Datums-und Währungsformate verwendet). Das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut stellt Typmetadaten für die integrierten ASP.NET MVC-Vorlagen bereit, sodass die Felder im richtigen Format gerenbt werden. Die Verwendung des `DataType`-Attributs ist der Verwendung des `DisplayFormat` Attributs vorzuziehen, das ursprünglich im Code verwendet wurde, da das `DataType`-Attribut das Modell sauberer und flexibler für Zwecke wie die Internationalisierung ist.

Im nächsten Abschnitt erfahren Sie, wie Sie benutzerdefinierte Vorlagen erstellen, um Datumsfelder anzuzeigen.

> [!div class="step-by-step"]
> [Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
