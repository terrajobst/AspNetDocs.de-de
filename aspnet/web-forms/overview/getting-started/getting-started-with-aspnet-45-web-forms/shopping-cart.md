---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Warenkorb | Microsoft-Dokumentation
author: Erikre
description: Diese tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: d3b619ebd9448d30857ffbaf17fd245b1d54a662
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521115"
---
# <a name="shopping-cart"></a>Einkaufswagen

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für das Web. Für diese tutorialreihe steht ein Visual Studio 2013- [Projekt mit C# Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) zur Verfügung.

In diesem Tutorial wird die Geschäftslogik beschrieben, die zum Hinzufügen eines Einkaufswagen zum Beispiel ASP.net Web Forms Anwendung Wingtip Toys erforderlich ist. Dieses Tutorial baut auf dem vorherigen Tutorial "Anzeigen von Datenelementen und-Details" auf und ist Teil der Wingtip Toy Store-tutorialreihe. Wenn Sie dieses Tutorial abgeschlossen haben, können die Benutzer ihrer Beispiel-APP die Produkte in Ihrem Warenkorb hinzufügen, entfernen und ändern.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

1. Erstellen eines Einkaufswagen für die Webanwendung.
2. Hiermit wird Benutzern das Hinzufügen von Elementen zum Warenkorb ermöglicht.
3. Hinzufügen eines [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) -Steuer Elements zum Anzeigen von Einkaufswagen Details
4. Berechnen und Anzeigen der Bestellsumme.
5. Entfernen und Aktualisieren von Elementen im Warenkorb
6. Einschließen eines Einkaufswagen Zählers

## <a name="code-features-in-this-tutorial"></a>Code Funktionen in diesem Tutorial:

1. Entity Framework Code First
2. Datenanmerkungen
3. Stark typisierte Daten Steuerelemente
4. Modellbindung

## <a name="creating-a-shopping-cart"></a>Erstellen eines Einkaufswagen

Weiter oben in dieser tutorialreihe haben Sie Seiten und Code zum Anzeigen von Produktdaten aus einer-Datenbank hinzugefügt. In diesem Tutorial erstellen Sie einen Warenkorb, um die Produkte zu verwalten, die die Benutzer erwerben möchten. Benutzer können Elemente im Warenkorb durchsuchen und hinzufügen, auch wenn Sie nicht registriert oder angemeldet sind. Zum Verwalten des Einkaufswagen Zugriffs weisen Sie Benutzern eine eindeutige `ID` mithilfe einer Globally Unique Identifier (GUID) zu, wenn der Benutzer zum ersten Mal auf den Warenkorb zugreift. Sie speichern diese `ID` mithilfe des ASP.NET-Sitzungs Zustands.

> [!NOTE] 
> 
> Der ASP.NET-Sitzungszustand ist ein guter Ort zum Speichern von benutzerspezifischen Informationen, die ablaufen, wenn der Benutzer die Website verlässt. Obwohl die Verwendung des Sitzungs Zustands die Auswirkungen auf die Leistung von größeren Websites haben kann, eignet sich die einfache Verwendung des Sitzungs Zustands für Demonstrationszwecke. Das Wingtip Toys-Beispiel Projekt zeigt, wie der Sitzungszustand ohne einen externen Anbieter verwendet wird, wobei der Sitzungszustand auf dem Webserver, auf dem die Website gehostet wird, Prozess unabhängig gespeichert wird. Für größere Standorte, die mehrere Instanzen einer Anwendung bereitstellen, oder für Standorte, die mehrere Instanzen einer Anwendung auf unterschiedlichen Servern ausführen, sollten Sie die Verwendung von **Windows Azure Cache Service**in Erwägung gezogen. Diese Cache Service bietet einen verteilten Cache Dienst, der für die Website extern ist, und löst das Problem der Verwendung des Prozess internen Sitzungs Zustands. Weitere Informationen finden Sie unter Gewusst [wie: Verwenden des ASP.NET-Sitzungs Zustands mit Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider)-Websites.

### <a name="add-cartitem-as-a-model-class"></a>Hinzufügen von cartitem als Modell Klasse

Weiter oben in dieser tutorialreihe haben Sie das Schema für die Kategorie-und Produktdaten definiert, indem Sie die `Category`-und `Product` Klassen im Ordner *Modelle* erstellt haben. Fügen Sie nun eine neue Klasse hinzu, um das Schema für den Warenkorb zu definieren. Später in diesem Tutorial fügen Sie eine Klasse hinzu, um den Datenzugriff auf die `CartItem` Tabelle zu verarbeiten. Diese Klasse stellt die Geschäftslogik bereit, um Elemente im Warenkorb hinzuzufügen, zu entfernen und zu aktualisieren.

1. Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie **Hinzufügen** -&gt; **Neues Element** 

    ![Warenkorb-neues Element](shopping-cart/_static/image1.png)
2. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt. Wählen Sie **Code**aus, und wählen Sie dann **Klasse**aus. 

    ![Warenkorb-Dialog Feld "Neues Element hinzufügen"](shopping-cart/_static/image2.png)
3. Benennen Sie diese neue Klasse *CartItem.cs*.
4. Klicken Sie auf **Hinzufügen**.  
   Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch den folgenden Code:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

Die `CartItem`-Klasse enthält das Schema, das die einzelnen Produkte definiert, die ein Benutzer in den Warenkorb einfügt. Diese Klasse ähnelt den anderen Schema Klassen, die Sie zuvor in dieser tutorialreihe erstellt haben. Gemäß der Konvention wird von Entity Framework Code First erwartet, dass der Primärschlüssel für die `CartItem` Tabelle entweder `CartItemId` oder `ID`ist. Der Code überschreibt jedoch das Standardverhalten, indem das Daten Anmerkung-`[Key]` Attribut verwendet wird. Das `Key`-Attribut der ItemId-Eigenschaft gibt an, dass die `ItemID`-Eigenschaft der Primärschlüssel ist.

Die `CartId`-Eigenschaft gibt die `ID` des Benutzers an, der dem zu erwerbenden Element zugeordnet ist. Sie fügen Code hinzu, um diesen Benutzer `ID` zu erstellen, wenn der Benutzer auf den Warenkorb zugreift. Diese `ID` wird auch als ASP.NET-Sitzungs Variable gespeichert.

### <a name="update-the-product-context"></a>Aktualisieren des Produkt Kontexts

Zusätzlich zum Hinzufügen der `CartItem` Klasse müssen Sie die Daten Bank Kontext Klasse aktualisieren, die die Entitäts Klassen verwaltet und den Datenzugriff auf die Datenbank ermöglicht. Zu diesem Zweck fügen Sie der `ProductContext` Klasse die neu erstellte `CartItem` Modell Klasse hinzu.

1. Suchen Sie in **Projektmappen-Explorer**die Datei *ProductContext.cs* im Ordner *Models* , und öffnen Sie Sie.
2. Fügen Sie der Datei *ProductContext.cs* den markierten Code wie folgt hinzu:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Wie bereits in dieser tutorialreihe erwähnt, fügt der Code in der *ProductContext.cs* -Datei den `System.Data.Entity`-Namespace hinzu, damit Sie auf alle Kernfunktionen des Entity Framework zugreifen können. Diese Funktion bietet die Möglichkeit, Daten durcharbeiten mit stark typisierten Objekten abzufragen, einzufügen, zu aktualisieren und zu löschen. Die `ProductContext`-Klasse bietet Zugriff auf die neu hinzugefügte `CartItem` Model-Klasse.

### <a name="managing-the-shopping-cart-business-logic"></a>Verwalten der Einkaufswagen-Geschäftslogik

Als Nächstes erstellen Sie die `ShoppingCart`-Klasse in einem neuen *Logik* Ordner. Die `ShoppingCart`-Klasse übernimmt den Datenzugriff auf die `CartItem`-Tabelle. Die-Klasse enthält auch die Geschäftslogik, um Elemente im Warenkorb hinzuzufügen, zu entfernen und zu aktualisieren.

Die Warenkorb-Logik, die Sie hinzufügen werden, enthält die Funktionalität zum Verwalten der folgenden Aktionen:

1. Hinzufügen von Elementen zum Warenkorb
2. Entfernen von Elementen aus dem Warenkorb
3. Die Warenkorb-ID wird erhalten.
4. Abrufen von Elementen aus dem Warenkorb
5. Gesamtmenge aller waren Korb Elemente
6. Aktualisieren der Warenkorb-Daten

Eine Einkaufswagen Seite ("*ShoppingCart. aspx*") und die Warenkorb-Klasse wird für den Zugriff auf Einkaufswagen Daten verwendet. Auf der Seite Warenkorb werden alle Elemente angezeigt, die der Benutzer in den Warenkorb einfügt. Neben der Warenkorb-Seite und-Klasse erstellen Sie eine Seite ("*adddecart. aspx*"), um dem Warenkorb Produkte hinzuzufügen. Außerdem fügen Sie der Seite " *productList. aspx* " und der Seite " *ProductDetails. aspx* " Code hinzu, der einen Link zur Seite " *adddecart. aspx* " enthält, sodass der Benutzer Produkte zum Warenkorb hinzufügen kann.

Im folgenden Diagramm wird der grundlegende Prozess veranschaulicht, der auftritt, wenn der Benutzer dem Warenkorb ein Produkt hinzufügt.

![Warenkorb: Hinzufügen zum Warenkorb](shopping-cart/_static/image3.png)

Wenn der Benutzer auf der Seite " *productList. aspx* " oder auf der Seite " *ProductDetails. aspx* " auf den Link **zum Warenkorb hinzufügen** klickt, wird die Anwendung zur Seite " *AddTo Cart. aspx* " und dann automatisch zur Seite " *ShoppingCart. aspx* " navigiert. Auf der Seite " *AddToCart. aspx* " wird das Select-Produkt zum Warenkorb hinzugefügt, indem eine Methode in der Klasse "ShoppingCart" aufgerufen wird. Auf der Seite " *ShoppingCart. aspx* " werden die Produkte angezeigt, die dem Warenkorb hinzugefügt wurden.

#### <a name="creating-the-shopping-cart-class"></a>Erstellen der Warenkorb-Klasse

Die `ShoppingCart` Klasse wird einem separaten Ordner in der Anwendung hinzugefügt, sodass zwischen dem Modell (Modell Ordner), den Seiten (Stamm Ordner) und der Logik (Logik Ordner) ein eindeutiger Unterschied besteht.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **wingtiptoys**, und wählen Sie-&gt;**neuen Ordner** **Hinzufügen** aus. Benennen Sie die neue Ordner *Logik*.
2. Klicken Sie mit der rechten Maustaste auf den Ordner *Logic* , und wählen Sie -&gt; **Neues Element** **Hinzufügen** aus.
3. Fügen Sie eine neue Klassendatei mit dem Namen *ShoppingCartActions.cs*hinzu.
4. Ersetzen Sie den Standardcode durch den folgenden Code:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

Die `AddToCart`-Methode ermöglicht das einschließen einzelner Produkte in den Warenkorb basierend auf dem Product `ID`. Das Produkt wird dem Warenkorb hinzugefügt, oder wenn der Warenkorb bereits ein Element für dieses Produkt enthält, wird die Menge inkrementiert.

Die `GetCartId`-Methode gibt den Warenkorb `ID` für den Benutzer zurück. Der Warenkorb-`ID` wird verwendet, um die Elemente zu verfolgen, die ein Benutzer in seinem Warenkorb hat. Wenn für den Benutzer kein Warenkorb `ID`vorhanden ist, wird eine neue Warenkorb-`ID` erstellt. Wenn der Benutzer als registrierter Benutzer angemeldet ist, wird der Warenkorb-`ID` auf seinen Benutzernamen festgelegt. Wenn der Benutzer jedoch nicht angemeldet ist, wird der Warenkorb-`ID` auf einen eindeutigen Wert (eine GUID) festgelegt. Eine GUID stellt sicher, dass auf der Grundlage der Sitzung nur ein Warenkorb für jeden Benutzer erstellt wird.

Die `GetCartItems`-Methode gibt eine Liste der Warenkorb-Elemente für den Benutzer zurück. Später in diesem Tutorial sehen Sie, dass die Modell Bindung verwendet wird, um die Warenkorb-Elemente im Warenkorb mithilfe der `GetCartItems`-Methode anzuzeigen.

### <a name="creating-the-add-to-cart-functionality"></a>Erstellen der Add-to-Cart-Funktionalität

Wie bereits erwähnt, erstellen Sie eine Verarbeitungsseite mit dem Namen " *AddTo Cart. aspx* ", die zum Hinzufügen neuer Produkte zum Warenkorb des Benutzers verwendet wird. Auf dieser Seite wird die `AddToCart`-Methode in der `ShoppingCart`-Klasse aufgerufen, die Sie soeben erstellt haben. Auf der Seite " *AddTo Cart. aspx* " wird davon ausgegangen, dass ein Produkt `ID` an die Seite übermittelt wird. Diese Produkt `ID` wird verwendet, wenn die `AddToCart`-Methode in der `ShoppingCart`-Klasse aufgerufen wird.

> [!NOTE] 
> 
> Sie ändern den Code-Behind (*AddToCart.aspx.cs*) für diese Seite und nicht die Seiten-UI (*adddecart. aspx*).

#### <a name="to-create-the-add-to-cart-functionality"></a>So erstellen Sie die Add-to-Cart-Funktionalität:

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **wingtiptoys**, und klicken Sie dann auf -&gt; **Neues Element** **Hinzufügen** .  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Fügen Sie der Anwendung mit dem Namen " *adddecart. aspx*" eine neue Standardseite (Web Form) hinzu. 

    ![Warenkorb-Webformular hinzufügen](shopping-cart/_static/image4.png)
3. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Seite *adddecart. aspx* , und klicken Sie dann auf **Code anzeigen**. Die *AddToCart.aspx.cs* -Code Behind-Datei wird im Editor geöffnet.
4. Ersetzen Sie den vorhandenen Code im Code Behind *AddToCart.aspx.cs* durch Folgendes:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Wenn die Seite " *adddecart. aspx* " geladen wird, wird der Product `ID` aus der Abfrage Zeichenfolge abgerufen. Anschließend wird eine Instanz der Warenkorb-Klasse erstellt und verwendet, um die `AddToCart`-Methode aufzurufen, die Sie zuvor in diesem Tutorial hinzugefügt haben. Die `AddToCart`-Methode, die in der *ShoppingCartActions.cs* -Datei enthalten ist, enthält die Logik, um das ausgewählte Produkt dem Warenkorb hinzuzufügen oder die Produktmenge des ausgewählten Produkts zu erhöhen. Wenn das Produkt nicht zum Warenkorb hinzugefügt wurde, wird das Produkt der `CartItem` Tabelle der Datenbank hinzugefügt. Wenn das Produkt bereits zum Warenkorb hinzugefügt wurde und der Benutzer ein zusätzliches Element desselben Produkts hinzufügt, wird die Produktmenge in der `CartItem` Tabelle inkrementiert. Schließlich wird die Seite zurück an die *ShoppingCart. aspx* -Seite umgeleitet, die Sie im nächsten Schritt hinzufügen, wo der Benutzer eine aktualisierte Liste der Elemente im Warenkorb sehen kann.

Wie bereits erwähnt, wird ein Benutzer `ID` verwendet, um die Produkte zu identifizieren, die einem bestimmten Benutzer zugeordnet sind. Diese `ID` wird zu einer Zeile in der `CartItem` Tabelle hinzugefügt, wenn der Benutzer dem Warenkorb ein Produkt hinzufügt.

### <a name="creating-the-shopping-cart-ui"></a>Erstellen der Einkaufswagen-Benutzeroberfläche

Auf der Seite " *ShoppingCart. aspx* " werden die Produkte angezeigt, die der Benutzer dem Warenkorb hinzugefügt hat. Außerdem wird die Möglichkeit bereitgestellt, Elemente im Warenkorb hinzuzufügen, zu entfernen und zu aktualisieren.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf **wingtiptoys**, klicken Sie auf -&gt; **Neues Element** **Hinzufügen** .  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Fügen Sie eine neue Seite (Web Form) hinzu, die eine Master Seite enthält, indem **Sie Web Form mithilfe der Master Seite**auswählen. Nennen Sie die neue Seite " *ShoppingCart. aspx*".
3. Wählen Sie **Site. Master** aus, um die Master Seite an die neu erstellte *aspx* -Seite anzufügen.
4. Ersetzen Sie auf der Seite *ShoppingCart. aspx* das vorhandene Markup durch das folgende Markup:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

Die *ShoppingCart. aspx* -Seite enthält ein **GridView** -Steuerelement mit dem Namen `CartList`. Dieses Steuerelement verwendet die Modell Bindung, um die Warenkorb-Daten aus der Datenbank an das **GridView** -Steuerelement zu binden. Wenn Sie die `ItemType`-Eigenschaft des **GridView** -Steuer Elements festlegen, ist der Daten Bindungs Ausdruck `Item` im Markup des-Steuer Elements verfügbar, und das Steuerelement wird stark typisiert. Wie bereits in dieser tutorialreihe erwähnt, können Sie mithilfe von IntelliSense Details des `Item` Objekts auswählen. Wenn Sie ein Daten Steuerelement für die Verwendung der Modell Bindung zum Auswählen von Daten konfigurieren möchten, legen Sie die `SelectMethod`-Eigenschaft des Steuer Elements fest. Im obigen Markup legen Sie die `SelectMethod` für die Verwendung der getshoppingcartitems-Methode fest, die eine Liste von `CartItem` Objekten zurückgibt. Das **GridView** -Daten Steuerelement ruft die Methode zum entsprechenden Zeitpunkt im Lebenszyklus der Seite auf und bindet die zurückgegebenen Daten automatisch. Die `GetShoppingCartItems`-Methode muss noch hinzugefügt werden.

#### <a name="retrieving-the-shopping-cart-items"></a>Abrufen der Warenkorb-Elemente

Als Nächstes fügen Sie Code zum *ShoppingCart.aspx.cs* -Code-Behind hinzu, um die Warenkorb-Benutzeroberfläche abzurufen und aufzufüllen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Seite *ShoppingCart. aspx* , und klicken Sie dann auf **Code anzeigen**. Die *ShoppingCart.aspx.cs* -Code Behind-Datei wird im Editor geöffnet.
2. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Wie bereits erwähnt, ruft das Daten Steuerelement `GridView` die `GetShoppingCartItems`-Methode zum entsprechenden Zeitpunkt im Lebenszyklus der Seite auf und bindet die zurückgegebenen Daten automatisch. Die `GetShoppingCartItems`-Methode erstellt eine Instanz des `ShoppingCartActions`-Objekts. Anschließend verwendet der Code diese Instanz, um die Elemente im Warenkorb durch Aufrufen der `GetCartItems`-Methode zurückzugeben.

### <a name="adding-products-to-the-shopping-cart"></a>Hinzufügen von Produkten zum Warenkorb

Wenn die Seite " *productList. aspx* " oder " *ProductDetails. aspx* " angezeigt wird, kann der Benutzer das Produkt mithilfe eines Links zum Warenkorb hinzufügen. Wenn Sie auf den Link klicken, navigiert die Anwendung zur Verarbeitungsseite mit dem Namen " *AddTo Cart. aspx*". Auf der Seite *adddecart. aspx* wird die `AddToCart`-Methode in der `ShoppingCart`-Klasse aufgerufen, die Sie zuvor in diesem Tutorial hinzugefügt haben.

Nun fügen Sie der Seite " *productList. aspx* " und der Seite " *ProductDetails. aspx* " einen Link **zum Warenkorb** hinzu. Dieser Link enthält die Produkt `ID`, die aus der Datenbank abgerufen werden.

1. Suchen und öffnen Sie in **Projektmappen-Explorer**die Seite *productList. aspx*.
2. Fügen Sie das in gelber hervorgehobene Markup zur Seite *productList. aspx* hinzu, sodass die gesamte Seite wie folgt aussieht:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testen des Warenkorbs

Führen Sie die Anwendung aus, um zu sehen, wie Sie Produkte zum Warenkorb hinzufügen.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Nachdem das Projekt die Datenbank neu erstellt hat, wird der Browser geöffnet und zeigt die Seite *default. aspx* an.
2. Wählen Sie im Navigationsmenü der Kategorie die Option **Autos** aus.  
 Die Seite " *productList. aspx* " wird angezeigt, die nur Produkte enthält, die in der Kategorie "Auto" enthalten sind. 

    ![Waren Wagen Wagen](shopping-cart/_static/image5.png)
3. Klicken Sie neben dem ersten aufgeführten Produkt auf den Link **zum Warenkorb hinzufügen** (das konvertierbare Auto).   
 Die Seite " *ShoppingCart. aspx* " wird angezeigt und zeigt die Auswahl in Ihrem Warenkorb. 

    ![Warenkorb-Warenkorb](shopping-cart/_static/image6.png)
4. Zeigen Sie weitere Produkte an, indem Sie im Navigationsmenü der Kategorie die Option **Ebenen** auswählen.
5. Klicken Sie neben dem ersten aufgeführten Produkt auf den Link **zum Warenkorb hinzufügen** .  
 Die Seite " *ShoppingCart. aspx* " wird mit dem zusätzlichen Element angezeigt.
6. Schließen Sie den Browser.

### <a name="calculating-and-displaying-the-order-total"></a>Berechnen und Anzeigen der Bestellsumme

Zusätzlich zum Hinzufügen von Produkten zum Warenkorb fügen Sie der `ShoppingCart`-Klasse eine `GetTotal`-Methode hinzu und zeigen die Gesamtbestellmenge auf der Seite "Warenkorb" an.

1. Öffnen Sie in **Projektmappen-Explorer**die Datei *ShoppingCartActions.cs* im Ordner *Logic* .
2. Fügen Sie die folgende `GetTotal`-Methode, die in gelb hervorgehoben ist, der `ShoppingCart`-Klasse hinzu, sodass die-Klasse wie folgt aussieht:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Zuerst Ruft die `GetTotal`-Methode die ID des Warenkorbs für den Benutzer ab. Dann ruft die Methode den Gesamtwert des Warenkorbs ab, indem der Produkt Preis von der Produktmenge für jedes Produkt multipliziert wird, das im Warenkorb aufgeführt ist.

> [!NOTE] 
> 
> Der obige Code verwendet den Werte zulässt-Typ "`int?`". Typen, die NULL-Werte zulassen, können alle Werte eines zugrunde liegenden Typs und auch als NULL-Wert darstellen. Weitere Informationen finden Sie unter [Verwenden von Typen](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx), die NULL-Werte zulassen.

### <a name="modify-the-shopping-cart-display"></a>Ändern der Warenkorb-Anzeige

Im nächsten Schritt ändern Sie den Code für die Seite " *ShoppingCart. aspx* ", um die `GetTotal`-Methode aufzurufen und diese Summe auf der Seite " *ShoppingCart. aspx* " anzuzeigen, wenn die Seite geladen wird.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Seite *ShoppingCart. aspx* , und wählen Sie **Code anzeigen**aus.
2. Aktualisieren Sie in der Datei *ShoppingCart.aspx.cs* den `Page_Load` Handler, indem Sie den folgenden Code in gelb hervorheben:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Wenn die *ShoppingCart. aspx* -Seite geladen wird, lädt Sie das Warenkorb-Objekt und ruft dann den Warenkorb ab, indem die `GetTotal`-Methode der `ShoppingCart`-Klasse aufgerufen wird. Wenn der Warenkorb leer ist, wird eine entsprechende Meldung angezeigt.

### <a name="testing-the-shopping-cart-total"></a>Testen des Warenkorbs Gesamt

Führen Sie die Anwendung jetzt aus, um zu sehen, wie Sie dem Warenkorb nicht nur ein Produkt hinzufügen können, sondern auch den Einkaufswagen gesamt.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Der Browser wird geöffnet und zeigt die Seite *Default.aspx* an.
2. Wählen Sie im Navigationsmenü der Kategorie die Option **Autos** aus.
3. Klicken Sie neben dem ersten Produkt auf den Link **zum Warenkorb hinzufügen** .   
 Die Seite " *ShoppingCart. aspx* " wird mit dem Gesamtbetrag der Bestellung angezeigt. 

    ![Warenkorb-Wagen Gesamt](shopping-cart/_static/image7.png)
4. Fügen Sie dem Warenkorb einige andere Produkte (z. b. eine Ebene) hinzu.
5. Die Seite " *ShoppingCart. aspx* " wird mit einer aktualisierten Summe für alle Produkte angezeigt, die Sie hinzugefügt haben. 

    ![Warenkorb: mehrere Produkte](shopping-cart/_static/image8.png)
6. Beenden Sie die laufende APP, indem Sie das Browserfenster schließen.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Hinzufügen von Update-und Checkout-Schaltflächen zum Warenkorb

Damit die Benutzer den Warenkorb ändern können, fügen Sie die Schaltfläche " **Aktualisieren** " und die **Schaltfläche** "Auschecken" zur Warenkorb-Seite hinzu. Die **Schalt** Fläche "Auschecken" wird erst später in dieser tutorialreihe verwendet.

1. Öffnen Sie in **Projektmappen-Explorer**die Seite *ShoppingCart. aspx* im Stammverzeichnis des Webanwendungs Projekts.
2. Um die Schaltfläche " **Aktualisieren** " und **die Schaltfläche** "Auschecken" der Seite " *ShoppingCart. aspx* " hinzuzufügen, fügen Sie das in gelber hervorgehobene Markup dem vorhandenen Markup hinzu, wie im folgenden Code gezeigt:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Wenn der Benutzer auf die Schaltfläche **Aktualisieren** klickt, wird der `UpdateBtn_Click`-Ereignishandler aufgerufen. Dieser Ereignishandler ruft den Code auf, den Sie im nächsten Schritt hinzufügen werden.

Als nächstes können Sie den in der *ShoppingCart.aspx.cs* -Datei enthaltenen Code aktualisieren, um die Warenkorb-Elemente zu durchlaufen und die `RemoveItem`-und `UpdateItem` Methoden aufzurufen.

1. Öffnen Sie in **Projektmappen-Explorer**die Datei *ShoppingCart.aspx.cs* im Stammverzeichnis des Webanwendungs Projekts.
2. Fügen Sie die folgenden Code Abschnitte, die in gelb hervorgehoben sind, in die Datei *ShoppingCart.aspx.cs*   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Wenn der Benutzer auf der Seite " *ShoppingCart. aspx* " auf die Schaltfläche " **Aktualisieren** " klickt, wird die updatecartitems-Methode aufgerufen. Die updatecartitems-Methode ruft die aktualisierten Werte für jedes Element im Warenkorb ab. Dann ruft die updatecartitems-Methode die `UpdateShoppingCartDatabase`-Methode auf (die im nächsten Schritt hinzugefügt und erläutert wird), um dem Warenkorb Elemente hinzuzufügen oder daraus zu entfernen. Nachdem die Datenbank aktualisiert wurde, um die Aktualisierungen des Warenkorbs widerzuspiegeln, wird das **GridView** -Steuerelement auf der Einkaufswagen Seite aktualisiert, indem die `DataBind`-Methode für die **GridView**aufgerufen wird. Außerdem wird die Gesamtbestellmenge auf der Warenkorb-Seite aktualisiert, um die aktualisierte Liste der Elemente widerzuspiegeln.

### <a name="updating-and-removing-shopping-cart-items"></a>Aktualisieren und Entfernen von Warenkorb-Elementen

Auf der Seite *ShoppingCart. aspx* können Sie sehen, dass Steuerelemente hinzugefügt wurden, um die Menge eines Elements zu aktualisieren und ein Element zu entfernen. Fügen Sie nun den Code hinzu, mit dem diese Steuerelemente funktionieren.

1. Öffnen Sie in **Projektmappen-Explorer**die Datei *ShoppingCartActions.cs* im Ordner *Logic* .
2. Fügen Sie den folgenden Code, der in gelb hervorgehoben ist, der *ShoppingCartActions.cs* -Klassendatei hinzu   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

Die `UpdateShoppingCartDatabase`-Methode, die von der `UpdateCartItems`-Methode auf der Seite *ShoppingCart.aspx.cs* aufgerufen wird, enthält die Logik zum Aktualisieren oder Entfernen von Elementen aus dem Warenkorb. Die `UpdateShoppingCartDatabase`-Methode durchläuft alle Zeilen in der Liste der Einkaufswagen. Wenn ein waren Korb Element als entfernt markiert wurde oder die Menge kleiner als 1 ist, wird die `RemoveItem`-Methode aufgerufen. Andernfalls wird das Warenkorb-Element auf Updates geprüft, wenn die `UpdateItem`-Methode aufgerufen wird. Nachdem das waren Korb Element entfernt oder aktualisiert wurde, werden die Daten Bank Änderungen gespeichert.

Die `ShoppingCartUpdates` Struktur wird verwendet, um alle waren Korb Elemente aufzunehmen. Die `UpdateShoppingCartDatabase`-Methode verwendet die `ShoppingCartUpdates`-Struktur, um zu bestimmen, ob eines der Elemente aktualisiert oder entfernt werden muss.

Im nächsten Tutorial verwenden Sie die `EmptyCart`-Methode, um den Warenkorb nach dem Erwerb von Produkten zu löschen. Aber vorerst verwenden Sie die `GetCount`-Methode, die Sie soeben der *ShoppingCartActions.cs* -Datei hinzugefügt haben, um zu bestimmen, wie viele Elemente sich im Warenkorb befinden.

### <a name="adding-a-shopping-cart-counter"></a>Hinzufügen eines Einkaufswagen Zählers

Um dem Benutzer die Anzeige der Gesamtzahl der Elemente im Warenkorb zu ermöglichen, fügen Sie der Seite *Site. Master* einen Gegenstand hinzu. Dieser Counter fungiert auch als Link zum Warenkorb.

1. Öffnen Sie in **Projektmappen-Explorer**die Seite *Site. Master* .
2. Ändern Sie das Markup, indem Sie den Link für den Warenkorb des Warenkorbs dem Navigations Abschnitt hinzufügen, sodass er wie folgt aussieht:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Aktualisieren Sie anschließend den Code Behind der *Site.Master.cs* -Datei, indem Sie den in gelb markierten Code wie folgt hinzufügen:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Bevor die Seite als HTML gerendert wird, wird das `Page_PreRender`-Ereignis ausgelöst. Im `Page_PreRender`-Handler wird die Gesamtanzahl der Warenkorb durch Aufrufen der `GetCount`-Methode bestimmt. Der zurückgegebene Wert wird der `cartCount` Spanne hinzugefügt, die im Markup der *Site. Master* -Seite enthalten ist. Mit den `<span>`-Tags können innere Elemente ordnungsgemäß gerendert werden. Wenn eine Seite der Website angezeigt wird, wird der Gesamt Warenkorb angezeigt. Der Benutzer kann auch auf den Warenkorb klicken, um den Warenkorb anzuzeigen.

## <a name="testing-the-completed-shopping-cart"></a>Testen des abgeschlossenen Warenkorbs

Sie können die Anwendung jetzt ausführen, um zu erfahren, wie Sie Elemente im Warenkorb hinzufügen, löschen und aktualisieren können. Der Einkaufswagen Gesamt zeigt die Gesamtkosten aller Produkte im Warenkorb an.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Der Browser wird geöffnet und zeigt die Seite *default. aspx* an.
2. Wählen Sie im Navigationsmenü der Kategorie die Option **Autos** aus.
3. Klicken Sie neben dem ersten Produkt auf den Link **zum Warenkorb hinzufügen** .   
 Die Seite " *ShoppingCart. aspx* " wird mit dem Gesamtbetrag der Bestellung angezeigt.
4. Wählen Sie im Navigationsmenü der Kategorie die Option **Ebenen** aus.
5. Klicken Sie neben dem ersten Produkt auf den Link **zum Warenkorb hinzufügen** .
6. Legen Sie die Menge des ersten Elements im Warenkorb auf 3 fest, und aktivieren Sie das Kontrollkästchen **Element entfernen** des zweiten Elements.<a id="a"></a>
7. Klicken Sie auf die Schaltfläche **Aktualisieren** , um die Warenkorb-Seite zu aktualisieren und die neue Bestellsumme anzuzeigen. 

    ![Warenkorb-Warenkorb-Update](shopping-cart/_static/image9.png)

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie einen Warenkorb für die Beispielanwendung Wingtip Toys Web Forms erstellt. In diesem Tutorial haben Sie Entity Framework Code First, Daten Anmerkungen, stark typisierte Daten Steuerelemente und Modell Bindung verwendet.

Der Warenkorb unterstützt das Hinzufügen, löschen und Aktualisieren von Elementen, die der Benutzer für den Kauf ausgewählt hat. Zusätzlich zur Implementierung der Warenkorb-Funktion haben Sie erfahren, wie Sie waren Korb Elemente in einem **GridView** -Steuerelement anzeigen und die Bestellsumme berechnen.

Um zu verstehen, wie die beschriebene Funktionalität in einer realen Geschäftsanwendung funktioniert, können Sie das Beispiel für [nopcommerce](https://github.com/nopSolutions/nopCommerce) -ASP.net Based Open Source-e-Commerce-Einkaufswagen anzeigen. Ursprünglich war es auf Web Forms und im Laufe der Jahre aufgebaut, das es in MVC verlagert hat und jetzt ASP.net Core.

## <a name="addition-information"></a>Zusätzliche Informationen

[Überblick über den ASP.NET-Sitzungsstatus](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Zurück](display_data_items_and_details.md)
> [Weiter](checkout-and-payment-with-paypal.md)
