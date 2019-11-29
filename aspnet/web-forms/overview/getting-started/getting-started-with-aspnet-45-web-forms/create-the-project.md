---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Erstellen Sie das Projekt | Microsoft-Dokumentation
author: Erikre
description: Diese tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 62918b17f42e54dfe4e45a08927b1039dcbb7012
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576067"
---
# <a name="create-the-project"></a>Erstellen des Projekts

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für das Web. Für diese tutorialreihe steht ein Visual Studio 2013- [Projekt mit C# Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) zur Verfügung.

In diesem Tutorial erstellen, überprüfen und führen Sie das Standard Projekt in Visual Studio aus. Dadurch können Sie sich mit den Features von ASP.NET vertraut machen. Außerdem wird die Visual Studio-Umgebung überprüft.

## <a name="what-youll-learn"></a>Lernen Sie Folgendes:

- Erstellen eines neuen Web Forms Projekts
- Die Dateistruktur des Web Forms Projekts.
- So führen Sie das Projekt in Visual Studio aus.
- Die verschiedenen Funktionen der Web Forms-Standardanwendung.
- Einige Grundlagen zur Verwendung der Visual Studio-Umgebung.

## <a name="creating-the-project"></a>Erstellen des Projekts

1. Öffnen Sie Visual Studio.
2. Wählen Sie im Menü **Datei** in Visual Studio die Option **Neues Projekt** aus. 

    ![Erstellen des Menü Elements "Projekt-Neues Projekt"](create-the-project/_static/image1.png)
3. Wählen Sie auf der linken Seite die **Vorlagen** -&gt;  **C# Visual** -&gt; **Webvorlagen** Gruppe aus.
4. Wählen Sie die Vorlage **ASP.NET-Webanwendung** in der mittleren Spalte aus.  
 In dieser tutorialreihe wird .NET Framework 4.5.2 verwendet.
5. Benennen Sie Ihr Projekt mit *wingtiptoys* , und wählen Sie die Schaltfläche **OK** . 

    ![Projekt erstellen-Dialog Feld "Neues Projekt"](create-the-project/_static/image2.png)

    > [!NOTE]
    > Der Name des Projekts in dieser tutorialreihe ist **wingtiptoys**. Es wird empfohlen, dass Sie diesen *exakten* Projektnamen verwenden, damit der Code, der in der tutorialreihe bereitgestellt wird, erwartungsgemäß funktioniert.

6. Klicken Sie auf die Schaltfläche **Authentifizierung ändern** . Wählen Sie **einzelne Benutzerkonten** aus, und klicken Sie auf **OK** .

7. Wählen Sie die **Web Forms** Vorlage aus, und klicken Sie auf **OK** .

    ![Erstellen der Projektvorlage "Neues Projekt"](create-the-project/_static/image3.png)

Die Erstellung des Projekts nimmt einige Zeit in Anspruch. Wenn Sie fertig sind, öffnen Sie die Seite **default. aspx** .

![Erstellen der Projektvorlage "Neues Projekt"](create-the-project/_static/image4.png)

Sie können zwischen der **Entwurfs** Ansicht und der **Quell** Ansicht wechseln, indem Sie unten im mittleren Fenster eine Option auswählen. In der **Entwurfs** Ansicht werden ASP.NET Webseiten, Masterseiten, Inhaltsseiten, HTML-Seiten und Benutzer Steuerelemente mithilfe einer near-WYSIWYG-Ansicht angezeigt. In der **Quell** Ansicht wird das HTML-Markup für die Webseite angezeigt, das Sie bearbeiten können.

> [!TIP] 
> 
> **Grundlegendes zu ASP.NET-Frameworks**
> 
> Mit ASP.NET Web Forms können Sie dynamische Websites mit einem vertrauten ereignisgesteuerten Modell und Unterstützung für Ziehen und Ablegen erstellen. Mit einer Designoberfläche und Hunderten von Steuerelementen und Komponenten können Sie schnell und einfach komplexe und umfangreiche GUI-gesteuerte Websites mit Datenzugriff erstellen. Der Wingtip-Spielzeug Speicher basiert auf ASP.net Web Forms, aber viele der Konzepte, die Sie in dieser tutorialreihe erlernen, gelten für alle ASP.net.
> 
> ASP.net bietet vier primäre Entwicklungs-Frameworks:
> 
> - [ASP.net Web Forms](../../../index.md)  
>  Das Web Forms Framework richtet sich an Entwickler, die die deklarative und Steuerelement basierte Programmierung bevorzugen, wie z. b. Microsoft Windows Forms (WinForms) und WPF/XAML/Silverlight. Es bietet ein WYSIWYG-Designer-gesteuerte Entwicklungsmodell, sodass es für Entwickler beliebt ist, eine schnelle Anwendungs Entwicklungsumgebung (Rad) für die Webentwicklung zu suchen. Wenn Sie noch keine Erfahrungen mit der Webprogrammierung haben und mit den herkömmlichen Microsoft Rad-Client Entwicklungs Tools (z. b. für C#Visual Basic und Visual) vertraut sind, können Sie schnell eine Webanwendung erstellen, ohne in HTML und JavaScript zu arbeiten.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC richtet sich an Entwickler, die an Mustern und Prinzipien interessiert sind, wie z. b. die Test gesteuerte Entwicklung, die Trennung von Anliegen, die Inversion of Control (IOC) und die Abhängigkeitsinjektion (di). Dieses Framework fördert die Trennung der Geschäftslogik Schicht einer Webanwendung von der Darstellungs Schicht.
> - [ASP.NET-Webseiten 2](../../../../web-pages/index.md)  
>  ASP.net Web Pages ist für Entwickler konzipiert, die eine einfache webentwicklungsprostory in den PHP-Zeilen wünschen. Im Web Pages-Modell erstellen Sie HTML-Seiten und fügen der Seite dann serverbasierten Code hinzu, um dynamisch zu steuern, wie das Markup gerendert wird. Web Pages wurde speziell als einfaches Framework entwickelt und ist der einfachste Einstiegspunkt in ASP.net für Personen, die HTML kennen, aber möglicherweise nicht über eine umfassende Programmiersprache verfügen, z. b. Schüler/Studenten oder Hobby Entwickler. Es ist auch eine gute Möglichkeit für Webentwickler, die PHP oder ähnliche Frameworks kennen, um mit der Verwendung von ASP.net zu beginnen.
> - [ASP.net Single-Page-Anwendung](../../../../single-page-application/index.md)  
>  ASP.net Single Page Application (Spa) unterstützt Sie beim Erstellen von Anwendungen, die bedeutende Client seitige Interaktionen mit HTML 5, CSS 3 und JavaScript einschließen. Das ASP.net and Web Tools 2012,2-Update enthält eine neue Vorlage zum Erstellen von Single-Page-Anwendungen mithilfe von Knockout. js und ASP.net-Web-API. Neben der neuen Spa-Vorlage stehen auch neue, von der Community erstellte Spa-Vorlagen zum Download zur Verfügung.
> 
> Zusätzlich zu den vier Haupt Entwicklungs-Frameworks bietet ASP.net auch weitere Technologien, die Sie kennen und kennen müssen, aber nicht in dieser tutorialreihe behandelt werden:
> 
> - [ASP.net-Web-API](../../../../web-api/index.md) : ein Framework zum Entwickeln von http-Diensten, die eine breite Palette von Clients erreichen, einschließlich Browsern und mobilen Geräten.
> - [ASP.net signalr](../../../../signalr/index.md) : eine Bibliothek, die die Entwicklung von Echt Zeit Webrollen erleichtert.

### <a name="reviewing-the-project"></a>Überprüfen des Projekts

In Visual Studio können Sie im **Projektmappen-Explorer** Fenster Dateien für das Projekt verwalten. Werfen wir einen Blick auf die Ordner, die Ihrer Anwendung in **Projektmappen-Explorer**hinzugefügt wurden. Die Webanwendungsvorlage fügt eine grundlegende Ordnerstruktur hinzu:

![Erstellen Sie das Projekt Projektmappen-Explorer](create-the-project/_static/image5.png)

Visual Studio erstellt einige anfängliche Ordner und Dateien für das Projekt. Die ersten Dateien, mit denen Sie später in diesem Tutorial arbeiten werden, lauten wie folgt:

| **Datei** | **Darin** |
| --- | --- |
| *Default. aspx* | In der Regel die erste Seite, die angezeigt wird, wenn die Anwendung in einem Browser ausgeführt wird. |
| *Site. Master* | Eine Seite, auf der Sie ein konsistentes Layout erstellen und das Standardverhalten für Seiten in Ihrer Anwendung verwenden können. |
| *Global. asax* | Eine optionale Datei, die Code zum reagieren auf Ereignisse auf Anwendungsebene und auf Sitzungs Ebene enthält, die von ASP.net oder HTTP-Modulen ausgelöst werden. |
| *"Web. config"* | Die Konfigurationsdaten für eine Anwendung. |

### <a name="running-the-default-web-application"></a>Ausführen der Standardweb Anwendung

Die Standardweb Anwendung bietet umfassende Funktionen, die auf integrierten Funktionen und Unterstützung basieren. Ohne Änderungen am standardmäßigen Web Forms-Projekt kann die Anwendung im lokalen Webbrowser ausgeführt werden.

1. Drücken Sie in Visual Studio die Taste ***F5*** .   
 Die Anwendung wird im Webbrowser erstellt und angezeigt.  

    ![Erstellen des Projekts: Standardseite](create-the-project/_static/image6.png)
2. Nachdem Sie die laufende Anwendung überprüft haben, schließen Sie das Browserfenster.

Diese Standardweb Anwendung enthält drei Hauptseiten: " *default. aspx* (Home)", " *about. aspx*" und " *Contact. aspx*". Jede dieser Seiten kann von der oberen Navigationsleiste aus erreicht werden. Im Konto Ordner sind auch zwei zusätzliche Seiten enthalten, die Seite "Register. aspx" und die Seite "Login. aspx". Diese beiden Seiten ermöglichen Ihnen die Verwendung der Mitgliedschafts Funktionen von ASP.net, um Benutzer Anmelde Informationen zu erstellen, zu speichern und zu überprüfen.

## <a name="aspnet-web-forms-background"></a>ASP.net Web Forms Hintergrund

ASP.net Web Forms sind Seiten, die auf Microsoft ASP.NET Technologie basieren, bei der auf dem Server ausgeführte Code dynamisch eine Webseiten Ausgabe an den Browser oder das Client Gerät generiert. Eine ASP.net Web Forms Seite rendert automatisch den richtigen Browser kompatiblen HTML-Code für Funktionen wie Stile, Layout usw. Web Forms sind kompatibel mit jeder Sprache, die von der .NET-Common Language Runtime unterstützt wird, z. C#b. Microsoft Visual Basic und Microsoft Visual. Außerdem werden Web Forms auf [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123)erstellt, das Vorteile bietet, wie z. b. eine verwaltete Umgebung, Typsicherheit und Vererbung.

Wenn eine ASP.net-Web Forms Seite ausgeführt wird, durchläuft die Seite einen Lebenszyklus, in dem eine Reihe von Verarbeitungsschritten ausgeführt werden. Diese Schritte umfassen Initialisierung, Instanziierung von Steuerelementen, Wiederherstellung und Wartung des Zustands, Ausführen von Ereignishandlercode und Rendering. Wenn Sie sich mit der Leistungsfähigkeit von ASP.net Web Forms vertraut machen, ist es wichtig, dass Sie den [Lebenszyklus der ASP.NET-Seite](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) verstehen, damit Sie Code in der richtigen Lebenszyklusphase für die gewünschte Auswirkung schreiben können.

Wenn ein Webserver eine Anforderung für eine Seite empfängt, sucht er die Seite, verarbeitet sie und sendet Sie an den Browser und verwirft dann alle Seiten Informationen. Wenn der Benutzer die gleiche Seite erneut anfordert, wiederholt der Server die gesamte Sequenz und verarbeitet die Seite neu. Anders ausgedrückt: ein Server hat keinen Arbeitsspeicher für Seiten, auf denen er verarbeitet hat-Seiten sind zustandslos. Das ASP.NET-Seiten Framework behandelt automatisch die Aufgabe der Beibehaltung des Zustands der Seite und der zugehörigen Steuerelemente und bietet explizite Möglichkeiten, den Zustand von anwendungsspezifischen Informationen beizubehalten.

> [!TIP] 
> 
> **Webanwendungs Features in der Web Forms-Anwendungs Vorlage**
> 
> Die Anwendungs Vorlage ASP.net Web Forms bietet einen umfangreichen Satz integrierter Funktionen. Es bietet Ihnen nicht nur die Seite " *Home. aspx* ", eine Seite " *about. aspx* ", eine " *Contact. aspx* "-Seite, sondern auch Mitgliedschafts Funktionen, die Benutzer registrieren und ihre Anmelde Informationen speichern, damit Sie sich bei Ihrer Website anmelden können. Diese Übersicht enthält weitere Informationen zu einigen Features, die in der ASP.NET-Anwendungs Vorlage Web Forms und ihrer Verwendung in der Wingtip Toys-Anwendung enthalten sind.
> 
> **Mitgliedschaft**
> 
> [ASP.net](https://msdn.microsoft.com/library/yh26yfzy.aspx) Identity speichert die Anmelde Informationen Ihrer Benutzer in einer Datenbank, die von der Anwendung erstellt wurde. Wenn sich Ihre Benutzer anmelden, überprüft die Anwendung ihre Anmelde Informationen, indem Sie die Datenbank liest. Der *Konto* Ordner Ihres Projekts enthält die Dateien, die die verschiedenen Teile der Mitgliedschaft implementieren: registrieren, anmelden, Ändern eines Kennworts und Autorisierungs Zugriff. Darüber hinaus unterstützt ASP.net Web Forms OAuth und OpenID. Diese Authentifizierungs Erweiterungen ermöglichen es Benutzern, sich bei der Website mit vorhandenen Anmelde Informationen anzumelden, wie z. b. Facebook, Twitter, Windows Live und Google.
> 
> ![Erstellen der Projekt Projektmappen-Explorer (ASP.net Identity)](create-the-project/_static/image7.png)
> 
> Standardmäßig erstellt die Vorlage eine Mitgliedschafts Datenbank unter Verwendung eines Standarddaten Banknamens in einer Instanz von SQL Server Express localdb, dem Entwicklungsdaten Bank Server, der mit Visual Studio Express 2013 für Web bereit steht.
> 
> **SQL Server Express localdb**
> 
> [SQL Server Express localdb](https://technet.microsoft.com/library/hh510202.aspx) ist eine vereinfachte Version von SQL Server, die viele Programmierbarkeits Funktionen einer SQL Server-Datenbank aufweist. SQL Server Express localdb wird im Benutzermodus ausgeführt und verfügt über eine schnelle, Konfigurations freie Installation mit einer kurzen Liste von Installations Voraussetzungen. In Microsoft SQL Server kann jeder Datenbank-oder Transact-SQL-Code von SQL Server Express localdb zu SQL Server und SQL Azure verschoben werden, ohne dass Upgradeschritte ausgeführt werden müssen. Daher kann SQL Server Express localdb als Entwicklerumgebung für Anwendungen verwendet werden, die auf alle Editionen von SQL Server abzielen. SQL Server Express localdb ermöglicht Features wie z. b. gespeicherte Prozeduren, benutzerdefinierte Funktionen und Aggregate, .NET Framework Integration, räumliche Typen und andere, die in SQL Server Compact nicht verfügbar sind.
> 
> **Masterseiten**
> 
> Eine [ASP.net-Master Seite](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definiert eine konsistente Darstellung und ein konsistentes Verhalten für alle Seiten in der Anwendung. Das Layout der Master Seite wird mit dem Inhalt einer einzelnen Inhaltsseite zusammengeführt, um die letzte Seite zu erstellen, die dem Benutzer angezeigt wird. In der Wingtip Toys-Anwendung ändern Sie die Master Seite " *Site. Master* " so, dass alle Seiten auf der Wingtip Toys-Website das gleiche besondere Logo und die gleiche Navigationsleiste gemeinsam verwenden.
> 
> **HTML5**
> 
> Die ASP.net-Web Forms-Anwendungs Vorlage unterstützt [HTML5](http://www.w3schools.com/html/html5_intro.asp), bei dem es sich um die neueste Version der HTML-Markup Sprache handelt. HTML5 unterstützt neue Elemente und Funktionen, die das Erstellen von Websites vereinfachen.
> 
> **Modernizr**
> 
> Für Browser, die HTML5 nicht unterstützen, können Sie [modernizr](http://www.modernizr.com/)verwenden. Modernizr ist eine Open-Source-JavaScript-Bibliothek, mit der erkannt werden kann, ob ein Browser HTML5-Features unterstützt, und wenn dies nicht der Fall ist. In der ASP.net-Web Forms Anwendungs Vorlage wird modernizr als nuget-Paket installiert.
> 
> **Bootstrap**
> 
> Die Visual Studio 2013-Projektvorlagen verwenden [Bootstrap](http://getbootstrap.com/), ein Layout und Design-Framework, das von Twitter erstellt wurde. Bootstrap verwendet CSS3, um reaktionsfähiges Design bereitzustellen. Dies bedeutet, dass Layouts dynamisch an verschiedene Browserfenster Größen angepasst werden können. Sie können auch die Funktion des Bootstrap-Features verwenden, um auf einfache Weise eine Änderung im Aussehen und Verhalten der Anwendung zu beeinflussen. Standardmäßig enthält die Vorlage ASP.NET-Webanwendung in Visual Studio 2013 Bootstrap als nuget-Paket.
> 
> **Nuget-Pakete**
> 
> Die Anwendungs Vorlage ASP.net Web Forms enthält einen Satz von [nuget](http://www.nuget.org/) -Paketen. Diese Pakete bieten Komponenten in Form von Open Source-Bibliotheken und-Tools. Es gibt eine Vielzahl von Paketen, die Sie beim Erstellen und Testen Ihrer Anwendungen unterstützen. Visual Studio erleichtert das Hinzufügen, entfernen und Aktualisieren von nuget-Paketen. Entwickler können auch nuget-Pakete erstellen und hinzufügen.
> 
> ![Erstellen des Dialog Felds "Projekt-nuget"](create-the-project/_static/image8.png)
> 
> Wenn Sie ein Paket installieren, werden von nuget Dateien in die Projekt Mappe kopiert und automatisch alle erforderlichen Änderungen vorgenommen, z. b. das Hinzufügen von verweisen und das Ändern der Konfiguration, die der Webanwendung zugeordnet ist. Wenn Sie sich entschließen, die Bibliothek zu entfernen, entfernt nuget die Dateien und kehrt alle Änderungen, die Sie im Projekt vorgenommen haben, so um, dass keine Übersichtlichkeit vorhanden ist. Nuget steht im Menü Extras in Visual **Studio zur Verfügung** .
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) ist eine schnelle und präzise JavaScript-Bibliothek, die das Durchlaufen von HTML-Dokumenten, Ereignis Behandlung, Animation und AJAX-Interaktionen für die schnelle Webentwicklung vereinfacht. Die jQuery-JavaScript-Bibliothek ist in der ASP.net-Web Forms-Anwendungs Vorlage als nuget-Paket enthalten.
> 
> **Unaufdringliche Validierung**
> 
> Integrierte Prüfungs-Steuerelemente wurden so konfiguriert, dass unaufdringliches JavaScript für die Client seitige Validierungs Logik verwendet wird. Dadurch wird die Menge an JavaScript, die Inline im Seiten Markup gerendert wird, erheblich reduziert, und die Gesamt Seitengröße wird reduziert. Der ASP.net-Web Forms Anwendungs Vorlage wird auf der Grundlage der Einstellung im &lt;appSettings-&gt; Element der Datei " *Web. config* " im Stammverzeichnis der Anwendung Global eine unaufdringliche Validierung hinzugefügt.
> 
> **Entity Framework Code First**
> 
> Neben den Features in der ASP.net-Web Forms-Anwendungs Vorlage verwendet die Wingtip Toys-Anwendung [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx). dabei handelt es sich um eine nuget-Bibliothek, die die Code zentrierte Entwicklung bei der Arbeit mit Daten ermöglicht. Einfach ausgedrückt, erstellt Sie den Daten Bank Teil Ihrer Anwendung für Sie basierend auf dem Code, den Sie schreiben. Mithilfe der Entity Framework können Sie Daten als stark typisierte Objekte abrufen und bearbeiten. Auf diese Weise können Sie sich auf die Geschäftslogik in Ihrer Anwendung konzentrieren, anstatt auf die Details der Art des Zugriffs auf Daten zuzugreifen.
> 
> Weitere Informationen zu den installierten Bibliotheken und Paketen, die in der ASP.net-Web Forms Vorlage enthalten sind, finden Sie in der Liste der installierten nuget-Pakete. Erstellen Sie hierzu in Visual Studio ein neues Web Forms-Projekt, wählen Sie Extras > **nuget-Paket-Manager** > **nuget-Pakete für Projekt Mappe verwalten aus**, und klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **installierte Pakete** .

### <a name="touring-visual-studio"></a>Touring Visual Studio

Die primären Fenster in Visual Studio umfassen die **Projektmappen-Explorer**, die **Server-Explorer** (**Datenbank-Explorer** in Express), das **Eigenschaften Fenster**, die **Toolbox**, die **Symbolleiste**und das **Dokument Fenster**.

![Erstellen des Dialog Felds "Projekt-nuget"](create-the-project/_static/image9.png)

Weitere Informationen zu Visual Studio finden [Sie unter visueller Leitfaden für Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Summary

In diesem Tutorial haben Sie die Standard Web Forms Anwendung erstellt, überprüft und ausgeführt. Sie haben die verschiedenen Features der Web Forms-Standardanwendung überprüft und einige Grundlagen zur Verwendung der Visual Studio-Umgebung kennengelernt. In den folgenden Tutorials erstellen Sie die Datenzugriffs Ebene.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Auswählen des richtigen Programmiermodells](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Webanwendungs Projekte im Vergleich zu Website Projekten](https://msdn.microsoft.com/library/dd547590.aspx)   
[Übersicht über ASP.net Web Forms Pages](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Zurück](introduction-and-overview.md)
> [Weiter](create_the_data_access_layer.md)
