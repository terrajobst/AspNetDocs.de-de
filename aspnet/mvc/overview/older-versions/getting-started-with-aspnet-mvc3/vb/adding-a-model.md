---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Hinzufügen eines Modells (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434601"
---
# <a name="adding-a-model-vb"></a>Hinzufügen eines Modells (VB)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:
> 
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit VB.NET-Quellcode ist für dieses Thema verfügbar. [Laden Sie die VB.NET-Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wechseln Sie C#ggf. zur [ C# Version](../cs/adding-a-model.md) dieses Tutorials.

## <a name="adding-a-model"></a>Hinzufügen eines Modells

In diesem Abschnitt fügen Sie einige Klassen zum Verwalten von Filmen in einer-Datenbank hinzu. Diese Klassen werden als "Modell"-Teil der ASP.NET MVC-Anwendung verwendet.

Sie verwenden eine .NET Framework Datenzugriffs Technologie, die als Entity Framework bezeichnet wird, um diese Modellklassen zu definieren und mit Ihnen zu arbeiten. Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklungsparadigma namens *Code First*. Code First ermöglicht es Ihnen, Modell Objekte zu erstellen, indem einfache Klassen geschrieben werden. (Diese werden auch als poco-Klassen bezeichnet, von "Plain-Old CLR Objects".) Anschließend können Sie die Datenbank dynamisch von ihren Klassen erstellen lassen, wodurch ein sehr sauberer und schneller Entwicklungs Workflow ermöglicht wird.

## <a name="adding-model-classes"></a>Hinzufügen von Modellklassen

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie **Hinzufügen**und dann **Klasse**aus.

![](adding-a-model/_static/image1.png)

Benennen Sie die Klasse "Movie".

Fügen Sie der `Movie`-Klasse die folgenden fünf Eigenschaften hinzu:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Wir verwenden die `Movie`-Klasse, um Filme in einer Datenbank darzustellen. Jede Instanz eines `Movie` Objekts entspricht einer Zeile in einer Datenbanktabelle, und jede Eigenschaft der `Movie` Klasse wird einer Spalte in der Tabelle zugeordnet.

Fügen Sie in der gleichen Datei die folgende `MovieDBContext`-Klasse hinzu:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

Die `MovieDBContext`-Klasse stellt den Entity Framework Movie-Daten Bank Kontext dar, der das Abrufen, speichern und Aktualisieren von `Movie`-Klassen Instanzen in einer Datenbank übernimmt. Die `MovieDBContext` wird von der `DbContext` Basisklasse abgeleitet, die vom Entity Framework bereitgestellt wird. Weitere Informationen zu `DbContext` und `DbSet`finden Sie unter [Produktivitätsverbesserungen für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Um auf `DbContext` und `DbSet`verweisen zu können, müssen Sie am Anfang der Datei die folgende `imports`-Anweisung hinzufügen:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Die komplette Datei *Movie. vb* ist unten dargestellt.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Erstellen einer Verbindungs Zeichenfolge und arbeiten mit SQL Server Compact

Die von Ihnen erstellte `MovieDBContext`-Klasse übernimmt die Aufgabe der Verbindung mit der Datenbank und die Zuordnung von `Movie` Objekten zu Datenbankdaten Sätzen. Eine Frage ist jedoch, wie Sie angeben können, mit welcher Datenbank eine Verbindung hergestellt werden soll. Hierzu fügen Sie Verbindungsinformationen in der Datei " *Web. config* " der Anwendung hinzu.

Öffnen Sie die *Web. config* -Datei der Anwendung. (Nicht die Datei " *Web. config* " im Ordner " *views* ".) Die folgende Abbildung zeigt beide *Web. config* -Dateien. Öffnen Sie die Datei *Web. config* in rot.

![](adding-a-model/_static/image2.png)

Fügen Sie dem `<connectionStrings>`-Element in der Datei " *Web. config* " die folgende Verbindungs Zeichenfolge hinzu.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Das folgende Beispiel zeigt einen Teil der Datei " *Web. config* ", in dem die neue Verbindungs Zeichenfolge hinzugefügt wurde:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Diese kleine Menge an Code und XML ist alles, was Sie schreiben müssen, um die Filmdaten in einer Datenbank darzustellen und zu speichern.

Als Nächstes erstellen Sie eine neue `MoviesController`-Klasse, die Sie verwenden können, um die Filmdaten anzuzeigen und Benutzern das Erstellen neuer Film Auflistungen zu ermöglichen.

> [!div class="step-by-step"]
> [Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)
