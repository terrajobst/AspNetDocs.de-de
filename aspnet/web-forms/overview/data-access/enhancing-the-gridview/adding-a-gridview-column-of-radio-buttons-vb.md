---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: Hinzufügen einer GridView-Spalte mit Options Feldern (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Sie einem GridView-Steuerelement eine Spalte mit Options Feldern hinzufügen, um dem Benutzer eine intuitivere Möglichkeit zur Auswahl einer einzelnen Zeile von...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: ee67a4556c65d2c9570bf15b42fc3c8e5f555bda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78477687"
---
# <a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Hinzufügen einer GridView-Spalte mit Optionsfeldern (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) oder [PDF herunterladen](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> In diesem Tutorial wird erläutert, wie einem GridView-Steuerelement eine Spalte mit Options Feldern hinzugefügt wird, um dem Benutzer eine intuitivere Methode zum Auswählen einer einzelnen Zeile der GridView zu bieten.

## <a name="introduction"></a>Einführung

Das GridView-Steuerelement bietet eine große Menge integrierter Funktionen. Sie enthält eine Reihe verschiedener Felder zum Anzeigen von Text, Bildern, Hyperlinks und Schaltflächen. Es werden Vorlagen zur weiteren Anpassung unterstützt. Mit wenigen Mausklicks können Sie eine GridView erstellen, in der jede Zeile über eine Schaltfläche ausgewählt werden kann, oder um Bearbeitungs-oder Löschfunktionen zu aktivieren. Trotz der Vielzahl der bereitgestellten Features gibt es häufig Situationen, in denen zusätzliche, nicht unterstützte Features hinzugefügt werden müssen. In diesem Tutorial und den nächsten beiden untersuchen wir, wie Sie die Funktionalität von GridView für zusätzliche Features erweitern können.

Dieses Tutorial und der nächste Schwerpunkt auf der Erweiterung des Zeilenauswahl Prozesses. Wie in der [Master-/Ausführungs-Sicht unter Verwendung eines auswählbaren Master GridView-Werts mit Detailansicht](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)untersucht, können wir ein CommandField-Feld zur GridView hinzufügen, das die Schaltfläche auswählen enthält Wenn Sie darauf klicken, wird ein Postback ausgeführt, und die GridView s `SelectedIndex`-Eigenschaft wird auf den Index der Zeile aktualisiert, auf deren Select-Schaltfläche geklickt wurde. In der [Master-/Detail-Verwendung eines auswählbaren Master GridView-Tutorials mit einem Detail DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) -Tutorial haben Sie erfahren, wie Sie diese Funktion verwenden, um Details für die ausgewählte GridView-Zeile anzuzeigen.

Obwohl die Select-Schaltfläche in vielen Situationen funktioniert, funktioniert Sie möglicherweise nicht auch für andere. Anstatt eine Schaltfläche zu verwenden, werden zwei weitere Benutzeroberflächen Elemente häufig zur Auswahl verwendet: das Optionsfeld und das Kontrollkästchen. Wir können die GridView so erweitern, dass jede Zeile anstelle einer SELECT-Taste ein Optionsfeld oder ein Kontrollkästchen enthält. In Szenarien, in denen der Benutzer nur einen der GridView-Datensätze auswählen kann, wird das Optionsfeld möglicherweise über die Schaltfläche auswählen bevorzugt. In Situationen, in denen der Benutzer möglicherweise mehrere Datensätze, z. b. in einer webbasierten e-Mail-Anwendung, auswählen kann, wenn ein Benutzer mehrere Nachrichten zum Löschen auswählen möchte, bietet das Kontrollkästchen Funktionen, die über die Schaltfläche auswählen oder Optionsfeld nicht verfügbar sind Benutzeroberflächen.

In diesem Tutorial wird erläutert, wie Sie der GridView eine Spalte mit Options Feldern hinzufügen. Das Lernprogramm zum Fortfahren erläutert mithilfe von Kontrollkästchen.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Schritt 1: Erstellen der verbesserten GridView-Webseiten

Bevor wir mit dem Erweitern der GridView-Spalte beginnen, um eine Spalte mit Options Feldern einzuschließen, nehmen Sie zunächst einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen, und die nächsten beiden. Fügen Sie zunächst einen neuen Ordner mit dem Namen `EnhancedGridView`hinzu. Fügen Sie dann die folgenden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`

![Fügen Sie die ASP.NET-Seiten für die SqlDataSource-bezogenen Tutorials hinzu.](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Abbildung 1**: Hinzufügen der ASP.NET-Seiten für die auf SqlDataSource bezogenen Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `EnhancedGridView` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Abbildung 2**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))

Fügen Sie schließlich diese vier Seiten als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach dem mit dem SqlDataSource-Steuerelement `<siteMapNode>`das folgende Markup hinzu:

[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials zum Bearbeiten, einfügen und löschen.

![Die Site Übersicht enthält nun Einträge für die verbessern der GridView-Tutorials.](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Abbildung 3**: die Site Übersicht enthält jetzt Einträge für die Erweiterung der GridView-Tutorials.

## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Schritt 2: Anzeigen der Lieferanten in einer GridView

In diesem Tutorial erstellen Sie eine GridView, die die Lieferanten aus den USA auflistet, wobei jede GridView-Zeile ein Optionsfeld bereitstellt. Nachdem Sie einen Lieferanten über das Optionsfeld ausgewählt haben, kann der Benutzer die Produkte des Lieferanten anzeigen, indem er auf eine Schaltfläche klickt. Diese Aufgabe mag trivial klingen, aber es gibt eine Reihe von Feinheiten, die dies besonders schwierig machen. Bevor wir uns mit diesen Feinheiten befassen, sollten Sie zunächst eine GridView-Liste der Lieferanten erhalten.

Öffnen Sie zunächst die Seite `RadioButtonField.aspx` im Ordner `EnhancedGridView`, indem Sie eine GridView aus der Toolbox auf den Designer ziehen. Legen Sie die `ID` GridView s auf `Suppliers` fest, und wählen Sie aus dem Smarttag aus, um eine neue Datenquelle zu erstellen. Erstellen Sie insbesondere eine ObjectDataSource mit dem Namen `SuppliersDataSource`, die die Daten aus dem `SuppliersBLL`-Objekt abruft.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen "suppliersdatasource".](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Abbildung 4**: Erstellen einer neuen ObjectDataSource mit dem Namen "`SuppliersDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))

[![Konfigurieren von ObjectDataSource für die Verwendung der suppliersbll-Klasse](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Abbildung 5**: Konfigurieren von ObjectDataSource für die Verwendung der `SuppliersBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))

Da wir nur die Lieferanten in den USA auflisten möchten, wählen Sie in der Dropdown Liste auf der Registerkarte auswählen die `GetSuppliersByCountry(country)` Methode aus.

[![Konfigurieren von ObjectDataSource für die Verwendung der suppliersbll-Klasse](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Abbildung 6**: Konfigurieren von ObjectDataSource für die Verwendung der `SuppliersBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))

Wählen Sie auf der Registerkarte aktualisieren die Option (None) aus, und klicken Sie auf Weiter.

[![Konfigurieren von ObjectDataSource für die Verwendung der suppliersbll-Klasse](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Abbildung 7**: Konfigurieren von ObjectDataSource für die Verwendung der `SuppliersBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))

Da die `GetSuppliersByCountry(country)`-Methode einen Parameter annimmt, werden Sie vom Assistenten zum Konfigurieren von Datenquellen zur Quelle dieses Parameters aufgefordert. Wenn Sie einen hart codierten Wert (in diesem Beispiel USA) angeben möchten, lassen Sie die Dropdown Liste Parameter Quelle auf None fest, und geben Sie den Standardwert in das Textfeld ein. Klicken Sie auf Fertigstellen, um den Assistenten abzuschließen.

[![die USA als Standardwert für den Country-Parameter verwenden.](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Abbildung 8**: Verwenden von USA als Standardwert für den `country`-Parameter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))

Nachdem Sie den Assistenten abgeschlossen haben, enthält die GridView ein BoundField für jedes der Lieferantendaten Felder. Entfernen Sie alle außer den `CompanyName`, `City`und `Country` boundfields, und benennen Sie die `CompanyName` boundfields `HeaderText`-Eigenschaft in Supplier um. Anschließend sollte die deklarative Syntax GridView und ObjectDataSource in etwa wie folgt aussehen.

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

In diesem Tutorial gestatten Sie es Benutzern, die ausgewählten Lieferanten Produkte auf derselben Seite wie die Lieferantenliste oder auf einer anderen Seite anzuzeigen. Fügen Sie der Seite zwei Schaltflächen-websteuer Elemente hinzu, um dies zu ermöglichen. Ich habe die `ID` s dieser beiden Schaltflächen auf `ListProducts` und `SendToProducts`festgelegt, mit der Idee, dass beim Klicken auf `ListProducts` ein Postback stattfindet und die Produkte der ausgewählten Lieferanten auf der gleichen Seite aufgeführt werden. Wenn jedoch auf `SendToProducts` geklickt wird, wird der Benutzer auf eine andere Seite mit den Produkten angezeigt.

Abbildung 9 zeigt die `Suppliers` GridView und die zwei Schaltflächen-websteuer Elemente, wenn Sie in einem Browser angezeigt werden.

[![diesen Lieferanten aus den USA sind ihre Namen, Städte und Länder Informationen aufgeführt.](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Abbildung 9**: bei den Lieferanten aus den USA werden Ihre Namen, Städte und Länder Informationen aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))

## <a name="step-3-adding-a-column-of-radio-buttons"></a>Schritt 3: Hinzufügen einer Spalte mit Options Feldern

An diesem Punkt enthält die `Suppliers` GridView drei boundfields, die den Firmennamen, den Ort und das Land der einzelnen Lieferanten in den USA anzeigen. Es fehlt jedoch noch eine Spalte mit Options Feldern. Leider ist das GridView-Paket nicht in der es enthalten, da es ein integriertes RadioButton Field-Feld enthält. andernfalls könnten wir das Raster einfach dem Raster hinzufügen. Stattdessen können Sie ein TemplateField-Element hinzufügen und dessen `ItemTemplate` konfigurieren, um ein Optionsfeld zu erstellen, wodurch für jede GridView-Zeile ein Optionsfeld angezeigt wird.

Anfänglich könnten wir davon ausgehen, dass die gewünschte Benutzeroberfläche implementiert werden kann, indem dem `ItemTemplate` eines TemplateField-Steuer Elements ein RadioButton-Steuerelement hinzugefügt wird. Zwar wird für jede Zeile der GridView tatsächlich ein einzelnes Optionsfeld hinzugefügt, die Options Felder können jedoch nicht gruppiert werden und schließen sich daher nicht gegenseitig aus. Das heißt, ein Endbenutzer kann mehrere Options Felder gleichzeitig aus der GridView auswählen.

Obwohl das Verwenden eines TemplateField-Steuer Elements für RadioButton nicht die erforderliche Funktionalität bietet, können Sie diesen Ansatz implementieren, da es sinnvoll ist, zu überprüfen, warum die resultierenden Options Felder nicht gruppiert sind. Fügen Sie zunächst ein TemplateField-Element zur Ansicht "Suppliers GridView" hinzu, sodass es das Feld ganz links ist. Klicken Sie als nächstes auf das Smarttag für GridView s, klicken Sie auf den Link Vorlagen bearbeiten, und ziehen Sie ein RadioButton-websteuer Element aus der Toolbox in die `ItemTemplate` TemplateField s (siehe Abbildung 10). Legen Sie die Eigenschaft RadioButton s `ID` auf `RowSelector` und die Eigenschaft `GroupName` auf `SuppliersGroup`fest.

[![ein RadioButton-websteuer Element zur ItemTemplate hinzufügen](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Abbildung 10**: Hinzufügen eines RadioButton-websteuer Elements zum `ItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))

Nachdem Sie diese Ergänzungen über den Designer vorgenommen haben, sollte das GridView s-Markup in etwa wie folgt aussehen:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

Die Eigenschaften von RadioButton s [`GroupName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) werden verwendet, um eine Reihe von Options Feldern zu gruppieren. Alle RadioButton-Steuerelemente mit dem gleichen `GroupName` Wert gelten als gruppiert. Es kann jeweils nur ein Optionsfeld aus einer Gruppe ausgewählt werden. Die `GroupName`-Eigenschaft gibt den Wert für das `name` Attribut der gerenderten Options Schaltfläche s an. Der Browser überprüft die Options Felder `name` Attribute, um die Optionsfeld Gruppierungen zu bestimmen.

Wenn das RadioButton-websteuer Element der `ItemTemplate`hinzugefügt wurde, besuchen Sie diese Seite über einen Browser, und klicken Sie auf die Options Felder in den Raster Zeilen. Beachten Sie, dass die Options Felder nicht gruppiert sind, sodass alle Zeilen ausgewählt werden können, wie in Abbildung 11 dargestellt.

[![der GridView-Options Felder nicht gruppiert](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Abbildung 11**: die Options Felder der GridView-s sind nicht gruppiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))

Der Grund dafür, dass die Options Felder nicht gruppiert sind, liegt darin, dass sich die gerenderten `name` Attribute unterscheiden, obwohl die gleiche `GroupName` Eigenschaften Einstellung vorliegt. Um diese Unterschiede anzuzeigen, führen Sie eine Ansicht/Quelle aus dem Browser aus, und untersuchen Sie das Optionsfeld Markup:

[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Beachten Sie, dass die Attribute `name` und `id` nicht die exakten Werte sind, wie Sie im Eigenschaftenfenster angegeben werden, aber eine Reihe anderer `ID` Werte vorangestellt sind. Die zusätzlichen `ID` Werte, die der Vorderseite der gerenderten `id` und `name` Attribute hinzugefügt werden, sind die `ID` en der übergeordneten Options Felder, die `GridViewRow` s `ID` en, die GridView-`ID`, die `ID`des Inhalts Steuer Elements und die `ID`des Webformulars. Diese `ID` s werden hinzugefügt, sodass jedes gerenderte websteuer Element in der GridView über einen eindeutigen `id` und `name` Werte verfügt.

Jedes gerenderte Steuerelement benötigt eine andere `name` und `id`, da der Browser die einzelnen Steuerelemente auf der Clientseite eindeutig identifiziert und die Art und Weise, wie Sie für den Webserver identifiziert wird, welche Aktion oder Änderung beim Postback aufgetreten ist. Stellen Sie sich beispielsweise vor, dass Sie einen serverseitigen Code ausführen möchten, wenn der aktivierte Zustand eines RadioButton s geändert wurde. Dies können Sie erreichen, indem Sie die Eigenschaft RadioButton s `AutoPostBack` auf `True` festlegen und einen Ereignishandler für das `CheckChanged`-Ereignis erstellen. Wenn jedoch die gerenderten `name` und `id` Werte für alle Options Felder identisch waren, konnte beim Postback nicht bestimmt werden, auf welche Options Schaltfläche geklickt wurde.

Der kurze besteht darin, dass wir mit dem RadioButton-websteuer Element keine Spalte mit Options Feldern in einem GridView-Steuerelement erstellen können. Stattdessen müssen wir eher archaische Techniken verwenden, um sicherzustellen, dass das entsprechende Markup in jede GridView-Zeile eingefügt wird.

> [!NOTE]
> Wie das RadioButton-websteuer Element enthält das Optionsfeld-HTML-Steuerelement beim Hinzufügen zu einer Vorlage das eindeutige `name` Attribut, sodass die Options Felder im Raster nicht gruppiert werden. Wenn Sie mit HTML-Steuerelementen nicht vertraut sind, können Sie diese Notiz ignorieren, da HTML-Steuerelemente selten verwendet werden, insbesondere in ASP.NET 2,0. Wenn Sie mehr erfahren möchten, finden Sie weitere Informationen unter [K. Scott allen](http://odetocode.com/blogs/scott/default.aspx) s Blogeintrag [websteuer Elemente und HTML-Steuerelemente](http://www.odetocode.com/Articles/348.aspx).

## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Verwenden eines literalen Steuer Elements zum Einfügen von Radio Button-Markup

Um alle Options Felder in der GridView ordnungsgemäß zu gruppieren, müssen Sie das Optionsfeld Markup manuell in den `ItemTemplate`einfügen. Jedes Optionsfeld benötigt dasselbe `name` Attribut, sollte jedoch über ein eindeutiges `id` Attribut verfügen (für den Fall, dass Sie über ein Client seitiges Skript auf ein Optionsfeld zugreifen möchten). Nachdem ein Benutzer ein Optionsfeld ausgewählt und die Seite zurückgesendet hat, sendet der Browser den Wert der ausgewählten Options Felder `value` Attribut zurück. Daher wird für jedes Optionsfeld ein eindeutiges `value` Attribut benötigt. Schließlich müssen Sie beim Postback sicherstellen, dass Sie das `checked`-Attribut zu einem Optionsfeld hinzufügen, das ausgewählt ist. andernfalls werden die Options Felder in ihren Standardzustand zurückversetzt (alle nicht ausgewählt), nachdem der Benutzer eine Auswahl getroffen und zurückgegeben hat.

Es gibt zwei Ansätze, die ergriffen werden können, um Markup auf niedriger Ebene in eine Vorlage einzufügen. Eine besteht darin, eine Mischung aus Markup und Aufrufen von Formatierungs Methoden durchzuführen, die in der Code-Behind-Klasse definiert sind. Diese Technik wurde zunächst im Tutorial [Verwenden von templatefields im GridView-Steuer](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Element erläutert. In unserem Fall könnte es etwa wie folgt aussehen:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

Hier sind `GetUniqueRadioButton` und `GetRadioButtonValue` Methoden, die in der Code-Behind-Klasse definiert sind, die die entsprechenden Werte für `id` und `value` Attribute für jedes Optionsfeld zurückgegeben haben. Diese Vorgehensweise eignet sich hervorragend für die Zuweisung der Attribute "`id`" und "`value`", ist aber kurz, wenn der `checked` Attribut Wert angegeben werden muss, da die Datenbindung-Syntax nur ausgeführt wird, wenn Daten zuerst an die GridView gebunden werden. Wenn in der GridView der Ansichts Zustand aktiviert ist, werden die Formatierungs Methoden nur ausgelöst, wenn die Seite zum ersten Mal geladen wird (oder wenn die GridView explizit auf die Datenquelle zurückgesetzt wird). Daher wird die Funktion, die das `checked` Attribut festlegt, beim Postback nicht aufgerufen. Es handelt sich um ein ziemlich feines Problem und etwas über den Rahmen dieses Artikels hinaus. Ich empfehle Ihnen jedoch, den oben beschriebenen Ansatz zu testen und ihn bis zu dem Punkt zu bearbeiten, an dem Sie stecken werden. Während eine solche Übung Ihnen nicht näher an einer funktionierenden Version gewöhnt ist, wird Sie dabei helfen, ein tieferes Verständnis der GridView und des Datenbindung-Lebenszyklus zu erhalten.

Der andere Ansatz zum Einfügen von benutzerdefiniertem Markup auf niedriger Ebene in einer Vorlage und der für dieses Tutorial verwendete Ansatz besteht darin, der Vorlage ein [literales Steuer](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) Element hinzuzufügen. Im GridView s-`RowCreated` oder `RowDataBound`-Ereignishandler kann das Literale Steuerelement Programm gesteuert aufgerufen werden, und die `Text`-Eigenschaft wird auf das auszugebende Markup festgelegt.

Entfernen Sie zunächst das Optionsfeld aus den TemplateField s-`ItemTemplate`, und ersetzen Sie es durch ein Literalsteuerelement. Legen Sie für die Literalsteuerelemente `ID` `RadioButtonMarkup`fest.

[![der ItemTemplate ein Literalsteuerelement hinzufügen](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Abbildung 12**: Hinzufügen eines literalen Steuer Elements zum `ItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))

Erstellen Sie als nächstes einen Ereignishandler für das GridView s `RowCreated`-Ereignis. Das `RowCreated` Ereignis wird einmal für jede hinzugefügte Zeile ausgelöst, unabhängig davon, ob die Daten an die GridView-Ansicht zurückgesetzt werden. Dies bedeutet, dass auch bei einem Postback, bei dem die Daten aus dem Ansichts Zustand neu geladen werden, das `RowCreated` Ereignis weiterhin ausgelöst wird. Dies ist der Grund, warum wir es anstelle von `RowDataBound` verwenden (das nur ausgelöst wird, wenn die Daten explizit an das datenweb-Steuerelement gebunden sind).

In diesem Ereignishandler möchten wir nur fortfahren, wenn wir uns mit einer Daten Zeile beschäftigen. Für jede Daten Zeile möchten wir Programm gesteuert auf das `RadioButtonMarkup` Literalsteuerelement verweisen und dessen `Text`-Eigenschaft auf das auszugebende Markup festlegen. Wie der folgende Code zeigt, erstellt das ausgegebene Markup ein Optionsfeld, dessen `name`-Attribut auf `SuppliersGroup`festgelegt ist, dessen `id`-Attribut auf `RowSelectorX`festgelegt ist, wobei *X* für den Index der GridView-Zeile steht und dessen `value`-Attribut auf den Index der GridView-Zeile festgelegt ist.

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Wenn eine GridView-Zeile ausgewählt und ein Postback auftritt, sind wir an der `SupplierID` des ausgewählten Lieferanten interessiert. Daher könnte man vermuten, dass der Wert jedes Options Felds der tatsächlichen `SupplierID` (und nicht der Index der GridView-Zeile) sein sollte. Dies kann unter bestimmten Umständen funktionieren, aber es ist ein Sicherheitsrisiko, eine `SupplierID`blind zu akzeptieren und zu verarbeiten. Unsere GridView listet z. b. nur die Lieferanten in den USA auf. Wenn die `SupplierID` jedoch direkt aus dem Optionsfeld übergeben wird, was ist, um einen mischiedu-Benutzer daran zu hindern, den `SupplierID` Wert zu bearbeiten, der beim Postback zurückgesendet wird? Wenn Sie den Zeilen Index als `value`verwenden und dann die `SupplierID` beim Postback aus der `DataKeys` Auflistung erhalten, können wir sicherstellen, dass der Benutzer nur einen der `SupplierID` Werte verwendet, die einer der GridView-Zeilen zugeordnet sind.

Nehmen Sie nach dem Hinzufügen dieses Ereignishandlercodes eine Minute Zeit, um die Seite in einem Browser zu testen. Beachten Sie zunächst, dass jeweils nur ein Optionsfeld im Raster ausgewählt werden kann. Wenn Sie jedoch ein Optionsfeld auswählen und auf eine der Schaltflächen klicken, wird ein Postback durchgeführt, und die Options Felder werden in ihren ursprünglichen Zustand zurückversetzt (d. h. beim Postback ist das ausgewählte Optionsfeld nicht mehr ausgewählt). Um dieses Problem zu beheben, müssen wir den `RowCreated` Ereignishandler erweitern, sodass er den ausgewählten Optionsfeld Index überprüft, der aus dem Postback gesendet wurde, und das `checked="checked"` Attribut dem ausgegebenen Markup der Zeilen Index Übereinstimmungen hinzufügt.

Wenn ein Postback auftritt, sendet der Browser die `name` und `value` des ausgewählten Options Felds zurück. Der Wert kann Programm gesteuert mithilfe von `Request.Form("name")`abgerufen werden. Die [`Request.Form`-Eigenschaft](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) stellt eine [`NameValueCollection`](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) dar, die die Formularvariablen darstellt. Bei den Formularvariablen handelt es sich um die Namen und Werte der Formularfelder auf der Webseite, die von dem Webbrowser zurückgesendet werden, wenn ein Postback durchlaufen wird. Da das gerenderte `name`-Attribut der Options Felder in der GridView `SuppliersGroup`ist, sendet der Browser beim Zurücksenden der Webseite `SuppliersGroup=valueOfSelectedRadioButton` zurück an den Webserver (zusammen mit den anderen Formularfeldern). Auf diese Informationen kann dann über die `Request.Form`-Eigenschaft mithilfe von: `Request.Form("SuppliersGroup")`zugegriffen werden.

Da wir den ausgewählten Optionsfeld Index nicht nur im `RowCreated`-Ereignishandler bestimmen müssen, sondern in den `Click`-Ereignis Handlern für die Schaltflächen-websteuer Elemente, können Sie die `SuppliersSelectedIndex`-Eigenschaft der Code-Behind-Klasse hinzufügen, die `-1` zurückgibt, wenn kein Optionsfeld ausgewählt wurde, und den ausgewählten Index, wenn eine der Options Felder ausgewählt ist.

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Wenn diese Eigenschaft hinzugefügt wurde, wissen wir, dass das `checked="checked"` Markup im `RowCreated`-Ereignishandler hinzugefügt werden soll, wenn `SuppliersSelectedIndex` `e.Row.RowIndex`entspricht. Aktualisieren Sie den Ereignishandler, um diese Logik einzubeziehen:

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Mit dieser Änderung bleibt das ausgewählte Optionsfeld nach einem Postback ausgewählt. Nachdem Sie nun die Möglichkeit haben, das Optionsfeld anzugeben, können wir das Verhalten ändern, sodass beim ersten Besuch der Seite das Optionsfeld erste GridView-Zeile s ausgewählt wurde (anstatt standardmäßig Options Felder ausgewählt zu haben. Verhalten). Damit das erste Optionsfeld standardmäßig ausgewählt ist, ändern Sie einfach die `If SuppliersSelectedIndex = e.Row.RowIndex Then`-Anweisung wie folgt: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

An dieser Stelle haben wir der GridView eine Spalte mit gruppierten Options Feldern hinzugefügt, mit der eine einzelne GridView-Zeile ausgewählt und über Postbacks hinweg gespeichert werden kann. In den nächsten Schritten werden die Produkte angezeigt, die vom ausgewählten Lieferanten bereitgestellt werden. In Schritt 4 sehen Sie, wie Sie den Benutzer an eine andere Seite umleiten und dabei den ausgewählten `SupplierID`senden. In Schritt 5 sehen Sie, wie die ausgewählten Lieferanten Produkte in einer GridView auf derselben Seite angezeigt werden.

> [!NOTE]
> Anstatt ein TemplateField (den Schwerpunkt dieses langen Schritts 3) zu verwenden, können wir eine benutzerdefinierte `DataControlField` Klasse erstellen, die die entsprechende Benutzeroberfläche und Funktionalität rendert. Die [`DataControlField`-Klasse](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) ist die Basisklasse, von der die Felder BoundField, CheckBoxField, TemplateField und andere integrierte GridView-und DetailsView-Felder abgeleitet werden. Das Erstellen einer benutzerdefinierten `DataControlField` Klasse bedeutet, dass die Spalte der Options Felder einfach mithilfe von deklarativer Syntax hinzugefügt werden kann. Außerdem wird die Replikation der Funktionalität auf anderen Webseiten und anderen Webanwendungen erheblich vereinfacht.

Wenn Sie schon einmal benutzerdefinierte, kompilierte Steuerelemente in ASP.NET erstellt haben, wissen Sie jedoch, dass dies eine große Menge an Arbeit erfordert und einen Host von Feinheiten und Kanten Fällen beinhaltet, die sorgfältig behandelt werden müssen. Aus diesem Grund wird eine Spalte mit Options Feldern als benutzerdefinierte `DataControlField` Klasse für den Moment implementiert, und die Option TemplateField wird angezeigt. Vielleicht haben wir die Möglichkeit, das Erstellen, verwenden und Bereitstellen von benutzerdefinierten `DataControlField` Klassen in einem zukünftigen Tutorial zu untersuchen.

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Schritt 4: Anzeigen der ausgewählten Lieferanten Produkte auf einer separaten Seite

Nachdem der Benutzer eine GridView-Zeile ausgewählt hat, müssen die ausgewählten Lieferanten Produkte angezeigt werden. In einigen Fällen möchten wir diese Produkte möglicherweise auf einer separaten Seite anzeigen. in anderen Fällen ist es möglicherweise sinnvoll, Sie auf der gleichen Seite zu verwenden. Wir untersuchen zunächst, wie die Produkte auf einer separaten Seite angezeigt werden. in Schritt 5 betrachten wir das Hinzufügen eines GridView-`RadioButtonField.aspx`, um die ausgewählten Lieferanten Produkte anzuzeigen.

Zurzeit sind auf der Seite zwei Schaltflächen-websteuer Elemente vorhanden `ListProducts` und `SendToProducts`. Wenn Sie auf die Schaltfläche "`SendToProducts`" klicken, möchten wir den Benutzer an `~/Filtering/ProductsForSupplierDetails.aspx`senden. Diese Seite wurde im Tutorial [Master/Detail-Filterung über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-vb.md) erstellt und zeigt die Produkte für den Lieferanten an, dessen `SupplierID` durch das QueryString-Feld mit dem Namen `SupplierID`weitergeleitet wird.

Um diese Funktionalität bereitzustellen, erstellen Sie einen Ereignishandler für die `SendToProducts` Schaltfläche s `Click`-Ereignis. In Schritt 3 haben wir die `SuppliersSelectedIndex`-Eigenschaft hinzugefügt, die den Index der Zeile zurückgibt, deren Optionsfeld ausgewählt ist. Die entsprechenden `SupplierID` können aus der GridView s `DataKeys` Collection abgerufen werden, und der Benutzer kann dann mithilfe `Response.Redirect("url")`an `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` gesendet werden.

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Dieser Code funktioniert wunderbar, solange eines der Options Felder aus der GridView ausgewählt ist. Wenn für die GridView anfänglich keine Options Felder ausgewählt sind und der Benutzer auf die Schaltfläche `SendToProducts` klickt, wird `SuppliersSelectedIndex` `-1`. Dies bewirkt, dass eine Ausnahme ausgelöst wird, da `-1` außerhalb des Index Bereichs der `DataKeys` Auflistung liegt. Dies ist jedoch kein Problem, wenn Sie sich entschieden haben, den `RowCreated` Ereignishandler wie in Schritt 3 beschrieben zu aktualisieren, damit das erste Optionsfeld in der GridView anfänglich ausgewählt wird.

Um einen `SuppliersSelectedIndex` Wert `-1`zu erhalten, fügen Sie der Seite oberhalb der GridView ein Label-websteuer Element hinzu. Legen Sie die `ID`-Eigenschaft auf `ChooseSupplierMsg`, die `CssClass`-Eigenschaft auf `Warning`, die `EnableViewState`-und `Visible`-Eigenschaften auf `False`und ihre `Text`-Eigenschaft auf einen Lieferanten aus dem Raster auszuwählen. Die CSS-Klasse `Warning` zeigt Text in einer roten, kursiv, Fett und großen Schriftart an und wird in `Styles.css`definiert. Wenn Sie die Eigenschaften `EnableViewState` und `Visible` auf `False`festlegen, wird die Bezeichnung nicht gerendert, außer nur bei den Postbacks, bei denen die Eigenschaft des Steuer Elements s `Visible` Programm gesteuert auf `True`festgelegt ist.

[![über der GridView ein Label-websteuer Element hinzufügen](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Abbildung 13**: Hinzufügen eines Beschriftungs-websteuer Elements oberhalb der GridView ([zum Anzeigen des Vollbild-Bilds klicken](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))

Erweitern Sie als nächstes den `Click`-Ereignishandler, um die `ChooseSupplierMsg` Bezeichnung anzuzeigen, wenn `SuppliersSelectedIndex` kleiner als 0 (null) ist, und leiten Sie den Benutzer an `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`. andernfalls.

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Besuchen Sie die Seite in einem Browser, und klicken Sie auf die Schaltfläche `SendToProducts`, bevor Sie einen Lieferanten in der GridView auswählen. Wie in Abbildung 14 gezeigt, wird die Bezeichnung `ChooseSupplierMsg` angezeigt. Wählen Sie anschließend einen Lieferanten aus, und klicken Sie auf die Schaltfläche `SendToProducts` Dadurch wird eine Seite angezeigt, auf der die vom ausgewählten Lieferanten gelieferten Produkte aufgeführt sind. Abbildung 15 zeigt die Seite "`ProductsForSupplierDetails.aspx`", wenn der Anbieter "Bigfoot Brauereien" ausgewählt wurde.

[![die Bezeichnung choosesuppliermsg angezeigt, wenn kein Lieferant ausgewählt ist](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Abbildung 14**: die `ChooseSupplierMsg` Bezeichnung wird angezeigt, wenn kein Lieferant ausgewählt ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))

[![die ausgewählten Lieferanten Produkte werden in productforsupplierdetails. aspx angezeigt.](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Abbildung 15**: die ausgewählten Lieferanten Produkte werden in `ProductsForSupplierDetails.aspx` angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))

## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Schritt 5: Anzeigen der ausgewählten Lieferanten Produkte auf derselben Seite

In Schritt 4 wurde erläutert, wie der Benutzer an eine andere Webseite gesendet wird, um die ausgewählten Lieferanten Produkte anzuzeigen. Alternativ können die ausgewählten Lieferanten Produkte auf der gleichen Seite angezeigt werden. Um dies zu veranschaulichen, fügen wir eine weitere GridView zum `RadioButtonField.aspx` hinzu, um die ausgewählten Lieferanten Produkte anzuzeigen.

Da nur diese GridView von Produkten angezeigt werden soll, sobald ein Lieferant ausgewählt ist, fügen Sie unterhalb der `Suppliers` GridView ein Panel-websteuer Element hinzu, und legen Sie dessen `ID` auf `ProductsBySupplierPanel` und dessen `Visible` Eigenschaft auf `False`fest. Fügen Sie im Panel die Text Produkte für den ausgewählten Lieferanten, gefolgt von einer GridView mit dem Namen `ProductsBySupplier`hinzu. Wählen Sie aus dem GridView s-Smarttag eine Bindung an eine neue ObjectDataSource mit dem Namen `ProductsBySupplierDataSource`.

[![das "produczbysupplier GridView" an eine neue "ObjectDataSource" binden.](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Abbildung 16**: Binden der `ProductsBySupplier` GridView an eine neue ObjectDataSource ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))

Konfigurieren Sie als nächstes die ObjectDataSource so, dass Sie die `ProductsBLL`-Klasse verwendet. Da wir nur die Produkte abrufen möchten, die vom ausgewählten Lieferanten bereitgestellt werden, geben Sie an, dass ObjectDataSource die `GetProductsBySupplierID(supplierID)`-Methode aufrufen soll, um die Daten abzurufen. Wählen Sie aus den Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen die Option (keine) aus.

[![konfigurieren Sie ObjectDataSource für die Verwendung der Methode getproductbysupplierid (SupplierID).](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Abbildung 17**: Konfigurieren von ObjectDataSource für die Verwendung der `GetProductsBySupplierID(supplierID)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))

[![in den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" die Dropdown Listen auf (keine) festgelegt.](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Abbildung 18**: Festlegen der Dropdown Listen auf "Aktualisieren", "Einfügen" und "Löschen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))

Nachdem Sie die Registerkarten auswählen, aktualisieren, einfügen und löschen konfiguriert haben, klicken Sie auf Weiter. Da die `GetProductsBySupplierID(supplierID)`-Methode einen Eingabeparameter erwartet, fordert der Assistent zum Erstellen von Datenquellen Sie dazu auf, die Quelle für den Wert des Parameters "s" anzugeben.

Hier finden Sie einige Optionen für die Angabe der Quelle des Parameters-Werts. Wir könnten das Default-Parameter Objekt verwenden und den Wert der `SuppliersSelectedIndex`-Eigenschaft Programm gesteuert dem Parameter s `DefaultValue`-Eigenschaft im `Selecting` Ereignishandler ObjectDataSource s zuweisen. Weitere Informationen zum programmgesteuerten Zuweisen von Werten für die Parameter [Werte von ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) finden Sie Unterprogramm gesteuertes Zuweisen von Werten zu den ObjectDataSource s-Parametern.

Alternativ können wir einen ControlParameter verwenden und auf die `Suppliers` GridView s`SelectedValue`- [Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) verweisen (siehe Abbildung 19). Die GridView s `SelectedValue`-Eigenschaft gibt den `DataKey`-Wert zurück, der der [`SelectedIndex`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx)entspricht. Damit diese Option funktioniert, muss die GridView s `SelectedIndex`-Eigenschaft Programm gesteuert auf die ausgewählte Zeile festgelegt werden, wenn auf die Schaltfläche `ListProducts` geklickt wird. Als zusätzlichen Vorteil wird durch Festlegen des `SelectedIndex`der ausgewählte Datensatz die im `DataWebControls` Design definierte `SelectedRowStyle` (ein gelber Hintergrund) übernehmen.

[![einen ControlParameter verwenden, um den GridView s-SelectedValue als Parameter Quelle anzugeben.](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Abbildung 19**: Verwenden eines ControlParameter zum Angeben des GridView s SelectedValue als Parameter Quelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))

Nach Abschluss des Assistenten werden von Visual Studio automatisch Felder für die Datenfelder des Produkts hinzugefügt. Entfernen Sie alle außer den `ProductName`, `CategoryName`und `UnitPrice` boundfields, und ändern Sie die `HeaderText` Eigenschaften in Product, Category und Price. Konfigurieren Sie das `UnitPrice` BoundField so, dass sein Wert als Währung formatiert ist. Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup Panel, GridView und ObjectDataSource das folgende Aussehen haben:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Um diese Übung abzuschließen, müssen wir die GridView s `SelectedIndex`-Eigenschaft auf die `SelectedSuppliersIndex` festlegen, und die `ProductsBySupplierPanel` Bereich s `Visible` Eigenschaft muss `True` werden, wenn auf die Schaltfläche `ListProducts` geklickt wird. Um dies zu erreichen, erstellen Sie einen Ereignishandler für das `ListProducts` Schaltfläche "Web Control s `Click` Event", und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Wenn ein Lieferant nicht in der GridView ausgewählt wurde, wird die `ChooseSupplierMsg` Bezeichnung angezeigt, und der `ProductsBySupplierPanel` Panel ist ausgeblendet. Wenn ein Lieferant ausgewählt wurde, wird der `ProductsBySupplierPanel` angezeigt, und die GridView s `SelectedIndex`-Eigenschaft wird aktualisiert.

In Abbildung 20 werden die Ergebnisse angezeigt, nachdem der Lieferant Bigfoot Brauereien ausgewählt und auf die Schaltfläche Produkte auf Seite Anzeigen geklickt wurde.

[![die von Bigfoot Brauereien gelieferten Produkte sind auf der gleichen Seite aufgeführt.](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Abbildung 20**: die von Bigfoot Brauereien gelieferten Produkte sind auf der gleichen Seite aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))

## <a name="summary"></a>Zusammenfassung

Wie in der [Master-/Details-Verwendung eines auswählbaren Master GridView-Tutorials mit einem Detail DetailView-](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) Tutorial erläutert, können Datensätze aus einem GridView mithilfe eines CommandField ausgewählt werden, dessen `ShowSelectButton`-Eigenschaft auf `True`festgelegt ist. Das CommandField zeigt seine Schaltflächen jedoch entweder als reguläre Schaltflächen, Links oder Bilder an. Eine alternative Benutzeroberfläche zur Zeilenauswahl besteht darin, ein Optionsfeld oder Kontrollkästchen in jeder GridView-Zeile bereitzustellen. In diesem Tutorial haben wir das Hinzufügen einer Spalte mit Options Feldern untersucht.

Leider ist es nicht möglich, eine Spalte mit Options Feldern hinzuzufügen. Es ist kein integriertes RadioButton Field vorhanden, das beim Klicken auf eine Schaltfläche hinzugefügt werden kann, und die Verwendung des RadioButton-websteuer Elements in einem TemplateField führt einen eigenen Satz von Problemen ein. Um eine solche Schnittstelle bereitzustellen, müssen wir entweder eine benutzerdefinierte `DataControlField` Klasse erstellen oder darauf zurückgreifen, den entsprechenden HTML-Code während des `RowCreated` Ereignisses in ein TemplateField-Element einzubringen.

Nachdem Sie sich mit dem Hinzufügen einer Spalte mit Options Feldern vertraut gemacht haben, können wir uns darauf konzentrieren, eine Spalte mit Kontrollkästchen hinzuzufügen. Mit einer Spalte mit Kontrollkästchen kann ein Benutzer eine oder mehrere GridView-Zeilen auswählen und dann einen Vorgang für alle ausgewählten Zeilen ausführen (z. b. das Auswählen eines Satzes von e-Mails von einem webbasierten e-Mail-Client und das anschließende Löschen aller ausgewählten e-Mails). Im nächsten Tutorial erfahren Sie, wie Sie eine solche Spalte hinzufügen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war David suru. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [Weiter](adding-a-gridview-column-of-checkboxes-vb.md)
