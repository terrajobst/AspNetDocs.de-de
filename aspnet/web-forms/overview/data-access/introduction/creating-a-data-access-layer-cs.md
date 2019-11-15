---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Erstellen einer Datenzugriffs Ebene (C#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial beginnen wir von Anfang an und erstellen die Datenzugriffs Schicht (Data Access Layer, DAL) unter Verwendung typisierter Datasets, um auf die Informationen in einer Datenbank zuzugreifen.
ms.author: riande
ms.date: 04/05/2010
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 5aaf97dc8448dcb7b94ef2e4e23f34fd37ac4426
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115213"
---
# <a name="creating-a-data-access-layer-c"></a>Erstellen einer Datenzugriffsschicht (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> In diesem Tutorial beginnen wir von Anfang an und erstellen die Datenzugriffs Schicht (Data Access Layer, DAL) unter Verwendung typisierter Datasets, um auf die Informationen in einer Datenbank zuzugreifen.

## <a name="introduction"></a>Einführung

Bei Webentwicklern dreht sich unser Leben um die Arbeit mit Daten. Wir erstellen Datenbanken zum Speichern der Daten, zum Abrufen und Ändern von Code und zum Erfassen und Zusammenfassen von Webseiten. Dies ist das erste Tutorial in einer langen Reihe, in dem Techniken zum Implementieren dieser allgemeinen Muster in ASP.NET 2,0 erläutert werden. Wir beginnen mit der Erstellung einer [Softwarearchitektur](http://en.wikipedia.org/wiki/Software_architecture) , die aus einer Datenzugriffs Schicht (Data Access Layer, DAL) unter Verwendung typisierter Datasets besteht, einer Geschäftslogik Schicht (Business Logic Layer, BLL), die benutzerdefinierte Geschäftsregeln erzwingt, und einer Präsentationsschicht bestehend aus ASP.NET Seiten, die ein gemeinsames Seitenlayout gemeinsam Nachdem diese Back-End-Grundlagen festgelegt wurde, werden wir uns mit der Berichterstellung beschäftigen und zeigen, wie Sie Daten aus einer Webanwendung anzeigen, zusammenfassen, erfassen und validieren. Diese Lernprogramme sind so konzipiert, dass Sie präzise sind, und enthalten Schritt-für-Schritt-Anleitungen mit vielen Screenshots, um Sie durch den Prozess visuell zu führen. Jedes Tutorial ist in C# und Visual Basic Versionen verfügbar und enthält einen Download des gesamten verwendeten Codes. (Dieses erste Lernprogramm ist recht lang, aber der Rest wird in wesentlich mehr verdaulichen Blöcken dargestellt.)

Für diese Tutorials verwenden wir eine Microsoft SQL Server 2005 Express Edition-Version der Northwind-Datenbank, die im **App-\_Daten** Verzeichnis platziert wird. Zusätzlich zur Datenbankdatei enthält der Ordner **App\_Data** auch die SQL-Skripts zum Erstellen der Datenbank, falls Sie eine andere Datenbankversion verwenden möchten. Diese Skripts können auch [direkt von Microsoft heruntergeladen](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)werden, wenn Sie es vorziehen. Wenn Sie eine andere SQL Server Version der Northwind-Datenbank verwenden, müssen Sie die **northwndconnectionstring** -Einstellung in der Datei " **Web. config** " der Anwendung aktualisieren. Die Webanwendung wurde mithilfe von Visual Studio 2005 Professional Edition als auf einem Dateisystem basierendes Website Projekt erstellt. Allerdings funktionieren alle Tutorials auch mit der kostenlosen Version von Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/), gleichermaßen gut.  
  
In diesem Tutorial beginnen wir von Anfang an mit der Erstellung der Datenzugriffs Schicht (Data Access Layer, DAL), gefolgt von der Erstellung der Geschäftslogik Schicht (Business Logic Layer, BLL) im zweiten Tutorial und dem Arbeiten an Seitenlayout und Navigation in der dritten. Die Tutorials nach dem dritten basieren auf der Grundlage der Grundlage der ersten drei. Wir haben in diesem ersten Tutorial viel zu tun, also sollten Sie Visual Studio starten und beginnen!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Schritt 1: Erstellen eines Webprojekts und Herstellen einer Verbindung mit der Datenbank

Bevor wir unsere Datenzugriffs Schicht (Data Access Layer, DAL) erstellen können, müssen wir zuerst eine Website erstellen und unsere Datenbank einrichten. Beginnen Sie mit dem Erstellen einer neuen Dateisystem basierten ASP.NET-Website. Wechseln Sie hierzu zum Menü Datei, und wählen Sie neue Website und dann das Dialogfeld neue Website aus. Wählen Sie die Vorlage ASP.NET-Website aus, legen Sie die Dropdown Liste Speicherort auf Datei System fest, wählen Sie einen Ordner zum Platzieren der Website aus, C#und legen Sie die Sprache auf fest.

[![Erstellen einer neuen Datei System basierten Website](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Abbildung 1**: Erstellen einer neuen Datei System basierten Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image3.png))

Dadurch wird eine neue Website mit der ASP.NET-Seite " **default. aspx** " und einem **App-\_Daten** Ordner erstellt.

Nachdem die Website erstellt wurde, besteht der nächste Schritt im Hinzufügen eines Verweises auf die Datenbank in der Server-Explorer von Visual Studio. Indem Sie der Server-Explorer eine Datenbank hinzufügen, können Sie in Visual Studio Tabellen, gespeicherte Prozeduren, Sichten usw. hinzufügen. Sie können auch Tabellendaten anzeigen oder eigene Abfragen entweder Hand oder grafisch über die Abfrage-Generator erstellen. Außerdem müssen wir beim Erstellen der typisierten Datasets für die DAL Visual Studio auf die Datenbank verweisen, aus der die typisierten Datasets erstellt werden sollen. Obwohl wir diese Verbindungsinformationen zu diesem Zeitpunkt bereitstellen können, füllt Visual Studio automatisch eine Dropdown Liste der Datenbanken auf, die bereits im Server-Explorer registriert sind.

Die Schritte zum Hinzufügen der Northwind-Datenbank zu den Server-Explorer sind davon abhängig, ob Sie die SQL Server 2005 Express Edition-Datenbank im Datenordner der **App\_** verwenden möchten oder ob Sie stattdessen über eine Microsoft SQL Server 2000-oder 2005-Daten Bank Server-Setup verfügen, die Sie stattdessen verwenden möchten.

## <a name="using-a-database-in-the-app_data-folder"></a>Verwenden einer Datenbank im App-\_Datenordner

Wenn Sie nicht über einen SQL Server 2000-oder 2005-Datenbankserver verfügen, mit dem eine Verbindung hergestellt werden soll, oder wenn Sie die Datenbank nur einem Datenbankserver hinzufügen möchten, können Sie die SQL Server 2005 Express Edition-Version der Northwind-Datenbank verwenden, die sich in der **App\_Daten** Ordner der heruntergeladenen Website befindet (**Northwnd). MDF**).

Eine Datenbank, die in den **App-\_Daten** Ordner eingefügt wird, wird automatisch der Server-Explorer hinzugefügt. Wenn Sie SQL Server 2005 Express Edition auf Ihrem Computer installiert haben, sollte ein Knoten mit dem Namen Northwnd angezeigt werden. MDF im Server-Explorer, das Sie erweitern und die Tabellen, Sichten, gespeicherten Prozeduren usw. untersuchen können (siehe Abbildung 2).

Der **App-\_Daten** Ordner kann auch Microsoft Access **. mdb** -Dateien enthalten, die wie Ihre SQL Server Entsprechungen automatisch dem Server-Explorer hinzugefügt werden. Wenn Sie keine SQL Server Optionen verwenden möchten, können Sie jederzeit [eine Microsoft Access-Version der Northwind-Datenbankdatei herunterladen](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) und in das Datenverzeichnis der **App\_** . Denken Sie jedoch daran, dass der Zugriff auf Datenbanken nicht so umfangreich wie SQL Server ist und nicht für die Verwendung in Website Szenarien konzipiert ist. Außerdem werden in einigen der 35-Tutorials bestimmte Funktionen auf Datenbankebene verwendet, die nicht durch den Zugriff unterstützt werden.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Herstellen einer Verbindung mit der Datenbank in einem Microsoft SQL Server 2000-oder 2005-Daten Bank Server

Alternativ dazu können Sie eine Verbindung mit einer Northwind-Datenbank herstellen, die auf einem Datenbankserver installiert ist. Wenn auf dem Datenbankserver die Northwind-Datenbank noch nicht installiert ist, müssen Sie Sie zuerst dem Datenbankserver hinzufügen, indem Sie das Installationsskript ausführen, das im Download dieses Tutorials enthalten ist, oder indem Sie [die SQL Server 2000-Version von Northwind und das Installationsskript](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) direkt von der Website von Microsoft herunterladen.

Wenn Sie die Datenbank installiert haben, navigieren Sie zum Server-Explorer in Visual Studio, klicken Sie mit der rechten Maustaste auf den Knoten Datenverbindungen, und wählen Sie dann Verbindung hinzufügen aus. Wenn die Server-Explorer nicht angezeigt wird, wechseln Sie zur Ansicht/Server-Explorer, oder drücken Sie STRG + ALT + S. Dadurch wird das Dialogfeld Verbindung hinzufügen angezeigt, in dem Sie den Server, mit dem eine Verbindung hergestellt werden soll, die Authentifizierungsinformationen und den Datenbanknamen angeben können. Nachdem Sie die Daten bankverbindungs Informationen erfolgreich konfiguriert und auf die Schaltfläche OK geklickt haben, wird die Datenbank als Knoten unterhalb des Knotens Datenverbindungen hinzugefügt. Sie können den Datenbankknoten erweitern, um die Tabellen, Sichten, gespeicherten Prozeduren usw. zu untersuchen.

![Hinzufügen einer Verbindung mit der Northwind-Datenbank des Datenbankservers](creating-a-data-access-layer-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen einer Verbindung mit der Northwind-Datenbank des Datenbankservers

## <a name="step-2-creating-the-data-access-layer"></a>Schritt 2: Erstellen der Datenzugriffs Ebene

Beim Arbeiten mit Daten besteht eine Option darin, die Daten spezifische Logik direkt in die Darstellungs Schicht einzubetten (in einer Webanwendung bilden die ASP.NET Seiten die Darstellungs Schicht). Dies kann das Schreiben von ADO.NET-Code in den Codeteil der ASP.NET-Seite oder das Verwenden des SqlDataSource-Steuer Elements aus dem Markup Teil in Anspruch nehmen. In beiden Fällen verbindet diese Methode die Datenzugriffs Logik eng mit der Präsentationsebene. Die empfohlene Vorgehensweise besteht jedoch darin, die Datenzugriffs Logik von der Darstellungs Schicht zu trennen. Diese separate Ebene wird als Datenzugriffs Schicht, kurz Dal, und wird in der Regel als separates Klassen Bibliotheksprojekt implementiert. Die Vorteile dieser geschichteten Architektur sind gut dokumentiert (Weitere Informationen zu diesen Vorteilen finden Sie im Abschnitt "Weitere Messwerte" am Ende dieses Tutorials) und ist der Ansatz, den wir in dieser Reihe durchführen werden.

Sämtlicher Code, der spezifisch für die zugrunde liegende Datenquelle ist, z. b. das Erstellen einer Verbindung mit der Datenbank, das Ausstellen von **Select**-, **Insert**-, **Update**-und **Delete** -Befehlen usw., sollte in der dal gefunden werden. Die Darstellungs Schicht sollte keine Verweise auf diesen Datenzugriffs Code enthalten, sondern stattdessen Aufrufe an die DAL für beliebige und alle Datenanforderungen richten. Datenzugriffsebenen enthalten in der Regel Methoden für den Zugriff auf die zugrunde liegenden Datenbankdaten. Die Datenbank Northwind enthält z. b. Tabellen mit **Produkten** und **Kategorien** , in denen die zu verkaufende Produkte und die Kategorien aufgezeichnet werden, zu denen Sie gehören. In unserer dal verfügen wir über Methoden wie:

- **GetCategories ():** gibt Informationen zu allen Kategorien zurück.
- **GetProducts ()** , die Informationen zu allen Produkten zurückgibt
- **Getproducungbycategoryid (*CategoryID*)** , die alle Produkte zurückgibt, die zu einer angegebenen Kategorie gehören.
- **Getproductbyproductid (*ProductID*)** , das Informationen zu einem bestimmten Produkt zurückgibt

Wenn diese Methoden aufgerufen werden, wird eine Verbindung mit der Datenbank hergestellt, die entsprechende Abfrage ausgegeben und die Ergebnisse zurückgegeben. Wie wir diese Ergebnisse zurückgeben, ist wichtig. Diese Methoden könnten einfach ein DataSet oder einen DataReader zurückgeben, das von der Datenbankabfrage aufgefüllt wird, aber im Idealfall sollten diese Ergebnisse mit *stark typisierten Objekten*zurückgegeben werden. Ein stark typisiertes Objekt ist ein Objekt, dessen Schema zum Zeitpunkt der Kompilierung streng definiert ist, wohingegen das Gegenteil, ein lose typisiertes Objekt, ein Objekt ist, dessen Schema bis zur Laufzeit nicht bekannt ist.

Beispielsweise handelt es sich bei dem DataReader-und dem DataSet (standardmäßig) um lose typisierte Objekte, da das Schema durch die Spalten definiert ist, die von der zum Auffüllen verwendeten Datenbankabfrage zurückgegeben werden. Für den Zugriff auf eine bestimmte Spalte aus einer lose typisierten Datentabelle muss die Syntax wie folgt verwendet werden:  <strong><em>datdatababel</em>. Rows [<em>Index</em>] ["<em>ColumnName</em>"]</strong>. Die lose Typisierung der Datentabelle in diesem Beispiel ist darauf zurückzuführen, dass wir mit einer Zeichenfolge oder einem Ordinalindex auf den Spaltennamen zugreifen müssen. Eine stark typisierte Datentabelle hingegen wird jede der Spalten als Eigenschaften implementiert, was zu Code führt, der wie folgt aussieht:  <strong><em>datdatababel</em>. Zeilen [<em>Index</em>]. *ColumnName</strong>* .

Um stark typisierte Objekte zurückzugeben, können Entwickler entweder eigene benutzerdefinierte Geschäftsobjekte erstellen oder typisierte Datasets verwenden. Ein Geschäftsobjekt wird vom Entwickler als Klasse implementiert, deren Eigenschaften üblicherweise die Spalten der zugrunde liegenden Datenbanktabelle widerspiegeln, die das Geschäftsobjekt darstellt. Ein typisiertes DataSet ist eine Klasse, die von Visual Studio basierend auf einem Datenbankschema generiert wird und dessen Member gemäß diesem Schema stark typisiert sind. Das typisierte Dataset selbst besteht aus Klassen, die die Klassen ADO.NET DataSet, Datable und DataRow erweitern. Zusätzlich zu stark typisierten DataTables enthalten typisierte Datasets jetzt auch TableAdapters, bei denen es sich um Klassen mit Methoden zum Auffüllen der DataTables des Datasets und zum erneuten Verteilen von Änderungen in den DataTables an die Datenbank handelt.

> [!NOTE]
> Weitere Informationen zu den vor-und Nachteile der Verwendung typisierter Datasets im Vergleich zu benutzerdefinierten Geschäftsobjekten finden Sie unter [Entwerfen von Datenebenenkomponenten und übergeben von Daten durch Ebenen](https://msdn.microsoft.com/library/ms978496.aspx).

Wir verwenden stark typisierte Datasets für die Architektur dieser Tutorials. In Abbildung 3 wird der Workflow zwischen den verschiedenen Ebenen einer Anwendung veranschaulicht, die typisierte Datasets verwendet.

[![sämtlicher Datenzugriffs Code an die DAL verwiesen wird](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Abbildung 3**: der gesamte Datenzugriffs Code wird an die DAL verwiesen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image7.png))

## <a name="creating-a-typed-dataset-and-table-adapter"></a>Erstellen eines typisierten Datasets und Tabellen Adapters

Um mit dem Erstellen unserer dal zu beginnen, fügen wir dem Projekt zunächst ein typisiertes Dataset hinzu. Klicken Sie hierzu im Projektmappen-Explorer mit der rechten Maustaste auf den Projekt Knoten, und wählen Sie neues Element hinzufügen aus. Wählen Sie in der Liste der Vorlagen die Option DataSet aus, und nennen Sie Sie **Northwind. xsd**.

[![auswählen, dem Projekt ein neues Dataset hinzuzufügen.](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Abbildung 4**: Auswählen eines neuen Datasets zum Projekt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image10.png))

Nachdem Sie auf Hinzufügen geklickt haben, klicken Sie auf Ja, wenn Sie aufgefordert werden, das DataSet dem **App-\_Code** Ordner hinzuzufügen Der Designer für das typisierte DataSet wird dann angezeigt, und der TableAdapter-Konfigurations-Assistent wird gestartet, sodass Sie dem typisierten DataSet ihren ersten TableAdapter hinzufügen können.

Ein typisiertes DataSet dient als stark typisierte Auflistung von Daten. Sie besteht aus stark typisierten Datentabelle-Instanzen, die wiederum aus stark typisierten DataRow-Instanzen bestehen. Wir erstellen eine stark typisierte Datentabelle für jede der zugrunde liegenden Datenbanktabellen, mit denen wir in dieser Reihe von Tutorials arbeiten müssen. Beginnen wir mit dem Erstellen einer Datentabelle für die Tabelle " **Products** ".

Beachten Sie, dass stark typisierte DataTables keine Informationen zum Zugriff auf Daten aus der zugrunde liegenden Datenbanktabelle enthalten. Um die Daten zum Auffüllen der Datentabelle abzurufen, verwenden wir eine TableAdapter-Klasse, die als Datenzugriffs Schicht fungiert. Für die Datentabelle " **Produkte** " enthält der TableAdapter die Methoden " **GetProducts ()** ", " **getproductbycategoryid" (*CategoryID*)** usw., die wir von der Präsentationsebene aus aufrufen. Die Rolle der Datentabelle besteht darin, als stark typisierte Objekte zu fungieren, die zum Übergeben von Daten zwischen den Ebenen verwendet werden.

Der TableAdapter-Konfigurations-Assistent beginnt mit der Aufforderung, auszuwählen, mit welcher Datenbank Sie arbeiten möchten. In der Dropdown Liste werden diese Datenbanken in der Server-Explorer angezeigt. Wenn Sie die Datenbank Northwind nicht zum Server-Explorer hinzugefügt haben, können Sie zu diesem Zeitpunkt auf die Schaltfläche neue Verbindung klicken.

[![Sie in der Dropdown Liste die Datenbank Northwind aus.](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Abbildung 5**: Auswählen der Northwind-Datenbank in der Dropdown Liste ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image13.png))

Nachdem Sie die Datenbank ausgewählt und auf "weiter" geklickt haben, werden Sie gefragt, ob Sie die Verbindungs Zeichenfolge in der Datei " **Web. config** " speichern möchten. Wenn Sie die Verbindungs Zeichenfolge speichern, sollten Sie in den TableAdapter-Klassen nicht hart codiert sein, was die Dinge vereinfacht, wenn sich die Verbindungs Zeichenfolgen-Informationen in Zukunft ändern. Wenn Sie die Verbindungs Zeichenfolge in der Konfigurationsdatei speichern, befindet Sie sich im Abschnitt **&lt;connectionStrings&gt;** , der optional für eine verbesserte Sicherheit [verschlüsselt](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) oder später über die neue ASP.NET 2,0-Eigenschaften Seite im IIS GUI Admin Tool geändert werden kann, was für Administratoren ideal ist.

[![die Verbindungs Zeichenfolge in "Web. config" speichern.](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Abbildung 6**: Speichern der Verbindungs Zeichenfolge in " **Web. config** " ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image16.png))

Als nächstes müssen wir das Schema für die erste Datentabelle mit starker Typisierung definieren und die erste Methode angeben, die der TableAdapter beim Auffüllen des stark typisierten Datasets verwenden soll. Diese beiden Schritte werden gleichzeitig ausgeführt, indem eine Abfrage erstellt wird, die die Spalten aus der Tabelle zurückgibt, die in unserer Datentabelle wiedergegeben werden sollen. Am Ende des Assistenten wird dieser Abfrage ein Methodenname übergeben. Nachdem dies geschehen ist, kann diese Methode von der Präsentationsebene aus aufgerufen werden. Die-Methode führt die definierte Abfrage aus und füllt eine stark typisierte Datentabelle auf.

Um mit der Definition der SQL-Abfrage zu beginnen, müssen Sie zunächst angeben, wie der TableAdapter die Abfrage ausgeben soll. Wir können eine Ad-hoc-SQL-Anweisung verwenden, eine neue gespeicherte Prozedur erstellen oder eine vorhandene gespeicherte Prozedur verwenden. Für diese Tutorials verwenden wir Ad-hoc-SQL-Anweisungen. Ein Beispiel für die Verwendung von gespeicherten Prozeduren finden Sie im Artikel zu [Brian Noyes](http://briannoyes.net/), [Erstellen einer Datenzugriffs Ebene mit dem Visual Studio 2005-DataSet-Designer](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) .

[![Abfragen der Daten mit einer Ad-hoc-SQL-Anweisung](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Abbildung 7**: Abfragen der Daten mit einer Ad-hoc-SQL-Anweisung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image19.png))

An dieser Stelle können wir die SQL-Abfrage in Hand eingeben. Wenn Sie die erste Methode im TableAdapter erstellen, möchten Sie in der Regel, dass die Abfrage die Spalten zurückgibt, die in der entsprechenden Datentabelle ausgedrückt werden müssen. Dies erreichen Sie, indem Sie eine Abfrage erstellen, die alle Spalten und alle Zeilen aus der **Products** -Tabelle zurückgibt:

[![geben Sie die SQL-Abfrage in das Textfeld ein.](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Abbildung 8**: Geben Sie die SQL-Abfrage in das Textfeld ein ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image22.png)).

Verwenden Sie alternativ die Abfrage-Generator, und erstellen Sie die Abfrage grafisch, wie in Abbildung 9 dargestellt.

[![Sie die Abfrage mithilfe des Abfrage-Editors grafisch zu erstellen.](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Abbildung 9**: grafisch erstellen der Abfrage mithilfe des Abfrage-Editors ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image25.png))

Nachdem Sie die Abfrage erstellt haben, aber bevor Sie auf den nächsten Bildschirm wechseln, klicken Sie auf die Schaltfläche Erweiterte Optionen. In Website Projekten ist "INSERT-, Update-und DELETE-Anweisungen generieren" die einzige erweiterte Option, die standardmäßig ausgewählt ist. Wenn Sie diesen Assistenten aus einer Klassenbibliothek oder einem Windows-Projekt ausführen, wird auch die Option "vollständige Parallelität verwenden" ausgewählt. Lassen Sie die Option "optimistische Parallelität verwenden" deaktiviert. Die optimistische Parallelität wird in zukünftigen Tutorials untersucht.

[![nur die Option INSERT-, Update-und DELETE-Anweisungen generieren auswählen](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Abbildung 10**: Wählen Sie nur die Option INSERT-, Update-und DELETE-Anweisungen generieren aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image28.png)).

Nachdem Sie die erweiterten Optionen überprüft haben, klicken Sie auf Weiter, um mit dem letzten Bildschirm fortzufahren. Hier werden wir aufgefordert, auszuwählen, welche Methoden dem TableAdapter hinzugefügt werden sollen. Es gibt zwei Muster zum Auffüllen von Daten:

- **Datentabelle ausfüllen** mit diesem Ansatz wird eine Methode erstellt, die eine Datentabelle als Parameter annimmt und Sie basierend auf den Ergebnissen der Abfrage auffüllt. Die ADO.NET DataAdapter-Klasse implementiert dieses Muster z. b. mit der **Fill ()** -Methode.
- **Zurückgeben einer Daten** Tabelle mit diesem Ansatz die Methode erstellt und füllt die Datentabelle für Sie und gibt Sie als Rückgabewert der Methode zurück.

Der TableAdapter kann ein oder beide dieser Muster implementieren. Sie können auch die hier bereitgestellten Methoden umbenennen. Lassen Sie beide Kontrollkästchen aktiviert, auch wenn in diesen Tutorials nur das letztere Muster verwendet wird. Benennen wir außerdem die eher generische **GetData** -Methode in " **GetProducts**" um.

Wenn dieses Kontrollkästchen aktiviert ist, erstellt das letzte Kontrollkästchen "GenerateDBDirectMethods" eine **Insert ()** -, **Update ()** -und **Delete ()** -Methode für den TableAdapter. Wenn Sie diese Option nicht aktiviert lassen, müssen alle Updates über die Methode "Single **Update ()** " des TableAdapter durchgeführt werden, die das typisierte DataSet, eine Datentabelle, eine einzelne DataRow oder ein Array von "DataRows" annimmt. (Wenn Sie die Option "INSERT-, Update-und DELETE-Anweisungen generieren" in den erweiterten Eigenschaften in Abbildung 9 deaktiviert haben, hat die Einstellung dieses Kontrollkästchens keine Auswirkung.) Lassen Sie dieses Kontrollkästchen deaktiviert.

[![Ändern des Methoden namens von "GetData" in "GetProducts"](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Abbildung 11**: Ändern des Methoden namens von " **GetData** " in " **GetProducts** " ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image31.png))

Beenden Sie den Assistenten, indem Sie auf Fertigstellen klicken. Nachdem der Assistent geschlossen wurde, werden Sie an den DataSet-Designer zurückgegeben, der die soeben erstellte Datentabelle anzeigt. Die Liste der Spalten in der Datentabelle " **Products** " (**ProductID**, **ProductName**usw.) sowie die Methoden von **ProductsTableAdapter** (**Fill ()** und **GetProducts ()** ) können angezeigt werden.

[![die Datentabelle "Products" und "ProductsTableAdapter" dem typisierten DataSet hinzugefügt.](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Abbildung 12**: die Datentabelle " **Products** " und " **ProductsTableAdapter** " wurden dem typisierten DataSet hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image34.png))

An diesem Punkt verfügen wir über ein typisiertes DataSet mit einer einzelnen Datentabelle (**Northwind. Products**) und einer stark typisierten DataAdapter-Klasse (**northwindtableadapters. ProductsTableAdapter**) mit einer **GetProducts ()** -Methode. Diese Objekte können verwendet werden, um auf eine Liste aller Produkte aus Code wie zu zugreifen:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Dieser Code erforderte nicht das Schreiben eines Bits von Datenzugriffs spezifischem Code. Es mussten keine ADO.NET-Klassen instanziiert werden. es mussten keine Verbindungs Zeichenfolgen, SQL-Abfragen oder gespeicherten Prozeduren bezogen werden. Stattdessen stellt der TableAdapter den Datenzugriffs Code auf niedriger Ebene für uns bereit.

Jedes in diesem Beispiel verwendete Objekt ist ebenfalls stark typisiert und ermöglicht Visual Studio die Bereitstellung von IntelliSense und der Typüberprüfung zur Kompilierzeit. Und das Beste aus allen vom TableAdapter zurückgegebenen DataTables kann an ASP.net-datenweb Steuerelemente gebunden werden, z. b. GridView, DetailsView, DropDownList, CheckBoxList und verschiedene andere. Im folgenden Beispiel wird veranschaulicht, wie die Datentabelle, die von der **GetProducts ()** -Methode zurückgegeben wurde, an ein GridView-Steuerelemente in nur wenigen drei Codezeilen innerhalb der **Seite\_Lade** Ereignishandler gebunden wird.

Allproducts. aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]

[![die Liste der Produkte in einer GridView angezeigt wird](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Abbildung 13**: die Liste der Produkte wird in einer GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image37.png))

Obwohl dieses Beispiel erfordert, dass wir drei Codezeilen auf der **Seite\_Load** -Ereignishandler unserer ASP.NET-Seite schreiben, untersuchen wir in zukünftigen Tutorials, wie Sie ObjectDataSource verwenden, um die Daten deklarativ aus der dal abzurufen. Mit ObjectDataSource müssen wir keinen Code schreiben, und es werden auch Paging-und Sortierungs Unterstützung unterstützt.

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Schritt 3: Hinzufügen parametrisierter Methoden zur Datenzugriffs Ebene

An diesem Punkt verfügt unsere **ProductsTableAdapter** -Klasse über eine Methode, **GetProducts ()** , die alle Produkte in der Datenbank zurückgibt. Obwohl die Arbeit mit allen Produkten absolut sinnvoll ist, gibt es Zeiten, in denen wir Informationen zu einem bestimmten Produkt oder zu allen Produkten abrufen möchten, die zu einer bestimmten Kategorie gehören. Um dieser Datenzugriffs Ebene eine solche Funktionalität hinzuzufügen, können wir dem TableAdapter parametrisierte Methoden hinzufügen.

Fügen Sie die **getproduczbycategoryid (*CategoryID*)** -Methode hinzu. Kehren Sie zum Hinzufügen einer neuen Methode zur dal zurück zum DataSet-Designer, klicken Sie mit der rechten Maustaste in den Abschnitt **ProductsTableAdapter** , und wählen Sie Abfrage hinzufügen aus.

![Klicken Sie mit der rechten Maustaste auf den TableAdapter und wählen Sie Abfrage hinzufügen](creating-a-data-access-layer-cs/_static/image38.png)

**Abbildung 14**: Klicken Sie mit der rechten Maustaste auf den TableAdapter, und wählen Sie Abfrage hinzufügen

Wir werden zuerst gefragt, ob Sie mit einer Ad-hoc-SQL-Anweisung oder einer neuen oder vorhandenen gespeicherten Prozedur auf die Datenbank zugreifen möchten. Wir möchten eine Ad-hoc-SQL-Anweisung erneut verwenden. Als nächstes werden wir gefragt, welche Art von SQL-Abfrage wir verwenden möchten. Da wir alle Produkte zurückgeben möchten, die zu einer bestimmten Kategorie gehören, möchten wir eine **Select** -Anweisung schreiben, die Zeilen zurückgibt.

[![erstellen Sie eine SELECT-Anweisung, die Zeilen zurückgibt](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Abbildung 15**: Auswählen der Erstellung einer **Select** -Anweisung, die Zeilen zurückgibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image41.png))

Der nächste Schritt besteht darin, die SQL-Abfrage zu definieren, die für den Datenzugriff verwendet wird. Da wir nur die Produkte zurückgeben möchten, die zu einer bestimmten Kategorie gehören, verwende ich dieselbe <strong>Select</strong> -Anweisung aus " <strong>GetProducts ()</strong>", füge aber die folgende <strong>Where</strong> -Klausel hinzu: <strong>Where CategoryID = @CategoryID</strong>. Der <strong>@CategoryID</strong> -Parameter gibt dem TableAdapter-Assistenten an, dass für die Methode, die wir erstellen, ein Eingabeparameter vom entsprechenden Typ erforderlich ist (also eine Ganzzahl, die NULL-Werte zulässt).

[![geben Sie eine Abfrage ein, um nur Produkte in einer bestimmten Kategorie zurückzugeben.](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Abbildung 16**: eingeben einer Abfrage zum Zurückgeben von Produkten in einer angegebenen Kategorie ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image44.png))

Im letzten Schritt können Sie auswählen, welche Datenzugriffs Muster verwendet werden sollen, und wie Sie die Namen der generierten Methoden anpassen. Für das Füllmuster ändern wir den Namen in <strong>fillbycategoryid</strong> und geben für die Rückgabe eines Datentabelle-Rückgabe Musters ( <strong>get*X</strong>*  -Methoden) <strong>getproductbycategoryid</strong>an.

[![wählen Sie die Namen für die TableAdapter-Methoden aus.](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Abbildung 17**: Auswählen der Namen für die TableAdapter-Methoden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image47.png))

Nachdem Sie den Assistenten abgeschlossen haben, enthält der DataSet-Designer die neuen TableAdapter-Methoden.

![Die Produkte können jetzt nach Kategorie abgefragt werden.](creating-a-data-access-layer-cs/_static/image48.png)

**Abbildung 18**: die Produkte können jetzt nach Kategorie abgefragt werden

Nehmen Sie sich einen Moment Zeit, um eine **getproductbyproductid (*ProductID*)** -Methode mit derselben Technik hinzuzufügen.

Diese parametrisierten Abfragen können direkt über den DataSet-Designer getestet werden. Klicken Sie mit der rechten Maustaste auf die Methode im TableAdapter, und wählen Sie Daten Vorschau aus. Geben Sie als nächstes die für die Parameter zu verwendenden Werte ein, und klicken Sie auf Vorschau.

[Es werden ![Produkte angezeigt, die zur Kategorie Getränke gehören.](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Abbildung 19**: die Produkte, die zur Kategorie Getränke gehören, werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image51.png))

Mit der **getproduczbycategoryid (*CategoryID*)** -Methode in unserer dal können wir nun eine ASP.NET-Seite erstellen, auf der nur die Produkte in einer bestimmten Kategorie angezeigt werden. Im folgenden Beispiel werden alle Produkte angezeigt, die in der Kategorie Getränke enthalten sind und die **CategoryID** 1 aufweisen.

Getränke. ASP

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]

[![diese Produkte in der Kategorie Getränke angezeigt werden.](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Abbildung 20**: diese Produkte in der Kategorie Getränke werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image54.png))

## <a name="step-4-inserting-updating-and-deleting-data"></a>Schritt 4: einfügen, aktualisieren und Löschen von Daten

Es gibt zwei Muster, die häufig für das Einfügen, aktualisieren und Löschen von Daten verwendet werden. Das erste Muster, das ich als direktes Daten Bank Muster nenne, umfasst das Erstellen von Methoden, die beim Aufrufen einen **Insert**-, **Update**-oder **Delete** -Befehl für die Datenbank ausgeben, die für einen einzelnen Datenbankdaten Satz funktioniert. Solche Methoden werden in der Regel in einer Reihe von skalaren Werten (ganze Zahlen, Zeichen folgen, boolesche Werte, DateTime usw.), die den Werten zum Einfügen, aktualisieren oder löschen entsprechen, weitergegeben. Mit diesem Muster für die **Products** -Tabelle würde die Delete-Methode z. b. einen ganzzahligen Parameter annehmen, der die **ProductID** des zu löschenden Datensatzes angibt, während die Insert-Methode eine Zeichenfolge für den **ProductName**, ein Dezimalzeichen für den **UnitPrice**, eine Ganzzahl für den **unitsonstock**usw. annimmt.

[![jede INSERT-, Update-und DELETE-Anforderung sofort an die Datenbank gesendet wird.](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Abbildung 21**: jede INSERT-, Update-und DELETE-Anforderung wird sofort an die Datenbank gesendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image57.png))

Das andere Muster, das ich als Batch Aktualisierungs Muster bezeichne, besteht darin, ein gesamtes DataSet, eine Datentabelle oder eine Auflistung von DataRows in einem Methoden aufzurufen zu aktualisieren. Mit diesem Muster löscht ein Entwickler die DataRows in einer Datentabelle, fügt Sie ein und ändert Sie und übergibt diese DataRows oder Datentabelle dann an eine Aktualisierungs Methode. Diese Methode listet dann die weiter gegebenen DataRows auf, bestimmt, ob Sie geändert, hinzugefügt oder gelöscht wurden (über den [RowState-Eigenschafts](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) Wert von DataRow), und gibt die entsprechende Daten Bank Anforderung für jeden Datensatz aus.

[![alle Änderungen mit der Datenbank synchronisiert werden, wenn die Aktualisierungs Methode aufgerufen wird.](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Abbildung 22**: alle Änderungen werden mit der Datenbank synchronisiert, wenn die Aktualisierungs Methode aufgerufen wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image60.png))

Der TableAdapter verwendet standardmäßig das Batch Aktualisierungs Muster, unterstützt aber auch das direkte DB-Muster. Da wir beim Erstellen des TableAdapters die Option "INSERT-, Update-und DELETE-Anweisungen generieren" in den erweiterten Eigenschaften ausgewählt haben, enthält der **ProductsTableAdapter** eine **Update ()** -Methode, die das Batch Aktualisierungs Muster implementiert. Der TableAdapter enthält insbesondere eine **Update ()** -Methode, der das typisierte DataSet, eine stark typisierte Datentabelle oder eine oder mehrere DataRows (DataRows) weitergegeben werden können. Wenn Sie das Kontrollkästchen "GenerateDBDirectMethods" beim ersten Erstellen des TableAdapter aktiviert lassen, wird das direkte DB-Muster auch über die Methoden **Insert ()** , **Update ()** und **Delete ()** implementiert.

Beide Daten Änderungs Muster verwenden die Eigenschaften **InsertCommand**, **UpdateCommand**und **DeleteCommand** des TableAdapter, um Ihre **Insert**-, **Update**-und **Delete** -Befehle an die Datenbank auszugeben. Sie können die Eigenschaften **InsertCommand**, **UpdateCommand**und **DeleteCommand** überprüfen und ändern, indem Sie auf den TableAdapter im DataSet-Designer klicken und dann zum Eigenschaftenfenster wechseln. (Stellen Sie sicher, dass Sie den TableAdapter ausgewählt haben und dass das **ProductsTableAdapter** -Objekt das in der Dropdown Liste in der Eigenschaftenfenster ausgewählte ist.)

[![der TableAdapter über die Eigenschaften InsertCommand, UpdateCommand und DeleteCommand verfügt.](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Abbildung 23**: der TableAdapter verfügt über die Eigenschaften **InsertCommand**, **UpdateCommand**und **DeleteCommand** ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image63.png))

Um die Eigenschaften der Daten Bank Befehle zu überprüfen oder zu ändern, klicken Sie auf die untergeordnete Eigenschaft **CommandText** , um die Abfrage-Generator zu übernehmen.

[![Konfigurieren der INSERT-, Update-und DELETE-Anweisungen im Abfrage-Generator](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Abbildung 24**: Konfigurieren der **Insert**-, **Update**-und **Delete** -Anweisungen im Abfrage-Generator ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image66.png))

Im folgenden Codebeispiel wird veranschaulicht, wie das Batch Aktualisierungs Muster verwendet wird, um den Preis aller Produkte zu verdoppeln, die nicht eingestellt werden und 25 Einheiten in Aktien oder weniger aufweisen:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Der folgende Code veranschaulicht, wie das direkte Daten Bank Muster verwendet wird, um ein bestimmtes Produktprogramm gesteuert zu löschen, anschließend ein Update zu aktualisieren und dann ein neues hinzuzufügen:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Erstellen von benutzerdefinierten INSERT-, Update-und Delete-Methoden

Die von der direkten Daten Bank Methode erstellten **Insert ()** -, **Update ()** -und **Delete ()** -Methoden können etwas umständlich sein, insbesondere bei Tabellen mit vielen Spalten. Wenn Sie sich das vorherige Codebeispiel ansehen, ist es ohne IntelliSense nicht besonders zu verdeutlichen, welche **Produkte** Table Column den einzelnen Eingabe Parametern der **Update ()** -Methode und der **Insert ()** -Methode zugeordnet wird. Es kann vorkommen, dass Sie nur eine einzelne Spalte oder zwei aktualisieren möchten oder eine angepasste **Insert ()** **-Methode** wünschen, die den Wert des neu eingefügten Datensatzes (automatisch Inkrement) zurückgeben soll.

Kehren Sie zum Erstellen einer solchen benutzerdefinierten Methode zum DataSet-Designer zurück. Klicken Sie mit der rechten Maustaste auf den TableAdapter, und wählen Sie Abfrage hinzufügen, um zum TableAdapter-Assistenten zurückzukehren. Auf dem zweiten Bildschirm können Sie den Typ der zu erstellenden Abfrage angeben. Wir erstellen eine Methode, die ein neues Produkt hinzufügt und dann den Wert der **ProductID**des neu hinzugefügten Datensatzes zurückgibt. Wählen Sie daher aus, dass Sie eine **Insert** -Abfrage erstellen möchten.

[![eine Methode erstellen, um der Tabelle "Products" eine neue Zeile hinzuzufügen.](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Abbildung 25**: Erstellen einer Methode zum Hinzufügen einer neuen Zeile zur Tabelle " **Products** " ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image69.png))

Auf dem nächsten Bildschirm wird der **CommandText** von **InsertCommand**angezeigt. Erweitern Sie diese Abfrage, indem Sie am Ende der Abfrage **Select Scope\_Identity ()** hinzufügen, wodurch der letzte Identitäts Wert zurückgegeben wird, der in eine **Identitäts** Spalte im selben Bereich eingefügt wurde. (In der [technischen Dokumentation](https://msdn.microsoft.com/library/ms190315.aspx) finden Sie weitere Informationen zum **Bereich\_Identity ()** und dazu, warum Sie den [Bereich\_Identity () anstelle von @@IDENTITYverwenden ](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx)möchten.) Stellen Sie sicher, dass Sie die **Insert** -Anweisung mit einem Semikolon beenden, bevor Sie die **Select** -Anweisung hinzufügen.

[![die Abfrage erweitern, um den SCOPE_IDENTITY ()-Wert zurückzugeben.](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Abbildung 26**: Erweitern der Abfrage, um den **Bereich\_Identity ()** -Wert zurückzugeben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image72.png))

Benennen Sie schließlich die neue Methode **insertProduct**.

[![legen Sie den Namen der neuen Methode auf insertProduct fest.](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Abbildung 27**: Festlegen des Namens der neuen Methode auf **insertProduct** ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image75.png))

Wenn Sie zum DataSet-Designer zurückkehren, sehen Sie, dass der **ProductsTableAdapter** die neue Methode **insertProduct**enthält. Wenn diese neue Methode keinen Parameter für jede Spalte in der **Products** -Tabelle enthält, haben Sie die Wahrscheinlichkeit, dass Sie die **Insert** -Anweisung mit einem Semikolon beenden. Konfigurieren Sie die **insertProduct** -Methode, und stellen Sie sicher, dass Sie über ein Semikolon mit den Anweisungen **Insert** und **Select** verfügen.

Standardmäßig geben Insert-Methoden nicht-Abfrage Methoden aus, was bedeutet, dass Sie die Anzahl der betroffenen Zeilen zurückgeben. Wir möchten jedoch, dass die **insertProduct** -Methode den von der Abfrage zurückgegebenen Wert zurückgibt, nicht die Anzahl der betroffenen Zeilen. Um dies zu erreichen, passen Sie die **executemode** -Eigenschaft der **insertProduct** -Methode auf **Scalar**an.

[![die executemode-Eigenschaft in Skalar ändern](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Abbildung 28**: Ändern der **executemode** -Eigenschaft in **Skalar** ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image78.png))

Der folgende Code zeigt diese neue **insertProduct** -Methode in Aktion:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Schritt 5: Abschließen der Datenzugriffs Ebene

Beachten Sie, dass die **productstableadapters** -Klasse den **CategoryID** -Wert und den **SupplierID-** Wert aus der **Products** -Tabelle zurückgibt, jedoch nicht die **CategoryName** -Spalte aus der **Categories** -Tabelle oder die **CompanyName** -Spalte aus der **Suppliers** -Tabelle, obwohl dies wahrscheinlich die Spalten ist, die beim Anzeigen von Produktinformationen angezeigt werden sollen. Wir können die anfängliche Methode " **GetProducts ()** " von TableAdapter erweitern, um die Spaltenwerte " **CategoryName** " und " **CompanyName** " einzuschließen. Dadurch wird die stark typisierte Datentabelle so aktualisiert, dass auch diese neuen Spalten enthalten sind.

Dies kann jedoch ein Problem darstellen, da die Methoden des TableAdapter zum Einfügen, aktualisieren und Löschen von Daten auf dieser anfänglichen Methode basieren. Glücklicherweise sind die automatisch generierten Methoden zum Einfügen, aktualisieren und Löschen von Unterabfragen in der **Select** -Klausel nicht betroffen. Wenn Sie die Abfragen in **Kategorien** und **Lieferanten** als Unterabfragen und nicht als **Join** hinzufügen, müssen wir diese Methoden zum Ändern von Daten nicht mehr bearbeiten. Klicken Sie mit der rechten Maustaste auf die **GetProducts ()** -Methode in **ProductsTableAdapter** , und wählen Sie konfigurieren aus. Passen Sie dann die **Select** -Klausel so an, dass Sie wie folgt aussieht:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]

[![aktualisieren Sie die SELECT-Anweisung für die GetProducts ()-Methode.](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Abbildung 29**: Aktualisieren der **Select** -Anweisung für die **GetProducts ()** -Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image81.png))

Nachdem Sie die **GetProducts ()** -Methode aktualisiert haben, um diese neue Abfrage zu verwenden, enthält die Datentabelle zwei neue Spalten: **CategoryName** und **suppliername**.

![Die Datentabelle "Products" hat zwei neue Spalten.](creating-a-data-access-layer-cs/_static/image82.png)

**Abbildung 30**: die Datentabelle " **Products** " hat zwei neue Spalten

Nehmen Sie sich einen Moment Zeit, um die **Select** -Klausel in der **getproduczbycategoryid-Methode (*CategoryID*)** zu aktualisieren.

Wenn Sie die **GetProducts ()** **Select** -Anweisung mithilfe der **joinsyntax aktualisieren** , kann der DataSet-Designer die Methoden zum Einfügen, aktualisieren und Löschen von Datenbankdaten mit dem direkten DB-Muster nicht automatisch generieren. Stattdessen müssen Sie diese manuell erstellen, ähnlich wie bei der **insertProduct** -Methode weiter oben in diesem Tutorial. Außerdem müssen Sie die Eigenschaftswerte **InsertCommand**, **UpdateCommand**und **DeleteCommand** manuell angeben, wenn Sie das Batch Aktualisierungs Muster verwenden möchten.

## <a name="adding-the-remaining-tableadapters"></a>Hinzufügen der verbleibenden TableAdapters

Bis jetzt haben wir uns nur mit einem einzigen TableAdapter für eine einzelne Datenbanktabelle beschäftigt. Die Datenbank Northwind enthält jedoch mehrere verknüpfte Tabellen, mit denen wir in unserer Webanwendung zusammenarbeiten müssen. Ein typisiertes DataSet kann mehrere verknüpfte DataTables enthalten. Um unsere dal abzuschließen, müssen wir daher DataTables für die anderen Tabellen hinzufügen, die wir in diesen Tutorials verwenden werden. Öffnen Sie zum Hinzufügen eines neuen TableAdapter zu einem typisierten Dataset den DataSet-Designer, klicken Sie mit der rechten Maustaste in den Designer, und wählen Sie hinzufügen/TableAdapter aus. Dadurch wird eine neue Datentabelle und ein neuer TableAdapter erstellt, und Sie werden durch den Assistenten geführt, den wir zuvor in diesem Tutorial untersucht haben.

Nehmen Sie sich einige Minuten Zeit, um die folgenden TableAdapters und Methoden mithilfe der folgenden Abfragen zu erstellen: Beachten Sie, dass die Abfragen im **ProductsTableAdapter** die Unterabfragen enthalten, um die Kategorie-und Lieferanten Namen der einzelnen Produkte zu erfassen. Außerdem haben Sie, wenn Sie es bereits getan haben, bereits die Methoden **GetProducts ()** und **getproductbycategoryid (*CategoryID*)** der **ProductsTableAdapter** -Klasse hinzugefügt.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **Getproductenbycategoryid**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **Getproducandbysupplierid**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **Getproductbyproductid**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **Categoriestableadapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **Getcategorybycategoryid**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **Supplierstableadapter**

  - **Getsuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **Getsuppliersbycountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **Getsupplierbysupplierid**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **Mitarbeiter Adapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **Getemployeesbymanager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **Getemployeebymitarbeiter Eid**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]

[![Sie den DataSet-Designer, nachdem die vier TableAdapter hinzugefügt wurden.](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Abbildung 31**: der DataSet-Designer nach dem Hinzufügen der vier TableAdapters ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image85.png))

## <a name="adding-custom-code-to-the-dal"></a>Hinzufügen von benutzerdefiniertem Code zur Dal

Die dem typisierten DataSet hinzugefügten TableAdapters und DataTables werden als XML-Schema Definitionsdatei (**Northwind. xsd**) ausgedrückt. Sie können diese Schema Informationen anzeigen, indem Sie in der Projektmappen-Explorer mit der rechten Maustaste auf die Datei **Northwind. xsd** klicken und Code anzeigen auswählen.

[![Sie die XSD-Datei (XML Schema Definition) für das typisierte Northwinds-DataSet.](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Abbildung 32**: die XSD-Datei (XML Schema Definition) für das typisierte Northwinds-DataSet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image88.png))

Diese Schema Informationen werden zur Entwurfs C# Zeit in oder Visual Basic Code übersetzt (falls erforderlich). an diesem Punkt können Sie Sie mit dem Debugger schrittweise durchlaufen. Um diesen automatisch generierten Code anzuzeigen, wechseln Sie zum Klassenansicht, und führen Sie einen Drilldown zu den TableAdapter-oder typisierten DataSet-Klassen aus. Wenn die Klassenansicht auf dem Bildschirm nicht angezeigt wird, klicken Sie auf das Menü Ansicht, und wählen Sie es aus, oder drücken Sie STRG + UMSCHALT + C. Im Klassenansicht werden die Eigenschaften, Methoden und Ereignisse der typisierten DataSet-und TableAdapter-Klassen angezeigt. Wenn Sie den Code für eine bestimmte Methode anzeigen möchten, doppelklicken Sie in der Klassenansicht auf den Methodennamen, oder klicken Sie mit der rechten Maustaste darauf, und wählen Sie Gehe zu Definition aus.

![Überprüfen Sie den automatisch generierten Code, indem Sie im Klassenansicht die Option Gehe zu Definition auswählen.](creating-a-data-access-layer-cs/_static/image89.png)

**Abbildung 33**: Überprüfen Sie den automatisch generierten Code, indem Sie in der Klassenansicht die Option Gehe zu Definition auswählen.

Obwohl automatisch generierter Code sehr viel Zeit sparen kann, ist der Code oft sehr generisch und muss angepasst werden, um die besonderen Anforderungen einer Anwendung zu erfüllen. Das Risiko der Erweiterung von automatisch generiertem Code besteht jedoch darin, dass das Tool, das den Code generiert hat, möglicherweise die Zeit zum erneuten generieren und überschreiben Ihrer Anpassungen feststellt. Mit dem neuen partiellen Klassenkonzept von .NET 2.0 ist es einfach, eine Klasse auf mehrere Dateien aufzuteilen. Dadurch können wir unsere eigenen Methoden, Eigenschaften und Ereignisse zu den automatisch generierten Klassen hinzufügen, ohne sich über Visual Studio Gedanken machen zu müssen, um die Anpassungen zu überschreiben.

Um die Anpassung der dal zu veranschaulichen, fügen wir der **suppliersrow** -Klasse eine **GetProducts ()** -Methode hinzu. Die **suppliersrow** -Klasse stellt einen einzelnen Datensatz in der **Suppliers** -Tabelle dar. Jeder Lieferant kann eine beliebige Anzahl von Produkten als Anbieter angeben, sodass **GetProducts ()** diese Produkte des angegebenen Lieferanten zurückgibt. Um dies zu erreichen, erstellen Sie eine neue Klassendatei in der **App\_dem Code** Ordner mit dem Namen **SuppliersRow.cs** , und fügen Sie folgenden Code hinzu:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Diese partielle Klasse weist den Compiler an, dass bei der Erstellung der **Northwind. suppliersrow** -Klasse, um die soeben definierte **GetProducts ()** -Methode einzubeziehen. Wenn Sie das Projekt erstellen und dann zum Klassenansicht zurückkehren, sehen Sie, dass " **GetProducts ()** " nun als Methode von " **Northwind. suppliersrow**" aufgeführt ist.

![Die GetProducts ()-Methode ist jetzt Teil der Northwind. suppliersrow-Klasse.](creating-a-data-access-layer-cs/_static/image90.png)

**Abbildung 34**: die **GetProducts ()** -Methode ist nun Bestandteil der **Northwind. suppliersrow** -Klasse.

Die **GetProducts ()** -Methode kann jetzt verwendet werden, um die Produktgruppe für einen bestimmten Lieferanten aufzulisten, wie im folgenden Code gezeigt:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Diese Daten können auch in einem beliebigen ASP angezeigt werden. Daten-websteuer Elemente des Netzes. Auf der folgenden Seite wird ein GridView-Steuerelement mit zwei Feldern verwendet:

- Ein BoundField, das den Namen der einzelnen Lieferanten anzeigt, und
- Ein TemplateField-Steuerelement, das ein BulletedList-Steuerelement enthält, das an die von der **GetProducts ()** -Methode für jeden Lieferanten zurückgegebenen Ergebnisse gebunden ist.

Wir untersuchen, wie solche Master/Detail-Berichte in zukünftigen Tutorials angezeigt werden. In diesem Beispiel soll die Verwendung der benutzerdefinierten Methode veranschaulicht werden, die der **Northwind. suppliersrow** -Klasse hinzugefügt wurde.

Suppliersandproducts. aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]

[![der Unternehmens Name des Lieferanten in der linken Spalte aufgeführt ist, werden die Produkte auf der rechten Seite angezeigt.](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Abbildung 35**: der Unternehmens Name des Lieferanten ist in der linken Spalte aufgeführt, ihre Produkte auf der rechten Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-data-access-layer-cs/_static/image93.png))

## <a name="summary"></a>Zusammenfassung

Wenn Sie eine Webanwendung erstellen, sollte die DAL einer der ersten Schritte sein, bevor Sie mit dem Erstellen der Präsentationsschicht beginnen. Mit Visual Studio ist das Erstellen einer dal basierend auf typisierten Datasets eine Aufgabe, die innerhalb von 10-15 Minuten durchgeführt werden kann, ohne eine Codezeile zu schreiben. Die Lernprogramme, die sich weiterentwickeln, basieren auf dieser dal. Im [nächsten Tutorial](creating-a-business-logic-layer-cs.md) definieren wir eine Reihe von Geschäftsregeln und sehen, wie Sie in einer separaten Geschäftslogik Schicht implementiert werden.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Aufbauen einer dal mit stark typisierten TableAdapters und DataTables in VS 2005 und ASP.NET 2,0](https://weblogs.asp.net/scottgu/435498)
- [Entwerfen von Datenebenenkomponenten und übergeben von Daten über Ebenen](https://msdn.microsoft.com/library/ms978496.aspx)
- [Erstellen einer Datenzugriffs Ebene mit dem DataSet-Designer von Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Verschlüsseln von Konfigurationsinformationen in ASP.NET 2,0-Anwendungen](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Übersicht über TableAdapters](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Arbeiten mit einem typisierten DataSet](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Verwenden von stark typisiertem Datenzugriff in Visual Studio 2005 und ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Erweitern von TableAdapter-Methoden](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Abrufen von skalaren Daten aus einer gespeicherten Prozedur](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video Schulung zu Themen in diesem Tutorial

- [Datenzugriffsschichten in ASP.NET-Anwendungen](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Manuelles Binden eines Datasets an ein DataGrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Arbeiten mit Datasets und Filtern aus einer ASP-Anwendung](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Ron grün, Hilton Giesenow, Dennis Patterson, Liz shulok, Abel Gomez und Carlos Santos. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](creating-a-business-logic-layer-cs.md)
