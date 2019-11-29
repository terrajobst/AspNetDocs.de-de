---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Behandeln von Ausnahmen auf BLL-und Dal-Ebene (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Ausnahmen, die während des Aktualisierungs Workflows eines bearbeitbaren DataList-Workflows ausgelöst wurden, schrittweise behandelt werden.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 319ab44f2e65afc77f6f89ca8aa58c529f40d05c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624238"
---
# <a name="handling-bll--and-dal-level-exceptions-vb"></a>Verarbeiten von Ausnahmen auf BLL- und DAL-Ebene (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) oder [PDF herunterladen](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> In diesem Tutorial wird erläutert, wie Ausnahmen, die während des Aktualisierungs Workflows eines bearbeitbaren DataList-Workflows ausgelöst wurden, schrittweise behandelt werden.

## <a name="introduction"></a>Einführung

In der [Übersicht über das Bearbeiten und Löschen von Daten im DataList-](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) Tutorial haben wir einen DataList erstellt, der einfache Funktionen zum Bearbeiten und löschen bot. Obwohl es voll funktionsfähig ist, war es kaum benutzerfreundlich, da jeder Fehler, der während des Bearbeitungs-oder Löschvorgangs aufgetreten ist, zu einer nicht behandelten Ausnahme geführt hat. Wenn Sie z. b. den Produktnamen weglassen oder bei der Bearbeitung eines Produkts einen Preis Wert von sehr kostengünstiger! eingeben, löst eine Ausnahme aus. Da diese Ausnahme im Code nicht abgefangen wird, wird Sie auf die ASP.NET-Laufzeit hochskalieren, in der dann die Details der Ausnahme s auf der Webseite angezeigt werden.

Wie in der Behandlung von [Ausnahmen auf BLL-und Dal-Ebene in einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) gezeigt, werden die Ausnahme Details an die ObjectDataSource und dann an die GridView zurückgegeben, wenn eine Ausnahme aus den Tiefen der Geschäftslogik oder der Datenzugriffsebenen ausgelöst wird. Wir haben gesehen, wie Sie diese Ausnahmen ordnungsgemäß behandeln können, indem wir `Updated` oder `RowUpdated` Ereignishandler für ObjectDataSource oder GridView erstellen, eine Ausnahme überprüfen und dann angeben, dass die Ausnahme behandelt wurde.

Unsere DataList-Tutorials verwenden jedoch nicht die ObjectDataSource zum Aktualisieren und Löschen von Daten. Stattdessen arbeiten wir direkt mit der BLL. Um Ausnahmen zu erkennen, die von der BLL oder dal stammen, müssen wir den Code für die Ausnahmebehandlung im Code Behind unserer ASP.NET-Seite implementieren. In diesem Tutorial erfahren Sie, wie Sie Ausnahmen, die bei einem bearbeitbaren DataList s-Aktualisierungs Workflow ausgelöst wurden, schrittweise behandeln.

> [!NOTE]
> In der *Übersicht über das Bearbeiten und Löschen von Daten im DataList-* Tutorial haben wir verschiedene Techniken zum Bearbeiten und Löschen von Daten aus dem DataList erläutert, einige Techniken, die zum Aktualisieren und Löschen von ObjectDataSource verwendet wurden. Wenn Sie diese Techniken anwenden, können Sie Ausnahmen von der BLL oder dal über die ObjectDataSource s-`Updated` oder `Deleted`-Ereignishandler behandeln.

## <a name="step-1-creating-an-editable-datalist"></a>Schritt 1: Erstellen eines bearbeitbaren DataList

Bevor wir uns mit der Behandlung von Ausnahmen beschäftigen, die während des Aktualisierungs Workflows auftreten, erstellen Sie zunächst einen bearbeitbaren DataList. Öffnen Sie die Seite `ErrorHandling.aspx` im Ordner `EditDeleteDataList`, fügen Sie dem Designer einen DataList hinzu, legen Sie dessen Eigenschaft `ID` auf `Products`fest, und fügen Sie eine neue ObjectDataSource mit dem Namen `ProductsDataSource`hinzu. Konfigurieren Sie ObjectDataSource so, dass die `ProductsBLL` Klasse s `GetProducts()` Methode zum Auswählen von Datensätzen verwendet wird. Legen Sie die Dropdown Listen auf den Registerkarten einfügen, aktualisieren und löschen auf (keine) fest.

[![die Produktinformationen mithilfe der GetProducts ()-Methode zurückgeben](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Abbildung 1**: Zurückgeben der Produktinformationen mithilfe der `GetProducts()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))

Nachdem Sie den ObjectDataSource-Assistenten abgeschlossen haben, erstellt Visual Studio automatisch eine `ItemTemplate` für den DataList. Ersetzen Sie dies durch eine `ItemTemplate`, die den Namen und den Preis der einzelnen Produkte anzeigt und eine Bearbeitungs Schaltfläche enthält. Erstellen Sie als nächstes eine `EditItemTemplate` mit einem TextBox-websteuer Element für die Schaltflächen Name und Price und Update und Abbrechen. Legen Sie abschließend die DataList s-`RepeatColumns`-Eigenschaft auf 2 fest.

Nachdem Sie diese Änderungen vorgenommen haben, sollte Ihr deklaratives Markup der Seite in etwa wie folgt aussehen. Vergewissern Sie sich, dass die `CommandName` Eigenschaften für die Schaltflächen Bearbeiten, Abbrechen und Aktualisieren auf Bearbeiten, Abbrechen bzw. aktualisieren festgelegt sind.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Für dieses Tutorial muss der Ansichts Zustand "DataList s" aktiviert sein.

Nehmen Sie sich einen Moment Zeit, um den Fortschritt über einen Browser anzuzeigen (siehe Abbildung 2).

[![jedes Produkt enthält eine Bearbeitungs Schaltfläche.](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Abbildung 2**: jedes Produkt enthält eine Schaltfläche "Bearbeiten" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))

Zurzeit verursacht die Schaltfläche "Bearbeiten" nur ein Postback, das das Produkt noch nicht bearbeitbar macht. Um die Bearbeitung zu aktivieren, müssen wir Ereignishandler für die `EditCommand`-, `CancelCommand`-und `UpdateCommand`-Ereignisse des DataList s erstellen. Die `EditCommand`-und `CancelCommand` Ereignisse aktualisieren einfach die DataList-`EditItemIndex`-Eigenschaft und binden die Daten erneut an den DataList-DataList:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

Der `UpdateCommand` Ereignishandler ist etwas mehr beteiligt. Es muss die `ProductID` der bearbeiteten Produkte aus der `DataKeys` Auflistung zusammen mit dem Namen und dem Preis des Produkts aus den Textfeldern in der `EditItemTemplate`lesen und dann die `ProductsBLL` Class s `UpdateProduct`-Methode aufzurufen, bevor der DataList in den Zustand der vorabbearbeitung zurückgegeben wird.

Verwenden Sie vorerst einfach genau denselben Code aus dem `UpdateCommand`-Ereignishandler in der *Übersicht über das Bearbeiten und Löschen von Daten im DataList-* Tutorial. Wir fügen den Code hinzu, um Ausnahmen in Schritt 2 ordnungsgemäß zu behandeln.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

Im Falle einer ungültigen Eingabe, die in Form eines nicht ordnungsgemäß formatierten Einheits Preises vorliegen kann, wird ein unzulässiger Preis Wert von Einheiten wie z. b.-$5,00 oder das Weglassen des Product s Name ausgelöst. Da der `UpdateCommand` Ereignishandler an dieser Stelle keinen Code für die Ausnahmebehandlung enthält, wird die Ausnahme in die ASP.NET-Laufzeit hochskalieren, wo Sie dem Endbenutzer angezeigt wird (siehe Abbildung 3).

![Wenn eine nicht behandelte Ausnahme auftritt, wird dem Endbenutzer eine Fehlerseite angezeigt.](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Abbildung 3**: Wenn eine nicht behandelte Ausnahme auftritt, wird dem Endbenutzer eine Fehlerseite angezeigt.

## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Schritt 2: ordnungsgemäße Behandlung von Ausnahmen im UpdateCommand-Ereignis Handler

Während des Aktualisierungs Workflows können Ausnahmen im `UpdateCommand` Ereignishandler, der BLL oder der dal auftreten. Wenn ein Benutzer beispielsweise einen Preis zu teuer eingibt, löst die `Decimal.Parse`-Anweisung im `UpdateCommand`-Ereignishandler eine `FormatException`-Ausnahme aus. Wenn der Benutzer den Namen des Produkts auslässt oder der Preis einen negativen Wert aufweist, wird von der dal eine Ausnahme ausgelöst.

Wenn eine Ausnahme auftritt, möchten wir eine informative Meldung auf der Seite Selbstanzeigen. Fügen Sie der Seite ein Label-websteuer Element hinzu, dessen `ID` auf `ExceptionDetails`festgelegt ist. Konfigurieren Sie den Text der Bezeichnung, sodass er in einer roten, extra großen, Fetten und kursiv Schrift angezeigt wird, indem Sie die `CssClass`-Eigenschaft der `Warning` CSS-Klasse zuweisen, die in der `Styles.css`-Datei definiert ist.

Wenn ein Fehler auftritt, soll die Bezeichnung nur einmal angezeigt werden. Das heißt, bei nachfolgenden Postbacks sollte die Warnmeldung der Bezeichnung s verschwinden. Dies kann erreicht werden, indem die Bezeichnung s `Text`-Eigenschaft gelöscht wird oder die `Visible`-Eigenschaft der-Eigenschaft für den `Page_Load`-Ereignishandler `False` wird (wie in der [Behandlung von Ausnahmen auf BLL-und Dal-Ebene in einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) ) oder durch Deaktivieren der Unterstützung für den Ansichts Zustand der Bezeichnung. Verwenden Sie die zweite Option.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Wenn eine Ausnahme ausgelöst wird, weisen wir die Details der Ausnahme der `ExceptionDetails` Label-Steuerelement-`Text` Eigenschaft zu. Da der Ansichts Zustand deaktiviert ist, gehen bei nachfolgenden Postbacks die programmgesteuerten Änderungen der `Text`-Eigenschaft verloren und kehren zum Standardtext (eine leere Zeichenfolge) zurück. Dadurch wird die Warnmeldung ausgeblendet.

Um zu ermitteln, wann ein Fehler ausgelöst wurde, um eine hilfreiche Meldung auf der Seite anzuzeigen, müssen wir dem `UpdateCommand`-Ereignishandler einen `Try ... Catch`-Block hinzufügen. Der `Try` Teil enthält Code, der möglicherweise zu einer Ausnahme führt, während der `Catch` Block Code enthält, der in der Gegenseite einer Ausnahme ausgeführt wird. Weitere Informationen zum `Try ... Catch`-Block finden Sie im Abschnitt [Grundlagen zur Ausnahmebehandlung](https://msdn.microsoft.com/library/2w8f0bss.aspx) in der .NET Framework-Dokumentation.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Wenn eine Ausnahme eines beliebigen Typs durch Code im `Try`-Block ausgelöst wird, wird die Ausführung des `Catch` Block s-Codes gestartet. Der Typ der Ausnahme, die `DbException`, `NoNullAllowedException`, `ArgumentException`usw. ausgelöst wird, hängt davon ab, was genau den Fehler ausgelöst hat. Wenn auf Datenbankebene ein Problem vorliegt, wird eine `DbException` ausgelöst. Wenn für die Felder `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`oder `ReorderLevel` ein unzulässiger Wert eingegeben wird, wird eine `ArgumentException` ausgelöst, da wir Code zum Validieren dieser Feldwerte in der `ProductsDataTable`-Klasse hinzugefügt haben (siehe das Tutorial [Erstellen einer Geschäftslogik Schicht](../introduction/creating-a-business-logic-layer-vb.md) ).

Wir können für den Endbenutzer eine ausführlichere Erläuterung bereitstellen, indem Sie den Nachrichtentext auf den Typ der aufgefangenen Ausnahme stützen. Der folgende Code, der in einer nahezu identischen Form in der Behandlung von [Ausnahmen auf der BLL-und Dal-Ebene in einem ASP.net page-](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) Tutorial verwendet wurde, bietet diese Detailstufe:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Um dieses Lernprogramm abzuschließen, müssen Sie einfach die `DisplayExceptionDetails`-Methode vom `Catch`-Block aus, der die abgefangene `Exception` Instanz (`ex`) übergibt.

Wenn der `Try ... Catch`-Block vorhanden ist, wird den Benutzern eine informative Fehlermeldung angezeigt, wie in Abbildung 4 und 5 dargestellt. Beachten Sie, dass sich der DataList im Falle einer Ausnahme im Bearbeitungsmodus befindet. Dies liegt daran, dass die Ablauf Steuerung nach dem Auftreten der Ausnahme sofort an den `Catch`-Block umgeleitet wird, wobei der Code umgangen wird, der den DataList in den Zustand der vorabbearbeitung zurückgibt.

[![eine Fehlermeldung angezeigt wird, wenn ein Benutzer ein Pflichtfeld auslässt](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Abbildung 4**: eine Fehlermeldung wird angezeigt, wenn ein Benutzer ein Pflichtfeld auslässt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))

[![eine Fehlermeldung angezeigt wird, wenn ein negativer Preis eingegeben wird](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Abbildung 5**: eine Fehlermeldung wird angezeigt, wenn ein negativer Preis eingegeben wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))

## <a name="summary"></a>Summary

GridView und ObjectDataSource stellen Ereignishandler auf der Post-Ebene bereit, die Informationen zu allen Ausnahmen enthalten, die während des Aktualisierungs-und Lösch Workflows ausgelöst wurden, sowie Eigenschaften, die festgelegt werden können, um anzugeben, ob die Ausnahme aufgetreten ist. Umgangs. Diese Funktionen sind jedoch nicht verfügbar, wenn Sie mit dem DataList arbeiten und die BLL direkt verwenden. Stattdessen sind wir für die Implementierung der Ausnahmebehandlung verantwortlich.

In diesem Tutorial wurde erläutert, wie Sie eine Ausnahmebehandlung zu einem bearbeitbaren DataList s-Aktualisierungs Workflow hinzufügen, indem Sie dem `UpdateCommand`-Ereignishandler einen `Try ... Catch`-Block hinzufügen. Wenn während des Aktualisierungs Workflows eine Ausnahme ausgelöst wird, wird der `Catch`-Block s-Code ausgeführt, der hilfreiche Informationen in der `ExceptionDetails` Bezeichnung anzeigt.

An diesem Punkt unternimmt der DataList keinen Aufwand, um zu verhindern, dass Ausnahmen an erster Stelle auftreten. Obwohl wir wissen, dass ein negativer Preis zu einer Ausnahme führt, haben wir noch keine Funktionen hinzugefügt, um proaktiv zu verhindern, dass ein Benutzer solche ungültige Eingaben eingibt. Im nächsten Tutorial erfahren Sie, wie Sie die Ausnahmen, die durch ungültige Benutzereingaben verursacht werden, durch Hinzufügen von Validierungs Steuerelementen in der `EditItemTemplate`reduzieren können.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Entwurfsrichtlinien für Ausnahmen](https://msdn.microsoft.com/library/ms298399.aspx)
- [Fehler Protokollierung von Modulen und Handlern (ELMAH)](http://workspaces.gotdotnet.com/elmah) (eine Open Source-Bibliothek zum Protokollieren von Fehlern)
- [Enterprise Library für .NET Framework 2,0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (schließt den Anwendungs Block für die Ausnahme Verwaltung ein)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Ken pespisa. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](performing-batch-updates-vb.md)
> [Weiter](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
