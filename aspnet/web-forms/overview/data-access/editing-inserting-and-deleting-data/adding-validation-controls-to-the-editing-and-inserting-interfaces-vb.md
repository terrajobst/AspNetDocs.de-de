---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Hinzufügen von Validierungs Steuerelementen zu den Bearbeitungs-und einfügeschnittstellen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erfahren Sie, wie einfach es ist, den EditItemTemplate und InsertItemTemplate eines datenweb-Steuer Elements Validierungs Steuerelemente hinzuzufügen, um mehr zu erfahren...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c5ad110ee0836f0a464b02a2b29254e2e06381e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78479433"
---
# <a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Hinzufügen von Validierungssteuerelementen zu Oberflächen zum Bearbeiten und Einfügen (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) oder [PDF herunterladen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> In diesem Lernprogramm erfahren Sie, wie einfach es ist, der EditItemTemplate und der InsertItemTemplate eines datenweb-Steuer Elements Validierungs Steuerelemente hinzuzufügen, um eine weitere narrensicher-Benutzeroberfläche bereitzustellen.

## <a name="introduction"></a>Einführung

Die GridView-und DetailsView-Steuerelemente in den Beispielen, die wir in den letzten drei Tutorials untersucht haben, bestehen aus boundfields und checkboxfields (die Feldtypen, die von Visual Studio automatisch hinzugefügt wurden, wenn eine GridView oder DetailsView an eine Datenquelle gebunden wurde. Steuern über das Smarttags). Wenn Sie eine Zeile in einer GridView oder DetailsView bearbeiten, werden diese boundfields-Dateien, die nicht schreibgeschützt sind, in Textfelder konvertiert, aus denen der Endbenutzer die vorhandenen Daten ändern kann. Ebenso werden beim Einfügen eines neuen Datensatzes in ein DetailsView-Steuerelement die boundfields-Eigenschaft, deren `InsertVisible`-Eigenschaft auf `True` (Standard) festgelegt ist, als leere Textfelder gerendert, in die der Benutzer die Feldwerte des neuen Datensatzes bereitstellen kann. Ebenso werden checkboxfields, die in der schreibgeschützten Standardschnittstelle deaktiviert sind, in aktivierte Kontrollkästchen in den Bearbeitungs-und einfügeschnittstellen konvertiert.

Die standardmäßigen Bearbeitungs-und einfügeschnittstellen für BoundField und CheckBoxField sind zwar hilfreich, aber die Schnittstelle verfügt über keine Validierung. Wenn ein Benutzer einen Dateneingabe Fehler vornimmt, z. b. das `ProductName` Feld auslassen oder einen ungültigen Wert für `UnitsInStock` (z. b.-50) eingeben, wird eine Ausnahme in den Tiefen der Anwendungsarchitektur ausgelöst. Diese Ausnahme kann, wie im [vorherigen Tutorial](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)gezeigt, ordnungsgemäß behandelt werden. im Idealfall würde die Bearbeitungs-oder einfügende Benutzeroberfläche Validierungs Steuerelemente enthalten, um zu verhindern, dass ein Benutzer solche ungültigen Daten überhaupt eingibt.

Um eine angepasste Bearbeitungs-oder einfügeschnittstelle bereitzustellen, müssen Sie BoundField oder CheckBoxField durch ein TemplateField ersetzen. Templatefields, das Thema der [Verwendung von templatefields im GridView-Steuer](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Element und [die Verwendung von templatefields in den DetailsView-Steuer](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) Element-Tutorials, kann aus mehreren Vorlagen bestehen, die separate Schnittstellen für verschiedene Zeilen Zustände definieren. Der `ItemTemplate` von TemplateField wird beim Rendern Schreib geschützter Felder oder Zeilen in den DetailsView-oder GridView-Steuerelementen verwendet, während der `EditItemTemplate` und `InsertItemTemplate` die Schnittstellen angeben, die für den Bearbeitungs-bzw. Einfügemodus verwendet werden sollen.

In diesem Lernprogramm erfahren Sie, wie einfach es ist, dem `EditItemTemplate` des TemplateField-Steuer Elements Validierungs Steuerelemente hinzuzufügen und `InsertItemTemplate`, um eine Benutzeroberfläche mit einer größeren Größe zu erstellen. Insbesondere wird in diesem Tutorial das Beispiel veranschaulicht, das in der unter [suchung der mit dem Einfügen, aktualisieren und löschen verknüpften Ereignisse](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) erstellt wurde, und erweitert die Bearbeitungs-und einfügeschnittstellen, um die entsprechende Validierung zu berücksichtigen.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deleting"></a>Schritt 1: Replizieren des Beispiels aus[der Untersuchung der Ereignisse im Zusammenhang mit dem Einfügen, aktualisieren und löschen](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

Im Tutorial unter [Suchen der mit dem Einfügen, aktualisieren und löschen verknüpften Ereignisse](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) haben wir eine Seite erstellt, auf der die Namen und Preise der Produkte in einer bearbeitbaren GridView aufgelistet sind. Darüber hinaus enthält die Seite eine DetailsView, deren `DefaultMode`-Eigenschaft auf `Insert`festgelegt wurde, wodurch sich das Rendering immer im Einfügemodus befindet. In dieser DetailsView könnte der Benutzer den Namen und den Preis für ein neues Produkt eingeben, auf Einfügen klicken und ihn dem System hinzufügen (siehe Abbildung 1).

[![im vorherigen Beispiel haben Benutzer die Möglichkeit, neue Produkte hinzuzufügen und vorhandene zu bearbeiten.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Abbildung 1**: das vorherige Beispiel ermöglicht Benutzern das Hinzufügen neuer Produkte und das Bearbeiten vorhandener Produkte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png)).

Unser Ziel dieses Tutorials besteht darin, die DetailsView und GridView zu erweitern, um Validierungs Steuerelemente bereitzustellen. Die Validierungs Logik führt insbesondere Folgendes aus:

- Beim Einfügen oder Bearbeiten eines Produkts muss der Name angegeben werden.
- Erfordert, dass der Preis beim Einfügen eines Datensatzes angegeben wird. beim Bearbeiten eines Datensatzes benötigen wir weiterhin einen Preis, verwenden jedoch die programmgesteuerte Logik im `RowUpdating` Ereignishandler der GridView, der bereits im vorherigen Tutorial vorhanden ist.
- Stellen Sie sicher, dass der für den Preis eingegebene Wert ein gültiges Währungs Format ist.

Bevor wir uns mit der Erweiterung des vorherigen Beispiels befassen, um die Validierung einzuschließen, müssen wir zuerst das Beispiel von der Seite `DataModificationEvents.aspx` auf die Seite für dieses Tutorial, `UIValidation.aspx`, replizieren. Um dies zu erreichen, müssen sowohl das deklarative Markup der `DataModificationEvents.aspx` Seite als auch der zugehörige Quellcode kopiert werden. Kopieren Sie zuerst das deklarative Markup, indem Sie die folgenden Schritte ausführen:

1. Öffnen der Seite "`DataModificationEvents.aspx`" in Visual Studio
2. Wechseln Sie zum deklarativen Markup der Seite (Klicken Sie am unteren Rand der Seite auf die Quell Schaltfläche).
3. Kopieren Sie den Text innerhalb der `<asp:Content>` und `</asp:Content>` Tags (Zeilen 3 bis 44), wie in Abbildung 2 dargestellt.

[![den Text innerhalb des &lt;ASP: Content&gt;-Steuer Elements kopieren](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Abbildung 2**: Kopieren des Texts innerhalb des `<asp:Content>` Steuer Elements ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))

1. Öffnen der Seite "`UIValidation.aspx`"
2. Zum deklarativen Markup der Seite wechseln
3. Fügen Sie den Text in das `<asp:Content>`-Steuerelement ein.

Um den Quellcode zu kopieren, öffnen Sie die Seite `DataModificationEvents.aspx.vb`, und kopieren Sie nur den Text *in* der `EditInsertDelete_DataModificationEvents`-Klasse. Kopieren Sie die drei Ereignishandler (`Page_Load`, `GridView1_RowUpdating`und `ObjectDataSource1_Inserting`), aber kopieren Sie **nicht** die Klassen Deklaration. Fügen Sie den kopierten Text *innerhalb* der `EditInsertDelete_UIValidation` Klasse in `UIValidation.aspx.vb`ein.

Nachdem Sie den Inhalt und den Code von `DataModificationEvents.aspx` zu `UIValidation.aspx`verschoben haben, nehmen Sie sich einen Moment Zeit, um den Fortschritt in einem Browser zu testen. Die gleiche Ausgabe sollte angezeigt werden, und es sollte dieselbe Funktionalität auf jeder dieser beiden Seiten angezeigt werden (Weitere Informationen finden Sie in Abbildung 1 für einen Screenshot der `DataModificationEvents.aspx` in Aktion).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Schritt 2: Konvertieren von boundfields in templatefields

Um den Bearbeitungs-und einfügeschnittstellen Validierungs Steuerelemente hinzuzufügen, müssen die von den DetailsView-und GridView-Steuerelementen verwendeten boundfields in templatefields konvertiert werden. Um dies zu erreichen, klicken Sie auf die Verknüpfungen Spalten bearbeiten und Felder bearbeiten in den Smarttags von GridView und DetailsView. Wählen Sie dort jedes der boundfields aus, und klicken Sie auf den Link "dieses Feld in einen TemplateField konvertieren".

[![jede der boundfields-und GridView-Felder der DetailsView in templatefields konvertieren.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Abbildung 3**: Konvertieren der boundfields-Felder der DetailsView und der GridView in templatefields ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))

Durch das Konvertieren eines BoundField in ein TemplateField über das Dialogfeld "Felder" wird ein TemplateField generiert, das die gleichen schreibgeschützten, Bearbeitungs-und einfügeschnittstellen wie das BoundField selbst aufweist. Das folgende Markup zeigt die deklarative Syntax für das `ProductName`-Feld in der DetailsView, nachdem es in ein TemplateField-Element konvertiert wurde:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Beachten Sie, dass in diesem TemplateField drei Vorlagen automatisch erstellt wurden `ItemTemplate`, `EditItemTemplate`und `InsertItemTemplate`. Der `ItemTemplate` zeigt einen einzelnen Daten Feldwert (`ProductName`) mithilfe eines Bezeichnungs-websteuer Elements an, während der `EditItemTemplate` und `InsertItemTemplate` den Wert des Daten Felds in einem TextBox-websteuer Element darstellen, das das Datenfeld mit der `Text`-Eigenschaft des Textfelds mit bidirektionaler Datenbindung verknüpft. Da wir nur die DetailsView auf dieser Seite zum Einfügen verwenden, können Sie die `ItemTemplate` und `EditItemTemplate` aus den beiden templatefields entfernen, auch wenn Sie nicht beeinträchtigt werden.

Da die GridView die integrierten einfügefeatures der DetailsView nicht unterstützt, führt das Konvertieren des `ProductName` Felds der GridView in ein TemplateField zu einer `ItemTemplate` und `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Durch Klicken auf das "konvertieren dieses Felds in ein TemplateField" hat Visual Studio ein TemplateField erstellt, dessen Vorlagen die Benutzeroberfläche des konvertierten BoundField imitieren. Sie können dies überprüfen, indem Sie diese Seite über einen Browser aufrufen. Sie werden feststellen, dass die Darstellung und das Verhalten von templatefields mit der Darstellung identisch sind, wenn boundfields stattdessen verwendet wurde.

> [!NOTE]
> Sie können die Bearbeitungs Schnittstellen in den Vorlagen nach Bedarf anpassen. Beispielsweise kann es sein, dass das Textfeld in der `UnitPrice` templatefields als kleineres Textfeld gerendert wird als das Textfeld `ProductName`. Um dies zu erreichen, können Sie die `Columns`-Eigenschaft des Textfelds auf einen geeigneten Wert festlegen oder über die `Width`-Eigenschaft eine absolute Breite angeben. Im nächsten Tutorial erfahren Sie, wie Sie die Bearbeitungs Schnittstelle vollständig anpassen, indem Sie das Textfeld durch ein alternatives Dateneingabe-websteuer Element ersetzen.

## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Schritt 3: Hinzufügen der Validierungs Steuerelemente zum`EditItemTemplate` s der GridView

Beim Erstellen von Dateneingabe Formularen ist es wichtig, dass Benutzer alle erforderlichen Felder eingeben und dass alle bereitgestellten Eingaben gültige und korrekt formatierte Werte sind. Um sicherzustellen, dass die Eingaben eines Benutzers gültig sind, bietet ASP.net fünf integrierte Validierungs Steuerelemente, die zum Überprüfen des Werts eines einzelnen Eingabe Steuer Elements verwendet werden können:

- "Requirements [dfieldvalidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) " stellt sicher, dass ein Wert angegeben wurde.
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) überprüft einen Wert mit einem anderen websteuerungs Wert oder einem konstanten Wert oder stellt sicher, dass das Format des Werts für einen angegebenen Datentyp zulässig ist.
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) stellt sicher, dass ein Wert innerhalb eines Wertebereichs liegt.
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) überprüft einen Wert anhand eines [regulären Ausdrucks](http://en.wikipedia.org/wiki/Regular_expression) .
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) überprüft einen Wert mit einer benutzerdefinierten benutzerdefinierten Methode.

Weitere Informationen zu diesen fünf Steuerelementen finden Sie im [Abschnitt Validierungs Steuerelemente](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) der [ASP.NET-Schnellstart-Tutorials](https://asp.net/QuickStart/aspnet/).

Für unser Tutorial müssen wir ein "Requirements dfieldvalidator" in den `ProductName` templatefields von DetailsView und GridView und im `UnitPrice` TemplateField der DetailsView ein "Requirements dfieldvalidator" verwenden. Außerdem müssen wir der `UnitPrice` templatefields-Steuerelemente ein CompareValidator-Steuerelement hinzufügen, das sicherstellt, dass der eingegebene Preis einen Wert größer oder gleich 0 (null) aufweist und in einem gültigen Währungs Format dargestellt wird.

> [!NOTE]
> Während ASP.NET 1. x dieselben fünf Validierungs Steuerelemente enthielt, hat ASP.NET 2,0 eine Reihe von Verbesserungen hinzugefügt. die beiden wichtigsten sind die Client seitige Skriptunterstützung für andere Browser als Internet Explorer und die Möglichkeit, Validierungs Steuerelemente auf einer Seite in zu partitionieren. Validierungs Gruppen. Weitere Informationen zu den neuen Validierungs Steuerungsfunktionen in 2,0 finden Sie unter [Dissecting the Validation Controls in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Fügen Sie zunächst die erforderlichen Validierungs Steuerelemente zu den `EditItemTemplate` s in den templatefields-Elementen der GridView hinzu. Um dies zu erreichen, klicken Sie auf den Link Vorlagen bearbeiten des Smarttags von GridView, um die Vorlagen Bearbeitungs Schnittstelle aufzubringen. Von hier aus können Sie die zu bearbeitende Vorlage in der Dropdown Liste auswählen. Da die Bearbeitungs Schnittstelle erweitert werden soll, müssen Sie der `ProductName` und `UnitPrice`der `EditItemTemplate` s Validierungs Steuerelemente hinzufügen.

[![wir die edititemtemplates von ProductName und UnitPrice erweitern müssen.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Abbildung 4**: Wir müssen die `ProductName` und `UnitPrice`der `EditItemTemplate` s erweitern ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))

Fügen Sie in der `ProductName` `EditItemTemplate`ein "Requirements dfieldvalidator" hinzu, indem Sie es aus der Toolbox in die Vorlagen Bearbeitungs Schnittstelle ziehen und hinter das Textfeld platzieren.

[![dem ProductName EditItemTemplate ein "Requirements dfieldvalidator" hinzufügen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Abbildung 5**: Hinzufügen eines "Requirements dfieldvalidator" zum `ProductName` `EditItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))

Alle Validierungs Steuerelemente funktionieren, indem die Eingabe eines einzelnen ASP.net-websteuer Elements überprüft wird. Daher müssen wir angeben, dass das soeben hinzugefügte "Requirements dfieldvalidator" mit dem Textfeld im `EditItemTemplate`überprüft werden soll. Dies wird erreicht, indem die [ControlToValidate-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) des Validierungs Steuer Elements auf die `ID` des entsprechenden websteuer Elements festgelegt wird. Das Textfeld verfügt zurzeit über den nicht beschreibenden `ID` `TextBox1`, aber wir ändern ihn in etwas geeignetere. Klicken Sie in der Vorlage auf das Textfeld, und ändern Sie dann im Eigenschaftenfenster die `ID` von `TextBox1` in `EditProductName`.

[![Ändern der Textfeld-ID in editproductname](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Abbildung 6**: Ändern der `ID` des Textfelds in `EditProductName` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))

Legen Sie als nächstes die `ControlToValidate`-Eigenschaft von Requirements dfieldvalidator auf `EditProductName`fest. Legen Sie abschließend die [Eigenschaft ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) auf "Sie müssen den Namen des Produkts bereitstellen" und die [Text-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) auf "\*" fest. Der `Text`-Eigenschafts Wert (sofern vorhanden) ist der Text, der vom Validierungs Steuerelement angezeigt wird, wenn die Validierung fehlschlägt. Der `ErrorMessage`-Eigenschafts Wert, der erforderlich ist, wird vom ValidationSummary-Steuerelement verwendet. Wenn der `Text`-Eigenschafts Wert weggelassen wird, ist der `ErrorMessage`-Eigenschafts Wert auch der Text, der vom Validierungs Steuerelement bei ungültiger Eingabe angezeigt wird.

Nachdem Sie diese drei Eigenschaften von "Requirements dfieldvalidator" festgelegt haben, sollte der Bildschirm in etwa wie in Abbildung 7 aussehen.

[![die Eigenschaften "controldevalidate", "ErrorMessage" und "Text" von "Requirements dfieldvalidator" festlegen.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Abbildung 7**: Festlegen der Eigenschaften `ControlToValidate`, `ErrorMessage`und `Text` von "Requirements dfieldvalidator" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))

Wenn das Element "Requirements dfieldvalidator" dem `ProductName` `EditItemTemplate`hinzugefügt wurde, müssen Sie nur noch dem `UnitPrice` `EditItemTemplate`die erforderliche Validierung hinzufügen. Da wir beschlossen haben, dass die `UnitPrice` bei der Bearbeitung eines Datensatzes optional ist, müssen wir kein "Requirements dfieldvalidator" hinzufügen. Wir müssen jedoch ein CompareValidator hinzufügen, um sicherzustellen, dass die `UnitPrice`, falls angegeben, ordnungsgemäß als Währung formatiert ist und größer oder gleich 0 ist.

Bevor wir das CompareValidator der `UnitPrice` `EditItemTemplate`hinzufügen, ändern wir zuerst die ID des TextBox-websteuer Elements von `TextBox2` in `EditUnitPrice`. Fügen Sie nach dieser Änderung das CompareValidator hinzu, und legen Sie dessen `ControlToValidate`-Eigenschaft auf `EditUnitPrice`, dessen `ErrorMessage`-Eigenschaft auf "der Preis muss größer oder gleich 0 (null) und kann das Währungssymbol nicht enthalten sein" und dessen `Text`-Eigenschaft auf "\*" festgelegt werden.

Um anzugeben, dass der `UnitPrice` Wert größer oder gleich 0 sein muss, legen Sie die [Operator-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) des CompareValidator auf `GreaterThanEqual`, seine [ValueToCompare-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) auf "0" und seine Type- [Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) auf "`Currency`" fest. Die folgende deklarative Syntax zeigt die `EditItemTemplate` des `UnitPrice` TemplateField, nachdem diese Änderungen vorgenommen wurden:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Nachdem Sie diese Änderungen vorgenommen haben, öffnen Sie die Seite in einem Browser. Wenn Sie versuchen, den Namen auszulassen oder einen ungültigen Preis einzugeben, wenn Sie ein Produkt bearbeiten, wird ein Sternchen neben dem Textfeld angezeigt. Wie in Abbildung 8 gezeigt, wird ein Preis Wert, der das Währungssymbol wie $19,95 enthält, als ungültig angesehen. Die `Currency` `Type` des CompareValidator ermöglicht Ziffern Trennzeichen (z. b. Kommas oder Zeiträume, abhängig von den Kultur Einstellungen) und ein führendes Plus-oder Minuszeichen, *lässt jedoch kein Währungs* Symbol zu. Dieses Verhalten kann Benutzer perplex sein, da die Bearbeitungs Schnittstelle die `UnitPrice` derzeit unter Verwendung des Währungs formats rendert.

> [!NOTE]
> Beachten Sie, dass in den Ereignissen, die dem Tutorial zum *Einfügen, aktualisieren und löschen zugeordnet sind* , die `DataFormatString`-Eigenschaft von BoundField auf `{0:c}` festgelegt wird, um Sie als Währung zu formatieren. Außerdem wird die `ApplyFormatInEditMode`-Eigenschaft auf true festgelegt, sodass die Bearbeitungs Schnittstelle von GridView das `UnitPrice` als Währung formatiert. Beim Konvertieren von BoundField in ein TemplateField hat Visual Studio diese Einstellungen notiert und die `Text`-Eigenschaft des Textfelds als Währung mithilfe der Datenbindung-Syntax `<%# Bind("UnitPrice", "{0:c}") %>`formatiert.

[![ein Sternchen neben den Textfeldern mit ungültiger Eingabe angezeigt wird.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Abbildung 8**: ein Sternchen wird neben den Textfeldern mit ungültiger Eingabe angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))

Während die Überprüfung unverändert funktioniert, muss der Benutzer das Währungssymbol beim Bearbeiten eines Datensatzes manuell entfernen, was nicht zulässig ist. Um dieses Problem zu beheben, haben wir drei Möglichkeiten:

1. Konfigurieren Sie die `EditItemTemplate` so, dass der `UnitPrice` Wert nicht als Währung formatiert ist.
2. Ermöglicht es dem Benutzer, ein Währungssymbol einzugeben, indem das CompareValidator entfernt und durch ein RegularExpressionValidator ersetzt wird, das ordnungsgemäß einen ordnungsgemäß formatierten Währungswert überprüft. Das Problem hier ist, dass der reguläre Ausdruck zum Überprüfen eines Währungs Werts nicht ganz so ist, dass es erforderlich ist, Code zu schreiben, wenn Kultur Einstellungen integriert werden sollen.
3. Entfernen Sie das Validierungs Steuerelement vollständig, und verlassen Sie sich auf die serverseitige Validierungs Logik im `RowUpdating` Ereignishandler von GridView.

Lassen Sie uns die Option #1 für diese Übung verwenden. Derzeit wird die `UnitPrice` als Währung formatiert, weil der Datenbindung-Ausdruck für das Textfeld in der `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`ist. Ändern Sie die BIND-Anweisung in `Bind("UnitPrice", "{0:n2}")`, wodurch das Ergebnis als Zahl mit zwei Ziffern der Genauigkeit formatiert wird. Dies kann direkt über die deklarative Syntax erfolgen oder durch Klicken auf den Link "DataBindings bearbeiten" im Textfeld "`EditUnitPrice`" in der `UnitPrice` TemplateField-`EditItemTemplate` (siehe Abbildung 9 und 10).

[![klicken Sie auf den Link "DataBindings bearbeiten" des Textfelds.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Abbildung 9**: Klicken auf den Link "DataBindings bearbeiten" des Textfelds ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))

[![den Format Bezeichner in der Bind-Anweisung anzugeben.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Abbildung 10**: Angeben des Format Bezeichnern in der `Bind`-Anweisung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))

Durch diese Änderung schließt der formatierte Preis in der Bearbeitungs Schnittstelle Kommas als Trennzeichen für die Gruppe und einen Punkt als Dezimaltrennzeichen ein, aber verlässt das Währungssymbol.

> [!NOTE]
> Der `UnitPrice` `EditItemTemplate` enthält kein "Requirements dfieldvalidator", sodass das Postback und die Aktualisierungs Logik beginnen können. Der `RowUpdating` Ereignishandler, der aus der unter *suchung der mit dem Einfügen, aktualisieren und löschen verknüpften Ereignisse* kopiert wurde, enthält jedoch eine programmgesteuerte Prüfung, mit der sichergestellt wird, dass die `UnitPrice` bereitgestellt wird. Sie können diese Logik entfernen, unverändert belassen oder dem `UnitPrice` `EditItemTemplate`ein "Requirements dfieldvalidator" hinzufügen.

## <a name="step-4-summarizing-data-entry-problems"></a>Schritt 4: Zusammenfassung der Dateneingabe Probleme

Zusätzlich zu den fünf Validierungs Steuerelementen enthält ASP.NET das [ValidationSummary-Steuer](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)Element, das die `ErrorMessage` en dieser Validierungs Steuerelemente anzeigt, die ungültige Daten erkannt haben. Diese Zusammenfassungs Daten können als Text auf der Webseite oder über eine modale Client seitige MessageBox angezeigt werden. Wir möchten dieses Tutorial erweitern, um eine Client seitige MessageBox mit einer Zusammenfassung aller Überprüfungs Probleme zu integrieren.

Ziehen Sie hierzu ein ValidationSummary-Steuerelement aus der Toolbox auf den Designer. Der Speicherort des Validierungs Steuer Elements ist nicht ganz wichtig, da wir ihn so konfigurieren, dass nur die Zusammenfassung als MessageBox angezeigt wird. Nachdem Sie das Steuerelement hinzugefügt haben, legen Sie dessen [ShowSummary-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) auf `False` und dessen [ShowMessageBox-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) auf `True`fest. Mit dieser Addition werden alle Validierungs Fehler in einer Client seitigen MessageBox zusammengefasst.

[![werden die Validierungs Fehler in einer Client seitigen MessageBox zusammengefasst.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Abbildung 11**: die Validierungs Fehler werden in einer Client seitigen MessageBox zusammengefasst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))

## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Schritt 5: Hinzufügen der Validierungs Steuerelemente zum`InsertItemTemplate` der DetailsView

Für dieses Lernprogramm müssen lediglich die Validierungs Steuerelemente zur einfügeschnittstelle der DetailsView hinzugefügt werden. Der Vorgang zum Hinzufügen von Validierungs Steuerelementen zu den Vorlagen der DetailsView ist mit der in Schritt 3 untersuchten identisch. aus diesem Grund wird die Aufgabe in diesem Schritt durch die Aufgabe überschritten. Wie bei den `EditItemTemplate` en der GridView, empfehle ich Ihnen, die `ID` s der Textfelder aus der nicht beschreibenden `TextBox1` umbenennen und `TextBox2` `InsertProductName` und `InsertUnitPrice`.

Fügen Sie dem `ProductName` `InsertItemTemplate`ein "Requirements dfieldvalidator"-Element hinzu. Legen Sie den `ControlToValidate` auf den `ID` des Textfelds in der Vorlage fest, dessen `Text`-Eigenschaft auf "\*" und deren `ErrorMessage`-Eigenschaft auf "Sie müssen den Namen des Produkts angeben".

Da der `UnitPrice` für diese Seite beim Hinzufügen eines neuen Datensatzes erforderlich ist, fügen Sie der `UnitPrice` `InsertItemTemplate`ein "Requirements dfieldvalidator"-Element hinzu, und legen Sie dessen Eigenschaften für `ControlToValidate`, `Text`und `ErrorMessage` entsprechend fest. Fügen Sie abschließend der `UnitPrice` `InsertItemTemplate` ein CompareValidator-Element hinzu, und konfigurieren Sie die Eigenschaften `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`und `ValueToCompare` genauso wie mit dem CompareValidator des `UnitPrice`in der `EditItemTemplate`der GridView.

Nach dem Hinzufügen dieser Validierungs Steuerelemente kann dem System kein neues Produkt hinzugefügt werden, wenn der Name nicht angegeben ist oder wenn der Preis eine negative Zahl oder eine negative Zahl ist.

[der einfügeschnittstelle der DetailsView wurde ![Validierungs Logik hinzugefügt.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Abbildung 12**: Validierungs Logik wurde der einfügeschnittstelle der DetailsView hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))

## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Schritt 6: Partitionierung der Validierungs Steuerelemente in Validierungs Gruppen

Unsere Seite besteht aus zwei logisch unterschiedlichen Sätzen von Validierungs Steuerelementen: solche, die der Bearbeitungs Schnittstelle von GridView entsprechen, und diejenigen, die der einfügeschnittstelle der DetailsView entsprechen. Standardmäßig werden bei einem Postback *alle* Validierungs Steuerelemente auf der Seite überprüft. Wenn Sie jedoch einen Datensatz bearbeiten, möchten wir nicht, dass die Validierungs Steuerelemente der einfügeschnittstelle der DetailsView überprüft werden. Abbildung 13 zeigt das aktuelle Problem, wenn ein Benutzer ein Produkt mit vollständig gültigen Werten bearbeitet. Wenn Sie auf Aktualisieren klicken, wird ein Überprüfungs Fehler verursacht, da die Werte für Name und Preis in der einfügeschnittstelle leer sind.

[![Aktualisieren eines Produkts bewirkt, dass die Validierungs Steuerelemente der einfügeschnittstelle ausgelöst werden.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Abbildung 13**: das Aktualisieren eines Produkts bewirkt, dass die Validierungs Steuerelemente der einfügeschnittstelle ausgelöst werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))

Die Validierungs Steuerelemente in ASP.NET 2,0 können über Ihre `ValidationGroup`-Eigenschaft in Validierungs Gruppen partitioniert werden. Um einer Gruppe eine Reihe von Validierungs Steuerelementen zuzuordnen, legen Sie die `ValidationGroup`-Eigenschaft einfach auf denselben Wert fest. Legen Sie für dieses Tutorial die `ValidationGroup` Eigenschaften der Validierungs Steuerelemente in den templatefields-Steuerelementen der GridView auf `EditValidationControls` und die `ValidationGroup`-Eigenschaften der templatefields-Eigenschaft der DetailsView auf `InsertValidationControls`fest. Diese Änderungen können direkt im deklarativen Markup oder über die Eigenschaftenfenster durchgeführt werden, wenn die Designer-Vorlagen Schnittstellen Vorlage verwendet wird.

Zusätzlich zu den Validierungs Steuerelementen enthalten die Schaltflächen-und Schaltflächen bezogenen Steuerelemente in ASP.NET 2,0 auch eine `ValidationGroup`-Eigenschaft. Die Validierungs Steuerelemente einer Validierungs Gruppe werden nur dann auf Gültigkeit überprüft, wenn ein Postback durch eine Schaltfläche ausgelöst wird, die dieselbe `ValidationGroup` Eigenschaften Einstellung hat. Damit z. b. die einfügeschaltfläche "DetailsView" die `InsertValidationControls` Validierungs Gruppe auslöst, müssen wir die Eigenschaft "`ValidationGroup`" des CommandField auf "`InsertValidationControls`" festlegen (siehe Abbildung 14). Legen Sie außerdem die `ValidationGroup`-Eigenschaft von GridView auf `EditValidationControls`fest.

[![die ValidationGroup-Eigenschaft der DetailsView auf insertvalidationcontrols festlegen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Abbildung 14**: Festlegen der Eigenschaft "commandfieldes `ValidationGroup`" der DetailsView auf `InsertValidationControls` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))

Nachdem diese Änderungen vorgenommen wurden, sollten die templatefields und commandfields der DetailsView und GridView in etwa wie folgt aussehen:

Templatefields und CommandField der DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField und templatefields der GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

An diesem Punkt werden die Bearbeitungs Steuerelemente nur dann ausgelöst, wenn auf die Schaltfläche "Aktualisieren" der GridView geklickt wird und die Einfügungs spezifischen Validierungs Steuerelemente nur ausgelöst werden, wenn auf die Schaltfläche "Einfügen" der DetailsView geklickt wird. Dadurch wird das in Abbildung 13 markierte Problem behoben Mit dieser Änderung wird das ValidationSummary-Steuerelement jedoch nicht mehr angezeigt, wenn ungültige Daten eingegeben werden. Das ValidationSummary-Steuerelement enthält auch eine `ValidationGroup`-Eigenschaft und zeigt nur Zusammenfassungs Informationen für diese Validierungs Steuerelemente in der zugehörigen Validierungs Gruppe an. Daher benötigen wir zwei Validierungs Steuerelemente auf dieser Seite, eine für die `InsertValidationControls` Validierungs Gruppe und eine für `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Mit dieser Ergänzung ist unser Tutorial fertiggestellt!

## <a name="summary"></a>Zusammenfassung

Obwohl boundfields sowohl eine Einfüge-als auch eine Bearbeitungs Schnittstelle bereitstellen kann, kann die Schnittstelle nicht angepasst werden. Im Allgemeinen möchten wir der Bearbeitungs-und einfügeschnittstelle Validierungs Steuerelemente hinzufügen, um sicherzustellen, dass der Benutzer die erforderlichen Eingaben in einem gültigen Format eingibt. Um dies zu erreichen, müssen wir die boundfields in templatefields konvertieren und die Validierungs Steuerelemente den entsprechenden Vorlagen hinzufügen. In diesem Tutorial haben wir das Beispiel aus der unter *suchung der Ereignisse im Zusammenhang mit dem Einfügen, aktualisieren und löschen* des Tutorials erweitert, indem Sie der einfügeschnittstelle der DetailsView und der Bearbeitungs Schnittstelle von GridView Validierungs Steuerelemente hinzufügen. Außerdem wurde erläutert, wie zusammenfassende Validierungs Informationen mithilfe des ValidationSummary-Steuer Elements angezeigt werden und wie die Validierungs Steuerelemente auf der Seite in verschiedene Validierungs Gruppen partitioniert werden.

Wie in diesem Tutorial gezeigt, ermöglichen templatefields, dass die Schnittstellen für die Bearbeitung und das Einfügen erweitert werden, sodass Sie Validierungs Steuerelemente enthalten. Templatefields kann auch erweitert werden, um zusätzliche Eingabe-websteuer Elemente einzuschließen, sodass das Textfeld durch ein geeignetere websteuer Element ersetzt werden kann. Im nächsten Tutorial wird erläutert, wie das TextBox-Steuerelement durch ein Daten gebundenes DropDownList-Steuerelement ersetzt wird, das ideal ist, wenn ein Fremdschlüssel (z. b. `CategoryID` oder `SupplierID` in der `Products` Tabelle) bearbeitet wird.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Liz shulok und Zack Jones. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [Weiter](customizing-the-data-modification-interface-vb.md)
