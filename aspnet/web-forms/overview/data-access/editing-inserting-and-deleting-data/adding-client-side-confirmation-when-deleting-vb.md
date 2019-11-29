---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: Hinzufügen der Client seitigen Bestätigung beim Löschen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In den bisher erstellten Schnittstellen können Benutzer versehentlich Daten löschen, indem Sie auf die Schaltfläche "Löschen" klicken, wenn Sie auf die Schaltfläche "Bearbeiten" klicken. In diesem t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: addb5a1fdc5793309388c5f06b44fb3b145bc102
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74589354"
---
# <a name="adding-client-side-confirmation-when-deleting-vb"></a>Hinzufügen von clientseitiger Bestätigung beim Löschen (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) oder [PDF herunterladen](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> In den bisher erstellten Schnittstellen können Benutzer versehentlich Daten löschen, indem Sie auf die Schaltfläche "Löschen" klicken, wenn Sie auf die Schaltfläche "Bearbeiten" klicken. In diesem Tutorial fügen wir ein Client seitiges Bestätigungs Dialogfeld hinzu, das beim Klicken auf die Schaltfläche löschen angezeigt wird.

## <a name="introduction"></a>Einführung

In den letzten Tutorials haben wir gesehen, wie wir unsere Anwendungsarchitektur, ObjectDataSource und die datenweb-Steuerelemente in Concert verwenden, um Funktionen zum Einfügen, bearbeiten und löschen bereitzustellen. Die bisher untersuchten Lösch Schnittstellen bestehen aus einer DELETE-Schaltfläche, die beim Klicken auf ein Postback führt und die `Delete()` Methode ObjectDataSource s aufruft. Die `Delete()`-Methode ruft dann die konfigurierte Methode aus der Geschäftslogik Schicht auf, die den Aufruf an die Datenzugriffs Ebene weitergibt und die tatsächliche `DELETE` Anweisung an die Datenbank ausgibt.

Wenngleich diese Benutzeroberfläche es Benutzern ermöglicht, Datensätze über das GridView-, DetailsView-oder FormView-Steuerelement zu löschen, fehlt die Bestätigung, wenn der Benutzer auf die Schaltfläche "Löschen" klickt. Wenn ein Benutzer versehentlich auf die Schaltfläche Löschen klickt, wenn er auf Bearbeiten geklickt hat, wird der Datensatz, für den die Aktualisierung durchgeführt wurde, stattdessen gelöscht. Um dies zu verhindern, fügen Sie in diesem Tutorial ein Client seitiges Bestätigungs Dialogfeld hinzu, das beim Klicken auf die Schaltfläche löschen angezeigt wird.

Die JavaScript-`confirm(string)` Funktion zeigt den Eingabeparameter der Zeichenfolge als Text in einem modalen Dialogfeld an, das mit zwei Schaltflächen ausgestattet ist: OK und Abbrechen (siehe Abbildung 1). Die `confirm(string)`-Funktion gibt einen booleschen Wert zurück, abhängig davon, auf welche Schaltfläche geklickt wird (`true`, wenn der Benutzer auf OK klickt und `false`, wenn er auf Abbrechen klickt).

![Die Javascript Confirm (String)-Methode zeigt eine modale Client seitige MessageBox an.](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Abbildung 1**: die JavaScript-`confirm(string)`-Methode zeigt eine modale Client seitige MessageBox an

Wenn während einer Formular Übermittlung der Wert `false` von einem Client seitigen Ereignishandler zurückgegeben wird, wird die Formular Übermittlung abgebrochen. Mit dieser Funktion können Sie die Client seitige DELETE-Schaltfläche s verwenden, `onclick` Ereignishandler den Wert eines Aufrufens `confirm("Are you sure you want to delete this product?")`zurückgibt. Wenn der Benutzer auf Abbrechen klickt, wird `confirm(string)` false zurückgeben, wodurch die Formular Übermittlung abgebrochen wird. Ohne Postback wird das Produkt, auf dessen Lösch Schaltfläche geklickt wurde, nicht gelöscht. Wenn der Benutzer jedoch im Bestätigungs Dialogfeld auf OK klickt, wird das Postback nicht fortgesetzt, und das Produkt wird gelöscht. Weitere Informationen zu dieser Technik finden [Sie unter Verwenden der JavaScript s `confirm()`-Methode zum Steuern der Formular Übermittlung](http://www.webreference.com/programming/javascript/confirm/) .

Das Hinzufügen des erforderlichen Client seitigen Skripts unterscheidet sich geringfügig, wenn Vorlagen verwendet werden, als bei Verwendung eines CommandField. In diesem Tutorial sehen wir uns daher ein Beispiel für FormView und GridView an.

> [!NOTE]
> Bei der Verwendung von Client seitigen Bestätigungs Techniken, wie in diesem Tutorial erläutert, wird davon ausgegangen, dass die Benutzer mit Browsern, die JavaScript unterstützen, und JavaScript aktiviert sind. Wenn eine dieser Annahmen für einen bestimmten Benutzer nicht wahr ist, wird beim Klicken auf die Schaltfläche "Löschen" sofort ein Postback ausgelöst (keine Überprüfung der MessageBox).

## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Schritt 1: Erstellen einer FormView, die das Löschen unterstützt

Fügen Sie zunächst der Seite `ConfirmationOnDelete.aspx` im Ordner `EditInsertDelete` einen FormView hinzu, und binden Sie ihn an eine neue ObjectDataSource, die die Produktinformationen über die `GetProducts()`-Methode der `ProductsBLL`-Klasse zurückgibt. Konfigurieren Sie auch ObjectDataSource so, dass die `ProductsBLL` Class s `DeleteProduct(productID)`-Methode der ObjectDataSource s `Delete()`-Methode zugeordnet ist. Stellen Sie sicher, dass die Dropdown Listen der Registerkarten einfügen und Aktualisieren auf (keine) festgelegt sind. Aktivieren Sie abschließend das Kontrollkästchen Paging aktivieren im Smarttag von FormView s.

Nach diesen Schritten sieht das neue deklarative Markup von ObjectDataSource s wie folgt aus:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Wie in den vorherigen Beispielen, in denen keine vollständige Parallelität verwendet wurde, sollten Sie sich einen Moment Zeit nehmen, um die ObjectDataSource s-`OldValuesParameterFormatString`-Eigenschaft zu löschen.

Da es an ein ObjectDataSource-Steuerelement gebunden wurde, das nur das Löschen unterstützt, bietet das FormView s-`ItemTemplate` nur die Schaltfläche "Löschen", ohne die Schaltflächen neu und aktualisieren. Das deklarative Markup von FormView s enthält jedoch eine überflüssige `EditItemTemplate` und `InsertItemTemplate`, die entfernt werden können. Nehmen Sie sich einen Moment Zeit, um die `ItemTemplate` anzupassen, sodass nur eine Teilmenge der Product Data-Felder anzeigt. Ich habe "My" so konfiguriert, dass der Name des Produkts in einer `<h3>` Überschrift über deren Lieferanten-und Kategorienamen (und die Schaltfläche "Löschen") angezeigt wird

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

Mit diesen Änderungen verfügen wir über eine voll funktionsfähige Webseite, die es Benutzern ermöglicht, die Produkte einzeln zu wechseln, und die Möglichkeit zum Löschen eines Produkts zu haben, indem Sie einfach auf die Schaltfläche "Löschen" klicken. Abbildung 2 zeigt einen Screenshot dieses Fortschritts, der in einem Browser angezeigt wird.

[![FormView zeigt Informationen zu einem einzelnen Produkt an.](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**Abbildung 2**: FormView zeigt Informationen zu einem einzelnen Produkt an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))

## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Schritt 2: Aufrufen der Confirm (String)-Funktion über das Client seitige OnClick-Ereignis der Schaltfläche "Löschen"

Wenn die Form Ansicht erstellt wurde, ist der letzte Schritt die Konfiguration der Schaltfläche "Löschen", sodass die JavaScript-`confirm(string)` Funktion aufgerufen wird, wenn Sie vom Besucher darauf geklickt wird. Das Hinzufügen eines Client seitigen Skripts zu einem Client seitigen "Button"-, "LinkButton"-oder "ImageButton s"-`onclick` Ereignis kann durch die Verwendung des `OnClientClick property`erreicht werden, das neu in ASP.NET 2,0 ist. Da der Wert der `confirm(string)`-Funktion zurückgegeben werden soll, legen Sie diese Eigenschaft einfach auf fest: `return confirm('Are you certain that you want to delete this product?');`

Nachdem Sie diese Änderung vorgenommen haben, sollte die deklarative Syntax von LinkButton s löschen in etwa wie folgt aussehen:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

Das ist schon alles! Abbildung 3 zeigt einen Screenshot dieser Bestätigung in Aktion. Wenn Sie auf die Schaltfläche Löschen klicken, wird das Dialogfeld bestätigen geöffnet. Wenn der Benutzer auf Abbrechen klickt, wird das Postback abgebrochen, und das Produkt wird nicht gelöscht. Wenn der Benutzer jedoch auf OK klickt, wird das Postback fortgesetzt, und die ObjectDataSource s-`Delete()`-Methode wird aufgerufen, die im Datenbankdaten Satz steht, der gelöscht wird.

> [!NOTE]
> Die Zeichenfolge, die an die JavaScript-Funktion von `confirm(string)` übertragen wird, wird durch Apostrophe (anstelle von Anführungszeichen) getrennt. In JavaScript können Zeichen folgen mithilfe von Zeichen getrennt werden. Hier werden Apostrophe verwendet, damit die Trennzeichen für die Zeichenfolge, die an `confirm(string)` übergeben wird, keine Mehrdeutigkeit mit den Trennzeichen enthalten, die für den `OnClientClick`-Eigenschafts Wert verwendet werden.

[![eine Bestätigung angezeigt wird, wenn Sie auf die Schaltfläche "Löschen" klicken](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Abbildung 3**: eine Bestätigung wird angezeigt, wenn Sie auf die Schaltfläche "Löschen" klicken ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))

## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Schritt 3: Konfigurieren der OnClientClick-Eigenschaft für die Schaltfläche "Löschen" in einem CommandField

Wenn Sie direkt in einer Vorlage mit einer Schaltfläche, einem LinkButton oder einem ImageButton arbeiten, kann Ihr ein Bestätigungs Dialogfeld zugeordnet werden, indem einfach die `OnClientClick`-Eigenschaft so konfiguriert wird, dass die Ergebnisse der JavaScript-`confirm(string)`-Funktion zurückgegeben werden. Das CommandField-Objekt, das ein Feld mit Lösch Schaltflächen zu einem GridView-oder DetailsView-Objekt hinzufügt, verfügt jedoch nicht über eine `OnClientClick`-Eigenschaft, die deklarativ festgelegt werden kann. Stattdessen müssen Sie Programm gesteuert auf die Schaltfläche "Löschen" in der GridView-oder DetailsView-`DataBound` Ereignishandler verweisen und dann die zugehörige `OnClientClick`-Eigenschaft festlegen.

> [!NOTE]
> Beim Festlegen der DELETE-Schaltfläche s `OnClientClick`-Eigenschaft im entsprechenden `DataBound`-Ereignishandler haben wir Zugriff auf die Daten, die an den aktuellen Datensatz gebunden wurden. Dies bedeutet, dass wir die Bestätigungsnachricht so erweitern können, dass Sie Details zum jeweiligen Datensatz enthält, z. b. "möchten Sie das Chai-Produkt wirklich löschen?". Diese Anpassung ist auch in Vorlagen möglich, die die Datenbindung-Syntax verwenden.

Wenn Sie die `OnClientClick`-Eigenschaft für die Lösch Schaltfläche (n) in einem CommandField festlegen möchten, fügen Sie der Seite eine GridView hinzu. Konfigurieren Sie diese GridView für die Verwendung desselben ObjectDataSource-Steuer Elements, das von der FormView verwendet wird. Begrenzen Sie außerdem die "GridView s boundfields", sodass Sie nur den Namen, die Kategorie und den Lieferanten des Produkts enthalten. Aktivieren Sie abschließend das Kontrollkästchen "Löschen" im GridView-Smarttag. Dadurch wird der GridView s `Columns`-Auflistung ein CommandField hinzugefügt, dessen `ShowDeleteButton`-Eigenschaft auf `true`festgelegt ist.

Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup der GridView s wie folgt aussehen:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

Das CommandField enthält eine einzelne DELETE LinkButton-Instanz, auf die Programm gesteuert über den GridView s `RowDataBound`-Ereignishandler zugegriffen werden kann. Nachdem auf Sie verwiesen wurde, können wir Ihre `OnClientClick`-Eigenschaft entsprechend festlegen. Erstellen Sie einen Ereignishandler für das `RowDataBound`-Ereignis, indem Sie den folgenden Code verwenden:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Dieser Ereignishandler funktioniert mit Daten Zeilen (mit der Schaltfläche "Löschen") und beginnt mit der programmgesteuerten Referenzierung der Schaltfläche "Löschen". Verwenden Sie im Allgemeinen das folgende Muster:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType* ist der Typ der Schaltfläche, die von CommandField-Button, LinkButton oder ImageButton verwendet wird. Standardmäßig verwendet das CommandField Link Buttons, aber dies kann über die CommandField s-`ButtonType property`angepasst werden. Der *commandfieldindex* ist der Ordinalindex des CommandField innerhalb der GridView s-`Columns` Auflistung, während *controlindex* der Index der DELETE-Schaltfläche in der CommandField s `Controls` Auflistung ist. Der *controlindex* -Wert hängt von der Position der Schaltflächen in Bezug auf andere Schaltflächen im CommandField ab. Wenn z. b. die einzige Schaltfläche, die im CommandField angezeigt wird, die Schaltfläche Löschen ist, verwenden Sie einen Index von 0. Wenn jedoch eine Bearbeitungs Schaltfläche vorhanden ist, die der Schaltfläche "Löschen" vorangestellt ist, verwenden Sie einen Index von 2. Der Grund für die Verwendung eines Index von 2 liegt darin, dass zwei Steuerelemente von CommandField vor der Schaltfläche Löschen hinzugefügt werden: die Schaltfläche Bearbeiten und ein LiteralControl, das zum Hinzufügen von Leerzeichen zwischen den Schaltflächen Bearbeiten und Löschen verwendet wurde.

In unserem Beispiel verwendet das CommandField Link Buttons, und das Feld ganz links hat einen *commandfieldindex* von 0. Da keine anderen Schaltflächen, sondern die Schaltfläche "Löschen" im CommandField vorhanden sind, verwenden wir einen *controlindex* von 0.

Nachdem Sie auf die Schaltfläche "Löschen" im CommandField verwiesen haben, werden Informationen über das an die aktuelle GridView-Zeile gebundene Produkt angezeigt. Zum Schluss legen wir die Delete-Schaltfläche s `OnClientClick`-Eigenschaft auf das entsprechende JavaScript fest, das den Namen des Produkts enthält. Da die an die `confirm(string)` Funktion über gegebene JavaScript-Zeichenfolge mithilfe von Apostrophe getrennt wird, müssen wir alle Apostrophe mit Escapezeichen versehen, die innerhalb des Namens des Produkts enthalten sind. Vor allem werden alle Apostrophe im Product s-Namen mit "`\'`" versehen.

Nachdem diese Änderungen vorgenommen wurden, wird durch Klicken auf die Schaltfläche Löschen in der GridView ein angepasstes Bestätigungs Dialogfeld angezeigt (siehe Abbildung 4). Wie bei der Bestätigungs-MessageBox in FormView, wenn der Benutzer auf Abbrechen klickt, wird das Postback abgebrochen. Dadurch wird verhindert, dass das Löschen auftritt.

> [!NOTE]
> Diese Technik kann auch verwendet werden, um Programm gesteuert auf die Schaltfläche "Löschen" im CommandField in einer DetailsView zuzugreifen. Für die DetailsView erstellen Sie jedoch einen Ereignishandler für das `DataBound`-Ereignis, da die DetailsView kein `RowDataBound`-Ereignis enthält.

[![klicken auf die Schaltfläche Löschen von GridView s wird ein angepasstes Bestätigungs Dialogfeld](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Abbildung 4**: durch Klicken auf die Schaltfläche "Löschen"[von](adding-client-side-confirmation-when-deleting-vb/_static/image10.png)GridView s wird ein angepasstes Bestätigungs Dialogfeld angezeigt.

## <a name="using-templatefields"></a>Verwenden von templatefields

Einer der Nachteile des CommandField ist, dass auf seine Schaltflächen über die Indizierung zugegriffen werden muss und dass das resultierende Objekt in den entsprechenden Schalt Flächentyp (Schaltfläche, LinkButton oder ImageButton) umgewandelt werden muss. Die Verwendung von "Magic Numbers" und hart codierten Typen lädt Probleme ein, die bis zur Laufzeit nicht erkannt werden können. Wenn Sie oder ein anderer Entwickler z. b. dem CommandField zu einem späteren Zeitpunkt neue Schaltflächen hinzufügen (z. b. eine Bearbeitungs Schaltfläche) oder die `ButtonType`-Eigenschaft ändern, wird der vorhandene Code weiterhin fehlerfrei kompiliert. das Aufrufen der Seite kann jedoch eine Ausnahme oder ein unerwartetes Verhalten verursachen, je nachdem, wie Ihr Code geschrieben wurde und welche Änderungen vorgenommen wurden.

Ein alternativer Ansatz ist das Konvertieren der Befehls Felder GridView und DetailsView s in templatefields. Dadurch wird ein TemplateField-Element mit einem `ItemTemplate` generiert, das für jede Schaltfläche im CommandField einen LinkButton (oder eine Schaltfläche oder ein ImageButton) enthält. Diese Schaltflächen `OnClientClick` Eigenschaften können deklarativ zugewiesen werden, wie es bei FormView der Fall war, oder Sie können im entsprechenden `DataBound`-Ereignishandler Programm gesteuert mithilfe des folgenden Musters aufgerufen werden:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

Where *ControlID* ist der Wert der Schaltfläche s `ID`-Eigenschaft. Obwohl dieses Muster weiterhin einen hart codierten Typ für die Umwandlung erfordert, entfällt die Indizierung, sodass das Layout ohne einen Laufzeitfehler geändert werden kann.

## <a name="summary"></a>Summary

Die JavaScript-`confirm(string)`-Funktion ist eine häufig verwendete Technik zum Steuern des Workflows für die Formular Übermittlung. Wenn die Funktion ausgeführt wird, wird ein modales, Client seitiges Dialogfeld angezeigt, das zwei Schaltflächen (OK und Abbrechen) enthält. Wenn der Benutzer auf OK klickt, gibt die `confirm(string)`-Funktion `true`zurück. durch Klicken auf Abbrechen wird `false`zurückgegeben Diese Funktion ist mit einem Browser-Verhalten gekoppelt, um eine Formular Übermittlung abzubrechen, wenn ein Ereignishandler während des Übermittlungs Prozesses `false`zurückgibt, kann verwendet werden, um beim Löschen eines Datensatzes eine Bestätigungs-MessageBox anzuzeigen.

Die `confirm(string)`-Funktion kann einem Client seitigen `onclick` Ereignishandler für Schaltflächen-websteuer Elemente über die Steuerelement-`OnClientClick` Eigenschaft zugeordnet werden. Beim Arbeiten mit einer Lösch Schaltfläche in einer Vorlage (entweder in einer der FormView s-Vorlagen oder in einem TemplateField in der DetailsView oder GridView), kann diese Eigenschaft entweder deklarativ oder Programm gesteuert festgelegt werden, wie in diesem Tutorial gezeigt.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](implementing-optimistic-concurrency-vb.md)
> [Weiter](limiting-data-modification-functionality-based-on-the-user-vb.md)
