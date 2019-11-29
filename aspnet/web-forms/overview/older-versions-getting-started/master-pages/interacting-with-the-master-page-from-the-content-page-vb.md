---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interaktion mit der Master Seite von der Inhaltsseite (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Untersucht, wie Methoden, Eigenschaften usw. der Master Seite aus dem Code auf der Seite Inhalt aufgerufen werden.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: fe1f7b80b26ff25c1ce744e4f823e3fb35eea074
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74577242"
---
# <a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interaktion mit der Inhaltsseite über die Masterseite (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Untersucht, wie Methoden, Eigenschaften usw. der Master Seite aus dem Code auf der Seite Inhalt aufgerufen werden.

## <a name="introduction"></a>Einführung

Im Verlauf der letzten fünf Tutorials haben wir uns mit dem Erstellen einer Master Seite, dem Definieren von Inhalts Bereichen, dem Binden von ASP.NET-Seiten an eine Master Seite und dem Definieren von Seiten spezifischem Inhalt beschäftigt. Wenn ein Besucher eine bestimmte Inhaltsseite anfordert, werden das Markup der Inhalts-und Masterseiten zur Laufzeit miteinander verschmolzen, was zum Rendern einer vereinheitlichten Steuerelement Hierarchie führt. Daher haben wir bereits eine Möglichkeit gesehen, wie die Master Seite und eine ihrer Inhaltsseiten interagieren können: die Inhaltsseite gibt das Markup aus, das in die contentplachalter-Steuerelemente der Master Seite übersetzt werden soll.

Wir haben noch untersucht, wie die Master Seite und die Inhaltsseite Programm gesteuert interagieren können. Zusätzlich zum Definieren des Markups für die contentplachalter-Steuerelemente der Master Seite kann eine Inhaltsseite auch den öffentlichen Eigenschaften der Master Seite Werte zuweisen und deren öffentliche Methoden aufrufen. Auf ähnliche Weise kann eine Master Seite mit ihren Inhaltsseiten interagieren. Während die programmgesteuerte Interaktion zwischen einer Master-und einer Inhaltsseite weniger häufig ist als die Interaktion zwischen ihren deklarativen Markups, gibt es viele Szenarios, in denen eine solche programmgesteuerte Interaktion erforderlich ist.

In diesem Tutorial untersuchen wir, wie eine Inhaltsseite Programm gesteuert mit der Master Seite interagieren kann. im nächsten Tutorial wird erläutert, wie die Master Seite auf ähnliche Weise mit ihren Inhaltsseiten interagieren kann.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Beispiele für die programmgesteuerte Interaktion zwischen einer Inhaltsseite und ihrer Master Seite

Wenn ein bestimmter Bereich einer Seite Seite für Seite konfiguriert werden muss, verwenden wir ein contentplachalter-Steuerelement. Aber wie sieht es mit Situationen aus, in denen die Mehrzahl der Seiten eine bestimmte Ausgabe ausgeben muss, aber eine kleine Anzahl von Seiten muss Sie so anpassen, dass Sie etwas anderes zeigt? Ein Beispiel, das wir im Lernprogramm mit [*mehreren Content-und Standard Inhalten*](multiple-contentplaceholders-and-default-content-vb.md) untersucht haben, beinhaltet die Anzeige einer Anmelde Schnittstelle auf jeder Seite. Die meisten Seiten sollten zwar eine Anmelde Schnittstelle enthalten, Sie sollten jedoch für eine Handvoll Seiten unterdrückt werden, z. b.: die Haupt Anmeldeseite (`Login.aspx`). die Seite "Konto erstellen"; und andere Seiten, auf die nur authentifizierte Benutzer zugreifen können. Im Tutorial " [*mehrere Inhalts Platzhalter und Standard Inhalte*](multiple-contentplaceholders-and-default-content-vb.md) " wurde gezeigt, wie Sie den Standard Inhalt für einen contentplachalter auf der Master Seite definieren und dann auf den Seiten überschreiben, auf denen der Standard Inhalt nicht gewünscht war.

Eine andere Möglichkeit besteht darin, eine öffentliche Eigenschaft oder Methode auf der Master Seite zu erstellen, die angibt, ob die Anmelde Schnittstelle angezeigt oder ausgeblendet werden soll. Die Master Seite könnte z. b. eine öffentliche Eigenschaft mit dem Namen `ShowLoginUI` enthalten, deren Wert verwendet wurde, um die `Visible`-Eigenschaft des Login-Steuer Elements auf der Master Seite festzulegen. Die Inhaltsseiten, auf denen die Anmelde Benutzeroberfläche unterdrückt werden soll, können dann die `ShowLoginUI`-Eigenschaft Programm gesteuert auf `False`festlegen.

Möglicherweise ist das häufigste Beispiel für die Interaktion von Inhalten und der Master Seite, wenn die auf der Master Seite angezeigten Daten aktualisiert werden müssen, nachdem eine Aktion auf der Inhaltsseite aufgetreten ist. Betrachten Sie eine Master Seite, die eine GridView enthält, in der die fünf zuletzt hinzugefügten Datensätze aus einer bestimmten Datenbanktabelle angezeigt werden und eine ihrer Inhaltsseiten eine Schnittstelle zum Hinzufügen neuer Datensätze zu derselben Tabelle enthält.

Wenn ein Benutzer die Seite besucht, um einen neuen Datensatz hinzuzufügen, werden die fünf zuletzt hinzugefügten Datensätze auf der Master Seite angezeigt. Nachdem Sie die Werte für die Spalten des neuen Datensatzes ausgefüllt haben, wird das Formular übermittelt. Wenn für die GridView auf der Master Seite die `EnableViewState`-Eigenschaft auf true (Standardeinstellung) festgelegt ist, wird der zugehörige Inhalt aus dem Ansichts Zustand neu geladen. Folglich werden die fünf gleichen Datensätze angezeigt, auch wenn ein neuerer Datensatz soeben der Datenbank hinzugefügt wurde. Dadurch kann der Benutzer verwirrend sein.

> [!NOTE]
> Auch wenn Sie den Ansichts Zustand der GridView deaktivieren, sodass er bei jedem Postback an die zugrunde liegende Datenquelle zurückgebunden wird, wird der soeben hinzugefügte Datensatz immer noch nicht angezeigt, da die Daten zuvor im Seiten Lebenszyklus an die GridView gebunden sind, als wenn der neue Datensatz dem datab hinzugefügt wird. Wissens.

Um dies zu beheben, damit der soeben hinzugefügte Datensatz in der GridView der Master Seite im Postback angezeigt wird, müssen wir die GridView anweisen, eine erneute Bindung an die Datenquelle herzustellen, *nachdem* der neue Datensatz der Datenbank hinzugefügt wurde. Dies erfordert eine Interaktion zwischen dem Inhalt und den Masterseiten, da die Schnittstelle zum Hinzufügen des neuen Datensatzes (und seiner Ereignishandler) auf der Seite Inhalt angezeigt wird, die zu Aktualisier Ende GridView jedoch auf der Master Seite angezeigt wird.

Da die Aktualisierung der Anzeige der Master Seite von einem Ereignishandler auf der Seite Inhalt einer der gängigsten Anforderungen für die Interaktion von Inhalten und Masterseiten ist, betrachten wir dieses Thema ausführlicher. Der Download für dieses Tutorial enthält eine Microsoft SQL Server 2005 Express Edition-Datenbank mit dem Namen `NORTHWIND.MDF` im `App_Data` Ordner der Website. In der Northwind-Datenbank werden Produkt-, Mitarbeiter-und Vertriebsinformationen für ein fiktives Unternehmen, Northwind Traders, gespeichert.

In Schritt 1 werden die fünf zuletzt hinzugefügten Produkte in einer GridView auf der Master Seite angezeigt. Schritt 2 erstellt eine Inhaltsseite zum Hinzufügen neuer Produkte. In Schritt 3 wird erläutert, wie Sie öffentliche Eigenschaften und Methoden auf der Master Seite erstellen, und Schritt 4 veranschaulicht, wie Sie eine programmgesteuerte Schnittstelle mit diesen Eigenschaften und Methoden von der Inhaltsseite aus erstellen.

> [!NOTE]
> In diesem Tutorial werden die Besonderheiten der Arbeit mit Daten in ASP.net nicht behandelt. Die Schritte zum Einrichten der Master Seite zum Anzeigen von Daten und der Inhaltsseite für das Einfügen von Daten sind fertig, aber immer noch. Eine ausführlichere Betrachtung der Anzeige und des Einfügens von Daten und der Verwendung der SqlDataSource-und GridView-Steuerelemente finden Sie im Abschnitt Weitere Messwerte am Ende dieses Tutorials.

## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Schritt 1: Anzeigen der fünf zuletzt hinzugefügten Produkte auf der Master Seite

Öffnen Sie die Master Seite Site. Master, und fügen Sie der `leftContent` `<div>`eine Bezeichnung und ein GridView-Steuerelement hinzu. Löschen Sie die `Text`-Eigenschaft der Bezeichnung, legen Sie die `EnableViewState`-Eigenschaft auf `False`und deren `ID`-Eigenschaft auf `GridMessage`fest. Legen Sie die `ID`-Eigenschaft der GridView auf `RecentProducts`fest. Erweitern Sie anschließend im Designer das Smarttag der GridView, und wählen Sie die Bindung an eine neue Datenquelle aus. Dadurch wird der Assistent zum Konfigurieren von Datenquellen gestartet. Da die Datenbank Northwind im `App_Data` Ordner eine Microsoft SQL Server Datenbank ist, wählen Sie aus, ob Sie eine SqlDataSource erstellen möchten, indem Sie auswählen (siehe Abbildung 1). Benennen Sie die SqlDataSource-`RecentProductsDataSource`.

[![das GridView-Steuerelement an ein SqlDataSource-Steuerelement namens recentproductdatasource binden.](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Abbildung 01**: Binden der GridView an ein SqlDataSource-Steuerelement mit dem Namen `RecentProductsDataSource` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))

Im nächsten Schritt werden wir aufgefordert, anzugeben, mit welcher Datenbank eine Verbindung hergestellt werden soll. Wählen Sie in der Dropdown Liste die `NORTHWIND.MDF` Datenbankdatei aus, und klicken Sie auf Weiter. Da diese Datenbank zum ersten Mal verwendet wird, bietet der Assistent die Verwendung der Verbindungs Zeichenfolge in `Web.config`. Speichern Sie die Verbindungs Zeichenfolge mit dem Namen `NorthwindConnectionString`.

[![Herstellen einer Verbindung mit der Northwind-Datenbank](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Abbildung 02**: Herstellen der Verbindung mit der Northwind-Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))

Der Assistent zum Konfigurieren von Datenquellen bietet zwei Möglichkeiten, um die Abfrage anzugeben, die zum Abrufen von Daten verwendet wird:

- Durch Angeben einer benutzerdefinierten SQL-Anweisung oder gespeicherten Prozedur oder
- Durch Auswahl einer Tabelle oder Sicht und anschließendes angeben der zurück zugebende Spalten

Da wir nur die fünf zuletzt hinzugefügten Produkte zurückgeben möchten, müssen wir eine benutzerdefinierte SQL-Anweisung angeben. Verwenden Sie die folgende `SELECT` Abfrage:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

Das `TOP 5`-Schlüsselwort gibt nur die ersten fünf Datensätze aus der Abfrage zurück. Der Primärschlüssel der `Products` Tabelle (`ProductID`) ist eine `IDENTITY` Spalte, mit der wir sichergestellt werden, dass jedes neue Produkt, das der Tabelle hinzugefügt wird, einen größeren Wert als der vorherige Eintrag hat. Wenn Sie die Ergebnisse nach `ProductID` in absteigender Reihenfolge sortieren, werden die Produkte zurückgegeben, beginnend mit den zuletzt erstellten.

[die fünf zuletzt hinzugefügten Produkte werden ![zurückgegeben.](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Abbildung 03**: Zurückgeben der fünf zuletzt hinzugefügten Produkte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))

Nachdem Sie den Assistenten abgeschlossen haben, generiert Visual Studio zwei boundfields für die GridView, um die `ProductName`-und `UnitPrice` Felder anzuzeigen, die von der Datenbank zurückgegeben werden. An diesem Punkt sollte das deklarative Markup der Master Seite ein Markup enthalten, das dem folgenden ähnelt:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Wie Sie sehen, enthält das Markup Folgendes: das Beschriftungs-websteuer Element (`GridMessage`). die GridView-`RecentProducts`mit zwei boundfields. und ein SqlDataSource-Steuerelement, das die fünf zuletzt hinzugefügten Produkte zurückgibt.

Wenn Sie diese GridView erstellt und das zugehörige SqlDataSource-Steuerelement konfiguriert haben, besuchen Sie die Website über einen Browser. Wie in Abbildung 4 gezeigt, wird ein Raster in der unteren linken Ecke angezeigt, in dem die fünf zuletzt hinzugefügten Produkte aufgeführt sind.

[![in der GridView werden die fünf zuletzt hinzugefügten Produkte angezeigt.](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Abbildung 04**: in der GridView werden die fünf zuletzt hinzugefügten Produkte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))

> [!NOTE]
> Sie können die Darstellung der GridView jederzeit bereinigen. Einige Vorschläge umfassen das Formatieren des angezeigten `UnitPrice` Werts als Währung und das Verwenden von Hintergrundfarben und Schriftarten, um die Darstellung des Rasters zu verbessern.

## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Schritt 2: Erstellen einer Inhaltsseite zum Hinzufügen neuer Produkte

Die nächste Aufgabe besteht darin, eine Inhaltsseite zu erstellen, von der ein Benutzer der `Products` Tabelle ein neues Produkt hinzufügen kann. Fügen Sie eine neue Inhaltsseite zum Ordner "`Admin`" mit dem Namen `AddProduct.aspx`hinzu, und stellen Sie sicher, dass Sie an die `Site.master` Master Seite gebunden ist. Abbildung 5 zeigt die Projektmappen-Explorer, nachdem diese Seite der Website hinzugefügt wurde.

[![dem Administrator Ordner eine neue ASP.NET-Seite hinzufügen](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Abbildung 05**: Hinzufügen einer neuen ASP.NET-Seite zum Ordner "`Admin`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))

Beachten Sie, dass im Tutorial zum [*angeben des Titels, der Meta-Tags und anderer HTML-Header in der Master Seite*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) eine benutzerdefinierte Basis Seiten Klasse mit dem Namen `BasePage` erstellt wurde, die den Titel der Seite generiert hat, wenn er nicht explizit festgelegt wurde. Wechseln Sie zur Code Behind-Klasse der `AddProduct.aspx` Seite, und lassen Sie Sie von `BasePage` (anstelle von `System.Web.UI.Page`) ableiten.

Aktualisieren Sie abschließend die `Web.sitemap` Datei, sodass Sie einen Eintrag für diese Lektion enthält. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für die Lektion namens Probleme der Steuerelement-ID hinzu:

[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Wie in Abbildung 6 gezeigt, wird das Hinzufügen dieses `<siteMapNode>` Elements in der Liste der Lektionen widergespiegelt.

Kehren Sie zu `AddProduct.aspx`zurück. Fügen Sie im Inhalts Steuerelement für den `MainContent` contentplachalter ein DetailsView-Steuerelement hinzu, und benennen Sie es `NewProduct`. Binden Sie die DetailsView an ein neues SqlDataSource-Steuerelement mit dem Namen `NewProductDataSource`. Konfigurieren Sie den Assistenten wie bei SqlDataSource in Schritt 1 so, dass er die Datenbank Northwind verwendet, und wählen Sie eine benutzerdefinierte SQL-Anweisung aus. Da die DetailsView zum Hinzufügen von Elementen zur Datenbank verwendet wird, müssen wir sowohl eine `SELECT`-Anweisung als auch eine `INSERT`-Anweisung angeben. Verwenden Sie die folgende `SELECT` Abfrage:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Fügen Sie dann auf der Registerkarte Einfügen die folgende `INSERT`-Anweisung hinzu:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Nachdem Sie den Assistenten abgeschlossen haben, wechseln Sie zum Smarttag der DetailsView, und aktivieren Sie das Kontrollkästchen "Einfügen aktivieren". Dadurch wird der DetailsView ein CommandField hinzugefügt, dessen `ShowInsertButton`-Eigenschaft auf true festgelegt ist. Da diese DetailsView ausschließlich für das Einfügen von Daten verwendet wird, legen Sie die `DefaultMode`-Eigenschaft der DetailsView auf `Insert`fest.

Das ist schon alles! Testen Sie diese Seite. Besuchen Sie `AddProduct.aspx` über einen Browser, und geben Sie einen Namen und einen Preis ein (siehe Abbildung 6).

[![der Datenbank ein neues Produkt hinzufügen](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Abbildung 06**: Hinzufügen eines neuen Produkts zur Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))

Nachdem Sie den Namen und den Preis für das neue Produkt eingegeben haben, klicken Sie auf die Schaltfläche einfügen. Dies bewirkt, dass das Formular Postback durchgesetzt wird. Beim Postback wird die `INSERT`-Anweisung des SqlDataSource-Steuer Elements ausgeführt. die beiden Parameter werden mit den vom Benutzer eingegebenen Werten in den zwei TextBox-Steuerelementen der DetailsView aufgefüllt. Leider gibt es kein visuelles Feedback, dass eine Einfügung aufgetreten ist. Es wäre schön, dass eine Meldung angezeigt wird, die bestätigt, dass ein neuer Datensatz hinzugefügt wurde. Ich lasse dies als Übung für den Reader aus. Nach dem Hinzufügen eines neuen Datensatzes aus der DetailsView zeigt die GridView auf der Master Seite weiterhin dieselben fünf Datensätze an wie zuvor. der soeben hinzugefügte Datensatz ist nicht enthalten. Wir untersuchen, wie dies in den nächsten Schritten behoben werden kann.

> [!NOTE]
> Zusätzlich zum Hinzufügen einer Form des visuellen Feedbacks, dass die Einfügung erfolgreich war, empfiehlt es sich, auch die einfügeschnittstelle der DetailsView so zu aktualisieren, dass Sie die Validierung einschließt. Zurzeit ist keine Validierung vorhanden. Wenn ein Benutzer einen ungültigen Wert für das `UnitPrice` Feld, z. b. "zu teuer", eingibt, wird eine Ausnahme beim Postback ausgelöst, wenn das System versucht, diese Zeichenfolge in eine Dezimalzahl zu konvertieren. Weitere Informationen zum Anpassen der einfügeschnittstelle finden Sie im Tutorial [ *Anpassen der Daten Änderungs Schnittstelle* ](https://asp.net/learn/data-access/tutorial-20-vb.aspx) in der Reihe " [Arbeiten mit Daten](../../data-access/index.md)".

## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Schritt 3: Erstellen von öffentlichen Eigenschaften und Methoden auf der Master Seite

In Schritt 1 haben wir auf der Master Seite ein Label-websteuer Element mit dem Namen `GridMessage` oberhalb der GridView hinzugefügt. Diese Bezeichnung soll optional eine Meldung anzeigen. Wenn Sie z. b. der `Products` Tabelle einen neuen Datensatz hinzugefügt haben, möchten Sie möglicherweise eine Meldung anzeigen, die Folgendes enthält: "*ProductName* wurde der Datenbank hinzugefügt." Anstatt den Text für diese Bezeichnung auf der Master Seite hart zu codieren, möchten wir möglicherweise, dass die Nachricht von der Inhaltsseite angepasst wird.

Da das Label-Steuerelement als geschützte Element Variable innerhalb der Master Seite implementiert ist, kann auf Inhaltsseiten nicht direkt zugegriffen werden. Um die Bezeichnung in einer Master Seite von der Inhaltsseite aus zu bearbeiten (oder für jedes websteuer Element auf der Master Seite), müssen wir auf der Master Seite eine öffentliche Eigenschaft erstellen, die das websteuer Element verfügbar macht oder als Proxy fungiert, mit dem eine ihrer Eigenschaften sein kann.  auf. Fügen Sie der Code Behind-Klasse der Master Seite die folgende Syntax hinzu, um die `Text`-Eigenschaft der Bezeichnung verfügbar zu machen:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Wenn der `Products` Tabelle auf einer Inhaltsseite ein neuer Datensatz hinzugefügt wird, muss die `RecentProducts` GridView auf der Master Seite erneut an die zugrunde liegende Datenquelle gebunden werden. Zum erneuten Binden der GridView-Methode wird die `DataBind`-Methode aufgerufen. Da die GridView auf der Master Seite nicht Programm gesteuert für die Inhaltsseiten zugänglich ist, müssen wir eine öffentliche Methode auf der Master Seite erstellen, die die Daten bei der aufgerufenen wieder an die GridView bindet. Fügen Sie der Code Behind-Klasse der Master Seite die folgende Methode hinzu:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Wenn die `GridMessageText`-Eigenschaft und `RefreshRecentProductsGrid`-Methode vorhanden sind, kann jede Inhaltsseite den Wert der `Text` Eigenschaft `GridMessage` Bezeichnung Programm gesteuert festlegen oder lesen oder die Daten erneut an die `RecentProducts` GridView binden. In Schritt 4 wird der Zugriff auf die öffentlichen Eigenschaften und Methoden der Master Seite über eine Inhaltsseite untersucht.

> [!NOTE]
> Vergessen Sie nicht, die Eigenschaften und Methoden der Master Seite als `Public`zu markieren. Wenn Sie diese Eigenschaften und Methoden nicht explizit als `Public`bezeichnen, kann auf Sie nicht über die Inhaltsseite zugegriffen werden.

## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Schritt 4: Aufrufen der öffentlichen Member der Master Seite über eine Inhaltsseite

Nun, da die Master Seite über die erforderlichen öffentlichen Eigenschaften und Methoden verfügt, sind wir bereit, diese Eigenschaften und Methoden auf der Seite "`AddProduct.aspx` Inhalt" aufzurufen. Insbesondere müssen wir die `GridMessageText`-Eigenschaft der Master Seite festlegen und ihre `RefreshRecentProductsGrid`-Methode aufzurufen, nachdem das neue Produkt der Datenbank hinzugefügt wurde. Alle ASP.NET Data Web-Steuerelemente lösen Ereignisse unmittelbar vor und nach dem Durchführen verschiedener Aufgaben aus, die es Seiten Entwicklern erleichtern, eine programmgesteuerte Aktion entweder vor oder nach der Aufgabe auszuführen. Wenn z. b. der Endbenutzer auf die Schaltfläche Einfügen der DetailsView klickt, löst die DetailsView bei Postback das `ItemInserting`-Ereignis aus, bevor der Einfügungs Workflow gestartet wird. Anschließend wird der Datensatz in die Datenbank eingefügt. Danach löst die DetailsView das `ItemInserted`-Ereignis aus. Um mit der Master Seite zu arbeiten, nachdem das neue Produkt hinzugefügt wurde, erstellen Sie daher einen Ereignishandler für das `ItemInserted`-Ereignis der DetailsView.

Es gibt zwei Möglichkeiten, wie eine Inhaltsseite Programm gesteuert mit der Master Seite in eine Schnittstelle gehen kann:

- Verwenden der `Page.Master`-Eigenschaft, die einen lose typisierten Verweis auf die Master Seite zurückgibt, oder
- Geben Sie den Master Seitentyp der Seite oder den Dateipfad über eine `@MasterType` Direktive an. Dadurch wird der Seite mit dem Namen `Master`automatisch eine stark typisierte Eigenschaft hinzugefügt.

Sehen wir uns beide Ansätze genauer an.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Verwenden der lose typisierten`Page.Master`-Eigenschaft

Alle ASP.NET Webseiten müssen von der `Page`-Klasse abgeleitet werden, die sich im `System.Web.UI`-Namespace befindet. Die `Page`-Klasse enthält eine [`Master`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) , die einen Verweis auf die Master Seite der Seite zurückgibt. Wenn die Seite keine Master Seite hat `Master` gibt `Nothing`zurück.

Die `Master`-Eigenschaft gibt ein Objekt vom Typ " [`MasterPage`](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) " zurück (auch im `System.Web.UI` Namespace). Dies ist der Basistyp, von dem alle Masterseiten abgeleitet werden. Damit öffentliche Eigenschaften oder Methoden verwendet werden können, die auf der Master Seite unserer Website definiert sind, müssen wir das `MasterPage` Objekt, das von der `Master`-Eigenschaft zurückgegeben wird, in den entsprechenden Typ umwandeln. Da wir unsere Masterseiten Datei `Site.master`benannt haben, wurde die Code-Behind-Klasse `Site`benannt. Daher wandelt der folgende Code die `Page.Master`-Eigenschaft in eine Instanz der `Site`-Klasse um.

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Nachdem wir nun die lose typisierte `Page.Master` Eigenschaft in den Standorttyp eingefügt haben, können wir auf die Eigenschaften und Methoden verweisen, die für den Standort spezifisch sind. Wie in Abbildung 7 gezeigt, wird die öffentliche Eigenschaften `GridMessageText` in der IntelliSense-Dropdown-Anzeige angezeigt.

[![IntelliSense zeigt die öffentlichen Eigenschaften und Methoden unserer Master Seite an.](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Abbildung 07**: IntelliSense zeigt die öffentlichen Eigenschaften und Methoden unserer Master Seite[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))

> [!NOTE]
> Wenn Sie die Masterseiten Datei `MasterPage.master` haben, wird der Code Behind-Klassenname der Master Seite `MasterPage`. Dies kann zu mehrdeutigem Code führen, wenn die Umwandlung vom Typ `System.Web.UI.MasterPage` in Ihre `MasterPage` Klasse durchgeführt wird. Kurz gesagt, Sie müssen den Typ, in den Sie umgewandelt werden, vollständig qualifizieren, was bei der Verwendung des Website-Projekt Modells etwas kompliziert sein kann. Ich möchte entweder sicherstellen, dass Sie beim Erstellen der Master Seite einen anderen Namen als `MasterPage.master` haben oder einen stark typisierten Verweis auf die Master Seite erstellen.

### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Erstellen eines stark typisierten Verweises mit der`@MasterType`-Direktive

Wenn Sie sich genau ansehen, sehen Sie, dass die Code-Behind-Klasse einer ASP.NET-Seite eine partielle Klasse ist (Beachten Sie das `Partial`-Schlüsselwort in der Klassendefinition). Partielle Klassen wurden in C# und Visual Basic with.NET Framework 2,0 eingeführt und ermöglichen es, den Membern einer Klasse in einer kurzen Zahl über mehrere Dateien hinweg zu definieren. Die Code-Behind-Klasse File-`AddProduct.aspx.vb`z. b. enthält den Code, den wir, den Seiten Entwickler, erstellen. Zusätzlich zu unserem Code erstellt die ASP.net-Engine automatisch eine separate Klassendatei mit Eigenschaften und Ereignis Handlern in, die das deklarative Markup in die Klassenhierarchie der Seite übersetzen.

Die automatische Codegenerierung, die jedes Mal auftritt, wenn eine ASP.NET-Seite besucht wird, ist für einige sehr interessante und nützliche Möglichkeiten geeignet. Wenn wir bei Masterseiten der ASP.net-Engine mitteilen, welche Master Seite von unserer Inhaltsseite verwendet wird, generiert Sie eine stark typisierte `Master`-Eigenschaft für uns.

Verwenden Sie die [`@MasterType`-Direktive](https://msdn.microsoft.com/library/ms228274.aspx) , um die ASP.net-Engine über den Master Seitentyp der Inhaltsseite zu informieren. Die `@MasterType`-Direktive kann entweder den Typnamen der Master Seite oder den zugehörigen Dateipfad akzeptieren. Um anzugeben, dass die `AddProduct.aspx` Seite `Site.master` als Master Seite verwendet, fügen Sie die folgende Direktive am Anfang der `AddProduct.aspx`hinzu:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Diese Direktive weist die ASP.net-Engine an, einen stark typisierten Verweis auf die Master Seite über eine Eigenschaft mit dem Namen `Master`hinzuzufügen. Wenn die `@MasterType`-Direktive vorhanden ist, können die öffentlichen Eigenschaften und Methoden der `Site.master` Master Seite direkt über die `Master`-Eigenschaft aufgerufen werden, ohne dass Umwandlungen vorhanden sind.

> [!NOTE]
> Wenn Sie die `@MasterType`-Direktive weglassen, wird die Syntax `Page.Master` und `Master` dasselbe Ergebnis zurückgeben: ein lose typisiertes Objekt zur Master Seite der Seite. Wenn Sie die `@MasterType`-Direktive einschließen, gibt `Master` einen stark typisierten Verweis auf die angegebene Master Seite zurück. `Page.Master`gibt jedoch immer noch einen locker-typisierten Verweis zurück. Eine ausführlichere Betrachtung der Gründe, warum dies der Fall ist und wie die `Master`-Eigenschaft erstellt wird, wenn die `@MasterType`-Direktive enthalten ist, finden Sie im Blogeintrag von [K. Scott allen](http://odetocode.com/blogs/scott/default.aspx) [`@MasterType` in ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).

### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aktualisieren der Master Seite nach dem Hinzufügen eines neuen Produkts

Nachdem wir nun wissen, wie die öffentlichen Eigenschaften und Methoden einer Master Seite auf einer Inhaltsseite aufgerufen werden, können wir die `AddProduct.aspx` Seite aktualisieren, sodass die Master Seite nach dem Hinzufügen eines neuen Produkts aktualisiert wird. Zu Beginn von Schritt 4 haben wir einen Ereignishandler für das `ItemInserting`-Ereignis des DetailsView-Steuer Elements erstellt, das unmittelbar nach dem Hinzufügen des neuen Produkts zur Datenbank ausgeführt wird. Fügen Sie dem Ereignishandler folgenden Code hinzu:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

Der obige Code verwendet sowohl die lose typisierte `Page.Master`-Eigenschaft als auch die stark typisierte `Master`-Eigenschaft. Beachten Sie, dass die `GridMessageText`-Eigenschaft auf "*ProductName* wurde dem Raster hinzugefügt..." festgelegt ist. Die Werte des soeben hinzugefügten Produkts sind über die `e.Values` Auflistung verfügbar. wie Sie sehen können, erfolgt der Zugriff auf den soeben hinzugefügten `ProductName` Wert über `e.Values("ProductName")`.

Abbildung 8 zeigt die `AddProduct.aspx` Seite unmittelbar nach dem Hinzufügen eines neuen Produkts, das der-Datenbank hinzugefügt wurde. Beachten Sie, dass der soeben hinzugefügte Produktname in der Bezeichnung der Master Seite vermerkt ist und dass die GridView aktualisiert wurde, um das Produkt und den Preis einzuschließen.

[![die Bezeichnung und die GridView der Master Seite das soeben hinzugefügte Produkt](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Abbildung 08**: die Bezeichnung und die GridView der Master Seite zeigen das soeben hinzugefügte Produkt an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))

## <a name="summary"></a>Summary

Idealerweise sind eine Master Seite und ihre Inhaltsseiten vollständig voneinander getrennt und erfordern keine Interaktions Ebene. Während Masterseiten und Inhaltsseiten mit diesem Ziel entworfen werden sollten, gibt es eine Reihe allgemeiner Szenarien, in denen eine Inhaltsseite mit der Master Seite in eine Schnittstelle gesetzt werden muss. Einer der häufigsten Gründe ist das Aktualisieren eines bestimmten Teils der Masterseiten Anzeige auf der Grundlage einer Aktion, die auf der Inhaltsseite aufgetreten ist.

Die gute Nachricht ist, dass es relativ einfach ist, eine Inhaltsseite Programm gesteuert mit der Master Seite zu interagieren. Beginnen Sie, indem Sie auf der Master Seite öffentliche Eigenschaften oder Methoden erstellen, die die Funktionalität Kapseln, die von einer Inhaltsseite aufgerufen werden muss. Greifen Sie dann auf der Seite Inhalt über die lose typisierte `Page.Master`-Eigenschaft auf die Eigenschaften und Methoden der Master Seite zu, oder verwenden Sie die `@MasterType`-Direktive, um einen stark typisierten Verweis auf die Master Seite zu erstellen.

Im nächsten Tutorial wird erläutert, wie die Master Seite Programm gesteuert mit einer ihrer Inhaltsseiten interagieren kann.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.net](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.net Master Seiten: Tipps, Tricks und Traps](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` in ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Übergeben von Informationen zwischen Inhalt und Master Seiten](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Arbeiten mit Daten in ASP.net-Tutorials](../../data-access/index.md)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Prüfer für dieses Tutorial war Zack Jones. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](control-id-naming-in-content-pages-vb.md)
> [Weiter](interacting-with-the-content-page-from-the-master-page-vb.md)
