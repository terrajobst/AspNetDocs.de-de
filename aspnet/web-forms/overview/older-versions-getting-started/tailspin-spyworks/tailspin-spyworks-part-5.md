---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Teil 5: Geschäftslogik | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 5 fügt einige Geschäftslogik hinzu.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511545"
---
# <a name="part-5-business-logic"></a>Teil 5: Geschäftslogik

von [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen. Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 5 fügt einige Geschäftslogik hinzu.

## <a id="_Toc260221671"></a>Hinzufügen von Geschäftslogik

Wir möchten, dass unsere Einkaufsmöglichkeiten immer verfügbar sind, wenn jemand unsere Website besucht. Besucher können Elemente im Warenkorb durchsuchen und hinzufügen, auch wenn Sie nicht registriert oder angemeldet sind. Wenn Sie bereit sind, sich zu authentifizieren, erhalten Sie die Möglichkeit, sich zu authentifizieren. Wenn Sie noch keine Mitglieder sind, können Sie ein Konto erstellen.

Dies bedeutet, dass wir die Logik implementieren müssen, um den Warenkorb von einem anonymen Zustand in den Zustand "registrierter Benutzer" zu konvertieren.

Erstellen Sie ein Verzeichnis mit dem Namen "classes", klicken Sie dann mit der rechten Maustaste auf den Ordner, und erstellen Sie eine neue Klassendatei mit dem Namen MyShoppingCart.cs.

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Wie bereits erwähnt, erweitern wir die-Klasse, die die Seite "myshoppingcart. aspx" implementiert, und wir werden dies mit tun. Das leistungsfähige Konstrukt "partielle Klasse" von net.

Der generierte-Befehl für die MyShoppingCart.aspx.CF-Datei sieht wie folgt aus.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Beachten Sie die Verwendung des Schlüssel Worts "Partial".

Die soeben generierte Klassendatei sieht wie folgt aus.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Wir werden unsere Implementierungen zusammenführen, indem wir das partielle Schlüsselwort zu dieser Datei hinzufügen.

Unsere neue Klassendatei sieht nun wie folgt aus.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Die erste Methode, die wir unserer Klasse hinzufügen werden, ist die "AddItem"-Methode. Dies ist die Methode, die letztendlich aufgerufen wird, wenn der Benutzer auf der Seite Produktliste und Produkt Details auf die Links "Add to Art" klickt.

Fügen Sie Folgendes an die using-Anweisungen am oberen Rand der Seite an.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Und fügen Sie der myshoppingcart-Klasse diese Methode hinzu.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Wir verwenden LINQ to Entities, um festzustellen, ob das Element bereits im Warenkorb ist. Wenn dies der Fall ist, aktualisieren wir die Bestellmenge des Elements, andernfalls erstellen wir einen neuen Eintrag für das ausgewählte Element.

Um diese Methode aufzurufen, implementieren wir eine AddTo Cart. aspx-Seite, die nicht nur diese Methode verwendet, sondern dann den aktuellen Warenkorb a =-Warenkorb angezeigt, nachdem das Element hinzugefügt wurde.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektmappennamen, und fügen Sie eine neue Seite mit dem Namen "AddTo Cart. aspx" hinzu.

Diese Seite kann zwar verwendet werden, um Zwischenergebnisse wie niedrige Aktien Probleme anzuzeigen usw. in unserer Implementierung wird die Seite jedoch nicht tatsächlich gerendert, sondern ruft stattdessen die "Add"-Logik und die Umleitung auf.

Um dies zu erreichen, fügen Sie der Seite\_Lade Ereignis den folgenden Code hinzu.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Beachten Sie, dass das Produkt, das dem Warenkorb hinzugefügt wird, aus einem QueryString-Parameter abgerufen und die AddItem-Methode der Klasse aufgerufen wird.

Wenn keine Fehler auftreten, wird die Steuerung an die ShoppingCart. aspx-Seite weitergeleitet, die wir als nächstes vollständig implementieren werden. Wenn ein Fehler vorliegt, wird eine Ausnahme ausgelöst.

Zurzeit haben wir noch keinen globalen Fehlerhandler implementiert, sodass diese Ausnahme von der Anwendung nicht behandelt wird. Wir werden dies jedoch in Kürze beheben.

Beachten Sie auch die Verwendung der Anweisung Debug. Fail () (verfügbar über `using System.Diagnostics;)`

Wenn die Anwendung im Debugger ausgeführt wird, zeigt diese Methode ein detailliertes Dialogfeld mit Informationen zum Status der Anwendung zusammen mit der angegebenen Fehlermeldung an.

Bei der Ausführung in der Produktion wird die Debug. Fail ()-Anweisung ignoriert.

Beachten Sie im obigen Code einen Aufrufen einer Methode in unseren Warenkorb-Klassennamen "getshoppingcartid".

Fügen Sie den Code zum Implementieren der-Methode wie folgt hinzu.

Beachten Sie, dass wir auch die Schaltflächen "Aktualisieren" und "Auschecken" sowie eine Bezeichnung hinzugefügt haben.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Wir können nun Artikel zum Warenkorb hinzufügen, aber wir haben die Logik zum Anzeigen des Warenkorbs nach dem Hinzufügen eines Produkts nicht implementiert.

Auf der Seite myshoppingcart. aspx fügen wir also ein EntityDataSource-Steuerelement und ein gridvire-Steuerelement wie folgt hinzu.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Rufen Sie das Formular im Designer auf, damit Sie auf die Schaltfläche zum Aktualisieren des Einkaufswagens doppelklicken und den Click-Ereignishandler generieren können, der in der Deklaration im Markup angegeben ist.

Die Details werden später implementiert, aber wir können die Anwendung ohne Fehler erstellen und ausführen.

Wenn Sie die Anwendung ausführen und dem Warenkorb ein Element hinzufügen, wird dies angezeigt.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Beachten Sie, dass die "Standard"-Raster Anzeige durch die Implementierung von drei benutzerdefinierten Spalten getrennt wurde.

Der erste ist ein bearbeitbares, "gebundenes" Feld für die Menge:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Der nächste Schritt ist eine "berechnete" Spalte, die die Summe der Zeilen Elemente anzeigt (die Kosten für die Anzahl der zu befolgenden Elemente):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Schließlich haben wir eine benutzerdefinierte Spalte, die ein CheckBox-Steuerelement enthält, mit dem der Benutzer angeben kann, dass das Element aus dem Einkaufs Diagramm entfernt werden soll.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Wie Sie sehen können, ist die Reihenfolge der Bestellungen leer, daher fügen wir eine Logik hinzu, um den Gesamtbetrag der Bestellung zu berechnen.

Wir implementieren zuerst eine "GetTotal"-Methode in unsere myshoppingcart-Klasse.

Fügen Sie in der Datei MyShoppingCart.cs den folgenden Code hinzu.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Anschließend können Sie auf der Seite\_Lade Ereignishandler unsere GetTotal-Methode aufrufen. Gleichzeitig fügen wir einen Test hinzu, um zu überprüfen, ob der Warenkorb leer ist, und passen die Anzeige entsprechend an.

Wenn der Warenkorb nun leer ist, erhalten Sie Folgendes:

![](tailspin-spyworks-part-5/_static/image4.jpg)

Wenn dies nicht der Wert ist, wird die Summe angezeigt.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Diese Seite ist jedoch noch nicht fertig.

Wir benötigen zusätzliche Logik, um den Warenkorb neu zu berechnen, indem Sie zum Entfernen markierte Elemente entfernen und neue Mengen Werte festlegen, da einige möglicherweise im Raster vom Benutzer geändert wurden.

Fügen Sie der Warenkorb-Klasse in MyShoppingCart.cs eine "RemoveItem"-Methode hinzu, um den Fall zu behandeln, wenn ein Benutzer ein Element zum Entfernen kennzeichnet.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Nun sehen wir uns eine Methode an, die den Umstand behandelt, wenn ein Benutzer einfach die Qualität ändert, die in der GridView angeordnet werden soll.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Mit den grundlegenden Features zum Entfernen und aktualisieren können wir die Logik implementieren, die tatsächlich den Warenkorb in der Datenbank aktualisiert. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Sie werden feststellen, dass diese Methode zwei Parameter erwartet. Eine ist die Warenkorb-ID und die andere ein Array von Objekten des benutzerdefinierten Typs.

Um die Abhängigkeit unserer Logik in Bezug auf die Benutzeroberfläche möglichst gering zu halten, haben wir eine Datenstruktur definiert, die wir verwenden können, um die Warenkorb-Elemente an unseren Code zu übergeben, ohne dass die Methode direkt auf das GridView-Steuerelement zugreifen muss.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

In unserer MyShoppingCart.aspx.cs-Datei können wir diese Struktur in unserem Click-Ereignishandler für die Update-Schaltfläche wie folgt verwenden. Beachten Sie, dass wir nicht nur den Warenkorb aktualisieren, sondern auch die Summe des Warenkorbs.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Beachten Sie, dass diese Codezeile besonders interessiert ist:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues () ist eine spezielle Hilfsfunktion, die in MyShoppingCart.aspx.cs wie folgt implementiert wird.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Dies bietet eine saubere Möglichkeit, auf die Werte der gebundenen Elemente in unserem GridView-Steuerelement zuzugreifen. Da das Kontrollkästchen "Element entfernen" nicht gebunden ist, wird es über die FindControl ()-Methode darauf zugegriffen.

In dieser Phase der Projektentwicklung sind wir bereit, den Checkout-Prozess zu implementieren.

Bevor Sie dies tun, verwenden wir Visual Studio zum Generieren der Mitgliedschafts Datenbank und zum Hinzufügen eines Benutzers zum Mitgliedschaftsrepository.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-4.md)
> [Weiter](tailspin-spyworks-part-6.md)
