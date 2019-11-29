---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Hinzufügen von Validierungs Steuerelementen zur Bearbeitungs Schnittstelle von DataList (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie einfach es ist, der EditItemTemplate des DataList-Steuer Elements Validierungs Steuerelemente hinzuzufügen, um einen benutzerfreundlichen Bearbeitungs Benutzer int...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: f952a7bb95e956a2ad935f8bdef5c3efa7437ecb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621931"
---
# <a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Hinzufügen von Validierungssteuerelementen zu Oberfläche für die Bearbeitung von DataList (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) oder [PDF herunterladen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> In diesem Lernprogramm erfahren Sie, wie einfach es ist, dem EditItemTemplate-Element des DataList-Steuer Elements Validierungs Steuerelemente hinzuzufügen, um eine Benutzeroberfläche mit einer gestellteren Benutzeroberfläche bereitzustellen.

## <a name="introduction"></a>Einführung

In den bisherigen Lernprogrammen für die DataList-Bearbeitung haben die DataList-Bearbeitungs Schnittstellen keine proaktive Validierung der Benutzereingaben eingeschlossen, obwohl ungültige Benutzereingaben, wie z. b. ein fehlender Produktname oder negativer Preis, zu einer Ausnahme führen. Im [vorherigen Tutorial](handling-bll-and-dal-level-exceptions-vb.md) wurde erläutert, wie Sie dem DataList s-`UpdateCommand` Ereignishandler Ausnahme Behandlungs Code hinzufügen, um Informationen zu allen ausgelösten Ausnahmen zu erfassen und ordnungsgemäß anzuzeigen. Im Idealfall würde die Bearbeitungs Schnittstelle jedoch Validierungs Steuerelemente enthalten, um zu verhindern, dass ein Benutzer solche ungültigen Daten an erster Stelle eingibt.

In diesem Lernprogramm erfahren Sie, wie einfach es ist, dem DataList-`EditItemTemplate` Validierungs Steuerelemente hinzuzufügen, um eine Benutzeroberfläche mit einer weiteren narrensicher-Bearbeitung bereitzustellen. Insbesondere wird in diesem Tutorial das Beispiel erstellt, das im vorherigen Tutorial erstellt wurde, und die Bearbeitungs Schnittstelle wird so erweitert, dass Sie die entsprechende Validierung einschließt.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Schritt 1: Replizieren des Beispiels aus der[Behandlung von Ausnahmen auf BLL-und Dal-Ebene](handling-bll-and-dal-level-exceptions-vb.md)

Im Tutorial [Behandeln von BLL-und Dal-Ebenen-Ausnahmen](handling-bll-and-dal-level-exceptions-vb.md) haben wir eine Seite erstellt, die die Namen und Preise der Produkte in einem bearbeitbaren DataList mit zwei Spalten aufführte. Unser Ziel dieses Tutorials besteht darin, die Bearbeitungs Schnittstelle von DataList zu erweitern, um Validierungs Steuerelemente einzuschließen. Die Validierungs Logik führt insbesondere Folgendes aus:

- Der Name des Produkts muss angegeben werden.
- Stellen Sie sicher, dass der für den Preis eingegebene Wert ein gültiges Währungs Format ist.
- Stellen Sie sicher, dass der für den Preis eingegebene Wert größer oder gleich 0 (null) ist, da ein negativer `UnitPrice` Wert ungültig ist.

Bevor wir uns mit der Erweiterung des vorherigen Beispiels befassen, um die Validierung einzuschließen, müssen wir zunächst das Beispiel von der Seite `ErrorHandling.aspx` im Ordner `EditDeleteDataList` auf die Seite für dieses Tutorial `UIValidation.aspx`replizieren. Um dies zu erreichen, müssen sowohl das deklarative Markup der `ErrorHandling.aspx` Seite als auch der zugehörige Quellcode kopiert werden. Kopieren Sie zuerst das deklarative Markup, indem Sie die folgenden Schritte ausführen:

1. Öffnen der Seite "`ErrorHandling.aspx`" in Visual Studio
2. Wechseln Sie zum deklarativen Markup der Seite (Klicken Sie unten auf der Seite auf die Schaltfläche Quelle).
3. Kopieren Sie den Text innerhalb der `<asp:Content>` und `</asp:Content>` Tags (Zeilen 3 bis 32), wie in Abbildung 1 dargestellt.

[![den Text innerhalb des &lt;ASP: Content&gt;-Steuer Elements kopieren](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Abbildung 2**: Kopieren des Texts innerhalb des `<asp:Content>` Steuer Elements ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))

1. Öffnen der Seite "`UIValidation.aspx`"
2. Zum deklarativen Markup der Seite wechseln
3. Fügen Sie den Text in das `<asp:Content>`-Steuerelement ein.

Um den Quellcode zu kopieren, öffnen Sie die Seite `ErrorHandling.aspx.vb`, und kopieren Sie nur den Text *in* der `EditDeleteDataList_ErrorHandling`-Klasse. Kopieren Sie die drei Ereignishandler (`Products_EditCommand`, `Products_CancelCommand`und `Products_UpdateCommand`) zusammen mit der `DisplayExceptionDetails`-Methode, kopieren Sie jedoch **nicht** die Klassen Deklaration oder `using` Anweisungen. Fügen Sie den kopierten Text *innerhalb* der `EditDeleteDataList_UIValidation` Klasse in `UIValidation.aspx.vb`ein.

Nachdem Sie den Inhalt und den Code von `ErrorHandling.aspx` zu `UIValidation.aspx`verschoben haben, nehmen Sie sich einen Moment Zeit, um die Seiten in einem Browser zu testen. Die gleiche Ausgabe sollte angezeigt werden, und es sollte dieselbe Funktionalität auf jeder dieser beiden Seiten angezeigt werden (siehe Abbildung 2).

[![die Seite "uivalidation. aspx" die Funktionalität in ErrorHandling. aspx imitiert.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Abbildung 2**: die `UIValidation.aspx` Seite imitiert die Funktionalität in `ErrorHandling.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))

## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Schritt 2: Hinzufügen der Validierungs Steuerelemente zum DataList s EditItemTemplate

Beim Erstellen von Dateneingabe Formularen ist es wichtig, dass Benutzer alle erforderlichen Felder eingeben und dass alle bereitgestellten Eingaben gültige und korrekt formatierte Werte sind. Um sicherzustellen, dass die Eingaben eines Benutzers gültig sind, bietet ASP.net fünf integrierte Validierungs Steuerelemente, die zum Überprüfen des Werts eines einzelnen Eingabe-websteuer Elements entworfen wurden:

- "Requirements [dfieldvalidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) " stellt sicher, dass ein Wert angegeben wurde.
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) überprüft einen Wert mit einem anderen websteuer Elementwert oder einem konstanten Wert oder stellt sicher, dass das Format des Werts s für einen angegebenen Datentyp zulässig ist.
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) stellt sicher, dass ein Wert innerhalb eines Wertebereichs liegt.
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) überprüft einen Wert anhand eines [regulären Ausdrucks](http://en.wikipedia.org/wiki/Regular_expression) .
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) überprüft einen Wert mit einer benutzerdefinierten benutzerdefinierten Methode.

Weitere Informationen zu diesen fünf Steuerelementen finden Sie im Tutorial [Hinzufügen von Validierungs Steuerelementen zum Bearbeitungs-und Einfügevorgang von Schnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) . Weitere Informationen finden Sie im [Abschnitt Validierungs Steuerelemente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) der [ASP.net Quick Start-Tutorials](https://quickstarts.asp.net).

Für unser Tutorial müssen wir ein "Requirements dfieldvalidator" verwenden, um sicherzustellen, dass ein Wert für den Produktnamen angegeben wurde, und ein CompareValidator, um sicherzustellen, dass der eingegebene Preis einen Wert größer oder gleich 0 hat und in einem gültigen Währungs Format vorliegt.

> [!NOTE]
> Obwohl ASP.NET 1. x dieselben fünf Validierungs Steuerelemente enthielt, hat ASP.NET 2,0 eine Reihe von Verbesserungen hinzugefügt, die beiden Hauptfunktionen, die Client seitige Skriptunterstützung für Browser zusätzlich zu Internet Explorer ist, sowie die Möglichkeit, Validierungs Steuerelemente auf einer Seite in zu partitionieren. Validierungs Gruppen. Weitere Informationen zu den neuen Validierungs Steuerungsfunktionen in 2,0 finden Sie unter [Dissecting the Validation Controls in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Beginnen Sie, indem Sie dem DataList-`EditItemTemplate`die erforderlichen Validierungs Steuerelemente hinzufügen. Diese Aufgabe kann über den Designer durchgeführt werden, indem Sie auf den Link Vorlagen bearbeiten aus dem DataList s-Smarttag oder die deklarative Syntax klicken. Durchlaufen Sie den Prozess mithilfe der Option Vorlagen bearbeiten aus der Designansicht. Nachdem Sie sich für die Bearbeitung der DataList s `EditItemTemplate`entschieden haben, fügen Sie ein "Requirements dfieldvalidator" hinzu, indem Sie es aus der Toolbox in die Vorlagen Bearbeitungs Schnittstelle ziehen und hinter das Textfeld "`ProductName`" platzieren.

[![dem EditItemTemplate-Element nach dem ProductName-Textfeld ein "Requirements dfieldvalidator" hinzufügen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Abbildung 3**: Hinzufügen eines "Requirements dfieldvalidator" zum `EditItemTemplate After` das Textfeld "`ProductName`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))

Alle Validierungs Steuerelemente funktionieren, indem die Eingabe eines einzelnen ASP.net-websteuer Elements überprüft wird. Daher müssen wir angeben, dass das soeben hinzugefügte "Requirements dfieldvalidator" mit dem `ProductName` Textfeld überprüft werden soll. Dies erfolgt durch Festlegen der [`ControlToValidate` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) des Validierungs Steuer Elements auf die `ID` des entsprechenden websteuer Elements (`ProductName`in dieser Instanz). Legen Sie als nächstes die [`ErrorMessage`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) auf fest. Sie müssen den Product s-Namen und die`Text`- [Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) angeben, um \*. Der `Text`-Eigenschafts Wert (sofern vorhanden) ist der Text, der vom Validierungs Steuerelement angezeigt wird, wenn die Validierung fehlschlägt. Der `ErrorMessage`-Eigenschafts Wert, der erforderlich ist, wird vom ValidationSummary-Steuerelement verwendet. Wenn der `Text`-Eigenschafts Wert weggelassen wird, wird der `ErrorMessage`-Eigenschafts Wert vom Validierungs Steuerelement bei ungültiger Eingabe angezeigt.

Nachdem Sie diese drei Eigenschaften von "Requirements dfieldvalidator" festgelegt haben, sollte der Bildschirm in etwa wie in Abbildung 4 aussehen.

[![die Eigenschaften "controldevalidate", "ErrorMessage" und "Text" von "Requirements dfieldvalidator" festlegen.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Abbildung 4**: Festlegen der Eigenschaften "Requirements dfieldvalidator s `ControlToValidate`", "`ErrorMessage`" und "`Text`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))

Wenn das Element "Requirements dfieldvalidator" dem `EditItemTemplate`hinzugefügt wurde, müssen Sie nur noch die erforderliche Überprüfung für das Textfeld "Product s Price" hinzufügen. Da die `UnitPrice` beim Bearbeiten eines Datensatzes optional ist, müssen wir kein "Requirements dfieldvalidator" hinzufügen. Wir müssen jedoch ein CompareValidator hinzufügen, um sicherzustellen, dass die `UnitPrice`, falls angegeben, ordnungsgemäß als Währung formatiert ist und größer oder gleich 0 ist.

Fügen Sie das CompareValidator in das `EditItemTemplate` ein, und legen Sie dessen `ControlToValidate`-Eigenschaft auf `UnitPrice`fest, dessen `ErrorMessage`-Eigenschaft auf den Preis größer oder gleich 0 (null) und nicht das Währungssymbol enthalten darf und dessen `Text`-Eigenschaft \*ist. Um anzugeben, dass der `UnitPrice` Wert größer oder gleich 0 sein muss, legen Sie die Eigenschaft CompareValidator s [`Operator`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) auf `GreaterThanEqual`, die [`ValueToCompare`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) auf 0 und deren [`Type`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) auf `Currency`fest.

Nach dem Hinzufügen dieser beiden Validierungs Steuerelemente sollte die deklarative Syntax DataList s `EditItemTemplate` s in etwa wie folgt aussehen:

[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Nachdem Sie diese Änderungen vorgenommen haben, öffnen Sie die Seite in einem Browser. Wenn Sie versuchen, den Namen auszulassen oder einen ungültigen Preis einzugeben, wenn Sie ein Produkt bearbeiten, wird ein Sternchen neben dem Textfeld angezeigt. Wie in Abbildung 5 gezeigt, wird ein Preis Wert, der das Währungssymbol wie $19,95 enthält, als ungültig angesehen. Das Vergleichs `Currency` `Type` ermöglicht Ziffern Trennzeichen (z. b. Kommas oder Zeiträume, abhängig von den Kultur Einstellungen) und ein führendes Plus-oder Minuszeichen, lässt jedoch *kein* Währungssymbol zu. Dieses Verhalten kann Benutzer perplex sein, da die Bearbeitungs Schnittstelle die `UnitPrice` derzeit unter Verwendung des Währungs formats rendert.

[![ein Sternchen neben den Textfeldern mit ungültiger Eingabe angezeigt wird.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Abbildung 5**: ein Sternchen wird neben den Textfeldern mit ungültiger Eingabe angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))

Während die Überprüfung unverändert funktioniert, muss der Benutzer das Währungssymbol beim Bearbeiten eines Datensatzes manuell entfernen, was nicht zulässig ist. Wenn außerdem ungültige Eingaben in der Bearbeitungs Schnittstelle vorhanden sind, wird beim Klicken auf die Schaltflächen aktualisieren und Abbrechen ein Postback aufgerufen. Im Idealfall würde die Schaltfläche Abbrechen den DataList unabhängig von der Gültigkeit der Eingaben des Benutzers in den Zustand der vorabbearbeitung zurückgeben. Außerdem müssen Sie sicherstellen, dass die Seiten Daten gültig sind, bevor Sie die Produktinformationen im DataList-`UpdateCommand` Ereignishandler aktualisieren, da die Validierungs Steuerung der Client seitigen Logik von Benutzern umgangen werden kann, deren Browser entweder JavaScript unterstützen oder die Unterstützung deaktiviert ist.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Entfernen des Währungs Symbols aus dem EditItemTemplate s UnitPrice-Textfeld

Bei der Verwendung des CompareValidator s `Currency``Type`dürfen die validierten Eingaben keine Währungssymbole enthalten. Das vorhanden sein solcher Symbole bewirkt, dass das CompareValidator die Eingabe als ungültig markiert. Allerdings enthält unsere Bearbeitungs Schnittstelle derzeit ein Währungssymbol in das Textfeld "`UnitPrice`". Dies bedeutet, dass der Benutzer das Währungssymbol explizit entfernen muss, bevor die Änderungen gespeichert werden. Um dieses Problem zu beheben, haben wir drei Möglichkeiten:

1. Konfigurieren Sie die `EditItemTemplate` so, dass der Wert für `UnitPrice` Textfeld nicht als Währung formatiert ist.
2. Ermöglicht es dem Benutzer, ein Währungssymbol einzugeben, indem das CompareValidator entfernt und durch ein RegularExpressionValidator ersetzt wird, das einen ordnungsgemäß formatierten Währungswert überprüft. Die Herausforderung besteht darin, dass der reguläre Ausdruck zum Überprüfen eines Währungs Werts nicht so einfach wie das CompareValidator ist und Code schreiben muss, wenn Kultur Einstellungen integriert werden sollen.
3. Entfernen Sie das Validierungs Steuerelement vollständig, und verlassen Sie sich auf eine benutzerdefinierte serverseitige Validierungs Logik im GridView s `RowUpdating`-Ereignishandler.

Verwenden Sie für dieses Tutorial die Option 1. Der `UnitPrice` wird derzeit aufgrund des Datenbindung-Ausdrucks für das Textfeld in der `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`als Währungswert formatiert. Ändern Sie die `Eval`-Anweisung in `Eval("UnitPrice", "{0:n2}")`, wodurch das Ergebnis als Zahl mit zwei Ziffern der Genauigkeit formatiert wird. Dies kann direkt über die deklarative Syntax erfolgen oder durch Klicken auf den Link "DataBindings bearbeiten" im Textfeld "`UnitPrice`" in der `EditItemTemplate`DataList s.

Durch diese Änderung schließt der formatierte Preis in der Bearbeitungs Schnittstelle Kommas als Trennzeichen für die Gruppe und einen Punkt als Dezimaltrennzeichen ein, aber verlässt das Währungssymbol.

> [!NOTE]
> Wenn Sie das Währungs Format aus der editierbaren Schnittstelle entfernen, ist es hilfreich, das Währungssymbol als Text außerhalb des Textfelds zu platzieren. Dies dient als Hinweis für den Benutzer, dass Sie das Währungssymbol nicht angeben müssen.

## <a name="fixing-the-cancel-button"></a>Reparieren der Schaltfläche "Abbrechen"

Standardmäßig geben die Validierungs-websteuer Elemente JavaScript aus, um die Client seitige Validierung auszuführen. Wenn Sie auf eine Schaltfläche, einen LinkButton oder eine ImageButton-Taste klicken, werden die Validierungs Steuerelemente auf der Seite auf der Clientseite überprüft, bevor das Postback auftritt. Wenn ungültige Daten vorhanden sind, wird das Postback abgebrochen. Für bestimmte Schaltflächen kann jedoch die Gültigkeit der Daten unerheblich sein. in einem solchen Fall ist das Rück streichen des Postbacks aufgrund ungültiger Daten ein Ärgernis.

Die Schaltfläche Abbrechen ist ein Beispiel. Stellen Sie sich vor, dass ein Benutzer ungültige Daten eingibt, z. b. den Namen des Produkts weglassen und dann entscheidet, dass er das Produkt nicht mehr speichern möchte, und dann auf die Schaltfläche Abbrechen. Die Schaltfläche Abbrechen löst aktuell die Validierungs Steuerelemente auf der Seite aus, die melden, dass der Produktname fehlt und das Postback verhindert wird. Der Benutzer muss Text in das Textfeld "`ProductName`" eingeben, um den Bearbeitungsprozess abzubrechen.

Glücklicherweise verfügen die Schaltfläche, LinkButton und ImageButton über eine [`CausesValidation`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) , die angeben kann, ob durch Klicken auf die Schaltfläche die Validierungs Logik (standardmäßig `True`) initiiert werden soll. Legen Sie die Schaltfläche "Abbrechen" `CausesValidation`-Eigenschaft auf `False`fest.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Sicherstellen, dass die Eingaben im UpdateCommand-Ereignis Handler gültig sind

Aufgrund des Client seitigen Skripts, das von den Validierungs Steuerelementen ausgegeben wird, werden durch die Validierungs Steuerelemente alle Postbacks abgebrochen, die von Schaltflächen-, LinkButton-oder ImageButton-Steuerelementen initiiert werden, deren `CausesValidation` Eigenschaften `True` (Standard) sind. Wenn ein Benutzer jedoch mit einem veralteten Browser oder einem, dessen JavaScript-Unterstützung deaktiviert ist, nicht ausgeführt wird, werden die Client seitigen Validierungs Überprüfungen nicht ausgeführt.

Alle ASP.net-Validierungs Steuerelemente wiederholen ihre Validierungs Logik sofort nach dem Postback und melden die Gesamt Gültigkeit der Seiten Eingaben über die [`Page.IsValid`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Der Seiten Fluss wird jedoch in keiner Weise unterbrochen oder angehalten, basierend auf dem Wert `Page.IsValid`. Als Entwickler müssen Sie sicherstellen, dass die `Page.IsValid`-Eigenschaft den Wert `True` hat, bevor Sie mit Code fortfahren, der gültige Eingabedaten annimmt.

Wenn ein Benutzer JavaScript deaktiviert hat, besucht unsere Seite, bearbeitet ein Produkt, gibt einen Preis Wert von zu teuer ein und klickt auf die Schaltfläche "Aktualisieren". die Client seitige Validierung wird umgangen, und ein Postback wird durchlaufen. Beim Postback führt der ASP.net page s-`UpdateCommand` Ereignishandler aus, und es wird eine Ausnahme ausgelöst, wenn versucht wird, eine zu teure `Decimal`zu analysieren. Da eine Ausnahmebehandlung auftritt, wird eine solche Ausnahme ordnungsgemäß behandelt, aber wir könnten verhindern, dass die ungültigen Daten an erster `UpdateCommand` Stelle durchlaufen werden, wenn `Page.IsValid` den Wert `True`hat.

Fügen Sie am Anfang des `UpdateCommand` Ereignis Handlers direkt vor dem `Try` Block den folgenden Code ein:

[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Mit dieser Addition wird nur dann versucht, das Produkt zu aktualisieren, wenn die übermittelten Daten gültig sind. Die meisten Benutzer können aufgrund der Überprüfungs Steuerung Client seitige Skripts keine ungültigen Daten zurücksenden, aber Benutzer, deren Browser JavaScript nicht unterstützen oder die JavaScript-Unterstützung deaktiviert haben, können die Client seitigen Überprüfungen umgehen und ungültige Daten übermitteln.

> [!NOTE]
> Der Leser erinnert sich daran, dass wir beim Aktualisieren von Daten mit der GridView nicht explizit die `Page.IsValid`-Eigenschaft in der Code Behind-Klasse der Page s überprüfen müssen. Dies liegt daran, dass die GridView die `Page.IsValid`-Eigenschaft für uns konsultiert und nur mit dem Update fort fährt, wenn Sie den Wert `True`zurückgibt.

## <a name="step-3-summarizing-data-entry-problems"></a>Schritt 3: Zusammenfassen von Dateneingabe Problemen

Zusätzlich zu den fünf Validierungs Steuerelementen enthält ASP.NET das [ValidationSummary-Steuer](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)Element, das die `ErrorMessage` en dieser Validierungs Steuerelemente anzeigt, die ungültige Daten erkannt haben. Diese Zusammenfassungs Daten können als Text auf der Webseite oder über eine modale Client seitige MessageBox angezeigt werden. Verbessern Sie dieses Lernprogramm, um eine Client seitige MessageBox mit einer Zusammenfassung aller Überprüfungs Probleme zu schließen.

Ziehen Sie hierzu ein ValidationSummary-Steuerelement aus der Toolbox auf den Designer. Der Speicherort des ValidationSummary-Steuer Elements ist nicht wirklich wichtig, da wir ihn so konfigurieren, dass nur die Zusammenfassung als MessageBox angezeigt wird. Legen Sie nach dem Hinzufügen des Steuer Elements seine [`ShowSummary`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) auf `False` und dessen [`ShowMessageBox`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) auf `True`fest. Mit dieser Addition werden alle Validierungs Fehler in einer Client seitigen MessageBox zusammengefasst (siehe Abbildung 6).

[![werden die Validierungs Fehler in einer Client seitigen MessageBox zusammengefasst.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Abbildung 6**: die Validierungs Fehler werden in einer Client seitigen MessageBox zusammengefasst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))

## <a name="summary"></a>Summary

In diesem Tutorial wurde erläutert, wie Sie die Wahrscheinlichkeit von Ausnahmen verringern, indem Sie Validierungs Steuerelemente verwenden, um proaktiv sicherzustellen, dass unsere Benutzereingaben gültig sind, bevor Sie versuchen, Sie im Aktualisierungs Workflow zu verwenden. ASP.net bietet fünf Validierungs-websteuer Elemente, die entworfen wurden, um eine bestimmte websteuer Element Eingaben zu überprüfen und die Gültigkeit der Eingaben zu melden. In diesem Tutorial haben wir zwei dieser fünf Steuerelemente verwendet: "Requirements dfieldvalidator" und "CompareValidator", um sicherzustellen, dass der Name des Produkts angegeben wurde und dass der Preis ein Währungs Format mit einem Wert größer oder gleich 0 (null) aufweist.

Das Hinzufügen von Validierungs Steuerelementen zur Bearbeitungs Schnittstelle von DataList s ist so einfach wie das Ziehen auf die `EditItemTemplate` aus der Toolbox und das Festlegen einiger Eigenschaften. Standardmäßig geben die Validierungs Steuerelemente automatisch Client seitiges Validierungs Skript aus. Außerdem wird die serverseitige Validierung für das Postback bereitgestellt, wobei das kumulative Ergebnis in der `Page.IsValid`-Eigenschaft gespeichert wird. Um die Client seitige Validierung zu umgehen, wenn Sie auf eine Schaltfläche, einen LinkButton oder einen ImageButton klicken, legen Sie die Schaltfläche s `CausesValidation`-Eigenschaft auf `False`fest. Stellen Sie vor dem Ausführen von Aufgaben mit den Daten, die beim Postback übermittelt werden, sicher, dass die `Page.IsValid`-Eigenschaft `True`zurückgibt.

Alle bisher untersuchten DataList-Bearbeitungs Lernprogramme haben eine TextBox für den Product s-Namen und eine andere für den Preis. Die Bearbeitungs Schnittstelle kann jedoch eine Mischung verschiedener websteuer Elemente enthalten, wie z. b. Dropdown Listen, Kalender, Radiobuttons, Kontrollkästchen usw. Im nächsten Tutorial betrachten wir das Entwickeln einer Schnittstelle, die eine Vielzahl von websteuer Elementen verwendet.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Dennis Patterson, Ken pespisa und Liz shulok. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](handling-bll-and-dal-level-exceptions-vb.md)
> [Weiter](customizing-the-datalist-s-editing-interface-vb.md)
