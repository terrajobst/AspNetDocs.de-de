---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: Effizientes Paging durch große Datenmengen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Die Standard-Paging-Option eines Daten Präsentations Steuer Elements ist nicht geeignet, wenn Sie mit großen Datenmengen arbeiten, da das zugrunde liegende Datenquellen-Steuerelement abgerufen wird...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 0c788c4109d0d2839de969c628399290376a1ccd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74612951"
---
# <a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Effizientes Auslagern von großen Datenmengen (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) oder [PDF herunterladen](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> Die Standard-Paging-Option eines Daten Präsentations Steuer Elements ist nicht geeignet, wenn Sie mit großen Datenmengen arbeiten, da das zugrunde liegende Datenquellen-Steuerelement alle Datensätze abruft, auch wenn nur eine Teilmenge der Daten angezeigt wird. In diesen Fällen müssen wir das benutzerdefinierte Paging aktivieren.

## <a name="introduction"></a>Einführung

Wie bereits im vorherigen Tutorial erläutert, kann das Paging auf eine von zwei Arten implementiert werden:

- Das **Standardpaging** kann implementiert werden, indem Sie einfach die Option Paging aktivieren im Smarttags des dateiweb-Steuer Elements aktivieren. Wenn Sie jedoch eine Datenseite anzeigen, ruft ObjectDataSource *alle* Datensätze ab, auch wenn nur eine Teilmenge der Daten auf der Seite angezeigt wird.
- Durch **benutzerdefiniertes Paging** wird die Leistung der Standard Auslagerung verbessert, indem nur die Datensätze aus der Datenbank abgerufen werden, die für die vom Benutzer angeforderte Datenseite angezeigt werden müssen. das benutzerdefinierte Paging umfasst jedoch etwas mehr Aufwand für die Implementierung als die Standard Auslagerung.

Aufgrund der einfachen Implementierung aktivieren Sie ein Kontrollkästchen, und Sie haben es wieder getan! Standard-Paging ist eine attraktive Option. Wenn Sie alle Datensätze abrufen, ist dies jedoch eine unplausible Wahl, wenn Sie durch ausreichend große Datenmengen oder für Standorte mit vielen gleichzeitigen Benutzern Paging. In diesen Fällen müssen wir das benutzerdefinierte Paging aktivieren, um ein reaktionsfähiges System bereitzustellen.

Die Herausforderung von benutzerdefiniertem Paging besteht darin, eine Abfrage zu schreiben, die den genauen Satz von Datensätzen zurückgibt, die für eine bestimmte Datenseite benötigt werden. Glücklicherweise stellt Microsoft SQL Server 2005 ein neues Schlüsselwort für die Rangfolge der Ergebnisse bereit, das es uns ermöglicht, eine Abfrage zu schreiben, mit der die richtige Teilmenge der Datensätze effizient abgerufen werden kann. In diesem Tutorial erfahren Sie, wie Sie das neue Schlüsselwort "SQL Server 2005" verwenden, um benutzerdefiniertes Paging in einem GridView-Steuerelement zu implementieren. Obwohl die Benutzeroberfläche für benutzerdefiniertes Paging identisch mit der für die Standard Auslagerung ist, kann das Ausführen eines Einzel Vorgangs von einer Seite zur nächsten mithilfe von benutzerdefiniertem Paging mehrere Größenordnungen beschleunigen als die standardpaginierung.

> [!NOTE]
> Der genaue Leistungsgewinn, der durch das benutzerdefinierte Paging festgestellt wird, hängt von der Gesamtzahl der Datensätze ab, die durch den Datenbankserver eingefügt werden. Am Ende dieses Tutorials betrachten wir einige grobe Metriken, die die Vorteile der Leistung veranschaulichen, die durch das benutzerdefinierte Paging erzielt werden.

## <a name="step-1-understanding-the-custom-paging-process"></a>Schritt 1: Grundlegendes zum benutzerdefinierten pagingprozess

Beim Paging durch Daten sind die auf einer Seite angezeigten exakten Datensätze abhängig von der angeforderten Datenmenge und der Anzahl der pro Seite angezeigten Datensätze. Stellen Sie sich beispielsweise vor, dass Sie die 81-Produkte durchlaufen möchten, die 10 Produkte pro Seite anzeigen. Wenn Sie die erste Seite anzeigen, benötigen wir die Produkte 1 bis 10. beim Anzeigen der zweiten Seite interessieren wir uns für die Produkte 11 bis 20 usw.

Es gibt drei Variablen, die vorgeben, welche Datensätze abgerufen werden müssen und wie die pagingschnittstelle gerendert werden soll:

- **Zeilen Anfang Index** : der Index der ersten Zeile in der Datenseite, die angezeigt werden soll. Dieser Index kann berechnet werden, indem der Seitenindex von den Datensätzen multipliziert wird, die pro Seite angezeigt werden sollen. Wenn Sie z. b. für die erste Seite (deren Seitenindex den Wert 0 hat) die Datensätze 10 gleichzeitig durchsuchen, ist der Start Zeilen Index 0 \* 10 + 1 oder 1. für die zweite Seite (deren Seitenindex 1 ist) ist der Start Zeilen Index 1 \* 10 + 1 oder 11.
- **Maximale Zeilen** Anzahl die maximale Anzahl der Datensätze, die pro Seite angezeigt werden sollen. Diese Variable wird als maximale Anzahl von Zeilen bezeichnet, da für die letzte Seite möglicherweise weniger Datensätze als die Seitengröße zurückgegeben werden. Wenn Sie z. b. das Paging durch die 81 Produkte 10 Datensätze pro Seite durchlaufen, wird auf der neunten und letzten Seite nur ein Datensatz angezeigt. Keine Seite zeigt jedoch mehr Datensätze an als der Wert für die maximale Zeilen Anzahl.
- **Gesamt** Anzahl der Datensätze die Gesamtanzahl der Datensätze, die durchlaufen werden. Diese Variable ist zwar nicht erforderlich, um zu bestimmen, welche Datensätze für eine bestimmte Seite abgerufen werden sollen, aber es wird die Paging-Schnittstelle vorgegeben. Wenn z. b. 81 Produkte durchlaufen werden, weiß die Paging-Schnittstelle, dass neun Seitenzahlen in der Pagingbenutzeroberfläche angezeigt werden.

Beim standardmäßigen Paging wird der Start Zeilen Index als Produkt des Seiten Indexes und der Seitengröße Plus 1 berechnet, während die maximale Zeilen Anzahl einfach die Seitengröße ist. Da bei der Standard Auslagerung alle Datensätze aus der Datenbank abgerufen werden, wenn eine Datenseite gerendert wird, ist der Index für jede Zeile bekannt, sodass der Übergang zur Zeile für den Zeilen Index zu einer trivialen Aufgabe wird. Außerdem ist die Gesamtanzahl der Datensätze sofort verfügbar, da Sie einfach die Anzahl der Datensätze in der Datentabelle (oder das Objekt, das zum Speichern der Daten Bank Ergebnisse verwendet wird) enthält.

Bei Angabe der Variablen "Start Row Index" und "Maximum rows" muss eine benutzerdefinierte pagingimplementierung nur die genaue Teilmenge der Datensätze zurückgeben, beginnend beim Start Zeilen Index und die maximale Zeilen Anzahl von Datensätzen. Das benutzerdefinierte Paging bietet zwei Herausforderungen:

- Wir müssen in der Lage sein, jeder Zeile in den auslagerbaren Daten einen Zeilen Index effizient zuzuordnen, damit wir mit dem Zurückgeben von Datensätzen am angegebenen Start Zeilen Index beginnen können.
- Wir müssen die Gesamtzahl der Datensätze angeben, die durchlaufen werden.

In den nächsten beiden Schritten untersuchen wir das SQL-Skript, das für die Beantwortung dieser beiden Probleme erforderlich ist. Zusätzlich zum SQL-Skript müssen auch Methoden in Dal und BLL implementiert werden.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Schritt 2: Zurückgeben der Gesamtanzahl von Datensätzen, die durchlaufen werden

Bevor wir untersuchen, wie die genaue Teilmenge der Datensätze für die anzuzeigende Seite abgerufen wird, sehen Sie sich zunächst an, wie die Gesamtanzahl der Datensätze zurückgegeben werden soll, die durchlaufen werden. Diese Informationen sind erforderlich, um die Auslagerungs Benutzeroberfläche ordnungsgemäß zu konfigurieren. Die Gesamtanzahl der Datensätze, die von einer bestimmten SQL-Abfrage zurückgegeben werden, kann mithilfe der [`COUNT` Aggregatfunktion](https://msdn.microsoft.com/library/ms175997.aspx)abgerufen werden. Wenn Sie z. b. die Gesamtanzahl der Datensätze in der `Products` Tabelle ermitteln möchten, können Sie die folgende Abfrage verwenden:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Fügen Sie der dal eine Methode hinzu, die diese Informationen zurückgibt. Insbesondere erstellen wir eine dal-Methode mit dem Namen `TotalNumberOfProducts()`, die die oben gezeigte `SELECT` Anweisung ausführt.

Öffnen Sie zunächst die `Northwind.xsd` typisierte DataSet-Datei im Ordner `App_Code/DAL`. Klicken Sie als nächstes mit der rechten Maustaste auf die `ProductsTableAdapter` im Designer, und wählen Sie Abfrage hinzufügen aus. Wie bereits in den vorherigen Tutorials gezeigt, können wir der dal eine neue Methode hinzufügen, die, wenn Sie aufgerufen wird, eine bestimmte SQL-Anweisung oder gespeicherte Prozedur ausführt. Wie bei unseren TableAdapter-Methoden in vorherigen Tutorials haben Sie sich dafür entschieden, eine Ad-hoc-SQL-Anweisung zu verwenden.

![Verwenden einer Ad-hoc-SQL-Anweisung](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Abbildung 1**: Verwenden einer Ad-hoc-SQL-Anweisung

Auf dem nächsten Bildschirm können Sie angeben, welche Art von Abfrage erstellt werden soll. Da diese Abfrage einen einzelnen Skalarwert zurückgibt, wird die Gesamtanzahl der Datensätze in der `Products` Tabelle mit der `SELECT`, die die Option für das Debugwert zurückgibt.

![Konfigurieren der Abfrage für die Verwendung einer SELECT-Anweisung, die einen einzelnen Wert zurückgibt](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Abbildung 2**: Konfigurieren der Abfrage für die Verwendung einer SELECT-Anweisung, die einen einzelnen Wert zurückgibt

Nachdem Sie den Typ der zu verwendenden Abfrage angegeben haben, müssen Sie als nächstes die Abfrage angeben.

![Verwenden Sie die Abfrage SELECT count (*) from products.](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Abbildung 3**: Verwenden der SELECT count (\*) from Products-Abfrage

Geben Sie abschließend den Namen für die Methode an. Wie bereits erwähnt, können Sie `TotalNumberOfProducts`verwenden.

![Benennen Sie die DAL-Methode totalnumofproducts.](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Abbildung 4**: Benennen der dal-Methode totalnumofproducts

Nachdem Sie auf Fertigstellen geklickt haben, fügt der Assistent die `TotalNumberOfProducts` Methode der dal hinzu. Die skalaren Rückgabe Methoden in der dal geben Werte zulässt-Typen zurück, wenn das Ergebnis der SQL-Abfrage `NULL`ist. Unsere `COUNT` Abfrage gibt jedoch immer einen Wert zurück, der nicht`NULL` ist. die DAL-Methode gibt eine Ganzzahl zurück, die NULL-Werte zulässt.

Zusätzlich zur dal-Methode benötigen wir auch eine-Methode in der BLL. Öffnen Sie die Datei `ProductsBLL`-Klasse, und fügen Sie eine `TotalNumberOfProducts` Methode hinzu, die einfach an die DAL s-`TotalNumberOfProducts`-Methode aufruft:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

Die DAL s-`TotalNumberOfProducts` Methode gibt eine Ganzzahl zurück, die NULL zulässt. Wir haben jedoch die `ProductsBLL` Klasse s `TotalNumberOfProducts` Methode erstellt, sodass Sie eine Standard Ganzzahl zurückgibt. Daher muss die `ProductsBLL` Klasse s `TotalNumberOfProducts` Methode den Wert Teil der von der dal s `TotalNumberOfProducts`-Methode zurückgegebenen Ganzzahl zurückgeben, die NULL-Werte zulässt. Der Aufruf von `GetValueOrDefault()` gibt den Wert der Ganzzahl zurück, die NULL-Werte zulässt, wenn Sie vorhanden ist. Wenn die Ganzzahl, die NULL-Werte zulässt, `null`ist, gibt Sie jedoch den standardmäßigen ganzzahligen Wert 0 zurück.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Schritt 3: Zurückgeben der exakten Teilmenge von Datensätzen

Die nächste Aufgabe besteht darin, Methoden in der dal und der BLL zu erstellen, die die zuvor erläuterten Zeilen Index-und Maximum Rows-Variablen akzeptieren und die entsprechenden Datensätze zurückgeben. Bevor wir dies tun, betrachten wir zuerst das erforderliche SQL-Skript. Die Herausforderung besteht darin, dass wir in der Lage sein müssen, jeder Zeile in den gesamten auslagerenden Ergebnissen effizient einen Index zuzuweisen, damit wir nur die Datensätze zurückgeben können, beginnend beim Start Zeilen Index (und bis zur maximalen Anzahl von Datensätzen).

Dies ist keine Herausforderung, wenn in der Datenbanktabelle bereits eine Spalte vorhanden ist, die als Zeilen Index fungiert. Auf den ersten Blick könnten wir sehen, dass die `Products` Tabelle s `ProductID` Feld ausreichen würde, weil das erste Produkt `ProductID` von 1, das zweite a 2 usw. hat. Wenn Sie ein Produkt löschen, bleibt jedoch eine Lücke in der Sequenz, wodurch dieser Ansatz aufgehoben wird.

Es gibt zwei allgemeine Verfahren, mit denen ein Zeilen Index effizient mit den Daten verknüpft wird, die durchlaufen werden können, sodass die genaue Teilmenge der Datensätze abgerufen werden kann:

- Wenn **Sie SQL Server 2005 s `ROW_NUMBER()` Schlüsselwort** New in SQL Server 2005 verwenden, ordnet das `ROW_NUMBER()`-Schlüsselwort eine Rangfolge den einzelnen zurückgegebenen Datensätzen zu Diese Rangfolge kann als Zeilen Index für jede Zeile verwendet werden.
- **Verwenden einer Tabellen Variablen und `SET ROWCOUNT`** SQL Server s [`SET ROWCOUNT` Anweisung](https://msdn.microsoft.com/library/ms188774.aspx) kann verwendet werden, um anzugeben, wie viele Datensätze insgesamt von einer Abfrage verarbeitet werden sollen, bevor Sie beendet wird. [Tabellen Variablen](http://www.sqlteam.com/item.asp?ItemID=9454) sind lokale T-SQL-Variablen, die Tabellendaten enthalten können, vergleichbar mit [temporären Tabellen](http://www.sqlteam.com/item.asp?ItemID=2029). Dieser Ansatz funktioniert gleichermaßen gut mit Microsoft SQL Server 2005 und SQL Server 2000 (während der `ROW_NUMBER()` Ansatz nur mit SQL Server 2005) funktioniert.  
  
  Die Idee besteht darin, eine Tabellen Variable zu erstellen, die eine `IDENTITY` Spalte und Spalten für die Primärschlüssel der Tabelle enthält, deren Daten per Pager durchlaufen werden. Als nächstes wird der Inhalt der Tabelle, deren Daten durchlaufen werden, in die Tabellen Variable eingefügt, wodurch ein sequenzieller Zeilen Index (über die `IDENTITY` Spalte) für jeden Datensatz in der Tabelle zugeordnet wird. Nachdem die Tabellen Variable aufgefüllt wurde, kann eine `SELECT`-Anweisung für die Tabellen Variable, die mit der zugrunde liegenden Tabelle verknüpft ist, ausgeführt werden, um die jeweiligen Datensätze abzurufen. Die `SET ROWCOUNT`-Anweisung wird verwendet, um die Anzahl der Datensätze, die in die Tabellen Variable gekippt werden müssen, intelligent einzuschränken.  
  
  Die Effizienz dieses Ansatzes basiert auf der angeforderten Seitenzahl, da dem `SET ROWCOUNT` Wert der Wert des Start Zeilen Index zuzüglich der maximalen Zeilen zugewiesen wird. Beim Paging durch Seiten mit niedriger Nummerierung, wie z. b. die ersten Seiten der Daten, ist dieser Ansatz sehr effizient. Sie stellt jedoch beim Abrufen einer Seite am Ende eine standardmäßige Auslagerungs ähnliche Leistung dar.

In diesem Tutorial wird das benutzerdefinierte Paging mit dem `ROW_NUMBER()`-Schlüsselwort implementiert Weitere Informationen zur Verwendung der Tabellen Variablen und `SET ROWCOUNT` Technik finden Sie in [einer effizienteren Methode zum Paging durch große Resultsets](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

Das `ROW_NUMBER()`-Schlüsselwort, das einer Rangfolge mit jedem Datensatz zugeordnet ist, der über eine bestimmte Reihenfolge zurückgegeben wird

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` gibt einen numerischen Wert zurück, der den Rang für jeden Datensatz in Bezug auf die festgestellte Reihenfolge angibt. Um z. b. den Rang für jedes Produkt zu sehen, geordnet nach dem teuersten zum geringsten, können wir die folgende Abfrage verwenden:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

Abbildung 5 zeigt die Ergebnisse dieser Abfrage, wenn Sie über das Abfragefenster in Visual Studio ausgeführt werden. Beachten Sie, dass die Produkte nach Preis zusammen mit einem Preis Rang für jede Zeile geordnet sind.

![Der Preis Rang ist für jeden zurückgegebenen Datensatz enthalten.](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Abbildung 5**: der Preis Rang ist für jeden zurückgegebenen Datensatz enthalten.

> [!NOTE]
> `ROW_NUMBER()` ist nur eine der vielen neuen Rang Folge Funktionen, die in SQL Server 2005 verfügbar sind. Eine ausführlichere Erläuterung der `ROW_NUMBER()`sowie der anderen Rang Folge Funktionen finden Sie unter Zurückgeben von [Rang Ergebnissen mit Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).

Beim Sortieren der Ergebnisse anhand der angegebenen `ORDER BY` Spalte in der `OVER`-Klausel (`UnitPrice`im obigen Beispiel) müssen SQL Server die Ergebnisse sortieren. Dies ist ein schneller Vorgang, wenn ein gruppierter Index für die Spalten vorhanden ist, nach denen die Ergebnisse geordnet werden, oder wenn es einen Abdeck enden Index gibt, der andernfalls kostengünstiger sein kann. Um die Leistung für ausreichend große Abfragen zu verbessern, sollten Sie einen nicht gruppierten Index für die Spalte hinzufügen, nach der die Ergebnisse geordnet sind. Eine ausführlichere Betrachtung der Leistungs Überlegungen finden Sie unter [Rang Folge Funktionen und Leistung in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) .

Die von `ROW_NUMBER()` zurückgegebenen Rang Folge Informationen können nicht direkt in der `WHERE`-Klausel verwendet werden. Allerdings kann eine abgeleitete Tabelle verwendet werden, um das `ROW_NUMBER()` Ergebnis zurückzugeben, das dann in der `WHERE`-Klausel angezeigt werden kann. Beispielsweise wird in der folgenden Abfrage eine abgeleitete Tabelle verwendet, um die Spalten ProductName und UnitPrice zusammen mit dem `ROW_NUMBER()` Ergebnis zurückzugeben, und anschließend wird eine `WHERE` Klausel verwendet, um nur die Produkte zurückzugeben, deren Preis Rang zwischen 11 und 20 liegt:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Um dieses Konzept etwas weiter zu erweitern, können wir diesen Ansatz verwenden, um eine bestimmte Datenseite mithilfe der gewünschten Werte für Start Zeilen Index und maximale Zeilen Anzahl abzurufen:

[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Wie wir später in diesem Tutorial sehen werden, wird der von ObjectDataSource bereitgestellte *`StartRowIndex`* ab Null indiziert, wohingegen der von SQL Server 2005 zurückgegebene `ROW_NUMBER()` Wert ab 1 indiziert wird. Daher gibt die `WHERE`-Klausel die Datensätze zurück, bei denen `PriceRank` streng größer als *`StartRowIndex`* und kleiner als oder gleich *`StartRowIndex`*  +  *`MaximumRows`* ist.

Nun haben wir erläutert, wie `ROW_NUMBER()` verwendet werden kann, um eine bestimmte Datenseite mithilfe der Werte für Start Zeilen Index und maximale Zeilen Anzahl abzurufen. nun müssen wir diese Logik als Methoden in Dal und BLL implementieren.

Beim Erstellen dieser Abfrage müssen wir die Reihenfolge festlegen, nach der die Ergebnisse sortiert werden. Sortieren Sie die Produkte nach Ihrem Namen in alphabetischer Reihenfolge. Dies bedeutet, dass wir mit der Implementierung des benutzerdefinierten Pagings in diesem Tutorial keinen benutzerdefinierten Auslagerungs Bericht erstellen können, der auch sortiert werden kann. Im nächsten Tutorial erfahren Sie jedoch, wie diese Funktionen bereitgestellt werden können.

Im vorherigen Abschnitt haben wir die DAL-Methode als Ad-hoc-SQL-Anweisung erstellt. Leider funktioniert der t-SQL-Parser in Visual Studio, der vom TableAdapter-Assistenten verwendet wird, nicht mit der `OVER` Syntax, die von der `ROW_NUMBER()`-Funktion verwendet wird. Daher müssen wir diese dal-Methode als gespeicherte Prozedur erstellen. Wählen Sie im Menü Ansicht die Server-Explorer aus (oder drücken Sie STRG + ALT + S), und erweitern Sie den Knoten `NORTHWND.MDF`. Um eine neue gespeicherte Prozedur hinzuzufügen, klicken Sie mit der rechten Maustaste auf den Knoten gespeicherte Prozeduren, und wählen Sie neue gespeicherte Prozedur hinzufügen aus (siehe Abbildung 6).

![Fügen Sie eine neue gespeicherte Prozedur für das Paging durch die Produkte hinzu.](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Abbildung 6**: Hinzufügen einer neuen gespeicherten Prozedur für das Paging durch die Produkte

Diese gespeicherte Prozedur sollte zwei ganzzahlige Eingabeparameter akzeptieren: `@startRowIndex` und `@maximumRows` und die `ROW_NUMBER()` Funktion verwenden, geordnet nach dem `ProductName` Feld, wobei nur die Zeilen zurückgegeben werden, die größer sind als der angegebene `@startRowIndex` und kleiner oder gleich `@startRowIndex` + `@maximumRow`. Geben Sie das folgende Skript in die neue gespeicherte Prozedur ein, und klicken Sie dann auf das Symbol speichern, um die gespeicherte Prozedur der Datenbank hinzuzufügen.

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Nehmen Sie sich nach dem Erstellen der gespeicherten Prozedur einen Moment Zeit, um Sie zu testen. Klicken Sie mit der rechten Maustaste auf den Namen der gespeicherten Prozedur `GetProductsPaged` im Server-Explorer, und wählen Sie die Option Ausführen aus. Anschließend werden Sie von Visual Studio zur Eingabe von Eingabe Parametern, `@startRowIndex` und `@maximumRow` s aufgefordert (siehe Abbildung 7). Testen Sie verschiedene Werte, und überprüfen Sie die Ergebnisse.

![Geben Sie einen Wert für die Parameter @startRowIndex und @maximumRows ein.](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>Abbildung 7</strong>: Eingeben eines Werts für die Parameter "@startRowIndex" und "@maximumRows"

Nachdem Sie diese Eingabeparameter Werte ausgewählt haben, zeigt das Ausgabefenster die Ergebnisse an. Abbildung 8 zeigt die Ergebnisse bei der Übergabe von 10 für die Parameter `@startRowIndex` und `@maximumRows`.

[Es werden ![die Datensätze zurückgegeben, die auf der zweiten Seite der Daten angezeigt werden.](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Abbildung 8**: die Datensätze, die auf der zweiten Seite der Daten angezeigt werden, werden zurückgegeben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))

Nachdem Sie diese gespeicherte Prozedur erstellt haben, können Sie die `ProductsTableAdapter`-Methode erstellen. Öffnen Sie das `Northwind.xsd` typisierte DataSet, klicken Sie mit der rechten Maustaste in den `ProductsTableAdapter`, und wählen Sie die Option Abfrage hinzufügen aus. Anstatt die Abfrage mit einer Ad-hoc-SQL-Anweisung zu erstellen, erstellen Sie Sie mithilfe einer vorhandenen gespeicherten Prozedur.

![Erstellen der dal-Methode mithilfe einer vorhandenen gespeicherten Prozedur](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Abbildung 9**: Erstellen der dal-Methode mithilfe einer vorhandenen gespeicherten Prozedur

Als nächstes werden wir aufgefordert, die aufzurufende gespeicherte Prozedur auszuwählen. Wählen Sie in der Dropdown Liste die gespeicherte Prozedur `GetProductsPaged` aus.

![Wählen Sie in der Dropdown Liste die gespeicherte Prozedur getproductspaged aus.](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Abbildung 10**: Auswählen der gespeicherten Prozedur "getproductspaged" aus der Dropdown Liste

Im nächsten Bildschirm werden Sie gefragt, welche Art von Daten von der gespeicherten Prozedur zurückgegeben wird: tabellarische Daten, ein einzelner Wert oder kein Wert. Da die gespeicherte Prozedur `GetProductsPaged` mehrere Datensätze zurückgeben kann, geben Sie an, dass Tabellendaten zurückgegeben werden.

![Gibt an, dass die gespeicherte Prozedur Tabellendaten zurückgibt.](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Abbildung 11**: angeben, dass die gespeicherte Prozedur Tabellendaten zurückgibt

Geben Sie abschließend die Namen der Methoden an, die Sie erstellen möchten. Wie bei den vorherigen Tutorials können Sie auch Methoden erstellen, indem Sie die Datentabelle ausfüllen verwenden und eine Datentabelle zurückgeben. Benennen Sie die erste Methode `FillPaged` und die zweite `GetProductsPaged`.

![Benennen Sie die Methoden fillpout und getproductspaged.](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Abbildung 12**: Benennen der Methoden "fillpout" und "getproductspaged"

Zusätzlich zum Erstellen einer dal-Methode, um eine bestimmte Produktseite zurückzugeben, müssen wir diese Funktionalität auch in der BLL bereitstellen. Wie die DAL-Methode muss die getproductspaged-Methode der BLL s zwei ganzzahlige Eingaben zum Angeben des Start Zeilen Indexes und der maximalen Zeilen akzeptieren und nur die Datensätze zurückgeben, die in den angegebenen Bereich fallen. Erstellen Sie eine solche BLL-Methode in der productbll-Klasse, die lediglich die getproductspaged-Methode von Dal s aufruft, wie im folgenden Beispiel:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

Sie können einen beliebigen Namen für die Eingabeparameter der BLL-Methode verwenden, aber wie wir in Kürze sehen werden, werden wir uns für die Verwendung von `startRowIndex` und `maximumRows` bei der Konfiguration einer ObjectDataSource für die Verwendung dieser Methode von einem zusätzlichen Arbeitsaufwand ersparen.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Schritt 4: Konfigurieren von ObjectDataSource für die Verwendung von benutzerdefiniertem Paging

Wenn die BLL-und Dal-Methoden für den Zugriff auf eine bestimmte Teilmenge von Datensätzen abgeschlossen sind, können wir ein GridView-Steuerelement erstellen, das die zugrunde liegenden Datensätze mithilfe von benutzerdefiniertem Paging durchläuft. Öffnen Sie zunächst die Seite `EfficientPaging.aspx` im Ordner `PagingAndSorting`, fügen Sie der Seite eine GridView hinzu, und konfigurieren Sie Sie für die Verwendung eines neuen ObjectDataSource-Steuer Elements. In unseren vorherigen Tutorials haben wir die ObjectDataSource häufig so konfiguriert, dass Sie die `ProductsBLL` Class s `GetProducts`-Methode verwendet. Diesmal möchten wir jedoch die `GetProductsPaged`-Methode verwenden, da die `GetProducts`-Methode *alle* Produkte in der Datenbank zurückgibt, während `GetProductsPaged` nur eine bestimmte Teilmenge von Datensätzen zurückgibt.

![Konfigurieren von ObjectDataSource für die Verwendung der getproductspaged-Methode der productbll-Klasse](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Abbildung 13**: Konfigurieren von ObjectDataSource für die Verwendung der getproductspaged-Methode der productbll-Klasse

Da wir eine schreibgeschützte GridView-Sicht neu erstellen, nehmen Sie sich einen Moment Zeit, um die Dropdown Liste der Methode auf den Registerkarten einfügen, aktualisieren und löschen auf (keine) festzulegen.

Im nächsten Schritt werden Sie vom Assistenten für ObjectDataSource zur Eingabe der Quellen der `GetProductsPaged` Methode s `startRowIndex` und `maximumRows` Eingabeparameter Werten aufgefordert. Diese Eingabeparameter werden tatsächlich automatisch von der GridView festgelegt. lassen Sie die Quelle also auf keine festgelegt, und klicken Sie auf Fertigstellen.

![Belassen Sie die Eingabe Parameter Quellen als "None".](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Abbildung 14**: belassen der Eingabe Parameter Quellen als "None"

Nachdem Sie den ObjectDataSource-Assistenten abgeschlossen haben, enthält das GridView-Objekt ein BoundField-oder CheckBoxField-Objekt für jedes der Product Data-Felder. Sie können die Darstellung des GridView-s beliebig anpassen. Ich habe entschieden, nur die `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`und `UnitPrice` boundfields anzuzeigen. Konfigurieren Sie außerdem die GridView so, dass Paging unterstützt wird, indem Sie das Kontrollkästchen Paging aktivieren im Smarttags aktivieren. Nach diesen Änderungen sollten das deklarative Markup View-und ObjectDataSource-Markup in etwa wie folgt aussehen:

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

Wenn Sie die Seite jedoch über einen Browser besuchen, ist die GridView nicht gefunden.

![Die GridView wird nicht angezeigt.](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Abbildung 15**: die GridView wird nicht angezeigt

Die GridView fehlt, weil ObjectDataSource aktuell 0 als Werte für die `GetProductsPaged` `startRowIndex`-und `maximumRows`-Eingabeparameter verwendet. Daher gibt die resultierende SQL-Abfrage keine Datensätze zurück, weshalb die GridView nicht angezeigt wird.

Um dieses Problem zu beheben, müssen wir ObjectDataSource für die Verwendung von benutzerdefiniertem Paging konfigurieren. Dies kann in den folgenden Schritten erreicht werden:

1. **Legen Sie die Eigenschaft ObjectDataSource s `EnablePaging` auf fest `true`** dies der ObjectDataSource angibt, dass Sie an die `SelectMethod` zwei zusätzlichen Parametern übergeben werden muss: eine, um den Start Zeilen Index ([`StartRowIndexParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) anzugeben, und eine, um die maximale Anzahl von Zeilen ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)) anzugeben.
2. **Legen Sie die Eigenschaften ObjectDataSource s `StartRowIndexParameterName` und `MaximumRowsParameterName` entsprechend** die Eigenschaften `StartRowIndexParameterName` und `MaximumRowsParameterName` die Namen der Eingabeparameter an, die für benutzerdefinierte Paginierung an die `SelectMethod` übergeben werden. Standardmäßig sind diese Parameternamen `startIndexRow` und `maximumRows`. aus diesem Grund wurden beim Erstellen der `GetProductsPaged`-Methode in der BLL diese Werte für die Eingabeparameter verwendet. Wenn Sie andere Parameternamen für die BLL s-`GetProductsPaged` Methode verwenden möchten, z. b. `startIndex` und `maxRows`, müssen Sie z. b. die Eigenschaften von ObjectDataSource s `StartRowIndexParameterName` und `MaximumRowsParameterName` entsprechend festlegen (z. b. startIndex für `StartRowIndexParameterName` und MaxRows für `MaximumRowsParameterName`).
3. **Legen Sie die [Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) ObjectDataSource s`SelectCountMethod` auf den Namen der Methode fest, die die Gesamtanzahl der Datensätze zurückgibt, die durchlaufen werden (`TotalNumberOfProducts`)** , dass die `ProductsBLL` Class s `TotalNumberOfProducts`-Methode die Gesamtzahl der Datensätze zurückgibt, die mithilfe einer dal-Methode, die eine `SELECT COUNT(*) FROM Products` Abfrage ausführt, durchlaufen werden. Diese Informationen werden von ObjectDataSource benötigt, um die Paging-Schnittstelle ordnungsgemäß zu erzeugen.
4. **Entfernen Sie die `startRowIndex`-und `maximumRows` `<asp:Parameter>` Elemente aus dem deklarativen Markup von ObjectDataSource s** , wenn Sie ObjectDataSource mithilfe des Assistenten konfigurieren. Visual Studio hat automatisch zwei `<asp:Parameter>` Elemente für die Eingabeparameter der `GetProductsPaged`-Methode hinzugefügt. Wenn Sie `EnablePaging` auf `true`festlegen, werden diese Parameter automatisch übermittelt. Wenn Sie auch in der deklarativen Syntax angezeigt werden, versucht ObjectDataSource, *vier* Parameter an die `GetProductsPaged`-Methode und zwei Parameter an die `TotalNumberOfProducts`-Methode zu übergeben. Wenn Sie vergessen, diese `<asp:Parameter>` Elemente zu entfernen, erhalten Sie beim Aufrufen der Seite über einen Browser eine Fehlermeldung wie die folgende: *ObjectDataSource "ObjectDataSource1" konnte keine nicht generische Methode "totalmaximiofproducts" finden, die Parameter hat: StartRowIndex, MaximumRows*.

Nachdem Sie diese Änderungen vorgenommen haben, sollte die deklarative Syntax von ObjectDataSource s wie folgt aussehen:

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Beachten Sie, dass die Eigenschaften "`EnablePaging`" und "`SelectCountMethod`" festgelegt wurden und die `<asp:Parameter>` Elemente entfernt wurden. Abbildung 16 zeigt einen Screenshot der Eigenschaftenfenster, nachdem diese Änderungen vorgenommen wurden.

![Konfigurieren Sie zum Verwenden von benutzerdefiniertem Paging das ObjectDataSource-Steuerelement.](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Abbildung 16**: Konfigurieren des ObjectDataSource-Steuer Elements für die Verwendung von benutzerdefiniertem Paging

Nachdem Sie diese Änderungen vorgenommen haben, besuchen Sie diese Seite über einen Browser. Es sollten 10 Produkte aufgelistet werden, die alphabetisch sortiert sind. Nehmen Sie sich einen Moment Zeit, um die Datenseiten Weise zu durchlaufen. Es gibt zwar keinen visuellen Unterschied zwischen der Perspektive des Endbenutzers zwischen Standard-Paging und benutzerdefiniertem Paging, das benutzerdefinierte Paging wird jedoch effizienter durch große Datenmengen abgerufen, da nur die Datensätze abgerufen werden, die für eine bestimmte Seite angezeigt werden müssen.

[![die Daten nach dem Namen des Produkts geordnet, werden mithilfe von benutzerdefiniertem Paging ausgelagert.](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Abbildung 17**: die Daten, geordnet nach dem Namen des Produkts, werden mithilfe von benutzerdefiniertem Paging ausgelagert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))

> [!NOTE]
> Bei benutzerdefiniertem Paging wird der von der ObjectDataSource s-`SelectCountMethod` zurückgegebene Wert der Seitenanzahl im GridView s-Ansichts Zustand gespeichert. Andere GridView-Variablen die `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` Auflistung usw. werden im Steuerelement *Zustand*gespeichert, der unabhängig vom Wert der Eigenschaft "GridView s `EnableViewState`" beibehalten wird. Da der `PageCount` Wert über Postbacks hinweg mit dem Ansichts Zustand beibehalten wird, ist es zwingend erforderlich, dass der GridView s-Ansichts Zustand aktiviert ist, wenn Sie eine Paging-Schnittstelle verwenden, die einen Link zur letzten Seite enthält. (Wenn die pagingschnittstelle keinen direkten Link zur letzten Seite enthält, können Sie den Ansichts Zustand deaktivieren.)

Wenn Sie auf den Link letzte Seite klicken, wird ein Postback ausgelöst, und die GridView wird angewiesen, die `PageIndex`-Eigenschaft zu aktualisieren. Wenn auf den letzten Seiten Link geklickt wird, weist das GridView-Objekt seine `PageIndex`-Eigenschaft einem Wert zu, der kleiner als seine `PageCount`-Eigenschaft ist. Wenn der Ansichts Zustand deaktiviert ist, geht der `PageCount` Wert über Postbacks verloren, und dem `PageIndex` wird stattdessen der maximale ganzzahlige Wert zugewiesen. Als nächstes versucht die GridView, den Zeilen Index zu ermitteln, indem die Eigenschaften `PageSize` und `PageCount` multipliziert werden. Dies führt zu einer `OverflowException`, da das Produkt die maximal zulässige ganzzahlige Größe überschreitet.

## <a name="implement-custom-paging-and-sorting"></a>Implementieren von benutzerdefiniertem Paging und Sortieren

Die aktuelle Implementierung der benutzerdefinierten Paginierung erfordert, dass die Reihenfolge, in der die Daten durchlaufen werden, bei der Erstellung der gespeicherten Prozedur `GetProductsPaged` statisch angegeben wird. Möglicherweise haben Sie jedoch bemerkt, dass das GridView s-Smarttag neben der Option Paging aktivieren auch das Kontrollkästchen Sortierung aktivieren enthält. Das Hinzufügen von Sortierungs Unterstützung zu GridView mit unserer aktuellen benutzerdefinierten pagingimplementierung wird jedoch nur die Datensätze auf der aktuell angezeigten Datenseite sortieren. Wenn Sie z. b. die GridView so konfigurieren, dass auch Paging unterstützt wird, und dann beim Anzeigen der ersten Seite der Daten in absteigender Reihenfolge nach Produktname sortieren, wird die Reihenfolge der Produkte auf Seite 1 umgekehrt. Wie in Abbildung 18 gezeigt, zeigt Carnarvon Tigers als erstes Produkt, wenn die Sortierung in umgekehrter alphabetischer Reihenfolge erfolgt, die die 71 anderen Produkte ignoriert, die in alphabetischer Reihenfolge nach Carnarvon Tigers stehen. nur die Datensätze auf der ersten Seite werden in der Sortierung berücksichtigt.

[nur ![die auf der aktuellen Seite angezeigten Daten sortiert werden.](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Abbildung 18**: nur die auf der aktuellen Seite angezeigten Daten werden sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))

Die Sortierung gilt nur für die aktuelle Datenseite, da die Sortierung erfolgt, nachdem die Daten aus der BLL s-`GetProductsPaged`-Methode abgerufen wurden, und diese Methode gibt nur die Datensätze für die jeweilige Seite zurück. Um die Sortierung ordnungsgemäß zu implementieren, müssen wir den Sortier Ausdruck an die `GetProductsPaged`-Methode übergeben, damit die Daten entsprechend sortiert werden können, bevor die jeweilige Datenseite zurückgegeben wird. Im nächsten Tutorial erfahren Sie, wie Sie dies erreichen.

## <a name="implementing-custom-paging-and-deleting"></a>Implementieren von benutzerdefiniertem Paging und löschen

Wenn Sie das Löschen von Funktionen in einer GridView aktivieren, deren Daten mithilfe von benutzerdefinierten pagingverfahren ausgelagert werden, werden Sie feststellen, dass beim Löschen des letzten Datensatzes aus der letzten Seite die GridView nicht mehr ordnungsgemäß herabgesetzt wird, anstatt die `PageIndex`der GridView zu verringern. Zum Reproduzieren dieses Fehlers aktivieren Sie das Löschen für das soeben erstellte Tutorial. Wechseln Sie zur letzten Seite (Seite 9), auf der ein einzelnes Produkt angezeigt werden soll, da wir das Paging über 81 Produkte, 10 Produkte auf einmal durchlaufen. Löschen Sie dieses Produkt.

Nach dem Löschen des letzten Produkts *sollte* die GridView automatisch zur achten Seite wechseln, und diese Funktionalität wird mit Standard Auslagerung angezeigt. Mit benutzerdefiniertem Paging wird jedoch nach dem Löschen des letzten Produkts auf der letzten Seite das GridView-Fenster ganz einfach auf dem Bildschirm ausgeblendet. Der genaue Grund dafür ist, dass dieser Vorgang etwas über den *Rahmen dieses Tutorials* hinausgeht. Weitere Informationen finden Sie unter [Löschen des letzten Datensatzes auf der letzten Seite aus einer GridView mit benutzerdefiniertem Paging](http://scottonwriting.net/sowblog/posts/7326.aspx) für die Details auf niedriger Ebene. In der Zusammenfassung werden die folgenden Schritte ausgeführt, die von der GridView ausgeführt werden, wenn auf die Schaltfläche "Löschen" geklickt wird:

1. Löschen des Datensatzes
2. Hiermit werden die entsprechenden Datensätze zum Anzeigen der angegebenen `PageIndex` und `PageSize`
3. Stellen Sie sicher, dass die `PageIndex` die Anzahl der Datenseiten in der Datenquelle nicht überschreitet. Wenn dies der Fall ist, wird die `PageIndex` Eigenschaft GridView s automatisch Dekrement
4. Binden der entsprechenden Datenseite an die GridView mithilfe der in Schritt 2 erhaltenen Datensätze

Das Problem ist darauf zurückzuführen, dass in Schritt 2 die `PageIndex`, die beim Abrufen der anzuzeigenden Datensätze verwendet werden, nach wie vor die `PageIndex` der letzten Seite ist, deren einziger Datensatz soeben gelöscht wurde. Aus diesem Grund werden in Schritt 2 *keine* Datensätze zurückgegeben, da die letzte Seite der Daten keine Datensätze mehr enthält. In Schritt 3 erkennt die GridView, dass die `PageIndex`-Eigenschaft größer ist als die Gesamtanzahl der Seiten in der Datenquelle (seit dem Löschen des letzten Datensatzes auf der letzten Seite) und verringert daher seine `PageIndex`-Eigenschaft. In Schritt 4 versucht die GridView, sich an die in Schritt 2 abgerufenen Daten zu binden. in Schritt 2 wurden jedoch keine Datensätze zurückgegeben, was zu einer leeren GridView führte. Beim standardmäßigen Paging wird dieses Problem nicht in der Oberfläche gelöst, da in Schritt 2 *alle* Datensätze aus der Datenquelle abgerufen werden.

Um dieses Problem zu beheben, haben wir zwei Möglichkeiten. Der erste besteht darin, einen Ereignishandler für den GridView s `RowDeleted`-Ereignishandler zu erstellen, der bestimmt, wie viele Datensätze auf der Seite angezeigt wurden, die soeben gelöscht wurde. Wenn nur ein Datensatz vorhanden ist, muss der soeben gelöschte Datensatz der letzte Datensatz sein, und die GridView s-`PageIndex`muss verringert werden. Natürlich möchten wir nur die `PageIndex` aktualisieren, wenn der Löschvorgang tatsächlich erfolgreich war. Dies kann durch sicherstellen sichergestellt werden, dass die Eigenschaft `e.Exception` `null`ist.

Diese Vorgehensweise funktioniert, weil Sie die `PageIndex` nach Schritt 1, aber vor Schritt 2 aktualisiert. Daher wird in Schritt 2 der entsprechende Satz von Datensätzen zurückgegeben. Verwenden Sie hierzu Code wie den folgenden:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Eine Alternative Problem Umgehung besteht darin, einen Ereignishandler für das Ereignis "ObjectDataSource s `RowDeleted`" zu erstellen und die `AffectedRows`-Eigenschaft auf den Wert "1" festzulegen. Nach dem Löschen des Datensatzes in Schritt 1 (aber vor dem erneuten Abrufen der Daten in Schritt 2) aktualisiert die GridView die `PageIndex`-Eigenschaft, wenn eine oder mehrere Zeilen vom Vorgang betroffen sind. Die `AffectedRows`-Eigenschaft wird jedoch nicht von ObjectDataSource festgelegt, weshalb dieser Schritt ausgelassen wird. Eine Möglichkeit, diesen Schritt auszuführen, besteht darin, die `AffectedRows`-Eigenschaft manuell festzulegen, wenn der Löschvorgang erfolgreich abgeschlossen wurde. Dies kann mithilfe von Code wie dem folgenden erreicht werden:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Der Code für beide Ereignishandler finden Sie in der Code Behind-Klasse des `EfficientPaging.aspx` Beispiels.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Vergleichen der Leistung von Default und Custom Paging

Da das benutzerdefinierte Paging nur die benötigten Datensätze abruft, während die Standard Paginierung *alle* Datensätze für jede angezeigte Seite zurückgibt, ist es klar, dass das benutzerdefinierte Paging effizienter ist als das Standardpaging. Aber wie viel effizienter ist das benutzerdefinierte Paging? Welche Art von Leistungssteigerungen können Sie anzeigen, indem Sie von der Standard Auslagerung zu benutzerdefiniertem Paging wechseln?

Leider gibt es hier keine Antwort. Der Leistungsgewinn hängt von einer Reihe von Faktoren ab. die wichtigsten zwei sind die Anzahl der Datensätze, die durchlaufen werden, sowie die Last auf dem Datenbankserver und die Kommunikationskanäle zwischen dem Webserver und dem Datenbankserver. Bei kleinen Tabellen mit nur wenigen Dutzend Datensätzen kann der Leistungsunterschied vernachlässigbar sein. Bei großen Tabellen mit Tausenden bis Hunderttausenden von Zeilen ist der Leistungsunterschied jedoch akut.

Ein Artikel von My, [Custom Paging in ASP.NET 2,0 mit SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), enthält einige Leistungstests, die ich ausgeführt habe, um die Unterschiede in der Leistung zwischen diesen beiden Paging-Techniken beim Paging durch eine Datenbanktabelle mit 50.000 Datensätzen zu präsentieren. In diesen Tests habe ich die Zeit für die Ausführung der Abfrage auf SQL Server Ebene (mithilfe von [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) und auf der ASP.NET-Seite unter Verwendung von [ASP.net s-Ablauf Verfolgungs Funktionen](https://msdn.microsoft.com/library/y13fw6we.aspx)überprüft. Beachten Sie, dass diese Tests in meinem Entwicklungsfeld mit einem einzelnen aktiven Benutzer ausgeführt wurden und daher nicht sicher sind und keine typischen Website-Auslastungs Muster imitieren. Unabhängig davon veranschaulichen die Ergebnisse die relativen Unterschiede in der Ausführungszeit für das standardmäßige und benutzerdefinierte Paging bei der Arbeit mit ausreichend großen Datenmengen.

|  | **Durchschn. Dauer (Sek.)** | **Reads** |
| --- | --- | --- |
| **Standard-Paging-SQL-Profiler** | 1,411 | 383 |
| **Benutzerdefiniertes Paging von SQL Profiler** | 0,002 | 29 |
| **Standard-Paging ASP.net Ablauf Verfolgung** | 2,379 | *nicht zutreffend* |
| **Benutzerdefiniertes Paging ASP.net Ablauf Verfolgung** | 0,029 | *nicht zutreffend* |

Wie Sie sehen können, erforderte das Abrufen einer bestimmten Datenseite im Durchschnitt 354 weniger Lesevorgänge und wurde in einem Bruchteil der Zeit abgeschlossen. Auf der Seite "ASP.net" konnte die Seite in der Nähe des standardmäßigen Auslagerungs Zeitraums (<sup>in der 1/100</sup> Nähe des standardmäßigen Paging) wieder hergestellt werden. Weitere Informationen zu diesen Ergebnissen sowie Code und eine Datenbank, die Sie herunterladen können, um diese Tests in ihrer eigenen Umgebung zu reproduzieren, finden Sie in diesem [Artikel](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) .

## <a name="summary"></a>Summary

Standard-Paging ist ein geliefert zur Implementierung, aktivieren Sie das Kontrollkästchen Paging aktivieren im Smarttags des dateiweb-Steuer Elements, aber eine solche Einfachheit ergibt sich aus den Kosten der Leistung. Beim standardmäßigen Paging werden *alle* Datensätze zurückgegeben, wenn ein Benutzer eine beliebige Seite mit Daten anfordert, obwohl möglicherweise nur ein kleiner Bruchteil davon angezeigt wird. Zur Bekämpfung dieses Leistungs Aufwands bietet ObjectDataSource eine Alternative Paging-Option für benutzerdefiniertes Paging.

Während das benutzerdefinierte Paging bei der standardmäßigen Auslagerung von Leistungsproblemen verbessern kann, indem nur die Datensätze abgerufen werden, die angezeigt werden müssen, ist die Implementierung von benutzerdefiniertem Paging mehr beteiligt. Zuerst muss eine Abfrage geschrieben werden, die ordnungsgemäß (und effizient) auf die bestimmte Teilmenge der angeforderten Datensätze zugreift. Dies kann auf verschiedene Arten erreicht werden: das, das wir in diesem Tutorial untersucht haben, besteht darin, die neuen `ROW_NUMBER()` Funktion SQL Server 2005 s zu verwenden, um Ergebnisse zu ordnen und dann nur die Ergebnisse zurückzugeben, deren Rangfolge innerhalb eines angegebenen Bereichs liegt. Außerdem müssen wir eine Möglichkeit hinzufügen, um die Gesamtanzahl der Datensätze zu ermitteln, die durchlaufen werden. Nachdem Sie diese dal-und BLL-Methoden erstellt haben, müssen wir auch die ObjectDataSource so konfigurieren, dass Sie bestimmen kann, wie viele Datensätze durchlaufen werden, und die Werte für Start Zeilen Index und maximale Zeilen Menge an die BLL übergeben können.

Die Implementierung von benutzerdefiniertem Paging erfordert zwar eine Reihe von Schritten und ist nicht so einfach wie das standardmäßige Paging, das benutzerdefinierte Paging ist jedoch beim Paging durch ausreichend große Datenmengen notwendig. Wenn die untersuchten Ergebnisse angezeigt werden, kann das benutzerdefinierte Paging Sekunden aus der ASP.NET Seiten-Rendering-Zeit verringern und die Last auf dem Datenbankserver um eine oder mehrere Größenordnungen erhöhen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](paging-and-sorting-report-data-vb.md)
> [Weiter](sorting-custom-paged-data-vb.md)
