---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Benutzeroberfläche und Navigation | Microsoft-Dokumentation
author: Erikre
description: Diese tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: ac1dcaf1ba911fdcaeb3845c6836ec771733d93e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636816"
---
# <a name="ui-and-navigation"></a>Benutzeroberfläche und Navigation

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für das Web. Für diese tutorialreihe steht ein Visual Studio 2013- [Projekt mit C# Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) zur Verfügung.

In diesem Tutorial ändern Sie die Benutzeroberfläche der Standardweb Anwendung, um Funktionen der Anwendung Wingtip Toys Store Front zu unterstützen. Außerdem fügen Sie einfache und Daten gebundene Navigation hinzu. Dieses Tutorial baut auf dem vorherigen Tutorial "Erstellen der Datenzugriffs Ebene" auf und ist Teil der Wingtip Toys-tutorialreihe.

## <a name="what-youll-learn"></a>Lernen Sie Folgendes:

- Ändern der Benutzeroberfläche zur Unterstützung der Funktionen der Wingtip Toys Store-Front-Anwendung.
- So konfigurieren Sie ein HTML5-Element, um die Seitennavigation einzuschließen
- Erstellen eines datengesteuerten Steuer Elements, um zu bestimmten Produktdaten zu navigieren.
- Anzeigen von Daten aus einer Datenbank, die mit Entity Framework Code First erstellt wurde.

ASP.net Web Forms das Erstellen dynamischer Inhalte für Ihre Webanwendung ermöglicht. Jede ASP.NET-Webseite wird ähnlich wie eine statische HTML-Webseite (eine Seite, die keine serverbasierte Verarbeitung umfasst) erstellt, aber die ASP.NET-Webseite enthält zusätzliche Elemente, die ASP.net erkennt und verarbeitet, um HTML zu generieren, wenn die Seite ausgeführt wird.

Bei einer statischen HTML-Seite (*htm* -oder *HTML* -Datei) erfüllt der Server eine `Web` Anforderung, indem er die Datei liest und Sie unverändert an den Browser sendet. Wenn dagegen eine ASP.NET-Webseite (*aspx* -Datei) angefordert wird, wird die Seite als Programm auf dem Webserver ausgeführt. Während die Seite ausgeführt wird, kann Sie beliebige Aufgaben ausführen, die für Ihre Website erforderlich sind. dazu gehören das Berechnen von Werten, das Lesen oder Schreiben von Datenbankinformationen oder das Aufrufen anderer Programme. Als Ausgabe erzeugt die Seite dynamisch Markup (z. b. Elemente in HTML) und sendet diese dynamische Ausgabe an den Browser.

## <a name="modifying-the-ui"></a>Ändern der Benutzeroberfläche

Sie werden diese tutorialreihe fortsetzen, indem Sie die Seite " *default. aspx* " ändern. Sie ändern die Benutzeroberfläche, die bereits von der Standardvorlage zum Erstellen der Anwendung erstellt wurde. Die Art der Änderungen, die Sie vornehmen, ist typisch für das Erstellen von Web Forms Anwendung. Dies geschieht durch Ändern des Titels, Ersetzen von Inhalten und Entfernen von nicht benötigter Standard Inhalten.

1. Öffnen Sie, oder wechseln Sie zur Seite *default. aspx* .
2. Wenn die Seite in der **Entwurfs** Ansicht angezeigt wird, wechseln Sie zur **Quell** Ansicht.
3. Ändern Sie am oberen Rand der Seite in der `@Page`-Direktive das `Title`-Attribut in "Willkommen", wie unten gezeigt hervorgehoben. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Ersetzen Sie außerdem auf der Seite *default. aspx* den gesamten Standard Inhalt, der im `<asp:Content>`-Tag enthalten ist, sodass das Markup wie folgt aussieht. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Speichern Sie die Seite *default. aspx* , indem Sie im Menü **Datei** die Option **default. aspx speichern** auswählen.

   Die resultierende Seite *default. aspx* wird wie folgt angezeigt: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Im Beispiel haben Sie das `Title`-Attribut der `@Page`-Direktive festgelegt. Wenn der HTML-Code in einem Browser angezeigt wird, wird der Servercode `<%: Page.Title %>` in den Inhalt aufgelöst, der im `Title`-Attribut enthalten ist.

Die Beispielseite enthält die grundlegenden Elemente, die eine ASP.NET-Webseite bilden. Die Seite enthält statischen Text, wie Sie es möglicherweise auf einer HTML-Seite haben, sowie Elemente, die für ASP.net spezifisch sind. Der Inhalt, der auf der Seite " *default. aspx* " enthalten ist, wird in den Inhalt der Master Seite integriert, der später in diesem Tutorial erläutert wird.

### <a name="page-directive"></a>@Page-Direktive

ASP.net Web Forms in der Regel Direktiven enthalten, die es Ihnen ermöglichen, Seiteneigenschaften und Konfigurationsinformationen für die Seite anzugeben. Die-Direktiven werden von ASP.net als Anweisungen zum Verarbeiten der Seite verwendet, aber Sie werden nicht als Teil des Markups gerendert, das an den Browser gesendet wird.

Die am häufigsten verwendete-Direktive ist die `@Page`-Direktive, die es Ihnen ermöglicht, viele Konfigurationsoptionen für die Seite anzugeben, einschließlich der folgenden:

1. Die Server Programmiersprache für Code auf der Seite, z C#. b.
2. Gibt an, ob die Seite eine Seite mit Servercode direkt auf der Seite ist, die als Einzel Datei Seite bezeichnet wird, oder ob es sich um eine Seite mit Code in einer separaten Klassendatei handelt, die als Code Behind-Seite bezeichnet wird.
3. Gibt an, ob die Seite über eine zugeordnete Master Seite verfügt und daher als Inhaltsseite behandelt werden soll.
4. Debug-und Ablauf Verfolgungs Optionen.

Wenn Sie keine `@Page` Direktive auf der Seite einschließen oder wenn die Direktive keine bestimmte Einstellung enthält, wird eine Einstellung von der Konfigurationsdatei " *Web. config* " oder der Konfigurationsdatei " *Machine. config* " geerbt. Die Datei *Machine. config* bietet zusätzliche Konfigurationseinstellungen für alle Anwendungen, die auf einem Computer ausgeführt werden.

> [!NOTE] 
> 
> *Machine. config* enthält auch Details zu allen möglichen Konfigurationseinstellungen.

### <a name="web-server-controls"></a>Webserver Steuerelemente

In den meisten ASP.net-Web Forms Anwendungen fügen Sie Steuerelemente hinzu, die es dem Benutzer ermöglichen, mit der Seite zu interagieren, z. b. Schaltflächen, Textfelder, Listen usw. Diese Webserver Steuerelemente ähneln HTML-Schaltflächen und Eingabe Elementen. Sie werden jedoch auf dem Server verarbeitet, sodass Sie Ihre Eigenschaften mithilfe von Servercode festlegen können. Diese Steuerelemente lösen auch Ereignisse aus, die Sie im Servercode behandeln können.

Server Steuerelemente verwenden eine spezielle Syntax, die ASP.net erkennt, wenn die Seite ausgeführt wird. Der TagName für ASP.NET-Server Steuerelemente beginnt mit einem `asp:`-Präfix. Dadurch kann ASP.net diese Server Steuerelemente erkennen und verarbeiten. Das Präfix kann anders sein, wenn das Steuerelement nicht Teil der .NET Framework ist. Zusätzlich zum `asp:`-Präfix enthalten ASP.NET-Server Steuerelemente auch das `runat="server"`-Attribut und ein `ID`, das Sie verwenden können, um im Servercode auf das Steuerelement zu verweisen.

Wenn die Seite ausgeführt wird, identifiziert ASP.net die Server Steuerelemente und führt den Code aus, der diesen Steuerelementen zugeordnet ist. Viele Steuerelemente werden HTML-oder andere Markup Elemente auf der Seite darstellen, wenn Sie in einem Browser angezeigt wird.

### <a name="server-code"></a>Server Code

Die meisten ASP.net-Web Forms Anwendungen enthalten Code, der auf dem Server ausgeführt wird, wenn die Seite verarbeitet wird. Wie bereits erwähnt, kann Servercode verwendet werden, um eine Vielzahl von Dingen auszuführen, z. b. das Hinzufügen von Daten zu einem ListView-Steuerelement. ASP.NET unterstützt eine Vielzahl von Sprachen, die auf dem C#Server ausgeführt werden können, einschließlich, Visual Basic, j# und anderen.

ASP.NET unterstützt zwei Modelle zum Schreiben von Servercode für eine Webseite. Im Einzel Datei Modell befindet sich der Code für die Seite in einem Script-Element, in dem das öffnende Tag das `runat="server"`-Attribut enthält. Alternativ können Sie den Code für die Seite in einer separaten Klassendatei erstellen, die als Code Behind-Modell bezeichnet wird. In diesem Fall enthält die Seite ASP.net Web Forms in der Regel keinen Servercode. Stattdessen enthält die `@Page`-Direktive Informationen, die die *aspx* -Seite mit der zugehörigen Code Behind-Datei verknüpft.

Das `CodeBehind`-Attribut, das in der `@Page`-Direktive enthalten ist, gibt den Namen der separaten Klassendatei an, und das `Inherits`-Attribut gibt den Namen der Klasse in der Code-Behind-Datei an, die der Seite entspricht.

### <a name="updating-the-master-page"></a>Aktualisieren der Master Seite

In ASP.net Web Forms können Sie mit Masterseiten ein konsistentes Layout für die Seiten in Ihrer Anwendung erstellen. Eine einzelne Master Seite definiert das Aussehen und Verhalten und das Standardverhalten für alle Seiten (oder eine Gruppe von Seiten) in der Anwendung. Sie können dann einzelne Inhaltsseiten erstellen, die den Inhalt enthalten, den Sie anzeigen möchten, wie oben erläutert. Wenn Benutzer die Inhaltsseiten anfordern, werden Sie von ASP.net mit der Master Seite zusammengeführt, um eine Ausgabe zu erstellen, die das Layout der Master Seite mit dem Inhalt der Inhaltsseite kombiniert.

Die neue Website benötigt ein einzelnes Logo, das auf jeder Seite angezeigt werden soll. Um dieses Logo hinzuzufügen, können Sie den HTML-Code auf der Master Seite ändern.

1. Suchen und öffnen Sie in **Projektmappen-Explorer**die Seite **Site. Master** .
2. Wenn sich die Seite in der **Entwurfs** Ansicht befindet, wechseln Sie zur **Quell** Ansicht.
3. Aktualisieren Sie die Master Seite, indem Sie das in gelb markierte Markup **ändern oder hinzufügen** : 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Dieser HTML-Code zeigt das Image " *Logo. jpg* " aus dem Ordner " *Images* " der Webanwendung an, den Sie später hinzufügen. Wenn eine Seite, die die Master Seite verwendet, in einem Browser angezeigt wird, wird das Logo angezeigt. Wenn ein Benutzer auf das Logo klickt, wird der Benutzer zur Seite " *default. aspx* " zurück navigiert. Das HTML-Anchor-Tag `<a>` umschließt das Image Server-Steuerelement und ermöglicht das Einschließen des Bilds als Teil des Links. Das `href`-Attribut für das Anchor-Tag gibt die Stamm-"`~/`" der Website als Link Speicherort an. Standardmäßig wird die Seite *default. aspx* angezeigt, wenn der Benutzer zum Stammverzeichnis der Website navigiert. Das **Bild** `<asp:Image>` Server Steuerelement enthält Additions Eigenschaften, z. b. `BorderStyle`, die als HTML-Code angezeigt werden, wenn Sie in einem Browser angezeigt werden.

### <a name="master-pages"></a>Masterseiten

Eine Master Seite ist eine ASP.NET-Datei mit der Erweiterung. Master (z. b *. Site. Master*) mit einem vordefinierten Layout, das statischen Text, HTML-Elemente und Server Steuerelemente enthalten kann. Die Master Seite wird durch eine spezielle `@Master`-Direktive identifiziert, die die `@Page` Direktive ersetzt, die für gewöhnliche *aspx* -Seiten verwendet wird.

Neben der `@Master`-Direktive enthält die Master Seite auch alle HTML-Elemente der obersten Ebene für eine Seite, z. b. `html`, `head`und `form`. Auf der Master Seite, die Sie zuvor hinzugefügt haben, verwenden Sie z. b. eine HTML-`table` für das Layout, ein `img`-Element für das Firmenlogo, den statischen Text und die Server Steuerelemente, um die allgemeine Mitgliedschaft für Ihre Website zu verarbeiten. Sie können beliebige HTML-und beliebige ASP.net-Elemente als Teil der Master Seite verwenden.

Zusätzlich zu statischem Text und Steuerelementen, die auf allen Seiten angezeigt werden, enthält die Master Seite auch ein oder mehrere **contentplachalter** -Steuerelemente. Diese Platzhalter Steuerelemente definieren Bereiche, in denen austauschbare Inhalte angezeigt werden. Der ersetzbare Inhalt wird wiederum auf Inhaltsseiten definiert, z. b *. "default. aspx*", wobei das **Inhalts** Server Steuerelement verwendet wird.

#### <a name="adding-image-files"></a>Hinzufügen von Bilddateien

Das Logo Bild, das oben mit allen Produkt Images referenziert wird, muss der Webanwendung hinzugefügt werden, damit Sie sichtbar sind, wenn das Projekt in einem Browser angezeigt wird.

#### <a name="download-from-msdn-samples-site"></a>Download von der MSDN Samples-Website:

[Getting Started with ASP.NET 4,5 Web Forms and Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

Der Download umfasst Ressourcen im Ordner *wingtiptoys-Assets* , der zum Erstellen der Beispielanwendung verwendet wird.

1. Wenn Sie dies noch nicht getan haben, laden Sie die komprimierten Beispieldateien mithilfe des obigen Links auf der MSDN Samples-Website herunter.
2. Öffnen Sie nach dem herunterladen die ZIP-Datei, und kopieren Sie den Inhalt in einen lokalen Ordner auf Ihrem Computer.
3. Suchen und öffnen Sie den Ordner *wingtiptoys-Assets* .
4. Wenn Sie Drag & Drop, kopieren Sie den *Katalog* Ordner aus dem lokalen Ordner in das Stammverzeichnis des Webanwendungs Projekts in der **Projektmappen-Explorer** von Visual Studio. 

    ![Benutzeroberfläche und Navigation: Dateien kopieren](ui_and_navigation/_static/image1.png)
5. Erstellen Sie als nächstes einen neuen Ordner mit dem Namen *Images* , indem Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **wingtiptoys** klicken und -&gt; **neuen Ordner** **Hinzufügen** auswählen.
6. Kopieren Sie die Datei " *Logo. jpg* " aus dem Ordner *wingtiptoys-Assets* im **Datei-Explorer** in den Ordner *Images* des Webanwendungs Projekts in **Projektmappen-Explorer** von Visual Studio.
7. Klicken Sie oben in **Projektmappen-Explorer** auf die Option **alle Dateien anzeigen** , um die Liste der Dateien zu aktualisieren, wenn die neuen Dateien nicht angezeigt werden.  
  
    **Projektmappen-Explorer** zeigt nun die aktualisierten Projektdateien an. 

    ![Benutzeroberfläche und Navigation-Projektmappen-Explorer](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Hinzufügen von Seiten

Bevor Sie die Navigation zur Webanwendung hinzufügen, fügen Sie zunächst zwei neue Seiten hinzu, zu denen Sie navigieren. Später in dieser tutorialreihe zeigen Sie Produkte und Produktdetails zu diesen neuen Seiten an.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf **wingtiptoys**, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**.   
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie auf der linken Seite die Gruppe **Visual C#**  -&gt; **Web** Templates aus. Wählen Sie dann in der mittleren Liste **Web Form mit Master Seite** aus, und nennen Sie Sie *productList. aspx*. 

    ![UI und Navigation: Dialog Feld "Neues Element hinzufügen"](ui_and_navigation/_static/image3.png)
3. Wählen Sie **Site. Master** aus, um die Master Seite an die neu erstellte *aspx* -Seite anzufügen. 

    ![Benutzeroberfläche und Navigation: Master Seite auswählen](ui_and_navigation/_static/image4.png)
4. Fügen Sie eine zusätzliche Seite mit dem Namen *ProductDetails. aspx* hinzu, indem Sie die gleichen Schritte ausführen.

### <a name="updating-bootstrap"></a>Bootstrap wird aktualisiert

Die Visual Studio 2013-Projektvorlagen verwenden [Bootstrap](http://getbootstrap.com/), ein Layout und Design-Framework, das von Twitter erstellt wurde. Bootstrap verwendet CSS3, um reaktionsfähiges Design bereitzustellen. Dies bedeutet, dass Layouts dynamisch an verschiedene Browserfenster Größen angepasst werden können. Sie können auch die Funktion des Bootstrap-Features verwenden, um auf einfache Weise eine Änderung im Aussehen und Verhalten der Anwendung zu beeinflussen. Standardmäßig enthält die Vorlage ASP.NET-Webanwendung in Visual Studio 2013 Bootstrap als nuget-Paket.

In diesem Tutorial ändern Sie das Aussehen und fühlen der Wingtip Toys-Anwendung, indem Sie die Bootstrap-CSS-Dateien ersetzen.

1. Öffnen Sie in **Projektmappen-Explorer**den Ordner *Inhalt* .
2. Klicken Sie mit der rechten Maustaste auf die Datei *Bootstrap. CSS* , und benennen Sie Sie in *Bootstrap-Original. CSS*um.
3. Benennen Sie *Bootstrap. min. CSS* in *Bootstrap-Original. min. CSS*um.
4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Inhalt* , und wählen Sie **Ordner in Datei-Explorer öffnen aus**.  
   Der Datei-Explorer wird angezeigt. Sie speichern eine heruntergeladene Bootstrap-CSS-Datei an diesem Speicherort.
5. Navigieren Sie in Ihrem Browser zu [https://bootswatch.com/3/](https://bootswatch.com/3/).
6. Scrollen Sie im Browserfenster, bis das Zertifikat "Cerulean" angezeigt wird. 

    ![UI-und Navigations-Cerulean-Design](ui_and_navigation/_static/image5.png)
7. Laden Sie die Datei " *Bootstrap. CSS* " und die Datei " *Bootstrap. min. CSS* " in den *Inhalts* Ordner herunter. Verwenden Sie den Pfad zum Inhalts Ordner, der im zuvor geöffneten Fenster " **Datei-Explorer** " angezeigt wird.
8. Wählen Sie oben in **Projektmappen-Explorer**in **Visual Studio** die Option **alle Dateien anzeigen** aus, um die neuen Dateien im Inhalts Ordner anzuzeigen. 

    ![Benutzeroberfläche und Navigation-Projektmappen-Explorer](ui_and_navigation/_static/image6.png)

   Die beiden neuen CSS-Dateien werden im **Inhalts** Ordner angezeigt, aber beachten Sie, dass das Symbol neben den einzelnen Dateinamen ausgegraut ist. Dies bedeutet, dass die Datei noch nicht zum Projekt hinzugefügt wurde.
9. Klicken Sie mit der rechten Maustaste auf die Dateien " *Bootstrap. CSS* " und " *Bootstrap. min. CSS* ", und wählen Sie **in Projekt einschließen**.   
   Wenn Sie später in diesem Tutorial die Wingtip Toys-Anwendung ausführen, wird die neue Benutzeroberfläche angezeigt.

> [!NOTE] 
> 
> Die ASP.NET-Webanwendungs Vorlage verwendet die Datei " *Bundle. config* " im Stammverzeichnis des Projekts, um den Pfad der Bootstrap-CSS-Dateien zu speichern.

### <a name="modifying-the-default-navigation"></a>Ändern der Standard Navigation

Die Standard Navigation für jede Seite in der Anwendung kann geändert werden, indem Sie das ungeordnete Navigations Listenelement ändern, das sich auf der Seite " *Site. Master* " befindet.

1. Suchen und öffnen Sie in **Projektmappen-Explorer**die Seite *Site. Master* .
2. Fügen Sie den zusätzlichen Navigations Link, der in gelb hervorgehoben ist, der ungeordneten Liste hinzu:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Wie Sie im obigen HTML-Code sehen können, haben Sie alle Zeilen Element `<li>` mit einem Anchor-Tag `<a>` mit einem Link `href`-Attribut geändert. Jede `href` verweist auf eine Seite in der Webanwendung. Wenn ein Benutzer im Browser auf einen dieser Links (z. b. **Produkte**) klickt, wird er zu der Seite navigiert, die im `href` (z. **b. productList. aspx**) enthalten ist. Sie führen die Anwendung am Ende dieses Tutorials aus.

> [!NOTE] 
> 
> Das Zeichen Tilde (`~`) wird verwendet, um anzugeben, dass der `href` Pfad im Stammverzeichnis des Projekts beginnt.

### <a name="adding-a-data-control-to-display-navigation-data"></a>Hinzufügen eines Daten Steuer Elements zum Anzeigen von Navigationsdaten

Als Nächstes fügen Sie ein-Steuerelement hinzu, um alle Kategorien aus der Datenbank anzuzeigen. Jede Kategorie fungiert als Link zur Seite " *productList. aspx* ". Wenn ein Benutzer im Browser auf einen Kategorielink klickt, wird er zur Seite Produkte navigieren und nur die Produkte anzeigen, die der ausgewählten Kategorie zugeordnet sind.

Sie verwenden ein **ListView** -Steuerelement, um alle in der Datenbank enthaltenen Kategorien anzuzeigen. So fügen Sie der Master Seite ein **ListView** -Steuerelement hinzu:

1. Fügen Sie auf der Seite *Site. Master* **nach** dem `<div>`-Element, das die zuvor hinzugefügten `id="TitleContent"` enthält, das folgende hervorgehobene `<div>`-Element hinzu:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

In diesem Code werden alle Kategorien aus der Datenbank angezeigt. Das **ListView** -Steuerelement zeigt jeden Kategorienamen als Linktext an und enthält einen Link zur *productList. aspx* -Seite mit einem Abfrage Zeichen folgen Wert, der die `ID` der Kategorie enthält. Wenn Sie die `ItemType`-Eigenschaft im **ListView** -Steuerelement festlegen, ist der Daten Bindungs Ausdruck `Item` im `ItemTemplate` Knoten verfügbar, und das Steuerelement wird stark typisiert. Sie können Details des `Item` Objekts mithilfe von IntelliSense auswählen, z. b. durch Angeben des `CategoryName`. Dieser Code ist im Container `<%#: %>` enthalten, der einen Daten Bindungs Ausdruck kennzeichnet. Durch Hinzufügen der (:) am Ende des `<%#` Präfixes ist das Ergebnis des Daten Bindungs Ausdrucks HTML-codiert. Wenn das Ergebnis HTML-codiert ist, ist Ihre Anwendung besser vor Cross-Site Script Injection (XSS) und HTML Injection-Angriffen geschützt.

> [!NOTE] 
> 
> **PP**
> 
> Wenn Sie Code hinzufügen, indem Sie während der Entwicklung eingeben, können Sie sicher sein, dass ein gültiger Member eines Objekts gefunden wird, da stark typisierte Daten Steuerelemente die verfügbaren Member basierend auf IntelliSense anzeigen. IntelliSense bietet Kontext geeignete Code Optionen beim Eingeben von Code, z. b. Eigenschaften, Methoden und Objekte.

Im nächsten Schritt implementieren Sie die `GetCategories`-Methode, um Daten abzurufen.

### <a name="linking-the-data-control-to-the-database"></a>Verknüpfen des Daten Steuer Elements mit der Datenbank

Bevor Sie Daten im Daten Steuerelement anzeigen können, müssen Sie das Daten Steuerelement mit der Datenbank verknüpfen. Um den Link zu erstellen, können Sie den Code hinter der *Site.Master.cs* -Datei ändern.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Seite *Site. Master* , und klicken Sie dann auf **Code anzeigen**. Die Datei *Site.Master.cs* wird im Editor geöffnet.
2. Fügen Sie am Anfang der *Site.Master.cs* -Datei zwei zusätzliche Namespaces hinzu, damit alle enthaltenen Namespaces wie folgt aussehen:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Fügen Sie die hervorgehobene `GetCategories` Methode nach dem `Page_Load`-Ereignishandler wie folgt hinzu:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Der obige Code wird ausgeführt, wenn eine Seite, die die Master Seite verwendet, im Browser geladen wird. Das `ListView` Steuerelement (mit dem Namen "CategoryList"), das Sie zuvor in diesem Tutorial hinzugefügt haben, verwendet die Modell Bindung, um Daten auszuwählen. Im Markup des `ListView` Steuer Elements legen Sie die `SelectMethod`-Eigenschaft des Steuer Elements auf die `GetCategories`-Methode fest, wie oben gezeigt. Das `ListView`-Steuerelement ruft die `GetCategories`-Methode zum entsprechenden Zeitpunkt im Lebenszyklus der Seite auf und bindet die zurückgegebenen Daten automatisch. Im nächsten Tutorial erfahren Sie mehr über das Binden von Daten.

### <a name="running-the-application-and-creating-the-database"></a>Ausführen der Anwendung und Erstellen der Datenbank

An früherer Stelle in dieser tutorialreihe haben Sie eine Initialisiererklasse (mit dem Namen "productdatabaseinitializer") erstellt und diese Klasse in der *Global.asax.cs* -Datei angegeben. Der Entity Framework generiert die Datenbank, wenn die Anwendung zum ersten Mal ausgeführt wird, da die `Application_Start`-Methode, die in der *Global.asax.cs* -Datei enthalten ist, die Initialisiererklasse aufruft. Die Initialisiererklasse verwendet die Modellklassen (`Category` und `Product`), die Sie zuvor in dieser tutorialreihe hinzugefügt haben, um die Datenbank zu erstellen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Seite *default. aspx* , und wählen Sie **als Start Seite festlegen**aus.
2. Drücken Sie in Visual Studio die **Taste F5**.   
 Es dauert einige Zeit, bis alles während der ersten Ausführung festgelegt wird.   
    ![Benutzeroberflächen-und Navigations Browser Fenster](ui_and_navigation/_static/image7.png)  
 Wenn Sie die Anwendung ausführen, wird die Anwendung kompiliert, und die Datenbank mit dem Namen *wingtiptoys. mdf* wird im Ordner *App\_Data* erstellt. Im Browser wird ein Kategorie-Navigationsmenü angezeigt. Dieses Menü wurde durch Abrufen der Kategorien aus der Datenbank generiert. Im nächsten Tutorial implementieren Sie die Navigation.
3. Schließen Sie den Browser, um die Ausführung der Anwendung zu beenden.

### <a name="reviewing-the-database"></a>Überprüfen der Datenbank

Öffnen Sie die Datei *Web. config* , und sehen Sie sich den Abschnitt Verbindungs Zeichenfolge an. Sie können sehen, dass der `AttachDbFilename` Wert in der Verbindungs Zeichenfolge auf die `DataDirectory` für das Webanwendungs Projekt verweist. Der Wert `|DataDirectory|` ist ein reservierter Wert, der den *App-\_Daten* Ordner im Projekt darstellt. In diesem Ordner befindet sich die Datenbank, die aus den Entitäts Klassen erstellt wurde.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Wenn die *App\_Daten* Ordner nicht sichtbar ist oder der Ordner leer ist, wählen Sie das **Aktualisierungs** Symbol und dann oben im **Projektmappen-Explorer** Fenster das Symbol **alle Dateien anzeigen** aus. Um alle verfügbaren Symbole anzuzeigen, ist möglicherweise eine Erweiterung der **Projektmappen-Explorer** Fensterbreite erforderlich.

Nun können Sie die Daten, die in der Datenbankdatei *wingtiptoys. mdf* enthalten sind, über das Fenster **Server-Explorer** überprüfen.

1. Erweitern Sie den Ordner *App\_Data* . Wenn der *App-\_Daten* Ordner nicht sichtbar ist, lesen Sie den obigen Hinweis.
2. Wenn die Datenbankdatei *wingtiptoys. mdf* nicht sichtbar ist, wählen Sie das **Aktualisierungs** Symbol und dann oben im **Projektmappen-Explorer** Fenster das Symbol **alle Dateien anzeigen** aus.
3. Klicken Sie mit der rechten Maustaste auf die Datenbankdatei *wingtiptoys. mdf* , und wählen Sie **Öffnen**.  
    **Server-Explorer** wird angezeigt. 

    ![Benutzeroberfläche und Navigation-Server-Explorer](ui_and_navigation/_static/image8.png)
4. Erweitern Sie den Ordner *Tabellen* .
5. Klicken Sie mit der rechten Maustaste auf die Tabelle **Products**, und wählen Sie **Tabellendaten anzeigen**.  
 Die Tabelle " **Products** " wird angezeigt. 

    ![Tabelle für UI-und Navigationsprodukte](ui_and_navigation/_static/image9.png)
6. In dieser Ansicht können Sie die Daten in der Tabelle **Products** by Hand anzeigen und ändern.
7. Schließen Sie das Tabellenfenster **Produkte** .
8. Klicken Sie im **Server-Explorer**erneut mit der rechten Maustaste auf die Tabelle **Products** , und wählen Sie **Tabellen Definition öffnen**aus.  
 Der Daten Entwurf für die Tabelle " **Products** " wird angezeigt. 

    ![Design der Benutzeroberfläche und Navigation/Produkte](ui_and_navigation/_static/image10.png)
9. Auf der Registerkarte **T-SQL** wird die SQL-DDL-Anweisung angezeigt, die zum Erstellen der Tabelle verwendet wurde. Sie können auch die Benutzeroberfläche auf der Registerkarte **Entwurf** verwenden, um das Schema zu ändern.
10. Klicken Sie im **Server-Explorer**mit der rechten Maustaste auf **wingtiptoys** Database, und wählen Sie **Verbindung schließen**.   
 Wenn Sie die Datenbank von Visual Studio trennen, kann das Datenbankschema später in dieser tutorialreihe geändert werden.
11. Kehren Sie zu **Projektmappen-Explorer**zurück, indem Sie die Registerkarte **Projektmappen-Explorer** unten im **Server-Explorer** Fenster auswählen.

## <a name="summary"></a>Summary

In diesem Tutorial der Reihe haben Sie einige grundlegende Benutzeroberfläche, Grafiken, Seiten und Navigation hinzugefügt. Außerdem haben Sie die Webanwendung ausgeführt, die die Datenbank aus den Daten Klassen erstellt hat, die Sie im vorherigen Tutorial hinzugefügt haben. Sie haben auch den Inhalt der Tabelle " *Products* " der Datenbank angezeigt, indem Sie die Datenbank direkt anzeigen. Im nächsten Tutorial zeigen Sie Datenelemente und Details aus der Datenbank an.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Einführung in das Programmieren ASP.net Web Pages](https://msdn.microsoft.com/library/ms178125.aspx)   
[ASP.net Übersicht über Webserver Steuerelemente](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[CSS-Tutorial](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Zurück](create_the_data_access_layer.md)
> [Weiter](display_data_items_and_details.md)
