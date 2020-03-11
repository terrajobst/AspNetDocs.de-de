---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: Erstellen von gespeicherten Prozeduren und benutzerdefinierten Funktionen mit verwaltetem Code (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Microsoft SQL Server 2005 wird in die .NET Common Language Runtime integriert, um Entwicklern das Erstellen von Datenbankobjekten über verwalteten Code zu ermöglichen. Dieses Tutorial...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ac5f71d519689a9dc84fb82a04196d520cca6e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78443841"
---
# <a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Verwenden von gespeicherten Prozeduren und benutzerdefinierten Funktionen mit verwaltetem Code (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) oder [PDF herunterladen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 wird in die .NET Common Language Runtime integriert, um Entwicklern das Erstellen von Datenbankobjekten über verwalteten Code zu ermöglichen. In diesem Tutorial wird gezeigt, wie Sie verwaltete gespeicherte Prozeduren und verwaltete benutzerdefinierte Funktionen mit C# Ihrem Visual Basic oder Code erstellen. Wir sehen auch, wie diese Editionen von Visual Studio Ihnen ermöglichen, solche verwalteten Datenbankobjekte zu debuggen.

## <a name="introduction"></a>Einführung

Datenbanken wie Microsoft s SQL Server 2005 verwenden [Transact-strukturierte Abfragesprache (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) zum Einfügen, ändern und Abrufen von Daten. Die meisten Datenbanksysteme enthalten Konstrukte zum Gruppieren einer Reihe von SQL-Anweisungen, die dann als einzelne, wiederverwendbare Einheit ausgeführt werden können. Gespeicherte Prozeduren sind ein Beispiel. Ein weiterer ist *benutzerdefinierte Funktionen (User-Defined Functions*, UDFs), ein Konstrukt, das in Schritt 9 ausführlicher untersucht wird.

Im Kern ist SQL für die Arbeit mit Datensätzen konzipiert. Die Anweisungen `SELECT`, `UPDATE`und `DELETE` gelten grundsätzlich für alle Datensätze in der entsprechenden Tabelle und sind nur durch ihre `WHERE` Klauseln beschränkt. Es gibt jedoch viele sprach Features, die für die Arbeit mit jeweils einem Datensatz und der Bearbeitung von skalaren Daten entworfen wurden. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) erlauben, dass eine Gruppe von Datensätzen einzeln durchlaufen wird. Zeichen folgen Bearbeitungsfunktionen wie `LEFT`, `CHARINDEX`und `PATINDEX` arbeiten mit skalaren Daten. SQL enthält auch Ablauf Steuerungs Anweisungen wie `IF` und `WHILE`.

Vor Microsoft SQL Server 2005 konnten gespeicherte Prozeduren und UDFs nur als eine Auflistung von T-SQL-Anweisungen definiert werden. SQL Server 2005 wurde jedoch entwickelt, um die Integration mit der [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)zu ermöglichen, bei der es sich um die von allen .NET-Assemblys verwendete Laufzeit handelt. Folglich können die gespeicherten Prozeduren und UDFs in einer SQL Server 2005-Datenbank mithilfe von verwaltetem Code erstellt werden. Das heißt, Sie können eine gespeicherte Prozedur oder UDF als Methode in einer Visual Basic Klasse erstellen. Dadurch können diese gespeicherten Prozeduren und UDFs Funktionen in den .NET Framework und ihren eigenen benutzerdefinierten Klassen nutzen.

In diesem Tutorial wird erläutert, wie verwaltete gespeicherte Prozeduren und benutzerdefinierte Funktionen erstellt werden und wie Sie in die Northwind-Datenbank integriert werden. Legen Sie Los!

> [!NOTE]
> Verwaltete Datenbankobjekte bieten einige Vorteile gegenüber Ihren SQL-Entsprechungen. Der größte Vorteil von sprach Reichtum und Vertrautheit sowie die Möglichkeit, vorhandenen Code und Logik wiederzuverwenden. Verwaltete Datenbankobjekte sind jedoch wahrscheinlich weniger effizient, wenn Sie mit Datasets arbeiten, die keine viel prozedurale Logik betreffen. Eine ausführlichere Erläuterung zu den Vorteilen der Verwendung von verwaltetem Code im Vergleich zu T-SQL finden Sie unter [Vorteile der Verwendung von verwaltetem Code zum Erstellen von Datenbankobjekten](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).

## <a name="step-1-moving-the-northwind-database-out-ofapp_data"></a>Schritt 1: Verschieben der Northwind-Datenbank aus`App_Data`

Alle unsere Tutorials haben bisher eine Microsoft SQL Server 2005 Express Edition-Datenbankdatei im Ordner `App_Data` der Webanwendung verwendet. Das Platzieren der Datenbank in `App_Data` vereinfachte Verteilung und Ausführung dieser Tutorials, da sich alle Dateien in einem Verzeichnis befinden und keine weiteren Konfigurationsschritte zum Testen des Tutorials erforderlich sind.

In diesem Tutorial können Sie jedoch die Northwind-Datenbank aus `App_Data` verschieben und Sie explizit bei der SQL Server 2005 Express Edition Daten Bank Instanz registrieren. Wir können zwar die Schritte für dieses Tutorial mit der Datenbank im Ordner "`App_Data`" ausführen, aber eine Reihe von Schritten wird erheblich vereinfacht, indem die Datenbank explizit bei der SQL Server 2005 Express Edition Daten Bank Instanz registriert wird.

Der Download für dieses Tutorial enthält die beiden Datenbankdateien `NORTHWND.MDF` und `NORTHWND_log.LDF` in einem Ordner mit dem Namen `DataFiles`platziert. Wenn Sie mit ihrer eigenen Implementierung der Tutorials fortfahren, schließen Sie Visual Studio, und verschieben Sie die `NORTHWND.MDF`-und `NORTHWND_log.LDF` Dateien aus dem Ordner "Website" `App_Data` Ordner in einen Ordner außerhalb der Website. Nachdem die Datenbankdateien in einen anderen Ordner verschoben wurden, müssen Sie die Northwind-Datenbank bei der SQL Server 2005 Express Edition Daten Bank Instanz registrieren. Dies kann über SQL Server Management Studio erfolgen. Wenn Sie eine nicht-Express-Edition von SQL Server 2005 auf Ihrem Computer installiert haben, sind Sie wahrscheinlich bereits Management Studio installiert. Wenn Sie nur über SQL Server 2005 Express Edition auf dem Computer verfügen, nehmen Sie sich einen Moment Zeit, um [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)herunterzuladen und zu installieren.

Starten Sie SQL Server Management Studio. Wie in Abbildung 1 gezeigt, wird Management Studio zunächst gefragt, mit welchem Server eine Verbindung hergestellt werden soll. Geben Sie localhost\SQLExpress als Servernamen ein, wählen Sie in der Dropdown Liste Authentifizierung die Option Windows-Authentifizierung aus, und klicken Sie auf verbinden.

![Herstellen einer Verbindung mit der entsprechenden Daten Bank Instanz](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Abbildung 1**: Herstellen einer Verbindung mit der entsprechenden Daten Bank Instanz

Nachdem Sie die Verbindung hergestellt haben, werden im Objekt-Explorer Fenster Informationen zur SQL Server 2005 Express Edition Daten Bank Instanz, einschließlich der zugehörigen Datenbanken, Sicherheitsinformationen, Verwaltungs Optionen usw., angezeigt.

Wir müssen die Northwind-Datenbank in den `DataFiles` Ordner (oder wo immer, wo Sie Sie möglicherweise verschoben haben) an die SQL Server 2005 Express Edition Daten Bank Instanz anfügen. Klicken Sie mit der rechten Maustaste auf den Ordner Datenbanken, und wählen Sie im Kontextmenü die Option anfügen aus. Dadurch wird das Dialogfeld Datenbanken anfügen angezeigt. Klicken Sie auf die Schaltfläche hinzufügen, führen Sie einen Drilldown zum entsprechenden `NORTHWND.MDF` Datei aus, und klicken Sie An diesem Punkt sollte der Bildschirm in etwa wie in Abbildung 2 aussehen.

[![Herstellen einer Verbindung mit der entsprechenden Daten Bank Instanz](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Abbildung 2**: Herstellen einer Verbindung mit der entsprechenden Daten Bank Instanz ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))

> [!NOTE]
> Beim Herstellen einer Verbindung mit der SQL Server 2005 Express Edition Instanz über Management Studio das Dialogfeld Datenbanken anfügen keinen Drilldown in Benutzerprofil Verzeichnisse (z. b. "eigene Dateien"). Stellen Sie daher sicher, dass Sie die `NORTHWND.MDF`-und `NORTHWND_log.LDF` Dateien in einem Nichtbenutzer Profilverzeichnis platzieren.

Klicken Sie auf die Schaltfläche OK, um die Datenbank anzufügen. Das Dialogfeld Datenbanken anfügen wird geschlossen, und der Objekt-Explorer sollte nun die gerade angefügte Datenbank auflisten. Wahrscheinlichkeit ist, dass die Northwind-Datenbank einen Namen wie `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`hat. Benennen Sie die Datenbank in Northwind um, indem Sie mit der rechten Maustaste auf die Datenbank klicken und Umbenennen auswählen.

![Umbenennen der Datenbank in Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Abbildung 3**: Umbenennen der Datenbank in Northwind

## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Schritt 2: Erstellen einer neuen Projekt Mappe und SQL Server Projekt in Visual Studio

Zum Erstellen von verwalteten gespeicherten Prozeduren oder UDFs in SQL Server 2005 werden die gespeicherte Prozedur und die UDF-Logik als Visual Basic Code in einer Klasse geschrieben. Nachdem der Code geschrieben wurde, müssen Sie diese Klasse in eine Assembly (eine `.dll` Datei) kompilieren, die Assembly bei der SQL Server Datenbank registrieren und dann eine gespeicherte Prozedur oder ein UDF-Objekt in der Datenbank erstellen, die auf die entsprechende Methode in der Assembly verweist. Diese Schritte können alle manuell ausgeführt werden. Wir können den Code in einem beliebigen Text-Editor erstellen, ihn über die Befehlszeile kompilieren, indem Sie den Visual Basic Compiler (`vbc.exe`) verwenden, ihn mit dem [`CREATE ASSEMBLY`](https://msdn.microsoft.com/library/ms189524.aspx) -Befehl oder aus Management Studio bei der Datenbank registrieren und die gespeicherte Prozedur oder das UDF-Objekt über eine ähnliche Weise hinzufügen. Glücklicherweise beinhalten die Professional-und Team Systems-Versionen von Visual Studio einen SQL Server Projekttyp, der diese Aufgaben automatisiert. In diesem Tutorial wird die Verwendung des SQL Server Projekttyps zum Erstellen einer verwalteten gespeicherten Prozedur und einer UDF erläutert.

> [!NOTE]
> Wenn Sie Visual Web Developer oder die Standard Edition von Visual Studio verwenden, müssen Sie stattdessen den manuellen Ansatz verwenden. Schritt 13 enthält ausführliche Anweisungen zum manuellen Ausführen dieser Schritte. Ich empfehle Ihnen, vor dem Lesen von Schritt 13 Schritte 2 bis 12 zu lesen, da diese Schritte wichtige SQL Server Konfigurations Anweisungen enthalten, die unabhängig von der verwendeten Version von Visual Studio angewendet werden müssen.

Beginnen Sie mit dem Öffnen von Visual Studio. Wählen Sie im Menü Datei die Option Neues Projekt aus, um das Dialogfeld Neues Projekt anzuzeigen (siehe Abbildung 4). Führen Sie einen Drilldown zum Daten Bank Projekttyp aus, und wählen Sie dann aus den auf der rechten Seite aufgeführten Vorlagen ein neues SQL Server Projekt aus. Ich habe mich entschieden, dieses Projekt `ManagedDatabaseConstructs` zu benennen und es in eine Projekt Mappe mit dem Namen `Tutorial75`zu platzieren.

[![erstellen Sie ein neues SQL Server Projekt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Abbildung 4**: Erstellen eines neuen SQL Server Projekts ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))

Klicken Sie im Dialogfeld Neues Projekt auf die Schaltfläche OK, um die Projekt Mappe zu erstellen und SQL Server Projekt zu erstellen.

Ein SQL Server-Projekt ist an eine bestimmte Datenbank gebunden. Nach dem Erstellen des neuen SQL Server Projekts werden wir daher sofort aufgefordert, diese Informationen anzugeben. Abbildung 5 zeigt das neue Dialogfeld "Daten Bank Verweis", das ausgefüllt wurde, um auf die Northwind-Datenbank zu verweisen, die in der SQL Server 2005 Express Edition-Daten Bank Instanz wieder in Schritt 1 registriert wurde.

![Zuordnen des SQL Server Projekts zur Northwind-Datenbank](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Abbildung 5**: Zuordnen des SQL Server Projekts zur Datenbank Northwind

Um die verwalteten gespeicherten Prozeduren und UDFs zu debuggen, die wir in diesem Projekt erstellen, muss die SQL/CLR-Debugging-Unterstützung für die Verbindung aktiviert werden. Wenn ein SQL Server-Projekt einer neuen Datenbank zugeordnet wird (wie in Abbildung 5 dargestellt), werden Sie von Visual Studio gefragt, ob Sie das SQL/CLR-Debugging für die Verbindung aktivieren möchten (siehe Abbildung 6). Klicken Sie auf Ja.

![SQL/CLR-Debuggen aktivieren](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Abbildung 6**: Aktivieren des SQL/CLR-Debuggens

An diesem Punkt wurde das neue SQL Server Projekt der Projekt Mappe hinzugefügt. Sie enthält einen Ordner mit dem Namen `Test Scripts` mit einer Datei mit dem Namen `Test.sql`, die zum Debuggen der im Projekt erstellten verwalteten Datenbankobjekte verwendet wird. Wir werden uns mit dem Debuggen in Schritt 12 befassen.

Wir können jetzt neue verwaltete gespeicherte Prozeduren und UDFs zu diesem Projekt hinzufügen, aber bevor wir die vorhandene Webanwendung in die Projekt Mappe einbinden lassen. Wählen Sie im Menü Datei die Option hinzufügen aus, und wählen Sie vorhandene Website aus. Navigieren Sie zum entsprechenden Website Ordner, und klicken Sie auf OK. Wie in Abbildung 7 gezeigt, wird die Projekt Mappe so aktualisiert, dass Sie zwei Projekte enthält: die Website und die `ManagedDatabaseConstructs` SQL Server Projekt.

![Die Projektmappen-Explorer enthält jetzt zwei Projekte.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Abbildung 7**: die Projektmappen-Explorer enthält jetzt zwei Projekte.

Der `NORTHWNDConnectionString` Wert in `Web.config` verweist aktuell auf die `NORTHWND.MDF` Datei im `App_Data` Ordner. Da wir diese Datenbank aus `App_Data` entfernt und Sie explizit in der SQL Server 2005 Express Edition Daten Bank Instanz registriert haben, müssen wir den `NORTHWNDConnectionString` Wert entsprechend aktualisieren. Öffnen Sie die `Web.config` Datei auf der Website, und ändern Sie den Wert für `NORTHWNDConnectionString`, sodass die Verbindungs Zeichenfolge: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`lautet. Nach dieser Änderung sollte der `<connectionStrings>` Abschnitt in `Web.config` in etwa wie folgt aussehen:

[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Wie bereits im [vorherigen Tutorial](debugging-stored-procedures-vb.md)erläutert, müssen wir beim Debuggen eines SQL Server Objekts aus einer Client Anwendung, wie z. b. einer ASP.NET-Website, das Verbindungspooling deaktivieren. Die oben gezeigte Verbindungs Zeichenfolge deaktiviert das Verbindungspooling (`Pooling=false`). Wenn Sie nicht Vorhaben, die verwalteten gespeicherten Prozeduren und UDFs von der ASP.NET-Website zu debuggen, aktivieren Sie das Verbindungspooling.

## <a name="step-3-creating-a-managed-stored-procedure"></a>Schritt 3: Erstellen einer verwalteten gespeicherten Prozedur

Um der Northwind-Datenbank eine verwaltete gespeicherte Prozedur hinzuzufügen, müssen Sie zunächst die gespeicherte Prozedur als Methode im SQL Server Projekt erstellen. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Namen des `ManagedDatabaseConstructs` Projekts, und wählen Sie ein neues Element hinzufügen aus. Dadurch wird das Dialogfeld Neues Element hinzufügen angezeigt, in dem die Typen der verwalteten Datenbankobjekte aufgelistet sind, die dem Projekt hinzugefügt werden können. Wie in Abbildung 8 gezeigt, umfasst dies auch gespeicherte Prozeduren und benutzerdefinierte Funktionen.

Beginnen Sie mit dem Hinzufügen einer gespeicherten Prozedur, die einfach alle Produkte zurückgibt, die nicht mehr unterstützt werden. Nennen Sie die neue Datei mit gespeicherten Prozeduren `GetDiscontinuedProducts.vb`.

[![eine neue gespeicherte Prozedur mit dem Namen getdiscontinuedproducts. vb hinzufügen.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Abbildung 8**: Hinzufügen einer neuen gespeicherten Prozedur mit dem Namen `GetDiscontinuedProducts.vb` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))

Dadurch wird eine neue Visual Basic-Klassendatei mit folgendem Inhalt erstellt:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Beachten Sie, dass die gespeicherte Prozedur als `Shared` Methode in einer `Partial` Klassendatei mit dem Namen `StoredProcedures`implementiert wird. Außerdem wird die `GetDiscontinuedProducts`-Methode mit dem [`SqlProcedure`-Attribut](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx)ergänzt, das die Methode als gespeicherte Prozedur markiert.

Mit dem folgenden Code wird ein `SqlCommand` Objekt erstellt und dessen `CommandText` auf eine `SELECT` Abfrage festgelegt, die alle Spalten aus der `Products` Tabelle für Produkte zurückgibt, deren `Discontinued` Feld dem Wert 1 entspricht. Anschließend wird der Befehl ausgeführt, und die Ergebnisse werden an die Client Anwendung zurückgesendet. Fügen Sie der `GetDiscontinuedProducts`-Methode diesen Code hinzu.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Alle verwalteten Datenbankobjekte haben Zugriff auf ein [`SqlContext` Objekt](https://msdn.microsoft.com/library/ms131108.aspx) , das den Kontext des Aufrufers darstellt. Der `SqlContext` ermöglicht den Zugriff auf ein [`SqlPipe` Objekt](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) über seine [`Pipe`-Eigenschaft](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Dieses `SqlPipe` Objekt wird verwendet, um Informationen zwischen der SQL Server Datenbank und der aufrufenden Anwendung zu über. Wie der Name schon sagt, führt die [`ExecuteAndSend`-Methode](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) ein versektes `SqlCommand` Objekt aus und sendet die Ergebnisse an die Client Anwendung zurück.

> [!NOTE]
> Verwaltete Datenbankobjekte eignen sich am besten für gespeicherte Prozeduren und UDFs, die anstelle der festgelegten Logik prozedurale Logik verwenden. Die Verfahrens Logik umfasst das Arbeiten mit Datensätzen auf Zeilen Basis oder das Arbeiten mit skalaren Daten. Die `GetDiscontinuedProducts` Methode, die wir gerade erstellt haben, beinhaltet jedoch keine prozedurale Logik. Daher wird Sie idealerweise als gespeicherte T-SQL-Prozedur implementiert. Es ist als verwaltete gespeicherte Prozedur implementiert, um die erforderlichen Schritte zum Erstellen und Bereitstellen von verwalteten gespeicherten Prozeduren zu veranschaulichen.

## <a name="step-4-deploying-the-managed-stored-procedure"></a>Schritt 4: Bereitstellen der verwalteten gespeicherten Prozedur

Mit dieser Code Complete können wir Sie in der Northwind-Datenbank bereitstellen. Beim Bereitstellen eines SQL Server Projekts wird der Code in eine Assembly kompiliert, die Assembly wird bei der Datenbank registriert, und die entsprechenden Objekte werden in der Datenbank erstellt, sodass Sie mit den entsprechenden Methoden in der Assembly verknüpft werden. Der genaue Satz von Tasks, der von der Bereitstellungs Option ausgeführt wird, wird in Schritt 13 genauer beschrieben. Klicken Sie mit der rechten Maustaste auf den Namen des `ManagedDatabaseConstructs` Projekts im Projektmappen-Explorer, und wählen Sie die Option bereitstellen aus. Die Bereitstellung schlägt jedoch mit folgendem Fehler fehl: falsche Syntax in der Nähe von "extern". Möglicherweise müssen Sie für den Kompatibilitätsgrad der aktuellen Datenbank einen höheren Wert festlegen, um diese Funktion zu aktivieren. Weitere Informationen finden Sie in der Hilfe zum `sp_dbcmptlevel`der gespeicherten Prozedur

Diese Fehlermeldung tritt auf, wenn versucht wird, die Assembly bei der Northwind-Datenbank zu registrieren. Um eine Assembly bei einer SQL Server 2005-Datenbank zu registrieren, muss der Kompatibilitäts Grad der Datenbank auf 90 festgelegt werden. Standardmäßig haben neue SQL Server 2005-Datenbanken einen Kompatibilitäts Grad von 90. Datenbanken, die mit Microsoft SQL Server 2000 erstellt wurden, haben jedoch den Standard Kompatibilitäts Grad 80. Da die Northwind-Datenbank anfänglich eine Microsoft SQL Server 2000-Datenbank war, ist Ihr Kompatibilitäts Grad derzeit auf 80 festgelegt und muss daher auf 90 erweitert werden, um verwaltete Datenbankobjekte registrieren zu können.

Öffnen Sie ein neues Abfragefenster in Management Studio, und geben Sie Folgendes ein, um den Kompatibilitäts Grad der Datenbank zu aktualisieren:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Klicken Sie in der Symbolleiste auf das Symbol ausführen, um die obige Abfrage auszuführen.

[Aktualisieren Sie den Kompatibilitäts Grad der Datenbank "Northwind" ![.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Abbildung 9**: Aktualisieren des Kompatibilitäts Niveaus der Datenbank "Northwind" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))

Nachdem Sie den Kompatibilitäts Grad aktualisiert haben, stellen Sie das SQL Server Projekt erneut bereit. Dieses Mal sollte die Bereitstellung ohne Fehler beendet werden.

Kehren Sie zu SQL Server Management Studio zurück, klicken Sie mit der rechten Maustaste auf die Datenbank Northwind im Objekt-Explorer, und wählen Sie aktualisieren aus. Führen Sie als nächstes einen Drilldown in den Ordner Programmierbarkeit und dann den Ordner Assemblys aus. Wie in Abbildung 10 gezeigt, enthält die Datenbank Northwind nun die Assembly, die vom `ManagedDatabaseConstructs` Projekt generiert wurde.

![Die manageddatabaseconstructs-Assembly ist jetzt bei der Northwind-Datenbank registriert.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Abbildung 10**: die `ManagedDatabaseConstructs`-Assembly ist jetzt bei der Northwind-Datenbank registriert.

Erweitern Sie außerdem den Ordner gespeicherte Prozeduren. Dort wird eine gespeicherte Prozedur mit dem Namen `GetDiscontinuedProducts`angezeigt. Diese gespeicherte Prozedur wurde vom Bereitstellungs Prozess erstellt und verweist auf die `GetDiscontinuedProducts`-Methode in der `ManagedDatabaseConstructs`-Assembly. Wenn die gespeicherte Prozedur `GetDiscontinuedProducts` ausgeführt wird, führt Sie ihrerseits die `GetDiscontinuedProducts`-Methode aus. Da es sich hierbei um eine verwaltete gespeicherte Prozedur handelt, kann Sie nicht über Management Studio (also das Sperrsymbol neben dem Namen der gespeicherten Prozedur) bearbeitet werden.

![Die gespeicherte Prozedur getdiscontinuedproducts ist im Ordner gespeicherte Prozeduren aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Abbildung 11**: die gespeicherte Prozedur `GetDiscontinuedProducts` ist im Ordner gespeicherte Prozeduren aufgeführt.

Es gibt immer noch eine weitere Hürde, die wir beheben müssen, bevor wir die verwaltete gespeicherte Prozedur abrufen können: die Datenbank ist so konfiguriert, dass die Ausführung von verwaltetem Code verhindert wird. Überprüfen Sie dies, indem Sie ein neues Abfragefenster öffnen und die gespeicherte Prozedur `GetDiscontinuedProducts` ausführen. Sie erhalten die folgende Fehlermeldung: die Ausführung von Benutzercode in der .NET Framework ist deaktiviert. Aktivieren Sie die Konfigurationsoption "CLR-fähig".

Um die Konfigurationsinformationen für die Northwind-Datenbank zu überprüfen, geben Sie ein, und führen Sie den Befehl `exec sp_configure` im Abfragefenster aus. Dies zeigt, dass die Einstellung für CLR-fähig derzeit auf 0 festgelegt ist.

[![die Einstellung für CLR-fähig aktuell auf 0 festgelegt ist.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Abbildung 12**: die Einstellung für CLR-fähig ist derzeit auf 0 festgelegt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))

Beachten Sie, dass für jede Konfigurationseinstellung in Abbildung 12 vier Werte aufgelistet sind: die minimal-und Maximalwerte und die Konfigurations-und die Run-Werte. Führen Sie den folgenden Befehl aus, um den Konfigurations Wert für die Einstellung "CLR-fähig" zu aktualisieren:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Wenn Sie die `exec sp_configure` erneut ausführen, sehen Sie, dass die oben genannte Anweisung den Konfigurations Wert für CLR-aktivierte Einstellung s auf 1 aktualisiert hat, der Lauf Wert jedoch weiterhin auf 0 festgelegt ist. Damit diese Konfigurationsänderung wirksam wird, müssen wir den [`RECONFIGURE` Befehl](https://msdn.microsoft.com/library/ms176069.aspx)ausführen, mit dem der Ausführungs Wert auf den aktuellen Konfigurations Wert festgelegt wird. Geben Sie im Abfragefenster einfach `RECONFIGURE` ein, und klicken Sie auf der Symbolleiste auf das Symbol ausführen. Wenn Sie `exec sp_configure` jetzt ausführen, sollten Sie den Wert 1 für die Einstellung s config und Run Werte für CLR-fähig sehen.

Wenn die CLR-aktivierte Konfiguration abgeschlossen ist, können Sie die gespeicherte Prozedur `GetDiscontinuedProducts` ausführen. Geben Sie im Abfragefenster den Befehl `exec` `GetDiscontinuedProducts`ein, und führen Sie ihn aus. Das Aufrufen der gespeicherten Prozedur bewirkt, dass der entsprechende verwaltete Code in der `GetDiscontinuedProducts`-Methode ausgeführt wird. Dieser Code gibt eine `SELECT` Abfrage aus, um alle Produkte zurückzugeben, die nicht mehr unterstützt werden, und gibt diese Daten an die aufrufenden Anwendung zurück, die in dieser Instanz SQL Server Management Studio ist. Management Studio empfängt diese Ergebnisse und zeigt Sie im Fenster Ergebnisse an.

[![die gespeicherte Prozedur getdiscontinuedproducts alle nicht mehr unterstützten Produkte zurückgibt](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Abbildung 13**: die gespeicherte Prozedur `GetDiscontinuedProducts` gibt alle nicht mehr unterstützten Produkte zurück ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))

## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Schritt 5: Erstellen von verwalteten gespeicherten Prozeduren, die Eingabeparameter akzeptieren

Viele der Abfragen und gespeicherten Prozeduren, die wir in diesen Tutorials erstellt haben, haben *Parameter*verwendet. Beispielsweise haben Sie im Tutorial [Erstellen neuer gespeicherter Prozeduren für das typisierte DataSet s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) eine gespeicherte Prozedur mit dem Namen `GetProductsByCategoryID` erstellt, die einen Eingabeparameter mit dem Namen `@CategoryID`akzeptiert. Die gespeicherte Prozedur hat dann alle Produkte zurückgegeben, deren `CategoryID` Feld mit dem Wert des angegebenen `@CategoryID` Parameters übereinstimmt.

Um eine verwaltete gespeicherte Prozedur zu erstellen, die Eingabeparameter akzeptiert, geben Sie diese Parameter einfach in der Definition der Methode an. Um dies zu veranschaulichen, fügen Sie dem `ManagedDatabaseConstructs` Projekt mit dem Namen `GetProductsWithPriceLessThan`eine weitere verwaltete gespeicherte Prozedur hinzu. Diese verwaltete gespeicherte Prozedur akzeptiert einen Eingabeparameter, der einen Preis angibt, und gibt alle Produkte zurück, deren `UnitPrice` Feld kleiner ist als der Wert des-Parameters.

Um dem Projekt eine neue gespeicherte Prozedur hinzuzufügen, klicken Sie mit der rechten Maustaste auf den Namen des `ManagedDatabaseConstructs` Projekts, und wählen Sie eine neue gespeicherte Prozedur hinzufügen aus. Benennen Sie die Datei `GetProductsWithPriceLessThan.vb`. Wie in Schritt 3 gezeigt, wird hierdurch eine neue Visual Basic-Klassendatei erstellt, die eine Methode mit dem Namen `GetProductsWithPriceLessThan` innerhalb der `Partial` Klasse `StoredProcedures`platziert.

Aktualisieren Sie die Definition der `GetProductsWithPriceLessThan`-Methode, sodass Sie einen [`SqlMoney`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) Eingabeparameter mit dem Namen `price` annimmt und den Code zum Ausführen und Zurückgeben der Abfrageergebnisse zu schreiben:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

Die Definition und der Code der `GetProductsWithPriceLessThan`-Methode ähneln genau der Definition und dem Code der in Schritt 3 erstellten `GetDiscontinuedProducts` Methode. Der einzige Unterschied besteht darin, dass die `GetProductsWithPriceLessThan`-Methode als Eingabeparameter (`price`) akzeptiert, dass die `SqlCommand` s-Abfrage einen Parameter (`@MaxPrice`) enthält und dass der `SqlCommand`-Auflistung ein Parameter hinzugefügt wird und der Wert der `Parameters` Variable zugewiesen wird.`price`

Nachdem Sie diesen Code hinzugefügt haben, stellen Sie das SQL Server Projekt erneut bereit. Kehren Sie als nächstes zu SQL Server Management Studio zurück, und aktualisieren Sie den Ordner gespeicherte Prozeduren. Es sollte ein neuer Eintrag, `GetProductsWithPriceLessThan`, angezeigt werden. Geben Sie in einem Abfragefenster den Befehl `exec GetProductsWithPriceLessThan 25`ein, und führen Sie ihn aus, in dem alle Produkte aufgeführt werden, die kleiner als $25 sind, wie in Abbildung 14 gezeigt

[![Produkte unter $25 werden angezeigt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Abbildung 14**: Produkte unter $25 werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))

## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Schritt 6: Aufrufen der verwalteten gespeicherten Prozedur von der Datenzugriffs Ebene

An diesem Punkt haben wir dem `ManagedDatabaseConstructs` Projekt die `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` verwalteten gespeicherten Prozeduren hinzugefügt und Sie mit der Northwind-SQL Server Datenbank registriert. Wir haben diese verwalteten gespeicherten Prozeduren auch aus SQL Server Management Studio aufgerufen (siehe Abbildung 13 und 14). Damit unsere ASP.NET-Anwendung diese verwalteten gespeicherten Prozeduren verwenden kann, müssen wir Sie jedoch den Datenzugriffs-und Geschäftslogik Ebenen in der Architektur hinzufügen. In diesem Schritt fügen wir zwei neue Methoden zum `ProductsTableAdapter` in das `NorthwindWithSprocs` typisierte Dataset hinzu, das anfänglich im Tutorial [Erstellen neuer gespeicherter Prozeduren für das TableAdapters für typisierte Datasets](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) erstellt wurde. In Schritt 7 fügen wir der BLL entsprechende Methoden hinzu.

Öffnen Sie in Visual Studio das `NorthwindWithSprocs` typisierte DataSet, und fügen Sie dem `ProductsTableAdapter` mit dem Namen `GetDiscontinuedProducts`eine neue Methode hinzu. Wenn Sie einem TableAdapter eine neue Methode hinzufügen möchten, klicken Sie im Designer mit der rechten Maustaste auf den Namen des TableAdapter s, und wählen Sie im Kontextmenü die Option Abfrage hinzufügen aus.

> [!NOTE]
> Da wir die Northwind-Datenbank aus dem Ordner `App_Data` in die SQL Server 2005 Express Edition Daten Bank Instanz verschoben haben, ist es zwingend erforderlich, dass die entsprechende Verbindungs Zeichenfolge in Web. config aktualisiert wird, um diese Änderung widerzuspiegeln. In Schritt 2 haben wir das Aktualisieren des `NORTHWNDConnectionString` Werts in `Web.config`erläutert. Wenn Sie dieses Update vergessen haben, wird die Fehlermeldung Fehler beim Hinzufügen der Abfrage angezeigt. Es wurde keine Verbindung `NORTHWNDConnectionString` für Objekt `Web.config` in einem Dialogfeld gefunden, wenn versucht wird, dem TableAdapter eine neue Methode hinzuzufügen. Um diesen Fehler zu beheben, klicken Sie auf OK, navigieren Sie zu `Web.config`, und aktualisieren Sie den `NORTHWNDConnectionString` Wert, wie in Schritt 2 beschrieben. Versuchen Sie dann erneut, die Methode dem TableAdapter hinzuzufügen. Dieses Mal sollte Sie ohne Fehler funktionieren.

Durch das Hinzufügen einer neuen Methode wird der Konfigurations-Assistent für TableAdapter-Abfragen gestartet, den wir in früheren Tutorials mehrmals verwendet haben. Im ersten Schritt werden wir aufgefordert, anzugeben, wie der TableAdapter auf die Datenbank zugreifen soll: über eine Ad-hoc-SQL-Anweisung oder über eine neue oder vorhandene gespeicherte Prozedur. Da wir die `GetDiscontinuedProducts` verwaltete gespeicherte Prozedur bereits erstellt und mit der Datenbank registriert haben, wählen Sie die Option vorhandene gespeicherte Prozedur verwenden aus, und klicken Sie auf Weiter.

[Wählen Sie die Option vorhandene gespeicherte Prozedur verwenden ![aus.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Abbildung 15**: Auswählen der Option zum Verwenden vorhandener gespeicherter Prozeduren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))

Im nächsten Bildschirm werden wir zur Eingabe der gespeicherten Prozedur aufgefordert, die von der Methode aufgerufen wird. Wählen Sie in der Dropdown Liste die `GetDiscontinuedProducts` verwaltete gespeicherte Prozedur aus, und klicken Sie auf Weiter.

[![Sie die verwaltete gespeicherte Prozedur getdiscontinuedproducts aus.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Abbildung 16**: Auswählen der verwalteten gespeicherten Prozedur `GetDiscontinuedProducts` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))

Anschließend werden wir aufgefordert, anzugeben, ob die gespeicherte Prozedur Zeilen, einen einzelnen Wert oder nichts zurückgibt. Da `GetDiscontinuedProducts` den Satz nicht mehr unterstützter Produkt Zeilen zurückgibt, wählen Sie die erste Option (Tabellendaten) aus, und klicken Sie auf Weiter.

[![wählen Sie die Option Tabellendaten aus.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Abbildung 17**: Auswählen der Option für tabellarische Daten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))

Im letzten Assistenten können wir die verwendeten Datenzugriffs Muster und die Namen der resultierenden Methoden angeben. Lassen Sie beide Kontrollkästchen aktiviert, und benennen Sie die Methoden `FillByDiscontinued` und `GetDiscontinuedProducts`. Klicken Sie auf Fertigstellen, um den Assistenten abzuschließen.

[![den Namen der Methoden fillbyeingestellt und getdiscontinuedproducts.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Abbildung 18**: Benennen der Methoden `FillByDiscontinued` und `GetDiscontinuedProducts` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))

Wiederholen Sie diese Schritte, um Methoden mit dem Namen `FillByPriceLessThan` zu erstellen und im `ProductsTableAdapter` für die `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur `GetProductsWithPriceLessThan`.

Abbildung 19 zeigt einen Screenshot des DataSet-Designers, nachdem die Methoden zum `ProductsTableAdapter` für die `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` verwalteten gespeicherten Prozeduren hinzugefügt wurden.

[![ProductsTableAdapter die neuen Methoden enthält, die in diesem Schritt hinzugefügt wurden.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Abbildung 19**: der `ProductsTableAdapter` enthält die neuen Methoden, die in diesem Schritt hinzugefügt wurden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))

## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Schritt 7: Hinzufügen entsprechender Methoden zur Geschäftslogik Ebene

Nachdem Sie die Datenzugriffs Ebene aktualisiert haben, sodass Sie Methoden zum Aufrufen der in den Schritten 4 und 5 hinzugefügten verwalteten gespeicherten Prozeduren enthält, müssen wir der Geschäftslogik Schicht entsprechende Methoden hinzufügen. Fügen Sie der `ProductsBLLWithSprocs`-Klasse die folgenden beiden Methoden hinzu:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Beide Methoden geben einfach die entsprechende dal-Methode an und geben die `ProductsDataTable` Instanz zurück. Das `DataObjectMethodAttribute` Markup oberhalb jeder Methode bewirkt, dass diese Methoden in der Dropdown Liste auf der Registerkarte auswählen des Assistenten zum Konfigurieren von Datenquellen für ObjectDataSource s enthalten sind.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Schritt 8: Aufrufen der verwalteten gespeicherten Prozeduren von der Darstellungs Schicht

Wenn die Geschäftslogik und die Datenzugriffsebenen erweitert wurden, um Unterstützung für das Aufrufen der `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` verwalteten gespeicherten Prozeduren zu bieten, können wir diese gespeicherten Prozeduren nun über eine ASP.NET-Seite anzeigen.

Öffnen Sie die Seite `ManagedFunctionsAndSprocs.aspx` im Ordner `AdvancedDAL`, und ziehen Sie aus der Toolbox eine GridView-Ansicht auf den Designer. Legen Sie die Eigenschaft GridView s `ID` auf `DiscontinuedProducts` fest, und binden Sie das Smarttag an eine neue ObjectDataSource mit dem Namen `DiscontinuedProductsDataSource`. Konfigurieren Sie ObjectDataSource so, dass die Daten aus der `ProductsBLLWithSprocs` Klasse `GetDiscontinuedProducts` Methode abgerufen werden.

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbllwithsprocs-Klasse.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Abbildung 20**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLLWithSprocs`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))

[![wählen Sie in der Dropdown Liste auf der Registerkarte auswählen die Methode getdiscontinuedproducts aus.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Abbildung 21**: Auswählen der `GetDiscontinuedProducts` Methode aus der Dropdown Liste auf der Registerkarte "auswählen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))

Da dieses Raster zum Anzeigen von Produktinformationen verwendet wird, legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest, und klicken Sie dann auf Fertigstellen.

Nachdem Sie den Assistenten abgeschlossen haben, fügt Visual Studio automatisch ein BoundField-oder CheckBoxField-Element für jedes Datenfeld in der `ProductsDataTable`hinzu. Nehmen Sie sich einen Moment Zeit, um alle diese Felder außer `ProductName` und `Discontinued`zu entfernen. zu diesem Zeitpunkt sollten das deklarative Markup der GridView-und ObjectDataSource s in etwa wie folgt aussehen:

[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Nehmen Sie sich einen Moment Zeit, um diese Seite über einen Browser anzuzeigen. Wenn die Seite besucht wird, ruft ObjectDataSource die `ProductsBLLWithSprocs` Klasse s `GetDiscontinuedProducts` Methode auf. Wie in Schritt 7 erläutert, ruft diese Methode den dal s-`ProductsDataTable` Class-`GetDiscontinuedProducts` Methode auf, die die gespeicherte Prozedur `GetDiscontinuedProducts` aufruft. Diese gespeicherte Prozedur ist eine verwaltete gespeicherte Prozedur und führt den Code aus, den wir in Schritt 3 erstellt haben, und gibt die nicht mehr unterstützten Produkte zurück.

Die Ergebnisse, die von der verwalteten gespeicherten Prozedur zurückgegeben werden, werden von der Dal in eine `ProductsDataTable` verpackt und dann an die BLL zurückgegeben, die Sie dann an die Darstellungs Schicht zurückgibt, wo Sie an die GridView gebunden und angezeigt werden. Erwartungsgemäß listet das Raster die Produkte auf, die nicht mehr unterstützt wurden.

[![nicht mehr unterstützte Produkte aufgeführt](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Abbildung 22**: die unterstützten Produkte werden aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))

Fügen Sie der Seite ein Textfeld und eine weitere GridView hinzu. Lassen Sie in dieser GridView die Produkte anzeigen, die kleiner sind als der in das Textfeld eingegebene Wert, indem Sie die `ProductsBLLWithSprocs` Class s `GetProductsWithPriceLessThan`-Methode aufrufen.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Schritt 9: Erstellen und Aufrufen von T-SQL-UDFs

Bei benutzerdefinierten Funktionen (oder UDFs) handelt es sich um Datenbankobjekte, die die Semantik von Funktionen in Programmiersprachen genau imitieren. Wie eine Funktion in Visual Basic können UDFs eine Variable Anzahl von Eingabe Parametern einschließen und einen Wert eines bestimmten Typs zurückgeben. Eine UDF kann entweder skalare Daten, eine Zeichenfolge, eine ganze Zahl usw. oder Tabellendaten zurückgeben. Werfen Sie einen kurzen Blick auf beide Arten von UDFs, beginnend mit einer UDF, die einen skalaren Datentyp zurückgibt.

Mit der folgenden UDF wird der geschätzte Wert des Bestands für ein bestimmtes Produkt berechnet. Hierzu werden drei Eingabeparameter verwendet: die Werte für `UnitPrice`, `UnitsInStock`und `Discontinued` für ein bestimmtes Produkt, und es wird ein Wert vom Typ `money`zurückgegeben. Der geschätzte Wert des Inventars wird berechnet, indem die `UnitPrice` mit dem `UnitsInStock`multipliziert werden. Bei nicht unterstützten Elementen wird dieser Wert halbiert.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Nachdem diese benutzerdefinierte Funktion der-Datenbank hinzugefügt wurde, können Sie Sie über Management Studio finden, indem Sie den Programmierbarkeits Ordner, dann Functions und dann skalare-Wert-Funktionen erweitern. Sie kann in einer `SELECT` Abfrage wie folgt verwendet werden:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

Ich habe die `udf_ComputeInventoryValue` UDF zur Datenbank Northwind hinzugefügt. Abbildung 23 zeigt die Ausgabe der obigen `SELECT` Abfrage, wenn Sie durch Management Studio angezeigt wird. Beachten Sie auch, dass die UDF unter dem Ordner Skalarwertfunktionen in der Objekt-Explorer aufgeführt ist.

[![die Inventur Werte jedes Produkts aufgeführt werden.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Abbildung 23**: die Inventur Werte für alle Produkte werden aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))

UDFs können auch Tabellendaten zurückgeben. Beispielsweise können wir eine benutzerdefinierte Funktion erstellen, die Produkte zurückgibt, die zu einer bestimmten Kategorie gehören:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

Der `udf_GetProductsByCategoryID` UDF akzeptiert einen `@CategoryID` Input-Parameter und gibt die Ergebnisse der angegebenen `SELECT` Abfrage zurück. Nach der Erstellung kann auf diese UDF in der `FROM`-Klausel (oder `JOIN`) einer `SELECT` Abfrage verwiesen werden. Im folgenden Beispiel werden die Werte für `ProductID`, `ProductName`und `CategoryID` für jedes der Getränke zurückgegeben.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

Ich habe die `udf_GetProductsByCategoryID` UDF zur Datenbank Northwind hinzugefügt. Abbildung 24 zeigt die Ausgabe der obigen `SELECT` Abfrage, wenn Sie durch Management Studio angezeigt wird. UDFs, die tabellarische Daten zurückgeben, finden Sie im Ordner mit den Objekt-Explorer s-Tabellenwert Funktionen.

[!["ProductID", "ProductName" und "CategoryID" für jedes Getränk aufgelistet.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Abbildung 24**: die `ProductID`, `ProductName`und `CategoryID` werden für jedes Getränk aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))

> [!NOTE]
> Weitere Informationen zum Erstellen und Verwenden von UDFs finden Sie unter Einführung [in benutzerdefinierte Funktionen](http://www.sqlteam.com/item.asp?ItemID=1955). Sehen Sie sich auch die [Vorteile und Nachteile von benutzerdefinierten Funktionen an](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).

## <a name="step-10-creating-a-managed-udf"></a>Schritt 10: Erstellen einer verwalteten UDF

Die in den obigen Beispielen erstellten `udf_ComputeInventoryValue` und `udf_GetProductsByCategoryID` UDFs sind t-SQL-Datenbankobjekte. SQL Server 2005 unterstützt auch verwaltete UDFs, die dem `ManagedDatabaseConstructs`-Projekt hinzugefügt werden können, genau wie die verwalteten gespeicherten Prozeduren aus den Schritten 3 und 5. In diesem Schritt implementieren Sie die `udf_ComputeInventoryValue` UDF in verwaltetem Code.

Um dem `ManagedDatabaseConstructs` Projekt eine verwaltete benutzerdefinierte Funktion hinzuzufügen, klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie ein neues Element hinzufügen aus. Wählen Sie im Dialogfeld Neues Element hinzufügen die benutzerdefinierte Vorlage aus, und benennen Sie die neue UDF-Datei `udf_ComputeInventoryValue_Managed.vb`.

[![dem Projekt manageddatabaseconstructs eine neue verwaltete UDF hinzufügen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Abbildung 25**: Hinzufügen einer neuen verwalteten UDF zum `ManagedDatabaseConstructs` Projekt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))

Die Vorlage für benutzerdefinierte Funktionen erstellt eine `Partial` Klasse mit dem Namen `UserDefinedFunctions` mit einer Methode, deren Name mit dem Namen der Klassendatei (`udf_ComputeInventoryValue_Managed`in dieser Instanz) identisch ist. Diese Methode wird mithilfe des [`SqlFunction`-Attributs](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx)ergänzt, das die Methode als verwaltete UDF ausweist.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

Die `udf_ComputeInventoryValue`-Methode gibt derzeit ein [`SqlString`-Objekt](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) zurück und akzeptiert keine Eingabeparameter. Wir müssen die Methoden Definition aktualisieren, damit Sie drei Eingabeparameter akzeptiert: `UnitPrice`, `UnitsInStock`und `Discontinued`, und ein `SqlMoney` Objekt zurückgeben. Die Logik zum Berechnen des Inventur Werts ist identisch mit der Logik in der T-SQL-`udf_ComputeInventoryValue`-UDF.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Beachten Sie, dass es sich bei den Eingabe Parametern der UDF-Methode um die entsprechenden SQL-Typen handelt: `SqlMoney` für das `UnitPrice` Feld, [`SqlInt16`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) für `UnitsInStock`und [`SqlBoolean`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) für `Discontinued`. Diese Datentypen entsprechen den Typen, die in der `Products` Tabelle definiert sind: die `UnitPrice` Spalte hat den Typ `money`, die `UnitsInStock` Spalte vom Typ `smallint`und die Spalte `Discontinued` vom Typ `bit`.

Der Code beginnt mit dem Erstellen einer `SqlMoney` Instanz mit dem Namen `inventoryValue`, der der Wert 0 zugewiesen wird. Die `Products` Tabelle ermöglicht Daten Bank `NULL` Werte in den Spalten `UnitsInPrice` und `UnitsInStock`. Daher müssen wir zunächst überprüfen, ob diese Werte `NULL` s enthalten, die wir über die `SqlMoney` Objekt- [`IsNull` Eigenschaft](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx)durchführen. Wenn sowohl `UnitPrice` als auch `UnitsInStock` nicht`NULL` Werte enthalten, berechnen wir die `inventoryValue` als Produkt der beiden. Wenn `Discontinued` dann true ist, wird der Wert halbiert.

> [!NOTE]
> Das `SqlMoney`-Objekt ermöglicht nur das Multiplizieren zweier `SqlMoney` Instanzen. Es ist nicht zulässig, dass eine `SqlMoney` Instanz mit einer literalen Gleit Komma Zahl multipliziert wird. Um die `inventoryValue` zu halbieren, multiplizieren wir Sie daher mit einer neuen `SqlMoney` Instanz mit dem Wert 0,5.

## <a name="step-11-deploying-the-managed-udf"></a>Schritt 11: Bereitstellen der verwalteten UDF

Nachdem die verwaltete UDF nun erstellt wurde, können wir Sie in der Northwind-Datenbank bereitstellen. Wie in Schritt 4 gezeigt, werden die verwalteten Objekte in einem SQL Server Projekt bereitgestellt, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen klicken und im Kontextmenü die Option bereitstellen auswählen.

Nachdem Sie das Projekt bereitgestellt haben, kehren Sie zu SQL Server Management Studio zurück, und aktualisieren Sie den Ordner mit den skalaren Wert Funktionen. Nun sollten zwei Einträge angezeigt werden:

- `dbo.udf_ComputeInventoryValue`-die in Schritt 9 erstellte t-SQL-UDF und
- `dbo.udf ComputeInventoryValue_Managed`: die in Schritt 10 erstellte verwaltete UDF, die gerade bereitgestellt wurde.

Um diese verwaltete UDF zu testen, führen Sie die folgende Abfrage in Management Studio aus:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Dieser Befehl verwendet die verwaltete `udf ComputeInventoryValue_Managed` UDF anstelle des t-SQL-`udf_ComputeInventoryValue` UDF, aber die Ausgabe ist identisch. In Abbildung 23 finden Sie einen Screenshot der UDF s-Ausgabe.

## <a name="step-12-debugging-the-managed-database-objects"></a>Schritt 12: Debuggen der verwalteten Datenbankobjekte

Im Tutorial zum [Debuggen gespeicherter Prozeduren](debugging-stored-procedures-vb.md) wurden die drei Optionen zum Debuggen von SQL Server über Visual Studio erläutert: direktes Debuggen SQL Server von Datenbanken, Debuggen von Anwendungen und Debuggen Verwaltete Datenbankobjekte können nicht über das direkte Daten Bank Debugging debuggt werden, Sie können jedoch von einer Client Anwendung aus und direkt aus dem SQL Server Projekt debuggt werden. Damit das Debuggen funktioniert, muss die SQL Server 2005-Datenbank jedoch SQL/CLR-Debugging zulassen. Beim ersten Erstellen des `ManagedDatabaseConstructs` Projekts fragte Visual Studio, ob das SQL/CLR-Debuggen aktiviert werden soll (siehe Abbildung 6 in Schritt 2). Diese Einstellung kann geändert werden, indem Sie im Server-Explorer Fenster mit der rechten Maustaste auf die Datenbank klicken.

![Sicherstellen, dass die Datenbank SQL/CLR-Debugging zulässt](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Abbildung 26**: sicherstellen, dass die Datenbank SQL/CLR-Debugging zulässt

Stellen Sie sich vor, dass die `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur debuggt werden soll. Wir legen zunächst einen Haltepunkt im Code der `GetProductsWithPriceLessThan` Methode fest.

[![einen Haltepunkt in der getproductwithpriceless than-Methode festlegen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Abbildung 27**: Festlegen eines Breakpoints in der `GetProductsWithPriceLessThan`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))

Sehen Sie sich zunächst das Debuggen der verwalteten Datenbankobjekte aus dem SQL Server-Projekt an. Da unsere Lösung zwei Projekte enthält: die `ManagedDatabaseConstructs` SQL Server Projekt zusammen mit unserer Website. um das SQL Server Projekt zu debuggen, müssen Sie Visual Studio anweisen, das `ManagedDatabaseConstructs` SQL Server Projekt beim Debuggen zu starten. Klicken Sie mit der rechten Maustaste auf das Projekt `ManagedDatabaseConstructs` in Projektmappen-Explorer, und wählen Sie im Kontextmenü die Option als Startprojekt festlegen aus.

Wenn das `ManagedDatabaseConstructs` Projekt vom Debugger gestartet wird, werden die SQL-Anweisungen in der Datei `Test.sql` ausgeführt, die sich im Ordner `Test Scripts` befindet. Wenn Sie z. b. die `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur testen möchten, ersetzen Sie den vorhandenen `Test.sql` Dateiinhalt durch die folgende Anweisung, die die `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur aufruft, die den `@CategoryID` Wert 14,95 übergibt:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Nachdem Sie das obige Skript in `Test.sql`eingegeben haben, starten Sie das Debuggen, indem Sie im Menü Debuggen auf Debuggen starten klicken oder F5 oder das grüne Wiedergabe Symbol auf der Symbolleiste drücken. Dadurch werden die Projekte in der Projekt Mappe erstellt, die verwalteten Datenbankobjekte in der Northwind-Datenbank bereitgestellt und dann das `Test.sql` Skript ausgeführt. An diesem Punkt wird der Breakpoint angezeigt, und wir können die `GetProductsWithPriceLessThan`-Methode schrittweise durchlaufen, die Werte der Eingabeparameter überprüfen usw.

[![den Breakpoint in der getproductwithpriceless than-Methode wurde gefunden.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Abbildung 28**: der Haltepunkt in der `GetProductsWithPriceLessThan`-Methode wurde getroffen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))

Damit ein SQL-Datenbankobjekt über eine Client Anwendung debuggt werden kann, ist es zwingend erforderlich, dass die Datenbank so konfiguriert wird, dass das Anwendungs Debugging unterstützt wird. Klicken Sie in Server-Explorer mit der rechten Maustaste auf die Datenbank, und stellen Sie sicher, dass die Option Anwendungs Debugging aktiviert ist. Außerdem müssen wir die ASP.NET-Anwendung so konfigurieren, dass Sie in den SQL-Debugger integriert und das Verbindungspooling deaktiviert wird. Diese Schritte wurden in Schritt 2 des Tutorials zum [Debuggen gespeicherter Prozeduren](debugging-stored-procedures-vb.md) ausführlich erläutert.

Nachdem Sie die Anwendung und die Datenbank ASP.NET konfiguriert haben, legen Sie die ASP.NET-Website als Startprojekt fest, und starten Sie das Debuggen. Wenn Sie eine Seite aufrufen, die eines der verwalteten Objekte aufruft, die über einen Haltepunkt verfügt, wird die Anwendung angehalten, und die Steuerung wird an den Debugger übergeben, in dem Sie den Code schrittweise durchlaufen können (siehe Abbildung 28).

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Schritt 13: Manuelles kompilieren und Bereitstellen von verwalteten Datenbankobjekten

SQL Server Projekte vereinfachen die Erstellung, Kompilierung und Bereitstellung verwalteter Datenbankobjekte. Leider sind SQL Server Projekte nur in den Editionen Professional und Team Systems von Visual Studio verfügbar. Wenn Sie Visual Web Developer oder die Standard Edition von Visual Studio verwenden und verwaltete Datenbankobjekte verwenden möchten, müssen Sie diese manuell erstellen und bereitstellen. Dies umfasst vier Schritte:

1. Erstellen Sie eine Datei, die den Quellcode für das verwaltete Datenbankobjekt enthält.
2. Kompilieren Sie das Objekt in eine Assembly,
3. Registrieren Sie die Assembly bei der SQL Server 2005-Datenbank, und
4. Erstellen Sie in SQL Server ein Datenbankobjekt, das auf die entsprechende Methode in der Assembly zeigt.

Um diese Aufgaben zu veranschaulichen, erstellen Sie eine neue verwaltete gespeicherte Prozedur, die die Produkte zurückgibt, deren `UnitPrice` größer als ein angegebener Wert ist. Erstellen Sie auf Ihrem Computer eine neue Datei mit dem Namen `GetProductsWithPriceGreaterThan.vb`, und geben Sie den folgenden Code in die Datei ein (Sie können Visual Studio, Notepad oder einen beliebigen Text-Editor verwenden, um dies zu erreichen):

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Dieser Code ist nahezu identisch mit dem in Schritt 5 erstellten `GetProductsWithPriceLessThan` Methode. Die einzigen Unterschiede sind die Methodennamen, die `WHERE`-Klausel und der Parameter Name, der in der Abfrage verwendet wird. Zurück in der `GetProductsWithPriceLessThan`-Methode hat die `WHERE`-Klausel Folgendes gelesen: `WHERE UnitPrice < @MaxPrice`. Hier verwenden wir in `GetProductsWithPriceGreaterThan`: `WHERE UnitPrice > @MinPrice`.

Wir müssen diese Klasse nun in eine Assembly kompilieren. Navigieren Sie in der Befehlszeile zu dem Verzeichnis, in dem Sie die `GetProductsWithPriceGreaterThan.vb` Datei gespeichert haben C# , und verwenden Sie den Compiler (`csc.exe`), um die Klassendatei in eine Assembly zu kompilieren:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Wenn sich der Ordner, der die v-`bc.exe` enthält, nicht im `PATH`"System" befindet, müssen Sie den Pfad `%WINDOWS%\Microsoft.NET\Framework\version\`wie folgt vollständig referenzieren:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]

[!["getproduczwithpreigreaterthan. vb" in eine Assembly kompilieren](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Abbildung 29**: Kompilieren von `GetProductsWithPriceGreaterThan.vb` in eine Assembly ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))

Das `/t`-Flag gibt an, dass die Visual Basic Klassendatei in eine DLL (und nicht in eine ausführbare Datei) kompiliert werden soll. Das `/out`-Flag gibt den Namen der resultierenden Assembly an.

> [!NOTE]
> Anstatt die `GetProductsWithPriceGreaterThan.vb` Klassendatei von der Befehlszeile aus zu kompilieren, können Sie alternativ [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) verwenden oder in Visual Studio Standard Edition ein separates Klassen Bibliotheksprojekt erstellen. S ren Jacob Lauritsen hat ein solches Visual Basic Express Edition-Projekt mit Code für die gespeicherte Prozedur `GetProductsWithPriceGreaterThan` und die beiden verwalteten gespeicherten Prozeduren und UDF bereitgestellt, die in den Schritten 3, 5 und 10 erstellt wurden. Das s-Projekt enthält auch die T-SQL-Befehle, die zum Hinzufügen der entsprechenden Datenbankobjekte erforderlich sind.

Nachdem der Code in eine Assembly kompiliert wurde, können wir die Assembly innerhalb der SQL Server 2005-Datenbank registrieren. Dies kann über T-SQL erfolgen, mithilfe des Befehls `CREATE ASSEMBLY`oder über SQL Server Management Studio. Konzentrieren Sie sich auf die Verwendung Management Studio.

Erweitern Sie in Management Studio den Ordner Programmierbarkeit in der Northwind-Datenbank. Einer der Unterordner ist Assemblys. Wenn Sie der Datenbank manuell eine neue Assembly hinzufügen möchten, klicken Sie mit der rechten Maustaste auf den Ordner Assemblys, und wählen Sie im Kontextmenü neue Assembly aus. Dadurch wird das Dialogfeld neue Assembly angezeigt (siehe Abbildung 30). Klicken Sie auf die Schaltfläche Durchsuchen, wählen Sie die soeben kompilierte `ManuallyCreatedDBObjects.dll` Assembly aus, und klicken Sie dann auf OK, um die Assembly der Datenbank hinzuzufügen. Die `ManuallyCreatedDBObjects.dll` Assembly sollte in der Objekt-Explorer nicht angezeigt werden.

[![die Assembly manuallykreateddbobjects. dll zur Datenbank hinzufügen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Abbildung 30**: Hinzufügen der `ManuallyCreatedDBObjects.dll` Assembly zur Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))

![Die Datei "manuallykreateddbobjects. dll" wird im Objekt-Explorer](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Abbildung 31**: die `ManuallyCreatedDBObjects.dll` ist im Objekt-Explorer aufgeführt.

Obwohl wir die Assembly der Northwind-Datenbank hinzugefügt haben, müssen wir eine gespeicherte Prozedur mit der `GetProductsWithPriceGreaterThan`-Methode in der Assembly verknüpfen. Um dies zu erreichen, öffnen Sie ein neues Abfragefenster, und führen Sie das folgende Skript aus:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Dadurch wird eine neue gespeicherte Prozedur in der Northwind-Datenbank mit dem Namen `GetProductsWithPriceGreaterThan` erstellt und der verwalteten Methode `GetProductsWithPriceGreaterThan` zugeordnet (in der-Klasse `StoredProcedures`, die sich in der-Assembly befindet `ManuallyCreatedDBObjects`).

Aktualisieren Sie nach dem Ausführen des obigen Skripts den Ordner gespeicherte Prozeduren in der Objekt-Explorer. Es sollte ein neuer Eintrag für eine gespeicherte Prozedur angezeigt werden, `GetProductsWithPriceGreaterThan` mit einem Sperrsymbol daneben angezeigt wird. Um diese gespeicherte Prozedur zu testen, geben Sie das folgende Skript in das Abfragefenster ein, und führen Sie es aus:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Wie in Abbildung 32 gezeigt, zeigt der obige Befehl Informationen für diese Produkte mit einem `UnitPrice` an, der größer als $24,95 ist.

[![die Datei "manuallykreateddbobjects. dll" im Objekt-Explorer](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Abbildung 32**: die `ManuallyCreatedDBObjects.dll` ist im Objekt-Explorer aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))

## <a name="summary"></a>Zusammenfassung

Microsoft SQL Server 2005 bietet die Integration mit der Common Language Runtime (CLR), die das Erstellen von Datenbankobjekten mithilfe von verwaltetem Code ermöglicht. Zuvor konnten diese Datenbankobjekte nur mit T-SQL erstellt werden. Nun können wir diese Objekte jedoch mit .NET-Programmiersprachen wie Visual Basic erstellen. In diesem Tutorial haben wir zwei verwaltete gespeicherte Prozeduren und eine verwaltete benutzerdefinierte Funktion erstellt.

Der Visual Studio s-SQL Server Projekttyp erleichtert das Erstellen, kompilieren und Bereitstellen von verwalteten Datenbankobjekten. Außerdem bietet Sie umfassende Debuggingunterstützung. SQL Server Projekttypen sind jedoch nur in den Editionen Professional und Team Systems von Visual Studio verfügbar. Für Entwickler, die Visual Web Developer oder die Standard Edition von Visual Studio verwenden, müssen die Schritte zur Erstellung, Kompilierung und Bereitstellung manuell ausgeführt werden, wie in Schritt 13 erläutert.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Vorteile und Nachteile von benutzerdefinierten Funktionen](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Erstellen von SQL Server 2005-Objekten in verwaltetem Code](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Erstellen von Triggern mithilfe von verwaltetem Code in SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Gewusst wie: Erstellen und Ausführen einer gespeicherten CLR-Prozedur SQL Server](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Vorgehensweise: Erstellen und Ausführen einer CLR-SQL Server benutzerdefinierten Funktion](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Vorgehensweise: Bearbeiten des `Test.sql` Skripts zum Ausführen von SQL-Objekten](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Einführung in benutzerdefinierte Funktionen](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Verwalteter Code und SQL Server 2005 (Video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Transact-SQL-Referenz](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Exemplarische Vorgehensweise: Erstellen einer gespeicherten Prozedur in verwaltetem Code](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war S ren Jacob Lauritsen. Zusätzlich zur Überprüfung dieses Artikels hat S ren auch das Visual C# Express Edition-Projekt erstellt, das in diesem Artikel Download zum manuellen Kompilieren der verwalteten Datenbankobjekte enthalten ist. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Previous](debugging-stored-procedures-vb.md)
