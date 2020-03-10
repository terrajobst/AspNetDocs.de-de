---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Teil 7: Hinzufügen von Features | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 7 fügt zusätzliche Funktionen hinzu, wie z. b. Konto Revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521565"
---
# <a name="part-7-adding-features"></a>Teil 7: Hinzufügen von Features

von [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen. Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 7 bietet zusätzliche Features, wie z. b. Kontoüberprüfung, Produkt Reviews und "beliebte Elemente" und "auch erworbene" Benutzer Steuerelemente.

## <a id="_Toc260221673"></a>Hinzufügen von Features

Obwohl Benutzer unseren Katalog durchsuchen, Elemente in Ihren Warenkorb platzieren und den Checkout Vorgang durchführen können, gibt es eine Reihe von unterstützenden Features, die wir zur Verbesserung unserer Website berücksichtigen werden.

1. Kontoüberprüfung (Auflisten von Aufträgen und Anzeigen von Details.)
2. Fügen Sie der Vorderseite kontextspezifischen Inhalt hinzu.
3. Fügen Sie eine Funktion hinzu, mit der Benutzer die Produkte im Katalog überprüfen können.
4. Erstellen Sie ein Benutzer Steuerelement, um beliebte Elemente anzuzeigen und dieses Steuerelement auf der Vorderseite zu platzieren.
5. Erstellen Sie ein "auch erworbenes" Benutzer Steuerelement, und fügen Sie es der Seite "Produktdetails" hinzu.
6. Fügen Sie eine Kontaktseite hinzu.
7. Fügen Sie eine Info-Seite hinzu.
8. Globaler Fehler

## <a id="_Toc260221674"></a>Kontoüberprüfung

Erstellen Sie im Ordner "Account" zwei ASPX-Seiten mit dem Namen "OrderList. aspx" und die andere mit dem Namen "OrderDetails. aspx".

"OrderList. aspx" nutzt die GridView-und EntityDataSource-Steuerelemente ähnlich wie zuvor.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Die EntityDataSource wählt Datensätze aus der Tabelle Orders aus, die nach dem Benutzernamen gefiltert wird (siehe Where Parameter), den wir in einer Sitzungsvariablen festlegen, wenn der Benutzer sich in befindet.

Beachten Sie auch diese Parameter im HyperLinkField der GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Diese geben den Link zur Order Details-Ansicht für jedes Produkt an, das das OrderID-Feld als QueryString-Parameter für die Order Details. aspx-Seite angibt.

## <a id="_Toc260221675"></a>OrderDetails. aspx

Wir verwenden ein EntityDataSource-Steuerelement für den Zugriff auf die Bestellungen und eine FormView zum Anzeigen der Bestelldaten und eine weitere EntityDataSource mit einer GridView, um alle Positionen der Bestellung anzuzeigen.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

In der Code Behind-Datei (OrderDetails.aspx.cs) gibt es zwei wenige Teile der Hausführung.

Zuerst müssen wir sicherstellen, dass Order Details immer eine OrderID erhält.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Wir müssen auch die Bestellsumme aus den Zeilen Elementen berechnen und anzeigen.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>Die Startseite

Fügen wir der Seite Default. aspx einige statische Inhalte hinzu.

Zuerst erstelle ich einen Ordner "Content" (Inhalt) und innerhalb des Ordners "Images" (Ich füge ein Bild ein, das auf der Startseite verwendet werden soll.)

Fügen Sie im unteren Platzhalter der default. aspx-Seite das folgende Markup hinzu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>Produkt Reviews

Zuerst fügen wir eine Schaltfläche mit einem Link zu einem Formular hinzu, das wir verwenden können, um eine Produkt Überprüfung einzugeben.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Beachten Sie, dass die ProductID in der Abfrage Zeichenfolge übergeben wird.

Als Nächstes fügen wir die Seite "reviewadd. aspx" hinzu.

Auf dieser Seite wird das ASP.NET AJAX Control Toolkit verwendet. Wenn Sie dies noch nicht getan haben, können Sie es von [DevExpress](http://devexpress.com/act) herunterladen. es gibt Anleitungen zum Einrichten des Toolkits für die Verwendung mit Visual Studio [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

Ziehen Sie im Entwurfs Modus Steuerelemente und Validierungs Steuerelemente aus der Toolbox, und erstellen Sie ein Formular wie das folgende.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Das Markup sieht in etwa wie folgt aus.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Nachdem wir nun die Überprüfungen eingegeben haben, können diese Überprüfungen auf der Produktseite angezeigt werden.

Fügen Sie dieses Markup der Seite "ProductDetails. aspx" hinzu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Wenn Sie die Anwendung jetzt ausführen und zu einem Produkt navigieren, werden die Produktinformationen einschließlich Kunden Reviews angezeigt.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>Beliebte Elemente-Steuerelement (Erstellen von Benutzer Steuerelementen)

Um den Umsatz auf Ihrer Website zu steigern, fügen wir einige Features zu beliebten oder verwandten Produkten hinzu.

Die erste dieser Features ist eine Liste der beliebtesten Produkte in unserem Produktkatalog.

Wir erstellen ein "Benutzer Steuerelement", um die am besten verkauften Elemente auf der Startseite der Anwendung anzuzeigen. Da dies ein Steuerelement ist, können wir es auf jeder Seite verwenden, indem wir einfach das Steuerelement im Designer von Visual Studio auf eine beliebige Seite ziehen und ablegen.

Klicken Sie im Projektmappen-Explorer von Visual Studio mit der rechten Maustaste auf den Projektmappennamen, und erstellen Sie ein neues Verzeichnis namens "Controls". Dies ist zwar nicht erforderlich, aber wir helfen, das Projekt zu organisieren, indem wir alle Benutzer Steuerelemente im Verzeichnis "Controls" erstellen.

Klicken Sie mit der rechten Maustaste auf den Ordner Steuerelemente, und wählen Sie "Neues Element":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Geben Sie einen Namen für das Steuerelement "popultems" an. Beachten Sie, dass es sich bei der Dateierweiterung für Benutzer Steuerelemente um. ascx not. aspx handelt.

Das Benutzer Steuerelement "beliebte Elemente" wird wie folgt definiert.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Hier verwenden wir eine Methode, die wir noch nicht in dieser Anwendung verwendet haben. Wir verwenden das Repeater-Steuerelement und anstatt ein Datenquellen-Steuerelement zu verwenden, binden wir das Repeater-Steuerelement an die Ergebnisse einer LINQ to Entities Abfrage.

Im Code Behind unseres Steuer Elements gehen wir wie folgt vor.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Beachten Sie auch diese wichtige Zeile am oberen Rand des Markup Elements des Steuer Elements.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Da die am häufigsten verwendeten Elemente nicht auf Minutenbasis geändert werden, können wir eine aching-Direktive hinzufügen, um die Leistung unserer Anwendung zu verbessern. Diese Direktive bewirkt, dass der Steuerelement Code nur ausgeführt wird, wenn die zwischengespeicherte Ausgabe des Steuer Elements abläuft. Andernfalls wird die zwischengespeicherte Version der Ausgabe des Steuer Elements verwendet.

Nun müssen wir nur noch unser neues Steuerelement auf der Seite "default. aspx" einschließen.

Verwenden Sie Drag & Drop, um eine Instanz des Steuer Elements in der Spalte Öffnen des Standardformulars zu platzieren.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Wenn wir nun die Anwendung ausführen, werden auf der Startseite die beliebtesten Elemente angezeigt.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>"Auch gekauft"-Steuerelement (Benutzer Steuerelemente mit Parametern)

Das zweite Benutzer Steuerelement, das wir erstellen, wird durch Hinzufügen der Kontext Genauigkeit den suggestiven Verkauf auf der nächsten Ebene durchführen.

Die Logik zum Berechnen der obersten "auch erworbenen" Elemente ist nicht trivial.

Mit unserem "auch erworbenen" Steuerelement werden die Order Details-Datensätze (zuvor gekauft) für die aktuell ausgewählte ProductID ausgewählt und die OrderIDs für jede eindeutige Bestellung erfasst.

Anschließend wählen wir die Produkte von all diesen Aufträgen aus und fassen die erworbenen Mengen zusammen. Wir sortieren die Produkte nach der Summe und zeigen die ersten fünf Elemente an.

Aufgrund der Komplexität dieser Logik wird dieser Algorithmus als gespeicherte Prozedur implementiert.

Der T-SQL-Wert für die gespeicherte Prozedur lautet wie folgt.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Beachten Sie, dass diese gespeicherte Prozedur (selectpurchasedwithproducts) in der Datenbank vorhanden war, als wir Sie in unsere Anwendung eingefügt haben, und wenn wir das Entity Data Model, das wir angegeben haben, zusätzlich zu den Tabellen und Sichten, die wir benötigten, das Entity Data Model sollte diese gespeicherte Prozedur einschließen.

Um auf die gespeicherte Prozedur vom Entity Data Model aus zuzugreifen, muss die Funktion importiert werden.

Doppelklicken Sie im Projektmappen-Explorer auf die Entity Data Model, um Sie im Designer zu öffnen, und öffnen Sie den Modell Browser, klicken Sie mit der rechten Maustaste in den Designer, und wählen Sie "Funktions Import hinzufügen".

![](tailspin-spyworks-part-7/_static/image1.png)

Dadurch wird dieses Dialogfeld geöffnet.

![](tailspin-spyworks-part-7/_static/image2.png)

Füllen Sie die Felder wie oben angezeigt aus, wählen Sie "selectpurchasedwithproducts" aus, und verwenden Sie den Prozedur Namen als Name der importierten Funktion.

Klicken Sie auf "OK".

Nachdem Sie dies getan haben, können wir einfach mit der gespeicherten Prozedur programmieren, wie es bei jedem anderen Element im Modell möglich ist.

Erstellen Sie daher im Ordner "Steuerelemente" ein neues Benutzer Steuerelement mit dem Namen "alsopurgejagt. ascx".

Das Markup für dieses Steuerelement wird dem popultems-Steuerelement vertraut sein.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Der bedeutende Unterschied besteht darin, dass die Ausgabe nicht zwischenspeichert, da das zu rendernde Element je nach Produkt unterschiedlich ist.

Die ProductID ist eine "Eigenschaft" für das Steuerelement.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Im PreRender-Ereignishandler des Steuer Elements haben wir drei Dinge durchgeführt.

1. Stellen Sie sicher, dass ProductID festgelegt ist.
2. Überprüfen Sie, ob Produkte vorhanden sind, die mit dem aktuellen gekauft wurden.
3. Ausgabe einiger Elemente, wie in #2 festgelegt.

Beachten Sie, wie einfach es ist, die gespeicherte Prozedur über das Modell aufzurufen.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Nachdem Sie festgestellt haben, dass auch "auch gekauft" vorhanden ist, können wir einfach das Repeater an die von der Abfrage zurückgegebenen Ergebnisse binden.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Wenn keine "auch erworbenen" Elemente vorhanden waren, zeigen wir einfach andere beliebte Elemente aus unserem Katalog an.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Um die "auch erworbenen" Elemente anzuzeigen, öffnen Sie die Seite ProductDetails. aspx, und ziehen Sie das Steuerelement alsopurgejagt aus dem Projektmappen-Explorer, sodass es an dieser Position im Markup angezeigt wird.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Dadurch wird am oberen Rand der Seite "ProductDetails" ein Verweis auf das Steuerelement erstellt.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Da das Benutzer Steuerelement "alsopurgejagt" eine ProductID-Nummer erfordert, wird die ProductID-Eigenschaft des Steuer Elements mit einer eval-Anweisung für das aktuelle Datenmodell Element der Seite festgelegt.

![](tailspin-spyworks-part-7/_static/image3.png)

Wenn wir jetzt erstellen und ausführen und zu einem Produkt navigieren, werden die Elemente "auch gekauft" angezeigt.

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-6.md)
> [Weiter](tailspin-spyworks-part-8.md)
