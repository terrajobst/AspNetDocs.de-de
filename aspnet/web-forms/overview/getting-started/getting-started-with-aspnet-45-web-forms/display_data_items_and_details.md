---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Anzeigen von Datenelementen und Details | Microsoft-Dokumentation
author: Erikre
description: In dieser tutorialreihe werden die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,7 und Microsoft Visual Studio 2017 erläutert.
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520809"
---
# <a name="display-data-items-and-details"></a>Anzeigen von Datenelementen und-Details

von [Erik Reitan](https://github.com/Erikre)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen des Aufbaus einer ASP.net Web Forms-Anwendung mit ASP.NET 4,7 und Microsoft Visual Studio 2017.

In diesem Tutorial erfahren Sie, wie Datenelemente und Datenelement Details mit ASP.net-Web Forms und Entity Framework Code First angezeigt werden. Dieses Tutorial baut auf dem vorherigen Tutorial "Benutzeroberfläche und Navigation" im Rahmen der Wingtip-Lernprogramm Reihe "Tutorial Store" auf. Nach Abschluss dieses Tutorials sehen Sie Produkte auf der Seite " *productList. aspx* " und die Details eines Produkts auf der Seite " *ProductDetails. aspx* ".

## <a name="youll-learn-how-to"></a>Sie lernen, die folgende Aufgaben auszuführen:

- Hinzufügen eines Daten Steuer Elements zum Anzeigen von Produkten aus der Datenbank
- Verbinden eines Daten Steuer Elements mit den ausgewählten Daten
- Hinzufügen eines Daten Steuer Elements zum Anzeigen von Produktdetails aus der Datenbank
- Rufen Sie einen Wert aus der Abfrage Zeichenfolge ab, und verwenden Sie diesen Wert, um die aus der Datenbank abgerufenen Daten einzuschränken.

### <a name="features-introduced-in-this-tutorial"></a>In diesem Tutorial eingeführte Features:

- Modellbindung
- Wert Anbieter

## <a name="add-a-data-control"></a>Hinzufügen eines Daten Steuer Elements

Sie können verschiedene Optionen verwenden, um Daten an ein Server Steuerelement zu binden. Am häufigsten gehören:

* Hinzufügen eines Datenquellen-Steuer Elements
* Manuelles Hinzufügen von Code
* Verwenden der Modell Bindung

### <a name="use-a-data-source-control-to-bind-data"></a>Verwenden eines Datenquellen-Steuer Elements zum Binden von Daten

Durch das Hinzufügen eines Datenquellen-Steuer Elements können Sie das Datenquellen-Steuerelement mit dem Steuerelement verknüpfen, das die Daten anzeigt. Bei diesem Ansatz können Sie serverseitige Steuerelemente deklarativ und nicht Programm gesteuert mit Datenquellen verbinden.

### <a name="code-by-hand-to-bind-data"></a>Code von Hand zum Binden von Daten

Das Codieren per Hand umfasst Folgendes:

1. Lesen eines Werts
2. Überprüfen, ob es NULL ist
3. Umrechnen in einen geeigneten Typ
4. Überprüfen der Konvertierung erfolgreich
5. Verwenden des Werts in der Abfrage 

Mit diesem Ansatz haben Sie die volle Kontrolle über Ihre Datenzugriffs Logik.

### <a name="use-model-binding-to-bind-data"></a>Verwenden der Modell Bindung zum Binden von Daten

Mit der Modell Bindung können Sie Ergebnisse mit viel geringerem Code binden, und Sie können die Funktionalität in der gesamten Anwendung wieder verwenden. Dadurch wird die Arbeit mit Code orientierten Datenzugriffs Logik vereinfacht, während gleichzeitig ein umfassendes Daten Bindungs Framework bereitgestellt wird.

## <a name="display-products"></a>Produkte anzeigen

In diesem Tutorial verwenden Sie die Modell Bindung, um Daten zu binden. Wenn Sie ein Daten Steuerelement so konfigurieren möchten, dass die Modell Bindung verwendet wird, um Daten auszuwählen, legen Sie die `SelectMethod`-Eigenschaft des Steuer Elements auf einen Methodennamen im Code der Seite fest. Das Daten Steuerelement ruft die Methode zum entsprechenden Zeitpunkt im Lebenszyklus der Seite auf und bindet die zurückgegebenen Daten automatisch. Es ist nicht erforderlich, den `DataBind`-Methode explizit aufzurufen.

1. ÖffnenSie in Projektmappen-Explorer *productList. aspx*.
2. Ersetzen Sie das vorhandene Markup durch dieses Markup:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Dieser Code verwendet ein **ListView** -Steuerelement mit dem Namen `productList`, um Produkte anzuzeigen.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Mit Vorlagen und Stilen definieren Sie, wie Daten im **ListView** -Steuerelement angezeigt werden. Es ist nützlich für Daten in jeder sich wiederholenden Struktur. Obwohl in diesem **ListView** -Beispiel lediglich Datenbankdaten angezeigt werden, können Sie ohne Code auch Benutzer zum Bearbeiten, einfügen und Löschen von Daten sowie zum Sortieren und Sortieren von Daten aktivieren.

Wenn Sie die `ItemType`-Eigenschaft im **ListView** -Steuerelement festlegen, ist der Daten Bindungs Ausdruck `Item` verfügbar, und das Steuerelement wird stark typisiert. Wie bereits im vorherigen Tutorial erwähnt, können Sie Element Objektdetails mit IntelliSense auswählen, z. b. die `ProductName`angeben:

![Anzeigen von Datenelementen und Details-IntelliSense](display_data_items_and_details/_static/image1.png)

Außerdem verwenden Sie die Modell Bindung, um einen `SelectMethod` Wert anzugeben. Dieser Wert (`GetProducts`) entspricht der Methode, die Sie dem Code Behind hinzufügen, um im nächsten Schritt Produkte anzuzeigen.

### <a name="add-code-to-display-products"></a>Hinzufügen von Code zum Anzeigen von Produkten

In diesem Schritt fügen Sie Code hinzu, um das **ListView** -Steuerelement mit Produktdaten aus der Datenbank aufzufüllen. Der Code unterstützt das darstellen aller Produkte und Produkte der einzelnen Kategorien.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *productList. aspx* , und wählen Sie dann **Code anzeigen**aus.
2. Ersetzen Sie den vorhandenen Code in der *productList.aspx.cs* -Datei durch folgenden Code:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Dieser Code zeigt die `GetProducts` Methode, auf die die `ItemType`-Eigenschaft des **ListView** -Steuer Elements auf der Seite " *productList. aspx* " verweist. Um die Ergebnisse auf eine bestimmte Daten Bank Kategorie zu beschränken, legt der Code den `categoryId` Wert aus dem Wert der Abfrage Zeichenfolge fest, der an die Seite *productList. aspx* übermittelt wird, wenn auf die Seite *productList. aspx* navigiert wird. Die `QueryStringAttribute`-Klasse im `System.Web.ModelBinding`-Namespace wird verwendet, um den Wert der Abfrage Zeichenfolgen-Variablen `id`abzurufen. Dies weist die Modell Bindung an, einen Wert aus der Abfrage Zeichenfolge zur Laufzeit an den `categoryId`-Parameter zu binden.

Wenn eine gültige Kategorie als Abfrage Zeichenfolge an die Seite übermittelt wird, sind die Ergebnisse der Abfrage auf die Produkte in der Datenbank beschränkt, die dem `categoryId`-Wert entsprechen. Wenn z. b. die Seiten-URL der *productList. aspx* -Seite lautet:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Auf der Seite werden nur die Produkte angezeigt, bei denen der `categoryId` `1`ist.

Wenn die Seite *productList. aspx* aufgerufen wird, werden alle Produkte angezeigt, wenn keine Abfrage Zeichenfolge enthalten ist.

Die Quellen der Werte für diese Methoden werden als *Wert Anbieter* (z. b. *QueryString*) bezeichnet, und die Parameter Attribute, die angeben, welcher Wert Anbieter verwendet werden soll, werden als *Wert Anbieter Attribute* bezeichnet (z. b. `id`). ASP.net schließt Wert Anbieter und zugehörige Attribute für alle typischen Quellen der Benutzereingabe in einer Web Forms Anwendung ein, z. b. Abfrage Zeichenfolge, Cookies, Formular Werte, Steuerelemente, Ansichts Zustand, Sitzungszustand und Profil Eigenschaften. Sie können auch benutzerdefinierte Wert Anbieter schreiben.

### <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung jetzt aus, um alle Produkte oder die Produkte einer Kategorie anzuzeigen.

1. Drücken Sie in Visual Studio die **Taste F5** , um die Anwendung auszuführen.  
   Der Browser wird geöffnet und zeigt die Seite *default. aspx* an.

2. Wählen Sie im Navigationsmenü der Produktkategorie die Option **Autos** aus.  
   Die Seite " *productList. aspx* " zeigt nur Produkte der Kategorie " **Auto** " an. Später in diesem Tutorial zeigen Sie Produktdetails an.  

    ![Anzeigen von Datenelementen und Details-Autos](display_data_items_and_details/_static/image2.png)

3. Wählen Sie im oberen Bereich des Navigationsmenüs die Option **Produkte** aus.  
   Auch hier wird die Seite " *productList. aspx* " angezeigt. dieses Mal wird jedoch die gesamte Liste der Produkte angezeigt.   

    ![Anzeigen von Datenelementen und Details-Produkte](display_data_items_and_details/_static/image3.png)

4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.

### <a name="add-a-data-control-to-display-product-details"></a>Hinzufügen eines Daten Steuer Elements zum Anzeigen von Produktdetails

Anschließend ändern Sie das Markup auf der Seite " *ProductDetails. aspx* ", die Sie im vorherigen Tutorial hinzugefügt haben, um bestimmte Produktinformationen anzuzeigen.

1. Öffnen Sie in **Projektmappen-Explorer**die *Datei ProductDetails. aspx*.

2. Ersetzen Sie das vorhandene Markup durch dieses Markup:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Dieser Code verwendet ein **FormView** -Steuerelement, um bestimmte Produktdetails anzuzeigen. Dieses Markup verwendet Methoden wie die Methoden, die zum Anzeigen von Daten auf der Seite " *productList. aspx* " verwendet werden. Das **FormView** -Steuerelement wird verwendet, um einen einzelnen Datensatz gleichzeitig aus einer Datenquelle anzuzeigen. Wenn Sie das **FormView** -Steuerelement verwenden, erstellen Sie Vorlagen, um Daten gebundene Werte anzuzeigen und zu bearbeiten. Diese Vorlagen enthalten Steuerelemente, Bindungs Ausdrücke und Formatierungen, die das Aussehen und die Funktionalität des Formulars definieren.

Das Herstellen einer Verbindung zwischen dem vorherigen Markup und der Datenbank erfordert zusätzlichen Code.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *ProductDetails. aspx* , und klicken Sie dann auf **Code anzeigen**.  
   Die Datei *ProductDetails.aspx.cs* wird angezeigt.

2. Ersetzen Sie den vorhandenen Code durch den folgenden Code:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Dieser Code sucht nach einem Abfrage Zeichen folgen Wert vom Typ "`productID`". Wenn ein gültiger Wert für die Abfrage Zeichenfolge gefunden wird, wird das entsprechende Produkt angezeigt. Wenn die Abfrage Zeichenfolge nicht gefunden wird oder der Wert nicht gültig ist, wird kein Produkt angezeigt.

### <a name="run-the-application"></a>Ausführen der Anwendung

Nun können Sie die Anwendung ausführen, um ein einzelnes Produkt anzuzeigen, das auf der Grundlage der Produkt-ID angezeigt wird.

1. Drücken Sie in Visual Studio die **Taste F5** , um die Anwendung auszuführen.  
   Der Browser wird geöffnet und zeigt die Seite *default. aspx* an.

2. Wählen Sie im Navigationsmenü der Kategorie die Option **Boote** aus.  
   Die Seite " *productList. aspx* " wird angezeigt.

3. Wählen Sie **Papierboote** aus der Liste Produkt aus.
   Die Seite *ProductDetails. aspx* wird angezeigt.

    ![Anzeigen von Datenelementen und Details-Produkte](display_data_items_and_details/_static/image4.png)
    
4. Schließen Sie den Browser.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Abrufen und Anzeigen von Daten mit Modell Bindung und Web Forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Markup und Code zum Anzeigen von Produkten und Produktdetails hinzugefügt. Sie haben sich über stark typisierte Daten Steuerelemente, Modell Bindungen und Wert Anbieter kennengelernt. Im nächsten Tutorial fügen Sie der Wingtip Toys-Beispielanwendung einen Warenkorb hinzu. 

> [!div class="step-by-step"]
> [Zurück](ui_and_navigation.md)
> [Weiter](shopping-cart.md)
