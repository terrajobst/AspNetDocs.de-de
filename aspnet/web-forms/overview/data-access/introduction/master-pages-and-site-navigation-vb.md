---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: Master Seiten und Website Navigation (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Ein gängiges Merkmal benutzerfreundlicher Websites ist, dass Sie über ein konsistentes Seitenlayout und Navigations Schema verfügen. In diesem Tutorial wird erläutert, wie y...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4a2b5ba8c1781f1194f951a44661a8f7dd095f41
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578748"
---
# <a name="master-pages-and-site-navigation-vb"></a>Masterseiten und Sitenavigation (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe) oder [PDF herunterladen](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> Ein gängiges Merkmal benutzerfreundlicher Websites ist, dass Sie über ein konsistentes Seitenlayout und Navigations Schema verfügen. In diesem Tutorial wird erläutert, wie Sie ein konsistentes Erscheinungsbild auf allen Seiten erstellen können, die problemlos aktualisiert werden können.

## <a name="introduction"></a>Einführung

Ein gängiges Merkmal benutzerfreundlicher Websites ist, dass Sie über ein konsistentes Seitenlayout und Navigations Schema verfügen. Mit ASP.NET 2,0 werden zwei neue Features eingeführt, die die Implementierung eines Website-weiten Seitenlayouts und Navigations Schemas erheblich vereinfachen: Masterseiten und Website Navigation. Mithilfe von Master Seiten können Entwickler eine Website weite Vorlage mit festgelegten bearbeitbaren Regionen erstellen. Diese Vorlage kann dann auf ASP.NET Seiten der Website angewendet werden. Solche ASP.NET Seiten müssen nur Inhalte für die angegebenen bearbeitbaren Bereiche der Master Seite bereitstellen. alle anderen Markup in der Master Seite sind für alle ASP.NET Seiten, die die Master Seite verwenden, identisch. Dieses Modell ermöglicht es Entwicklern, ein Site weites Seitenlayout zu definieren und zu zentralisieren. Dadurch wird es einfacher, ein konsistentes Aussehen und Verhalten auf allen Seiten zu erstellen, die problemlos aktualisiert werden können.

Das [Standort Navigationssystem](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) bietet einen Mechanismus, mit dem Seiten Entwickler eine Site Übersicht und eine API für diese Site Zuordnung definieren können, die Programm gesteuert abgefragt werden soll. Das neue Navigations-Web steuert das Menü, TreeView und SiteMapPath, um das Rendering der gesamten Site Map in einem gemeinsamen Navigations-Benutzeroberflächen Element zu vereinfachen. Wir verwenden den standardmäßigen site Navigations Anbieter. Dies bedeutet, dass die Site Übersicht in einer XML-formatierten Datei definiert wird.

Um diese Konzepte zu veranschaulichen und unsere Tutorials-Website verwendbar zu machen, können Sie diese Lektion nutzen, indem Sie ein Site weites Seitenlayout definieren, eine Site Map implementieren und die Navigations Benutzeroberfläche hinzufügen. Am Ende dieses Tutorials verfügen wir über einen polierten Website Entwurf zum Erstellen unserer Tutorial-Webseiten.

[![das Endergebnis dieses Tutorials](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**Abbildung 1**: das Endergebnis dieses Tutorials ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image3.png))

## <a name="step-1-creating-the-master-page"></a>Schritt 1: Erstellen der Master Seite

Der erste Schritt besteht darin, die Master Seite für die Website zu erstellen. Zurzeit besteht unsere Website nur aus dem typisierten DataSet (`Northwind.xsd`im Ordner `App_Code`), den BLL-Klassen (`ProductsBLL.vb`, `CategoriesBLL.vb`usw., alle im `App_Code` Ordner), der Datenbank (`NORTHWND.MDF`, im Ordner `App_Data`), der Konfigurationsdatei (`Web.config`) und einer CSS-Stylesheet-Datei (`Styles.css`). Ich habe die Seiten und Dateien bereinigt, die die Verwendung von Dal und BLL in den ersten beiden Tutorials veranschaulichen, da wir diese Beispiele in zukünftigen Tutorials ausführlicher untersuchen werden.

![Die Dateien in unserem Projekt](master-pages-and-site-navigation-vb/_static/image4.png)

**Abbildung 2**: die Dateien in unserem Projekt

Zum Erstellen einer Master Seite klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie neues Element hinzufügen aus. Wählen Sie dann in der Liste der Vorlagen den Typ der Master Seite aus, und benennen Sie ihn `Site.master`.

[![neue Master Seite zur Website hinzufügen](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**Abbildung 3**: Hinzufügen einer neuen Master Seite zur Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image7.png))

Definieren Sie das standortweite Seitenlayout hier auf der Master Seite. Sie können den Designansicht verwenden und alle benötigten Layout-oder websteuer Elemente hinzufügen, oder Sie können das Markup manuell in der Quell Ansicht hinzufügen. Auf der Master Seite verwende ich [Cascading Stylesheets](http://www.w3schools.com/css/default.asp) für die Positionierung und Stile mit den CSS-Einstellungen, die in der externen Datei `Style.css`definiert sind. Obwohl Sie nicht aus dem unten gezeigten Markup erkennen können, werden die CSS-Regeln so definiert, dass der Inhalt der Navigations `<div>`absolut so positioniert ist, dass er auf der linken Seite angezeigt wird und über eine festgelegte Breite von 200 Pixeln verfügt.

Site. Master

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

Eine Master Seite definiert sowohl das statische Seitenlayout als auch die Bereiche, die von den ASP.NET-Seiten bearbeitet werden können, die die Master Seite verwenden. Diese bearbeitbaren Inhaltsbereiche werden durch das contentplachalter-Steuerelement angegeben, das in der Inhalts `<div>`angezeigt wird. Unsere Master Seite verfügt über einen einzelnen contentplachalter (`MainContent`), die Master Seite kann jedoch mehrere Inhalts Platzhalter enthalten.

Wenn Sie das obige Markup eingegeben haben, zeigt der Wechsel zum Designansicht das Layout der Master Seite an. Alle ASP.NET Seiten, auf denen diese Master Seite verwendet wird, verfügen über dieses einheitliche Layout, mit der Möglichkeit, das Markup für den `MainContent` Bereich anzugeben.

[![der Master Seite, wenn Sie in der Entwurfs Ansicht angezeigt wird](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**Abbildung 4**: die Master Seite, wenn Sie in der Entwurfs Ansicht angezeigt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image10.png))

## <a name="step-2-adding-a-homepage-to-the-website"></a>Schritt 2: Hinzufügen einer Homepage zur Website

Wenn die Master Seite definiert ist, können wir die ASP.NET-Seiten für die Website hinzufügen. Zunächst fügen wir `Default.aspx`, die Homepage unserer Website, hinzu. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie neues Element hinzufügen aus. Wählen Sie in der Vorlagenliste die Option Webformular aus, und benennen Sie die Datei `Default.aspx`. Aktivieren Sie außerdem das Kontrollkästchen "Master Seite auswählen".

[![neues Webformular hinzufügen, aktivieren Sie das Kontrollkästchen Master Seite auswählen.](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**Abbildung 5**: Hinzufügen eines neuen Webformulars, Aktivieren des Kontrollkästchens "Master Seite auswählen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image13.png))

Nachdem Sie auf die Schaltfläche "OK" geklickt haben, werden wir gefragt, welche Master Seite auf dieser neuen ASP.NET-Seite verwendet werden soll. Es können zwar mehrere Masterseiten in Ihrem Projekt vorhanden sein, aber es ist nur eine vorhanden.

[![wählen Sie die Master Seite aus, die diese ASP.NET Seite verwenden soll.](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**Abbildung 6**: Auswählen der Master Seite, die von dieser ASP.NET-Seite verwendet werden soll ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image16.png))

Nach dem Auswählen der Master Seite enthalten die neuen ASP.NET-Seiten das folgende Markup:

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

In der `@Page`-Direktive gibt es einen Verweis auf die verwendete Masterseiten Datei (`MasterPageFile="~/Site.master"`), und das Markup der ASP.NET-Seite enthält ein Inhalts Steuerelement für jedes der contentplachalter-Steuerelemente, die auf der Master Seite definiert sind, wobei die `ContentPlaceHolderID` das Inhalts Steuerelement einem bestimmten contentplachalter-Element entspricht. Im Inhalts Steuerelement platzieren Sie das Markup, das im entsprechenden contentplachalter angezeigt werden soll. Legen Sie das `Title`-Attribut der `@Page` Direktive auf Home fest, und fügen Sie dem Content-Steuerelement einige einladende Inhalte hinzu:

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

Mit dem `Title`-Attribut in der `@Page`-Direktive können wir den Titel der Seite auf der ASP.NET-Seite festlegen, auch wenn das `<title>`-Element auf der Master Seite definiert ist. Wir können den Titel auch Programm gesteuert festlegen, indem Sie `Page.Title`verwenden. Beachten Sie außerdem, dass die Verweise auf Stylesheets (z. b. `Style.css`) der Master Seite automatisch aktualisiert werden, sodass Sie auf jeder ASP.NET-Seite funktionieren, unabhängig davon, in welchem Verzeichnis sich die ASP.NET-Seite relativ zur Master Seite befindet.

Wenn Sie zum Designansicht wechseln, können wir sehen, wie die Seite in einem Browser aussieht. Beachten Sie, dass im Designansicht für die Seite ASP.net nur die bearbeitbaren Inhaltsbereiche bearbeitet werden können, wenn das in der Master Seite definierte nicht-contentplachalter-Markup ausgegraut ist.

[![der Entwurfs Ansicht für die Seite ASP.net werden sowohl die bearbeitbaren als auch die nicht bearbeitbaren Bereiche angezeigt.](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**Abbildung 7**: in der Entwurfs Ansicht für die Seite "ASP.net" werden sowohl die bearbeitbaren als auch die nicht bearbeitbaren Bereiche angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image19.png))

Wenn die `Default.aspx` Seite von einem Browser besucht wird, führt die ASP.net-Engine automatisch den Masterseiten Inhalt der Seite und den ASP zusammen. Inhalt von NET, und rendert den zusammengeführten Inhalt in der endgültigen HTML-Ausgabe, die an den anfordernden Browser gesendet wird. Wenn der Inhalt der Master Seite aktualisiert wird, werden die Inhalte aller ASP.NET-Seiten, auf denen diese Master Seite verwendet wird, erneut mit dem neuen Masterseiten Inhalt zusammengeführt, wenn Sie das nächste Mal angefordert werden. Kurz gesagt ermöglicht das Modell der Master Seite die Definition einer einzelnen Seitenlayout-Vorlage (die Master Seite), deren Änderungen sofort über die gesamte Website reflektiert werden.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Hinzufügen zusätzlicher ASP.NET-Seiten zur Website

Nehmen wir uns einen Moment Zeit, um der Site weitere ASP.net page Stubs hinzuzufügen, die letztendlich die verschiedenen Berichterstattungs Demos enthalten. Es sind insgesamt mehr als 35 Demos vorhanden. anstatt alle Stub-Seiten zu erstellen, erstellen wir lediglich die ersten. Da es auch viele Demo Kategorien gibt, fügen Sie zur besseren Verwaltung der Demos einen Ordner für die Kategorien hinzu. Fügen Sie jetzt die folgenden drei Ordner hinzu:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Fügen Sie schließlich neue Dateien hinzu, wie im Projektmappen-Explorer in Abbildung 8 gezeigt. Beachten Sie beim Hinzufügen der einzelnen Dateien, dass Sie das Kontrollkästchen "Master Seite auswählen" aktivieren.

![Fügen Sie die folgenden Dateien hinzu:](master-pages-and-site-navigation-vb/_static/image20.png)

**Abbildung 8**: Hinzufügen der folgenden Dateien

## <a name="step-2-creating-a-site-map"></a>Schritt 2: Erstellen einer Site Übersicht

Eine der Herausforderungen bei der Verwaltung einer Website, die mehr als eine Handvoll Seiten umfasst, bietet den Besuchern eine einfache Möglichkeit, durch die Website zu navigieren. Zunächst muss die navigationstruktur der Site definiert werden. Als nächstes muss diese Struktur in Navigier Bare Benutzeroberflächen Elemente, z. b. Menüs oder Brotkrümel, übersetzt werden. Schließlich muss der gesamte Prozess gewartet und aktualisiert werden, wenn der Website neue Seiten hinzugefügt werden und vorhandene entfernt werden. Vor ASP.NET 2,0 waren Entwickler allein zum Erstellen der Navigationsstruktur der Website, zur Wartung und zur Übersetzung in Navigier Bare Benutzeroberflächen Elemente. Mit ASP.NET 2,0 können Entwickler jedoch das sehr flexible integrierte Site Navigationssystem nutzen.

Das ASP.NET 2,0-Standort Navigationssystem bietet Entwicklern die Möglichkeit, eine Site Übersicht zu definieren und dann über eine programmgesteuerte API auf diese Informationen zuzugreifen. ASP.net wird mit einem Site Übersichts Anbieter ausgeliefert, der erwartet, dass Site Übersichts Daten in einer XML-Datei gespeichert werden, die auf eine bestimmte Weise formatiert ist. Da das Website Navigationssystem jedoch auf dem [Anbieter Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) basiert, kann es erweitert werden, um alternative Möglichkeiten zum Serialisieren der Site Übersichts Informationen zu unterstützen. Jeff Prosise der Artikel, [auf den Sie gewartet haben](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) , zeigt, wie Sie einen Site Übersichts Anbieter erstellen, der die Site Map in einer SQL Server-Datenbank speichert. eine andere Möglichkeit besteht darin, [einen Site Übersichts Anbieter basierend auf der Dateisystem Struktur](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)zu erstellen.

In diesem Tutorial verwenden wir jedoch den standardmäßigen Site Übersichts Anbieter, der mit ASP.NET 2,0 ausgeliefert wird. Um die Site Übersicht zu erstellen, klicken Sie einfach mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, wählen Sie neues Element hinzufügen aus, und wählen Sie die Option Site Map aus. Belassen Sie den Namen `Web.sitemap`, und klicken Sie auf die Schaltfläche hinzufügen.

[![Hinzufügen einer Site Map zum Projekt](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**Abbildung 9**: Hinzufügen einer Site Übersicht zu Ihrem Projekt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image23.png))

Die Site Übersichts Datei ist eine XML-Datei. Beachten Sie, dass Visual Studio IntelliSense für die Struktur der Site Map bereitstellt. Die Site Übersichts Datei muss den `<siteMap>`-Knoten als Stamm Knoten aufweisen, der genau ein `<siteMapNode>` untergeordnetes Element enthalten muss. Dieses erste `<siteMapNode>` Element kann dann eine beliebige Anzahl von untergeordneten `<siteMapNode>` Elementen enthalten.

Definieren Sie die Site Übersicht, um die Dateisystem Struktur zu imitieren. Fügen Sie das `<siteMapNode>`-Element für jeden der drei Ordner und die untergeordneten `<siteMapNode>` Elemente für jede ASP.NET-Seite in diesen Ordnern hinzu, wie im folgenden Beispiel:

Web. Sitemap

[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

Die Site Übersicht definiert die Navigationsstruktur der Website, bei der es sich um eine Hierarchie handelt, in der die verschiedenen Abschnitte der Site beschrieben werden. Jedes `<siteMapNode>` Element in `Web.sitemap` stellt einen Abschnitt in der Navigationsstruktur der Site dar.

[![Site Map eine hierarchische Navigationsstruktur darstellt](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**Abbildung 10**: die Site Übersicht stellt eine hierarchische Navigationsstruktur dar ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image26.png))

ASP.net macht die Struktur der Site Map über die Sitemap- [Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx)des .NET Framework verfügbar. Diese Klasse verfügt über eine `CurrentNode`-Eigenschaft, die Informationen über den Abschnitt zurückgibt, den der Benutzer zurzeit besucht. die `RootNode`-Eigenschaft gibt den Stamm der Site Map (Home, in unserer Site Übersicht) zurück. Die Eigenschaften `CurrentNode` und `RootNode` geben [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) -Instanzen zurück, die Eigenschaften wie `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`usw. aufweisen, die das Durchlaufen der Site Übersichts Hierarchie ermöglichen.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Schritt 3: Anzeigen eines Menüs auf der Grundlage der Site Übersicht

Der Zugriff auf Daten in ASP.NET 2,0 kann Programm gesteuert, wie in ASP.NET 1. x, oder deklarativ durch die neuen [Datenquellen](https://msdn.microsoft.com/library/ms227679.aspx)-Steuerelemente erfolgen. Es gibt mehrere integrierte Datenquellen-Steuerelemente, z. b. das SqlDataSource-Steuerelement, für den Zugriff auf relationale Datenbankdaten, das ObjectDataSource-Steuerelement, für den Zugriff auf Daten von Klassen und andere. Sie können sogar eigene [benutzerdefinierte Datenquellen](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp)-Steuerelemente erstellen.

Die Datenquellen-Steuerelemente dienen als Proxy zwischen Ihrer ASP.NET-Seite und den zugrunde liegenden Daten. Um die abgerufenen Daten eines Datenquellen-Steuer Elements anzuzeigen, wird der Seite in der Regel ein weiteres websteuer Element hinzugefügt und an das Datenquellen-Steuerelement gebunden. Wenn Sie ein websteuer Element an ein Datenquellen Steuerelement binden möchten, legen Sie einfach die `DataSourceID`-Eigenschaft des websteuer Elements auf den Wert der `ID`-Eigenschaft des Datenquellen-Steuer Elements fest.

Zur Unterstützung bei der Arbeit mit den Daten der Site Map enthält ASP.NET das SiteMapDataSource-Steuerelement, das es uns ermöglicht, ein websteuer Element an die Site Übersicht unserer Website zu binden. Zwei websteuer Elemente: die TreeView und das Menü werden häufig verwendet, um eine Navigations Benutzeroberfläche bereitzustellen. Fügen Sie der Seite einfach eine SiteMapDataSource mit einem TreeView-oder Menu-Steuerelement hinzu, dessen `DataSourceID`-Eigenschaft entsprechend festgelegt ist, um die Site Übersichts Daten an eines dieser beiden Steuerelemente zu binden. Beispielsweise könnten Sie der Master Seite ein Menü Steuerelement mit folgendem Markup hinzufügen:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

Um eine präzisere Steuerung des ausgegebenen HTML-Codes zu erreichen, können wir das SiteMapDataSource-Steuerelement wie folgt an das Repeater-Steuerelement binden:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

Das SiteMapDataSource-Steuerelement gibt die Site Übersichts Hierarchie eine Ebene zu einem Zeitpunkt zurück, beginnend mit dem Stamm Site Übersichts Knoten (Home, in unserem Site Map), dann der nächsten Ebene (grundlegende Berichterstellung, Filtern von Berichten und angepasster Formatierung) usw. Beim Binden von SiteMapDataSource an einen Repeater listet Sie die erste zurückgegebene Ebene auf und instanziiert die `ItemTemplate` für jede `SiteMapNode` Instanz auf dieser ersten Ebene. Um auf eine bestimmte Eigenschaft des `SiteMapNode`zuzugreifen, können wir `Eval(propertyName)`verwenden, um die `Url` und `Title` Eigenschaften jedes `SiteMapNode`für das Hyperlink-Steuerelement zu erhalten.

Mit dem obigen Wiederholungs Beispiel wird das folgende Markup angezeigt:

[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

Diese Site Übersichts Knoten (grundlegende Berichterstellung, Filtern von Berichten und angepasste Formatierung) umfassen die *zweite* Ebene der dargestellten Site Map und nicht die erste. Dies liegt daran, dass die `ShowStartingNode`-Eigenschaft von SiteMapDataSource auf false festgelegt ist, sodass SiteMapDataSource den Stamm Site Übersichts Knoten umgeht und stattdessen die zweite Ebene in der Site Übersichts Hierarchie zurückgibt.

Um die untergeordneten Elemente für die grundlegende Berichterstellung, das Filtern von Berichten und die angepasste Formatierung `SiteMapNode` en anzuzeigen, können Sie der `ItemTemplate`des ersten wiederholers einen weiteren Wiederholungs Modul hinzufügen. Dieser zweite Repeater wird wie folgt an die `ChildNodes`-Eigenschaft der `SiteMapNode` Instanz gebunden:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

Diese beiden Repeater führen zu folgendem Markup (ein Markup wurde aus Gründen der Kürze entfernt):

[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

Mithilfe von CSS-Stilen aus [Rachel Andrew](http://www.rachelandrew.co.uk/)Book [The CSS Anthology: 101 Essentials Tips, Tricks, &amp; Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), The `<ul>` und `<li>` Elemente sind so formatiert, dass das Markup die folgende visuelle Ausgabe erzeugt:

![Ein aus zwei repeattoren und CSS zusammengesetztes Menü](master-pages-and-site-navigation-vb/_static/image27.png)

**Abbildung 11**: ein Menü aus zwei repeatatoren und einigen CSS

Dieses Menü befindet sich auf der Master Seite und ist an die in `Web.sitemap`definierte Site Map gebunden. Dies bedeutet, dass jede Änderung an der Site Übersicht auf allen Seiten, auf denen die `Site.master` Master Seite verwendet wird, sofort widergespiegelt wird.

## <a name="disabling-viewstate"></a>"ViewState" wird deaktiviert.

Alle ASP.NET-Steuerelemente können Ihren Zustand optional in den [Ansichts Zustand](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/)einbewahren, der als ausgeblendetes Formularfeld im gerenderten HTML serialisiert wird. Der Ansichts Zustand wird von Steuerelementen verwendet, um den Programm gesteuert geänderten Zustand über Postbacks hinweg zu merken, z. b. die Daten, die an ein datenweb Steuerelement gebunden sind Während der Ansichts Zustand das Senden von Informationen über Postbacks zulässt, erhöht sich die Größe des Markups, das an den Client gesendet werden muss, und kann zu einem schweren seitenbloat führen, wenn es nicht genau überwacht wird. Datenweb Steuerelemente, insbesondere die GridView, sind besonders für das Hinzufügen von Dutzenden zusätzlicher Kilobyte von Markup zu einer Seite berüchtigt. Obwohl eine solche Zunahme für Breitband-oder Intranetbenutzer unerheblich sein kann, kann der Ansichts Zustand dem Roundtrip für Einwählbenutzer mehrere Sekunden hinzufügen.

Um die Auswirkung des Ansichts Zustands anzuzeigen, rufen Sie eine Seite in einem Browser auf, und zeigen Sie dann die von der Webseite gesendete Quelle an (Klicken Sie in Internet Explorer auf das Menü Ansicht, und wählen Sie die Option Quelle aus). Sie können die Seiten Ablauf [Verfolgung](https://msdn.microsoft.com/library/sfbfw58f.aspx) auch aktivieren, um die Ansichts Zustands Zuordnung anzuzeigen, die von den einzelnen Steuerelementen auf der Seite verwendet wird. Die Ansichts Zustandsinformationen werden in einem ausgeblendeten Formularfeld namens `__VIEWSTATE`serialisiert, das sich in einem `<div>`-Element unmittelbar nach dem öffnenden `<form>`-Tag befindet. Der Ansichts Zustand wird nur beibehalten, wenn ein Webformular verwendet wird. Wenn die ASP.NET-Seite keine `<form runat="server">` in der deklarativen Syntax enthält, gibt es kein `__VIEWSTATE` ausgeblendetes Formularfeld im gerenderten Markup.

Das von der Master Seite generierte `__VIEWSTATE` Formularfeld fügt dem generierten Markup der Seite ungefähr 1.800 Bytes hinzu. Dieser zusätzliche Überlastung liegt hauptsächlich an dem Repeater-Steuerelement, da der Inhalt des SiteMapDataSource-Steuer Elements dauerhaft im Ansichts Zustand gespeichert wird. Während eine zusätzliche Größe von 1.800 Bytes möglicherweise nicht so groß wie möglich erscheint, wenn Sie eine GridView mit vielen Feldern und Datensätzen verwenden, kann der Ansichts Zustand leicht um einen Faktor von 10 oder mehr ansteigen.

Der Ansichts Zustand kann auf Seiten-oder Steuerelement Ebene deaktiviert werden, indem die `EnableViewState`-Eigenschaft auf `False`festgelegt wird, wodurch die Größe des gerenderten Markups reduziert wird. Da der Ansichts Zustand für ein datenweb-Steuerelement die Daten, die an das datenweb Steuerelement gebunden sind, über Postbacks hinweg speichert, müssen die Daten bei der Deaktivierung des Ansichts Zustands für ein datenweb-Steuerelement an jedem und jedem Postback gebunden werden. In ASP.net, Version 1. x, fiel diese Verantwortung auf den Schultern des Seiten Entwicklers. mit ASP.NET 2,0 werden die datenweb Steuerelemente jedoch bei Bedarf bei jedem Postback an das Datenquellen-Steuerelement gebunden.

Um den Ansichts Zustand der Seite zu verringern, legen Sie die `EnableViewState`-Eigenschaft des Repeater-Steuer Elements auf `False`fest. Dies kann über die Eigenschaftenfenster im Designer oder deklarativ in der Quell Ansicht erfolgen. Nachdem Sie diese Änderung vorgenommen haben, sollte das deklarative Markup des Repeater wie folgt aussehen:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

Nach dieser Änderung wurde die gerenderte Ansichts Zustands Größe der Seite auf eine Länge von 52 Byte reduziert, eine 97% Ersparnis in der Ansichts Zustands Größe! In den Tutorials in dieser Reihe wird der Ansichts Zustand der datenweb Steuerelemente standardmäßig deaktiviert, um die Größe des gerenderten Markups zu verringern. In den meisten Beispielen wird die `EnableViewState`-Eigenschaft auf `False` und ohne Erwähnung festgelegt. Der einzige Zeitpunkt, in dem der Ansichts Zustand erläutert wird, ist in Szenarien, in denen er aktiviert werden muss, damit das Daten-websteuer Element seine erwartete Funktionalität bereitstellen kann.

## <a name="step-4-adding-breadcrumb-navigation"></a>Schritt 4: Hinzufügen der Breadcrumb-Navigation

Zum Vervollständigen der Master Seite fügen wir eine Breadcrumb-Navigations-UI-Element zu jeder Seite hinzu. Breadcrumb zeigt Benutzern schnell die aktuelle Position innerhalb der Standort Hierarchie an. Das Hinzufügen eines Breadcrumb in ASP.NET 2,0 ist einfach, indem Sie der Seite einfach ein SiteMapPath-Steuerelement hinzufügen. Es ist kein Code erforderlich.

Fügen Sie für unsere Website das Steuerelement dem Header `<div>`hinzu:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

Breadcrumb zeigt die aktuelle Seite, die der Benutzer in der Site Übersichts Hierarchie besucht, sowie die "Vorgänger" des Site Übersichts Knotens bis zum Stamm (Home, in unserer Site Übersicht).

![Breadcrumb zeigt die aktuelle Seite und deren Vorgänger in der Hierarchie der Site Map an](master-pages-and-site-navigation-vb/_static/image28.png)

**Abbildung 12**: Breadcrumb zeigt die aktuelle Seite und deren Vorgänger in der Hierarchie der Site Map an

## <a name="step-5-adding-the-default-page-for-each-section"></a>Schritt 5: Hinzufügen der Standardseite für jeden Abschnitt

Die Tutorials auf unserer Website sind in verschiedene Kategorien unterteilt: grundlegende Berichterstellung, Filterung, benutzerdefinierte Formatierung usw. mit einem Ordner für jede Kategorie und den entsprechenden Tutorials als ASP.net pages in diesem Ordner. Außerdem enthält jeder Ordner eine `Default.aspx` Seite. Für diese Standardseite werden alle Tutorials für den aktuellen Abschnitt angezeigt. Das heißt, für die `Default.aspx` im `BasicReporting` Ordner enthalten wir Links zu `SimpleDisplay.aspx`, `DeclarativeParams.aspx`und `ProgrammaticParams.aspx`. Hier können wir wieder die `SiteMap`-Klasse und ein datenweb-Steuerelement verwenden, um diese Informationen auf der Grundlage der in `Web.sitemap`definierten Site Map anzuzeigen.

Wir zeigen erneut eine ungeordnete Liste mit einem Wiederholungs Modul an, aber dieses Mal werden der Titel und die Beschreibung der Tutorials angezeigt. Da das Markup und der Code zu diesem Zweck für jede `Default.aspx` Seite wiederholt werden müssen, können wir diese Benutzeroberflächen Logik in einem [Benutzer Steuer](https://msdn.microsoft.com/library/y6wb1a0e.aspx)Element Kapseln. Erstellen Sie auf der Website einen Ordner mit dem Namen `UserControls`, und fügen Sie diesem ein neues Element vom Typ Web-Benutzer Steuerelement mit dem Namen `SectionLevelTutorialListing.ascx`hinzu. Fügen Sie das folgende Markup hinzu:

[![dem Ordner "UserControls" ein neues Webbenutzer Steuerelement hinzufügen](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**Abbildung 13**: Hinzufügen eines neuen Webbenutzer Steuer Elements zum Ordner "`UserControls`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image31.png))

Sectionleveltutoriallisting. ascx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

Sectionleveltutoriallisting. ascx. vb

[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

Im vorherigen Wiederholungs Beispiel haben wir die `SiteMap` Daten deklarativ an den Repeater gebunden. Das `SectionLevelTutorialListing` Benutzer Steuer Elements führt dies jedoch Programm gesteuert aus. Im Ereignishandler `Page_Load` wird überprüft, ob die URL dieser Seite einem Knoten in der Site Übersicht zugeordnet wird. Wenn dieses Benutzer Steuerelement auf einer Seite verwendet wird, die nicht über einen entsprechenden `<siteMapNode>` Eintrag verfügt, wird `SiteMap.CurrentNode` `Nothing` zurückgegeben, und es werden keine Daten an den Repeater gebunden. Wenn wir eine `CurrentNode`haben, binden wir die `ChildNodes` Auflistung an den Repeater. Da unsere Site Übersicht so eingerichtet ist, dass die `Default.aspx` Seite in jedem Abschnitt der übergeordnete Knoten aller Tutorials in diesem Abschnitt ist, zeigt dieser Code Links zu und Beschreibungen aller Tutorials des Abschnitts an, wie im Screenshot unten gezeigt.

Nachdem dieser Wiederholungs Vorgang erstellt wurde, öffnen Sie die `Default.aspx` Seiten in jedem Ordner, wechseln Sie zum Designansicht, und ziehen Sie das Benutzer Steuerelement einfach vom Projektmappen-Explorer auf die Entwurfs Oberfläche, auf der die Liste der Tutorials angezeigt werden soll.

[![wird das Benutzer Steuerelement zu "default. aspx" hinzugefügt.](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**Abbildung 14**: das Benutzer Steuerelement wurde `Default.aspx` hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image34.png))

[![die grundlegenden Tutorials für die Berichterstellung aufgeführt sind.](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**Abbildung 15**: die grundlegenden Tutorials für die Berichterstellung sind aufgeführt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-site-navigation-vb/_static/image37.png))

## <a name="summary"></a>Summary

Nachdem die Site Map definiert und die Master Seite fertiggestellt wurde, verfügen wir nun über ein konsistentes Seitenlayout und Navigations Schema für unsere datenbezogenen Tutorials. Unabhängig davon, wie viele Seiten wir unserer Website hinzufügen, ist das Aktualisieren des Website Layouts oder der Informationen zur Website Navigation ein schneller und einfacher Prozess, weil diese Informationen zentralisiert werden. Insbesondere werden die Seitenlayoutinformationen auf der Master Seite `Site.master` und die Site Map in `Web.sitemap`definiert. Wir mussten *keinen* Code schreiben, um dieses Website weite Seitenlayout und Navigations Mechanismus zu erreichen, und wir behalten die Unterstützung für den vollständigen WYSIWYG-Designer in Visual Studio bei.

Nachdem Sie die Datenzugriffs Schicht und die Geschäftslogik Schicht abgeschlossen und ein konsistentes Seitenlayout und eine Website Navigation definiert haben, können Sie mit dem untersuchen allgemeiner Berichts Muster beginnen. In den nächsten drei Tutorials betrachten wir grundlegende Berichterstattungs Aufgaben, die Daten anzeigen, die aus der BLL in den Steuerelementen GridView, DetailsView und FormView abgerufen wurden.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Übersicht über ASP.net-Master Seiten](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Master Seiten in ASP.NET 2,0](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2,0-Entwurfsvorlagen](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [ASP.net Übersicht über die Site Navigation](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Überprüfen der Site Navigation in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2,0 Site Navigations Features](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Grundlegendes zum Ansichts Zustand ASP.net](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Vorgehensweise: Aktivieren der Ablauf Verfolgung für eine ASP.NET-Seite](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET-Benutzer Steuerelemente](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Liz shulok, Dennis Patterson und Hilton Giesenow. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorheriges](creating-a-business-logic-layer-vb.md)
