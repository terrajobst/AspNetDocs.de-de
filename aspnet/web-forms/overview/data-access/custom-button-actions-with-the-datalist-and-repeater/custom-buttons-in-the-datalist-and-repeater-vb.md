---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: Benutzerdefinierte Schaltflächen im DataList und Repeater (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erstellen wir eine Schnittstelle, die einen Repeater zum Auflisten der Kategorien im System verwendet, wobei jede Kategorie eine Schaltfläche zum Anzeigen der ASSOCI...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: bc7e94e59226b739c2948434c1bfecb46b3d7856
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78465351"
---
# <a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Benutzerdefinierte Schaltflächen im DataList- oder Wiederholungssteuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) oder [PDF herunterladen](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> In diesem Lernprogramm erstellen wir eine Schnittstelle, die einen Repeater zum Auflisten der Kategorien im System verwendet, wobei jede Kategorie eine Schaltfläche zum Anzeigen der zugehörigen Produkte mithilfe eines "BulletedList"-Steuer Elements bereitstellt.

## <a name="introduction"></a>Einführung

In den letzten 17 DataList-und Repeater-Tutorials haben wir sowohl schreibgeschützte Beispiele als auch Beispiele zum Bearbeiten und löschen erstellt. Um das Bearbeiten und Löschen von Funktionen innerhalb eines DataList zu vereinfachen, haben wir dem DataList-`ItemTemplate` Schaltflächen hinzugefügt, die beim Klicken auf ein Postback geführt und ein DataList-Ereignis ausgelöst haben, das der Schaltfläche s `CommandName` Eigenschaft entspricht. Wenn Sie z. b. eine Schaltfläche zum `ItemTemplate` mit einem `CommandName`-Eigenschafts Wert bearbeiten hinzufügen, bewirkt dies, dass die DataList s-`EditCommand` beim Postback ausgelöst werden. mit dem `CommandName` DELETE wird das `DeleteCommand`ausgelöst.

Zusätzlich zu den Schaltflächen Bearbeiten und löschen können die Steuerelemente DataList und Repeater auch Schaltflächen, LinkButtons oder imagebuttons enthalten, die beim Klicken auf eine benutzerdefinierte serverseitige Logik ausgeführt werden. In diesem Tutorial erstellen wir eine Schnittstelle, die einen Repeater zum Auflisten der Kategorien im System verwendet. Für jede Kategorie schließt der Repeater eine Schaltfläche ein, um die zugeordneten Produkte der Kategorie mithilfe eines "BulletedList"-Steuer Elements anzuzeigen (siehe Abbildung 1).

[![auf den Link Produkte anzeigen klicken, werden die Produkte der Kategorie in einer Auflistungs Liste angezeigt.](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Abbildung 1**: Wenn Sie auf den Link Produkte anzeigen klicken, werden die Produkte der Kategorie in einer aufhefliste angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))

## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Schritt 1: Hinzufügen der benutzerdefinierten Schaltflächen-Tutorial-Webseiten

Bevor wir uns mit der Vorgehensweise zum Hinzufügen einer benutzerdefinierten Schaltfläche beschäftigen, nehmen wir uns zunächst einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen. Fügen Sie zunächst einen neuen Ordner mit dem Namen `CustomButtonsDataListRepeater`hinzu. Fügen Sie dann die folgenden beiden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `CustomButtons.aspx`

![Hinzufügen der ASP.NET-Seiten für benutzerdefinierte Schaltflächen bezogene Tutorials](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen der ASP.NET-Seiten für benutzerdefinierte Schaltflächen bezogene Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `CustomButtonsDataListRepeater` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement zu `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Abbildung 3**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))

Fügen Sie abschließend die Seiten als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach dem Paging und der Sortierung mit dem DataList-und Repeater-`<siteMapNode>`das folgende Markup hinzu:

[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials zum Bearbeiten, einfügen und löschen.

![Die Site Übersicht enthält jetzt den Eintrag für das Tutorial benutzerdefinierte Schaltflächen.](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Abbildung 4**: die Site Übersicht enthält jetzt den Eintrag für das Tutorial zu benutzerdefinierten Schaltflächen.

## <a name="step-2-adding-the-list-of-categories"></a>Schritt 2: Hinzufügen der Liste der Kategorien

Für dieses Tutorial müssen Sie einen Repeater erstellen, der alle Kategorien auflistet, zusammen mit dem Link Button Produkte anzeigen, bei dem die zugeordneten Kategorie s Produkte in einer Auflistungs Liste angezeigt werden. Erstellen Sie zunächst einen einfachen Repeater, der die Kategorien im System auflistet. Öffnen Sie zunächst die Seite `CustomButtons.aspx` im Ordner `CustomButtonsDataListRepeater`. Ziehen Sie einen Repeater aus der Toolbox auf den Designer, und legen Sie dessen `ID`-Eigenschaft auf `Categories`fest. Erstellen Sie als nächstes ein neues Datenquellen-Steuerelement aus dem Repeater s Smarttags. Erstellen Sie insbesondere ein neues ObjectDataSource-Steuerelement mit dem Namen `CategoriesDataSource`, das seine Daten aus der `CategoriesBLL` Class s `GetCategories()`-Methode auswählt.

[![Konfigurieren von ObjectDataSource für die Verwendung der Methode GetCategories () der kategoriesbll-Klasse](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Abbildung 5**: Konfigurieren von ObjectDataSource für die Verwendung der `CategoriesBLL` Class s `GetCategories()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))

Anders als das DataList-Steuerelement, für das Visual Studio eine Standard `ItemTemplate` erstellt, die auf der Datenquelle basiert, müssen die Repeater s-Vorlagen manuell definiert werden. Außerdem müssen die Wiederholungs Vorlagen für Wiederholungs Vorlagen deklarativ erstellt und bearbeitet werden (d. h., es gibt keine Option zum Bearbeiten von Vorlagen in der Repeater s Smarttags).

Klicken Sie in der linken unteren Ecke auf die Registerkarte Quelle, und fügen Sie eine `ItemTemplate` hinzu, die den Namen der Kategorie in einem `<h3>`-Element und seine Beschreibung in einem Absatz Kennzeichen anzeigt. Fügen Sie einen `SeparatorTemplate` ein, der eine horizontale Regel (`<hr />`) zwischen jeder Kategorie anzeigt. Fügen Sie auch einen Link Button hinzu, dessen `Text`-Eigenschaft auf Produkte anzeigen festgelegt ist. Nachdem Sie diese Schritte ausgeführt haben, sollte Ihr deklaratives Markup der Seite wie folgt aussehen:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

Abbildung 6 zeigt die Seite, wenn Sie in einem Browser angezeigt wird. Jeder Kategoriename und jede Beschreibung ist aufgeführt. Wenn Sie auf die Schaltfläche Produkte anzeigen klicken, wird ein Postback verursacht, aber es wird noch keine Aktion ausgeführt.

[![die einzelnen Kategorien "Name" und "Beschreibung" angezeigt werden, zusammen mit dem Link "Produkte anzeigen".](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Abbildung 6**: die Namen und Beschreibungen der Kategorien werden zusammen mit dem Link "Produkte anzeigen" angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))

## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Schritt 3: Ausführen der Server seitigen Logik, wenn auf die linktaste "Produkte anzeigen" geklickt wird

Jedes Mal, wenn auf eine Schaltfläche, einen LinkButton oder einen ImageButton innerhalb einer Vorlage in einem DataList-oder Wiederholungs Modul geklickt wird, tritt ein Postback auf, und das DataList-oder Repeater-`ItemCommand` Ereignis wird ausgelöst. Zusätzlich zum `ItemCommand`-Ereignis kann das DataList-Steuerelement auch ein weiteres, spezifischere Ereignis auslösen, wenn die Schaltfläche s `CommandName`-Eigenschaft auf eine der reservierten Zeichen folgen festgelegt ist (Delete, Edit, Cancel, Update oder SELECT), aber das `ItemCommand`-Ereignis *immer* ausgelöst wird.

Wenn in einem DataList-oder Wiederholungs Steuerelement auf eine Schaltfläche geklickt wird, müssen wir die Schaltfläche übergeben, auf die geklickt wurde (in dem Fall, dass es mehrere Schaltflächen im Steuerelement gibt, z. b. eine Schaltfläche zum Bearbeiten und löschen), und möglicherweise einige zusätzliche Informationen (z. b. der Primärschlüssel Wert des Elements, auf dessen Schaltfläche geklickt wurde. Die Schaltfläche, LinkButton und ImageButton enthalten zwei Eigenschaften, deren Werte an den `ItemCommand`-Ereignishandler übermittelt werden:

- `CommandName` eine Zeichenfolge, die normalerweise zum Identifizieren der einzelnen Schaltflächen in der Vorlage verwendet
- `CommandArgument`, die häufig verwendet wird, um den Wert eines Daten Felds (z. b. den Primärschlüssel Wert) aufzunehmen.

Legen Sie für dieses Beispiel die LinkButton s `CommandName`-Eigenschaft auf showProducts fest, und binden Sie den Wert des aktuellen Datensatzes s als Primärschlüssel `CategoryID` mithilfe der Datenbindung-Syntax `CategoryArgument='<%# Eval("CategoryID") %>'`an die `CommandArgument`-Eigenschaft. Nachdem Sie diese beiden Eigenschaften angegeben haben, sollte die deklarative Syntax von LinkButton s wie folgt aussehen:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

Wenn auf die Schaltfläche geklickt wird, erfolgt ein Postback, und das DataList-oder Repeater-`ItemCommand` Ereignis wird ausgelöst. Dem Ereignishandler werden die Schaltflächen s `CommandName` und `CommandArgument` Werte übermittelt.

Erstellen Sie einen Ereignishandler für das Repeater s `ItemCommand`-Ereignis, und notieren Sie sich den zweiten Parameter, der an den Ereignishandler übergeben wird (`e`). Dieser zweite Parameter ist vom Typ [`RepeaterCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) und verfügt über die folgenden vier Eigenschaften:

- `CommandArgument` den Wert der Schaltfläche s `CommandArgument` Eigenschaft, auf die geklickt wurde.
- `CommandName` den Wert der Schaltfläche s `CommandName` Eigenschaft
- `CommandSource` einen Verweis auf das Schaltflächen Steuerelement, auf das geklickt wurde.
- `Item` einen Verweis auf das [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) , das die Schaltfläche enthält, auf die geklickt wurde. jeder an den Repeater gebundene Datensatz wird als `RepeaterItem`

Da die ausgewählte Kategorie s `CategoryID` über die `CommandArgument`-Eigenschaft übermittelt wird, können wir den Satz von Produkten, die der ausgewählten Kategorie zugeordnet sind, im `ItemCommand`-Ereignishandler erhalten. Diese Produkte können dann an ein BulletedList-Steuerelement in der `ItemTemplate` gebunden werden (das wir noch hinzugefügt haben). Es bleibt alles übrig, und dann wird die aufzurufliste hinzugefügt, in der `ItemCommand`-Ereignishandler darauf verwiesen und an den Satz von Produkten für die ausgewählte Kategorie gebunden, den wir in Schritt 4 behandeln werden.

> [!NOTE]
> Dem DataList s `ItemCommand`-Ereignishandler wird ein Objekt vom Typ " [`DataListCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx)" übermittelt, das dieselben vier Eigenschaften wie die `RepeaterCommandEventArgs`-Klasse bietet.

## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Schritt 4: Anzeigen der Produkte der ausgewählten Kategorie in einer Auflistungs Liste

Die ausgewählten Produkte der Kategorie s können innerhalb der Repeater-`ItemTemplate` mit einer beliebigen Anzahl von Steuerelementen angezeigt werden. Wir könnten weitere geschaltete Wiederholungs Steuerelemente, DataList, Dropdown Listen, GridView usw. hinzufügen. Da wir die Produkte als Auflistungs Liste anzeigen möchten, verwenden wir jedoch das Steuerelement "BulletedList". Wenn Sie zum deklarativen Markup der `CustomButtons.aspx` Seite zurückkehren, fügen Sie der `ItemTemplate` nach dem Link Schaltfläche Produkte anzeigen ein Auflistungs Listen-Steuerelement hinzu. Legen Sie die `ID` für die aufzuruflisten s auf `ProductsInCategory`fest. Die Auflistungs Liste zeigt den Wert des Daten Felds an, das über die `DataTextField`-Eigenschaft angegeben wird. Da auf dieses Steuerelement Produktinformationen gebunden werden, legen Sie die `DataTextField`-Eigenschaft auf `ProductName`fest.

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

Verweisen Sie im `ItemCommand`-Ereignishandler mit `e.Item.FindControl("ProductsInCategory")` auf dieses Steuerelement, und binden Sie es an den Satz von Produkten, die der ausgewählten Kategorie zugeordnet sind.

[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Vor dem Ausführen einer Aktion im `ItemCommand`-Ereignishandler ist es ratsam, zuerst den Wert der eingehenden `CommandName`zu überprüfen. Da der `ItemCommand` Ereignishandler ausgelöst wird, wenn auf *eine* Schaltfläche geklickt wird, verwenden Sie den `CommandName` Wert, wenn mehrere Schaltflächen in der Vorlage vorhanden sind, um die auszuführende Aktion zu ermitteln. Das Überprüfen des `CommandName` hier ist "Muot", da wir nur eine einzige Schaltfläche haben, aber es ist eine gute Gewohnheit, zu bilden. Als nächstes wird der `CategoryID` der ausgewählten Kategorie aus der `CommandArgument`-Eigenschaft abgerufen. Auf das Steuerelement "BulletedList" in der Vorlage wird dann verwiesen, und es wird an die Ergebnisse der `ProductsBLL` Klasse "s `GetProductsByCategoryID(categoryID)`-Methode gebunden.

In vorherigen Tutorials, in denen die Schaltflächen in einem DataList verwendet wurden (z. b. [eine Übersicht über das Bearbeiten und Löschen von Daten im DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)), haben wir den Wert des Primärschlüssels eines bestimmten Elements über die `DataKeys` Collection bestimmt. Obwohl dieser Ansatz gut mit dem DataList funktioniert, verfügt der Repeater nicht über eine `DataKeys`-Eigenschaft. Stattdessen muss ein alternativer Ansatz zum Angeben des Primärschlüssel Werts verwendet werden, z. b. über die Schaltfläche s `CommandArgument` Eigenschaft oder durch Zuweisen des Primärschlüssel Werts zu einem ausgeblendeten Beschriftungs-websteuer Element innerhalb der Vorlage und durch das Lesen des Werts im `ItemCommand` Ereignishandler mithilfe von `e.Item.FindControl("LabelID")`.

Nachdem Sie den `ItemCommand`-Ereignishandler abgeschlossen haben, nehmen Sie sich einen Moment Zeit, um diese Seite in einem Browser zu testen. Wie in Abbildung 7 gezeigt, wird durch Klicken auf den Link Produkte anzeigen ein Postback verursacht, und die Produkte für die ausgewählte Kategorie werden in einer Auflistungs Liste angezeigt. Beachten Sie außerdem, dass diese Produktinformationen weiterhin angezeigt werden, auch wenn in anderen Kategorien auf "Produkte anzeigen" geklickt wird.

> [!NOTE]
> Wenn Sie das Verhalten dieses Berichts so ändern möchten, dass jeweils nur die Produkte der Kategorie s aufgeführt sind, legen Sie einfach die `EnableViewState` Eigenschaft des Steuer Elements "BulletedList" auf `False`fest.

[![eine "BulletedList" zum Anzeigen der Produkte der ausgewählten Kategorie verwendet wird.](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Abbildung 7**: eine "BulletedList" wird verwendet, um die Produkte der ausgewählten Kategorie anzuzeigen ([Klicken Sie, um das Bild in voller Größe](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png)anzuzeigen)

## <a name="summary"></a>Zusammenfassung

Die Steuerelemente DataList und Repeater können eine beliebige Anzahl von Schaltflächen, LinkButtons oder imagebuttons in ihren Vorlagen enthalten. Wenn Sie auf diese Schaltflächen klicken, wird ein Postback ausgelöst, und das `ItemCommand`-Ereignis wird ausgelöst. Erstellen Sie einen Ereignishandler für das `ItemCommand`-Ereignis, um eine benutzerdefinierte serverseitige Aktion einer Schaltfläche zuzuordnen, auf die geklickt wird. Überprüfen Sie in diesem Ereignishandler zuerst den Wert eingehender `CommandName`, um zu bestimmen, auf welche Schaltfläche geklickt wurde. Zusätzliche Informationen können optional über die Schaltfläche s `CommandArgument`-Eigenschaft bereitgestellt werden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Der Lead Reviewer für dieses Tutorial war Dennis Patterson. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Previous](custom-buttons-in-the-datalist-and-repeater-cs.md)
