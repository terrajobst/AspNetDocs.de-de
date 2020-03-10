---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Erstellen eines Bewertungs Steuer Elements (VB) | Microsoft-Dokumentation
author: wenz
description: Viele Websites, von e-Commerce bis hin zu Communitysites, bieten ihren Benutzern die Bewertung von Artikeln oder Elementen. Dies erfordert in der Regel einen gewissen Codierungsaufwand, aber wir haben die...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 08e245edfe73db4e3896db51151e5d7a0fa9697c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496035"
---
# <a name="creating-a-rating-control-vb"></a>Erstellen eines Bewertungssteuerelements (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Viele Websites, von e-Commerce bis hin zu Communitysites, bieten ihren Benutzern die Bewertung von Artikeln oder Elementen. Dies erfordert in der Regel einen gewissen Codierungsaufwand, aber wir haben das steuerungstooltoolkit zur Verfügung.

## <a name="overview"></a>Übersicht

Viele Websites, von e-Commerce bis hin zu Communitysites, bieten ihren Benutzern die Bewertung von Artikeln oder Elementen. Dies erfordert in der Regel einen gewissen Codierungsaufwand, aber wir haben das steuerungstooltoolkit zur Verfügung.

## <a name="steps"></a>Schritte

Zuerst benötigen Sie mindestens zwei Arten von Bildern: eine für ein ausgefülltes Bewertungs Element und eine für ein leeres Bewertungs Element. Ein Bewertungs Element ist normalerweise ein Stern oder ein Smiley. Für dieses Szenario finden Sie die drei Dateien "Smiley. png" und "Empty. png" und "Smiley-done. png" im Rahmen der Quell Code Downloads für dieses Tutorial.

Erstellen Sie dann eine neue ASP.NET-Datei, und beginnen Sie damit, ihr ein `ScriptManager`-Steuerelement hinzuzufügen:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Fügen Sie dann das `Rating`-Steuerelement aus dem ASP.NET AJAX Control Toolkit hinzu. Für dieses Beispiel müssen die folgenden Attribute festgelegt werden:

- `CurrentRating` die zu verwendende anfängliche Bewertung
- maximale Bewertung `MaxRating`
- `EmptyStarCssClass` Sie die zu verwendende CSS-Klasse, wenn ein Bewertungs Element (Star) leer ist.
- `FilledStarCssClass` der CSS-Klasse, die verwendet werden soll, wenn ein Bewertungs Element (Star) ausgefüllt wird
- `StarCssClass` der CSS-Klasse, die für einen sichtbaren stat verwendet werden soll
- `WaitingStarCssClass` Sie die zu verwendende CSS-Klasse, während eine Sterne-Bewertung an den Server zurückgesendet wird.

Und hier ist das Markup, das ein Bewertungs Steuerelement mit fünf Elementen (Smileys) erstellt, von denen keine anfänglich ausgefüllt wird:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Die drei CSS-Klassen, auf die verwiesen wird, müssen jetzt die entsprechenden Bilddateien anzeigen, was mit CSS leicht zu tun ist:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Stellen Sie sicher, dass Sie die Breite und Höhe der drei Bilder angeben. andernfalls kann die Anzeige ein wenig nach oben erscheinen.

Schließlich sollte das Ergebnis der Bewertung dem Benutzer angezeigt werden (oder, zumindest in einer Datenbank gespeichert). Fügen Sie daher eine Bezeichnung für die Ausgabe einer Textnachricht und eine Schaltfläche "Senden" hinzu, um das Bewertungsformular an den Server zurückzusenden:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Greifen Sie im serverseitigen Code über den `ID` auf das Bewertungs Steuerelement zu, und greifen Sie dann auf seine `CurrentRating`-Eigenschaft zu. Dies ist die Anzahl der ausgewählten Bewertungs Elemente, in unserem Beispiel ein Wert zwischen 0 und 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Speichern Sie die Seite, und laden Sie Sie in Ihren Browser. Wenn Sie mit dem Mauszeiger auf die (anfänglich leeren) Bewertungs Elemente zeigen, tritt ein JavaScript-Effekt auf: die Bewertung ändert sich. Wenn Sie auf den Sternen Satz klicken, wird die aktuelle Bewertung beibehalten. Wenn Sie das Formular übermitteln, gibt der serverseitige Code die ausgewählte Bewertung aus.

[![Erstellen eines Bewertungssystems mit minimalem Code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Erstellen eines Bewertungssystems mit minimalem Code ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Previous](creating-a-rating-control-cs.md)
