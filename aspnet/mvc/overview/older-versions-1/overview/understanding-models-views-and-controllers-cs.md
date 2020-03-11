---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Grundlegendes zu Modellen, Ansichten und Controllern (C#) | Microsoft-Dokumentation
author: StephenWalther
description: Sie sind mit Modellen, Ansichten und Controllern verwechselt? In diesem Tutorial führt Stephen Walther Sie in die verschiedenen Teile einer ASP.NET-MVC-Anwendung ein.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 57dc82d02d38adc2514aa2c02c6f156ed0fb88a6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486087"
---
# <a name="understanding-models-views-and-controllers-c"></a>Grundlegendes zu Modellen, Ansichten und Controllern (C#)

von [Stephen Walther](https://github.com/StephenWalther)

> Sie sind mit Modellen, Ansichten und Controllern verwechselt? In diesem Tutorial führt Stephen Walther Sie in die verschiedenen Teile einer ASP.NET-MVC-Anwendung ein.

Dieses Tutorial bietet eine allgemeine Übersicht über ASP.NET MVC-Modelle,-Ansichten und-Controller. Anders ausgedrückt: Es werden das M ", V" und C "in ASP.NET MVC erläutert.

Nachdem Sie dieses Tutorial gelesen haben, sollten Sie wissen, wie die verschiedenen Teile einer ASP.NET MVC-Anwendung zusammenarbeiten. Außerdem sollten Sie verstehen, wie sich die Architektur einer ASP.NET MVC-Anwendung von einer Web Forms ASP.NET-Anwendung oder Active Server Pages-Anwendung unterscheidet.

## <a name="the-sample-aspnet-mvc-application"></a>Das Beispiel ASP.NET MVC-Anwendung

Die standardmäßige Visual Studio-Vorlage zum Erstellen von ASP.NET-MVC-Webanwendungen enthält eine äußerst einfache Beispielanwendung, die verwendet werden kann, um die verschiedenen Teile einer ASP.NET MVC-Anwendung zu verstehen. Wir nutzen diese einfache Anwendung in diesem Tutorial.

Sie erstellen eine neue ASP.NET MVC-Anwendung mit der MVC-Vorlage, indem Sie Visual Studio 2008 starten und die Menü Optionsdatei, neues Projekt auswählen (siehe Abbildung 1). Wählen Sie im Dialogfeld Neues Projekt Unterprojekt Typen (Visual Basic oder C#) Ihre bevorzugte Programmiersprache aus, und wählen Sie unter Vorlagen die Option **ASP.NET MVC-Webanwendung** aus. Klicken Sie auf die Schaltfläche "OK".

[Dialog Feld "Neues Projekt !["](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Abbildung 01**: Dialog Feld "Neues Projekt" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-models-views-and-controllers-cs/_static/image2.png))

Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, wird das Dialogfeld Komponenten **Test Projekt erstellen** angezeigt (siehe Abbildung 2). In diesem Dialogfeld können Sie ein separates Projekt in der Projekt Mappe erstellen, um Ihre ASP.NET MVC-Anwendung zu testen. Wählen Sie die Option **Nein, kein Komponenten Testprojekt erstellen aus,** und klicken Sie auf die Schaltfläche **OK** .

[![Dialog Feld "Komponenten Test erstellen"](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Abbildung 02**: Dialog Feld "Komponenten Test erstellen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-models-views-and-controllers-cs/_static/image4.png))

Nachdem die neue ASP.NET MVC-Anwendung erstellt wurde. Im Fenster Projektmappen-Explorer werden mehrere Ordner und Dateien angezeigt. Insbesondere werden drei Ordner mit den Namen "Models", "Views" und "Controllers" angezeigt. Wie Sie möglicherweise von den Ordnernamen ausgehen, enthalten diese Ordner die Dateien zum Implementieren von Modellen, Ansichten und Controllern.

Wenn Sie den Ordner "Controllers" erweitern, sollte eine Datei mit dem Namen AccountController.cs und eine Datei mit dem Namen HomeController.cs angezeigt werden. Wenn Sie den Ordner "Views" erweitern, sollten drei Unterordner namens "Account", "Home" und "Shared" angezeigt werden. Wenn Sie den Basisordner erweitern, sehen Sie zwei weitere Dateien mit den Namen "about. aspx" und "index. aspx" (siehe Abbildung 3). Diese Dateien bilden die Beispielanwendung, die in der standardmäßigen ASP.NET-MVC-Vorlage enthalten ist.

[Fenster "Projektmappen-Explorer" ![](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Abbildung 03**: das Fenster "Projektmappen-Explorer" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-models-views-and-controllers-cs/_static/image6.png))

Sie können die Beispielanwendung ausführen, indem Sie die Menüoption **Debuggen, Debuggen starten**auswählen. Alternativ können Sie die Taste F5 drücken.

Wenn Sie eine ASP.NET-Anwendung zum ersten Mal ausführen, wird das Dialogfeld in Abbildung 4 angezeigt, in dem Sie den Debugmodus aktivieren. Klicken Sie auf die Schaltfläche OK, damit die Anwendung ausgeführt wird.

[Dialogfeld ![Debuggen nicht aktiviert](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Abbildung 04**: Dialogfeld "Debuggen deaktiviert" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-models-views-and-controllers-cs/_static/image8.png))

Wenn Sie eine ASP.NET MVC-Anwendung ausführen, wird die Anwendung von Visual Studio in Ihrem Webbrowser gestartet. Die Beispielanwendung besteht nur aus zwei Seiten: der Index Seite und der Info-Seite. Wenn die Anwendung zum ersten Mal gestartet wird, wird die Index Seite angezeigt (siehe Abbildung 5). Sie können zur Info-Seite navigieren, indem Sie oben rechts in der Anwendung auf den Menü Link klicken.

[![der Index Seite](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Abbildung 05**: die Index Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-models-views-and-controllers-cs/_static/image11.png))

Beachten Sie die URLs in der Adressleiste Ihres Browsers. Wenn Sie z. b. auf den Link info (Info) klicken, wird die URL in der Adressleiste des Browsers in **/Home/About**geändert.

Wenn Sie das Browserfenster schließen und zu Visual Studio zurückkehren, sind Sie nicht in der Lage, eine Datei mit dem Pfad "Home/Info" zu finden. Die Dateien sind nicht vorhanden. Wie ist dies möglich?

## <a name="a-url-does-not-equal-a-page"></a>Eine URL entspricht nicht der Seite.

Wenn Sie ein herkömmliches ASP.net-Web Forms Anwendung oder eine Active Server Pages-Anwendung erstellen, besteht eine 1:1-Entsprechung zwischen einer URL und einer Seite. Wenn Sie eine Seite mit dem Namen "somepage. aspx" vom Server anfordern, gab es eine Seite auf dem Datenträger mit dem Namen "somepage. aspx". Wenn die Datei "somepage. aspx" nicht vorhanden ist, erhalten Sie den Fehler "ungeschützte **404-Seite nicht gefunden** ".

Wenn Sie eine ASP.NET MVC-Anwendung entwickeln, besteht im Gegensatz dazu keine Übereinstimmung zwischen der URL, die Sie in die Adressleiste Ihres Browsers eingeben, und den Dateien, die Sie in der Anwendung finden. In einer ASP.NET-MVC-Anwendung entspricht eine URL einer Controller Aktion anstelle einer Seite auf dem Datenträger.

In einer herkömmlichen ASP.net-oder ASP-Anwendung werden Browser Anforderungen Seiten zugeordnet. In einer ASP.NET MVC-Anwendung werden Browser Anforderungen dagegen Controller Aktionen zugeordnet. Eine ASP.net Web Forms-Anwendung ist Inhalts orientiert. Eine ASP.NET MVC-Anwendung dagegen ist Anwendungslogik zentrisch.

## <a name="understanding-aspnet-routing"></a>Grundlegendes ASP.NET Routing

Eine Browser Anforderung wird einer Controller Aktion durch eine Funktion des ASP.NET-Frameworks namens *ASP.NET Routing*zugeordnet. ASP.NET Routing wird vom ASP.NET MVC-Framework verwendet, um eingehende Anforderungen an Controller Aktionen weiter *zuleiten* .

ASP.NET Routing verwendet eine Routing Tabelle, um eingehende Anforderungen zu verarbeiten. Diese Routing Tabelle wird erstellt, wenn Ihre Webanwendung zum ersten Mal gestartet wird. Die Routing Tabelle wird in der Datei Global. asax eingerichtet. Die MVC-Standarddatei "Global. asax" ist in der Liste 1 enthalten.

**Codebeispiel 1: Global. asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Wenn eine ASP.NET-Anwendung zum ersten Mal gestartet wird, wird die Anwendung\_Start ()-Methode aufgerufen. In der Liste 1 ruft diese Methode die Methode "RegisterRoutes ()" auf, und die Methode "RegisterRoutes ()" erstellt die Standardrouten Tabelle.

Die Standardrouten Tabelle besteht aus einer Route. Diese Standardroute unterbricht alle eingehenden Anforderungen in drei Segmente (ein URL-Segment ist alles zwischen Schrägstrichen). Das erste Segment ist einem Controller Namen zugeordnet, das zweite Segment ist einem Aktions Namen zugeordnet, und das letzte Segment wird einem Parameter zugeordnet, der an die Aktion mit dem Namen ID übergeben wird.

Nehmen wir beispielsweise die folgende URL:

/Product/Details/3

Diese URL wird in drei Parameter wie folgt analysiert:

Controller = Produkt

Aktion = Details

ID = 3

Die Standardroute, die in der Datei Global. asax definiert ist, enthält Standardwerte für alle drei Parameter. Der Standard Controller ist Home, die Standardaktion ist Index, und die Standard-ID ist eine leere Zeichenfolge. Berücksichtigen Sie bei diesen Standardeinstellungen die Analyse der folgenden URL:

/Employee

Diese URL wird in drei Parameter wie folgt analysiert:

Controller = Mitarbeiter

Action = Index

Id = ��

Wenn Sie schließlich eine ASP.NET MVC-Anwendung ohne Angabe einer URL öffnen (z. b. `http://localhost`), wird die URL wie folgt analysiert:

Controller = Startseite

Action = Index

Id = ��

Die Anforderung wird an die Index ()-Aktion für die HomeController-Klasse weitergeleitet.

## <a name="understanding-controllers"></a>Grundlagen der Controller

Ein Controller ist verantwortlich für die Steuerung der Art und Weise, in der ein Benutzer mit einer MVC-Anwendung interagiert. Ein Controller enthält die Fluss Steuerungslogik für eine ASP.NET MVC-Anwendung. Ein Controller bestimmt, welche Antwort an einen Benutzer zurückgesendet wird, wenn ein Benutzer eine Browser Anforderung sendet.

Ein Controller ist nur eine Klasse (z. b. ein Visual Basic C# oder eine Klasse). Die MVC-Beispielanwendung enthält einen Controller namens HomeController.cs, der sich im Ordner Controller befindet. Der Inhalt der Datei HomeController.cs wird in der Liste 2 reproduziert.

**Codebeispiel 2: HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Beachten Sie, dass der HomeController zwei Methoden mit dem Namen "Index ()" und "about ()" aufweist. Diese beiden Methoden entsprechen den beiden Aktionen, die vom Controller verfügbar gemacht werden. Die URL/Home/Index Ruft die Methode HomeController. Index () auf, und die URL/Home/About Ruft die Methode HomeController. about () auf.

Alle öffentlichen Methoden in einem Controller werden als Controller Aktion verfügbar gemacht. Hierfür müssen Sie vorsichtig vorgehen. Dies bedeutet, dass jede öffentliche Methode, die in einem Controller enthalten ist, von jeder Person mit Zugriff auf das Internet aufgerufen werden kann, indem die richtige URL in einen Browser eingegeben wird.

## <a name="understanding-views"></a>Grundlegendes zu Sichten

Die beiden Controller Aktionen, die von der HomeController-Klasse, Index () und about () verfügbar gemacht werden, geben eine Ansicht zurück. Eine Ansicht enthält das HTML-Markup und den Inhalt, die an den Browser gesendet werden. Eine Sicht entspricht einer Seite bei der Arbeit mit einer ASP.NET MVC-Anwendung.

Sie müssen die Ansichten am richtigen Speicherort erstellen. Die Aktion HomeController. Index () gibt eine Ansicht zurück, die sich unter folgendem Pfad befindet:

\Views\home\index.aspx

Die Aktion HomeController. about () gibt eine Ansicht zurück, die sich unter folgendem Pfad befindet:

\Views\home\about.aspx

Im Allgemeinen müssen Sie, wenn Sie eine Ansicht für eine Controller Aktion zurückgeben möchten, einen Unterordner im Ordner "Views" mit dem gleichen Namen wie Ihr Controller erstellen. Im Unterordner müssen Sie eine ASPX-Datei mit dem gleichen Namen wie die Controller Aktion erstellen.

Die Datei in der Liste 3 enthält die Ansicht Info. aspx.

**Codebeispiel 3: about. aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Wenn Sie die erste Zeile in der Liste 3 ignorieren, besteht der größte Teil der Ansicht aus HTML-Standard. Sie können den Inhalt der Ansicht ändern, indem Sie einen beliebigen HTML-Code eingeben, den Sie hier wünschen.

Eine Ansicht ähnelt einer Seite in Active Server Seiten oder ASP.net Web Forms. Eine Sicht kann HTML-Inhalt und Skripts enthalten. Sie können die Skripts in Ihre bevorzugte .NET-Programmiersprache schreiben (z C# . b. oder Visual Basic .net). Skripts werden zum Anzeigen dynamischer Inhalte verwendet, z. b. Datenbankdaten.

## <a name="understanding-models"></a>Grundlegendes zu Modellen

Wir haben auf Controller eingegangen, und wir haben die Ansichten erläutert. Das letzte Thema, das wir erörtern müssen, sind Modelle. Was ist ein MVC-Modell?

Ein MVC-Modell enthält die gesamte Anwendungslogik, die nicht in einer Ansicht oder einem Controller enthalten ist. Das Modell sollte sämtliche Anwendungs Geschäftslogik, Validierungs Logik und Datenbankzugriffs Logik enthalten. Wenn Sie z. b. den Microsoft-Entity Framework verwenden, um auf Ihre Datenbank zuzugreifen, erstellen Sie Ihre Entity Framework Klassen (die EDMX-Datei) im Ordner "Models".

Eine Sicht sollte nur Logik enthalten, die sich auf das Erstellen der Benutzeroberfläche bezieht. Ein Controller sollte nur das erforderliche Minimum an Logik enthalten, die erforderlich ist, um die Rechte Ansicht zurückzugeben oder den Benutzer an eine andere Aktion (Fluss Steuerung) umzuleiten. Alles andere sollte im Modell enthalten sein.

Im Allgemeinen sollten Sie sich für FAT-Modelle und Dinge-Controller bemühen. Die Controller Methoden sollten nur einige wenige Codezeilen enthalten. Wenn eine Controller Aktion zu dick ist, sollten Sie erwägen, die Logik in eine neue Klasse im Ordner Models zu verschieben.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie einen Überblick über die verschiedenen Bestandteile einer ASP.NET MVC-Webanwendung erhalten. Sie haben gelernt, wie ASP.NET-Routing eingehende Browser Anforderungen bestimmten Controller Aktionen zuordnet. Sie haben gelernt, wie Controller orchestrieren, wie Ansichten an den Browser zurückgegeben werden. Schließlich haben Sie gelernt, wie Modelle Anwendungs Geschäfts-, Validierungs-und Datenbankzugriffs Logik enthalten.
