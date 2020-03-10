---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Verwenden von templatefields im GridView-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Um Flexibilität zu schaffen, bietet das GridView-Element das TemplateField-Element, das mithilfe einer Vorlage gerendert wird. Eine Vorlage kann eine Mischung aus statischem HTML, websteuer Elementen und...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3c090dbf65d9acbcc0e343cda5e8da7fff2d35d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78426849"
---
# <a name="using-templatefields-in-the-gridview-control-vb"></a>Verwenden von TemplateFields im GridView-Steuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) oder [PDF herunterladen](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Um Flexibilität zu schaffen, bietet das GridView-Element das TemplateField-Element, das mithilfe einer Vorlage gerendert wird. Eine Vorlage kann eine Mischung aus statischem HTML, websteuer Elementen und Datenbindung-Syntax enthalten. In diesem Tutorial wird erläutert, wie Sie TemplateField verwenden, um ein höheres Anpassungs Niveau mit dem GridView-Steuerelement zu erreichen.

## <a name="introduction"></a>Einführung

Die GridView besteht aus einem Satz von Feldern, die angeben, welche Eigenschaften aus den `DataSource` in der gerenderten Ausgabe enthalten sein sollen und wie die Daten angezeigt werden. Der einfachste Feldtyp ist das BoundField, das einen Datenwert als Text anzeigt. Andere Feldtypen zeigen die Daten mithilfe alternativer HTML-Elemente an. Das CheckBoxField wird z. b. als Kontrollkästchen gerendert, dessen aktivierter Zustand von dem Wert eines angegebenen Daten Felds abhängt. das ImageField rendert ein Bild, dessen Bildquelle auf einem angegebenen Datenfeld basiert. Hyperlinks und Schaltflächen, deren Zustand von einem zugrunde liegenden Daten Feldwert abhängt, können mithilfe der Feld Typen HyperLinkField und ButtonField gerendert werden.

Obwohl die Feldtypen CheckBoxField, ImageField, HyperLinkField und ButtonField eine Alternative Ansicht der Daten zulassen, sind Sie hinsichtlich der Formatierung weiterhin relativ eingeschränkt. Ein CheckBoxField kann nur ein einzelnes Kontrollkästchen anzeigen, während ein ImageField nur ein einzelnes Bild anzeigen kann. Was geschieht, wenn ein bestimmtes Feld Text, ein Kontrollkästchen *und* ein Bild anzeigen muss, die auf unterschiedlichen Daten Feldwerten basieren? Oder was geschieht, wenn die Daten mit einem anderen websteuer Element als dem Kontrollkästchen, dem Bild, dem Hyperlink oder der Schaltfläche angezeigt werden sollen? Außerdem schränkt BoundField die Anzeige auf ein einzelnes Datenfeld ein. Was geschieht, wenn zwei oder mehr Daten Feldwerte in einer einzelnen GridView-Spalte angezeigt werden sollen?

Um dieser Flexibilität Rechnung zu tragen, bietet das GridView-Element das TemplateField-Element, das mithilfe einer *Vorlage*gerendert wird. Eine Vorlage kann eine Mischung aus statischem HTML, websteuer Elementen und Datenbindung-Syntax enthalten. Außerdem verfügt TemplateField über eine Vielzahl von Vorlagen, mit denen das Rendering für verschiedene Situationen angepasst werden kann. Beispielsweise wird die-`ItemTemplate` standardmäßig verwendet, um die Zelle für jede Zeile zu erzeugen, aber die `EditItemTemplate` Vorlage kann verwendet werden, um die Schnittstelle beim Bearbeiten von Daten anzupassen.

In diesem Tutorial wird erläutert, wie Sie TemplateField verwenden, um ein höheres Anpassungs Niveau mit dem GridView-Steuerelement zu erreichen. Im [vorherigen Tutorial](custom-formatting-based-upon-data-vb.md) wurde erläutert, wie die Formatierung basierend auf den zugrunde liegenden Daten mithilfe der `DataBound`-und `RowDataBound`-Ereignishandler angepasst wird. Eine andere Möglichkeit, die Formatierung auf der Grundlage der zugrunde liegenden Daten anzupassen, besteht darin, die Formatierungs Methoden aus einer Vorlage heraus aufrufen. Wir betrachten dieses Verfahren auch in diesem Tutorial.

In diesem Tutorial verwenden wir templatefields, um die Darstellung einer Liste von Mitarbeitern anzupassen. Insbesondere werden alle Mitarbeiter aufgelistet, aber die vor-und Nachnamen des Mitarbeiters in einer Spalte, das Einstellungs Datum in einem Kalender Steuerelement und eine Statusspalte, die angibt, wie viele Tage Sie im Unternehmen verwendet haben.

[![drei templatefields zum Anpassen der Anzeige verwendet.](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Abbildung 1**: drei Vorlagen Felder werden verwendet, um die Anzeige anzupassen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-gridview"></a>Schritt 1: Binden der Daten an die GridView

In Bericht Erstellungs Szenarien, in denen Sie templatefields zum Anpassen der Darstellung verwenden müssen, ist es am einfachsten, wenn Sie zunächst ein GridView-Steuerelement erstellen, das nur boundfields enthält, und dann neue templatefields-Elemente hinzufügen oder die vorhandenen boundfields-Elemente konvertieren. Templatefields nach Bedarf. Aus diesem Grund soll dieses Tutorial gestartet werden, indem der Seite über den Designer eine GridView hinzugefügt und an eine ObjectDataSource gebunden wird, die die Liste der Mitarbeiter zurückgibt. Mit diesen Schritten wird eine GridView mit boundfields für jedes der Mitarbeiter Felder erstellt.

Öffnen Sie die Seite `GridViewTemplateField.aspx`, und ziehen Sie eine GridView aus der Toolbox auf den Designer. Wählen Sie aus dem Smarttag der GridView ein neues ObjectDataSource-Steuerelement aus, das die `GetEmployees()` Methode der `EmployeesBLL` Klasse aufruft.

[![ein neues ObjectDataSource-Steuerelement hinzufügen, das die GetEmployees ()-Methode aufruft.](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen eines neuen ObjectDataSource-Steuer Elements, das die `GetEmployees()`-Methode aufruft ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image6.png))

Wenn Sie die GridView auf diese Weise binden, wird automatisch ein BoundField für die einzelnen Eigenschaften der Mitarbeiter hinzugefügt: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`und `Country`. Bei diesem Bericht geht es nicht darum, die Eigenschaften "`EmployeeID`", "`ReportsTo`" oder "`Country`" anzuzeigen. Um diese boundfields zu entfernen, können Sie Folgendes tun:

- Verwenden Sie das Dialogfeld Felder, klicken Sie auf den Link Spalten bearbeiten im Smarttag der GridView, um dieses Dialogfeld anzuzeigen. Wählen Sie als nächstes die Option boundfields aus der unteren linken Liste aus, und klicken Sie auf die rote X-Schaltfläche, um das BoundField zu entfernen
- Bearbeiten Sie die deklarative Syntax der GridView manuell aus der Quell Ansicht, und löschen Sie das `<asp:BoundField>`-Element für das BoundField, das Sie entfernen möchten.

Nachdem Sie die `EmployeeID`, `ReportsTo`und `Country` boundfields entfernt haben, sollte das Markup der GridView wie folgt aussehen:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Nehmen Sie sich einen Moment Zeit, um den Fortschritt in einem Browser anzuzeigen. An diesem Punkt sollten Sie eine Tabelle mit einem Datensatz für jeden Mitarbeiter und vier Spalten sehen: einen für den Nachnamen des Mitarbeiters, einen für den Vornamen, einen für seinen Titel und einen für das Einstellungs Datum.

[![die Felder "LastName", "FirstName", "Title" und "HireDate" für jeden Mitarbeiter angezeigt.](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Abbildung 3**: die Felder `LastName`, `FirstName`, `Title`und `HireDate` werden für jeden Mitarbeiter angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image9.png))

## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Schritt 2: Anzeigen der vor-und Nachnamen in einer einzelnen Spalte

Derzeit werden die vor-und Nachnamen der einzelnen Mitarbeiter in einer separaten Spalte angezeigt. Es kann sinnvoll sein, Sie stattdessen in einer einzelnen Spalte zu kombinieren. Um dies zu erreichen, müssen wir ein TemplateField-Element verwenden. Wir können entweder ein neues TemplateField-Element hinzufügen, ihm die erforderliche Markup-und Datenbindung-Syntax hinzufügen und dann die `FirstName` und `LastName` boundfields löschen. Alternativ können Sie das `FirstName` BoundField in ein TemplateField-Element konvertieren, das TemplateField-Element so bearbeiten, dass es den `LastName` Wert enthält, und dann das `LastName` BoundField entfernen.

Beide Ansätze sind identisch mit dem gleichen Ergebnis, aber ich persönlich möchte boundfields in templatefields konvertieren, wenn dies möglich ist, da bei der Konvertierung automatisch ein `ItemTemplate` und `EditItemTemplate` mit websteuer Elementen und Datenbindung-Syntax hinzugefügt werden, um das Erscheinungsbild und die Funktionalität des BoundField zu imitieren. Der Vorteil besteht darin, dass wir mit "TemplateField" weniger Arbeit erledigen müssen, da der Konvertierungs Vorgang einige der Arbeit für uns erledigt hat.

Wenn Sie ein vorhandenes BoundField-Element in ein TemplateField-Element konvertieren möchten, klicken Sie auf den Link Spalten bearbeiten im Smarttags von GridView, und klicken Sie dann auf das Dialogfeld Felder. Wählen Sie in der linken unteren Ecke das zu konvertierende BoundField aus der Liste aus, und klicken Sie dann auf den Link "this field into a TemplateField" in der unteren rechten Ecke.

[![ein BoundField-Element aus dem Dialog Feld "Felder" in ein TemplateField-Element konvertieren.](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Abbildung 4**: Konvertieren eines BoundField-Objekts in ein TemplateField-Element aus dem Dialog Feld "Felder" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image12.png))

Konvertieren Sie den `FirstName` BoundField in ein TemplateField. Nach dieser Änderung gibt es keinen perpertiven Unterschied im Designer. Dies liegt daran, dass das Konvertieren von BoundField in ein TemplateField-Element ein TemplateField-Element erstellt, das das Erscheinungsbild des BoundField-Objekts beibehält. Obwohl an dieser Stelle im Designer kein visueller Unterschied vorhanden ist, hat dieser Konvertierungsprozess die deklarative Syntax des BoundField-`<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` mit der folgenden TemplateField-Syntax ersetzt:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Wie Sie sehen, besteht das TemplateField aus zwei Vorlagen: einer `ItemTemplate`, die über eine Bezeichnung verfügt, deren `Text`-Eigenschaft auf den Wert des `FirstName` Datenfelds festgelegt ist, und ein `EditItemTemplate` mit einem TextBox-Steuerelement, dessen `Text`-Eigenschaft ebenfalls auf das `FirstName` Datenfeld festgelegt ist. Die Datenbindung-Syntax-`<%# Bind("fieldName") %>`-gibt an, dass das Datenfeld *`fieldName`* an die angegebene websteuer Element Eigenschaft gebunden ist.

Zum Hinzufügen des `LastName` Daten Feldwerts zu diesem TemplateField müssen wir ein weiteres Label-websteuer Element in der `ItemTemplate` hinzufügen und dessen `Text`-Eigenschaft an `LastName`binden. Dies kann entweder per Hand oder über den Designer erreicht werden. Fügen Sie der `ItemTemplate`einfach die entsprechende deklarative Syntax hinzu:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Um es über den Designer hinzuzufügen, klicken Sie auf den Link Vorlagen bearbeiten im Smarttags von GridView. Dadurch wird die Vorlagen Bearbeitungs Schnittstelle von GridView angezeigt. Das Smarttag dieser Schnittstelle ist eine Liste der Vorlagen in der GridView. Da an dieser Stelle nur ein TemplateField-Element vorhanden ist, sind die einzigen Vorlagen, die in der Dropdown Liste aufgeführt sind, die Vorlagen für die `FirstName` TemplateField zusammen mit den `EmptyDataTemplate` und `PagerTemplate`. Die `EmptyDataTemplate` Vorlage wird verwendet, um die Ausgabe der GridView zu renderten, wenn keine Ergebnisse in den Daten an die GridView gebunden sind. der `PagerTemplate`wird verwendet, um die pagingschnittstelle für eine GridView zu erzeugen, die das Paging unterstützt.

[![die Vorlagen von GridView über den Designer bearbeitet werden können.](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Abbildung 5**: die Vorlagen der GridView können über den Designer bearbeitet werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image15.png))

Um auch die `LastName` in der `FirstName` TemplateField anzuzeigen, ziehen Sie das Label-Steuerelement aus der Toolbox in die `ItemTemplate` des `FirstName` TemplateField in der Vorlagen Bearbeitungs Schnittstelle von GridView.

[![dem ItemTemplate-Element von FirstName TemplateField ein Label-websteuer Element hinzufügen](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Abbildung 6**: Hinzufügen eines Label-websteuer Elements zum ItemTemplate-Element von `FirstName` TemplateField ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image18.png))

An diesem Punkt hat das dem TemplateField hinzugefügte Label-websteuer Element seine `Text`-Eigenschaft auf "Label" festgelegt. Dies muss geändert werden, damit diese Eigenschaft stattdessen an den Wert des `LastName` Datenfelds gebunden wird. Um dies zu erreichen, klicken Sie auf das Smarttag des Bezeichnungs Steuer Elements und wählen die Option "DataBindings bearbeiten" aus.

[![die Option "DataBindings bearbeiten" im Smarttags der Bezeichnung auswählen.](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Abbildung 7**: Auswählen der Option "DataBindings bearbeiten" im Smarttag der Bezeichnung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image21.png))

Dadurch wird das Dialogfeld "DataBindings" angezeigt. Von hier aus können Sie die Eigenschaft für die Datenbindung in der Liste auf der linken Seite auswählen und in der Dropdown Liste auf der rechten Seite das Feld auswählen, an das die Daten gebunden werden sollen. Wählen Sie die `Text`-Eigenschaft von Links und das `LastName` Feld von rechts aus, und klicken Sie auf OK.

[![die Text-Eigenschaft an das LastName-Datenfeld binden.](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Abbildung 8**: Binden der `Text`-Eigenschaft an das `LastName` Datenfeld ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image24.png))

> [!NOTE]
> Im Dialogfeld DataBindings können Sie angeben, ob eine bidirektionale Datenbindung durchgeführt werden soll. Wenn Sie diese Option nicht aktivieren, wird die Datenbindung-Syntax `<%# Eval("LastName")%>` anstelle von `<%# Bind("LastName")%>`verwendet. Beide Vorgehensweisen sind für dieses Tutorial in Ordnung. Das bidirektionale Datenbindung wird beim Einfügen und Bearbeiten von Daten wichtig. Für das einfache Anzeigen von Daten funktioniert jedoch jeder Ansatz gleichermaßen gut. In zukünftigen Tutorials wird das bidirektionale Datenbindung ausführlich erläutert.

Nehmen Sie sich einen Moment Zeit, um diese Seite über einen Browser anzuzeigen. Wie Sie sehen können, enthält die GridView weiterhin vier Spalten. in der Spalte `FirstName` werden nun jedoch *sowohl* die Daten Feldwerte `FirstName` als auch `LastName` aufgelistet.

[![werden die Werte "FirstName" und "LastName" in einer einzelnen Spalte angezeigt.](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Abbildung 9**: die Werte für "`FirstName`" und "`LastName`" werden in einer einzelnen Spalte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image27.png))

Entfernen Sie den `LastName` BoundField, und benennen Sie die `HeaderText`-Eigenschaft `FirstName` TemplateField in "Name" um, um diesen ersten Schritt abzuschließen. Nachdem diese Änderungen vorgenommen wurden, sollte das deklarative Markup der GridView wie folgt aussehen:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]

[![die vor-und Nachnamen der einzelnen Mitarbeiter in einer Spalte angezeigt werden.](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Abbildung 10**: die vor-und Nachnamen der einzelnen Mitarbeiter werden in einer Spalte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image30.png))

## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Schritt 3: Verwenden des Kalender Steuer Elements zum Anzeigen des`HiredDate`Felds

Das Anzeigen eines Daten Feldwerts als Text in einer GridView-Sicht ist so einfach wie die Verwendung eines BoundField. Für bestimmte Szenarien werden die Daten jedoch am besten mit einem bestimmten websteuer Element anstelle von Text ausgedrückt. Eine solche Anpassung der Datenanzeige ist mit templatefields möglich. Anstatt z. b. das Einstellungs Datum des Mitarbeiters als Text anzuzeigen, könnte ein Kalender (mit [dem Kalender Steuer](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)Element) angezeigt werden, bei dem das Einstellungs Datum hervorgehoben ist.

Um dies zu erreichen, konvertieren Sie zunächst den `HiredDate` BoundField in ein TemplateField-Element. Wechseln Sie einfach zum Smarttag der GridView, und klicken Sie auf den Link Spalten bearbeiten, und klicken Sie dann auf das Dialogfeld Felder. Wählen Sie das `HiredDate` BoundField aus, und klicken Sie auf "dieses Feld in ein TemplateField konvertieren".

[![das hireddate BoundField in ein TemplateField-Element konvertieren.](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Abbildung 11**: Konvertieren des `HiredDate` BoundField in ein TemplateField-Element ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image33.png))

Wie in Schritt 2 gezeigt, ersetzt dies das BoundField durch ein TemplateField, das eine `ItemTemplate` enthält und `EditItemTemplate` mit einer Bezeichnung und einem Textfeld, dessen `Text` Eigenschaften mithilfe der Datenbindung-Syntax `<%# Bind("HiredDate")%>`an den `HiredDate` Wert gebunden werden.

Um den Text durch ein Kalender Steuerelement zu ersetzen, bearbeiten Sie die Vorlage, indem Sie die Bezeichnung entfernen und ein Kalender Steuerelement hinzufügen. Wählen Sie im Designer Vorlagen bearbeiten aus dem Smarttag der GridView aus, und wählen Sie in der Dropdown Liste die `ItemTemplate` des `HireDate` TemplateField aus. Löschen Sie anschließend das Label-Steuerelement, und ziehen Sie ein Calendar-Steuerelement aus der Toolbox in die Vorlagen Bearbeitungs Schnittstelle.

[![der ItemTemplate von HireDate TemplateField ein Kalender Steuerelement hinzufügen](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Abbildung 12**: Hinzufügen eines Kalender Steuer Elements zur `ItemTemplate` des `HireDate` TemplateField ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image36.png))

An diesem Punkt enthält jede Zeile in der GridView ein Calendar-Steuerelement in der `HiredDate` TemplateField. Allerdings wird der tatsächliche `HiredDate` Wert des Mitarbeiters nicht an einer beliebigen Stelle im Kalender Steuerelement festgelegt, sodass jedes Kalender Steuerelement standardmäßig den aktuellen Monat und das aktuelle Datum anzeigt. Um dieses Problem zu beheben, müssen wir die `HiredDate` jedes Mitarbeiters den Eigenschaften " [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) " und " [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) " des Kalender Steuer Elements zuweisen.

Wählen Sie im Smarttag des Kalender Steuer Elements die Option DataBindings bearbeiten aus. Binden Sie als nächstes die Eigenschaften `SelectedDate` und `VisibleDate` an das `HiredDate` Datenfeld.

[![Binden der SelectedDate-Eigenschaft und der VisibleDate-Eigenschaft an das hireddate-Datenfeld](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Abbildung 13**: Binden der Eigenschaften "`SelectedDate`" und "`VisibleDate`" an das `HiredDate` Datenfeld ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image39.png))

> [!NOTE]
> Das ausgewählte Datum des Kalender Steuer Elements muss nicht unbedingt sichtbar sein. Ein Kalender kann z. b. den 1<sup>.</sup>August 1999 als ausgewähltes Datum aufweisen, zeigt jedoch den aktuellen Monat und das Jahr an. Das ausgewählte Datum und sichtbare Datumsangaben werden durch die Eigenschaften `SelectedDate` und `VisibleDate` des Kalender Steuer Elements angegeben. Da wir die `HiredDate` des Mitarbeiters auswählen und sicherstellen möchten, dass diese beiden Eigenschaften an das `HireDate` Datenfeld gebunden werden müssen.

Wenn Sie die Seite in einem Browser anzeigen, zeigt der Kalender nun den Monat des Anmelders des Mitarbeiters an und wählt dieses bestimmte Datum aus.

[![das hireddate-Element des Mitarbeiters im Calendar-Steuerelement angezeigt wird.](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Abbildung 14**: der `HiredDate` des Mitarbeiters wird im Kalender Steuerelement angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image42.png))

> [!NOTE]
> Im Gegensatz zu allen bisher bereits gesehenen Beispielen haben wir für dieses Tutorial `EnableViewState`-Eigenschaft *nicht* auf `False` für diese GridView festgelegt. Der Grund für diese Entscheidung liegt darin, dass durch Klicken auf die Datumsangaben des Kalender Steuer Elements ein Postback verursacht wird, wobei das ausgewählte Datum des Kalenders auf das soeben angeklickte Datum festgelegt wird Wenn der Ansichts Zustand der GridView deaktiviert ist, werden die Daten von GridView bei jedem Postback an die zugrunde liegende Datenquelle zurückgesetzt, was bewirkt, dass das ausgewählte Datum des Kalenders auf den `HireDate`des Mitarbeiters *zurück* gesetzt wird und das vom Benutzer gewählte Datum überschrieben wird.

In diesem Tutorial ist dies eine Erörterung, da der Benutzer nicht in der Lage ist, die `HireDate`des Mitarbeiters zu aktualisieren. Wahrscheinlich empfiehlt es sich, das Kalender Steuerelement so zu konfigurieren, dass seine Datumsangaben nicht auswählbar sind. Unabhängig davon, ob in diesem Tutorial der Ansichts Zustand aktiviert werden muss, um bestimmte Funktionen bereitzustellen.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Schritt 4: zeigen Sie die Anzahl der Tage an, die der Mitarbeiter für das Unternehmen gearbeitet hat.

Bisher haben wir zwei Anwendungen von templatefields gesehen:

- Kombinieren von zwei oder mehr Daten Feldwerten in einer Spalte und
- Ausdrücken eines Daten Feldwerts mithilfe eines websteuer Elements anstelle von Text

Eine dritte Verwendung von templatefields besteht darin, Metadaten zu den zugrunde liegenden Daten der GridView anzuzeigen. Zusätzlich zum Anzeigen der Einstellungsdaten der Mitarbeiter ist es z. b. möglich, eine Spalte mit einer Spalte anzuzeigen, in der die Anzahl der Tage angezeigt wird, die für den Auftrag gelten.

In Szenarien, in denen die zugrunde liegenden Daten anders als in der Datenbank gespeichert werden müssen, wird jedoch eine andere Verwendung von templatefields durchführen. Stellen Sie sich vor, dass die `Employees` Tabelle ein `Gender` Feld enthielt, in dem das Zeichen `M` oder `F` gespeichert wurde, um das Geschlecht des Mitarbeiters anzugeben. Wenn diese Informationen auf einer Webseite angezeigt werden, können wir das Geschlecht entweder als "männlich" oder "weiblich" anzeigen, im Gegensatz zu "M" oder "F".

Beide Szenarien können verarbeitet werden, indem eine *Formatierungs Methode* in der Code Behind-Klasse der ASP.NET-Seite erstellt wird (oder in einer separaten Klassenbibliothek, die als `Shared` Methode implementiert ist), die aus der Vorlage aufgerufen wird. Eine solche Formatierungs Methode wird aus der Vorlage mit der gleichen Datenbindung-Syntax aufgerufen, die bereits zuvor gesehen wurde. Die Formatierungs Methode kann eine beliebige Anzahl von Parametern annehmen, muss jedoch eine Zeichenfolge zurückgeben. Diese zurückgegebene Zeichenfolge ist der HTML-Code, der in die Vorlage eingefügt wird.

Um dieses Konzept zu veranschaulichen, soll das Tutorial erweitert werden, um eine Spalte anzuzeigen, in der die Gesamtzahl der Tage aufgeführt ist, in denen ein Mitarbeiter für den Auftrag gearbeitet hat. Diese Formatierungs Methode nimmt ein `Northwind.EmployeesRow` Objekt an und gibt die Anzahl der Tage zurück, in denen der Mitarbeiter als Zeichenfolge verwendet wurde. Diese Methode kann der Code Behind-Klasse der ASP.NET-Seite hinzugefügt werden, *muss* jedoch als `Protected` oder `Public` gekennzeichnet werden, damit Sie über die Vorlage zugänglich ist.

[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Da das `HiredDate` Feld `NULL` Daten Bankwerte enthalten kann, muss zunächst sichergestellt werden, dass der Wert nicht `NULL` ist, bevor die Berechnung fortgesetzt wird. Wenn der `HiredDate` Wert `NULL`ist, wird einfach die Zeichenfolge "unknown" zurückgegeben. Wenn dies nicht `NULL`ist, berechnen wir den Unterschied zwischen der aktuellen Uhrzeit und dem `HiredDate` Wert und geben die Anzahl der Tage zurück.

Um diese Methode zu verwenden, müssen wir Sie mithilfe der Datenbindung-Syntax aus einem TemplateField in der GridView aufrufen. Fügen Sie zunächst ein neues TemplateField-Element zur GridView hinzu, indem Sie auf den Link Spalten bearbeiten im Smarttags von GridView klicken und ein neues TemplateField-Element hinzufügen.

[![der GridView ein neues TemplateField-Element hinzufügen](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Abbildung 15**: Hinzufügen eines neuen TemplateField zur GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image45.png))

Legen Sie die `HeaderText`-Eigenschaft des neuen TemplateField-Objekts auf "Days on the Job" und die `HorizontalAlign`-Eigenschaft des `ItemStyle`auf `Center`fest. Fügen Sie eine `ItemTemplate` hinzu, und verwenden Sie die folgende Datenbindung-Syntax, um die `DisplayDaysOnJob`-Methode aus der Vorlage aufzurufen:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` gibt ein `DataRowView` Objekt zurück, das dem `DataSource` Datensatz entspricht, der an den `GridViewRow`gebunden ist. Die `Row`-Eigenschaft gibt die stark typisierte `Northwind.EmployeesRow`zurück, die an die `DisplayDaysOnJob`-Methode übermittelt wird. Diese Datenbindung-Syntax kann direkt im `ItemTemplate` vorkommen (wie in der deklarativen Syntax unten dargestellt) oder der `Text`-Eigenschaft eines Label-websteuer Elements zugewiesen werden.

> [!NOTE]
> Anstatt eine `EmployeesRow` Instanz zu übergeben, können Sie alternativ den `HireDate` Wert auch mit `<%# DisplayDaysOnJob(Eval("HireDate")) %>`übergeben. Die `Eval`-Methode gibt jedoch eine `Object`zurück. Deshalb müssten wir unsere `DisplayDaysOnJob` Methoden Signatur so ändern, dass Sie einen Eingabeparameter vom Typ "`Object`" annimmt. Wir können den `Eval("HireDate")`-Aufrufe nicht blind in eine `DateTime` umwandeln, da die Spalte `HireDate` in der `Employees` Tabelle `NULL` Werte enthalten kann. Daher müssten wir eine `Object` als Eingabeparameter für die `DisplayDaysOnJob`-Methode akzeptieren, überprüfen, ob Sie über einen `NULL` Wert für die Datenbank verfügt (was mit `Convert.IsDBNull(objectToCheck)`erreicht werden kann), und dann entsprechend vorgehen.

Aufgrund dieser Feinheiten habe ich mich entschieden, die gesamte `EmployeesRow` Instanz zu übergeben. Im nächsten Tutorial sehen Sie ein passendes Beispiel für die Verwendung der `Eval("columnName")`-Syntax zum Übergeben eines Eingabe Parameters in eine Formatierungs Methode.

Das folgende Beispiel zeigt die deklarative Syntax für unsere GridView, nachdem das TemplateField-Element hinzugefügt wurde, und die `DisplayDaysOnJob`-Methode, die vom `ItemTemplate`aufgerufen wurde:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

Abbildung 16 zeigt das abgeschlossene Tutorial, das in einem Browser angezeigt wird.

[![die Anzahl der Tage, die der Mitarbeiter im Auftrag festgelegt wurde.](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Abbildung 16**: die Anzahl der Tage, die der Mitarbeiter für den Auftrag festgelegt wurde ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-gridview-control-vb/_static/image48.png))

## <a name="summary"></a>Zusammenfassung

Das TemplateField im GridView-Steuerelement ermöglicht ein höheres Maß an Flexibilität bei der Anzeige von Daten, als für die anderen Feld Steuerelemente verfügbar ist. Templatefields sind ideal für Situationen, in denen Folgendes gilt:

- In einer GridView-Spalte müssen mehrere Datenfelder angezeigt werden.
- Die Daten werden am besten mithilfe eines websteuer Elements anstelle von nur-Text ausgedrückt.
- Die Ausgabe hängt von den zugrunde liegenden Daten ab, z. b. durch das Anzeigen von Metadaten oder das Neuformatieren der Daten.

Zusätzlich zur Anpassung der Anzeige von Daten werden auch templatefields zum Anpassen der Benutzeroberflächen verwendet, die zum Bearbeiten und Einfügen von Daten verwendet werden, wie in zukünftigen Tutorials zu sehen ist.

In den nächsten beiden Tutorials wird das Untersuchen von Vorlagen fortgesetzt, beginnend mit der Verwendung von templatefields in einer DetailsView. Danach wird die Form View verwendet, in der Vorlagen anstelle von Feldern verwendet werden, um eine größere Flexibilität im Layout und in der Struktur der Daten zu gewährleisten.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Dan Jagers. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](custom-formatting-based-upon-data-vb.md)
> [Weiter](using-templatefields-in-the-detailsview-control-vb.md)
