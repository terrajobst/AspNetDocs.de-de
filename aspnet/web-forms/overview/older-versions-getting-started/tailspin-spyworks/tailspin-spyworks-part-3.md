---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Teil 3: Layout und Kategoriemenü | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 3 umfasst das Hinzufügen von Layout und einem Kategoriemenü.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519099"
---
# <a name="part-3-layout-and-category-menu"></a>Teil 3: Layout und Kategoriemenü

von [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen. Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 3 umfasst das Hinzufügen von Layout und einem Kategoriemenü.

## <a id="_Toc260221669"></a>Hinzufügen von Layout und einem Kategoriemenü

Auf unserer Website-Master Seite fügen wir eine div für die linke Spalte hinzu, die das Menü "Produktkategorie" enthält.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Beachten Sie, dass die gewünschte Ausrichtung und andere Formatierung von der CSS-Klasse bereitgestellt werden, die wir der Datei "Style. CSS" hinzugefügt haben.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Das Menü Product Category wird zur Laufzeit dynamisch erstellt, indem die Commerce-Datenbank nach vorhandenen Produktkategorien abgefragt und die Menü Elemente und die entsprechenden Verknüpfungen erstellt werden.

Um dies zu erreichen, werden zwei ASP verwendet. Leistungsstarke Daten Steuerelemente von net. Das Steuerelement "Entitäts Datenquelle" und das ListView-Steuerelement.

![](tailspin-spyworks-part-3/_static/image1.jpg)

Wechseln wir zu "Entwurfs Ansicht", und verwenden Sie die Hilfsprogramme, um die Steuerelemente zu konfigurieren.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Legen Sie die Eigenschaft für die EntityDataSource-ID auf EDS\_Kategorie\_Menü fest, und klicken Sie auf "Datenquelle konfigurieren".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Wählen Sie die commerceentities-Verbindung aus, die für uns erstellt wurde, als das Entity Data Source-Modell für unsere Commerce-Datenbank erstellt wurde, und klicken Sie auf "weiter".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Wählen Sie den Entitätenmengennamen "categories" aus, und belassen Sie den Rest der Optionen als Standardwert. Klicken Sie auf "Fertigstellen".

Nun legen wir die ID-Eigenschaft der ListView-Steuerelement Instanz fest, die wir auf der Seite auf "ListView"\_"productmenu" platziert haben

![](tailspin-spyworks-part-3/_static/image5.jpg)

Obwohl wir Steuerelement Optionen verwenden könnten, um die Anzeige und Formatierung von Datenelementen zu formatieren, erfordert unsere Menüerstellung nur einfaches Markup, damit wir den Code in der Quell Ansicht eingeben.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Beachten Sie die "eval"-Anweisung: &lt;% # eval ("CategoryName")%&gt;

Die ASP.NET-Syntax &lt;% #%&gt; ist eine Kurzschluss Konvention, die die Laufzeit anweist, beliebige Elemente auszuführen und die Ergebnisse "in Zeile" auszugeben.

Die Anweisung eval ("CategoryName") weist an, dass Sie für den aktuellen Eintrag in der gebundenen Auflistung von Datenelementen den Wert der Entitäts Modell-Elementnamen "CategoryName" abrufen. Dies ist eine präzise Syntax für ein sehr leistungsfähiges Feature.

Die Anwendung kann jetzt ausgeführt werden.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Beachten Sie, dass das Menü "Produktkategorie" nun angezeigt wird. wenn wir mit dem Mauszeiger auf einen der Kategorie-Menü Elemente zeigen, sehen wir, dass der Link "Menü Element" auf eine Seite verweist, die Sie noch implementieren müssen, und "productList. aspx"  Kategorie-ID.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-2.md)
> [Weiter](tailspin-spyworks-part-4.md)
