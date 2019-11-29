---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Behandeln von Ausnahmen auf BLL-und Dal-Ebene auf einer ASP.NET-Seite (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie eine benutzerfreundliche, informative Fehlermeldung angezeigt wird, wenn eine Ausnahme während eines Einfügungs-, Update-oder Löschvorgangs von...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: ee277596ade18d2603892d134b47c2c8697836bb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621050"
---
# <a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Verarbeiten von Ausnahmen auf BLL- und DAL-Ebene in einer ASP.NET-Seite (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) oder [PDF herunterladen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> In diesem Tutorial wird erläutert, wie eine benutzerfreundliche, informative Fehlermeldung angezeigt wird, wenn eine Ausnahme während eines Einfügungs-, Aktualisierungs-oder Löschvorgangs eines ASP.NET Data Web-Steuer Elements auftritt.

## <a name="introduction"></a>Einführung

Das Arbeiten mit Daten aus einer ASP.NET-Webanwendung mithilfe einer mehrstufigen Anwendungsarchitektur umfasst die folgenden drei allgemeinen Schritte:

1. Bestimmen Sie, welche Methode der Geschäftslogik Schicht aufgerufen werden muss und welche Parameterwerte Sie übergeben müssen. Die Parameterwerte können hart codiert, Programm gesteuert zugewiesen oder Eingaben sein, die vom Benutzer eingegeben wurden.
2. Rufen Sie die Methode auf.
3. Verarbeiten Sie die Ergebnisse. Wenn eine BLL-Methode aufgerufen wird, die Daten zurückgibt, kann dies das Binden der Daten an ein datenweb Steuerelement beinhalten. Bei BLL-Methoden, mit denen Daten geändert werden, kann dies das Ausführen von Aktionen auf der Grundlage eines Rückgabewerts oder die ordnungsgemäße Behandlung von Ausnahmen, die in Schritt 2 aufgetreten sind, umfassen.

Wie bereits im [vorherigen Tutorial](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)gezeigt, stellen sowohl die ObjectDataSource-als auch die Data Web-Steuerelemente Erweiterbarkeits Punkte für die Schritte 1 und 3 bereit. Die GridView löst z. b. das `RowUpdating`-Ereignis aus, bevor die Feldwerte der `UpdateParameters` Collection von ObjectDataSource zugewiesen werden. Das `RowUpdated` Ereignis wird ausgelöst, nachdem der Vorgang von ObjectDataSource abgeschlossen wurde.

Wir haben bereits die Ereignisse untersucht, die in Schritt 1 ausgelöst werden, und haben gesehen, wie Sie verwendet werden können, um die Eingabeparameter anzupassen oder den Vorgang abzubrechen. In diesem Tutorial werden wir uns auf die Ereignisse konzentrieren, die nach Abschluss des Vorgangs ausgelöst werden. Mit diesen Ereignis Handlern auf der Post-Ebene können Sie unter anderem ermitteln, ob eine Ausnahme während des Vorgangs aufgetreten ist, und diese ordnungsgemäß behandeln. dabei wird eine benutzerfreundliche, informative Fehlermeldung auf dem Bildschirm angezeigt, anstelle der standardmäßigen ASP.net Ausnahme Seite.

Um die Arbeit mit diesen Ereignissen auf postebene zu veranschaulichen, erstellen wir eine Seite, auf der die Produkte in einer bearbeitbaren GridView aufgeführt sind. Wenn beim Aktualisieren eines Produkts eine Ausnahme ausgelöst wird, wird auf der ASP.NET-Seite eine kurze Meldung oberhalb der GridView angezeigt, in der das Problem aufgetreten ist. Fangen wir an!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Schritt 1: Erstellen einer bearbeitbaren GridView für Produkte

Im vorherigen Tutorial haben wir eine bearbeitbare GridView mit nur zwei Feldern erstellt, `ProductName` und `UnitPrice`. Dies erforderte das Erstellen einer zusätzlichen Überladung für die `UpdateProduct`-Methode der `ProductsBLL` Klasse, von der nur drei Eingabeparameter (der Name, der Einheitspreis und die ID) des Produkts im Gegensatz zu einem Parameter für jedes Produktfeld akzeptiert wurden. In diesem Tutorial wird dieses Verfahren wiederholt, indem eine bearbeitbare GridView erstellt wird, in der der Name, die Menge pro Einheit, der Einheitspreis und die Einheiten im Lager angezeigt werden, aber nur der Name, der Einheitspreis und die Einheiten in Aktien bearbeitet werden können.

Um dieses Szenario zu ermöglichen, benötigen wir eine weitere Überladung der `UpdateProduct`-Methode, die vier Parameter akzeptiert: den Namen des Produkts, den Einzelpreis, die Einheiten in Aktien und die ID. Fügen Sie der `ProductsBLL`-Klasse die folgende Methode hinzu:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Nachdem diese Methode abgeschlossen ist, können Sie die ASP.NET-Seite erstellen, die die Bearbeitung dieser vier speziellen Produktfelder ermöglicht. Öffnen Sie die Seite `ErrorHandling.aspx` im Ordner `EditInsertDelete`, und fügen Sie der Seite über den Designer eine GridView hinzu. Binden Sie das GridView-Objekt an eine neue ObjectDataSource, und ordnen Sie die `Select()`-Methode der `GetProducts()`-Methode der `ProductsBLL`-Klasse und der `Update()`-Methode der soeben erstellten `UpdateProduct` Überladung zu.

[![die UpdateProduct-Methoden Überladung verwenden, die vier Eingabeparameter akzeptiert.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Abbildung 1**: Verwenden der `UpdateProduct`-Methoden Überladung, die vier Eingabeparameter akzeptiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))

Dadurch wird eine ObjectDataSource mit einer `UpdateParameters` Auflistung mit vier Parametern und einer GridView mit einem Feld für jedes der Produktfelder erstellt. Das deklarative Markup von ObjectDataSource weist die `OldValuesParameterFormatString`-Eigenschaft dem Wert `original_{0}`zu. Dies führt zu einer Ausnahme, da die BLL-Klasse nicht erwartet, dass ein Eingabeparameter mit dem Namen `original_productID` übergeben wird. Vergessen Sie nicht, diese Einstellung vollständig aus der deklarativen Syntax zu entfernen (oder legen Sie Sie auf den Standardwert fest, `{0}`).

Im nächsten Schritt wird die GridView-Ansicht heruntergefahren, um nur die `ProductName`, `QuantityPerUnit`, `UnitPrice`und `UnitsInStock` boundfields einzuschließen. Sie können auch beliebige Formatierungen auf Feldebene anwenden, die Sie als notwendig betrachten (z. b. das Ändern der `HeaderText` Eigenschaften).

Im vorherigen Tutorial haben wir uns mit dem Formatieren des `UnitPrice` BoundField als Währung im schreibgeschützten Modus und im Bearbeitungsmodus beschäftigt. Gehen wir hier wie folgt vor. Beachten Sie, dass dies erforderlich war, um die `DataFormatString`-Eigenschaft von BoundField auf `{0:c}`, dessen `HtmlEncode`-Eigenschaft auf `false`und die `ApplyFormatInEditMode` `true`, wie in Abbildung 2 dargestellt, festzulegen.

[!["UnitPrice BoundField" so konfigurieren, dass es als Währung angezeigt wird.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Abbildung 2**: Konfigurieren des `UnitPrice` BoundField für die Anzeige als Währung ([Klicken Sie, um das Bild in voller Größe](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png)anzuzeigen)

Wenn Sie die `UnitPrice` als Währung in der Bearbeitungs Schnittstelle formatieren, müssen Sie einen Ereignishandler für das `RowUpdating` Ereignis der GridView erstellen, das die Währungs formatierte Zeichenfolge in einen `decimal` Wert analysiert. Beachten Sie, dass der `RowUpdating` Ereignishandler aus dem letzten Tutorial ebenfalls geprüft hat, um sicherzustellen, dass der Benutzer einen `UnitPrice` Wert bereitgestellt hat. In diesem Tutorial ermöglicht es dem Benutzer jedoch, den Preis zu weglassen.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Unsere GridView enthält eine `QuantityPerUnit` BoundField, aber dieses BoundField sollte nur zu Anzeige Zwecken verwendet werden und sollte nicht vom Benutzer bearbeitet werden können. Um dies anzuordnen, legen Sie einfach die `ReadOnly` Eigenschaft boundfields auf `true`fest.

[![das "QuantityPerUnit BoundField" schreibgeschützt machen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Abbildung 3**: Festlegen des `QuantityPerUnit` BoundField als schreibgeschützt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))

Aktivieren Sie abschließend das Kontrollkästchen Bearbeitung aktivieren im Smarttags von GridView. Nachdem Sie diese Schritte ausgeführt haben, sollte der Designer der `ErrorHandling.aspx` Seite in etwa wie in Abbildung 4 aussehen.

[![alle außer den benötigten boundfields entfernen, und aktivieren Sie das Kontrollkästchen "Bearbeiten aktivieren".](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Abbildung 4**: Entfernen aller außer der benötigten boundfields-Datei und Aktivieren des Kontrollkästchens "Bearbeiten aktivieren" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png)

An dieser Stelle wird eine Liste aller Felder `ProductName`, `QuantityPerUnit`, `UnitPrice`und `UnitsInStock` der Produkte angezeigt. Es können jedoch nur die Felder `ProductName`, `UnitPrice`und `UnitsInStock` bearbeitet werden.

[![Benutzer können jetzt ganz einfach die Namen, Preise und Einheiten von Produkten in Aktien Feldern bearbeiten.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Abbildung 5**: Benutzer können jetzt ganz einfach die Namen, Preise und Einheiten der Produkte in Aktien Feldern bearbeiten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))

## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Schritt 2: ordnungsgemäße Behandlung von Ausnahmen auf dal-Ebene

Während unsere bearbeitbare GridView wunderbar funktioniert, wenn Benutzer zulässige Werte für den Namen, den Preis und die Einheiten der bearbeiteten Produkte eingeben, führt die Eingabe von ungültigen Werten zu einer Ausnahme. Wenn Sie z. b. den `ProductName` Wert weglassen, wird eine " [nonullzuzudexception](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) " ausgelöst, da die Eigenschaft "`ProductName`" in der `ProductsRow` Klasse auf `false`festgelegt `AllowDBNull` ist. Wenn die Datenbank nicht herunter ist, wird vom TableAdapter eine `SqlException` ausgelöst, wenn versucht wird, eine Verbindung mit der Datenbank herzustellen. Ohne eine Aktion Blasen diese Ausnahmen von der Datenzugriffs Ebene bis zur Geschäftslogik Schicht, dann von der ASP.NET Seite und schließlich von der ASP.NET-Laufzeit.

Abhängig davon, wie Ihre Webanwendung konfiguriert ist und ob Sie die Anwendung von `localhost`aus besuchen, kann eine nicht behandelte Ausnahme zu einer generischen Server Fehlerseite, einem ausführlichen Fehlerbericht oder einer benutzerfreundlichen Webseite führen. Weitere Informationen dazu, wie die ASP.NET-Laufzeit auf eine nicht abgefangene Ausnahme antwortet, finden Sie unter [Fehlerbehandlung für Webanwendungen in ASP.net](http://www.15seconds.com/issue/030102.htm) und im [customErrors-Element](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) .

Abbildung 6 zeigt den Bildschirm, der beim Versuch aufgetreten ist, ein Produkt zu aktualisieren, ohne den `ProductName` Wert anzugeben. Dies ist der standardmäßige ausführliche Fehlerbericht, der angezeigt wird, wenn `localhost`angezeigt wird.

[![Weglassen des Produkt namens werden Ausnahme Details angezeigt.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Abbildung 6**: das Weglassen des Produkt namens zeigt Ausnahme Details an ([Klicken Sie, um das Bild in voller Größe](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png)anzuzeigen)

Obwohl solche Ausnahme Details beim Testen einer Anwendung hilfreich sind, ist die Darstellung eines Endbenutzers mit einem solchen Bildschirm in einer Ausnahme kleiner als ideal. Ein Endbenutzer weiß wahrscheinlich nicht, was ein `NoNullAllowedException` ist oder warum er verursacht wurde. Ein besserer Ansatz besteht darin, dem Benutzer eine benutzerfreundliche Meldung zu präsentieren, in der erläutert wird, dass beim Versuch, das Produkt zu aktualisieren, Probleme aufgetreten sind.

Wenn beim Ausführen des Vorgangs eine Ausnahme auftritt, stellen die Ereignisse auf der Post-Ebene in der ObjectDataSource und im datenweb-Steuerelement ein Mittel bereit, um Sie zu erkennen und die Ausnahme von der Blasen Suche zur ASP.NET-Laufzeit abzubrechen. In unserem Beispiel erstellen wir einen Ereignishandler für das `RowUpdated` Ereignis von GridView, das bestimmt, ob eine Ausnahme ausgelöst wurde, und zeigt, wenn dies der Fall ist, die Ausnahme Details in einem Label-websteuer Element an.

Fügen Sie zunächst der Seite ASP.net eine Bezeichnung hinzu, und legen Sie die `ID`-Eigenschaft auf `ExceptionDetails` und deren `Text`-Eigenschaft fest. Um das Auge des Benutzers auf diese Meldung zu zeichnen, legen Sie die `CssClass`-Eigenschaft auf `Warning`fest. dabei handelt es sich um eine CSS-Klasse, die der `Styles.css`-Datei im vorherigen Tutorial hinzugefügt wurde. Beachten Sie, dass diese CSS-Klasse bewirkt, dass der Text der Bezeichnung in einer roten, kursiv, Fett und extra großen Schriftart angezeigt wird.

[![der Seite ein Label-websteuer Element hinzufügen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Abbildung 7**: Hinzufügen eines Label-websteuer Elements zur Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))

Da dieses Bezeichnungs-websteuer Element nur unmittelbar nach dem Auftreten einer Ausnahme sichtbar sein soll, legen Sie die `Visible`-Eigenschaft im `Page_Load`-Ereignishandler auf false fest:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Mit diesem Code wird auf der ersten Seite und nachfolgenden Postbacks das `ExceptionDetails`-Steuerelement seine `Visible`-Eigenschaft auf `false`festgelegt. Bei einer Ausnahme auf dal-oder BLL-Ebene, die im `RowUpdated` Ereignishandler von GridView erkannt werden kann, wird die `Visible`-Eigenschaft des `ExceptionDetails` Steuer Elements auf true festgelegt. Da websteuer Element-Ereignishandler nach dem `Page_Load`-Ereignishandler im Seiten Lebenszyklus auftreten, wird die Bezeichnung angezeigt. Beim nächsten Postback setzt der `Page_Load` Ereignishandler die `Visible`-Eigenschaft jedoch wieder auf `false`zurück, wobei Sie erneut aus der Ansicht ausgeblendet wird.

> [!NOTE]
> Alternativ dazu können Sie die Notwendigkeit entfernen, die `Visible`-Eigenschaft des `ExceptionDetails`-Steuer Elements in `Page_Load` festzulegen, indem Sie die `Visible`-Eigenschaft `false` in der deklarativen Syntax zuweisen und den Ansichts Zustand (Festlegen der `EnableViewState`-Eigenschaft auf `false`) deaktivieren. Dieser Alternative Ansatz wird in einem zukünftigen Tutorial verwendet.

Wenn das Label-Steuerelement hinzugefügt wurde, ist der nächste Schritt das Erstellen des Ereignis Handlers für das `RowUpdated` Ereignis der GridView. Wählen Sie im Designer die GridView aus, wechseln Sie zum Eigenschaftenfenster, und klicken Sie auf das Blitz Symbol, um die Ereignisse der GridView aufzulisten. Für das `RowUpdating` Ereignis der GridView sollte bereits ein Eintrag vorhanden sein, da wir einen Ereignishandler für dieses Ereignis zuvor in diesem Tutorial erstellt haben. Erstellen Sie auch einen Ereignishandler für das `RowUpdated` Ereignis.

![Erstellen eines Ereignis Handlers für das rowaktualisierte-Ereignis von GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Abbildung 8**: Erstellen eines Ereignis Handlers für das `RowUpdated` Ereignis der GridView

> [!NOTE]
> Sie können den Ereignishandler auch über die Dropdown Listen am Anfang der Code Behind-Klassendatei erstellen. Wählen Sie in der Dropdown Liste auf der linken Seite das GridView-Ereignis aus, und klicken Sie auf der rechten Seite `RowUpdated` Ereignis.

Wenn Sie diesen Ereignishandler erstellen, wird der Code Behind-Klasse der ASP.NET-Seite der folgende Code hinzugefügt:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Der zweite Eingabeparameter dieses Ereignis Handlers ist ein Objekt vom Typ " [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx)", das drei Eigenschaften hat, die für die Behandlung von Ausnahmen von Interesse sind:

- `Exception` einen Verweis auf die ausgelöste Ausnahme ab. Wenn keine Ausnahme ausgelöst wurde, hat diese Eigenschaft den Wert `null`
- `ExceptionHandled` einen booleschen Wert ab, der angibt, ob die Ausnahme im `RowUpdated`-Ereignishandler behandelt wurde. Wenn `false` (die Standardeinstellung), wird die Ausnahme erneut ausgelöst, die bis zur ASP.NET-Laufzeit durchläuft.
- `KeepInEditMode`, wenn `true` der bearbeiteten GridView-Zeile im Bearbeitungsmodus verbleibt. Wenn `false` (Standardeinstellung), wird die GridView-Zeile wieder in den schreibgeschützten Modus versetzt.

Der Code sollte dann überprüfen, ob `Exception` nicht `null`ist, was bedeutet, dass bei der Ausführung des Vorgangs eine Ausnahme ausgelöst wurde. Wenn dies der Fall ist, möchten wir Folgendes tun:

- Anzeigen einer benutzerfreundlichen Meldung in der `ExceptionDetails` Bezeichnung
- Geben Sie an, dass die Ausnahme behandelt wurde.
- GridView-Zeile im Bearbeitungsmodus belassen

Mit dem folgenden Code werden die folgenden Ziele erreicht:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Der Ereignishandler beginnt mit der Überprüfung, ob `e.Exception` `null`ist. Wenn dies nicht der Fall ist, wird die `Visible`-Eigenschaft der `ExceptionDetails` Bezeichnung auf `true` und deren `Text`-Eigenschaft auf "Es gab ein Problem beim Aktualisieren des Produkts" festgelegt. Die Details der eigentlichen Ausnahme, die ausgelöst wurde, befinden sich in der `InnerException`-Eigenschaft des `e.Exception` Objekts. Diese innere Ausnahme wird untersucht, und wenn es sich um einen bestimmten Typ handelt, wird eine zusätzliche, hilfreiche Nachricht an die `Text`-Eigenschaft der `ExceptionDetails` Bezeichnung angehängt. Zum Schluss sind die Eigenschaften `ExceptionHandled` und `KeepInEditMode` beide auf `true`festgelegt.

Abbildung 9 zeigt einen Screenshot dieser Seite, wenn der Name des Produkts weggelassen wird. Abbildung 10 zeigt die Ergebnisse bei der Eingabe eines ungültigen `UnitPrice` Werts (-50).

[![ProductName BoundField muss einen Wert enthalten.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Abbildung 9**: das `ProductName` BoundField muss einen Wert enthalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))

[![negative UnitPrice-Werte sind nicht zulässig.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Abbildung 10**: negative `UnitPrice` Werte sind nicht zulässig ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))

Wenn Sie die `e.ExceptionHandled`-Eigenschaft auf `true`festlegen, hat der `RowUpdated`-Ereignishandler angegeben, dass die Ausnahme behandelt wurde. Daher wird die Ausnahme nicht bis zur ASP.NET-Laufzeit weitergegeben.

> [!NOTE]
> Die Abbildungen 9 und 10 veranschaulichen eine ordnungsgemäße Methode, um Ausnahmen zu behandeln, die aufgrund ungültiger Benutzereingaben ausgelöst wurden. Im Idealfall erreichen solche ungültige Eingaben jedoch nie zuerst die Geschäftslogik Schicht, da die ASP.NET-Seite sicherstellen sollte, dass die Eingaben des Benutzers gültig sind, bevor die `UpdateProduct` Methode der `ProductsBLL` Klasse aufgerufen wird. Im nächsten Tutorial erfahren Sie, wie Sie den Bearbeitungs-und einfügeschnittstellen Validierungs Steuerelemente hinzufügen, um sicherzustellen, dass die an die Geschäftslogik Schicht übermittelten Daten den Geschäftsregeln entsprechen. Die Validierungs Steuerelemente verhindern nicht nur den Aufruf der `UpdateProduct` Methode, bis die vom Benutzer bereitgestellten Daten gültig sind, sondern bieten auch eine informative Benutzer Darstellung, um Probleme mit der Dateneingabe zu identifizieren.

## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Schritt 3: ordnungsgemäße Behandlung von Ausnahmen auf BLL-Ebene

Beim Einfügen, aktualisieren oder Löschen von Daten kann die Datenzugriffs Ebene bei einem datenbezogenen Fehler eine Ausnahme auslösen. Möglicherweise ist die Datenbank offline, für eine erforderliche Datenbanktabellen Spalte wurde kein Wert angegeben, oder eine Einschränkung auf Tabellenebene wurde verletzt. Zusätzlich zu streng datenbezogenen Ausnahmen kann die Geschäftslogik Schicht Ausnahmen verwenden, um anzugeben, wann Geschäftsregeln verletzt wurden. Im Lernprogramm zum [Erstellen einer Geschäftslogik Schicht](../introduction/creating-a-business-logic-layer-vb.md) haben wir z. b. eine Geschäftsregel Überprüfung zur ursprünglichen `UpdateProduct` Überladung hinzugefügt. Insbesondere, wenn der Benutzer ein Produkt als nicht mehr unterstützt markiert hat, ist es erforderlich, dass das Produkt nicht das einzige vom Lieferanten bereitgestellte Produkt ist. Wenn diese Bedingung verletzt wurde, wurde ein `ApplicationException` ausgelöst.

Fügen Sie für die in diesem Tutorial erstellte `UpdateProduct` Überladung eine Geschäftsregel hinzu, die verhindert, dass das `UnitPrice` Feld auf einen neuen Wert festgelegt wird, der mehr als doppelt so groß ist wie der ursprüngliche `UnitPrice` Wert. Um dies zu erreichen, passen Sie die `UpdateProduct` Überladung so an, dass diese Überprüfung durchgeführt wird, und löst eine `ApplicationException`, wenn die Regel verletzt wird. Die aktualisierte Methode folgt:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Durch diese Änderung führt jedes Preis Update, das mehr als doppelt so hoch ist wie der vorhandene Preis, dazu, dass eine `ApplicationException` ausgelöst wird. Genau wie bei der von der dal ausgelöste Ausnahme kann diese BLL-ausgelöste `ApplicationException` erkannt und im `RowUpdated` Ereignishandler der GridView behandelt werden. Tatsächlich erkennt der Code des `RowUpdated` Ereignis Handlers diese Ausnahme ordnungsgemäß und zeigt den `Message` Eigenschafts Wert des `ApplicationException`an. Abbildung 11 zeigt einen Screenshot, wenn ein Benutzer versucht, den Preis von Chai auf $50,00 zu aktualisieren. Dies ist größer als der aktuelle Preis von $19,95.

[![die Geschäftsregeln Preissteigerungen nicht zulassen, die mehr als den doppelten Preis eines Produkts überschreiten.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Abbildung 11**: die Geschäftsregeln lassen Preissteigerungen nicht zu, die mehr als den doppelten Preis eines Produkts überschreiten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png)).

> [!NOTE]
> Im Idealfall würden unsere Geschäftslogik Regeln aus den `UpdateProduct` Methoden Überladungen und in eine gängige Methode umgestaltet werden. Dies bleibt für den Reader eine Übung.

## <a name="summary"></a>Summary

Beim Einfügen, aktualisieren und Löschen von Vorgängen lösen sowohl das datenweb Steuerelement als auch die ObjectDataSource Ereignisse vor und nach der Ebene aus, die den eigentlichen Vorgang beenden. Wie in diesem Tutorial und der vorhergehenden erläutert wurde, wird beim Arbeiten mit einem bearbeitbaren GridView-Ereignis das `RowUpdating` Ereignis von GridView ausgelöst, gefolgt vom `Updating`-Ereignis von ObjectDataSource. zu diesem Zeitpunkt wird der Update-Befehl an das zugrunde liegende Objekt von ObjectDataSource vorgenommen. Nachdem der Vorgang abgeschlossen wurde, wird das `Updated` Ereignis von ObjectDataSource ausgelöst, gefolgt vom `RowUpdated`-Ereignis der GridView.

Wir können Ereignishandler für die Ereignisse vor der Ebene erstellen, um die Eingabeparameter oder die Ereignisse auf der Post-Ebene anzupassen, um die Ergebnisse des Vorgangs zu überprüfen und auf diese zu reagieren. Ereignishandler auf Post-Ebene werden häufig verwendet, um zu erkennen, ob während des Vorgangs eine Ausnahme aufgetreten ist. Im Falle einer Ausnahme können diese Ereignishandler auf der Post-Ebene optional die Ausnahme selbst verarbeiten. In diesem Tutorial haben Sie erfahren, wie Sie eine solche Ausnahme behandeln, indem Sie eine benutzerfreundliche Fehlermeldung anzeigen.

Im nächsten Tutorial erfahren Sie, wie Sie die Wahrscheinlichkeit von Ausnahmen verringern, die durch Probleme mit der Datenformatierung entstehen (z. b. die Eingabe eines negativen `UnitPrice`). Insbesondere wird erläutert, wie Sie den Bearbeitungs-und einfügeschnittstellen Validierungs Steuerelemente hinzufügen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Liz shulok. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [Weiter](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
