---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Interaktion mit der Inhaltsseite von der Master Seite (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Hier wird untersucht, wie Methoden aufgerufen, Eigenschaften usw. auf der Inhaltsseite aus Code auf der Master Seite festgelegt werden.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 2cf57665aa584285351d874267949d61db69c7fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78501165"
---
# <a name="interacting-with-the-content-page-from-the-master-page-c"></a>Interaktion mit der Inhaltsseite von der Masterseite (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Hier wird untersucht, wie Methoden aufgerufen, Eigenschaften usw. auf der Inhaltsseite aus Code auf der Master Seite festgelegt werden.

## <a name="introduction"></a>Einführung

Im vorangehenden Tutorial wurde erläutert, wie die Inhaltsseite Programm gesteuert mit der Master Seite interagieren kann. Beachten Sie, dass die Master Seite aktualisiert wurde und ein GridView-Steuerelement enthält, in dem die fünf zuletzt hinzugefügten Produkte aufgeführt sind. Anschließend haben wir eine Inhaltsseite erstellt, von der aus der Benutzer ein neues Produkt hinzufügen konnte. Wenn Sie ein neues Produkt hinzufügen, ist die Inhaltsseite erforderlich, um die Master Seite anzuweisen, ihre GridView so zu aktualisieren, dass Sie das soeben hinzugefügte Produkt enthält. Diese Funktionalität wurde erreicht, indem der Master Seite eine öffentliche Methode hinzugefügt wurde, die die an die GridView gebundene Daten aktualisiert und diese Methode dann von der Inhaltsseite aufgerufen hat.

Die häufigste Form von Inhalten und die Interaktion der Master Seite stammt aus der Inhaltsseite. Es ist jedoch möglich, dass die Master Seite die aktuelle Inhaltsseite in Aktion anstellt. diese Funktionalität ist möglicherweise erforderlich, wenn die Master Seite Benutzeroberflächen Elemente enthält, die es Benutzern ermöglichen, Daten zu ändern, die auch auf der Inhaltsseite angezeigt werden. Betrachten Sie eine Inhaltsseite, auf der die Produktinformationen in einem GridView-Steuerelement und eine Master Seite mit einem Schaltflächen-Steuerelement angezeigt werden, das, wenn Sie darauf klicken, die Preise aller Produkte verdoppelt. Ähnlich wie im Beispiel im vorherigen Tutorial muss die GridView nach dem Klicken auf die Schaltfläche mit dem doppelten Preis aktualisiert werden, sodass die neuen Preise angezeigt werden. in diesem Szenario ist es jedoch die Master Seite, die die Inhaltsseite in eine Aktion hinein stellen muss.

In diesem Tutorial wird erläutert, wie die Funktion zum Aufrufen von Masterseiten auf der Seite Inhalt definiert wird.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Initigating der programmgesteuerten Interaktion über ein Ereignis und Ereignishandler

Das Aufrufen der Funktionalität von Inhaltsseiten von einer Master Seite aus ist schwieriger als die andere Methode. Da eine Inhaltsseite über eine einzige Master Seite verfügt, wissen wir beim Initiieren der programmatischen Interaktion von der Inhaltsseite, welche öffentlichen Methoden und Eigenschaften zur Verfügung stehen. Eine Master Seite kann jedoch viele verschiedene Inhaltsseiten aufweisen, die jeweils über einen eigenen Satz von Eigenschaften und Methoden verfügen. Wie können wir dann auf der Master Seite Code schreiben, um eine Aktion auf der Inhaltsseite auszuführen, wenn wir nicht wissen, welche Inhaltsseite bis zur Laufzeit aufgerufen wird?

Angenommen, ein ASP.net-websteuer Element, z. b. das Button-Steuerelement Ein Schaltflächen-Steuerelement kann auf einer beliebigen Anzahl von ASP.NET Seiten angezeigt werden und benötigt einen Mechanismus, mit dem die Seite gewarnt werden kann, auf die geklickt wurde. Dies wird mithilfe von *Ereignissen*erreicht. Insbesondere löst das Schaltflächen-Steuerelement das `Click`-Ereignis aus, wenn darauf geklickt wird. die Seite ASP.net, die die Schaltfläche enthält, kann optional über einen *Ereignishandler*auf diese Benachrichtigung reagieren.

Das gleiche Muster kann verwendet werden, um eine Masterseiten-Auslöserfunktion auf den Inhaltsseiten zu haben:

1. Fügen Sie der Master Seite ein Ereignis hinzu.
2. Rufen Sie das-Ereignis immer dann auf, wenn die Master Seite mit der zugehörigen Inhaltsseite kommunizieren muss. Wenn die Master Seite z. b. die Inhaltsseite warnen muss, dass der Benutzer die Preise verdoppelt hat, wird das zugehörige Ereignis sofort ausgelöst, nachdem die Preise verdoppelt wurden.
3. Erstellen Sie einen Ereignishandler auf den Inhaltsseiten, die einige Aktionen ausführen müssen.

In diesem Rest dieses Tutorials wird das Beispiel implementiert, das in der Einführung beschrieben wird. Dabei handelt es sich um eine Inhaltsseite, die die Produkte in der Datenbank und eine Master Seite auflistet, die ein Schaltflächen-Steuerelement zum verdoppeln der Preise enthält.

## <a name="step-1-displaying-products-in-a-content-page"></a>Schritt 1: Anzeigen von Produkten auf einer Inhaltsseite

Unsere erste Geschäftsordnung besteht darin, eine Inhaltsseite zu erstellen, die die Produkte aus der Northwind-Datenbank auflistet. (Im vorherigen Tutorial haben wir die Datenbank Northwind zum Projekt hinzugefügt, die [*mit der Master Seite von der Seite Inhalt interagiert*](interacting-with-the-master-page-from-the-content-page-cs.md).) Fügen Sie zunächst eine neue ASP.NET-Seite zum Ordner "`~/Admin`" mit dem Namen `Products.aspx`hinzu, und stellen Sie sicher, dass Sie an die `Site.master` Master Seite gebunden ist. Abbildung 1 zeigt die Projektmappen-Explorer, nachdem diese Seite der Website hinzugefügt wurde.

[![dem Administrator Ordner eine neue ASP.NET-Seite hinzufügen](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen einer neuen ASP.NET-Seite zum Ordner "`Admin`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))

Beachten Sie, dass im Tutorial [*angeben des Titels, der Meta-Tags und anderer HTML-Header im Lernprogramm für die Master Seite*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) eine benutzerdefinierte Basis Seiten Klasse mit dem Namen `BasePage` erstellt wurde, die den Titel der Seite generiert, wenn Sie nicht explizit festgelegt ist. Wechseln Sie zur Code Behind-Klasse der `Products.aspx` Seite, und lassen Sie Sie von `BasePage` (anstelle von `System.Web.UI.Page`) ableiten.

Aktualisieren Sie abschließend die `Web.sitemap` Datei, sodass Sie einen Eintrag für diese Lektion enthält. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für die Interaktions Lektion Inhalt in Master Seite hinzu:

[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

Das Hinzufügen dieses `<siteMapNode>` Elements wird in der Liste der Lektionen widergespiegelt (siehe Abbildung 5).

Kehren Sie zu `Products.aspx`zurück. Fügen Sie im Inhalts Steuerelement für `MainContent`ein GridView-Steuerelement hinzu, und benennen Sie es `ProductsGrid`. Binden Sie die GridView an ein neues SqlDataSource-Steuerelement mit dem Namen `ProductsDataSource`.

[![das GridView-Steuerelement an ein neues SqlDataSource-Steuerelement binden.](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Abbildung 02**: Binden der GridView an ein neues SqlDataSource-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))

Konfigurieren Sie den Assistenten so, dass er die Datenbank Northwind verwendet. Wenn Sie das vorherige Tutorial durchgearbeitet haben, sollten Sie bereits eine Verbindungs Zeichenfolge mit dem Namen `NorthwindConnectionString` in `Web.config`haben. Wählen Sie diese Verbindungs Zeichenfolge aus der Dropdown Liste aus, wie in Abbildung 3 gezeigt.

[![Konfigurieren von SqlDataSource für die Verwendung der Northwind-Datenbank](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Abbildung 03**: Konfigurieren von SqlDataSource für die Verwendung der Northwind-Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))

Geben Sie als nächstes die `SELECT`-Anweisung des Datenquellen-Steuer Elements an, indem Sie die Products-Tabelle aus der Dropdown Liste auswählen und die Spalten `ProductName` und `UnitPrice` zurückgeben (siehe Abbildung 4). Klicken Sie auf Weiter, und beenden Sie dann den Assistenten zum Konfigurieren von Datenquellen.

[![die Felder "ProductName" und "UnitPrice" aus der Tabelle "Products" zurückgeben](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Abbildung 04**: Zurückgeben der Felder "`ProductName`" und "`UnitPrice`" aus der `Products` Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))

Das ist schon alles! Nachdem Sie den Assistenten abgeschlossen haben, fügt Visual Studio zwei boundfields zur GridView hinzu, um die beiden Felder zu spiegeln, die vom SqlDataSource-Steuerelement zurückgegeben werden. Das Markup View-und SqlDataSource-Steuerelement-Steuerelement wird befolgt. Abbildung 5 zeigt die Ergebnisse, wenn Sie in einem Browser angezeigt werden.

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]

[![jedes Produkt und sein Preis in der GridView aufgeführt.](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Abbildung 05**: jedes Produkt und sein Preis sind in der GridView aufgeführt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))

> [!NOTE]
> Sie können die Darstellung der GridView jederzeit bereinigen. Einige Vorschläge umfassen das Formatieren des angezeigten UnitPrice-Werts als Währung und das Verwenden von Hintergrundfarben und Schriftarten, um die Darstellung des Rasters zu verbessern. Weitere Informationen zum Anzeigen und Formatieren von Daten in ASP.net finden Sie in der [Reihe zu arbeiten mit Daten](../../data-access/index.md).

## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Schritt 2: Hinzufügen der Schaltfläche "doppelte Preise" zur Master Seite

Die nächste Aufgabe besteht darin, der Master Seite ein Schaltflächen-websteuer Element hinzuzufügen, das, wenn Sie darauf klicken, den Preis aller Produkte in der Datenbank verdoppelt. Öffnen Sie die `Site.master` Master Seite, und ziehen Sie eine Schaltfläche aus der Toolbox auf den Designer, und platzieren Sie Sie unterhalb des `RecentProductsDataSource` SqlDataSource-Steuer Elements, das wir im vorherigen Tutorial hinzugefügt haben. Legen Sie die `ID`-Eigenschaft der Schaltfläche auf `DoublePrice` und deren `Text`-Eigenschaft auf "doppelte Produktpreise" fest.

Fügen Sie als nächstes der Master Seite ein SqlDataSource-Steuerelement hinzu, und benennen Sie es `DoublePricesDataSource`. Mit dieser SqlDataSource wird die `UPDATE`-Anweisung ausgeführt, um alle Preise zu verdoppeln. Insbesondere müssen die `ConnectionString`-und `UpdateCommand`-Eigenschaften auf die entsprechende Verbindungs Zeichenfolge und `UPDATE`-Anweisung festgelegt werden. Dann muss die `Update` Methode dieses SqlDataSource-Steuer Elements aufgerufen werden, wenn auf die Schaltfläche `DoublePrice` geklickt wird. Wählen Sie zum Festlegen der Eigenschaften für `ConnectionString` und `UpdateCommand` das SqlDataSource-Steuerelement aus, und wechseln Sie dann zum Eigenschaftenfenster. Die `ConnectionString`-Eigenschaft listet die Verbindungs Zeichenfolgen auf, die bereits in `Web.config` in einer Dropdown Liste gespeichert wurden. Wählen Sie die Option `NorthwindConnectionString` aus, wie in Abbildung 6 dargestellt.

[![konfigurieren Sie SqlDataSource für die Verwendung von NorthwindConnectionString.](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Abbildung 06**: Konfigurieren von SqlDataSource für die Verwendung des `NorthwindConnectionString` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))

Um die `UpdateCommand`-Eigenschaft festzulegen, suchen Sie die Option UpdateQuery in der Eigenschaftenfenster. Wenn diese Eigenschaft ausgewählt ist, wird eine Schaltfläche mit Auslassungs Zeichen angezeigt. Klicken Sie auf diese Schaltfläche, um das Dialogfeld Befehls-und Parameter-Editor in Abbildung 7 anzuzeigen. Geben Sie die folgende `UPDATE`-Anweisung in das Textfeld des Dialog Felds ein:

[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Diese Anweisung verdoppelt bei der Ausführung den `UnitPrice` Wert für jeden Datensatz in der `Products` Tabelle.

[![Festlegen der UpdateCommand-Eigenschaft von SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Abbildung 07**: Festlegen der `UpdateCommand` Eigenschaft von SqlDataSource ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))

Nachdem Sie diese Eigenschaften festgelegt haben, sollten das deklarative Markup Ihrer Schaltfläche und des SqlDataSource-Steuer Elements in etwa wie folgt aussehen:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Nur noch muss beim Klicken auf die Schaltfläche `DoublePrice` die `Update` Methode aufgerufen werden. Erstellen Sie einen `Click`-Ereignishandler für die Schaltfläche `DoublePrice`, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Um diese Funktionalität zu testen, besuchen Sie die `~/Admin/Products.aspx` Seite, die Sie in Schritt 1 erstellt haben, und klicken Sie auf die Schaltfläche "doppelte Produktpreise". Wenn Sie auf die Schaltfläche klicken, wird ein Postback ausgelöst und der `Click` Ereignishandler der `DoublePrice` Schaltfläche ausgeführt, wodurch die Preise aller Produkte verdoppelt werden. Die Seite wird dann erneut gerendert, und das Markup wird zurückgegeben und im Browser erneut angezeigt. Die GridView auf der Seite Inhalt listet jedoch dieselben Preise auf wie vor dem Klicken auf die Schaltfläche "doppelte Produktpreise". Dies liegt daran, dass der Zustand der ursprünglich in der GridView geladenen Daten im Ansichts Zustand gespeichert wurde, sodass er bei Postbacks nicht erneut geladen wird, es sei denn, er wird anderweitig angewiesen. Wenn Sie eine andere Seite besuchen und dann zur `~/Admin/Products.aspx` Seite zurückkehren, werden die aktualisierten Preise angezeigt.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Schritt 3: erhöhen eines Ereignisses, wenn die Preise verdoppelt werden

Da die GridView auf der `~/Admin/Products.aspx` Seite die Preis Verdoppelung nicht sofort widerspiegelt, kann ein Benutzer verständlicherweise denken, dass er nicht auf die Schaltfläche "doppelte Produktpreise" geklickt hat oder dass er nicht funktioniert hat. Sie können versuchen, mehrmals auf die Schaltfläche zu klicken, um die Preise nochmals zu verdoppeln. Um dieses Problem zu beheben, muss das Raster auf der Inhaltsseite die neuen Preise sofort nach der doppelten Anzeige anzeigen.

Wie bereits weiter oben in diesem Tutorial erläutert, muss ein Ereignis auf der Master Seite immer dann durchgeführt werden, wenn der Benutzer auf die Schaltfläche `DoublePrice` klickt. Ein Ereignis ist eine Möglichkeit für eine Klasse (einen Ereignis Verleger), eine andere Gruppe von anderen Klassen (den Ereignis Abonnenten) darüber zu benachrichtigen, dass etwas interessantes aufgetreten ist. In diesem Beispiel ist die Master Seite der Ereignis Herausgeber. die Inhaltsseiten, die beim Klicken auf die Schaltfläche "`DoublePrice`" berücksichtigt werden, sind die Abonnenten.

Eine Klasse abonniert ein Ereignis, indem ein *Ereignishandler*erstellt wird, bei dem es sich um eine Methode handelt, die als Reaktion auf das ausgelöbene Ereignis ausgeführt wird. Der Verleger definiert die Ereignisse, die er auslöst, indem er einen *Ereignis*Delegaten definiert. Der Ereignis Delegat gibt an, welche Eingabeparameter der Ereignishandler annehmen muss. In der .NET Framework geben Ereignis Delegaten keinen Wert zurück und akzeptieren zwei Eingabeparameter:

- Ein-`Object`, der die Ereignis Quelle identifiziert, und
- Eine von abgeleitete Klasse `System.EventArgs`

Der zweite Parameter, der an einen Ereignishandler übergeben wird, kann zusätzliche Informationen über das Ereignis enthalten. Während die Basis `EventArgs` Klasse keine Informationen übergibt, enthält die .NET Framework eine Reihe von Klassen, die `EventArgs` erweitern und zusätzliche Eigenschaften umfassen. Beispielsweise wird eine `CommandEventArgs` Instanz an Ereignishandler übermittelt, die auf das `Command`-Ereignis reagieren, und enthält zwei Informations Eigenschaften: `CommandArgument` und `CommandName`.

> [!NOTE]
> Weitere Informationen zum Erstellen, erhöhen und behandeln von Ereignissen finden Sie unter [Ereignisse und](https://msdn.microsoft.com/library/17sde2xt.aspx) Delegaten und Ereignis Delegaten [in einfachem Englisch](http://www.codeproject.com/KB/cs/eventdelegates.aspx).

Verwenden Sie zum Definieren eines Ereignisses die folgende Syntax:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Da wir nur die Inhaltsseite warnen müssen, wenn der Benutzer auf die Schaltfläche "`DoublePrice`" geklickt hat und keine weiteren zusätzlichen Informationen übergeben müssen, können wir den Ereignis Delegaten `EventHandler`verwenden, der einen Ereignishandler definiert, der als zweiten Parameter ein Objekt vom Typ `System.EventArgs`akzeptiert. Um das Ereignis auf der Master Seite zu erstellen, fügen Sie der Code Behind-Klasse der Master Seite die folgende Codezeile hinzu:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Der obige Code fügt der Master Seite mit dem Namen `PricesDoubled`ein öffentliches Ereignis hinzu. Wir müssen dieses Ereignis jetzt erhöhen, nachdem die Preise verdoppelt wurden. Verwenden Sie die folgende Syntax, um ein Ereignis zu verwenden:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Dabei sind *Absender* und *EventArgs* die Werte, die Sie an den Ereignishandler des Abonnenten übergeben möchten.

Aktualisieren Sie den `DoublePrice` `Click`-Ereignishandler mit folgendem Code:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Wie zuvor wird der `Click` Ereignishandler gestartet, indem die `Update` Methode des `DoublePricesDataSource` SqlDataSource-Steuer Elements aufgerufen wird, um die Preise aller Produkte zu verdoppeln. Danach gibt es zwei Ergänzungen zum-Ereignishandler. Zuerst werden die Daten der `RecentProducts` GridView aktualisiert. Diese GridView wurde der Master Seite im vorherigen Tutorial hinzugefügt und zeigt die fünf zuletzt hinzugefügten Produkte an. Wir müssen dieses Raster aktualisieren, damit es die eben doppelten Preise für diese fünf Produkte anzeigt. Danach wird das `PricesDoubled`-Ereignis ausgelöst. Ein Verweis auf die Master Seite selbst (`this`) wird als Ereignis Quelle an den Ereignishandler gesendet, und es wird ein leeres `EventArgs` Objekt als Ereignis Argumente gesendet.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Schritt 4: Behandeln des Ereignisses auf der Seite "Inhalt"

An diesem Punkt löst die Master Seite das `PricesDoubled`-Ereignis aus, wenn auf das `DoublePrice` Schaltflächen-Steuerelement geklickt wird. Dies ist jedoch nur die Hälfte des Kampfes. Wir müssen das Ereignis trotzdem auf dem Abonnenten behandeln. Dies umfasst zwei Schritte: Erstellen des Ereignis Handlers und Hinzufügen von Ereignishandlercode, sodass der Ereignishandler ausgeführt wird, wenn das Ereignis ausgelöst wird.

Erstellen Sie zunächst einen Ereignishandler mit dem Namen `Master_PricesDoubled`. Da wir das `PricesDoubled`-Ereignis auf der Master Seite definiert haben, müssen die beiden Eingabeparameter des Ereignis Handlers vom Typ `Object` bzw. `EventArgs`sein. Im-Ereignishandler wird die `DataBind`-Methode der `ProductsGrid` GridView aufgerufen, um die Daten erneut an das Raster zu binden.

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Der Code für den Ereignishandler ist fertiggestellt, aber wir haben noch das `PricesDoubled` Ereignis der Master Seite an diesen Ereignishandler übertragen. Der Abonnent verdrahtet ein Ereignis mithilfe der folgenden Syntax an einen Ereignishandler:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*Publisher* ist ein Verweis auf das Objekt, das das Ereignis *EventName*und *MethodName* den Namen des Ereignis Handlers enthält, der auf dem Abonnenten definiert ist, dessen Signatur *dem Ereignis Delegaten entspricht.* Anders ausgedrückt: Wenn der Ereignis Delegat `EventHandler`ist, muss *MethodName* der Name einer Methode im Abonnenten sein, die keinen Wert zurückgibt und zwei Eingabeparameter vom Typ "`Object`" bzw. "`EventArgs`" annimmt.

Dieser Ereignis Verdrahtungs Code muss auf dem ersten Seitenbesuch und nachfolgenden Postbacks ausgeführt werden und an einem Punkt im Seiten Lebenszyklus auftreten, der vor dem auslassen des Ereignisses liegt. Ein guter Zeitpunkt zum Hinzufügen von Ereignishandlercode befindet sich in der PreInit-Phase, die sehr früh im Lebenszyklus der Seite auftritt.

Öffnen Sie `~/Admin/Products.aspx`, und erstellen Sie einen `Page_PreInit`-Ereignishandler:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Zum vervollständigen dieses Codierungs Codes benötigen Sie einen programmgesteuerten Verweis auf die Master Seite von der Inhaltsseite. Wie bereits im vorherigen Tutorial erwähnt, gibt es zwei Möglichkeiten:

- Durch Umwandeln der lose typisierten `Page.Master` Eigenschaft in den entsprechenden Masterseitentyp oder
- Durch Hinzufügen einer `@MasterType`-Direktive auf der `.aspx` Seite und anschließendes Verwenden der stark typisierten `Master`-Eigenschaft.

Wir verwenden den letzteren Ansatz. Fügen Sie am Anfang des deklarativen Markups der Seite die folgende `@MasterType`-Direktive hinzu:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Fügen Sie dann dem Ereignishandler für `Page_PreInit` den folgenden Ereignis Code hinzu:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Wenn dieser Code vorhanden ist, wird die GridView auf der Inhaltsseite immer dann aktualisiert, wenn auf die Schaltfläche "`DoublePrice`" geklickt wird.

Die Abbildungen 8 und 9 veranschaulichen dieses Verhalten. Abbildung 8 zeigt die Seite beim ersten Besuch. Beachten Sie, dass die preiswerte sowohl in der `RecentProducts` GridView (in der linken Spalte der Master Seite) als auch in der `ProductsGrid` GridView (auf der Seite Inhalt) angezeigt werden. Abbildung 9 zeigt den gleichen Bildschirm unmittelbar nach dem Klicken auf die Schaltfläche "`DoublePrice`". Wie Sie sehen können, werden die neuen Preise sofort in beiden GridViews reflektiert.

[anfängliche preiswerte ![](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Abbildung 08**: anfängliche preiswerte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))

[in den GridViews werden ![die soeben doppelten Preise angezeigt.](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Abbildung 09**: die Preisübersicht werden in den GridViews angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))

## <a name="summary"></a>Zusammenfassung

Idealerweise sind eine Master Seite und ihre Inhaltsseiten vollständig voneinander getrennt und erfordern keine Interaktions Ebene. Wenn Sie jedoch eine Master Seite oder Inhaltsseite haben, auf der Daten angezeigt werden, die auf der Master Seite oder der Inhaltsseite geändert werden können, müssen Sie möglicherweise auf der Master Seite die Inhaltsseite warnen (oder umgekehrt), wenn die Daten geändert werden, sodass die Anzeige aktualisiert werden kann. Im vorherigen Tutorial wurde erläutert, wie eine Inhaltsseite Programm gesteuert mit der Master Seite interagieren kann. in diesem Tutorial haben wir erläutert, wie eine Master Seite die Interaktion initiiert.

Während die programmgesteuerte Interaktion zwischen einem Inhalt und einer Master Seite entweder aus der Inhalts-oder der Master Seite stammen kann, hängt das verwendete Interaktionsmuster vom Ursprung ab. Die Unterschiede sind darauf zurückzuführen, dass eine Inhaltsseite eine einzelne Master Seite hat, aber eine Master Seite kann viele verschiedene Inhaltsseiten enthalten. Anstatt eine Master Seite direkt mit einer Inhaltsseite zu interagieren, empfiehlt es sich, dass die Master Seite ein Ereignis auswirft, um zu signalisieren, dass eine Aktion stattfindet. Die Inhaltsseiten, die für die Aktion wichtig sind, können Ereignishandler erstellen.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.net](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Ereignisse und Delegaten](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Übergeben von Informationen zwischen Inhalt und Master Seiten](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Arbeiten mit Daten in ASP.net-Tutorials](../../data-access/index.md)

### <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Suchi Banerjee. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](interacting-with-the-master-page-from-the-content-page-cs.md)
> [Weiter](master-pages-and-asp-net-ajax-cs.md)
