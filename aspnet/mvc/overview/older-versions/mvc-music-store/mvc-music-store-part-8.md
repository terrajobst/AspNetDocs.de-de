---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Teil 8: Einkaufswagen mit AJAX-Updates | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 8 umfasst den Warenkorb mit AJAX-Updates.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433515"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a>Teil 8: Einkaufswagen mit AJAX-Updates

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.  
>   
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 8 umfasst den Warenkorb mit AJAX-Updates.

Wir gestatten Benutzern das Platzieren von Alben in Ihrem Warenkorb, ohne sich zu registrieren, aber Sie müssen sich als Gäste registrieren, um das Auschecken abzuschließen. Der Einkaufs-und Auscheck Prozess wird in zwei Controller unterteilt: einen ShoppingCart-Controller, der das anonyme Hinzufügen von Elementen zu einem Warenkorb ermöglicht, und einen Checkout-Controller, der den Checkout Prozess verarbeitet. Wir beginnen mit dem Warenkorb in diesem Abschnitt und erstellen dann den Checkout-Prozess im folgenden Abschnitt.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Hinzufügen der Modellklassen Warenkorb, Order und OrderDetail

In unseren Warenkorb-und Checkout-Prozessen werden einige neue Klassen verwendet. Klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und fügen Sie eine Warenkorb-Klasse (Cart.cs) mit dem folgenden Code hinzu.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Diese Klasse ähnelt den anderen, die bisher verwendet wurden, mit Ausnahme des [key]-Attributs für die RecordID-Eigenschaft. Unsere Warenkorb-Elemente verfügen über einen Zeichen folgen Bezeichner namens "cartId", um anonyme Einkäufe zuzulassen, aber die Tabelle enthält einen ganzzahligen Primärschlüssel namens "recordID". Gemäß der Konvention erwartet Entity Framework Code zunächst, dass der Primärschlüssel für eine Tabelle mit dem Namen "Cart" entweder "cartId" oder "ID" ist, aber wir können dies problemlos über Anmerkungen oder Code überschreiben. Dies ist ein Beispiel dafür, wie wir die einfachen Konventionen in Entity Framework Code verwenden können, wenn Sie sich an uns wenden, aber wir werden nicht durch Sie eingeschränkt, wenn dies nicht der Fall ist.

Fügen Sie als nächstes eine Order-Klasse (Order.cs) mit dem folgenden Code hinzu.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Diese Klasse verfolgt Zusammenfassungs-und Übermittlungs Informationen für eine Bestellung. Die **Kompilierung erfolgt noch nicht**, da Sie eine OrderDetails-Navigations Eigenschaft aufweist, die von einer Klasse abhängt, die noch nicht erstellt wurde. Beheben Sie dies jetzt, indem Sie eine Klasse mit dem Namen "OrderDetail.cs" hinzufügen und den folgenden Code hinzufügen.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Wir nehmen ein letztes Update für die Klasse "musicstoreentities" vor, um dbsets aufzunehmen, die diese neuen Modellklassen verfügbar machen, einschließlich einer dbset-&lt;Künstler&gt;. Die aktualisierte Klasse "musicstoreentities" wird wie unten dargestellt angezeigt.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Verwalten der Einkaufswagen-Geschäftslogik

Als Nächstes erstellen wir die Klasse "ShoppingCart" im Ordner "Models". Das ShoppingCart-Modell verarbeitet den Datenzugriff auf die Warenkorb-Tabelle. Außerdem wird die Geschäftslogik zum Hinzufügen und Entfernen von Elementen aus dem Warenkorb behandelt.

Da es nicht erforderlich sein soll, dass sich Benutzer für ein Konto registrieren, um dem Warenkorb Elemente hinzuzufügen, weisen wir Benutzern einen temporären eindeutigen Bezeichner (mit einer GUID oder Globally Unique Identifier) zu, wenn Sie auf den Warenkorb zugreifen. Wir speichern diese ID mithilfe der ASP.NET Session-Klasse.

*Hinweis: die ASP.NET-Sitzung ist ein bequemer Ort zum Speichern Benutzer spezifischer Informationen, die ablaufen, nachdem Sie die Website verlassen haben. Obwohl die Verwendung des Sitzungs Zustands die Auswirkungen auf die Leistung von größeren Standorten haben kann, ist unsere helle Verwendung für Demonstrationszwecke gut geeignet.*

Die ShoppingCart-Klasse stellt die folgenden Methoden zur Verfügung:

**AddTo Cart** nimmt ein Album als Parameter an und fügt es dem Warenkorb des Benutzers hinzu. Da die Warenkorb-Tabelle die Menge für jedes Album nachverfolgt, umfasst Sie Logik, um bei Bedarf eine neue Zeile zu erstellen, oder die Menge zu erhöhen, wenn der Benutzer bereits eine Kopie des Albums angeordnet hat.

**Removefromcart** nimmt eine Album-ID an und entfernt Sie aus dem Warenkorb des Benutzers. Wenn der Benutzer nur eine Kopie des Albums in seinem Warenkorb besaß, wird die Zeile entfernt.

**Emptycart** entfernt alle Elemente aus dem Einkaufswagen eines Benutzers.

" **Getcartitems** " Ruft eine Liste von "kartitems" zur Anzeige oder Verarbeitung ab.

**GetCount** Ruft die Gesamtzahl der Alben ab, die ein Benutzer in seinem Warenkorb hat.

**GetTotal** berechnet die Gesamtkosten aller Elemente im Warenkorb.

" **Anateorder** " konvertiert den Einkaufswagen in eine Bestellung während der Auscheck Phase.

**Getcart** ist eine statische Methode, mit der unsere Controller ein Warenkorb-Objekt abrufen können. Er verwendet die **getcartid** -Methode, um das Lesen der "cartId" aus der Sitzung des Benutzers zu verarbeiten. Die getcartid-Methode erfordert HttpContextBase, damit Sie die cartId des Benutzers aus der Benutzersitzung lesen kann.

Hier ist die komplette **ShoppingCart-Klasse**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Der Warenkorb des Warenkorb muss einige komplexe Informationen an seine Ansichten übermitteln, die den Modell Objekten nicht ordnungsgemäß zugeordnet werden. Wir möchten unsere Modelle nicht so ändern, dass Sie an unsere Ansichten angepasst werden. Modellklassen sollten unsere Domäne und nicht die Benutzeroberfläche darstellen. Eine Lösung besteht darin, die Informationen mithilfe der viewbag-Klasse an unsere Ansichten weiterzugeben, wie dies bei den Dropdown Informationen des Store-Managers der Fall ist, aber das Übergeben von vielen Informationen über viewbag ist schwierig zu verwalten.

Eine Lösung hierfür ist die Verwendung des *ViewModel* -Musters. Bei Verwendung dieses Musters erstellen wir stark typisierte Klassen, die für unsere spezifischen Ansichts Szenarien optimiert sind und die Eigenschaften für die dynamischen Werte und Inhalte verfügbar machen, die von unseren Ansichts Vorlagen benötigt werden. Unsere Controller Klassen können diese Ansichts optimierten Klassen dann auffüllen und an unsere Ansichts Vorlage übergeben, um Sie zu verwenden. Dies ermöglicht Typsicherheit, Kompilierungszeit Überprüfung und Editor-IntelliSense innerhalb von Ansichts Vorlagen.

Wir erstellen zwei Ansichts Modelle für die Verwendung in unserem Warenkorb-Controller: das shoppingcartviewmodel speichert den Inhalt des Einkaufswagens des Benutzers, und das shoppingcartremuveviewmodel wird verwendet, um Bestätigungs Informationen anzuzeigen, wenn ein Benutzer etwas entfernt. aus dem Warenkorb.

Erstellen Sie im Stammverzeichnis des Projekts einen neuen Ordner "ViewModels", um die Aktivitäten zu organisieren. Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie Hinzufügen/neuer Ordner.

![](mvc-music-store-part-8/_static/image1.jpg)

Nennen Sie den Ordner ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

Fügen Sie als nächstes die Klasse "shoppingcartviewmodel" im Ordner "ViewModels" hinzu. Sie verfügt über zwei Eigenschaften: eine Liste von waren Korb Elementen und einen Dezimalwert, der den Gesamtpreis für alle Artikel im Warenkorb enthält.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Fügen Sie nun das shoppingcartremuveviewmodel dem Ordner "ViewModels" mit den folgenden vier Eigenschaften hinzu.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Einkaufswagen Controller

Der Einkaufswagen Controller hat drei Hauptaufgaben: das Hinzufügen von Elementen zu einem Warenkorb, das Entfernen von Elementen aus dem Warenkorb und das Anzeigen von Elementen im Warenkorb. Dabei werden die drei soeben erstellten Klassen verwendet: shoppingcartviewmodel, shoppingcartremuveviewmodel und ShoppingCart. Wie bei StoreController und storemanagercontroller fügen wir ein Feld hinzu, um eine Instanz von "musicstoreentities" zu speichern.

Fügen Sie dem Projekt einen neuen Einkaufswagen Controller mit der leeren Controller Vorlage hinzu.

![](mvc-music-store-part-8/_static/image2.png)

Hier ist der komplette ShoppingCart-Controller. Die Aktionen "index" und "Controller hinzufügen" sollten sehr vertraut aussehen. Die Remove-und cartsummary-Controller Aktionen behandeln zwei Sonderfälle, die im folgenden Abschnitt erläutert werden.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>AJAX-Updates mit jQuery

Im nächsten Schritt erstellen wir eine waren Korb Index Seite, die stark an das shoppingcartviewmodel typisiert ist und die Listen Ansichts Vorlage verwendet, die dieselbe Methode wie zuvor verwendet.

![](mvc-music-store-part-8/_static/image3.png)

Anstatt jedoch einen HTML. Action Link zum Entfernen von Elementen aus dem Warenkorb zu verwenden, verwenden wir jQuery, um das Click-Ereignis für alle Links in dieser Ansicht zu übertragen, die über die HTML-Klasse removelink verfügen. Anstatt das Formular zu veröffentlichen, stellt dieser Click-Ereignishandler nur einen AJAX-Rückruf an unsere removefromcart-Controller Aktion. Removefromcart gibt ein serialisiertes JSON-Ergebnis zurück, das der jQuery-Rückruf dann analysiert und vier schnelle Aktualisierungen der Seite mithilfe von jQuery durchführt:

- 1. Entfernt das gelöschte Album aus der Liste.
- 2. Aktualisiert die Anzahl der waren Korb in der Kopfzeile.
- 3. Zeigt dem Benutzer eine Aktualisierungs Meldung an.
- 4. Aktualisiert den Gesamtpreis des Warenkorbs

Da das Entfernungs Szenario von einem AJAX-Rückruf innerhalb der Index Ansicht behandelt wird, benötigen wir keine zusätzliche Ansicht für die removefromcart-Aktion. Im folgenden finden Sie den gesamten Code für die/ShoppingCart/Index-Sicht:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Um dies zu testen, müssen wir in der Lage sein, dem Warenkorb Elemente hinzuzufügen. Wir aktualisieren die Ansicht " **Store-Details** ", um die Schaltfläche "zum Warenkorb hinzufügen" einzuschließen. Während wir uns befinden, können wir einige der zusätzlichen Informationen des Albums hinzufügen, die wir seit der letzten Aktualisierung dieser Ansicht hinzugefügt haben: Genre, Künstler, Preis und Album Kunst. Der aktualisierte Code für die Details der Speicher Details wird wie unten dargestellt angezeigt.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Nun können wir durch den Store klicken und testen, wie Sie Alben zu und aus dem Warenkorb hinzufügen und daraus entfernen. Führen Sie die Anwendung aus, und navigieren Sie zum Store-Index.

![](mvc-music-store-part-8/_static/image4.png)

Klicken Sie anschließend auf ein Genre, um eine Liste der Alben anzuzeigen.

![](mvc-music-store-part-8/_static/image5.png)

Wenn Sie auf einen Album Titel klicken, wird nun unsere aktualisierte Ansicht mit den Album Details angezeigt, einschließlich der Schaltfläche "zu Warenkorb hinzufügen".

![](mvc-music-store-part-8/_static/image6.png)

Wenn Sie auf die Schaltfläche "zum Warenkorb hinzufügen" klicken, wird unsere waren Korb Index Ansicht mit der zusammenfassenden Liste Warenkorb angezeigt.

![](mvc-music-store-part-8/_static/image7.png)

Nachdem Sie den Warenkorb geladen haben, können Sie auf den Link aus Warenkorb entfernen klicken, um das AJAX-Update für Ihren Warenkorb anzuzeigen.

![](mvc-music-store-part-8/_static/image8.png)

Wir haben einen funktionierenden Einkaufswagen erstellt, der es nicht registrierten Benutzern ermöglicht, Ihrem Warenkorb Elemente hinzuzufügen. Im folgenden Abschnitt können wir Sie registrieren und den Auscheck Prozess vervollständigen.

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-7.md)
> [Weiter](mvc-music-store-part-9.md)
