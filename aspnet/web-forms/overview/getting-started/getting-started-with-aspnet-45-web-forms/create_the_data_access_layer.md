---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Erstellen der Datenzugriffs Ebene | Microsoft-Dokumentation
author: Erikre
description: Diese tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 0fcf050474a57be9ed53ec0783a6d6b7dde2bf4c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438375"
---
# <a name="create-the-data-access-layer"></a>Erstellen der Datenzugriffsschicht

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für das Web. Für diese tutorialreihe steht ein Visual Studio 2013- [Projekt mit C# Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) zur Verfügung.

In diesem Tutorial wird das Erstellen, zugreifen auf und Überprüfen von Daten aus einer Datenbank mithilfe von ASP.net Web Forms und Entity Framework Code First beschrieben. Dieses Tutorial baut auf dem vorherigen Tutorial "Erstellen des Projekts" auf und ist Teil der Wingtip Toy Store-tutorialreihe. Wenn Sie dieses Tutorial abgeschlossen haben, haben Sie eine Gruppe von Datenzugriffsklassen erstellt, die sich im Ordner " *Models* " des Projekts befinden.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Erstellen der Datenmodelle.
- Initialisieren und Seed der Datenbank.
- Aktualisieren und Konfigurieren der Anwendung zur Unterstützung der Datenbank.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Dies sind die im Tutorial eingeführten Features:

- Entity Framework Code First
- LocalDB
- Datenanmerkungen

## <a name="creating-the-data-models"></a>Erstellen der Datenmodelle

[Entity Framework](https://msdn.microsoft.com/data/aa937723) ist ein ORM-Framework (Object-Relational Mapping). Sie können mit relationalen Daten als Objekte arbeiten und den größten Teil des Datenzugriffs Codes eliminieren, den Sie normalerweise schreiben müssen. Mithilfe Entity Framework können Sie Abfragen mithilfe von [LINQ](https://msdn.microsoft.com/library/bb397926.aspx)ausgeben und Daten dann als stark typisierte Objekte abrufen und bearbeiten. LINQ stellt Muster zum Abfragen und Aktualisieren von Daten bereit. Wenn Sie Entity Framework verwenden, können Sie sich auf das Erstellen der restlichen Anwendung konzentrieren, anstatt sich auf die Grundlagen des Datenzugriffs zu konzentrieren. Später in dieser tutorialreihe erfahren Sie, wie die Daten zum Auffüllen von Navigations-und Produkt Abfragen verwendet werden.

Entity Framework unterstützt ein Entwicklungsparadigma namens *Code First*. Mit Code First können Sie die Datenmodelle mithilfe von Klassen definieren. Eine Klasse ist ein Konstrukt, mit dem Sie eigene benutzerdefinierte Typen erstellen können, indem Sie Variablen anderer Typen, Methoden und Ereignisse gruppieren. Sie können Klassen einer vorhandenen Datenbank zuordnen oder Sie verwenden, um eine Datenbank zu generieren. In diesem Tutorial erstellen Sie die Datenmodelle, indem Sie Datenmodell Klassen schreiben. Anschließend können Sie Entity Framework die Datenbank dynamisch aus diesen neuen Klassen erstellen.

Zunächst erstellen Sie die Entitäts Klassen, die die Datenmodelle für die Web Forms Anwendung definieren. Anschließend erstellen Sie eine Kontext Klasse, die die Entitäts Klassen verwaltet und den Datenzugriff auf die Datenbank ermöglicht. Außerdem erstellen Sie eine Initialisiererklasse, die Sie zum Auffüllen der Datenbank verwenden.

### <a name="entity-framework-and-references"></a>Entity Framework und Verweise

Standardmäßig ist Entity Framework enthalten, wenn Sie eine neue **ASP.NET-Webanwendung** mithilfe der **Web Forms** Vorlage erstellen. Entity Framework können als nuget-Paket installiert, deinstalliert und aktualisiert werden.

Dieses nuget-Paket enthält die folgenden **Laufzeitassemblys** in Ihrem Projekt:

- EntityFramework. dll – sämtlicher allgemeiner Lauf Zeit Code, der von Entity Framework
- EntityFramework. SqlServer. dll – der Microsoft SQL Server Anbieter für Entity Framework

### <a name="entity-classes"></a>Entitäts Klassen

Die Klassen, die Sie erstellen, um das Schema der Daten zu definieren, werden als Entitäts Klassen bezeichnet. Wenn Sie mit dem Daten bankentwurf noch nicht vertraut sind, stellen Sie sich die Entitäts Klassen als Tabellendefinitionen einer Datenbank vor. Jede Eigenschaft in der Klasse gibt eine Spalte in der Tabelle der Datenbank an. Diese Klassen stellen eine kompakte, Objekt relationale Schnittstelle zwischen objektorientiertem Code und der relationalen Tabellenstruktur der Datenbank bereit.

In diesem Tutorial beginnen Sie mit dem Hinzufügen einfacher Entitäts Klassen, die die Schemas für Produkte und Kategorien darstellen. Die Products-Klasse enthält Definitionen für jedes Produkt. Die Namen der einzelnen Member der Product-Klasse werden `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`und `Category`. Die Kategorieklasse enthält Definitionen für jede Kategorie, zu der ein Produkt gehören kann, z. b. Auto, Boots oder Plane. Die Namen der einzelnen Member der Category-Klasse werden `CategoryID`, `CategoryName`, `Description`und `Products`. Jedes Produkt gehört zu einer der Kategorien. Diese Entitäts Klassen werden dem Ordner "vorhandene *Modelle* " des Projekts hinzugefügt.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie dann -&gt; **Neues Element** **Hinzufügen** aus. 

    ![Erstellen der Datenzugriffs Ebene-Menü "Neues Element"](create_the_data_access_layer/_static/image1.png)

   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie unter  **C# Visual** im Bereich **installiert** auf der linken Seite die Option **Code**aus. 

    ![Erstellen der Datenzugriffs Ebene-Menü "Neues Element"](create_the_data_access_layer/_static/image2.png)
3. Wählen Sie im mittleren Bereich **Klasse** aus, und benennen Sie diese neue Klasse *Product.cs*.
4. Klicken Sie auf **Hinzufügen**.  
   Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch den folgenden Code:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Erstellen Sie eine weitere Klasse, indem Sie die Schritte 1 bis 4 wiederholen, benennen Sie jedoch die neue Klasse *Category.cs* , und ersetzen Sie den Standard Code durch den folgenden Code:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Wie bereits erwähnt, stellt die `Category`-Klasse den Typ des Produkts dar, das die Anwendung verkaufen soll (z <a id="a"></a> . b.&quot;Cars&quot;, &quot;Boote&quot;, &quot;-&quot;-`Product`), und die-Klasse stellt die einzelnen Produkte (Toys) in der-Datenbank dar. Jede Instanz eines `Product` Objekts entspricht einer Zeile in einer relationalen Datenbanktabelle, und jede Eigenschaft der Product-Klasse wird einer Spalte in der relationalen Datenbanktabelle zugeordnet. Später in diesem Tutorial überprüfen Sie die in der-Datenbank enthaltenen Produktdaten.

### <a name="data-annotations"></a>Datenanmerkungen

Möglicherweise haben Sie bemerkt, dass bestimmte Member der Klassen über Attribute verfügen, die Details zum Element angeben, z. b. `[ScaffoldColumn(false)]`. Hierbei handelt es sich um *Daten Anmerkungen*. Die Attribute für die Daten Anmerkung können beschreiben, wie Benutzereingaben für diesen Member überprüft werden, wie Formatierungen für ihn festgelegt werden und wie er beim Erstellen der Datenbank modelliert wird.

### <a name="context-class"></a>Context-Klasse

Um mit der Verwendung der Klassen für den Datenzugriff zu beginnen, müssen Sie eine Kontext Klasse definieren. Wie bereits erwähnt, verwaltet die Context-Klasse die Entitäts Klassen (z. b. die `Product`-Klasse und die `Category`-Klasse) und ermöglicht den Datenzugriff auf die Datenbank.

Diese Prozedur fügt dem Ordner C# " *Models* " eine neue Kontext Klasse hinzu.

1. Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie -&gt; **Neues Element** **Hinzufügen** aus.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie im mittleren Bereich **Klasse** aus, nennen Sie Sie *ProductContext.cs* , und klicken Sie auf **Hinzufügen**.
3. Ersetzen Sie den in der-Klasse enthaltenen Standard Code durch den folgenden Code:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Mit diesem Code wird der `System.Data.Entity`-Namespace hinzugefügt, sodass Sie auf alle Kernfunktionen von Entity Framework zugreifen können. dazu gehört auch die Möglichkeit, Daten durcharbeiten mit stark typisierten Objekten abzufragen, einzufügen, zu aktualisieren und zu löschen.

Die `ProductContext`-Klasse stellt Entity Framework Produktdaten Bank Kontext dar, der das Abrufen, speichern und Aktualisieren von `Product`-Klassen Instanzen in der Datenbank behandelt. Die `ProductContext`-Klasse wird von der `DbContext` Basisklasse abgeleitet, die von Entity Framework bereitgestellt wird.

### <a name="initializer-class"></a>Initialisiererklasse

Sie müssen eine benutzerdefinierte Logik ausführen, um die Datenbank zu initialisieren, wenn der Kontext zum ersten Mal verwendet wird. Dadurch können Seed-Daten zur Datenbank hinzugefügt werden, sodass Sie sofort Produkte und Kategorien anzeigen können.

Diese Prozedur fügt dem Ordner C# " *Models* " eine neue Initialisiererklasse hinzu.

1. Erstellen Sie einen weiteren `Class` im Ordner *Models* , und nennen Sie ihn *ProductDatabaseInitializer.cs*.
2. Ersetzen Sie den in der-Klasse enthaltenen Standard Code durch den folgenden Code:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Wie Sie aus dem obigen Code sehen können, wird die `Seed`-Eigenschaft beim Erstellen und Initialisieren der Datenbank überschrieben und festgelegt. Wenn die `Seed`-Eigenschaft festgelegt ist, werden die Werte aus den Kategorien und Produkten zum Auffüllen der Datenbank verwendet. Wenn Sie versuchen, die Ausgangsdaten zu aktualisieren, indem Sie den obigen Code ändern, nachdem die Datenbank erstellt wurde, werden keine Updates angezeigt, wenn Sie die-Webanwendung ausführen. Der Grund ist, dass der obige Code eine Implementierung der `DropCreateDatabaseIfModelChanges`-Klasse verwendet, um zu erkennen, ob sich das Modell (Schema) vor dem Zurücksetzen der Seed-Daten geändert hat. Wenn keine Änderungen an den `Category`-und `Product` Entitäts Klassen vorgenommen werden, wird die Datenbank nicht mit den Seed-Daten erneut initialisiert.

> [!NOTE] 
> 
> Wenn Sie möchten, dass die Datenbank jedes Mal, wenn Sie die Anwendung ausgeführt haben, neu erstellt wird, können Sie die `DropCreateDatabaseAlways`-Klasse anstelle der `DropCreateDatabaseIfModelChanges`-Klasse verwenden. Verwenden Sie für diese tutorialreihe jedoch die `DropCreateDatabaseIfModelChanges`-Klasse.

An dieser Stelle dieses Tutorials verfügen Sie über einen Ordner " *Models* " mit vier neuen Klassen und einer Standardklasse:

![Erstellen des Ordners "Datenzugriffs Schicht-Modelle"](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Konfigurieren der Anwendung für die Verwendung des Datenmodells

Nachdem Sie die Klassen erstellt haben, die die Daten darstellen, müssen Sie die Anwendung so konfigurieren, dass die Klassen verwendet werden. Fügen Sie in der Datei *Global. asax* Code hinzu, mit dem das Modell initialisiert wird. In der Datei *Web. config* fügen Sie Informationen hinzu, die der Anwendung mitteilen, welche Datenbank Sie zum Speichern der Daten verwenden, die von den neuen Daten Klassen dargestellt werden. Die Datei " *Global. asax* " kann verwendet werden, um Anwendungs Ereignisse oder-Methoden zu verarbeiten. Mithilfe der Datei " *Web. config* " können Sie die Konfiguration Ihrer ASP.NET-Webanwendung steuern.

#### <a name="updating-the-globalasax-file"></a>Aktualisieren der Datei "Global. asax"

Um die Datenmodelle zu initialisieren, wenn die Anwendung gestartet wird, aktualisieren Sie den `Application_Start`-Handler in der Datei *Global.asax.cs* .

> [!NOTE] 
> 
> In Projektmappen-Explorer können Sie entweder die Datei " *Global. asax* " oder die Datei " *Global.asax.cs* " auswählen, um die *Global.asax.cs* -Datei zu bearbeiten.

1. Fügen Sie den folgenden Code, der in gelb hervorgehoben ist, der `Application_Start`-Methode in der Datei *Global.asax.cs* hinzu.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Ihr Browser muss HTML5 unterstützen, um den in gelb markierten Code anzuzeigen, wenn Sie diese tutorialreihe in einem Browser anzeigen.

Wie im obigen Code gezeigt, gibt die Anwendung beim Starten der Anwendung den Initialisierer an, der beim ersten Zugriff auf die Daten ausgeführt wird. Die beiden zusätzlichen Namespaces sind erforderlich, um auf das `Database` Objekt und das `ProductDatabaseInitializer` Objekt zuzugreifen.

 Ändern der Datei "Web. config" 

Obwohl Entity Framework Code First eine Datenbank für Sie an einem Standard Speicherort generiert, wenn die Datenbank mit Ausgangsdaten aufgefüllt wird, erhalten Sie durch das Hinzufügen Ihrer eigenen Verbindungsinformationen zu Ihrer Anwendung die Kontrolle über den Speicherort der Datenbank. Sie geben diese Datenbankverbindung mithilfe einer Verbindungs Zeichenfolge in der Datei " *Web. config* " der Anwendung im Stammverzeichnis des Projekts an. Indem Sie eine neue Verbindungs Zeichenfolge hinzufügen, können Sie den Speicherort der Datenbank (*wingtiptoys. mdf*), der im Datenverzeichnis der Anwendung (*App\_Daten*) erstellt werden soll, anstelle des Standard Speicher Orts weiterleiten. Wenn Sie diese Änderung vornehmen, können Sie die Datenbankdatei später in diesem Tutorial suchen und überprüfen.

1. Suchen Sie in **Projektmappen-Explorer**nach der Datei *Web. config* , und öffnen Sie Sie.
2. Fügen Sie die Verbindungs Zeichenfolge, die gelb hervorgehoben ist, dem `<connectionStrings>` Abschnitt der Datei " *Web. config* " wie folgt hinzu:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Wenn die Anwendung zum ersten Mal ausgeführt wird, erstellt Sie die Datenbank an dem Speicherort, der durch die Verbindungs Zeichenfolge angegeben wird. Bevor Sie die Anwendung ausführen, erstellen wir Sie zunächst.

## <a name="building-the-application"></a>Erstellen der Anwendung

Um sicherzustellen, dass alle Klassen und Änderungen an der Webanwendung funktionieren, sollten Sie die Anwendung erstellen.

1. Wählen Sie im Menü **Debuggen** die Option **wingtiptoys erstellen**aus.  
 Das Fenster **Ausgabe** wird angezeigt, und wenn alles gut funktioniert, wird eine Nachricht *erfolgreich* angezeigt.  

    ![Erstellen der Datenzugriffs Ebene: Ausgabefenster](create_the_data_access_layer/_static/image4.png)

Wenn ein Fehler auftritt, überprüfen Sie die obigen Schritte erneut. Die Informationen im **Ausgabe** Fenster geben an, in welcher Datei ein Problem aufgetreten ist und wo in der Datei eine Änderung erforderlich ist. Mithilfe dieser Informationen können Sie bestimmen, welcher Teil der obigen Schritte in Ihrem Projekt überprüft und korrigiert werden muss.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial der Reihe, in der Sie das Datenmodell erstellt haben, und wurde der Code hinzugefügt, der verwendet wird, um die Datenbank zu initialisieren und zu erstellen. Außerdem haben Sie die Anwendung so konfiguriert, dass Sie die Datenmodelle verwendet, wenn die Anwendung ausgeführt wird.

Im nächsten Tutorial aktualisieren Sie die Benutzeroberfläche, fügen die Navigation hinzu und rufen Daten aus der Datenbank ab. Dies führt dazu, dass die Datenbank automatisch basierend auf den Entitäts Klassen erstellt wird, die Sie in diesem Tutorial erstellt haben.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Entity Framework Übersicht](https://msdn.microsoft.com/library/bb399567.aspx)   
[Leitfaden für Einsteiger zum ADO.NET-Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Code First Entwicklung mit Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (Video)   
  der [Code First-Beziehung](https://msdn.microsoft.com/data/hh134698)  
[Code First Data Annotations](https://msdn.microsoft.com/data/gg193958)  
[Produktivitätsverbesserungen für die Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Zurück](create-the-project.md)
> [Weiter](ui_and_navigation.md)
