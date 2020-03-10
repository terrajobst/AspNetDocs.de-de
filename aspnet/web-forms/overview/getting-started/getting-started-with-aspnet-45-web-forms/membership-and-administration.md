---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Mitgliedschaft und Verwaltung | Microsoft-Dokumentation
author: Erikre
description: Diese tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: ab00bc90bfc767d06e747be6dfb973245b5aae88
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439929"
---
# <a name="membership-and-administration"></a>Mitgliedschaft und Verwaltung

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für das Web. Für diese tutorialreihe steht ein Visual Studio 2013- [Projekt mit C# Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) zur Verfügung.

In diesem Tutorial wird gezeigt, wie Sie die Wingtip Toys-Beispielanwendung aktualisieren, um eine benutzerdefinierte Rolle hinzuzufügen und ASP.net Identity zu verwenden. Außerdem wird gezeigt, wie Sie eine Verwaltungsseite implementieren, von der der Benutzer mit einer benutzerdefinierten Rolle Produkte zur Website hinzufügen und daraus entfernen kann.

[ASP.net Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) ist das Mitgliedschaftssystem, das zum Erstellen der ASP.NET-Webanwendung verwendet wird, und ist in ASP.NET 4,5 verfügbar ASP.net Identity wird in der Visual Studio 2013 Web Forms-Projektvorlage sowie in den Vorlagen für die [Einzelseiten Anwendung](../../../../single-page-application/index.md) [ASP.NET MVC](../../../../mvc/index.md), [ASP.net-Web-API](../../../../web-api/index.md)und ASP.NET verwendet. Sie können das ASP.net Identity System auch speziell mithilfe von nuget installieren, wenn Sie mit einer leeren Webanwendung beginnen. In dieser tutorialreihe verwenden Sie jedoch den **Web Forms**ProjectTemplate, der das ASP.net Identity System enthält. Mit ASP.net Identity können benutzerspezifische Profildaten problemlos in Anwendungsdaten integriert werden. Außerdem können Sie im ASP.NET Identity das Persistenzmodell für die Benutzerprofile in Ihrer Anwendung auswählen. Sie können die Daten in einer SQL Server Datenbank oder einem anderen Datenspeicher speichern, einschließlich der *nosql* -Datenspeicher, z. b. Windows Azure Storage Tabellen.

Dieses Tutorial baut auf dem vorherigen Tutorial mit dem Titel "Checkout und Zahlung mit PayPal" in der Wingtip Toys-tutorialreihe auf.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Verwenden von Code, um der Anwendung eine benutzerdefinierte Rolle und einen Benutzer hinzuzufügen.
- Einschränken des Zugriffs auf den Verwaltungs Ordner und die Seite.
- Bereitstellen der Navigation für den Benutzer, der zur benutzerdefinierten Rolle gehört.
- Verwenden der Modell Bindung zum Auffüllen eines [Dropdown List](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) -Steuer Elements mit Produktkategorien.
- Hochladen einer Datei in die Webanwendung mithilfe des [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) -Steuer Elements.
- Verwenden von Validierungs Steuerelementen zum Implementieren der Eingabevalidierung.
- Vorgehensweise beim Hinzufügen und Entfernen von Produkten aus der Anwendung.

## <a name="these-features-are-included-in-the-tutorial"></a>Diese Features sind im Tutorial enthalten:

- ASP.NET Identity
- Konfiguration und Autorisierung
- Modellbindung
- Unaufdringliche Validierung

ASP.net Web Forms bietet Mitgliedschafts Funktionen. Mithilfe der Standardvorlage verfügen Sie über integrierte Mitgliedschafts Funktionen, die Sie sofort verwenden können, wenn die Anwendung ausgeführt wird. In diesem Tutorial wird gezeigt, wie Sie mit ASP.net Identity eine benutzerdefinierte Rolle hinzufügen und dieser Rolle einen Benutzer zuweisen. Sie erfahren, wie Sie den Zugriff auf den Ordner "Verwaltung" einschränken. Sie fügen dem Ordnerverwaltung eine Seite hinzu, die einem Benutzer mit einer benutzerdefinierten Rolle das Hinzufügen und Entfernen von Produkten und das Anzeigen einer Vorschau eines Produkts ermöglicht, nachdem es hinzugefügt wurde.

## <a name="adding-a-custom-role"></a>Hinzufügen einer benutzerdefinierten Rolle

Mithilfe ASP.net Identity können Sie eine benutzerdefinierte Rolle hinzufügen und dieser Rolle mithilfe von Code einen Benutzer zuweisen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Logic* , und erstellen Sie eine neue Klasse.
2. Nennen Sie die neue Klasse *RoleActions.cs*.
3. Ändern Sie den Code so, dass er wie folgt aussieht:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. Öffnen Sie in **Projektmappen-Explorer**die Datei *Global.asax.cs* .
5. Ändern Sie die *Global.asax.cs* -Datei, indem Sie den in gelb markierten Code hinzufügen, sodass er wie folgt aussieht:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Beachten Sie, dass `AddUserAndRole` rot unterstrichen ist. Doppelklicken Sie auf den Code "adduserandrole".  
   Der Buchstabe "A" am Anfang der hervorgehobenen Methode wird unterstrichen.
7. Zeigen Sie auf den Buchstaben "A", und klicken Sie auf die Benutzeroberfläche, mit der Sie einen Methodenstub für die `AddUserAndRole`-Methode generieren können. 

    ![Mitgliedschaft und Administration-Methodenstub generieren](membership-and-administration/_static/image1.png)
8. Klicken Sie auf die Option:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Öffnen Sie die Datei *RoleActions.cs* aus dem Ordner *Logic* .  
   Die `AddUserAndRole`-Methode wurde der-Klassendatei hinzugefügt.
10. Ändern Sie die *RoleActions.cs* -Datei, indem Sie die `NotImplementedException` entfernen und den in gelb markierten Code hinzufügen, sodass Sie wie folgt aussieht:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Der obige Code erstellt zunächst einen Daten Bank Kontext für die Mitgliedschafts Datenbank. Die Mitgliedschafts Datenbank wird auch als *MDF* -Datei im Ordner *App\_Data* gespeichert. Sie können diese Datenbank anzeigen, sobald sich der erste Benutzer bei dieser Webanwendung angemeldet hat. 

> [!NOTE] 
> 
> Wenn Sie die Mitgliedschafts Daten zusammen mit den Produktdaten speichern möchten, können Sie die Verwendung desselben **dbcontext** in Erwägung gezogen haben, den Sie zum Speichern der Produktdaten im obigen Code verwendet haben.

 Das Schlüsselwort *internal* ist ein Zugriffsmodifizierer für Typen (z. b. Klassen) und Typmember (z. b. Methoden oder Eigenschaften). Auf interne Typen oder Member kann nur innerhalb von Dateien zugegriffen werden, die in derselben Assembly *(dll* -Datei) enthalten sind. Wenn Sie die Anwendung erstellen, wird eine Assemblydatei *(. dll*) erstellt, die den Code enthält, der beim Ausführen der Anwendung ausgeführt wird. 

Ein `RoleStore`-Objekt, das die Rollen Verwaltung bereitstellt, wird basierend auf dem Daten Bank Kontext erstellt.

> [!NOTE] 
> 
> Beachten Sie, dass beim Erstellen des `RoleStore` Objekts ein generischer `IdentityRole` Typ verwendet wird. Dies bedeutet, dass der `RoleStore` nur `IdentityRole` Objekte enthalten darf. Außerdem werden Ressourcen im Arbeitsspeicher durch die Verwendung von Generika besser verarbeitet.

Als nächstes wird das `RoleManager`-Objekt auf Grundlage des soeben erstellten `RoleStore` Objekts erstellt. Das `RoleManager`-Objekt macht eine Rollen bezogene API verfügbar, die zum automatischen Speichern von Änderungen am `RoleStore`verwendet werden kann. Der `RoleManager` darf nur `IdentityRole` Objekte enthalten, da der Code den `<IdentityRole>` generischen Typ verwendet.

Sie können die `RoleExists`-Methode aufzurufen, um zu bestimmen, ob die Rolle "CanEdit" in der Mitgliedschafts Datenbank vorhanden ist. Wenn dies nicht der Fall ist, erstellen Sie die Rolle.

Das Erstellen des `UserManager` Objekts scheint komplizierter als das `RoleManager` Steuerelement, aber es ist beinahe identisch. Sie ist nur in einer Zeile und nicht in mehreren codiert. Hier wird der Parameter, den Sie übergeben, als neues Objekt instanziiert, das in der Klammer enthalten ist.

Als Nächstes erstellen Sie den Benutzer "canedituser", indem Sie ein neues `ApplicationUser` Objekt erstellen. Wenn Sie den Benutzer erfolgreich erstellt haben, fügen Sie den Benutzer der neuen Rolle hinzu.

> [!NOTE] 
> 
> Die Fehlerbehandlung wird später in dieser tutorialreihe im Tutorial "ASP.net Error Handling" (Fehlerbehandlung für Fehler) aktualisiert.

Wenn die Anwendung das nächste Mal gestartet wird, wird der Benutzer mit dem Namen "canedituser" der Rolle "CanEdit" der Anwendung hinzugefügt. Später in diesem Tutorial melden Sie sich als "canedituser"-Benutzer an, um zusätzliche Funktionen anzuzeigen, die Sie in diesem Tutorial hinzufügen werden. API-Details zu ASP.net Identity finden Sie im [Microsoft. Aspnet. Identity-Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Weitere Informationen zum Initialisieren des ASP.net Identity Systems finden Sie unter [aspnettidentitysample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Einschränken des Zugriffs auf die Verwaltungsseite

Mit der Wingtip Toys-Beispielanwendung können anonyme Benutzer und angemeldete Benutzer Produkte anzeigen und erwerben. Der angemeldete Benutzer, der über die benutzerdefinierte Rolle "CanEdit" verfügt, kann jedoch auf eine eingeschränkte Seite zugreifen, um Produkte hinzuzufügen und zu entfernen.

#### <a name="add-an-administration-folder-and-page"></a>Hinzufügen eines Verwaltungs Ordners und einer Seite

Als Nächstes erstellen Sie einen Ordner mit dem Namen " *Admin* " für den Benutzer "canedituser", der zur benutzerdefinierten Rolle der Wingtip Toys-Beispielanwendung gehört.

1. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen (**Wingtip Toys**), und wählen Sie -&gt; **neuen Ordner** **Hinzufügen** aus.
2. Benennen Sie den neuen Ordner *Administrator*.
3. Klicken Sie mit der rechten Maustaste auf den Ordner *Admin* , und wählen Sie -&gt; **Neues Element** **Hinzufügen** aus.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
4. Wählen Sie auf der linken Seite die Gruppe <strong>Visual C#</strong> -&gt; <strong>Web</strong> Templates aus. Wählen Sie in der mittleren Liste <strong>Web Form mit Master Seite</strong>aus, nennen Sie es " <em>adminpage. aspx</em>"<strong>,</strong> und wählen Sie dann <strong>Hinzufügen</strong>aus.
5. Wählen Sie die Datei *Site. Master* als Master Seite aus, und klicken Sie dann auf **OK**.

#### <a name="add-a-webconfig-file"></a>Fügen Sie eine Web. config-Datei hinzu.

Indem Sie dem *Administrator* Ordner eine *Web. config* -Datei hinzufügen, können Sie den Zugriff auf die im Ordner enthaltene Seite einschränken.

1. Klicken Sie mit der rechten Maustaste auf den Ordner *Admin* , und wählen Sie -&gt; **Neues Element** **Hinzufügen**  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie in der Liste C# der visuellen Webvorlagen die Option <strong>Webkonfigurationsdatei</strong>aus der mittleren Liste aus, übernehmen Sie den Standardnamen " <em>Web. config</em>"<strong>,</strong> und wählen Sie dann <strong>Hinzufügen</strong>aus.
3. Ersetzen Sie den vorhandenen XML-Inhalt in der Datei *Web.config* durch den folgenden Code:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Speichern Sie die Datei *Web.config* . Die Datei " *Web. config* " gibt an, dass nur der Benutzer, der der Rolle "CanEdit" der Anwendung angehört, auf die im *Administrator* Ordner enthaltene Seite zugreifen kann.

### <a name="including-custom-role-navigation"></a>Einschließen der benutzerdefinierten Rollen Navigation

Um dem Benutzer der benutzerdefinierten Rolle "CanEdit" die Navigation zum Abschnitt Verwaltung der Anwendung zu ermöglichen, müssen Sie der Seite *Site. Master* einen Link hinzufügen. Nur Benutzer, die der Rolle "CanEdit" angehören, können den **Administrator** Link sehen und auf den Abschnitt "Verwaltung" zugreifen.

1. Suchen und öffnen Sie in Projektmappen-Explorer die Seite *Site. Master* .
2. Um einen Link für den Benutzer der Rolle "CanEdit" zu erstellen, fügen Sie das in gelber hervorgehobene Markup der folgenden ungeordneten Liste `<ul>`-Elements hinzu, sodass die Liste wie folgt aussieht:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Öffnen Sie die Datei *Site.Master.cs* . Legen Sie den **Admin** -Link nur für den Benutzer "canedituser" sichtbar, indem Sie den in gelb markierten Code dem `Page_Load`-Handler hinzufügen. Der `Page_Load`-Handler wird wie folgt angezeigt:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Wenn die Seite geladen wird, überprüft der Code, ob der angemeldete Benutzer die Rolle "CanEdit" hat. Wenn der Benutzer der Rolle "CanEdit" angehört, wird das Span-Element, das den Link zur *adminpage. aspx* -Seite enthält (und folglich der Link in der Spanne), sichtbar gemacht.

### <a name="enabling-product-administration"></a>Aktivieren der Produktverwaltung

Bisher haben Sie die Rolle "CanEdit" erstellt und einen "canedituser"-Benutzer, einen Verwaltungs Ordner und eine Verwaltungsseite hinzugefügt. Sie haben Zugriffsrechte für den Ordner und die Seite Verwaltung festgelegt und einen Navigations Link für den Benutzer der Rolle "CanEdit" zur Anwendung hinzugefügt. Als Nächstes fügen Sie der *adminpage. aspx* -Seite Markup und Code der *AdminPage.aspx.cs* -Code Behind-Datei hinzu, die dem Benutzer mit der Rolle "CanEdit" das Hinzufügen und Entfernen von Produkten ermöglicht.

1. Öffnen Sie in **Projektmappen-Explorer**die Datei " *adminpage. aspx* " aus dem Ordner " *Admin* ".
2. Ersetzen Sie das vorhandene Markup durch Folgendes:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Öffnen Sie anschließend die *AdminPage.aspx.cs* -Code Behind-Datei, indem Sie mit der rechten Maustaste auf die Datei " *adminpage. aspx* " klicken und dann **auf Code anzeigen**
4. Ersetzen Sie den vorhandenen Code in der Code Behind-Datei *AdminPage.aspx.cs* durch den folgenden Code:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

In dem Code, den Sie für die *AdminPage.aspx.cs* -Code-Behind-Datei eingegeben haben, bewirkt eine Klasse mit dem Namen `AddProducts`, dass der Datenbank Produkte hinzugefügt werden. Diese Klasse ist noch nicht vorhanden, sodass Sie Sie jetzt erstellen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Logic* , und wählen Sie dann -&gt; **Neues Element** **Hinzufügen** aus.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie auf der linken Seite die Gruppe **Visual C#**  -&gt; **Code** Templates aus. Wählen Sie dann in der mittleren Liste **Klasse**aus, und nennen Sie Sie *AddProducts.cs*.   
   Die neue Klassendatei wird angezeigt.
3. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

Die Seite " *adminpage. aspx* " ermöglicht dem Benutzer, der zur Rolle "CanEdit" gehört, das Hinzufügen und Entfernen von Produkten. Wenn ein neues Produkt hinzugefügt wird, werden die Details zum Produkt überprüft und dann in die Datenbank eingegeben. Das neue Produkt ist sofort für alle Benutzer der Webanwendung verfügbar.

#### <a name="unobtrusive-validation"></a>Unaufdringliche Validierung

Die Produktdetails, die der Benutzer auf der Seite " *adminpage. aspx* " bereitstellt, werden mithilfe von Validierungs Steuerelementen (`RequiredFieldValidator` und `RegularExpressionValidator`) überprüft. Diese Steuerelemente verwenden automatisch unaufdringliche Validierung. Die unaufdringliche Validierung ermöglicht es den Validierungs Steuerelementen, JavaScript für die Client seitige Validierungs Logik zu verwenden. Dies bedeutet, dass für die Seite keine Überprüfung auf dem Server erforderlich ist. Standardmäßig ist die unaufdringliche Validierung in der Datei " *Web. config* " basierend auf der folgenden Konfigurationseinstellung enthalten:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Reguläre Ausdrücke

Der Produkt Preis auf der Seite " *adminpage. aspx* " wird mithilfe eines **RegularExpressionValidator** -Steuer Elements überprüft. Dieses Steuerelement überprüft, ob der Wert des zugeordneten Eingabe Steuer Elements (das Textfeld "addproductprice") mit dem vom regulären Ausdruck angegebenen Muster übereinstimmt. Ein regulärer Ausdruck ist eine Muster Vergleichs Notation, die es Ihnen ermöglicht, bestimmte Zeichen Muster schnell zu finden und abzugleichen. Das **RegularExpressionValidator** -Steuerelement enthält eine Eigenschaft mit dem Namen `ValidationExpression`, die den regulären Ausdruck enthält, der zum Überprüfen der Preis Eingabe verwendet wird, wie unten dargestellt:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>FileUpload-Steuerelement

Zusätzlich zu den Eingabe-und Validierungs Steuerelementen haben Sie das **FileUpload** -Steuerelement der Seite " *adminpage. aspx* " hinzugefügt. Dieses Steuerelement bietet die Möglichkeit, Dateien hochzuladen. In diesem Fall können Sie nur Bilddateien hochladen. Wenn Sie in der Code Behind-Datei (*AdminPage.aspx.cs*) auf den `AddProductButton` klicken, überprüft der Code die `HasFile`-Eigenschaft des **FileUpload** -Steuer Elements. Wenn das Steuerelement über eine Datei verfügt und der Dateityp (basierend auf der Dateierweiterung) zulässig ist, wird das Bild im Ordner *Images* und im Ordner *Images/Thumbs* der Anwendung gespeichert.

#### <a name="model-binding"></a>Modellbindung

Weiter oben in dieser tutorialreihe haben Sie die Modell Bindung zum Auffüllen eines **ListView** -Steuer Elements, eines **formsview** -Steuer Elements, eines **GridView** -Steuer Elements und eines **DetailView** -Steuer Elements verwendet. In diesem Tutorial verwenden Sie die Modell Bindung, um ein **Dropdown List** -Steuerelement mit einer Liste von Produktkategorien aufzufüllen.

Das Markup, das Sie der Datei " *adminpage. aspx* " hinzugefügt haben, enthält ein **Dropdown List** -Steuerelement mit dem Namen `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Mithilfe der Modell Bindung füllen Sie diese **Dropdown Liste** aus, indem Sie das `ItemType`-Attribut und das `SelectMethod`-Attribut festlegen. Das `ItemType`-Attribut gibt an, dass Sie beim Auffüllen des-Steuer Elements den `WingtipToys.Models.Category`-Typ verwenden. Sie haben diesen Typ am Anfang dieser tutorialreihe definiert, indem Sie die `Category`-Klasse erstellen (siehe unten). Die `Category`-Klasse befindet sich im Ordner " *Models* " in der Datei " *Category.cs* ".

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

Das `SelectMethod`-Attribut des **DropDownList** -Steuer Elements gibt an, dass Sie die `GetCategories`-Methode (unten dargestellt) verwenden, die in der Code-Behind-Datei (*AdminPage.aspx.cs*) enthalten ist.

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Diese Methode gibt an, dass eine `IQueryable`-Schnittstelle verwendet wird, um eine Abfrage für einen `Category` Typ auszuwerten. Der zurückgegebene Wert wird verwendet, um die **Dropdown List** im Markup der Seite ("*adminpage. aspx*") aufzufüllen.

Der Text, der für jedes Element in der Liste angezeigt wird, wird durch Festlegen des `DataTextField` Attributs angegeben. Das `DataTextField`-Attribut verwendet die `CategoryName` der `Category`-Klasse (siehe oben), um jede Kategorie im **DropDownList** -Steuerelement anzuzeigen. Der tatsächliche Wert, der beim Auswählen eines Elements im **DropDownList** -Steuerelement weitergegeben wird, basiert auf dem `DataValueField`-Attribut. Das `DataValueField`-Attribut wird auf die `CategoryID` festgelegt, wie in der `Category`-Klasse definiert (siehe oben).

### <a name="how-the-application-will-work"></a>Funktionsweise der Anwendung

Wenn der Benutzer, der zur Rolle "CanEdit" gehört, zum ersten Mal zu der Seite navigiert, wird das `DropDownAddCategory`**DropDownList** -Steuerelement wie oben beschrieben aufgefüllt. Das `DropDownRemoveProduct`**DropDownList** -Steuerelement wird auch mit Produkten aufgefüllt, die denselben Ansatz verwenden. Der Benutzer, der zur Rolle "CanEdit" gehört, wählt den Kategorietyp aus und fügt Produktdetails (**Name**, **Beschreibung**, **Preis**und **Bilddatei**) hinzu. Wenn der Benutzer, der der Rolle "CanEdit" angehört, auf die Schaltfläche " **Produkt hinzufügen** " klickt, wird der `AddProductButton_Click` Ereignishandler ausgelöst. Der `AddProductButton_Click`-Ereignishandler in der Code Behind-Datei (*AdminPage.aspx.cs*) prüft die Bilddatei, um sicherzustellen, dass Sie den zulässigen Dateitypen *(GIF*, *PNG*, *JPEG*oder *JPG*) entspricht. Anschließend wird die Bilddatei in einem Ordner der Wingtip Toys-Beispielanwendung gespeichert. Anschließend wird das neue Produkt der Datenbank hinzugefügt. Um das Hinzufügen eines neuen Produkts zu erreichen, wird eine neue Instanz der `AddProducts`-Klasse erstellt und benannte Produkte. Die `AddProducts`-Klasse verfügt über eine Methode mit dem Namen `AddProduct`, und das Products-Objekt ruft diese Methode auf, um der Datenbank Produkte hinzuzufügen.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Wenn der Code das neue Produkt der Datenbank erfolgreich hinzufügt, wird die Seite mit dem Wert `ProductAction=add`der Abfrage Zeichenfolge erneut geladen.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Wenn die Seite erneut geladen wird, ist die Abfrage Zeichenfolge in der URL enthalten. Durch erneutes Laden der Seite kann der Benutzer, der der Rolle "CanEdit" angehört, die Aktualisierungen in den **Dropdown List** -Steuerelementen auf der Seite " *adminpage. aspx* " sofort sehen. Durch einschließen der Abfrage Zeichenfolge mit der URL kann die Seite auch eine Erfolgsmeldung für den Benutzer anzeigen, der zur Rolle "CanEdit" gehört.

Wenn die *adminpage. aspx* -Seite erneut geladen wird, wird das `Page_Load`-Ereignis aufgerufen.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Der `Page_Load`-Ereignishandler überprüft den Wert der Abfrage Zeichenfolge und bestimmt, ob eine Erfolgsmeldung angezeigt werden soll.

## <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt ausführen, um zu erfahren, wie Sie Elemente im Warenkorb hinzufügen, löschen und aktualisieren können. Der Einkaufswagen Gesamt zeigt die Gesamtkosten aller Produkte im Warenkorb an.

1. Drücken Sie in Projektmappen-Explorer **F5** , um die Beispielanwendung Wingtip Toys auszuführen.  
   Der Browser wird geöffnet und zeigt die Seite *default. aspx* an.
2. Klicken Sie oben auf der Seite auf den Link **Anmelden** . 

    ![Mitgliedschaft und Verwaltung: Anmelde Link](membership-and-administration/_static/image2.png)

   Die Seite *Login. aspx* wird angezeigt.
3. Verwenden Sie den folgenden Benutzernamen und das Kennwort:  
   Benutzername: canEditUser@wingtiptoys.com  
   Kennwort: PA $ $Word 1 

    ![Mitgliedschaft und Verwaltung-Anmeldeseite](membership-and-administration/_static/image3.png)
4. Klicken Sie am unteren Rand der Seite auf die Schaltfläche **Anmelden** .
5. Klicken Sie oben auf der nächsten Seite auf den Link **Admin** , um zur Seite *adminpage. aspx* zu navigieren. 

    ![Mitgliedschaft und Verwaltung-Administrator Link](membership-and-administration/_static/image4.png)
6. Um die Eingabevalidierung zu testen, klicken Sie auf die Schaltfläche **Produkt hinzufügen** , ohne Produktdetails hinzuzufügen. 

    ![Mitgliedschaft und Administration: Administrator Seite](membership-and-administration/_static/image5.png)

   Beachten Sie, dass die erforderlichen Feld Meldungen angezeigt werden.
7. Fügen Sie die Details für ein neues Produkt hinzu, und klicken Sie dann auf die Schaltfläche **Produkt hinzufügen** . 

    ![Mitgliedschaft und Verwaltung-Produkt hinzufügen](membership-and-administration/_static/image6.png)
8. Wählen Sie im oberen Navigationsmenü die Option **Produkte** aus, um das neu hinzugefügte Produkt anzuzeigen. 

    ![Mitgliedschaft und Administration-neues Produktanzeigen](membership-and-administration/_static/image7.png)
9. Klicken Sie auf den Link **Admin** , um zur Verwaltungsseite zurückzukehren.
10. Wählen Sie im Abschnitt **Produkt entfernen** der Seite das neue Produkt aus, das Sie in **dropdownlistbox**hinzugefügt haben.
11. Klicken Sie auf die Schaltfläche **Produkt entfernen** , um das neue Produkt aus der Anwendung zu entfernen. 

    ![Mitgliedschaft und Verwaltung-Produkt entfernen](membership-and-administration/_static/image8.png)
12. Wählen Sie im oberen Navigationsmenü die Option **Produkte** aus, um zu bestätigen, dass das Produkt entfernt wurde.
13. Klicken Sie auf für den existierenden Verwaltungsmodus **Abmelden** .   
    Beachten Sie, dass im oberen Navigationsbereich nicht mehr das Menü Element **Admin** angezeigt wird.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie eine benutzerdefinierte Rolle und einen Benutzer hinzugefügt, der zur benutzerdefinierten Rolle gehört, den Zugriff auf den Verwaltungs Ordner und die Seite eingeschränkt und die Navigation für den Benutzer bereitgestellt hat, der zur benutzerdefinierten Rolle gehört. Sie haben die Modell Bindung verwendet, um ein **Dropdown List** -Steuerelement mit Daten aufzufüllen. Sie haben das **FileUpload** -Steuerelement und Validierungs Steuerelemente implementiert. Außerdem haben Sie erfahren, wie Sie Produkte zu einer Datenbank hinzufügen und daraus entfernen. Im nächsten Tutorial erfahren Sie, wie Sie das ASP.NET-Routing implementieren.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Web. config-Authorization-Element](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Bereitstellen einer sicheren ASP.net-Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Kostenlose Testversion Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Zurück](checkout-and-payment-with-paypal.md)
> [Weiter](url-routing.md)
