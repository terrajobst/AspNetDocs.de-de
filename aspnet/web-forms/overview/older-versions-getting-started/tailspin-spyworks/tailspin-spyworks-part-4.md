---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Teil 4: Auflisten von Produkten | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 4 enthält eine Auflistung von Produkten mit der GridView-Konsole...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457281"
---
# <a name="part-4-listing-products"></a>Teil 4: Auflisten von Produkten

von [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen. Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 4 umfasst das Auflisten von Produkten mit dem GridView-Steuerelement.

## <a id="_Toc260221670"></a>Auflisten von Produkten mit dem GridView-Steuerelement

Wir beginnen mit der Implementierung der Seite "productList. aspx", indem wir mit der rechten Maustaste auf die Projekt Mappe Klicken und "hinzufügen" und "Neues Element" auswählen.

![](tailspin-spyworks-part-4/_static/image1.jpg)

Wählen Sie "Webformular mithilfe der Master Seite" aus, und geben Sie den Seitennamen "productList. aspx" ein.

Klicken Sie auf "hinzufügen".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Wählen Sie als nächstes den Ordner "Stile" aus, in den wir die Seite "Site. Master" eingefügt haben, und wählen Sie ihn im Fenster "Ordnerinhalte" aus.

![](tailspin-spyworks-part-4/_static/image3.jpg)

Klicken Sie auf "OK", um die Seite zu erstellen.

Die Datenbank wird wie unten dargestellt mit den Produktdaten aufgefüllt.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Nachdem unsere Seite erstellt wurde, verwenden wir erneut eine Entitäts Datenquelle, um auf diese Produktdaten zuzugreifen. in diesem Fall müssen wir jedoch die Product-Entitäten auswählen und die Elemente beschränken, die nur für die ausgewählte Kategorie zurückgegeben werden.

Um dies zu erreichen, wird der EntityDataSource mitgeteilt, dass die WHERE-Klausel automatisch generiert wird, und wir geben den Where-Parameter an.

Sie erinnern sich, dass Sie beim Erstellen der Menü Elemente im Menü "Produktkategorie" den Link dynamisch erstellt haben, indem Sie der Abfrage Zeichenfolge für jeden Link die CategoryID hinzufügen. Wir weisen die Entitäts Datenquelle an, den Where-Parameter von diesem QueryString-Parameter abzuleiten.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Als Nächstes konfigurieren wir das ListView-Steuerelement, um eine Liste der Produkte anzuzeigen. Zum Erstellen eines optimalen Einkaufserlebnisses komprimieren wir einige präzise Features in jedem einzelnen Produkt, das in unserer listvew angezeigt wird.

- Der Produktname ist ein Link zur Detailansicht des Produkts.
- Der Preis des Produkts wird angezeigt.
- Ein Bild des Produkts wird angezeigt, und wir wählen das Image dynamisch aus einem Katalog Abbild Verzeichnis in unserer Anwendung aus.
- Wir fügen einen Link ein, mit dem das jeweilige Produkt sofort dem Warenkorb hinzugefügt wird.

Hier ist das Markup für die ListView-Steuerelement Instanz.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Wir bauen dynamisch mehrere Verknüpfungen für jedes angezeigte Produkt auf.

Vor dem Testen der neuen Seite müssen wir die Verzeichnisstruktur für die Produktkatalog Images wie folgt erstellen.

![](tailspin-spyworks-part-4/_static/image1.png)

Sobald auf unsere Produkt Images zugegriffen werden kann, können wir unsere Produktlisten Seite testen.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Klicken Sie auf der Startseite der Site auf eine der Listen Verknüpfungen der Kategorie.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Nun müssen wir die Seite "ProductDetails. aspx" und die Funktion "adddecart" implementieren.

Verwenden Sie File-&gt;New, um einen Seitennamen "ProductDetails. aspx" mithilfe der Standort Master Seite wie zuvor zu erstellen.

Wir verwenden erneut ein EntityDataSource-Steuerelement, um auf einen bestimmten Produktdaten Satz in der Datenbank zuzugreifen, und wir verwenden ein ASP.net FormView-Steuerelement, um die Produktdaten wie folgt anzuzeigen.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Machen Sie sich keine Sorgen, wenn die Formatierung etwas lustig erscheint. Das obige Markup lässt im Anzeige Layout für einige Features, die später implementiert werden, Platz im Anzeige Layout aus.

Der Warenkorb stellt die komplexere Logik in unserer Anwendung dar. Verwenden Sie zunächst File-&gt;New, um eine Seite mit dem Namen myshoppingcart. aspx zu erstellen.

Beachten Sie, dass der Name "ShoppingCart. aspx" nicht ausgewählt wird.

Unsere Datenbank enthält eine Tabelle mit dem Namen "ShoppingCart". Beim Generieren einer Entity Data Model eine Klasse für jede Tabelle in der Datenbank erstellt. Daher hat der Entity Data Model eine Entitäts Klasse mit dem Namen "ShoppingCart" generiert. Wir könnten das Modell so bearbeiten, dass wir diesen Namen für die Warenkorb-Implementierung verwenden oder es für unsere Anforderungen erweitern können. wir entscheiden uns stattdessen dafür, einfach einen Namen auszuwählen, der den Konflikt vermeidet.

Beachten Sie auch, dass wir einen einfachen Einkaufswagen erstellen und die Warenkorb-Logik in die Warenkorb-Anzeige einbetten. Wir können uns auch entscheiden, den Warenkorb in einer vollständig separaten Geschäfts Schicht zu implementieren.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-3.md)
> [Weiter](tailspin-spyworks-part-5.md)
