---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Teil 10: endgültige Updates für Navigation und Standort Entwurf, Schlussfolgerung | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 10 behandelt abschließende Updates für Navigation und S...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433611"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Teil 10: endgültige Updates für Navigation und Standort Entwurf, Schlussfolgerung

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.  
>   
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 10 behandelt abschließende Aktualisierungen der Navigation und des Website Entwurfs, der Schlussfolgerung.

Wir haben alle wichtigen Funktionen für unsere Website abgeschlossen, aber wir haben noch einige Features, die Sie der Website Navigation, der Startseite und der Store-Seite zum Durchsuchen hinzufügen können.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Erstellen der Teilansicht der Einkaufswagen Zusammenfassung

Wir möchten die Anzahl der Elemente im Warenkorb des Benutzers auf der gesamten Website verfügbar machen.

![](mvc-music-store-part-10/_static/image1.png)

Dies kann problemlos implementiert werden, indem eine Teilansicht erstellt wird, die zu "Site. Master" hinzugefügt wird.

Wie bereits gezeigt, enthält der ShoppingCart-Controller eine cartsummary-Aktionsmethode, die eine Teilansicht zurückgibt:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Klicken Sie mit der rechten Maustaste auf den Ordner views/ShoppingCart, und wählen Sie Ansicht hinzufügen aus, um die Teilansicht von cartsummary zu erstellen. Benennen Sie die Ansicht cartsummary, und aktivieren Sie das Kontrollkästchen "partielle Ansicht erstellen", wie unten gezeigt.

![](mvc-music-store-part-10/_static/image2.png)

Die Teilansicht "cartsummary" ist wirklich einfach. es handelt sich lediglich um einen Link zur ShoppingCart-Index Sicht, in der die Anzahl der Elemente im Warenkorb angezeigt wird. Der vollständige Code für "cartsummary. cshtml" lautet wie folgt:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Mithilfe der HTML. renderaction-Methode können wir eine Teilansicht in eine beliebige Seite der Site einschließen, einschließlich des Standort Masters. Renderaction erfordert, dass wir den Aktions Namen ("cartsummary") und den Controller Namen ("ShoppingCart") wie unten beschrieben angeben.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Vor dem Hinzufügen des Website Layouts wird auch das Genre Menü erstellt, damit alle Website-. Master-Updates gleichzeitig durchführen können.

## <a name="creating-the-genre-menu-partial-view"></a>Erstellen der Teilansicht für Genre Menü

Wir können es unseren Benutzern sehr viel leichter machen, durch den Store zu navigieren, indem Sie ein Genre Menü hinzufügen, in dem alle im Store verfügbaren Genres aufgelistet sind.

![](mvc-music-store-part-10/_static/image3.png)

Wir führen die gleichen Schritte aus, um auch eine partielle genremenu-Ansicht zu erstellen. Anschließend können wir beide dem Website Master hinzufügen. Fügen Sie zunächst die folgende genremenu-Controller Aktion zum StoreController hinzu:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Mit dieser Aktion wird eine Liste der Genres zurückgegeben, die von der Teilansicht angezeigt werden, die wir als Nächstes erstellen.

*Hinweis: Wir haben der Controller Aktion das Attribut [childaction only] hinzugefügt, das angibt, dass diese Aktion nur von einer Teilansicht aus verwendet werden soll. Dieses Attribut verhindert, dass die Controller Aktion durch Browsen zu/Store/GenreMenu. ausgeführt wird. Dies ist für Teilansichten nicht erforderlich. es ist jedoch eine bewährte Vorgehensweise, da wir sicherstellen möchten, dass unsere Controller Aktionen wie geplant verwendet werden. Wir geben auch "partialview" anstelle von "View" zurück, sodass das Ansichts Modul weiß, dass es das Layout für diese Ansicht nicht verwenden sollte, da es in andere Ansichten eingeschlossen wird.*

Klicken Sie mit der rechten Maustaste auf die Aktion genremenu Controller, und erstellen Sie eine partielle Ansicht mit dem Namen genremenu, die stark typisiert ist, indem Sie die Datenklasse Genre Ansicht verwenden

![](mvc-music-store-part-10/_static/image4.png)

Aktualisieren Sie den Ansichts Code für die partielle genremenu-Ansicht, um die Elemente mithilfe einer ungeordneten Liste wie folgt anzuzeigen.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Das Website Layout wird aktualisiert, um die Teilansichten anzuzeigen.

Wir können unsere Teilansichten dem Website Layout (/Views/Shared/\_Layout. cshtml) hinzufügen, indem Sie "HTML. renderaction ()" aufrufen. Wir fügen Sie sowohl in als auch ein zusätzliches Markup hinzu, um Sie anzuzeigen, wie unten dargestellt:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Wenn wir nun die Anwendung ausführen, sehen wir das Genre im linken Navigationsbereich und die Warenkorb-Übersicht oben.

## <a name="update-to-the-store-browse-page"></a>Aktualisieren auf die Store-Suchseite

Die Store-Seite zum Durchsuchen ist funktionsfähig, ist aber nicht sehr gut geeignet. Wir können die Seite aktualisieren, um die Alben in einem besseren Layout anzuzeigen, indem Sie den Ansichts Code (in/views/Store/Browse.cshtml) wie folgt aktualisieren:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Hier verwenden wir "URL. action" anstelle von "HTML. Action Link", damit wir eine spezielle Formatierung auf den Link anwenden können, um die Album-Grafik einzuschließen.

*Hinweis: Es wird eine generische Album Abdeckung für diese Alben angezeigt. Diese Informationen werden in der-Datenbank gespeichert und sind über den Speicher-Manager bearbeitbar. Sie sind herzlich Willkommen beim Hinzufügen Ihrer eigenen Grafik.*

Wenn wir nun zu einem Genre navigieren, werden die in einem Raster gezeigten Alben mit der Album-Grafik angezeigt.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aktualisieren der Startseite zum Anzeigen der Top Selling-Alben

Wir möchten unsere Top Selling-Alben auf der Startseite präsentieren, um den Vertrieb zu steigern. Wir nehmen einige Aktualisierungen an unserem HomeController vor, um dies zu bewältigen, und fügen auch einige zusätzliche Grafiken hinzu.

Zuerst fügen wir der Album-Klasse eine Navigations Eigenschaft hinzu, damit EntityFramework weiß, dass Sie verknüpft sind. Die letzten Zeilen der **Album** -Klasse sollten nun wie folgt aussehen:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Hinweis: dazu muss eine using-Anweisung hinzugefügt werden, um den System. Collections. Generic-Namespace zu verwenden.*

Zuerst fügen wir ein storedb-Feld und mvcmusicstore. Models mithilfe von-Anweisungen hinzu, wie in unseren anderen Controllern. Als Nächstes fügen wir dem HomeController die folgende Methode hinzu, die die Datenbank abfragt, um die nachfolgend aufgeführten Alben nach OrderDetails zu suchen.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Dies ist eine private Methode, da wir Sie nicht als Controller Aktion zur Verfügung stellen möchten. Wir fügen ihn aus Gründen der Einfachheit in den HomeController ein. es wird jedoch empfohlen, die Geschäftslogik nach Bedarf in separate Dienst Klassen zu verschieben.

Damit können wir die Index Controller Aktion aktualisieren, um die fünf häufigsten verkauften Alben abzufragen und Sie an die Ansicht zurückzugeben.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Der gesamte Code für den aktualisierten HomeController ist wie unten gezeigt.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Schließlich müssen wir unsere Start Index Ansicht aktualisieren, damit Sie eine Liste der Alben anzeigen kann, indem Sie den Modelltyp aktualisieren und die Liste der Alben unten hinzufügen. Wir nutzen diese Gelegenheit, um der Seite auch eine Überschrift und einen promotionbereich hinzuzufügen.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Wenn wir nun die Anwendung ausführen, sehen wir unsere aktualisierte Startseite mit Top Selling Alben und unsere Werbe Meldung.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Zusammenfassung

Wir haben gesehen, dass ASP.NET MVC das Erstellen einer ausgereiften Website mit Datenbankzugriff, Mitgliedschaft, AJAX usw. erleichtert. ziemlich schnell. Hoffentlich haben Sie in diesem Tutorial die Tools erhalten, die Sie benötigen, um mit dem Aufbau eigener ASP.NET-MVC-Anwendungen zu beginnen.

> [!div class="step-by-step"]
> [Previous](mvc-music-store-part-9.md)
