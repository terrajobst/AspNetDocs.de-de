---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Konfigurieren der Einstellungen für die Verbindungs-und Befehls Ebene der Datenzugriffs Ebene (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Die TableAdapters in einem typisierten DataSet sorgen automatisch dafür, dass Sie eine Verbindung mit der Datenbank herstellen, Befehle ausgeben und eine Datentabelle mit den Ergebnissen auffüllen...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: fa2868fc0dd8acd76f600b47d92adb984ce8d105
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78444273"
---
# <a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Konfigurieren von Einstellungen der Datenzugriffsschicht auf Verbindungs- und Befehlsebene (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) oder [PDF herunterladen](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> Die TableAdapters in einem typisierten DataSet sorgen automatisch dafür, dass Sie eine Verbindung mit der Datenbank herstellen, Befehle ausgeben und eine Datentabelle mit den Ergebnissen auffüllen. Es gibt jedoch Fälle, in denen wir diese Details selbst berücksichtigen möchten, und in diesem Tutorial erfahren Sie, wie Sie im TableAdapter auf die Einstellungen für die Daten Bankverbindung und die Befehls Ebene zugreifen.

## <a name="introduction"></a>Einführung

In der tutorialreihe haben wir typisierte Datasets verwendet, um die Datenzugriffs Ebene und die Geschäftsobjekte unserer geschichteten Architektur zu implementieren. Wie im [ersten Tutorial](../introduction/creating-a-data-access-layer-vb.md)erläutert, dienen die DataTables der typisierten Datasets als Datenblatt Daten, während die TableAdapters als Wrapper fungieren, um mit der Datenbank zu kommunizieren, um die zugrunde liegenden Daten abzurufen und zu ändern. Die TableAdapters Kapseln die Komplexität, die bei der Arbeit mit der Datenbank beteiligt ist, und es ist nicht erforderlich, Code zu schreiben, um eine Verbindung mit der Datenbank herzustellen, einen Befehl auszugeben oder die Ergebnisse in eine Datentabelle aufzufüllen.

Es gibt jedoch Zeiten, in denen wir uns in die Tiefen des TableAdapter einreihen und Code schreiben müssen, der direkt mit den ADO.NET-Objekten funktioniert. Im Tutorial zum [Umpacken von Daten Bank Änderungen in einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) haben wir z. b. Methoden zum Starten, zum Commit und zum Rollback von ADO.NET-Transaktionen zum TableAdapter hinzugefügt. Diese Methoden haben ein internes, manuell erstelltes `SqlTransaction` Objekt verwendet, das den TableAdapter s `SqlCommand`-Objekten zugewiesen wurde.

In diesem Tutorial wird erläutert, wie Sie im TableAdapter auf die Einstellungen für die Daten Bankverbindung und auf Befehls Ebene zugreifen. Insbesondere werden dem `ProductsTableAdapter` Funktionen hinzugefügt, die den Zugriff auf die zugrunde liegende Verbindungs Zeichenfolge und die Befehls Timeout Einstellungen ermöglichen.

## <a name="working-with-data-using-adonet"></a>Arbeiten mit Daten mithilfe von ADO.net

Das Microsoft .NET Framework enthält eine Vielzahl von Klassen, die speziell für die Arbeit mit Daten entworfen wurden. Diese Klassen, die im [`System.Data`-Namespace](https://msdn.microsoft.com/library/system.data.aspx)gefunden werden, werden als *ADO.net* -Klassen bezeichnet. Einige der Klassen im ADO.net-Dach sind an einen bestimmten *Datenanbieter*gebunden. Sie können sich einen Datenanbieter als Kommunikationskanal vorstellen, der den Datenfluss zwischen den ADO.NET-Klassen und dem zugrunde liegenden Datenspeicher ermöglicht. Es gibt generalisierte Anbieter, wie z. b. OleDb und ODBC, sowie Anbieter, die speziell für ein bestimmtes Datenbanksystem entwickelt wurden. Beispielsweise ist es möglich, eine Verbindung mit einer Microsoft SQL Server Datenbank mithilfe des OLE DB-Anbieters herzustellen, aber der SqlClient-Anbieter ist viel effizienter, da er speziell für SQL Server entworfen und optimiert wurde.

Beim programmgesteuerten Zugriff auf Daten wird das folgende Muster häufig verwendet:

1. Stellen Sie eine Verbindung mit der Datenbank her.
2. Geben Sie einen Befehl aus.
3. Arbeiten Sie für `SELECT` Abfragen mit den resultierenden Datensätzen.

Es gibt separate ADO.NET-Klassen, mit denen Sie die einzelnen Schritte ausführen können. Wenn Sie z. b. mithilfe des SqlClient-Anbieters eine Verbindung mit einer Datenbank herstellen möchten, verwenden Sie die [`SqlConnection`-Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Verwenden Sie die [`SqlCommand`-Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx), um einen `INSERT`, `UPDATE`, `DELETE`oder `SELECT` Befehl an die Datenbank auszugeben.

Mit Ausnahme des Lernprogramms zum umschließen [von Daten Bank Änderungen in einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) mussten wir keinen ADO.NET-Code auf niedriger Ebene schreiben, da der automatisch generierte TableAdapters-Code die Funktionen enthält, die zum Herstellen einer Verbindung mit der Datenbank, ausgeben von Befehlen, Abrufen von Daten und Auffüllen dieser Daten in DataTables benötigt werden. Es kann jedoch vorkommen, dass diese Einstellungen auf niedriger Ebene angepasst werden müssen. In den nächsten Schritten wird erläutert, wie Sie auf die ADO.NET-Objekte tippen, die intern von den TableAdapters verwendet werden.

## <a name="step-1-examining-with-the-connection-property"></a>Schritt 1: untersuchen mit der Connection-Eigenschaft

Jede TableAdapter-Klasse verfügt über eine `Connection`-Eigenschaft, die Informationen zur Datenbankverbindung angibt. Der Datentyp der Eigenschaft und der `ConnectionString` Wert werden durch die im TableAdapter-Konfigurations-Assistenten getroffenen Auswahl bestimmt. Beachten Sie, dass der Assistent beim ersten Hinzufügen eines TableAdapter zu einem typisierten DataSet zur Datenbankquelle aufgefordert wird (siehe Abbildung 1). Die Dropdown Liste in diesem ersten Schritt umfasst die Datenbanken, die in der Konfigurationsdatei angegeben sind, sowie alle anderen Datenbanken in den Server-Explorer s-Datenverbindungen. Wenn die zu verwendende Datenbank nicht in der Dropdown Liste vorhanden ist, können Sie eine neue Datenbankverbindung angeben, indem Sie auf die Schaltfläche neue Verbindung klicken und die erforderlichen Verbindungsinformationen bereitstellen.

[![des ersten Schritts des TableAdapter-Konfigurations-Assistenten](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Abbildung 1**: der erste Schritt des TableAdapter-Konfigurations-Assistenten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))

Nehmen Sie sich einen Moment Zeit, um den Code für die TableAdapter s-`Connection`-Eigenschaft zu überprüfen. Wie im Tutorial [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md) erwähnt, können wir den automatisch generierten TableAdapter-Code anzeigen, indem wir zum Fenster "Klassenansicht" navigieren, einen Drilldown auf die entsprechende Klasse ausführen und dann auf den Elementnamen doppelklicken.

Navigieren Sie zum Klassenansicht Fenster, indem Sie zum Menü Ansicht navigieren und Klassenansicht (oder durch Drücken von STRG + UMSCHALT + C) auswählen. Führen Sie in der oberen Hälfte des Klassenansicht Fensters einen Drilldown zum `NorthwindTableAdapters`-Namespace aus, und wählen Sie die `ProductsTableAdapter`-Klasse aus. Dadurch werden die `ProductsTableAdapter` s-Elemente in der unteren Hälfte des Klassenansicht angezeigt, wie in Abbildung 2 dargestellt. Doppelklicken Sie auf die `Connection`-Eigenschaft, um Ihren Code anzuzeigen.

![Doppelklicken Sie im Klassenansicht auf die Verbindungs Eigenschaft, um den automatisch generierten Code anzuzeigen.](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Abbildung 2**: Doppelklicken Sie auf die Connection-Eigenschaft im Klassenansicht, um den automatisch generierten Code anzuzeigen.

Die TableAdapter s `Connection`-Eigenschaft und anderer Verbindungs bezogener Code folgendermaßen:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Wenn die TableAdapter-Klasse instanziiert wird, ist die Element Variable `_connection` gleich `Nothing`. Beim Zugriff auf die `Connection`-Eigenschaft wird zuerst überprüft, ob die `_connection` Member-Variable instanziiert wurde. Wenn dies nicht der Fall ist, wird die `InitConnection`-Methode aufgerufen, die `_connection` instanziiert und die `ConnectionString`-Eigenschaft auf den Wert der Verbindungs Zeichenfolge festlegt, der im ersten Schritt des TableAdapter-Konfigurations-Assistenten angegeben ist.

Die `Connection`-Eigenschaft kann auch einem `SqlConnection`-Objekt zugewiesen werden. Dadurch wird das neue `SqlConnection`-Objekt jedem der TableAdapter s `SqlCommand`-Objekten zugeordnet.

## <a name="step-2-exposing-connection-level-settings"></a>Schritt 2: verfügbar machen von Einstellungen auf Verbindungs Ebene

Die Verbindungsinformationen sollten im TableAdapter gekapselt bleiben und sind nicht für andere Ebenen in der Anwendungsarchitektur zugänglich. Allerdings kann es Szenarien geben, in denen die Informationen auf der Basis der TableAdapter-Informationen auf Verbindungs Ebene für eine Abfrage-, Benutzer-oder ASP.NET-Seite zugänglich sein müssen.

Erweitern Sie die `ProductsTableAdapter` im `Northwind` DataSet so, dass eine `ConnectionString`-Eigenschaft enthalten ist, die von der Geschäftslogik Schicht verwendet werden kann, um die vom TableAdapter verwendete Verbindungs Zeichenfolge zu lesen oder zu ändern.

> [!NOTE]
> Eine *Verbindungs Zeichenfolge* ist eine Zeichenfolge, die Daten bankverbindungs Informationen angibt, wie z. b. den zu verwendenden Anbieter, den Speicherort der Datenbank, Anmelde Informationen für die Authentifizierung und andere datenbankbezogene Einstellungen. Eine Liste der Muster der Verbindungs Zeichenfolge, die von einer Vielzahl von Daten speichern und Anbietern verwendet werden, finden Sie unter [connectionStrings.com](http://www.connectionstrings.com/).

Wie im Tutorial [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md) erläutert, können die automatisch generierten Klassen des typisierten Datasets durch die Verwendung von partiellen Klassen erweitert werden. Erstellen Sie zunächst einen neuen Unterordner in dem Projekt mit dem Namen `ConnectionAndCommandSettings` unterhalb des `~/App_Code/DAL` Ordners.

![Fügen Sie einen Unterordner mit dem Namen connectionandcommandsettings hinzu.](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Abbildung 3**: Hinzufügen eines unter Ordners mit dem Namen `ConnectionAndCommandSettings`

Fügen Sie eine neue Klassendatei namens `ProductsTableAdapter.ConnectionAndCommandSettings.vb` hinzu, und geben Sie den folgenden Code ein:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Diese partielle Klasse fügt der `ProductsTableAdapter`-Klasse eine `Public` Eigenschaft mit dem Namen `ConnectionString` hinzu, die es allen Ebenen ermöglicht, die Verbindungs Zeichenfolge für die zugrunde liegende Verbindung von TableAdapter zu lesen oder zu aktualisieren.

Wenn diese partielle Klasse erstellt (und gespeichert) ist, öffnen Sie die `ProductsBLL`-Klasse. Wechseln Sie zu einer der vorhandenen Methoden, und geben Sie `Adapter` ein, und drücken Sie dann die Punkt Taste, um IntelliSense aufzurufen. Sie sollten sehen, dass die neue `ConnectionString`-Eigenschaft in IntelliSense verfügbar ist, was bedeutet, dass Sie diesen Wert Programm gesteuert aus der BLL lesen oder anpassen können.

## <a name="exposing-the-entire-connection-object"></a>Verfügbar machen des gesamten Verbindungs Objekts

Diese partielle Klasse macht nur eine Eigenschaft des zugrunde liegenden Verbindungs Objekts verfügbar: `ConnectionString`. Wenn Sie das gesamte Verbindungs Objekt über die Grenzen des TableAdapter hinaus verfügbar machen möchten, können Sie alternativ die Schutz Ebene für die `Connection`-Eigenschaft ändern. Der automatisch generierte Code, den wir in Schritt 1 untersucht haben, zeigte, dass die Eigenschaft TableAdapter s `Connection` als `Friend`gekennzeichnet ist, was bedeutet, dass nur Klassen in derselben Assembly darauf zugreifen können. Dies kann jedoch über die Eigenschaft TableAdapter s `ConnectionModifier` geändert werden.

Öffnen Sie das DataSet `Northwind`, klicken Sie im Designer auf die `ProductsTableAdapter`, und navigieren Sie zum Eigenschaftenfenster. Dort sehen Sie, dass die `ConnectionModifier` auf ihren Standardwert festgelegt ist, `Assembly`. Um die `Connection`-Eigenschaft außerhalb der Assembly des typisierten Datasets verfügbar zu machen, ändern Sie die Eigenschaft `ConnectionModifier` in `Public`.

[![die Barrierefreiheits Stufe der Verbindungs Eigenschaft kann über die connectionmodifier-Eigenschaft konfiguriert werden.](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Abbildung 4**: die Barrierefreiheits Ebene der `Connection` Eigenschaft kann über die `ConnectionModifier`-Eigenschaft konfiguriert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))

Speichern Sie das DataSet, und kehren Sie dann zur `ProductsBLL`-Klasse zurück. Wechseln Sie wie zuvor zu einer der vorhandenen Methoden, und geben Sie `Adapter` ein, und drücken Sie dann die Punkt Taste, um IntelliSense aufzurufen. Die Liste sollte eine `Connection`-Eigenschaft enthalten. Dies bedeutet, dass Sie nun Programm gesteuert alle Einstellungen auf Verbindungs Ebene aus der BLL lesen oder zuweisen können.

## <a name="step-3-examining-the-command-related-properties"></a>Schritt 3: Untersuchen der Befehls bezogenen Eigenschaften

Ein TableAdapter besteht aus einer Haupt Abfrage, die standardmäßig automatisch generierte `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen enthält. Diese Abfrage `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen werden im TableAdapter-Code als ADO.NET Data Adapter-Objekt über die `Adapter`-Eigenschaft implementiert. Wie bei der `Connection`-Eigenschaft wird der Datentyp `Adapter` Eigenschaft s vom verwendeten Datenanbieter bestimmt. Da in diesen Tutorials der SqlClient-Anbieter verwendet wird, ist die `Adapter`-Eigenschaft vom Typ [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

Die Eigenschaft TableAdapter s `Adapter` verfügt über drei Eigenschaften vom Typ `SqlCommand`, mit denen die Anweisungen `INSERT`, `UPDATE`und `DELETE` ausgestellt werden:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Ein `SqlCommand`-Objekt ist dafür verantwortlich, eine bestimmte Abfrage an die Datenbank zu senden, und verfügt über Eigenschaften wie: [`CommandText`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), das die auszuführende Ad-hoc-SQL-Anweisung oder gespeicherte Prozedur enthält. und [`Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), bei dem es sich um eine Auflistung von `SqlParameter` Objekten handelt. Wie bereits im Tutorial [Erstellen einer Datenzugriffs Schicht](../introduction/creating-a-data-access-layer-vb.md) erläutert wurde, können diese Befehls Objekte über die Eigenschaftenfenster angepasst werden.

Zusätzlich zu seiner Haupt Abfrage kann der TableAdapter eine Variable Anzahl von Methoden enthalten, die, wenn Sie aufgerufen wird, einen angegebenen Befehl an die Datenbank weitergeben. Das Haupt Befehls Objekt der Abfrage s und die Befehls Objekte für alle zusätzlichen Methoden werden in der Eigenschaft TableAdapter s `CommandCollection` gespeichert.

Nehmen Sie sich einen Moment Zeit, um sich den Code anzusehen, der vom `ProductsTableAdapter` im `Northwind` Dataset für diese beiden Eigenschaften und deren unterstützende Element Variablen und Hilfsmethoden generiert wurde:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Der Code für die `Adapter`-und `CommandCollection` Eigenschaften imitiert genau den Wert der `Connection`-Eigenschaft. Es gibt Element Variablen, die die Objekte enthalten, die von den Eigenschaften verwendet werden. Die Eigenschaften `Get` Accessoren beginnen, indem Sie überprüfen, ob die entsprechende Element Variable `Nothing`ist. Wenn dies der Fall ist, wird eine Initialisierungs Methode aufgerufen, die eine Instanz der Element Variablen erstellt und die Haupt Befehls bezogenen Eigenschaften zuweist.

## <a name="step-4-exposing-command-level-settings"></a>Schritt 4: verfügbar machen von Einstellungen auf Befehls Ebene

Im Idealfall sollten die Informationen auf Befehls Ebene in der Datenzugriffs Ebene gekapselt bleiben. Sollten diese Informationen in anderen Ebenen der Architektur benötigt werden, können Sie jedoch wie bei den Einstellungen auf Verbindungs Ebene über eine partielle Klasse verfügbar gemacht werden.

Da der TableAdapter nur über eine einzige `Connection`-Eigenschaft verfügt, ist der Code zum verfügbar machen von Einstellungen auf Verbindungs Ebene recht unkompliziert. Das Ändern der Einstellungen auf Befehls Ebene ist etwas komplizierter, da der TableAdapter mehrere Befehls Objekte aufweisen kann: eine `InsertCommand`, `UpdateCommand`und `DeleteCommand`sowie eine Variable Anzahl von Befehls Objekten in der `CommandCollection`-Eigenschaft. Beim Aktualisieren der Einstellungen auf Befehls Ebene müssen diese Einstellungen an alle Befehls Objekte weitergegeben werden.

Stellen Sie sich z. b. vor, dass im TableAdapter bestimmte Abfragen vorhanden waren, die eine außergewöhnlich lange Ausführungszeit gedauert haben. Wenn Sie den TableAdapter verwenden, um eine dieser Abfragen auszuführen, können Sie das Command-Objekt s [`CommandTimeout`-Eigenschaft](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)erhöhen. Diese Eigenschaft gibt die Anzahl der Sekunden an, die auf die Ausführung des Befehls gewartet werden soll. der Standardwert ist 30.

Fügen Sie die folgende `Public` Methode mithilfe der in Schritt 2 erstellten partiellen Klassendatei, die in Schritt 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`) erstellt wurde, der `ProductsDataTable` hinzu, um die Anpassung der `CommandTimeout`-Eigenschaft durch die BLL zuzulassen:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Diese Methode kann von der BLL-oder Präsentationsebene aufgerufen werden, um das Befehls Timeout für alle Befehls Probleme dieser TableAdapter-Instanz festzulegen.

> [!NOTE]
> Die Eigenschaften "`Adapter`" und "`CommandCollection`" sind als `Private`gekennzeichnet, d. h., auf Sie kann nur über Code innerhalb des TableAdapters zugegriffen werden. Anders als die `Connection`-Eigenschaft können diese Zugriffsmodifizierer nicht konfiguriert werden. Wenn Sie Eigenschaften auf Befehls Ebene für andere Ebenen in der Architektur verfügbar machen müssen, müssen Sie daher den oben beschriebenen partiellen Klassen Ansatz verwenden, um eine `Public` Methode oder Eigenschaft bereitzustellen, die die `Private` Befehls Objekte liest oder schreibt.

## <a name="summary"></a>Zusammenfassung

Die TableAdapters in einem typisierten DataSet dienen zum Kapseln von Datenzugriffs Details und-Komplexität. Mithilfe von TableAdapters müssen Sie sich keine Gedanken über das Schreiben von ADO.NET-Code machen, um eine Verbindung mit der Datenbank herzustellen, einen Befehl auszugeben oder die Ergebnisse in eine Datentabelle aufzufüllen. Sie werden alle automatisch für uns verarbeitet.

Es kann jedoch vorkommen, dass die ADO.net-Besonderheiten auf niedriger Ebene angepasst werden müssen, z. b. das Ändern der Verbindungs Zeichenfolge oder der standardmäßigen Verbindungs-oder Befehls Timeout Werte. Der TableAdapter verfügt über automatisch generierte Eigenschaften vom Typ `Connection`, `Adapter`und `CommandCollection`, diese sind jedoch standardmäßig entweder `Friend` oder `Private`. Diese internen Informationen können verfügbar gemacht werden, indem der TableAdapter mithilfe von partiellen Klassen erweitert wird, um `Public` Methoden oder Eigenschaften einzuschließen. Alternativ kann der TableAdapter s `Connection`-eigenschaftenzugriffsmodifizierer über die Eigenschaft TableAdapter s `ConnectionModifier` konfiguriert werden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Burnadette Leigh, S ren Jacob Lauritsen, Teresa Murphy und Hilton geisenow. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](working-with-computed-columns-vb.md)
> [Weiter](protecting-connection-strings-and-other-configuration-information-vb.md)
