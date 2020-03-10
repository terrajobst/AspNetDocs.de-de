---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Neues in Web Forms in ASP.NET 4,5 | Microsoft-Dokumentation
author: rick-anderson
description: Die neue Version von ASP.net Web Forms bietet eine Reihe von Verbesserungen, die sich auf die Verbesserung der Benutzererfahrung beim Arbeiten mit Daten konzentrieren. In früheren Versionen von...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 301af8ed877b58624e419c04f605c41f27dbdd0c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421923"
---
# <a name="whats-new-in-web-forms-in-aspnet-45"></a>Neue Funktionen in Web Forms in ASP.NET 4.5

vom [Web Camps-Team](https://twitter.com/webcamps)

> Die neue Version von ASP.net Web Forms bietet eine Reihe von Verbesserungen, die sich auf die Verbesserung der Benutzererfahrung beim Arbeiten mit Daten konzentrieren.
> 
> In früheren Versionen von Web Forms verwendete bei Verwendung der Datenbindung zum Ausgeben des Werts eines Objektmembers die Daten Bindungs Ausdrücke BIND () oder eval (). In der neuen Version von ASP.net können Sie deklarieren, an welche Art von Daten ein Steuerelement gebunden werden soll, indem Sie eine neue ItemType-Eigenschaft verwenden. Wenn Sie diese Eigenschaft festlegen, können Sie eine stark typisierte Variable verwenden, um die vollständigen Vorteile der Visual Studio-Entwicklungsumgebung zu erhalten, z. b. IntelliSense, Element Navigation und Überprüfung der Kompilierzeit.
> 
> Mit den Daten gebundenen Steuerelementen können Sie nun auch eigene benutzerdefinierte Methoden zum auswählen, aktualisieren, löschen und Einfügen von Daten angeben, um die Interaktion zwischen den Seiten Steuerelementen und der Anwendungslogik zu vereinfachen. Darüber hinaus wurden ASP.net Modell Bindungsfunktionen hinzugefügt. das bedeutet, dass Sie Daten von der Seite direkt in Methodentypparameter zuordnen können.
> 
> Die Überprüfung von Benutzereingaben sollte auch mit der neuesten Version von Web Forms einfacher sein. Nun können Sie Ihre Modellklassen mit Validierungs Attributen aus dem **System. ComponentModel. DataAnnotations** -Namespace kommentieren und anfordern, dass alle Ihre Site-Steuerelemente Benutzereingaben anhand dieser Informationen überprüfen. Die Client seitige Validierung in Web Forms ist nun in jQuery integriert, sodass sauberer Client seitiger Code und unaufdringliche Javascript-Funktionen bereitgestellt werden.
> 
> Im Bereich Anforderungs Validierung wurden Verbesserungen vorgenommen, um das selektive Deaktivieren der Anforderungs Validierung für bestimmte Teile Ihrer Anwendungen oder das Lesen von ungültigen Anforderungs Daten zu vereinfachen.
> 
> Es wurden einige Verbesserungen an Web Forms Server-Steuerelementen vorgenommen, um die Vorteile der neuen Features von HTML5 zu nutzen:
> 
> - Die TextMode-Eigenschaft des TextBox-Steuer Elements wurde aktualisiert, um die neuen HTML5-Eingabetypen wie z. b. e-Mail, DateTime usw. zu unterstützen.
> - Das FileUpload-Steuerelement unterstützt jetzt mehrere Datei Uploads von Browsern, die diese HTML5-Funktion unterstützen.
> - Validator-Steuerelemente unterstützen jetzt die Validierung von HTML5-Eingabe Elementen.
> - Neue HTML5-Elemente, die über Attribute verfügen, die eine URL darstellen, unterstützen jetzt runat =&quot;Server&quot;. Daher können Sie ASP.net-Konventionen in URL-Pfaden verwenden, wie z. b. den ~-Operator, um den Anwendungs Stamm darzustellen (z. b. &lt;Video runat =&quot;Server&quot; src =&quot;~/MyVideo.wmv&quot;&gt;&lt;/Video&gt;).
> - Das Update Panel-Steuerelement wurde korrigiert, um das Veröffentlichen von HTML5-Eingabefeldern zu unterstützen.
> 
> Im offiziellen ASP.net-Portal finden Sie weitere Beispiele für die neuen Features in ASP.net WebForms 4,5: Neuerungen [in ASP.NET 4,5 und Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)verfügbar ist.

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Verwenden von stark typisierten Daten Bindungs Ausdrücken
- Verwenden Sie die neuen Modell Bindungs Features in Web Forms
- Verwenden von Wert Anbietern zum Zuordnen von Seiten Daten für Code Behind-Methoden
- Verwenden von Daten Anmerkungen für die Validierung von Benutzereingaben
- Nutzen Sie die unaufdringliche Client seitige Validierung mit jQuery in Web Forms
- Granulare Anforderungs Validierung implementieren
- Implementieren der asynchronen Seiten Verarbeitung in Web Forms

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

**Installieren von Code Ausschnitten**

Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist. Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.

Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, können Sie den Anhang dieses Dokuments &quot;[Anhang C: Verwenden von Code Ausschnitten](#AppendixC)&quot;entnehmen.

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit umfasst die folgenden Übungen:

1. [Übung 1: Modell Bindung in ASP.net Web Forms](#Exercise1)
2. [Übung 2: Datenvalidierung](#Exercise2)
3. [Übung 3: asynchrone Seiten Verarbeitung in ASP.net Web Forms](#Exercise3)

> [!NOTE]
> Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Übung 1: Modell Bindung in ASP.net Web Forms

In der neuen Version von ASP.net Web Forms wird eine Reihe von Verbesserungen eingeführt, die sich auf die Verbesserung der Erfahrung beim Arbeiten mit Daten konzentrieren. In dieser Übung erfahren Sie mehr über stark typisierte Daten Steuerelemente und Modell Bindung.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Aufgabe 1: Verwenden von stark typisierten Daten Bindungen

In dieser Aufgabe entdecken Sie die neuen stark typisierten Bindungen, die in ASP.NET 4,5 verfügbar sind.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter **Quelle/EX1-modelbinding/BEGIN/** Folder.

   1. Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die Seite **Customers. aspx** . Platzieren Sie eine nicht nummerierte Liste im Hauptsteuer Element, und fügen Sie ein Repeater-Steuerelement in ein, um jeden Kunden aufzulisten. Legen Sie den Repeater-Namen wie im folgenden Code gezeigt auf **customersrepeater** fest.

    In früheren Versionen von Web Forms verwenden Sie bei Verwendung der Datenbindung zum Ausgeben des Werts eines Members für ein Objekt, an das Sie Daten binden, einen Daten Bindungs Ausdruck zusammen mit einem aufzurufenden Ausdruck der Eval-Methode, wobei der Name des Members als Zeichenfolge übergeben wird.

    Zur Laufzeit verwenden diese Aufrufe von eval die Reflektion für das zurzeit gebundene Objekt, um den Wert des Elements mit dem angegebenen Namen zu lesen, und zeigen das Ergebnis im HTML-Format an. Dank dieses Ansatzes ist es sehr einfach, Daten an beliebige, unformatierte Daten zu binden.

    Leider verlieren Sie viele der tollen Features für die Entwicklungszeit in Visual Studio, einschließlich IntelliSense für Elementnamen, Unterstützung für die Navigation (z. b. "Gehe zu Definition") und Überprüfung der Kompilierzeit.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Öffnen Sie die Datei **Customers.aspx.cs** .
4. Fügen Sie die folgende using-Anweisung hinzu.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. Fügen Sie in der **Seite\_Load** -Methode Code hinzu, um den Repeater mit der Kundenliste aufzufüllen.

    (Code Ausschnitt- *Web Forms Lab-Ex01-BIND Customers Data Source*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Die Lösung verwendet EntityFramework in Verbindung mit codefirst, um die Datenbank zu erstellen und darauf zuzugreifen. Im folgenden Code wird customersrepeater an eine materialisierte Abfrage gebunden, die alle Kunden aus der Datenbank zurückgibt.
6. Drücken Sie **F5** , um die Projekt Mappe auszuführen, und wechseln Sie zur Seite **Kunden** , um das Wiederholungs Modul in Aktion zu sehen. Da die Lösung Code First verwendet, wird die Datenbank beim Ausführen der Anwendung erstellt und in Ihrer lokalen SQL Express-Instanz aufgefüllt.

    ![Auflisten der Kunden mit einem Repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Auflisten der Kunden mit einem Repeater")

    *Auflisten der Kunden mit einem Repeater*

    > [!NOTE]
    > In Visual Studio 2012 ist IIS Express der Standardweb Development Server.
7. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.
8. Ersetzen Sie nun die-Implementierung, um stark typisierte Bindungen zu verwenden. Öffnen Sie die Seite **Customers. aspx** , und verwenden Sie das neue **ItemType** -Attribut im Repeater, um den **Customer** -Typ als Bindungstyp festzulegen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Mit der ItemType-Eigenschaft können Sie deklarieren, an welche Art von Daten das Steuerelement gebunden wird, und Sie können eine stark typisierte Bindung innerhalb des Daten gebundenen Steuer Elements verwenden.
9. Ersetzen Sie den ItemTemplate-Inhalt durch den folgenden Code.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Ein Nachteil bei den oben genannten Ansätzen besteht darin, dass die Aufrufe von eval () und BIND () spät gebunden sind. Dies bedeutet, dass Sie Zeichen folgen übergeben, um die Eigenschaften Namen darzustellen. Dies bedeutet, dass Sie IntelliSense nicht für die Elementnamen, die Unterstützung der Code Navigation (z. b. "Gehe zu Definition") oder die Unterstützung zur Kompilierzeit Überprüfung erhalten

    Das Festlegen der ItemType-Eigenschaft bewirkt, dass zwei neue typisierte Variablen im Gültigkeitsbereich der Daten Bindungs Ausdrücke generiert werden: **Item** und **binditem**. Diese stark typisierten Variablen können in den Daten Bindungs Ausdrücken verwendet werden, um die vollständigen Vorteile der Visual Studio-Entwicklungsfunktionen zu erhalten.

    Der &quot; **:** &quot;, der im Ausdruck verwendet wird, wird die Ausgabe automatisch HTML-codiert, um Sicherheitsprobleme zu vermeiden (z. b. Site übergreifende Skript Angriffe). Diese Notation war seit .NET 4 für das Schreiben von Antworten verfügbar, ist jetzt aber auch in Daten Bindungs Ausdrücken verfügbar.

    > [!NOTE]
    > Das Element Element funktioniert für eine unidirektionale Bindung. Wenn Sie die bidirektionale Bindung durchführen möchten, verwenden Sie das **binditem** -Element.

    ![IntelliSense-Unterstützung in stark typisierter Bindung](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "IntelliSense-Unterstützung in stark typisierter Bindung")

    *IntelliSense-Unterstützung in stark typisierter Bindung*
10. Drücken Sie **F5** , um die Projekt Mappe auszuführen, und gehen Sie zur Seite Kunden, um sicherzustellen, dass die Änderungen erwartungsgemäß funktionieren.

    ![Auflisten von Kunden Details](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Auflisten von Kunden Details")

    *Auflisten von Kunden Details*
11. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Aufgabe 2: Einführung in die Modell Bindung in Web Forms

In früheren Versionen von ASP.net Web Forms, wenn Sie eine bidirektionale Datenbindung durchführen wollten, sowohl das Abrufen als auch das Aktualisieren von Daten, mussten Sie ein Datenquellen Objekt verwenden. Dabei kann es sich um eine Objektdaten Quelle, eine SQL-Datenquelle, eine LINQ-Datenquelle usw. handeln. Wenn in Ihrem Szenario jedoch benutzerdefinierter Code für die Verarbeitung der Daten erforderlich ist, mussten Sie die Objektdaten Quelle verwenden, was zu einigen Nachteilen führte. Beispielsweise müssen Sie komplexe Typen vermeiden, und Sie mussten Ausnahmen beim Ausführen von Validierungs Logik behandeln.

In der neuen Version von Web Forms ASP.NET unterstützen die Daten gebundenen Steuerelemente die Modell Bindung. Dies bedeutet, dass Sie SELECT-, Update-, INSERT-und Delete-Methoden direkt im Daten gebundenen Steuerelement angeben können, um Logik aus der Code Behind-Datei oder einer anderen Klasse aufzurufen.

Um dies zu erfahren, verwenden Sie eine GridView, um die Produktkategorien mithilfe des neuen **SelectMethod** -Attributs aufzulisten. Mit diesem Attribut können Sie eine Methode zum Abrufen der GridView-Daten angeben.

1. Öffnen Sie die Seite **Products. aspx** , und schließen Sie eine **GridView**ein. Konfigurieren Sie die GridView wie unten dargestellt, um stark typisierte Bindungen zu verwenden und das Sortieren und Paging zu aktivieren.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Verwenden Sie das neue **SelectMethod** -Attribut, um die GridView so zu konfigurieren, dass eine **GetCategories** -Methode aufgerufen wird, um die Daten auszuwählen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Öffnen Sie die Code Behind-Datei **Products.aspx.cs** , und fügen Sie die folgenden using-Anweisungen hinzu.

    (Code Ausschnitt- *Web Forms Lab-Ex01-Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Fügen Sie einen privaten Member in der **Products** -Klasse hinzu, und weisen Sie eine neue Instanz von **productcontext**zu. Diese Eigenschaft speichert den Entity Framework Datenkontext, der Ihnen ermöglicht, eine Verbindung mit der Datenbank herzustellen.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Erstellen Sie eine **GetCategories** -Methode, um die Liste der Kategorien mithilfe von LINQ abzurufen. Die Abfrage enthält die **Products** -Eigenschaft, sodass in der GridView die Produktmenge für jede Kategorie angezeigt werden kann. Beachten Sie, dass die-Methode ein unformatierbares, iquerable-Objekt zurückgibt, das die Abfrage darstellt, die später für den Seiten Lebenszyklus

    (Code Ausschnitt- *Web Forms Lab-Ex01-GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > In früheren Versionen von ASP.net-Web Forms, das Sortieren und Paging mithilfe ihrer eigenen Repository-Logik innerhalb eines Objektdaten Quellen-Kontexts erforderte, ist erforderlich, um Ihren eigenen benutzerdefinierten Code zu schreiben und alle erforderlichen Parameter zu erhalten. Da die Daten Bindungsmethoden nun iquerable zurückgeben können, und dies stellt eine Abfrage dar, die noch ausgeführt werden kann, kann ASP.net eine Änderung der Abfrage vornehmen, um die richtigen Sortier-und Pagingparameter hinzuzufügen.
6. Drücken Sie **F5** , um das Debuggen der Site zu starten und zur Seite Produkte zu wechseln. Sie sollten sehen, dass die GridView mit den von der GetCategories-Methode zurückgegebenen Kategorien aufgefüllt wird.

    ![Auffüllen eines GridView-Objekts mithilfe der Modell Bindung](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Auffüllen eines GridView-Objekts mithilfe der Modell Bindung")

    *Auffüllen eines GridView-Objekts mithilfe der Modell Bindung*
7. Drücken Sie **UMSCHALT**+**F5** Debuggen.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Aufgabe 3: Wert Anbieter in der Modell Bindung

Mit der Modell Bindung können Sie nicht nur benutzerdefinierte Methoden angeben, um direkt im Daten gebundenen Steuerelement mit Ihren Daten zu arbeiten, sondern auch Daten aus der Seite in den Parametern dieser Methoden zuordnen. Im Methoden Parameter können Sie Wert Anbieter Attribute verwenden, um die Datenquelle des Werts anzugeben. Beispiel:

- Steuerelemente auf der Seite
- Abfrage Zeichen folgen Werte
- Anzeigen von Daten
- Sitzungszustand
- Cookies
- Veröffentlichte Formulardaten
- Ansichts Zustand
- Benutzerdefinierte Wert Anbieter werden ebenfalls unterstützt.

Wenn Sie ASP.NET MVC 4 verwendet haben, werden Sie feststellen, dass die Unterstützung für die Modell Bindung ähnlich ist. Tatsächlich wurden diese Features aus ASP.NET MVC entnommen und in die **System. Web** -Assembly verschoben, damit Sie auch auf Web Forms verwendet werden können.

In dieser Aufgabe aktualisieren Sie die GridView, um die Ergebnisse nach der Produktmenge für jede Kategorie zu filtern, wobei der Filter Parameter mit der Modell Bindung empfangen wird.

1. Wechseln Sie zurück zur Seite **Products. aspx** .
2. Fügen Sie am oberen Rand der GridView eine **Bezeichnung** und ein Kombinations **Feld** hinzu, um die Anzahl der Produkte für jede Kategorie auszuwählen, wie unten gezeigt.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Fügen Sie der GridView ein **EmptyDataTemplate** hinzu, um eine Meldung anzuzeigen, wenn keine Kategorien mit der ausgewählten Anzahl von Produkten vorhanden sind.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Öffnen Sie die Code Behind- **Products.aspx.cs** , und fügen Sie die folgende using-Anweisung hinzu.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Ändern Sie die Methode **GetCategories** so, dass Sie ein ganzzahliges **minproductcount** -Argument empfängt und die zurückgegebenen Ergebnisse filtert Ersetzen Sie hierzu die-Methode durch den folgenden Code.

    (Code Ausschnitt- *Web Forms Lab-Ex01-GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Das neue **[Control]** -Attribut im **minproductscount** -Argument ermöglicht ASP.net, dass sein Wert mit einem-Steuerelement auf der Seite aufgefüllt werden muss. ASP.NET sucht nach einem beliebigen Steuerelement, das mit dem Namen des Arguments (minproductcount) übereinstimmt, und führt die erforderliche Zuordnung und Konvertierung aus, um den Parameter mit dem Steuerelement Wert zu füllen.

    Alternativ bietet das-Attribut einen überladenen Konstruktor, mit dem Sie das Steuerelement angeben können, über den der Wert zu erhalten ist.

    > [!NOTE]
    > Ein Ziel der Daten Bindungsfunktionen besteht darin, den Code zu reduzieren, der für die Seiten Interaktion geschrieben werden muss. Neben dem Wert Anbieter [Control] können Sie andere Modell Bindungs Anbieter in den Methoden Parametern verwenden. Einige davon werden in der Aufgaben Einführung aufgeführt.
6. Drücken Sie **F5** , um das Debuggen der Site zu starten und zur Seite Produkte zu wechseln. Wählen Sie in der Dropdown Liste eine Reihe von Produkten aus, und sehen Sie sich an, wie die GridView jetzt aktualisiert wird.

    ![Filtern von GridView mit einem Dropdown-Listen Wert](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtern von GridView mit einem Dropdown-Listen Wert")

    *Filtern von GridView mit einem Dropdown-Listen Wert*
7. Beenden des Debuggens.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Aufgabe 4: Verwenden der Modell Bindung zum Filtern

In dieser Aufgabe fügen Sie ein zweites untergeordnetes GridView-Element hinzu, um die Produkte innerhalb der ausgewählten Kategorie anzuzeigen.

1. Öffnen Sie die Seite **Products. aspx** , und aktualisieren Sie die Kategorie GridView, um die Schaltfläche auswählen automatisch zu generieren.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Fügen Sie unten eine zweite **GridView** namens **produczgrid** hinzu. Legen Sie **ItemType** auf **webformslab. Model. Product**, **DataKeyNames** auf **ProductID** und **SelectMethod** auf **GetProducts**fest. Legen Sie **autogeneratecolumschlag** auf **false** fest, und fügen Sie die Spalten für ProductID, ProductName, Description und UnitPrice hinzu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Öffnen Sie die Code Behind-Datei **Products.aspx.cs** . Implementieren Sie die **GetProducts** -Methode, um die Kategorie-ID aus der Kategorie GridView zu erhalten und die Produkte zu filtern. Die Modell Bindung legt den Parameterwert mithilfe der ausgewählten Zeile in der **kategoriesrasters**fest. Da der Argument Name und der Name des Steuer Elements nicht stimmen, sollten Sie den Namen des Steuer Elements im Steuerelement Wert Anbieter angeben.

    (Code Ausschnitt- *Web Forms Lab-Ex01-GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Diese Vorgehensweise erleichtert das Komponenten Testen dieser Methoden. In einem unittestkontext, in dem Web Forms nicht ausgeführt wird, führt das [Control]-Attribut keine bestimmte Aktion aus.
4. Öffnen Sie die Seite **Products. aspx** , und suchen Sie nach der GridView-Produkte. Aktualisieren Sie die-Produkte GridView, um einen Link zum Bearbeiten des ausgewählten Produkts anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Öffnen Sie die Code Behind-Seite der **ProductDetails. aspx** -Seite, und ersetzen Sie die **selectProduct** -Methode durch den folgenden Code.

    (Code Ausschnitt- *Web Forms Lab-Ex01-selectProduct-Methode*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Beachten Sie, dass das Attribut **[QueryString]** verwendet wird, um den Methoden Parameter aus einem ProductID-Parameter in der Abfrage Zeichenfolge auszufüllen.
6. Drücken Sie **F5** , um das Debuggen der Site zu starten und zur Seite Produkte zu wechseln. Wählen Sie in den Kategorien GridView eine beliebige Kategorie aus, und beachten Sie, dass die GridView-Produkte aktualisiert wird.

    ![Produkte aus ausgewählter Kategorie werden angezeigt.](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Produkte aus ausgewählter Kategorie werden angezeigt.")

    *Produkte aus ausgewählter Kategorie werden angezeigt.*
7. Klicken Sie auf den Link **Ansicht** für ein Produkt, um die Seite ProductDetails. aspx zu öffnen.

    Beachten Sie, dass die Seite das Produkt mit der SelectMethod mit dem ProductID-Parameter aus der Abfrage Zeichenfolge abruft.

    ![Anzeigen der Produktdetails](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Anzeigen der Produktdetails")

    *Anzeigen der Produktdetails*

    > [!NOTE]
    > Die Möglichkeit, eine HTML-Beschreibung einzugeben, wird in der nächsten Übung implementiert.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Aufgabe 5: Verwenden der Modell Bindung für Aktualisierungs Vorgänge

In der vorherigen Aufgabe haben Sie die Modell Bindung hauptsächlich für die Auswahl von Daten verwendet. in dieser Aufgabe erfahren Sie, wie Sie die Modell Bindung in Aktualisierungs Vorgängen verwenden.

Sie werden die Kategorien GridView aktualisieren, damit Benutzer Kategorien aktualisieren können.

1. Öffnen Sie die Seite **Products. aspx** , und aktualisieren Sie die Kategorien GridView, um die Schaltfläche Bearbeiten automatisch zu generieren, und verwenden Sie das neue **UpdateMethod** -Attribut, um eine **UpdateCategory** -Methode anzugeben, um das ausgewählte Element zu aktualisieren.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Das DataKeyNames-Attribut in der GridView definiert, welche Elemente die Elemente, die das Modell gebundene Objekt eindeutig identifizieren, und somit die Parameter, die die Aktualisierungs Methode zumindest empfangen soll.
2. Öffnen Sie die Code Behind-Datei **Products.aspx.cs** , und implementieren Sie die **UpdateCategory** -Methode. Die-Methode sollte die Kategorie-ID zum Laden der aktuellen Kategorie empfangen, die Werte aus der GridView auffüllen und dann die Kategorie aktualisieren.

    (Code Ausschnitt- *Web Forms Lab-Ex01-UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Die neue **tryupdatemodel** -Methode in der Page-Klasse ist dafür verantwortlich, das Modell Objekt mit den Werten aus den Steuerelementen der Seite aufzufüllen. In diesem Fall werden die aktualisierten Werte aus der aktuellen GridView-Zeile, die bearbeitet wird, in das **Kategorieobjekt** ersetzt.

    > [!NOTE]
    > In der nächsten Übung wird die Verwendung von modelstate. IsValid zum Überprüfen der Daten erläutert, die der Benutzer beim Bearbeiten des Objekts eingegeben hat.
3. Führen Sie die Website aus, und wechseln Sie zur Seite Produkte. Bearbeiten Sie eine Kategorie. Geben Sie einen neuen Namen ein, und klicken Sie dann auf **Aktualisieren** , um die Änderungen beizubehalten.

    ![Kategorien werden bearbeitet](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Kategorien werden bearbeitet")

    *Kategorien werden bearbeitet*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Übung 2: Datenvalidierung

In dieser Übung erfahren Sie mehr über die neuen Funktionen zur Datenvalidierung in ASP.NET 4,5. Sehen Sie sich die neuen unaufdringlichen Validierungs Features in Web Forms an. Sie verwenden Daten Anmerkungen in den Anwendungsmodell Klassen für die Überprüfung der Benutzereingaben. schließlich erfahren Sie, wie Sie die Anforderungs Validierung für einzelne Steuerelemente auf einer Seite aktivieren oder deaktivieren.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Aufgabe 1: unaufdringliche Validierung

Formulare mit komplexen Daten, einschließlich Validierungs Steuerelemente, generieren tendenziell zu viel JavaScript-Code auf der Seite, der ungefähr 60% des Codes darstellen kann. Wenn die unaufdringliche Validierung aktiviert ist, sieht der HTML-Code sauberer und tidier aus.

In diesem Abschnitt aktivieren Sie die unaufdringliche Validierung in ASP.net, um den HTML-Code zu vergleichen, der von beiden Konfigurationen generiert wird.

1. Öffnen Sie **Visual Studio 2012** , und öffnen Sie die Projekt Mappe " **Anfang** ", die sich im Ordner " **source\ex2-validation\begin** " dieses Labs befindet. Alternativ können Sie weiterhin an Ihrer vorhandenen Projekt Mappe in der vorherigen Übung arbeiten.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie dazu im Projektmappen-Explorer auf das **webformslab** -Projekt **nuget-Pakete verwalten**.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Drücken Sie **F5** , um die Webanwendung zu starten. Wechseln Sie zur Seite Kunden, und klicken Sie auf den Link **neuen Kunden hinzufügen** .
3. Klicken Sie mit der rechten Maustaste auf die Browserseite, und wählen Sie **Quell Option anzeigen** aus, um den von der Anwendung generierten HTML-Code zu öffnen.

    ![Anzeige des HTML-Codes der Seite](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Anzeige des HTML-Codes der Seite")

    *Anzeige des HTML-Codes der Seite*
4. Scrollen Sie durch den Quellcode der Seite, und beachten Sie, dass ASP.net JavaScript-Code und Daten Validierungs Steuerelemente auf der Seite eingefügt hat, um die Überprüfungen auszuführen und die Fehlerliste anzuzeigen.

    ![JavaScript-Code für Validierung auf der customerdetails-Seite](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "JavaScript-Code für Validierung auf der customerdetails-Seite")

    *JavaScript-Code für Validierung auf der customerdetails-Seite*
5. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.
6. Nun aktivieren Sie die unaufdringliche Validierung. Öffnen Sie die Datei **Web. config** , und suchen Sie im Abschnitt **appSettings** den Schlüssel " **ValidationSettings: unauffällig-evalidationmode** " **.** Legen Sie den Schlüsselwert auf **WebForms**fest.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Sie können diese Eigenschaft auch auf der Seite "&quot;" festlegen **\_laden**&quot; Ereignisses, falls Sie die unaufdringliche Validierung nur für einige Seiten aktivieren möchten.
7. Öffnen Sie **customerdetails. aspx** , und drücken Sie **F5** , um die Webanwendung zu starten.
8. Drücken Sie die Taste F12, um die IE-Entwicklertools zu öffnen. Nachdem die Entwicklertools geöffnet sind, wählen Sie die Registerkarte Skript aus. Wählen Sie im Menü **customerdetails. aspx** aus, und beachten Sie, dass die Skripts, die zum Ausführen von jQuery auf der Seite erforderlich sind, vom lokalen Standort in den Browser geladen wurden.

    ![Direktes Laden der jQuery-JavaScript-Dateien vom lokalen IIS-Server](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Direktes Laden der jQuery-JavaScript-Dateien vom lokalen IIS-Server")

    *Direktes Laden der jQuery-JavaScript-Dateien vom lokalen IIS-Server*
9. Schließen Sie den Browser, um zu Visual Studio zurückzukehren. Öffnen Sie die Datei " **Site. Master** " erneut, und suchen Sie den **ScriptManager**. Fügen Sie die Eigenschaft **enablecdn** des Attributs mit dem Wert **true**hinzu. Dadurch wird das Laden von jQuery aus der Online-URL, nicht aus der URL des lokalen Standorts erzwungen.
10. Öffnen Sie **customerdetails. aspx** in Visual Studio. Drücken Sie die Taste F5, um die Website auszuführen. Nachdem Internet Explorer geöffnet wurde, drücken Sie die Taste F12, um die Entwicklertools zu öffnen. Wählen Sie die Registerkarte **Skript** aus, und sehen Sie sich dann die Dropdown Liste an. Beachten Sie, dass die jQuery-JavaScript-Dateien nicht mehr vom lokalen Standort geladen werden, sondern nicht mehr vom CDN der Online-jQuery.

    ![Laden der jQuery-JavaScript-Dateien aus dem CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Laden der jQuery-JavaScript-Dateien aus dem CDN")

    *Laden der jQuery-JavaScript-Dateien aus dem CDN*
11. Öffnen Sie den Quellcode der HTML-Seite erneut mit der Option Quelle anzeigen im Browser. Beachten Sie, dass durch Aktivieren der unaufdringlichen Validierung ASP.NET den injizierten JavaScript-Code durch Daten \*Attribute ersetzt hat.

    ![Unaufdringlichem Validierungscode](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Unaufdringlichem Validierungscode")

    *Unaufdringlichem Validierungscode*

    > [!NOTE]
    > In diesem Beispiel haben Sie gesehen, wie eine Validierungs Zusammenfassung mit Daten Anmerkungen in nur einige HTML-und JavaScript-Zeilen vereinfacht wurde. Früher, ohne unaufdringliche Validierung, desto mehr Validierungs Steuerelemente, die Sie hinzufügen, desto größer wird Ihr JavaScript-Validierungscode.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Aufgabe 2: Validieren des Modells mit Daten Anmerkungen

ASP.NET 4,5 führt die Validierung von Daten Anmerkungen für Web Forms ein. Anstatt für jede Eingabe ein Validierungs Steuerelement zu verwenden, können Sie nun Einschränkungen in den Modellklassen definieren und Sie für alle Webanwendungen verwenden. In diesem Abschnitt erfahren Sie, wie Sie Daten Anmerkungen zum Validieren eines neuen/Bearbeitungs Kunden Formulars verwenden.

1. Öffnen Sie die Seite **CustomerDetail. aspx** . Beachten Sie, dass der Kunden Vorname und der zweite Name in den Abschnitten **EditItemTemplate** und **InsertItemTemplate** mithilfe eines Requirements dfieldvalidator-Steuer Elements überprüft werden. Jedes Validierungs Steuerelement ist einer bestimmten Bedingung zugeordnet, sodass Sie so viele Validierungs Steuerelemente als zu überprüfende Bedingungen einschließen müssen.
2. Fügen Sie Daten Anmerkungen hinzu, um die Kundenmodell Klasse zu validieren. Öffnen Sie die **Customer.cs** *-Klasse* im **Modell** Ordner, und versehen Sie jede Eigenschaft mithilfe von Attributen für die Daten Anmerkung.

    (Code Ausschnitt- *Web Forms Lab-Ex02-Data-* Anmerkungen)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET Framework 4,5 hat die vorhandene Sammlung von Daten Anmerkungen erweitert. Dies sind einige der Daten Anmerkungen, die Sie verwenden können: [CreditCard], [Phone], [EmailAddress], [Range], [Compare], [URL], [File Extensions], [Required], [Schlüssel], [RegularExpression].
    > 
    > Einige Verwendungs Beispiele:
    > 
    > [Schlüssel]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Sie können auch eigene Fehlermeldungen in jedem Attribut definieren.
3. Öffnen Sie **customerdetails. aspx** , und entfernen Sie alle Requirements dfieldvalidators für die Felder First und Last Name in den Abschnitten in EditItemTemplate und InsertItemTemplate des FormView-Steuer Elements.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Ein Vorteil der Verwendung von Daten Anmerkungen besteht darin, dass die Validierungs Logik nicht in ihren Anwendungs Seiten dupliziert wird. Sie definieren Sie einmal im Modell und verwenden Sie auf allen Anwendungs Seiten, die Daten bearbeiten.
4. Öffnen Sie die Code Behind-Datei " **customerdetails. aspx** ", und suchen Sie die SaveCustomer-Methode. Diese Methode wird aufgerufen, wenn ein neuer Kunde eingefügt und der Customer-Parameter aus den FormView-Steuerelement Werten empfangen wird. Wenn die Zuordnung zwischen den Seiten Steuerelementen und dem Parameter Objekt stattfindet, führt ASP.net die Modell Überprüfung für alle Attribute der Daten Anmerkung aus und füllen das Model State-Wörterbuch mit den auftretenden Fehlern aus, sofern vorhanden.

    "Modelstate. IsValid" gibt nur dann "true" zurück, wenn alle Felder im Modell nach der Überprüfung gültig sind.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Fügen Sie am Ende der Seite customerdetails ein **ValidationSummary** -Steuerelement hinzu, um die Liste der Modell Fehler anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    " **Showmodelstateerrors** " ist eine neue Eigenschaft im ValidationSummary-Steuerelement, die bei Festlegung auf " **true**" die Fehler aus dem Model State-Wörterbuch anzeigt. Diese Fehler stammen aus der Überprüfung der Daten Anmerkungen.
6. Drücken Sie **F5** , um die Webanwendung auszuführen. Vervollständigen Sie das Formular mit einigen fehlerhaften Werten, und klicken Sie auf **Speichern** , um die Validierung auszuführen Beachten Sie die Fehler Zusammenfassung im unteren Bereich.

    ![Validierung mit Daten Anmerkungen](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validierung mit Daten Anmerkungen")

    *Validierung mit Daten Anmerkungen*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Aufgabe 3: Behandeln von benutzerdefinierten Daten Bank Fehlern mit modelstate

In der vorherigen Version von Web Forms das Behandeln von Daten Bank Fehlern, wie z. b. eine zu lange Zeichenfolge oder eine Verletzung eines eindeutigen Schlüssels, das Auslösen von Ausnahmen in Ihrem Repository-Code und die anschließende Behandlung der Ausnahmen in Ihrem Code-Behind zum Anzeigen eines Fehlers einschließen. Eine große Menge an Code ist erforderlich, um relativ einfach etwas zu tun.

In Web Forms 4,5 kann das modelstate-Objekt verwendet werden, um die Fehler auf der Seite entweder aus dem Modell oder aus der Datenbank auf konsistente Weise anzuzeigen.

In dieser Aufgabe fügen Sie Code hinzu, um Daten Bank Ausnahmen ordnungsgemäß zu behandeln und dem Benutzer die entsprechende Meldung mit dem modelstate-Objekt anzuzeigen.

1. Wenn die Anwendung noch ausgeführt wird, versuchen Sie, den Namen einer Kategorie mit einem duplizierten Wert zu aktualisieren.

    ![Aktualisieren einer Kategorie mit einem doppelten Namen](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Aktualisieren einer Kategorie mit einem doppelten Namen")

    *Aktualisieren einer Kategorie mit einem doppelten Namen*

    Beachten Sie, dass eine Ausnahme aufgrund der &quot;eindeutigen&quot;-Einschränkung der **CategoryName** -Spalte ausgelöst wird.

    ![Ausnahme für doppelte Kategorienamen](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Ausnahme für doppelte Kategorienamen")

    *Ausnahme für doppelte Kategorienamen*
2. Beenden des Debuggens. Aktualisieren Sie in der Code Behind-Datei **Products.aspx.cs** die **UpdateCategory** -Methode, um die von der Datenbank ausgelösten Ausnahmen zu verarbeiten. SaveChanges ()-Methodenaufrufe und Hinzufügen eines Fehlers zum **modelstate** -Objekt.

    Die neue **tryupdatemodel** -Methode aktualisiert das Kategorieobjekt, das aus der Datenbank abgerufen wurde, mithilfe der vom Benutzer bereitgestellten Formulardaten.

    (Code Ausschnitt- *Web Forms Lab-Ex02-UpdateCategory handle-Fehler*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Im Idealfall müssten Sie die Ursache der dbupdateexception identifizieren und überprüfen, ob die Ursache die Verletzung einer UNIQUE KEY-Einschränkung ist.
3. Öffnen Sie **Products. aspx** , und fügen Sie unterhalb der Kategorie GridView ein **ValidationSummary** -Steuerelement hinzu, um die Liste der Modell Fehler anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Führen Sie die Website aus, und wechseln Sie zur Seite Produkte. Versuchen Sie, den Namen einer Kategorie mit einem duplizierten Wert zu aktualisieren.

    Beachten Sie, dass die Ausnahme behandelt wurde und die Fehlermeldung im **ValidationSummary** -Steuerelement angezeigt wird.

    ![Doppelter Kategoriefehler](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Doppelter Kategoriefehler")

    *Doppelter Kategoriefehler*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Aufgabe 4: Anforderungs Validierung in ASP.net Web Forms 4,5

Das Feature "Anforderungs Validierung" in ASP.net bietet eine bestimmte Ebene des Standard Schutzes vor Cross-Site Scripting (XSS)-Angriffen. In früheren Versionen von ASP.net war die Anforderungs Überprüfung standardmäßig aktiviert und konnte nur für eine gesamte Seite deaktiviert werden. Mit der neuen Version von ASP.net Web Forms Sie nun die Anforderungs Überprüfung für ein einzelnes Steuerelement deaktivieren, eine verzögerte Anforderungs Überprüfung ausführen oder auf nicht validierte Anforderungs Daten zugreifen können (seien Sie vorsichtig, wenn Sie dies tun!).

1. Drücken Sie **STRG + F5** , um die Website ohne Debugging zu starten, und wechseln Sie zur Seite Produkte. Wählen Sie eine Kategorie aus, und klicken Sie dann auf den Link **Bearbeiten** für eines der Produkte.
2. Geben Sie eine Beschreibung ein, die potenziell gefährlichen Inhalt enthält, z.b. HTML-Tags. Beachten Sie die Ausnahme, die aufgrund der Anforderungs Validierung ausgelöst wurde.

    ![Bearbeiten eines Produkts mit potenziell gefährlichem Inhalt](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Bearbeiten eines Produkts mit potenziell gefährlichem Inhalt")

    *Bearbeiten eines Produkts mit potenziell gefährlichem Inhalt*

    ![Ausnahme aufgrund der Anforderungs Validierung](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Ausnahme aufgrund der Anforderungs Validierung")

    *Ausnahme aufgrund der Anforderungs Validierung*
3. Schließen Sie die Seite, und drücken Sie in Visual Studio **UMSCHALT + F5** , um das Debuggen zu beenden.
4. Öffnen Sie die Seite **ProductDetails. aspx** , und suchen Sie das Textfeld **Beschreibung** .
5. Fügen Sie die neue **validaterequestmode** -Eigenschaft zum Textfeld hinzu, und legen Sie Ihren Wert auf **deaktiviert**fest.

    Mit dem neuen **validaterequestmode** -Attribut können Sie die Anforderungs Validierung für jedes Steuerelement granulär deaktivieren. Dies ist nützlich, wenn Sie eine Eingabe verwenden möchten, die möglicherweise HTML-Code empfängt, aber die Überprüfung für den Rest der Seite beibehalten soll.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Drücken Sie **F5** , um die Webanwendung auszuführen. Öffnen Sie erneut die Seite Produkt bearbeiten, und schließen Sie eine Produktbeschreibung einschließlich HTML-Tags ab. Beachten Sie, dass Sie jetzt HTML-Inhalt zur Beschreibung hinzufügen können.

    ![Die Anforderungs Validierung für die Produktbeschreibung ist deaktiviert.](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Die Anforderungs Validierung für die Produktbeschreibung ist deaktiviert.")

    *Die Anforderungs Validierung für die Produktbeschreibung ist deaktiviert.*

    > [!NOTE]
    > In einer Produktionsanwendung sollten Sie den HTML-Code bereinigen, der vom Benutzer eingegeben wurde, um sicherzustellen, dass nur sichere HTML-Tags eingegeben werden (z. b. gibt es keine &lt;Skript&gt; Tags). Zu diesem Zweck können Sie die [Microsoft-Webschutz Bibliothek](https://www.nuget.org/packages/AntiXSS)verwenden.
7. Bearbeiten Sie das Produkt erneut. Geben Sie im Feld Name HTML-Code ein, und klicken Sie auf **Speichern**. Beachten Sie, dass die Anforderungs Überprüfung nur für das Beschreibungsfeld deaktiviert ist und die restlichen Felder erneut anhand des potenziell gefährlichen Inhalts überprüft werden.

    ![Die Anforderungs Validierung ist in den restlichen Feldern aktiviert.](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Die Anforderungs Validierung ist in den restlichen Feldern aktiviert.")

    *Die Anforderungs Validierung ist in den restlichen Feldern aktiviert.*

    ASP.net Web Forms 4,5 enthält einen neuen Anforderungs Validierungs Modus, um die Anforderungs Validierung verzögert auszuführen. Wenn der Anforderungs Validierungs Modus auf **4,5**festgelegt ist und ein Code Abschnitt auf *Request. Form [&quot;Key&quot;]* zugreift, löst die Anforderungs Validierung von ASP.NET 4.5 nur die Anforderungs Validierung für dieses bestimmte Element in der Formular Auflistung aus.

    Darüber hinaus enthält ASP.NET 4,5 jetzt Kern Codierungs Routinen aus der Microsoft Anti-XSS Library v 4.0. Die Anti-XSS-Codierungs Routinen werden vom neuen *antixssencoder* -Typ implementiert, der im neuen **System. Web. Security. antixss** -Namespace enthalten ist. Wenn der **encodertype** -Parameter für die Verwendung von *antixssencoder*konfiguriert ist, verwendet alle Ausgabe Codierungen in ASP.NET automatisch die neuen Codierungs Routinen.
8. Die ASP.NET 4,5-Anforderungs Validierung unterstützt auch nicht validierten Zugriff auf Anforderungs Daten. ASP.NET 4,5 fügt dem **HttpRequest** -Objekt, das als **nicht validiert**bezeichnet wird, eine neue Sammlungs Eigenschaft hinzu. Wenn Sie zu **HttpRequest. unvalidieren** navigieren, haben Sie Zugriff auf alle allgemeinen Teile von Anforderungs Daten, einschließlich Formularen, Querystrings, Cookies, URLs usw.

    ![Request. nicht validiertes Objekt](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request. nicht validiertes Objekt")

    *Request. nicht validiertes Objekt*

    > [!NOTE]
    > **Verwenden Sie die HttpRequest. unvalivalidierte Eigenschaft mit Vorsicht!** Stellen Sie sicher, dass Sie eine benutzerdefinierte Überprüfung der Rohdaten anfordern, um sicherzustellen, dass gefährlicher Text nicht Roundtrip und wieder an nicht Aufforderungs Kunden gerendert wird!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Übung 3: asynchrone Seiten Verarbeitung in ASP.net Web Forms

In dieser Übung werden Sie in die neuen Funktionen für die asynchrone Seiten Verarbeitung in ASP.net Web Forms eingeführt.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Aufgabe 1: Aktualisieren der Seite "Produkt Details" zum Hochladen und Anzeigen von Bildern

In dieser Aufgabe aktualisieren Sie die Seite "Produktdetails", damit der Benutzer eine Bild-URL für das Produkt angeben und in der schreibgeschützten Ansicht anzeigen kann. Sie erstellen eine lokale Kopie des angegebenen Images, indem Sie es synchron herunterladen. In der nächsten Aufgabe aktualisieren Sie diese Implementierung, damit Sie asynchron funktioniert.

1. Öffnen Sie **Visual Studio 2012** , und laden Sie die **Begin** -Projekt Mappe, die sich in **source\ex3-async\begin** befindet, aus dem Ordner dieses Labs. Alternativ können Sie weiterhin an der vorhandenen Projekt Mappe aus den vorherigen Übungen arbeiten.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu in der Projektmappen-Explorer auf das Projekt **webformplatte** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die Seite **ProductDetails. aspx** -Seite, und fügen Sie ein Feld in das ItemTemplate-Element von FormView ein, um das Produktbild anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Fügen Sie ein Feld hinzu, um die Bild-URL im edittemplate von FormView anzugeben.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Öffnen Sie die Code Behind-Datei **ProductDetails.aspx.cs** , und fügen Sie die folgenden Namespace Direktiven hinzu.

    (Code Ausschnitt- *Web Forms Lab-Ex03-Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Erstellen Sie eine **updateproductimage** -Methode zum Speichern von Remote Images im Ordner lokale **Images** , und aktualisieren Sie die Product-Entität mit dem Wert neuer Image Speicherort.

    (Code Ausschnitt- *Web Forms Lab-Ex03-updateproductimage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Aktualisieren Sie die **UpdateProduct** -Methode, um die **updateproductimage** -Methode aufzurufen.

    (Code Ausschnitt- *Web Forms Lab-Ex03-updateproductimage-aufrufen*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Führen Sie die Anwendung aus, und versuchen Sie, ein Image für ein Produkt hochzuladen. Beispielsweise können Sie die folgende Bild-URL von Office Clip Arts verwenden: [ [http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Festlegen eines Abbilds für ein Produkt](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Festlegen eines Abbilds für ein Produkt")

    *Festlegen eines Abbilds für ein Produkt*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Aufgabe 2: Hinzufügen der asynchronen Verarbeitung zur Seite "Produkt Details"

In dieser Aufgabe aktualisieren Sie die Seite mit den Produktdetails, damit Sie asynchron funktioniert. Sie werden eine Aufgabe mit langer Ausführungszeit, den Abbild Downloadprozess, mithilfe der asynchronen Seiten Verarbeitung von ASP.NET 4,5 verbessern.

Asynchrone Methoden in Webanwendungen können zur Optimierung der Verwendung von ASP.net-Thread Pools verwendet werden. In ASP.net gibt es eine begrenzte Anzahl von Threads im Thread Pool für die Teilnahme an Anforderungen. Wenn also alle Threads ausgelastet sind, beginnt ASP.net mit dem Ablehnen neuer Anforderungen, sendet Anwendungs Fehlermeldungen und macht Ihre Website nicht verfügbar.

Zeitaufwändige Vorgänge auf Ihrer Website sind hervorragend für die asynchrone Programmierung geeignet, da Sie den zugewiesenen Thread für einen längeren Zeitraum belegen. Dies schließt Anforderungen mit langer Ausführungszeit, Seiten mit vielen unterschiedlichen Elementen und Seiten ein, die Offline Vorgänge erfordern, z. b. das Abfragen einer Datenbank oder den Zugriff auf einen externen Webserver. Der Vorteil besteht darin, dass bei der Verarbeitung von asynchronen Methoden für diese Vorgänge der Thread freigegeben und an den Thread Pool zurückgegeben wird und für die Teilnahme an einer neuen Seiten Anforderung verwendet werden kann. Dies bedeutet, dass die Seite in einem Thread aus dem Thread Pool verarbeitet wird und die Verarbeitung nach Abschluss der asynchronen Verarbeitung in einer anderen Thread Verarbeitung abgeschlossen werden kann.

1. Öffnen Sie die Seite **ProductDetails. aspx** . Fügen Sie das **Async** -Attribut im **Page** -Element hinzu, und legen Sie es auf **true**fest. Dieses Attribut weist ASP.net an, die IHttpAsyncHandler-Schnittstelle zu implementieren.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Fügen Sie unten auf der Seite eine Bezeichnung hinzu, um die Details der Threads anzuzeigen, die die Seite ausführen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Öffnen Sie **ProductDetails.aspx.cs** , und fügen Sie die folgenden Namespace-Direktiven hinzu.

    (Code Ausschnitt- *Web Forms Lab-Ex03-Namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Ändern Sie die **updateproductimage** -Methode, um das Image mit einer asynchronen Aufgabe herunterzuladen. Ersetzen Sie die **WebClient** **Download File** -Methode durch die **downloadfletaskasync** -Methode, und schließen Sie das **warte** Zeit Schlüsselwort ein.

    (Code Ausschnitt- *Web Forms Lab-Ex03-updateproductimage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask registriert eine neue Seite, die asynchron ausgeführt wird und in einem anderen Thread ausgeführt wird. Er empfängt einen Lambda-Ausdruck mit der Aufgabe (t), die ausgeführt werden soll. Das **warte** Zeit Schlüsselwort in der **downloadfletaskasync** -Methode konvertiert den Rest der Methode in einen Rückruf, der nach Abschluss der **downloadfletaskasync** -Methode asynchron aufgerufen wird. ASP.net setzt die Ausführung der-Methode fort, indem alle ursprünglichen Werte der HTTP-Anforderung automatisch verwaltet werden. Das neue asynchrone Programmiermodell in .NET 4,5 ermöglicht Ihnen das Schreiben von asynchronem Code, der stark wie synchroner Code aussieht, und ermöglicht dem Compiler, die Komplikationen von Rückruf Funktionen oder Fortsetzungs Code zu behandeln.
    > [!NOTE]
    > RegisterAsyncTask und PageAsyncTask waren bereits seit .NET 2,0 verfügbar. Das Erwartungs Wort "erwartungsgemäß" ist neu aus dem asynchronen Programmiermodell von .NET 4,5 und kann zusammen mit den neuen taskasync-Methoden aus dem .net WebClient-Objekt verwendet werden.
5. Fügen Sie Code hinzu, um die Threads anzuzeigen, in denen der Code gestartet und die Ausführung beendet wurde. Aktualisieren Sie zu diesem Zweck die **updateproductimage** -Methode mit dem folgenden Code.

    (Code Ausschnitt- *Web Forms Lab-Ex03-Show Threads*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Öffnen Sie die Datei **Web. config** der Website. Fügen Sie die folgende appSetting-Variable hinzu.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Drücken Sie **F5** , um die Anwendung auszuführen und ein Bild für das Produkt hochzuladen. Beachten Sie die Thread-ID, in der der Code gestartet und beendet werden kann. Dies liegt daran, dass asynchrone Tasks in einem separaten Thread aus dem ASP.net-Thread Pool ausgeführt werden. Wenn die Aufgabe abgeschlossen ist, setzt ASP.net die Aufgabe wieder in die Warteschlange und weist alle verfügbaren Threads zu.

    ![Asynchrones herunterladen eines Bilds](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Asynchrones herunterladen eines Bilds")

    *Asynchrones herunterladen eines Bilds*

> [!NOTE]
> Außerdem können Sie diese Anwendung in Azure bereitstellen, [indem Sie Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixB).

---

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit wurden die folgenden Konzepte behandelt und veranschaulicht:

- Verwenden von stark typisierten Daten Bindungs Ausdrücken
- Verwenden Sie die neuen Modell Bindungs Features in Web Forms
- Verwenden von Wert Anbietern zum Zuordnen von Seiten Daten für Code Behind-Methoden
- Verwenden von Daten Anmerkungen für die Validierung von Benutzereingaben
- Nutzen Sie die unaufdringliche Client seitige Validierung mit jQuery in Web Forms
- Granulare Anforderungs Validierung implementieren
- Implementieren der asynchronen Seiten Verarbeitung in Web Forms

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

In diesem Anhang wird erläutert, wie Sie eine neue Website aus dem Azure-Portal erstellen und die Anwendung, die Sie erhalten haben, mithilfe des Labs veröffentlichen. nutzen Sie dabei die von Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Azure-Portal

1. Wechseln Sie zum [Azure-Verwaltungsportal](https://manage.windowsazure.com/) , und melden Sie sich mit den mit Ihrem Abonnement verknüpften Microsoft-Anmelde Informationen an.

    > [!NOTE]
    > Mit Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt. Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.

    ![Anmelden bei Windows Azure-Portal](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Anmelden bei Windows Azure-Portal")

    *Anmelden beim Portal*
2. Klicken Sie in der Befehlsleiste auf **neu** .

    ![Erstellen einer neuen Website](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** - | **Website**. Wählen Sie dann **schneller** Fassung aus. Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Azure ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können. Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals in Azure bereitstellen. Sie enthält keine Schritte zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der schneller Fassung](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Erstellen einer neuen Website mithilfe der schneller Fassung")

    *Erstellen einer neuen Website mithilfe der schneller Fassung*
4. Warten Sie, bis die neue **Website** erstellt wurde.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte. Überprüfen Sie, ob die neue Website funktioniert.

    ![Navigieren zur neuen Website](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Navigieren zur neuen Website")

    *Navigieren zur neuen Website*

    ![Website wird ausgeführt](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Website wird ausgeführt")

    *Website wird ausgeführt*
6. Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.

    ![Öffnen der Website-Verwaltungs Seiten](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Öffnen der Website-Verwaltungs Seiten")

    *Öffnen der Website-Verwaltungs Seiten*
7. Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .

    > [!NOTE]
    > Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung in Azure für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind. Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist. **Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen in Azure zu automatisieren.

    ![Das Website-Veröffentlichungs Profil wird heruntergeladen.](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")

    *Das Website-Veröffentlichungs Profil wird heruntergeladen.*
8. Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter. In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung in Azure aus Visual Studio verwenden.

    ![Die Veröffentlichungs Profil Datei wird gespeichert.](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Das Veröffentlichungs Profil wird gespeichert.")

    *Die Veröffentlichungs Profil Datei wird gespeichert.*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbankservers

Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen. Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen. Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen. Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden. Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.

    ![Dashboard des SQL-Datenbankservers](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Dashboard des SQL-Datenbankservers")

    *Dashboard des SQL-Datenbankservers*
2. In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen. Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png)

    ![Client-IP-Adresse](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Client-IP-Adresse*
3. Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.

    ![Änderungen bestätigen](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Änderungen bestätigen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

1. Kehren Sie zur ASP.NET MVC 4-Lösung zurück. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.

    ![Veröffentlichen der Anwendung](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.

    ![Importieren des Veröffentlichungs Profils](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importieren des Veröffentlichungs Profils")

    *Veröffentlichungs Profil wird importiert.*
3. Klicken Sie auf **Verbindung**überprüfen. Klicken Sie nach Abschluss der Überprüfung auf **weiter**.

    > [!NOTE]
    > Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.

    ![Die Verbindung wird überprüft.](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Die Verbindung wird überprüft.")

    *Die Verbindung wird überprüft.*
4. Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).

    ![Webbereitstellungs Konfiguration](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Webbereitstellungs Konfiguration")

    *Webbereitstellungs Konfiguration*
5. Konfigurieren Sie die Datenbankverbindung wie folgt:

   - Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.
   - Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.
   - Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.
   - Geben Sie einen neuen Datenbanknamen ein.

     ![Konfigurieren der Ziel Verbindungs Zeichenfolge](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")

     *Konfigurieren der Ziel Verbindungs Zeichenfolge*
6. Klicken Sie dann auf **OK**. Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Erstellen der Daten Bank Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit SQL-Datenbank in Azure verwenden, wird im Textfeld Standardverbindung angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")

    *Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*
8. Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

![Beginnen Sie mit der Eingabe des Ausschnitt namens.](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

*Beginnen Sie mit der Eingabe des Ausschnitt namens.*

![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
2. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*
