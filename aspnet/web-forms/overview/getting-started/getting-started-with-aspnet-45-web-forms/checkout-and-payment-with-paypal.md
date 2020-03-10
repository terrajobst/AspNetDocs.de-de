---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Auschecken und bezahlen mit PayPal | Microsoft-Dokumentation
author: Erikre
description: Diese tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78455931"
---
# <a name="checkout-and-payment-with-paypal"></a>Bezahlvorgang und Zahlung mit PayPal

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für das Web. Für diese tutorialreihe steht ein Visual Studio 2013- [Projekt mit C# Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) zur Verfügung.

In diesem Tutorial wird beschrieben, wie Sie die Wingtip Toys-Beispielanwendung so ändern, dass Sie die Benutzer Autorisierung, Registrierung und Zahlung mithilfe von PayPal einschließt. Nur Benutzer, die angemeldet sind, verfügen über die Autorisierung zum Kauf von Produkten. Die integrierte Benutzer Registrierungsfunktion der Projektvorlage "ASP.NET 4,5 Web Forms" enthält bereits einen Großteil der benötigten Elemente. Sie werden die PayPal Express Checkout-Funktion hinzufügen. In diesem Tutorial verwenden Sie die PayPal-Testumgebung für Entwickler, sodass keine tatsächlichen Beträge übertragen werden. Am Ende des Tutorials testen Sie die Anwendung, indem Sie Produkte auswählen, die dem Warenkorb hinzugefügt werden sollen, indem Sie auf die Schaltfläche "Auschecken" klicken und Daten auf die Website "PayPal testing" übertragen. Auf der Website für die PayPal-Tests bestätigen Sie Ihre Versand-und Zahlungsinformationen, und kehren Sie dann zur lokalen Wingtip Toys-Beispielanwendung zurück, um den Kauf zu bestätigen und abzuschließen.

Es gibt mehrere erfahrene Zahlungsprozessoren von Drittanbietern, die sich auf den Online Einkauf spezialisiert haben, der die Skalierbarkeit und Sicherheit adressiert. ASP.NET-Entwickler sollten die Vorteile der Verwendung einer Drittanbieter-Zahlungslösung berücksichtigen, bevor Sie eine Einkaufs-und Kauf Lösung implementieren.

> [!NOTE] 
> 
> Die Wingtip Toys-Beispielanwendung wurde entwickelt, um spezifische ASP.net-Konzepte und Features anzuzeigen, die für ASP.NET-Webentwickler zur Verfügung stehen. Diese Beispielanwendung wurde nicht für alle möglichen Umstände in Bezug auf die Skalierbarkeit und Sicherheit optimiert.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Einschränken des Zugriffs auf bestimmte Seiten in einem Ordner.
- Erstellen eines bekannten Warenkorbs aus einem anonymen Warenkorb
- Aktivieren von SSL für das Projekt.
- Vorgehensweise beim Hinzufügen eines OAuth-Anbieters zum Projekt.
- Verwenden von PayPal zum Erwerb von Produkten mithilfe der PayPal-Testumgebung.
- Anzeigen von Details aus PayPal in einem **DetailsView** -Steuerelement
- Aktualisieren der Datenbank der Wingtip Toys-Anwendung mit Details, die von PayPal abgerufen wurden.

## <a name="adding-order-tracking"></a>Hinzufügen der Auftrags Überwachung

In diesem Tutorial erstellen Sie zwei neue Klassen zum Nachverfolgen von Daten aus der Reihenfolge, die ein Benutzer erstellt hat. Die Klassen verfolgen Daten zu Versandinformationen, Kauf Summe und Zahlungsbestätigung.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Hinzufügen der Order-und Order Detail-Modellklassen

Weiter oben in dieser tutorialreihe haben Sie das Schema für Kategorien, Produkte und waren Korb Elemente definiert, indem Sie die Klassen `Category`, `Product`und `CartItem` im Ordner *Modelle* erstellt haben. Nun fügen Sie zwei neue Klassen hinzu, um das Schema für die Produktbestellung und die Details der Bestellung zu definieren.

1. Fügen Sie im Ordner **Models** eine neue Klasse mit dem Namen *Order.cs*hinzu.   
   Die neue Klassendatei wird im Editor angezeigt.
2. Ersetzen Sie den Standard Code durch den folgenden Code:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Fügen Sie dem Ordner " *Models* " eine *OrderDetail.cs* -Klasse hinzu.
4. Ersetzen Sie den Standardcode durch den folgenden Code:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Die Klassen `Order` und `OrderDetail` enthalten das Schema zum Definieren der Auftrags Informationen, die für den Einkauf und Versand verwendet werden.

Außerdem müssen Sie die Daten Bank Kontext Klasse aktualisieren, die die Entitäts Klassen verwaltet und den Datenzugriff auf die Datenbank ermöglicht. Zu diesem Zweck fügen Sie der Klasse `ProductContext` Klasse die neu erstellten Order-und `OrderDetail` Modellklassen hinzu.

1. Suchen und öffnen Sie in **Projektmappen-Explorer**die Datei *ProductContext.cs* .
2. Fügen Sie der Datei *ProductContext.cs* den markierten Code hinzu, wie unten gezeigt:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Wie bereits in dieser tutorialreihe erwähnt, fügt der Code in der *ProductContext.cs* -Datei den `System.Data.Entity`-Namespace hinzu, damit Sie auf alle Kernfunktionen des Entity Framework zugreifen können. Diese Funktion bietet die Möglichkeit, Daten durcharbeiten mit stark typisierten Objekten abzufragen, einzufügen, zu aktualisieren und zu löschen. Der obige Code in der `ProductContext`-Klasse fügt Entity Framework Zugriff auf die neu hinzugefügten `Order`-und `OrderDetail`-Klassen hinzu.

## <a name="adding-checkout-access"></a>Hinzufügen des Checkout Zugriffs

Die Wingtip Toys-Beispielanwendung ermöglicht anonymen Benutzern das überprüfen und Hinzufügen von Produkten zu einem Einkaufswagen. Wenn anonyme Benutzer jedoch entscheiden, die Produkte zu kaufen, die Sie dem Warenkorb hinzugefügt haben, müssen Sie sich an der Website anmelden. Nachdem Sie sich angemeldet haben, können Sie auf die eingeschränkten Seiten der Webanwendung zugreifen, die den Checkout-und Kaufprozess verarbeiten. Diese eingeschränkten Seiten sind im Ordner *Checkout* der Anwendung enthalten.

### <a name="add-a-checkout-folder-and-pages"></a>Hinzufügen eines Checkout Ordners und von Seiten

Nun erstellen Sie den Ordner " *Checkout* " und die Seiten darin, die dem Kunden während des Auscheck Vorgangs angezeigt werden. Diese Seiten werden später in diesem Tutorial aktualisiert.

1. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen (**Wingtip Toys**), und wählen Sie **neuen Ordner hinzufügen**aus. 

    ![Auschecken und bezahlen mit PayPal: neuer Ordner](checkout-and-payment-with-paypal/_static/image1.png)
2. Benennen Sie den neuen *Ordner.*
3. Klicken Sie mit der rechten Maustaste auf den Ordner *Checkout* , und wählen Sie-&gt;**Neues Element** **Hinzufügen** aus. 

    ![Auschecken und bezahlen mit PayPal-neues Element](checkout-and-payment-with-paypal/_static/image2.png)
4. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
5. Wählen Sie auf der linken Seite die Gruppe **Visual C#**  -&gt; **Web** Templates aus. Wählen Sie dann im mittleren Bereich die Option **Web Form mit Master Seite**aus, und nennen Sie Sie *checkoutstart. aspx*. 

    ![Auschecken und bezahlen mit PayPal-Dialog Feld "Neues Element hinzufügen](checkout-and-payment-with-paypal/_static/image3.png)
6. Wählen Sie wie zuvor die Datei *Site. Master* als Master Seite aus.
7. Fügen Sie dem Ordner *Checkout* die folgenden zusätzlichen Seiten hinzu, indem Sie die oben genannten Schritte ausführen:   

    - Checkeinview. aspx
    - Checkoutcomplete. aspx
    - Checkoutcancel. aspx
    - Checkouterror. aspx

### <a name="add-a-webconfig-file"></a>Fügen Sie eine Web. config-Datei hinzu.

Wenn Sie dem *Checkout* Ordner eine neue Datei " *Web. config* " hinzufügen, können Sie den Zugriff auf alle im Ordner enthaltenen Seiten einschränken.

1. Klicken Sie mit der rechten Maustaste auf den Ordner *Checkout* , und wählen Sie **Hinzufügen** -&gt; **Neues Element**  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie auf der linken Seite die Gruppe **Visual C#**  -&gt; **Web** Templates aus. Wählen Sie dann im mittleren Bereich **Webkonfigurationsdatei**aus, übernehmen Sie den Standardnamen " *Web. config*", und wählen Sie dann **Hinzufügen**aus.
3. Ersetzen Sie den vorhandenen XML-Inhalt in der Datei *Web.config* durch den folgenden Code:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Speichern Sie die Datei *Web.config* .

Die Datei " *Web. config* " gibt an, dass allen unbekannten Benutzern der Webanwendung der Zugriff auf die im Ordner " *Checkout* " enthaltenen Seiten verweigert werden muss. Wenn der Benutzer jedoch ein Konto registriert hat und angemeldet ist, handelt es sich um einen bekannten Benutzer, der Zugriff auf die Seiten im Ordner " *Checkout* " hat.

Beachten Sie, dass die ASP.NET-Konfiguration auf eine Hierarchie folgt, wobei jede *Web. config* -Datei Konfigurationseinstellungen auf den Ordner anwendet, in dem Sie sich befindet, sowie auf alle untergeordneten Verzeichnisse darunter.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Aktivieren von SSL für das Projekt

 Secure Sockets Layer (SSL) ist ein Protokoll, das Webservern und Webclients eine sichere Kommunikation durch Verschlüsselung ermöglicht. Wird kein SSL verwendet, können die zwischen Client und Server gesendeten Daten von jedem mit physischem Zugriff auf das Netzwerk per Paket-Sniffing ausgespäht werden. Außerdem sind verschiedene häufig verwendete Authentifizierungsschemas über einfaches HTTP nicht sicher. Insbesondere senden die einfache Authentifizierung und Formularauthentifizierung unverschlüsselte Anmeldeinformationen. Aus Sicherheitsgründen sollten diese Authentifizierungsschemas SSL verwenden. 

1. Klicken Sie in **Projektmappen-Explorer**auf das Projekt **wingtiptoys** , und drücken Sie dann **F4** , um das Fenster **Eigenschaften** anzuzeigen.
2. Ändern Sie **SSL** in `true`.
3. Kopieren Sie die **SSL-URL** , damit Sie sie später verwenden können.   
 Die SSL-URL wird `https://localhost:44300/`, es sei denn, Sie haben zuvor SSL-Websites erstellt (wie unten gezeigt).   
    ![Projekteigenschaften](checkout-and-payment-with-paypal/_static/image4.png)
4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **wingtiptoys** und dann auf **Eigenschaften**.
5. Klicken Sie auf der linken Registerkarte auf **Web**.
6. Ändern Sie die **Projekt-URL** , um die zuvor gespeicherte SSL- **URL** zu verwenden.   
    ![Projekt-Webeigenschaften](checkout-and-payment-with-paypal/_static/image5.png)
7. Drücken Sie **STRG+S**, um die Seite zu speichern.
8. Drücken Sie **STRG+F5** , um die Anwendung auszuführen. Visual Studio zeigt eine Option an, die es Ihnen ermöglicht, SSL-Warnungen zu vermeiden.
9. Klicken Sie auf **Ja** , um dem IIS Express SSL-Zertifikat zu vertrauen und fortzufahren.   
    ![IIS Express SSL-Zertifikatdetails](checkout-and-payment-with-paypal/_static/image6.png)  
 Es wird eine Sicherheitswarnung angezeigt.
10. Klicken Sie auf **Ja** , um das Zertifikat zu Ihrem Localhost zu installieren.   
    ![Dialogfeld "Sicherheitswarnung"](checkout-and-payment-with-paypal/_static/image7.png)  
 Das Browserfenster wird angezeigt.

Sie können Ihre Webanwendung jetzt problemlos lokal mit SSL testen.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Hinzufügen eines OAuth 2.0-Anbieters

ASP.NET Web Forms bietet erweiterte Optionen für Mitgliedschaft und Authentifizierung. Diese Erweiterungen umfassen OAuth. OAuth ist ein offenes Protokoll, das die sichere Autorisierung in einer einfachen und Standardmethode von Web-, Mobil- und Desktopanwendungen ermöglicht. Die ASP.net-Web Forms Vorlage verwendet OAuth, um Facebook, Twitter, Google und Microsoft als Authentifizierungs Anbieter verfügbar zu machen. Auch wenn in diesem Lernprogramm nur Google als Authentifizierungsanbieter eingesetzt wird, können Sie den Code problemlos für die Verwendung einer der anderen Anbieter anpassen. Die Schritte zur Implementierung anderer Anbieter unterscheiden sich kaum von den Schritten in diesem Lernprogramm.

Abgesehen von der Authentifizierung werden in diesem Lernprogramm auch Rollen verwendet, um die Autorisierung zu implementieren. Nur Benutzer, die Sie der Rolle `canEdit` hinzufügen, können Daten ändern (Kontakte erstellen, bearbeiten oder löschen.

> [!NOTE] 
> 
> Windows Live-Anwendungen akzeptieren nur eine Live-URL für eine funktionierende Website, sodass Sie keine lokale Website-URL zum Testen von Anmeldungen verwenden können.

Mit den folgenden Schritten können Sie einen Google-Authentifizierungsanbieter hinzufügen.

1. Öffnen Sie die Datei *App\_Start\Startup.auth.cs* .
2. Entfernen Sie die Kommentarzeichen aus der `app.UseGoogleAuthentication()` -Methode, sodass sie wie folgt aussieht: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navigieren Sie zur [Google Developers Console](https://console.developers.google.com/). Außerdem müssen Sie sich mit Ihrem Google Entwickler-E-Mail-Konto anmelden (gmail.com). Wenn Sie nicht über ein Google-Konto verfügen, wählen Sie den Link **Konto erstellen** aus.   
   Danach sehen Sie die **Google Entwickler-Konsole**.   
    ![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)
4. Klicken Sie auf die Schaltfläche **Projekt erstellen** , und geben Sie einen Projektnamen und eine ID ein (Sie können die Standardwerte verwenden). Klicken Sie dann auf das **Kontrollkästchen Vereinbarung** und die Schaltfläche **Erstellen** .  

    ![Google-neues Projekt](checkout-and-payment-with-paypal/_static/image9.png)

   In wenigen Sekunden wird das neue Projekt erstellt, und Ihr Browser zeigt die Seite für neue Projekte an.
5. Klicken Sie auf der linken Registerkarte auf **APIs &amp;** Authentifizierung, und klicken Sie dann auf **Anmelde**Informationen.
6. Klicken Sie unter **OAuth**auf die **neue Client-ID erstellen** .   
   Das Dialogfeld **Client-ID erstellen** wird angezeigt.   
    ![Google - Client-ID erstellen](checkout-and-payment-with-paypal/_static/image10.png)
7. Behalten Sie im Dialogfeld **Client-ID erstellen** die **Standardweb Anwendung** für den Anwendungstyp bei.
8. Legen Sie die **autorisierten JavaScript-Ursprünge** auf die SSL-URL fest, die Sie zuvor in diesem Tutorial verwendet haben (`https://localhost:44300/`, es sei denn, Sie haben andere SSL-Projekte   
   Diese URL ist der Ursprung für Ihre Anwendung. In diesem Beispiel geben Sie nur die Test-URL "localhost" ein. Sie können jedoch mehrere URLs eingeben, um localhost und die Produktion zu berücksichtigen.
9. Legen Sie **Authorized Redirect URI** folgendermaßen fest: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Dieser Wert steht für den URI, den ASP.NET OAuth-Benutzer zur Kommunikation mit dem Google OAuth-Server verwenden. Merken Sie sich die zuvor verwendete SSL-URL (`https://localhost:44300/`, es sei denn, Sie haben andere SSL-Projekte erstellt).
10. Klicken Sie auf die Schaltfläche **Client-ID erstellen** .
11. Klicken Sie im linken Menü der Google Developers Console auf das Menü Element **Zustimmungs Bildschirm** , und legen Sie dann Ihre e-Mail-Adresse und den Produktnamen fest. Wenn Sie das Formular ausgefüllt haben, klicken Sie auf **Speichern**.
12. Klicken Sie auf das Menü Element **APIs** , Scrollen Sie nach unten, und klicken Sie auf die Schaltfläche **aus** neben **Google + API**.   
    Wenn Sie diese Option akzeptieren, wird die Google +-API aktiviert.
13. Außerdem müssen Sie das nuget-Paket **Microsoft. owin** auf Version 3.0.0 aktualisieren.   
    Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager** , und wählen Sie dann **nuget-Pakete für**Projekt Mappe verwalten aus.  
    Suchen Sie im Fenster **nuget-Pakete verwalten** das Paket **Microsoft. owin** auf Version 3.0.0, und aktualisieren Sie es.
14. Aktualisieren Sie in Visual Studio die `UseGoogleAuthentication`-Methode der *Startup.auth.cs* -Seite, indem Sie die **Client-ID** und den **geheimen Client** Schlüssel in die-Methode kopieren und einfügen. Die unten gezeigten Werte für **Client-ID** und **geheimer Client** Schlüssel sind Beispiele und können nicht verwendet werden. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Drücken Sie **STRG+F5** , um die Anwendung zu erstellen und auszuführen. Klicken Sie auf den Link **Log in** .
16. Klicken Sie unter **anderen Dienst zum Anmelden verwenden auf** **Google**.  
    ![Anmelden](checkout-and-payment-with-paypal/_static/image11.png)
17. Wenn Sie Ihre Anmeldeinformationen eingeben müssen, werden Sie zur Google-Website umgeleitet, wo Sie Ihre Anmeldeinformationen eingeben.  
    ![Google - Anmeldung](checkout-and-payment-with-paypal/_static/image12.png)
18. Nachdem Sie Ihre Anmelde Informationen eingegeben haben, werden Sie aufgefordert, der soeben erstellten Webanwendung Berechtigungen zu geben.  
    ![Standard-Dienstkonto für Projekt](checkout-and-payment-with-paypal/_static/image13.png)
19. Klicken Sie auf **Annehmen**. Sie werden nun zurück zur **Register** Seite der **wingtiptoys** -Anwendung umgeleitet, in der Sie Ihr Google-Konto registrieren können.  
    ![Registrieren Sie sich mit Ihrem Google-Konto](checkout-and-payment-with-paypal/_static/image14.png)
20. Sie können zwar den lokalen E-Mail-Registrierungsnamen in Ihr Gmail-Konto ändern, im Allgemeinen sollten Sie jedoch den Standard-E-Mail-Aliasnamen beibehalten (den Sie für die Authentifizierung verwendet haben). Klicken Sie auf **Anmelden** , wie oben gezeigt.

### <a name="modifying-login-functionality"></a>Ändern der Anmelde Funktionalität

Wie bereits in dieser tutorialreihe erwähnt, ist der Großteil der Benutzer Registrierungsfunktionen standardmäßig in der ASP.net-Web Forms Vorlage enthalten. Nun ändern Sie die standardmäßigen *Login. aspx* -und *Register. aspx* -Seiten, um die `MigrateCart`-Methode aufzurufen. Die `MigrateCart`-Methode ordnet einem neuen angemeldeten Benutzer einen anonymen Einkaufswagen zu. Durch Zuordnen des Benutzers und Warenkorb kann die Wingtip Toys-Beispielanwendung den Einkaufswagen des Benutzers zwischen den besuchen verwalten.

1. Suchen Sie in **Projektmappen-Explorer**den *Konto* Ordner, und öffnen Sie ihn.
2. Ändern Sie die Code Behind-Seite mit dem Namen *Login.aspx.cs* , um den in gelb markierten Code einzuschließen, sodass Sie wie folgt aussieht:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Speichern Sie die Datei *Login.aspx.cs* .

Vorerst können Sie die Warnung ignorieren, dass keine Definition für die `MigrateCart`-Methode vorhanden ist. Sie werden Sie später in diesem Tutorial hinzufügen.

Die *Login.aspx.cs* -Code Behind-Datei unterstützt eine Anmelde Methode. Durch die Überprüfung der Login. aspx-Seite sehen Sie, dass diese Seite eine Schaltfläche "Anmelden" enthält, die beim Klicken auf den `LogIn` Handler im Code Behind auslöst.

Wenn die `Login`-Methode in der *Login.aspx.cs* -Methode aufgerufen wird, wird eine neue Instanz des Warenkorbs namens `usersShoppingCart` erstellt. Die ID des Warenkorbs (eine GUID) wird abgerufen und auf die `cartId` Variable festgelegt. Anschließend wird die `MigrateCart`-Methode aufgerufen, wobei sowohl der `cartId` als auch der Name des angemeldeten Benutzers an diese Methode übergeben werden. Wenn der Warenkorb migriert wird, wird die GUID, die zum Identifizieren des anonymen Warenkorbs verwendet wird, durch den Benutzernamen ersetzt.

Zusätzlich zum Ändern der *Login.aspx.cs* -Code Behind-Datei zum Migrieren des Warenkorbs, wenn sich der Benutzer anmeldet, müssen Sie auch die *Register.aspx.cs-Code Behind-Datei* ändern, um den Warenkorb zu migrieren, wenn der Benutzer ein neues Konto erstellt und sich anmeldet.

1. Öffnen Sie im *Konto* Ordner die Code-Behind-Datei mit dem Namen *Register.aspx.cs*.
2. Ändern Sie die Code Behind-Datei, indem Sie den Code in gelb einschließen, damit Sie wie folgt aussieht:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Speichern Sie die Datei *Register.aspx.cs* . Ignorieren Sie die Warnung über die `MigrateCart` Methode.

Beachten Sie, dass der Code, den Sie im `CreateUser_Click` Ereignishandler verwendet haben, dem Code sehr ähnlich ist, den Sie in der `LogIn`-Methode verwendet haben. Wenn sich der Benutzer registriert oder bei der Website anmeldet, wird ein `MigrateCart` Methode aufgerufen.

## <a name="migrating-the-shopping-cart"></a>Migrieren des Warenkorbs

Nachdem Sie nun den Anmelde-und Registrierungsprozess aktualisiert haben, können Sie den Code zum Migrieren des Warenkorbs mithilfe der `MigrateCart`-Methode hinzufügen.

1. Suchen Sie in **Projektmappen-Explorer**den Ordner " *Logic* ", und öffnen Sie die *ShoppingCartActions.cs* -Klassendatei.
2. Fügen Sie den in gelb markierten Code dem vorhandenen Code in der *ShoppingCartActions.cs* -Datei hinzu, sodass der Code in der *ShoppingCartActions.cs* -Datei wie folgt aussieht:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Die `MigrateCart`-Methode verwendet die vorhandene "cartId", um den Warenkorb des Benutzers zu suchen. Als nächstes durchläuft der Code alle waren Korb Elemente und ersetzt die `CartId`-Eigenschaft (wie durch das `CartItem` Schema angegeben) durch den angemeldeten Benutzernamen.

### <a name="updating-the-database-connection"></a>Aktualisieren der Datenbankverbindung

Wenn Sie dieses Tutorial mithilfe der **vorgefertigten** Wingtip Toys-Beispielanwendung ausführen, müssen Sie die Standard-Mitgliedschafts Datenbank neu erstellen. Durch Ändern der Standard Verbindungs Zeichenfolge wird die Mitgliedschafts Datenbank erstellt, wenn die Anwendung das nächste Mal ausgeführt wird.

1. Öffnen Sie die Datei " *Web. config* " im Stammverzeichnis des Projekts.
2. Aktualisieren Sie die Standard Verbindungs Zeichenfolge so, dass Sie wie folgt aussieht:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrieren von PayPal

Bei PayPal handelt es sich um eine webbasierte Abrechnungs Plattform, die Zahlungen durch Onlinehändler akzeptiert. In diesem Tutorial wird als nächstes erläutert, wie Sie die Express Checkout-Funktion von PayPal in Ihre Anwendung integrieren. Express Checkout ermöglicht Ihren Kunden die Verwendung von PayPal zum bezahlen der Elemente, die Sie dem Warenkorb hinzugefügt haben.

### <a name="create-paypal-test-accounts"></a>Erstellen von Paypal-Test Konten

Um die PayPal-Testumgebung zu verwenden, müssen Sie ein Entwickler Testkonto erstellen und überprüfen. Sie verwenden das Test Konto des Entwicklers, um ein Käufer Testkonto und ein Verkäufer Testkonto zu erstellen. Die Anmelde Informationen für das Entwickler-Test Konto ermöglichen der Wingtip Toys-Beispielanwendung auch den Zugriff auf die PayPal-Testumgebung.

1. Navigieren Sie in einem Browser zur Website "PayPal-Entwickler Test":   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Wenn Sie nicht über ein PayPal-Entwicklerkonto verfügen, erstellen Sie ein neues Konto, indem Sie auf **registrieren**klicken, und befolgen Sie die Schritte zur Registrierung. Wenn Sie über ein vorhandenes PayPal-Entwicklerkonto verfügen, melden Sie sich an, indem Sie auf **Anmelden**klicken. Sie benötigen Ihr PayPal-Entwicklerkonto, um die Wingtip Toys-Beispielanwendung später in diesem Tutorial zu testen.
3. Wenn Sie sich gerade für Ihr PayPal-Entwicklerkonto registriert haben, müssen Sie möglicherweise Ihr PayPal-Entwicklerkonto mit PayPal verifizieren. Sie können Ihr Konto überprüfen, indem Sie die Schritte befolgen, die an Ihr e-Mail-Konto gesendet wurden. Nachdem Sie Ihr PayPal-Entwicklerkonto überprüft haben, melden Sie sich wieder bei der Website für die PayPal-Entwickler Tests an.
4. Nachdem Sie sich mit Ihrem PayPal-Entwicklerkonto bei der PayPal-Entwickler Website angemeldet haben, müssen Sie ein PayPal-Käufer Testkonto erstellen, falls Sie noch keines besitzen. Um ein Käufer Testkonto zu erstellen, klicken Sie auf der Website PayPal auf die Registerkarte **Anwendungen** , und klicken Sie dann auf **Sandbox-Konten**.   
 Die Seite **Sandbox-Testkonten** wird angezeigt.   

    > [!NOTE] 
    > 
    > Die Website für die PayPal-Entwickler stellt bereits ein Händler Testkonto bereit.

    ![Auschecken und bezahlen mit PayPal-Sandbox-Testkonten](checkout-and-payment-with-paypal/_static/image15.png)
5. Klicken Sie auf der Seite Sandbox-Testkonten auf **Konto erstellen**.
6. Wählen Sie auf der Seite **Testkonto erstellen** eine e-Mail und ein Kennwort Ihrer Wahl für das Käufer Test Konto aus.   

    > [!NOTE] 
    > 
    > Sie benötigen die Käufer-e-Mail-Adressen und das Kennwort, um die Wingtip Toys-Beispielanwendung am Ende dieses Tutorials zu testen.

    ![Auschecken und bezahlen mit PayPal-Sandbox-Testkonten](checkout-and-payment-with-paypal/_static/image16.png)
7. Erstellen Sie das Käufer Testkonto, indem Sie auf die Schaltfläche **Konto erstellen** klicken.  
 Die Seite **Sandbox-Test Konten** wird angezeigt. 

    ![Auschecken und bezahlen mit PayPal-PayPal-Konten](checkout-and-payment-with-paypal/_static/image17.png)
8. Klicken Sie auf der Seite **Sandbox-Testkonten** auf das e-Mail-Konto für den **Vermittler** .  
    **Profil** -und **Benachrichtigungs** Optionen werden angezeigt.
9. Wählen Sie die Option **Profil** aus, und klicken Sie dann auf **API-Anmelde** Informationen, um Ihre API-Anmelde Informationen für das Händler Testkonto
10. Kopieren Sie die Test-API-Anmelde Informationen in Notepad.

Sie benötigen die angezeigten klassischen Test-API-Anmelde Informationen (Benutzername, Kennwort und Signatur), um API-Aufrufe von der Wingtip Toys-Beispielanwendung an die PayPal-Testumgebung vorzunehmen. Die Anmelde Informationen werden im nächsten Schritt hinzugefügt.

### <a name="add-paypal-class-and-api-credentials"></a>Hinzufügen von PayPal-und API-Anmelde Informationen

Der Großteil des PayPal-Codes wird in einer einzigen Klasse platziert. Diese Klasse enthält die Methoden, die für die Kommunikation mit PayPal verwendet werden. Außerdem fügen Sie dieser Klasse Ihre PayPal-Anmelde Informationen hinzu.

1. Klicken Sie in der Wingtip Toys-Beispielanwendung in Visual Studio mit der rechten Maustaste auf den Ordner **Logic** , und wählen Sie dann -&gt; **Neues Element** **Hinzufügen** aus.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie unter  **C# Visual** im Bereich **installiert** auf der linken Seite die Option **Code**aus.
3. Wählen Sie im mittleren Bereich die Option **Klasse**aus. Benennen Sie diese neue Klasse **PayPalFunctions.cs**.
4. Klicken Sie auf **Hinzufügen**.  
   Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch den folgenden Code:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Fügen Sie die Anmelde Informationen für die Händler-API (Benutzername, Kennwort und Signatur) hinzu, die Sie zuvor in diesem Tutorial angezeigt haben, damit Sie Funktionsaufrufe an die PayPal-Testumgebung durchführen können.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> In dieser Beispielanwendung fügen Sie einfach Anmelde Informationen zu einer C# Datei (. cs) hinzu. In einer implementierten Lösung sollten Sie jedoch in Erwägung gezogen werden, Ihre Anmelde Informationen in einer Konfigurationsdatei zu verschlüsseln.

Die nvpapicaller-Klasse enthält die Mehrzahl der PayPal-Funktionen. Der Code in der-Klasse stellt die Methoden bereit, die erforderlich sind, um einen Test Einkauf aus der PayPal-Testumgebung zu tätigen. Zum tätigen von Käufen werden die folgenden drei PayPal-Funktionen verwendet:

- `SetExpressCheckout`-Funktion
- `GetExpressCheckoutDetails`-Funktion
- `DoExpressCheckoutPayment`-Funktion

Die `ShortcutExpressCheckout`-Methode sammelt die Test Kauf Informationen und Produktdetails aus dem Warenkorb und ruft die `SetExpressCheckout` PayPal-Funktion auf. Die `GetCheckoutDetails`-Methode bestätigt Kauf Details und ruft die `GetExpressCheckoutDetails` PayPal-Funktion auf, bevor der Test gekauft wird. Die `DoCheckoutPayment`-Methode schließt den Test Kauf aus der Testumgebung ab, indem die `DoExpressCheckoutPayment` PayPal-Funktion aufgerufen wird. Der verbleibende Code unterstützt die PayPal-Methoden und-Prozesse, z. b. Codieren von Zeichen folgen, Decodieren von Zeichen folgen, Verarbeiten von Arrays und

> [!NOTE] 
> 
> Mithilfe von PayPal können Sie optionale Kauf Details basierend auf [der API-Spezifikation von PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)einschließen. Indem Sie den Code in der Wingtip Toys-Beispielanwendung erweitern, können Sie Lokalisierungs Details, Produktbeschreibungen, Steuern, eine Kundendienstnummer sowie viele weitere optionale Felder einschließen.

Beachten Sie, dass die Rückgabe-und Abbruch-URLs, die in der **shortcutexpresscheckout** -Methode angegeben sind, eine Portnummer verwenden.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Wenn Visual Web Developer ein Webprojekt mit SSL ausführt, wird in der Regel der Port 44300 für den Webserver verwendet. Wie oben gezeigt, lautet die Portnummer 44300. Wenn Sie die Anwendung ausführen, wird möglicherweise eine andere Portnummer angezeigt. Die Portnummer muss im Code korrekt festgelegt werden, damit Sie die Wingtip Toys-Beispielanwendung am Ende dieses Tutorials erfolgreich ausführen können. Im nächsten Abschnitt dieses Tutorials wird erläutert, wie die Portnummer des lokalen Hosts abgerufen und die PayPal-Klasse aktualisiert wird.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aktualisieren der localhost-Port Nummer in der PayPal-Klasse

Die Wingtip Toys-Beispielanwendung kauft Produkte, indem Sie zur Website für die PayPal-Tests navigieren und Ihre lokale Instanz der Wingtip Toys-Beispielanwendung zurückgibt. Damit PayPal zur richtigen URL zurückkehren kann, müssen Sie die Portnummer der lokal laufenden Beispielanwendung in dem oben erwähnten Paypal-Code angeben.

1. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen (**wingtiptoys**), und wählen Sie **Eigenschaften**aus.
2. Wählen Sie in der linken Spalte die Registerkarte **Web** aus.
3. Rufen Sie die Portnummer aus dem Feld **Projekt-URL** ab.
4. Aktualisieren Sie bei Bedarf die `returnURL` und `cancelURL` in der PayPal-Klasse (`NVPAPICaller`) in der *PayPalFunctions.cs* -Datei, um die Portnummer Ihrer Webanwendung zu verwenden:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Nun entspricht der Code, den Sie hinzugefügt haben, dem erwarteten Port für Ihre lokale Webanwendung. PayPal kann zur richtigen URL auf dem lokalen Computer zurückkehren.

### <a name="add-the-paypal-checkout-button"></a>Hinzufügen der PayPal-Schaltfläche

Nachdem der Beispielanwendung die primären PayPal-Funktionen hinzugefügt wurden, können Sie damit beginnen, das Markup und den Code hinzuzufügen, die erforderlich sind, um diese Funktionen aufzurufen. Zuerst müssen Sie die Schaltfläche "Auschecken" hinzufügen, die dem Benutzer auf der Seite "Warenkorb" angezeigt wird.

1. Öffnen Sie die Datei " *ShoppingCart. aspx* ".
2. Scrollen Sie zum Ende der Datei, und suchen Sie den `<!--Checkout Placeholder -->` Kommentar.
3. Ersetzen Sie den Kommentar durch ein `ImageButton`-Steuerelement, sodass die Markierung wie folgt ersetzt wird:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. Fügen Sie in der Datei *ShoppingCart.aspx.cs* hinter dem `UpdateBtn_Click`-Ereignishandler in der Nähe des Datei Endes den `CheckOutBtn_Click`-Ereignishandler hinzu:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Fügen Sie in der Datei *ShoppingCart.aspx.cs* auch einen Verweis auf die `CheckoutBtn`hinzu, damit auf die Schaltfläche Neues Bild wie folgt verwiesen wird:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Speichern Sie die Änderungen in der Datei " *ShoppingCart. aspx* " und in der Datei " *ShoppingCart.aspx.cs* ".
7. Wählen Sie im Menü **Debuggen**-aus, um **wingtiptoys &gt;erstellen**.  
   Das Projekt wird mit dem neu hinzugefügten **ImageButton** -Steuerelement neu erstellt.

### <a name="send-purchase-details-to-paypal"></a>Kauf Details an PayPal senden

Wenn der Benutzer auf der Warenkorb-Seite (*ShoppingCart. aspx*) auf **die Schaltfläche** "Auschecken" klickt, beginnt er mit dem Kaufvorgang. Der folgende Code Ruft die erste PayPal-Funktion auf, die zum Erwerb von Produkten benötigt wird.

1. Öffnen Sie im Ordner *Checkout* die Code-Behind-Datei mit dem Namen *CheckoutStart.aspx.cs*.   
   Stellen Sie sicher, dass Sie die Code Behind-Datei öffnen.
2. Ersetzen Sie den vorhandenen Code durch folgenden Code:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Wenn der Benutzer der Anwendung auf der Warenkorb-Seite auf **die Schaltfläche** Auschecken klickt, wird der Browser zur *checkoutstart. aspx* -Seite navigiert. Wenn die *checkoutstart. aspx* -Seite geladen wird, wird die `ShortcutExpressCheckout`-Methode aufgerufen. An diesem Punkt wird der Benutzer auf die Website für das PayPal-testen übertragen. Auf der Website von PayPal gibt der Benutzer seine PayPal-Anmelde Informationen ein, überprüft die Kauf Details, akzeptiert die PayPal-Vereinbarung und kehrt zur Wingtip Toys-Beispielanwendung zurück, in der die `ShortcutExpressCheckout`-Methode abgeschlossen ist. Wenn die `ShortcutExpressCheckout`-Methode beendet ist, wird der Benutzer an die in der `ShortcutExpressCheckout`-Methode angegebene *checkeinview. aspx* -Seite umgeleitet. Dadurch kann der Benutzer die Bestelldetails innerhalb der Wingtip Toys-Beispielanwendung überprüfen.

### <a name="review-order-details"></a>Bestell Details überprüfen

Nach der Rückgabe von PayPal zeigt die Seite *checkstartview. aspx* der Wingtip Toys-Beispielanwendung die Bestelldetails an. Auf dieser Seite kann der Benutzer die Bestelldetails vor dem Erwerb der Produkte überprüfen. Die Seite *checkstartview. aspx* muss wie folgt erstellt werden:

1. Öffnen Sie im Ordner *Checkout* die Seite mit dem Namen *checkeinview. aspx*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Öffnen Sie die Code Behind-Seite mit dem Namen *CheckoutReview.aspx.cs* , und ersetzen Sie den vorhandenen Code durch Folgendes:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Das **DetailsView** -Steuerelement wird zum Anzeigen der Auftragsdetails verwendet, die von PayPal zurückgegeben wurden. Der obige Code speichert außerdem die Bestelldetails in der Wingtip Toys-Datenbank als `OrderDetail` Objekt. Wenn der Benutzer auf die Schaltfläche " **Bestellung vervollständigen** " klickt, wird er auf die Seite *checkoutcomplete. aspx* umgeleitet.

> [!NOTE] 
> 
> **Tipp**
> 
> Beachten Sie im Markup der Seite *checkstartview. aspx* , dass das `<ItemStyle>`-Tag verwendet wird, um den Stil der Elemente innerhalb des **DetailsView** -Steuer Elements am unteren Rand der Seite zu ändern. Wenn Sie die Seite in der **Entwurfs Ansicht** anzeigen (indem Sie in der unteren linken Ecke von Visual Studio die Option **Entwurf** auswählen), dann das **DetailsView** -Steuerelement auswählen und das **Smarttag** (das Pfeilsymbol oben rechts im Steuerelement) auswählen, können Sie die **DetailsView-Aufgaben**sehen.
> 
> ![Auschecken und bezahlen mit PayPal-Bearbeitungs Feldern](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Wenn Sie **Felder bearbeiten**auswählen, wird das Dialogfeld **Felder** angezeigt. In diesem Dialogfeld können Sie die visuellen Eigenschaften, z. b. **ItemStyle**, des **DetailsView** -Steuer Elements leicht steuern.
> 
> ![Dialog Feld "Auschecken und bezahlen mit PayPal-Fields](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Abschluss des Kaufs

Die Seite *checkoutcomplete. aspx* macht den Kauf von PayPal. Wie bereits erwähnt, muss der Benutzer auf die Schaltfläche " **Bestellung vervollständigen** " klicken, bevor die Anwendung zur *checkoutcomplete. aspx* -Seite navigiert.

1. Öffnen Sie im Ordner *Checkout* die Seite *checkoutcomplete. aspx*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Öffnen Sie die Code Behind-Seite mit dem Namen *CheckoutComplete.aspx.cs* , und ersetzen Sie den vorhandenen Code durch Folgendes:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Wenn die Seite *checkoutcomplete. aspx* geladen wird, wird die `DoCheckoutPayment`-Methode aufgerufen. Wie bereits erwähnt, schließt die `DoCheckoutPayment`-Methode den Kauf aus der PayPal-Testumgebung ab. Nachdem PayPal den Kauf der Bestellung abgeschlossen hat, wird auf der Seite *checkoutcomplete. aspx* eine Zahlungs Transaktion `ID` dem Käufer angezeigt.

### <a name="handle-cancel-purchase"></a>Handle beim Abbruch beenden

Wenn der Benutzer entscheidet, den Kauf abzubrechen, wird er an die Seite *checkoutcancel. aspx* weitergeleitet, auf der Sie sehen, dass die Bestellung abgebrochen wurde.

1. Öffnen Sie im Ordner *Checkout* die Seite *checkoutcancel. aspx* .
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Behandeln von Kauf Fehlern

Fehler während des Kaufvorgangs werden von der Seite *checkouterror. aspx* verarbeitet. Der Code Behind der Seite *checkoutstart. aspx* , der Seite *checkoutview. aspx* und der Seite *checkoutcomplete. aspx* werden bei einem Fehler an die Seite *checkouterror. aspx* umgeleitet.

1. Öffnen Sie die Seite mit dem Namen *checkouterror. aspx* im Ordner *Checkout* .
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

Die Seite *checkouterror. aspx* wird mit den Fehlerdetails angezeigt, wenn während des Auscheck Vorgangs ein Fehler auftritt.

## <a name="running-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung aus, um zu sehen, wie Produkte gekauft werden. Beachten Sie, dass Sie in der PayPal-Testumgebung ausgeführt werden. Es wird kein tatsächliches Geld ausgetauscht.

1. Stellen Sie sicher, dass alle Dateien in Visual Studio gespeichert sind.
2. Öffnen Sie einen Webbrowser, und navigieren Sie zu [https://developer.paypal.com](https://developer.paypal.com/).
3. Melden Sie sich mit Ihrem PayPal-Entwicklerkonto an, das Sie zuvor in diesem Tutorial erstellt haben.  
   Für die Developer Sandbox von PayPal müssen Sie bei [https://developer.paypal.com](https://developer.paypal.com/) angemeldet sein, um Express Checkout testen zu können. Dies gilt nur für Sandbox-Tests von PayPal, nicht für die-Live Umgebung von PayPal.
4. Drücken Sie in Visual Studio die **Taste F5** , um die Beispielanwendung Wingtip Toys auszuführen.  
   Nachdem die Datenbank neu erstellt wurde, wird der Browser geöffnet und zeigt die Seite *default. aspx* an.
5. Fügen Sie dem Warenkorb drei verschiedene Produkte hinzu, indem Sie die Produktkategorie auswählen, z. b. "Auto", und klicken Sie dann neben jedem Produkt **auf zum Warenkorb hinzufügen** .  
   Der Warenkorb zeigt das Produkt an, das Sie ausgewählt haben.
6. Klicken Sie zum Auschecken auf die Schaltfläche **PayPal** . 

    ![Auschecken und bezahlen mit PayPal-Cart](checkout-and-payment-with-paypal/_static/image20.png)

   Das Auschecken erfordert, dass Sie über ein Benutzerkonto für die Wingtip Toys-Beispielanwendung verfügen.
7. Klicken Sie rechts auf der Seite auf den Link **Google** , um sich mit einem vorhandenen gmail.com-e-Mail-Konto anzumelden.  
   Wenn Sie nicht über ein gmail.com-Konto verfügen, können Sie es für Testzwecke unter [www.gmail.com](https://www.gmail.com/)erstellen. Sie können auch ein lokales Standardkonto verwenden, indem Sie auf "registrieren" klicken. 

    ![Auschecken und bezahlen mit PayPal-Log in](checkout-and-payment-with-paypal/_static/image21.png)
8. Melden Sie sich mit Ihrem Gmail-Konto und-Kennwort an. 

    ![Auschecken und bezahlen mit der PayPal-Gmail-Anmeldung](checkout-and-payment-with-paypal/_static/image22.png)
9. Klicken Sie auf die Schaltfläche **Anmelden** , um Ihr Gmail-Konto mit dem Benutzernamen ihrer Wingtip Toys-Beispielanwendung zu registrieren. 

    ![Auschecken und bezahlen mit einem PayPal-Register Konto](checkout-and-payment-with-paypal/_static/image23.png)
10. Fügen Sie auf der Website "Paypal-Test" Ihre e-Mail-Adresse und das Kennwort **für den** **Käufer** ein, die Sie zuvor in diesem Tutorial erstellt haben. 

    ![Auschecken und bezahlen mit der PayPal-PayPal-Anmeldung](checkout-and-payment-with-paypal/_static/image24.png)
11. Stimmen Sie der PayPal-Richtlinie zu, und klicken Sie auf die Schaltfläche **zustimmen**  
    Beachten Sie, dass diese Seite nur angezeigt wird, wenn Sie dieses PayPal-Konto zum ersten Mal verwenden. Beachten Sie, dass es sich hierbei um ein Testkonto handelt. es werden keine echten Kosten ausgetauscht. 

    ![Auschecken und bezahlen mit PayPal-PayPal-Richtlinie](checkout-and-payment-with-paypal/_static/image25.png)
12. Überprüfen Sie die Bestellinformationen auf der Überprüfungs Seite der PayPal-Testumgebung, und klicken Sie auf **weiter**. 

    ![Auschecken und bezahlen mit PayPal-Review-Informationen](checkout-and-payment-with-paypal/_static/image26.png)
13. Überprüfen Sie auf der Seite *checkstartview. aspx* den Bestellbetrag, und zeigen Sie die generierte Lieferadresse an. Klicken Sie dann auf die Schaltfläche **Bestellung vervollständigen** . 

    ![Auschecken und bezahlen mit der PayPal-Bestell Überprüfung](checkout-and-payment-with-paypal/_static/image27.png)
14. Die Seite **checkoutcomplete. aspx** wird mit einer Zahlungs Transaktions-ID angezeigt. 

    ![Auschecken und Zahlung mit PayPal-Checkout fertiggestellt](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Überprüfen der Datenbank

Wenn Sie die aktualisierten Daten in der Wingtip Toys-Beispiel Anwendungsdatenbank nach dem Ausführen der Anwendung überprüfen, können Sie sehen, dass die Anwendung den Kauf der Produkte erfolgreich aufgezeichnet hat.

Sie können die Daten in der Datenbankdatei *wingtiptoys. mdf* überprüfen, indem Sie das Fenster **Datenbank-Explorer** (**Server-Explorer** Fenster in Visual Studio) wie zuvor in dieser tutorialreihe verwendet haben.

1. Schließen Sie das Browserfenster, wenn es noch geöffnet ist.
2. Wählen Sie in Visual Studio das Symbol **alle Dateien anzeigen** oben in **Projektmappen-Explorer** aus, damit Sie den Ordner **App\_Data** erweitern können.
3. Erweitern Sie den Ordner **App\_Data** .  
 Möglicherweise müssen Sie das Symbol **alle Dateien anzeigen** für den Ordner auswählen.
4. Klicken Sie mit der rechten Maustaste auf die Datenbankdatei *wingtiptoys. mdf* , und wählen Sie **Öffnen**.  
    **Server-Explorer** wird angezeigt.
5. Erweitern Sie den Ordner **Tabellen** .
6. Klicken Sie mit der rechten Maustaste auf die Tabelle **Orders**, und wählen Sie **Tabellendaten anzeigen**.  
 Die Tabelle **Orders** wird angezeigt.
7. Überprüfen Sie die Spalte " **paymenttransaktionid** ", um erfolgreiche Transaktionen zu bestätigen. 

    ![Auschecken und bezahlen mit der PayPal-Review-Datenbank](checkout-and-payment-with-paypal/_static/image29.png)
8. Schließen Sie das Tabellenfenster **Orders** .
9. Klicken Sie im Server-Explorer mit der rechten Maustaste auf die Tabelle **Order Details** , und wählen Sie **Tabellendaten anzeigen**aus.
10. Überprüfen Sie die Werte für `OrderId` und `Username` in der Tabelle **Order Details** . Beachten Sie, dass diese Werte den Werten für `OrderId` und `Username` entsprechen, die in der Tabelle **Orders** enthalten sind.
11. Schließen Sie das Fenster **Order Details** Table.
12. Klicken Sie mit der rechten Maustaste auf die Wingtip Toys-Datenbankdatei (*wingtiptoys. mdf*), und wählen Sie **Verbindung schließen**.
13. Wenn das **Projektmappen-Explorer** Fenster nicht angezeigt wird, klicken Sie unten im **Server-Explorer** Fenster auf **Projektmappen-Explorer** , um die **Projektmappen-Explorer** erneut anzuzeigen.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie Auftrags-und Bestell Detail Schemas hinzugefügt, um den Kauf von Produkten zu verfolgen. Außerdem haben Sie die PayPal-Funktionalität in die Wingtip Toys-Beispielanwendung integriert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.NET Konfiguration (Übersicht)](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Stellen Sie eine sichere ASP.net Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank bereit, um Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Kostenlose Testversion Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Haftungsausschluss

Dieses Tutorial enthält Beispielcode. Dieser Beispielcode wird "wie immer" ohne jegliche Gewährleistung bereitgestellt. Dementsprechend garantiert Microsoft nicht die Genauigkeit, Integrität oder Qualität des Beispielcodes. Sie erklären sich damit einverstanden, den Beispielcode auf eigene Gefahr zu verwenden. Unter keinen Umständen ist Microsoft für Sie in irgendeiner Weise für den Beispielcode, Inhalte, einschließlich, aber nicht beschränkt auf Fehler oder Ausfälle in beliebigen Beispielcodes, Inhalten oder jeglichen Verlusten oder Schäden jeglicher Art, die durch die Verwendung von Beispielcode verursacht werden. Sie werden benachrichtigt und erklären sich damit einverstanden, dass Microsoft vor und nach dem Verlust, dem Verlust, dem Verlust oder der Beschädigung jeglicher Art und ohne Einschränkung, die von Ihnen oder von Ihnen bereitgestellten Materialien verursacht wurden, nicht sicher ist. Verwenden Sie die in diesem Beispiel enthaltenen Sichten, um Sie zu übertragen, zu verwenden oder zu verwenden.

> [!div class="step-by-step"]
> [Zurück](shopping-cart.md)
> [Weiter](membership-and-administration.md)
