---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Verwenden der FormView-Vorlagen (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Anders als die DetailsView besteht die FormView nicht aus Feldern. Stattdessen wird die FormView mithilfe von Vorlagen gerendert. In diesem Tutorial untersuchen wir die Verwendung der F...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 013c6878aad1a2277b0a334c096ff16ed84a95f1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74625674"
---
# <a name="using-the-formviews-templates-c"></a>Verwenden der FormView-Vorlagen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) oder [PDF herunterladen](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> Anders als die DetailsView besteht die FormView nicht aus Feldern. Stattdessen wird die FormView mithilfe von Vorlagen gerendert. In diesem Tutorial untersuchen wir die Verwendung des FormView-Steuer Elements, um eine weniger strenge Anzeige von Daten darzustellen.

## <a name="introduction"></a>Einführung

In den letzten beiden Tutorials wurde erläutert, wie die Ausgaben von GridView und DetailsView-Steuerelementen mithilfe von templatefields angepasst werden. Templatefields ermöglicht, dass die Inhalte für ein bestimmtes Feld hochgradig angepasst werden, aber am Ende verfügen sowohl GridView als auch DetailsView über eine ziemlich boxy-Darstellung (Raster ähnlich). In vielen Szenarios ist ein Raster ähnliches Layout ideal, aber manchmal wird eine weniger starre Anzeige benötigt. Beim Anzeigen eines einzelnen Datensatzes ist ein solches dynamisches Layout mit dem FormView-Steuerelement möglich.

Anders als die DetailsView besteht die FormView nicht aus Feldern. Sie können einem FormView-Element kein BoundField-oder TemplateField-Element hinzufügen. Stattdessen wird die FormView mithilfe von Vorlagen gerendert. Stellen Sie sich die Form View als DetailsView-Steuerelement vor, das ein einzelnes TemplateField-Steuerelement enthält. Die FormView unterstützt die folgenden Vorlagen:

- `ItemTemplate`, der verwendet wird, um den in der FormView angezeigten Datensatz zu Rendering.
- `HeaderTemplate` zum Angeben einer optionalen Kopfzeile verwendet.
- `FooterTemplate`, die verwendet wird, um eine optionale Footerzeile anzugeben.
- `EmptyDataTemplate`, wenn die `DataSource` der FormView keine Datensätze enthält, wird der `EmptyDataTemplate` anstelle der `ItemTemplate` zum Rendern des Markup des Steuer Elements verwendet.
- `PagerTemplate` kann verwendet werden, um die pagingschnittstelle für formviews anzupassen, für die Paging aktiviert ist.
- `EditItemTemplate` / `InsertItemTemplate`, die zum Anpassen der Bearbeitungs Schnittstelle oder zum Einfügen der Schnittstelle für formviews verwendet werden, die diese Funktionalität unterstützen

In diesem Tutorial untersuchen wir die Verwendung des FormView-Steuer Elements, um eine weniger strenge Anzeige von Produkten darzustellen. Anstatt Felder für den Namen, die Kategorie, den Lieferanten usw. zu verwenden, zeigen die `ItemTemplate` der FormView diese Werte mit einer Kombination aus einem Header Element und einem `<table>` an (siehe Abbildung 1).

[![die FormView aus dem Raster ähnlichen Layout in der DetailsView heraus bricht.](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Abbildung 1**: die FormView wird aus dem Raster ähnlichen Layout, das in der DetailsView angezeigt wird, unterbrochen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-formview-s-templates-cs/_static/image3.png)).

## <a name="step-1-binding-the-data-to-the-formview"></a>Schritt 1: Binden der Daten an FormView

Öffnen Sie die Seite `FormView.aspx`, und ziehen Sie eine FormView aus der Toolbox auf den Designer. Beim ersten Hinzufügen von FormView wird es als graues Feld angezeigt, das anweist, dass ein `ItemTemplate` erforderlich ist.

[![FormView kann erst im Designer gerendert werden, wenn eine ItemTemplate bereitgestellt wird.](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Abbildung 2**: die FormView kann erst im Designer gerendert werden, wenn ein `ItemTemplate` bereitgestellt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-formview-s-templates-cs/_static/image6.png))

Der `ItemTemplate` kann von Hand (durch die deklarative Syntax) erstellt werden oder automatisch erstellt werden, indem die FormView über den Designer an ein Datenquellen-Steuerelement gebunden wird. Diese automatisch erstellte `ItemTemplate` enthält HTML, in der die Namen der einzelnen Felder aufgeführt sind, und ein Label-Steuerelement, dessen `Text`-Eigenschaft an den Wert des Felds gebunden ist. Bei diesem Ansatz werden auch automatisch eine `InsertItemTemplate` und `EditItemTemplate`erstellt, die beide mit Eingabe Steuerelementen für jedes der vom Datenquellen-Steuerelement zurückgegebenen Datenfelder aufgefüllt werden.

Wenn Sie die Vorlage automatisch erstellen möchten, fügen Sie im Smarttag von FormView ein neues ObjectDataSource-Steuerelement hinzu, das die `GetProducts()` Methode der `ProductsBLL` Klasse aufruft. Dadurch wird eine FormView mit einer `ItemTemplate`, `InsertItemTemplate`und `EditItemTemplate`erstellt. Entfernen Sie in der Quell Ansicht die `InsertItemTemplate` und `EditItemTemplate`, da wir nicht an der Erstellung einer FormView interessiert sind, die die Bearbeitung oder das Einfügen noch nicht unterstützt. Löschen Sie als nächstes das Markup innerhalb der `ItemTemplate`, damit wir über ein sauberes Slate verfügen, aus dem Sie arbeiten können.

Wenn Sie die `ItemTemplate` manuell erstellen möchten, können Sie die ObjectDataSource hinzufügen und konfigurieren, indem Sie Sie aus der Toolbox auf den Designer ziehen. Legen Sie jedoch die Datenquelle von FormView nicht vom Designer fest. Wechseln Sie stattdessen zur Quell Ansicht, und legen Sie die `DataSourceID`-Eigenschaft von FormView manuell auf den `ID` Wert von ObjectDataSource fest. Fügen Sie als nächstes manuell den `ItemTemplate`hinzu.

Unabhängig davon, welchen Ansatz Sie sich entschieden haben, sollte das deklarative Markup Ihrer FormView nun wie folgt aussehen:

[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

Nehmen Sie sich einen Moment Zeit, um das Kontrollkästchen Paging aktivieren im Smarttag von FormView zu aktivieren. Dadurch wird das `AllowPaging="True"`-Attribut zur deklarativen Syntax von FormView hinzugefügt. Legen Sie außerdem die `EnableViewState`-Eigenschaft auf false fest.

## <a name="step-2-defining-theitemtemplates-markup"></a>Schritt 2: Definieren des Markups des`ItemTemplate`

Wenn die FormView an das ObjectDataSource-Steuerelement gebunden ist und für die Unterstützung von Paging konfiguriert ist, können wir den Inhalt für die `ItemTemplate`angeben. In diesem Tutorial soll der Name des Produkts in einer `<h3>` Überschrift angezeigt werden. Danach verwenden wir eine HTML-`<table>`, um die verbleibenden Produkteigenschaften in einer Tabelle mit vier Spalten anzuzeigen, in der die erste und die dritte Spalte die Eigenschaftsnamen und die zweite und vierte Liste ihre Werte auflisten.

Dieses Markup kann über die Vorlagen Bearbeitungs Schnittstelle von FormView im Designer eingegeben oder manuell über die deklarative Syntax eingegeben werden. Beim Arbeiten mit Vorlagen ist es in der Regel schneller, dass Sie direkt mit der deklarativen Syntax arbeiten, aber Sie können auch beliebige Techniken verwenden, mit denen Sie am meisten vertraut sind.

Das folgende Markup zeigt das deklarative FormView-Markup, nachdem die Struktur des `ItemTemplate`abgeschlossen wurde:

[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Beachten Sie, dass die Datenbindung-Syntax `<%# Eval("ProductName") %>`z. b. direkt in die Ausgabe der Vorlage eingefügt werden kann. Das heißt, dass Sie nicht der `Text`-Eigenschaft eines Label-Steuer Elements zugewiesen werden muss. Beispielsweise wird der `ProductName` Wert in einem `<h3>`-Element mit `<h3><%# Eval("ProductName") %></h3>`angezeigt, das für das Produkt Chai als `<h3>Chai</h3>`dargestellt wird.

Die `ProductPropertyLabel`-und `ProductPropertyValue` CSS-Klassen werden verwendet, um den Stil der Produkt Eigenschaftsnamen und-Werte in der `<table>`anzugeben. Diese CSS-Klassen werden in `Styles.css` definiert und bewirken, dass die Eigenschaftsnamen Fett und rechtsbündig ausgerichtet werden und den Eigenschafts Werten eine Rechte Auffüll Zeichen hinzugefügt werden.

Da in FormView keine checkboxfields-Elemente verfügbar sind, müssen Sie ein eigenes CheckBox-Steuerelement hinzufügen, um den `Discontinued` Wert als Kontrollkästchen anzuzeigen. Die `Enabled`-Eigenschaft ist auf false festgelegt, sodass Sie schreibgeschützt ist, und die `Checked`-Eigenschaft des Kontrollkästchens wird an den Wert des `Discontinued` Datenfelds gebunden.

Wenn die `ItemTemplate` vollständig ist, werden die Produktinformationen auf eine viel schnellere Weise angezeigt. Vergleichen Sie die DetailsView-Ausgabe aus dem letzten Tutorial (Abbildung 3) mit der Ausgabe, die von der FormView in diesem Tutorial generiert wurde (Abbildung 4).

[![die starre DetailsView-Ausgabe](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Abbildung 3**: die Ausgabe der starren DetailsView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-formview-s-templates-cs/_static/image9.png))

[![der flüssigen FormView-Ausgabe](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Abbildung 4**: die flüssige FormView-Ausgabe ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-formview-s-templates-cs/_static/image12.png))

## <a name="summary"></a>Summary

Während die Ausgabe der GridView-und DetailsView-Steuerelemente mithilfe von templatefields angepasst werden kann, stellen beide ihre Daten immer noch in einem Raster ähnlichen boxy-Format dar. Wenn ein einzelner Datensatz mithilfe eines weniger festgelegten Layouts angezeigt werden muss, ist FormView eine ideale Wahl. Wie die DetailsView rendert FormView einen einzelnen Datensatz aus seinem `DataSource`, aber im Gegensatz zu DetailsView besteht er nur aus Vorlagen und unterstützt keine Felder.

Wie in diesem Tutorial gezeigt, ermöglicht FormView ein flexibleres Layout, wenn ein einzelner Datensatz angezeigt wird. In zukünftigen Tutorials untersuchen wir die DataList-und Repeater-Steuerelemente, die das gleiche Maß an Flexibilität wie die formsview bieten, aber mehrere Datensätze anzeigen können (wie z. b. GridView).

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war E.R. Gilmore. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](using-templatefields-in-the-detailsview-control-cs.md)
> [Weiter](displaying-summary-information-in-the-gridview-s-footer-cs.md)
