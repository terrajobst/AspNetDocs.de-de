---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Anzeigen von Daten mit den DataList-und RepeaterC#-Steuerelementen () | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir das GridView-Steuerelement verwendet, um Daten anzuzeigen. Ab diesem Tutorial untersuchen wir die Erstellung allgemeiner Berichts Muster mit...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 09d3faf811f21a66bb5c234f71d77b2552ae6516
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74623169"
---
# <a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Anzeigen von Daten mit dem DataList- und Wiederholungssteuerelement (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) oder [PDF herunterladen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> In den vorherigen Tutorials haben wir das GridView-Steuerelement verwendet, um Daten anzuzeigen. Ab diesem Tutorial untersuchen wir die Erstellung allgemeiner Berichts Muster mit den Steuerelementen DataList und Repeater, beginnend mit den Grundlagen der Anzeige von Daten mit diesen Steuerelementen.

## <a name="introduction"></a>Einführung

In allen Beispielen in den letzten 28 Tutorials haben wir, wenn wir mehrere Datensätze aus einer Datenquelle anzeigen mussten, das GridView-Steuerelement aktiviert. Die GridView rendert eine Zeile für jeden Datensatz in der Datenquelle und zeigt die Datenfelder Datensatz s in Spalten an. In der GridView ist es zwar ein Snap-in zum Anzeigen, durchlaufen, sortieren, bearbeiten und Löschen von Daten, aber das Aussehen ist ein bitboxy. Außerdem ist das Markup, das für die Struktur von GridView s verantwortlich ist, so festgelegt, dass es eine HTML-`<table>` mit einer Tabellenzeile (`<tr>`) für jeden Datensatz und eine Tabellenzelle (`<td>`) für jedes Feld enthält.

Um bei der Anzeige mehrerer Datensätze ein höheres Maß an Anpassung an die Darstellung und das gerenderte Markup bereitzustellen, bietet ASP.NET 2,0 die DataList-und Repeater-Steuerelemente (beide sind auch in ASP.NET Version 1. x verfügbar). Die Steuerelemente DataList und Repeater erzeugen ihren Inhalt mithilfe von Vorlagen anstelle von boundfields, checkboxfields, buttonfields usw. Wie bei GridView rendert der DataList als HTML-`<table>`, ermöglicht jedoch das Anzeigen mehrerer Datensätze von Datenquellen pro Tabellenzeile. Der Repeater rendert hingegen kein zusätzliches Markup als das, was Sie explizit angeben, und ist ein idealer Kandidat, wenn Sie eine genaue Kontrolle über das ausgegebene Markup benötigen.

Im Laufe der nächsten Dutzend Lernprogramme betrachten wir das Erstellen allgemeiner Bericht Erstellungs Muster mit den Steuerelementen DataList und Repeater, beginnend mit den Grundlagen der Anzeige von Daten mit diesen Steuerelement Vorlagen. Wir werden Ihnen zeigen, wie Sie diese Steuerelemente formatieren, wie Sie das Layout von Datenquellen Datensätzen in den DataList-, allgemeinen Master-/detailszenarien, Möglichkeiten zum Bearbeiten und Löschen von Daten, zum Durchlaufen von Datensätzen usw. ändern.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Webseiten "DataList" und "Repeater Tutorial"

Bevor wir mit diesem Tutorial beginnen, nehmen wir uns einen Moment Zeit, um die ASP.NET-Seiten hinzuzufügen, die für dieses Tutorial benötigt werden, und die nächsten Tutorials, in denen die Anzeige von Daten mit dem DataList und Repeater behandelt wird. Erstellen Sie zunächst einen neuen Ordner in dem Projekt mit dem Namen `DataListRepeaterBasics`. Fügen Sie anschließend die folgenden fünf ASP.NET-Seiten zu diesem Ordner hinzu, wobei alle für die Verwendung der Master Seite `Site.master`konfiguriert sind:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`

![Erstellen eines Ordners "datalistrepaterbasics" und Hinzufügen des Tutorials ASP.NET Seiten](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Abbildung 1**: Erstellen eines `DataListRepeaterBasics` Ordners und Hinzufügen des Tutorials ASP.NET Seiten

Öffnen Sie die Seite `Default.aspx`, und ziehen Sie das Steuerelement `SectionLevelTutorialListing.ascx` Benutzer aus dem Ordner `UserControls` auf die Entwurfs Oberfläche. Dieses Benutzer Steuerelement, das wir im Lernprogramm zu [Master Seiten und zur Website Navigation](../introduction/master-pages-and-site-navigation-cs.md) erstellt haben, listet die Site Map auf und zeigt die Tutorials aus dem aktuellen Abschnitt in einer Aufzählung an.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Abbildung 2**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))

Damit in der Auflistungs Liste die Lernprogramme DataList und Repeater angezeigt werden, die wir erstellen, müssen wir Sie der Site Map hinzufügen. Öffnen Sie die Datei `Web.sitemap`, und fügen Sie das folgende Markup nach dem Hinzufügen von benutzerdefinierten Schaltflächen Site Übersichts Knoten Markup hinzu:

[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]

![Aktualisieren der Site Übersicht, um die neuen ASP.NET-Seiten einzuschließen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Abbildung 3**: Aktualisieren der Site Übersicht, um die neuen ASP.NET-Seiten einzuschließen

## <a name="step-2-displaying-product-information-with-the-datalist"></a>Schritt 2: Anzeigen von Produktinformationen mit dem DataList

Ähnlich wie bei FormView hängt das Rendern des DataList-Steuer Elements von Vorlagen anstelle von boundfields, checkboxfields usw. ab. Anders als bei FormView ist der DataList so konzipiert, dass eine Reihe von Datensätzen anstelle eines Eintrags angezeigt werden. Beginnen Sie dieses Tutorial mit einem Blick auf das Binden von Produktinformationen an einen DataList. Öffnen Sie zunächst die Seite `Basics.aspx` im Ordner `DataListRepeaterBasics`. Ziehen Sie als nächstes einen DataList aus der Toolbox auf den Designer. Wie in Abbildung 4 dargestellt, zeigt der Designer vor der Angabe der Vorlagen für DataList s als graues Feld an.

[![den DataList aus der Toolbox auf den Designer ziehen.](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Abbildung 4**: Ziehen des DataList-Elements aus der Toolbox auf den Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))

Fügen Sie vom Smarttags DataList s eine neue ObjectDataSource hinzu, und konfigurieren Sie Sie so, dass Sie die `ProductsBLL` Class s `GetProducts`-Methode verwendet. Da wir in diesem Tutorial erneut einen schreibgeschützten DataList erstellen, legen Sie die Dropdown Liste auf den Registerkarten einfügen, aktualisieren und Löschen des Assistenten auf (keine) fest.

[![Erstellen einer neuen ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbll-Klasse.](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Abbildung 6**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))

[![Informationen über alle Produkte mithilfe der GetProducts-Methode abrufen.](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Abbildung 7**: Abrufen von Informationen über alle Produkte mithilfe der `GetProducts`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))

Nachdem Sie die ObjectDataSource konfiguriert und dem DataList über das Smarttag zugeordnet haben, erstellt Visual Studio automatisch ein `ItemTemplate` in der DataList, das den Namen und den Wert der einzelnen von der Datenquelle zurückgegebenen Datenfelder anzeigt (siehe das Markup unten). Diese standardmäßige `ItemTemplate` s-Darstellung ist mit der der Vorlagen identisch, die automatisch erstellt werden, wenn eine Datenquelle über den Designer an die Form View gebunden wird.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Denken Sie daran, dass Visual Studio beim Binden einer Datenquelle an ein FormView-Steuerelement über das FormView s-Smarttag eine `ItemTemplate`, `InsertItemTemplate`und `EditItemTemplate`erstellt hat. Beim DataList-DataList wird jedoch nur ein `ItemTemplate` erstellt. Dies liegt daran, dass der DataList nicht die gleiche integrierte Bearbeitungs-und einfügeunterstützung hat, die von FormView geboten wird. Der DataList enthält Ereignisse, die auf Bearbeitung und Löschung bezogen sind, und das Bearbeiten und Löschen von Unterstützung kann mit etwas Code hinzugefügt werden. es gibt jedoch keine einfache, sofort eingefügte Unterstützung wie bei der FormView. In einem zukünftigen Tutorial wird erläutert, wie Sie die Unterstützung für die Bearbeitung und Löschung mit dem DataList einschließen.

Nehmen Sie sich einen Moment Zeit, um die Darstellung dieser Vorlage zu verbessern. Anstatt alle Datenfelder anzuzeigen, dürfen nur die Produkte Name, Lieferant, Category, Menge pro Einheit und Einheitspreis angezeigt werden. Außerdem wird der Name in einer `<h4>` Überschrift angezeigt, und die restlichen Felder werden mithilfe eines `<table>` unterhalb der Überschrift festgelegt.

Um diese Änderungen vorzunehmen, können Sie entweder die Vorlagen Bearbeitungsfunktionen im Designer aus dem DataList s-Smarttag verwenden. Klicken Sie auf den Link Vorlagen bearbeiten, oder ändern Sie die Vorlage manuell über die deklarative Syntax der Seite. Wenn Sie die Option Vorlagen bearbeiten im Designer verwenden, stimmt das resultierende Markup möglicherweise nicht exakt mit dem folgenden Markup überein, aber wenn es über einen Browser angezeigt wird, sollte der Screenshot in Abbildung 8 sehr ähnlich aussehen.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> Das obige Beispiel verwendet Label-websteuer Elemente, deren `Text`-Eigenschaft dem Wert der Datenbindung-Syntax zugewiesen wird. Alternativ könnten wir die Bezeichnungen ganz weggelassen haben und nur die Datenbindung-Syntax eingeben. Das heißt, anstelle von `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` wir stattdessen die deklarative Syntax `<%# Eval("CategoryName") %>`verwenden können.

Das belassen der websteuer Elemente der Bezeichnung bietet jedoch zwei Vorteile. Erstens bietet es eine einfachere Möglichkeit, die Daten auf der Grundlage der Daten zu formatieren, wie im nächsten Tutorial zu sehen ist. Zweitens zeigt die Option Vorlagen bearbeiten im Designer keine deklarative Datenbindung-Syntax an, die außerhalb eines websteuer Elements angezeigt wird. Stattdessen ist die Schnittstelle zum Bearbeiten von Vorlagen so konzipiert, dass die Arbeit mit statischen Markup-und websteuer Elementen vereinfacht wird. dabei wird davon ausgegangen, dass die Datenbindung über das Dialogfeld "DataBindings bearbeiten" erfolgt

Wenn Sie mit dem DataList arbeiten, das die Option zum Bearbeiten der Vorlagen über den Designer bietet, bevorzugen Sie daher die Bezeichnung websteuer Elemente, damit der Inhalt über die Edit Templates-Schnittstelle zugänglich ist. Wie wir in Kürze sehen werden, erfordert der Repeater, dass der Inhalt der Vorlage in der Quell Ansicht bearbeitet wird. Folglich wird beim Erstellen der Repeater s-Vorlagen die Bezeichnung websteuer Elemente häufig weggelassen, es sei denn, ich weiß, dass ich die Darstellung des Daten gebundenen Texts basierend auf der programmgesteuerten Logik formatieren muss.

[![die Ausgabe jedes Produkts wird mithilfe des DataList s ItemTemplate gerendert.](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Abbildung 8**: die Ausgabe jedes Produkts wird mithilfe der DataList s `ItemTemplate` gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))

## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Schritt 3: verbessern der Darstellung des DataList-Steuerelemente

Wie bei GridView bietet der DataList eine Reihe von Eigenschaften im Stil, wie z. b. `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`usw. Beim Arbeiten mit den GridView-und DetailsView-Steuerelementen haben wir Skin-Dateien im `DataWebControls` Design erstellt, die die `CssClass` Eigenschaften für diese beiden Steuerelemente und die `CssClass`-Eigenschaft für einige ihrer untergeordneten Eigenschaften (`RowStyle`, `HeaderStyle`usw.) vordefiniert haben. Das gleiche gilt für den DataList.

Wie im Tutorial [Anzeigen von Daten mit dem ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) -Tutorial erläutert, gibt eine Skin-Datei die Standardeigenschaften für die Darstellung eines websteuer Elements an. ein Design ist eine Sammlung von Skin-, CSS-, Bild-und JavaScript-Dateien, die ein bestimmtes Aussehen und Gefühl für eine Website definieren. Im Tutorial *Anzeigen von Daten mit dem ObjectDataSource* -Tutorial haben wir ein `DataWebControls` Design erstellt (das als Ordner innerhalb des Ordners "`App_Themes`" implementiert ist), in dem derzeit zwei Skin-Dateien `GridView.skin` und `DetailsView.skin`sind. Fügen Sie eine dritte Skin-Datei hinzu, um die vordefinierten Stileinstellungen für den DataList anzugeben.

Zum Hinzufügen einer Skin-Datei klicken Sie mit der rechten Maustaste auf den Ordner `App_Themes/DataWebControls`, wählen Sie neues Element hinzufügen aus, und wählen Sie in der Liste die Option Skin File aus. Nennen Sie die Datei `DataList.skin`.

[![eine neue Skin-Datei mit dem Namen "DataList. Skin" erstellen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Abbildung 9**: Erstellen einer neuen Skin-Datei mit dem Namen "`DataList.skin`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))

Verwenden Sie das folgende Markup für die `DataList.skin`-Datei:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Diese Einstellungen weisen die gleichen CSS-Klassen den entsprechenden DataList-Eigenschaften zu, die in den GridView-und DetailsView-Steuerelementen verwendet wurden. Die hier verwendeten CSS-Klassen `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`usw. werden in der `Styles.css` Datei definiert und in den vorherigen Tutorials hinzugefügt.

Durch das Hinzufügen dieser Skin-Datei wird die Darstellung des DataList-s im Designer aktualisiert (möglicherweise müssen Sie die Designer Ansicht aktualisieren, um die Auswirkungen der neuen Skin-Datei zu sehen; wählen Sie im Menü Ansicht die Option aktualisieren aus). Wie in Abbildung 10 gezeigt, verfügt jedes abwechselnde Produkt über eine helle Rosa Hintergrundfarbe.

[![eine neue Skin-Datei mit dem Namen "DataList. Skin" erstellen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Abbildung 10**: Erstellen einer neuen Skin-Datei mit dem Namen "`DataList.skin`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))

## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Schritt 4: Untersuchen der anderen Vorlagen des DataList-s

Zusätzlich zum `ItemTemplate`unterstützt der DataList sechs weitere optionale Vorlagen:

- `HeaderTemplate` wenn bereitgestellt, fügt der Ausgabe eine Kopfzeile hinzu und wird zum Rendern dieser Zeile verwendet.
- `AlternatingItemTemplate` zum Rendering abwechselnder Elemente verwendet.
- `SelectedItemTemplate`, der zum Rendering des ausgewählten Elements verwendet wird. das ausgewählte Element ist das Element, dessen Index der DataList s- [`SelectedIndex`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx) entspricht.
- `EditItemTemplate`, das zum Rendering des bearbeiteten Elements verwendet wird.
- Wenn bereitgestellt, wird ein Trennzeichen zwischen jedem Element hinzugefügt und zum Rendering dieses Trenn Zeichens verwendet. `SeparatorTemplate`
- `FooterTemplate`: Wenn bereitgestellt, fügt der Ausgabe eine Footerzeile hinzu und wird zum renderingwert dieser Zeile verwendet.

Wenn Sie die `HeaderTemplate` oder `FooterTemplate`angeben, fügt der DataList der gerenderten Ausgabe eine zusätzliche Kopf-oder Fußzeile hinzu. Wie bei den Kopf-und Fußzeilen Zeilen von GridView sind die Kopfzeile und die Fußzeile in einem DataList nicht an Daten gebunden. Folglich gibt eine beliebige Datenbindung-Syntax in der `HeaderTemplate` oder `FooterTemplate`, die versucht, auf gebundene Daten zuzugreifen, eine leere Zeichenfolge zurück.

> [!NOTE]
> Wie im Tutorial zum [Anzeigen von Zusammenfassungs Informationen im GridView-Fußzeile](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) -Lernprogramm gezeigt, können die Kopf-und footerzeilen die Datenbindung-Syntax direkt in diese Zeilen einfügen, und zwar aus dem GridView s `RowDataBound`-Ereignishandler. Diese Technik kann verwendet werden, um sowohl die laufenden Summen als auch andere Informationen aus den Daten zu berechnen, die an das Steuerelement gebunden sind, und diese Informationen der Fußzeile zuzuweisen. Das gleiche Konzept kann auf die Steuerelemente DataList und Repeater angewendet werden. der einzige Unterschied besteht darin, dass für DataList und Repeater ein Ereignishandler für das `ItemDataBound`-Ereignis erstellt wird (anstelle von für das `RowDataBound`-Ereignis).

In unserem Beispiel soll der Titel Product Information an der Spitze der DataList s-Ergebnisse in einer `<h3>` Überschrift angezeigt werden. Fügen Sie hierfür eine `HeaderTemplate` mit dem entsprechenden Markup hinzu. Klicken Sie hierzu im Designer auf den Link Vorlagen bearbeiten im Smarttags DataList s, wählen Sie die Header Vorlage aus der Dropdown Liste aus, und geben Sie den Text ein, nachdem Sie die Option Überschrift 3 aus der Dropdown Liste Style ausgewählt haben (siehe Abbildung 11).

[![eine HeaderTemplate mit den Text Produktinformationen hinzufügen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Abbildung 11**: Hinzufügen einer `HeaderTemplate` mit den Text Produktinformationen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))

Alternativ kann dies deklarativ hinzugefügt werden, indem Sie das folgende Markup innerhalb der `<asp:DataList>` Tags eingeben:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Fügen Sie eine `SeparatorTemplate` hinzu, die eine Linie zwischen den einzelnen Abschnitten enthält, um zwischen den einzelnen Produktlisten ein wenig Platz hinzuzufügen. Das horizontale regeltag (`<hr>`) fügt einen solchen unter Teiler hinzu. Erstellen Sie die `SeparatorTemplate`, sodass Sie das folgende Markup hat:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Die `SeparatorTemplate` ist wie die `HeaderTemplate` und `FooterTemplates`nicht an einen Datensatz aus der Datenquelle gebunden und kann daher nicht direkt auf die Datenquellen Datensätze zugreifen, die an den DataList gebunden sind.

Wenn Sie die Seite über einen Browser anzeigen, sollte Sie in etwa wie in Abbildung 12 aussehen. Notieren Sie sich die Kopfzeile und die Zeile zwischen den einzelnen Produktlisten.

[![DataList enthält eine Kopfzeile und eine horizontale Regel zwischen den einzelnen Produktlisten.](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Abbildung 12**: DataList enthält eine Kopfzeile und eine horizontale Regel zwischen den einzelnen Produktlisten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))

## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Schritt 5: Rendern eines bestimmten Markups mit dem Repeater-Steuerelement

Wenn Sie beim Aufrufen des DataList-Beispiels aus Abbildung 12 eine Ansicht/Quelle aus dem Browser ausführen, sehen Sie, dass der DataList eine HTML-`<table>` ausgibt, die eine Tabellenzeile (`<tr>`) mit einer einzigen Tabellenzelle (`<td>`) für jedes an den DataList gebundene Element enthält. Diese Ausgabe ist tatsächlich identisch mit dem, was von einem GridView-Element mit einem einzelnen TemplateField ausgegeben wird. Wie wir in einem zukünftigen Tutorial sehen werden, lässt der DataList eine weitere Anpassung der Ausgabe zu, sodass wir mehrere Datenquellen Datensätze pro Tabellenzeile anzeigen können.

Was geschieht, wenn Sie keine HTML-`<table>`ausgeben möchten? Um das von einem datenweb Steuerelement generierte Markup vollständig und vollständig zu steuern, müssen wir das Repeater-Steuerelement verwenden. Wie der DataList wird der Repeater basierend auf Vorlagen erstellt. Der Repeater bietet jedoch nur die folgenden fünf Vorlagen:

- Fügt `HeaderTemplate` wenn bereitgestellt, das angegebene Markup vor den Elementen hinzu.
- `ItemTemplate` zum Rendering von Elementen verwendet.
- `AlternatingItemTemplate` wenn bereitgestellt, wird zum Rendering abwechselnder Elemente verwendet.
- Fügt `SeparatorTemplate` wenn bereitgestellt, das angegebene Markup zwischen den einzelnen Elementen hinzu.
- `FooterTemplate`: Wenn bereitgestellt, wird das angegebene Markup nach den Elementen hinzugefügt.

In ASP.NET 1. x wurde das Repeater-Steuerelement häufig zum Anzeigen einer aufzurufenen Liste verwendet, deren Daten aus einer Datenquelle stammen. In diesem Fall enthalten die `HeaderTemplate` und `FooterTemplates` die öffnenden und schließenden `<ul>` Tags, während das `ItemTemplate` `<li>` Elemente mit Datenbindung-Syntax enthalten würde. Dieser Ansatz kann weiterhin in ASP.NET 2,0 verwendet werden, wie in den beiden Beispielen im Lernprogramm [Master Pages und Website Navigation](../introduction/master-pages-and-site-navigation-cs.md) gezeigt:

- Auf der `Site.master` Master Seite wurde ein Repeater verwendet, um eine Auflistungs Liste mit dem Inhalt der obersten Ebene der Site Map anzuzeigen (grundlegende Berichterstellung, Filtern von Berichten, angepasste Formatierung usw.). ein weiterer, ein geklichter wiederholter Repeater wurde verwendet, um die untergeordneten Abschnitte der Abschnitte der obersten Ebene anzuzeigen.
- In `SectionLevelTutorialListing.ascx`wurde ein Repeater verwendet, um eine Auflistung der untergeordneten Abschnitte des Abschnitts "aktuelle Site Map" anzuzeigen.

> [!NOTE]
> ASP.NET 2,0 führt das neue [Steuerelement "BulletedList](https://msdn.microsoft.com/library/ms228101.aspx)" ein, das an ein Datenquellen-Steuerelement gebunden werden kann, um eine einfache Auflistungs Liste anzuzeigen. Mit dem Steuerelement "BulletedList" müssen wir keinen der Listen bezogenen HTML-Code angeben. Stattdessen geben wir einfach das Datenfeld an, das als Text für die einzelnen Listenelemente angezeigt werden soll.

Der Repeater fungiert als Catch All Data-websteuer Element. Wenn kein vorhandenes Steuerelement vorhanden ist, das das benötigte Markup generiert, kann das Repeater-Steuerelement verwendet werden. Um die Verwendung des Repeater zu veranschaulichen, lassen Sie die Liste der Kategorien über dem in Schritt 2 erstellten DataList in den Produktinformationen anzeigen. Die Kategorien der einzelnen Zeilen werden insbesondere in HTML-`<table>` angezeigt, wobei jede Kategorie als Spalte in der Tabelle angezeigt wird.

Um dies zu erreichen, ziehen Sie zunächst ein Repeater-Steuerelement aus der Toolbox auf den Designer oberhalb der Product Information DataList. Wie beim DataList wird der Repeater zunächst als graues Feld angezeigt, bis seine Vorlagen definiert wurden.

[![Hinzufügen eines Wiederholungs Moduls zum Designer](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Abbildung 13**: Hinzufügen eines Wiederholungs Moduls zum Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))

Es gibt nur eine Option im "Repeater s smartag": Wählen Sie "Datenquelle". Erstellen Sie eine neue ObjectDataSource, und konfigurieren Sie Sie so, dass Sie die `CategoriesBLL` Class s `GetCategories`-Methode verwendet.

[Erstellen eines neuen ObjectDataSource-![](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Abbildung 14**: Erstellen einer neuen ObjectDataSource ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))

[![Konfigurieren von ObjectDataSource für die Verwendung der Klasse "kategoriesbll"](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Abbildung 15**: Konfigurieren von ObjectDataSource für die Verwendung der `CategoriesBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))

[![Informationen zu allen Kategorien mithilfe der GetCategories-Methode abrufen.](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Abbildung 16**: Abrufen von Informationen zu allen Kategorien mit der `GetCategories`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))

Im Gegensatz zum DataList erstellt Visual Studio nicht automatisch ein ItemTemplate-Element für das Wiederholungs Modul, nachdem es an eine Datenquelle gebunden wurde. Darüber hinaus können die Repeater s-Vorlagen nicht über den Designer konfiguriert werden und müssen deklarativ angegeben werden.

Um die Kategorien als einzeilige `<table>` mit einer Spalte für jede Kategorie anzuzeigen, muss der Repeater ein Markup ähnlich dem folgenden ausgeben:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Da der `<td>Category X</td>` Text der Teil ist, der wiederholt wird, wird dieser in den repeatater s ItemTemplate angezeigt. Das Markup, das vor dem `<table><tr>` angezeigt wird, wird in der `HeaderTemplate` platziert, während das endmarkup `</tr></table>` in der `FooterTemplate`platziert wird. Um diese Vorlagen Einstellungen einzugeben, wechseln Sie zum deklarativen Teil der ASP.NET-Seite, indem Sie in der unteren linken Ecke auf die Quell Schaltfläche klicken, und geben Sie die folgende Syntax ein:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Der Repeater gibt das genaue Markup wie in den Vorlagen angegeben an, nichts weiter, nichts weiter. Abbildung 17 zeigt die Ausgabe des repeatater s, wenn Sie in einem Browser angezeigt wird.

[![einer HTML-&lt;Tabelle mit einer Zeile,&gt; jede Kategorie in einer separaten Spalte auflistet](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Abbildung 17**: eine einzeilige HTML-`<table>` listet jede Kategorie in einer separaten Spalte auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))

## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Schritt 6: verbessern der Darstellung des Wiederholungs Moduls

Da der Repeater genau das Markup ausgibt, das von seinen Vorlagen festgelegt wird, sollte es nicht verwunderlich sein, dass es keine Stil bezogenen Eigenschaften für den Repeater gibt. Um die Darstellung des vom Repeater generierten Inhalts zu ändern, müssen wir die erforderlichen HTML-oder CSS-Inhalte direkt den repeatater s-Vorlagen hinzufügen.

In unserem Beispiel haben wir die Kategoriespalten alternative Hintergrundfarben, wie z. b. die wechselnden Zeilen in der DataList. Um dies zu erreichen, müssen wir jedem Repeater-Element und der `AlternatingRowStyle` CSS-Klasse jedes abwechselnde Wiederholungs-Element über die `ItemTemplate`-und `AlternatingItemTemplate`-Vorlagen die `RowStyle` CSS-Klasse zuweisen, wie im folgenden Beispiel:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Außerdem wird der Ausgabe mit den Text Produktkategorien eine Kopfzeile hinzugefügt. Da wir nicht wissen, wie viele Spalten das resultierende `<table>` enthalten wird, ist die einfachste Möglichkeit, eine Kopfzeile zu generieren, die garantiert alle Spalten umfasst, *zwei* `<table>` s zu verwenden. Der erste `<table>` enthält zwei Zeilen der Kopfzeile und eine Zeile, die die zweite, einzeilige `<table>` enthalten, die für jede Kategorie im System eine Spalte enthält. Das heißt, wir möchten das folgende Markup ausgeben:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Die folgenden `HeaderTemplate` und `FooterTemplate` führen zu dem gewünschten Markup:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

In Abbildung 18 wird der Repeater angezeigt, nachdem diese Änderungen vorgenommen wurden.

[![die Kategoriespalten Alternative in der Hintergrundfarbe und enthält eine Kopfzeile.](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Abbildung 18**: die Kategoriespalten Alternativen in der Hintergrundfarbe und enthalten eine Kopfzeile ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))

## <a name="summary"></a>Summary

Obwohl das GridView-Steuerelement das anzeigen, bearbeiten, löschen, Sortieren und durchlaufen von Daten vereinfacht, ist die Darstellung sehr boxy und Raster ähnlich. Zur weiteren Kontrolle über das Erscheinungsbild müssen wir entweder die DataList-oder Repeater-Steuerelemente aktivieren. Beide Steuerelemente zeigen eine Reihe von Datensätzen mithilfe von Vorlagen anstelle von boundfields, checkboxfields usw. an.

Der DataList wird als HTML-`<table>` gerendert, der standardmäßig jeden Datenquellen Daten Satz in einer einzelnen Tabellenzeile anzeigt, genau wie ein GridView-Element mit einem einzelnen TemplateField. Wie wir in einem zukünftigen Tutorial sehen werden, gestattet der DataList jedoch, dass mehrere Datensätze pro Tabellenzeile angezeigt werden. Der Repeater gibt hingegen streng das in seinen Vorlagen angegebene Markup aus. Es fügt kein zusätzliches Markup hinzu und wird daher häufig zum Anzeigen von Daten in HTML-Elementen verwendet, die keine `<table>` sind (z. b. in einer Auflistungs Liste).

Während DataList und Repeater mehr Flexibilität in der gerenderten Ausgabe bieten, fehlen viele der in der GridView gefundenen integrierten Features. Wie wir in den bevorstehenden Tutorials untersuchen werden, können einige dieser Features ohne zu viel Aufwand wieder eingebunden werden. Bedenken Sie jedoch, dass die Verwendung des DataList-oder Repeater anstelle der GridView die Features einschränkt, die Sie verwenden können, ohne diese Features implementieren zu müssen. dich.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Prüfer für dieses Tutorial waren Yaakov Ellis, Liz shulok, Randy Schmidt und Stacy Park. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
