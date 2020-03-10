---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Einführung in das Lernprogramm zu nerddinner | Microsoft-Dokumentation
author: shanselman
description: Die beste Möglichkeit, ein neues Framework kennenzulernen, besteht darin, etwas mit diesem Framework zu erstellen. Dieses Tutorial führt Sie durch die Schritte zum Erstellen einer kleinen, aber abgeschlossenen Anwendung mithilfe von ASP.ne...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468933"
---
# <a name="introducing-the-nerddinner-tutorial"></a>Einführung zum NerdDinner-Tutorial

von [Scott Hanselman](https://github.com/shanselman)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Die beste Möglichkeit, ein neues Framework kennenzulernen, besteht darin, etwas mit diesem Framework zu erstellen. In diesem Tutorial wird Schritt für Schritt erläutert, wie Sie eine kleine, aber abgeschlossene Anwendung mit ASP.NET MVC 1 erstellen, und es werden einige der grundlegenden Konzepte dahinter vorgestellt.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-tutorial"></a>Nerddinner-Tutorial

Die beste Möglichkeit, ein neues Framework kennenzulernen, besteht darin, etwas mit diesem Framework zu erstellen. Dieses Tutorial führt Sie durch die Erstellung einer kleinen, aber abgeschlossenen Anwendung mithilfe von ASP.NET MVC und führt einige der grundlegenden Konzepte dahinter ein.

Die Anwendung, die wir erstellen, wird als "nerddinner" bezeichnet. Nerddinner bietet Benutzern eine einfache Möglichkeit, Abendessen online zu finden und zu organisieren:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

Mit "nerddinner" können registrierte Benutzer Abendessen erstellen, bearbeiten und löschen. Es erzwingt einen konsistenten Satz von Validierungs-und Geschäftsregeln in der gesamten Anwendung:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Besucher können eine AJAX-basierte Karte verwenden, um nach bevorstehenden Abendessen zu suchen, die in Ihrer Nähe gehalten werden:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Wenn Sie auf ein Dinner klicken, gelangen Sie zu einer Detailseite, auf der Sie mehr darüber erfahren können:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Wenn Sie an der Teilnahme an Dinner interessiert sind, können Sie sich auf der Website anmelden oder registrieren:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Sie können dann auf einen AJAX-basierten RSVP-Link klicken, um an dem Ereignis teilzunehmen:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementieren von nerddinner

Wir beginnen mit der "nerddinner"-Anwendung, indem wir den Befehl "File-&gt;New Project" in Visual Studio verwenden, um ein neues ASP.NET MVC-Projekt zu erstellen. Anschließend fügen wir Funktionen und Features inkrementell hinzu. Dabei werden die folgenden Themen behandelt:

1. [Erstellen eines neuen ASP.NET MVC-Projekts](create-a-new-aspnet-mvc-project.md)
2. [Erstellen einer Datenbank](create-a-database.md)
3. [Erstellen eines Modells mit Geschäftsregel Überprüfungen](build-a-model-with-business-rule-validations.md)
4. [Verwenden von Controllern und Ansichten zum Implementieren einer Auflistung/Details-Benutzeroberfläche](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [Bereitstellen von CRUD (Create, Read, Update, DELETE)-Unterstützung für Datenformular Einträge](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [Verwenden von ViewData und Implementieren von ViewModel-Klassen](use-viewdata-and-implement-viewmodel-classes.md)
7. [Wieder verwenden der Benutzeroberfläche mithilfe von Masterseiten und partialen](re-use-ui-using-master-pages-and-partials.md)
8. [So implementieren Sie effizientes Datenpaging](implement-efficient-data-paging.md)
9. [Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung](secure-applications-using-authentication-and-authorization.md)
10. [Verwenden von AJAX zum bereitzustellen dynamischer Updates](use-ajax-to-deliver-dynamic-updates.md)
11. [Verwenden von AJAX zum Implementieren von Mapping-Szenarios](use-ajax-to-implement-mapping-scenarios.md)
12. [Aktivieren von automatisierten Komponententests](enable-automated-unit-testing.md)

Sie können eine eigene Kopie von nerddinner von Grund auf erstellen, indem Sie jeden Schritt abschließen, den wir in diesem Kapitel Exemplarische Vorgehensweise ausführen. Alternativ können Sie eine vollständige Version des Quellcodes hier herunterladen: [nerddinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Sie können auch optional [eine kostenlose PDF-Version dieses Tutorials herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) , wenn Sie das Tutorial offline lesen möchten.

Sie können entweder Visual Studio 2008 oder den kostenlosen Visual Web Developer 2008 Express verwenden, um die Anwendung zu erstellen. Sie können entweder SQL Server oder den kostenlosen SQL Server Express für die Datenbank verwenden.

Sie können ASP.NET MVC, Visual Web Developer 2008 Express und SQL Server Express (alle kostenlos) mithilfe von V2 des [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) installieren.

### <a name="now-lets-get-started"></a>Fangen wir nun an...

Nun, da wir uns mit der Bedeutung von "nerddinner" abkennen, können wir uns ein Rollup für die Ärmel durchführen und Code

Wir beginnen mit der Verwendung von Datei&gt;neues Projekt in Visual Studio, um die "nerddinner"-Anwendung zu erstellen.

> [!div class="step-by-step"]
> [Weiter](create-a-new-aspnet-mvc-project.md)
