---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Verwenden von SQL-Cache Abhängigkeiten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Die einfachste Strategie zum Zwischenspeichern besteht darin, die zwischengespeicherten Daten nach einem bestimmten Zeitraum ablaufen zu lassen. Diese einfache Vorgehensweise bedeutet aber, dass die zwischengespeicherten Daten maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 7d095538bd92d50675e5fce44f5ca68e8ee6c0e8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78442905"
---
# <a name="using-sql-cache-dependencies-vb"></a>Verwenden von SQL-Cacheabhängigkeiten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) oder [PDF herunterladen](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> Die einfachste Strategie zum Zwischenspeichern besteht darin, die zwischengespeicherten Daten nach einem bestimmten Zeitraum ablaufen zu lassen. Diese einfache Vorgehensweise bedeutet aber, dass die zwischengespeicherten Daten nicht mit der zugrunde liegenden Datenquelle zusammengeführt werden. Dies führt zu veralteten Daten, die zu lange dauern, oder aktuelle Daten, die zu bald abgelaufen sind. Ein besserer Ansatz ist die Verwendung der sqlcachedepend-Klasse, damit die Daten zwischengespeichert bleiben, bis die zugrunde liegenden Daten in der SQL-Datenbank geändert wurden. In diesem Tutorial erfahren Sie, wie.

## <a name="introduction"></a>Einführung

Die zwischen Speicherungs Techniken, die unter zwischen [Speichern von Daten mit der ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) und zwischen [Speichern von Daten in den Architektur](caching-data-in-the-architecture-vb.md) Lernprogrammen untersucht wurden, verwendeten einen zeitbasierten Ablauf, um die Daten aus dem Cache nach einem bestimmten Zeitraum zu entfernen. Diese Vorgehensweise ist die einfachste Methode, um die Leistungssteigerungen der Zwischenspeicherung gegen Daten Veraltung auszugleichen. Durch die Auswahl eines Zeitverlaufs von *x* Sekunden gibt ein Seiten Entwickler an, dass Sie die Leistungsvorteile der Zwischenspeicherung nur für *x* Sekunden genießen, aber Sie können sich darauf verlassen, dass Ihre Daten nie länger als maximal *x* Sekunden abgelaufen sind. Natürlich kann *x* für statische Daten auf die Lebensdauer der Webanwendung erweitert werden, wie im Tutorial zwischen [Speichern von Daten beim Anwendungsstart](caching-data-at-application-startup-vb.md) untersucht.

Beim Zwischenspeichern von Datenbankdaten wird häufig eine zeitbasierte Ablaufzeit für die einfache Verwendung ausgewählt, aber häufig eine unzureichende Lösung. Im Idealfall bleiben die Datenbankdaten so lange zwischengespeichert, bis die zugrunde liegenden Daten in der Datenbank geändert wurden. nur dann würde der Cache entfernt werden. Durch diese Vorgehensweise werden die Leistungsvorteile der Zwischenspeicherung maximiert und die Dauer veralteter Daten minimiert. Um diese Vorteile nutzen zu können, muss jedoch ein System vorhanden sein, das weiß, wann die zugrunde liegenden Datenbankdaten geändert wurden, und die entsprechenden Elemente aus dem Cache entfernt werden. Vor ASP.NET 2,0 waren Seiten Entwickler für die Implementierung dieses Systems verantwortlich.

ASP.NET 2,0 bietet eine [`SqlCacheDependency`-Klasse](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) und die erforderliche-Infrastruktur, um zu bestimmen, wann eine Änderung in der Datenbank aufgetreten ist, sodass die entsprechenden zwischengespeicherten Elemente entfernt werden können. Es gibt zwei Verfahren, um zu bestimmen, wann sich die zugrunde liegenden Daten geändert haben: Benachrichtigung und Abruf. Nach der Erörterung der Unterschiede zwischen Benachrichtigung und Abruf erstellen wir die Infrastruktur, die für die Unterstützung von Abruf erforderlich ist, und untersuchen dann die Verwendung der `SqlCacheDependency`-Klasse in deklarativen und programmgesteuerten Szenarien.

## <a name="understanding-notification-and-polling"></a>Informationen zu Benachrichtigungen und Abruf

Es gibt zwei Techniken, mit denen bestimmt werden kann, wann die Daten in einer Datenbank geändert wurden: Benachrichtigung und Abruf. Bei der Benachrichtigung wird die ASP.NET-Laufzeit automatisch von der Datenbank benachrichtigt, wenn die Ergebnisse einer bestimmten Abfrage seit der letzten Ausführung der Abfrage geändert wurden. zu diesem Zeitpunkt werden die der Abfrage zugeordneten zwischengespeicherten Elemente entfernt. Beim Abrufen behält der Datenbankserver Informationen darüber bei, wann bestimmte Tabellen zuletzt aktualisiert wurden. Die ASP.NET-Laufzeit fragt regelmäßig die Datenbank ab, um zu überprüfen, welche Tabellen geändert wurden, seit Sie in den Cache eingegeben wurden. Bei den Tabellen, deren Daten geändert wurden, werden die zugehörigen Cache Elemente entfernt.

Die Benachrichtigungs Option erfordert weniger Setup als Abruf und ist präziser, da Änderungen auf Abfrage Ebene und nicht auf Tabellenebene nachverfolgt werden. Leider sind Benachrichtigungen nur in den vollständigen Editionen von Microsoft SQL Server 2005 verfügbar (d. h. die nicht-Express-Editionen). Die Abruf Option kann jedoch für alle Versionen von Microsoft SQL Server von 7,0 bis 2005 verwendet werden. Da in diesen Tutorials die Express Edition von SQL Server 2005 verwendet wird, konzentrieren wir uns auf das Einrichten und Verwenden der Abruf Option. Weitere Informationen zu SQL Server von Benachrichtigungsfunktionen von 2005 s finden Sie im Abschnitt weiter unten in diesem Tutorial.

Beim Abrufen muss die Datenbank so konfiguriert werden, dass eine Tabelle mit dem Namen `AspNet_SqlCacheTablesForChangeNotification` mit drei Spalten `tableName`, `notificationCreated`und `changeId`enthalten ist. Diese Tabelle enthält eine Zeile für jede Tabelle, die Daten enthält, die möglicherweise in einer SQL-Cache Abhängigkeit in der-Webanwendung verwendet werden müssen. Die Spalte `tableName` gibt den Namen der Tabelle an, während `notificationCreated` das Datum und die Uhrzeit angibt, zu der die Zeile der Tabelle hinzugefügt wurde. Die `changeId` Spalte ist vom Typ `int` und hat den Anfangswert 0. Der Wert wird bei jeder Änderung der Tabelle inkrementiert.

Zusätzlich zur `AspNet_SqlCacheTablesForChangeNotification` Tabelle muss die Datenbank auch Trigger für jede der Tabellen enthalten, die möglicherweise in einer SQL-Cache Abhängigkeit angezeigt werden. Diese Trigger werden immer dann ausgeführt, wenn eine Zeile eingefügt, aktualisiert oder gelöscht und der Wert der Tabelle s `changeId` in `AspNet_SqlCacheTablesForChangeNotification`erhöht wird.

Die ASP.NET-Laufzeit verfolgt beim Zwischenspeichern von Daten mit einem `SqlCacheDependency`-Objekt den aktuellen `changeId` für eine Tabelle. Die Datenbank wird in regelmäßigen Abständen überprüft, und alle `SqlCacheDependency` Objekte, deren `changeId` sich vom Wert in der Datenbank unterscheidet, werden entfernt, da ein abweichender `changeId` Wert angibt, dass seit der Zwischenspeicherung der Daten eine Änderung an der Tabelle vorgenommen wurde.

## <a name="step-1-exploring-theaspnet_regsqlexecommand-line-program"></a>Schritt 1: Untersuchen des`aspnet_regsql.exe`Befehlszeilen Programms

Beim Abruf Ansatz muss die Datenbank so eingerichtet werden, dass Sie die oben beschriebene Infrastruktur enthält: eine vordefinierte Tabelle (`AspNet_SqlCacheTablesForChangeNotification`), eine Handvoll gespeicherter Prozeduren und Trigger für jede Tabelle, die in SQL-Cache Abhängigkeiten in der Webanwendung verwendet werden kann. Diese Tabellen, gespeicherten Prozeduren und Trigger können über das Befehlszeilenprogramm `aspnet_regsql.exe`erstellt werden, das sich im Ordner `$WINDOWS$\Microsoft.NET\Framework\version` befindet. Um die `AspNet_SqlCacheTablesForChangeNotification` Tabelle und die zugehörigen gespeicherten Prozeduren zu erstellen, führen Sie den folgenden Befehl über die Befehlszeile aus:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Um diese Befehle auszuführen, muss sich die angegebene Daten Bank Anmeldung in den Rollen [`db_securityadmin`](https://msdn.microsoft.com/library/ms188685.aspx) und [`db_ddladmin`](https://msdn.microsoft.com/library/ms190667.aspx) befinden. Informationen zum Überprüfen des T-SQL, das vom `aspnet_regsql.exe` Befehlszeilenprogramm an die Datenbank gesendet wird, finden Sie in [diesem Blogeintrag](http://scottonwriting.net/sowblog/posts/10709.aspx).

Wenn Sie z. b. die Infrastruktur zum Abrufen einer Microsoft SQL Server Datenbank mit dem Namen `pubs` auf einem Daten Bank Server mit dem Namen `ScottsServer` mithilfe der Windows-Authentifizierung hinzufügen möchten, navigieren Sie zum entsprechenden Verzeichnis, und geben Sie in der Befehlszeile Folgendes ein:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Nachdem die Infrastruktur auf Datenbankebene hinzugefügt wurde, müssen die Trigger den Tabellen hinzugefügt werden, die in SQL-Cache Abhängigkeiten verwendet werden. Verwenden Sie das `aspnet_regsql.exe` Befehlszeilenprogramm erneut, geben Sie den Tabellennamen jedoch mithilfe des `-t`-Schalters an, und verwenden Sie anstelle des `-ed` Schalters `-et`wie folgt:

[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Verwenden Sie zum Hinzufügen der Trigger zum `authors` und `titles` Tabellen in der `pubs`-Datenbank auf `ScottsServer`Folgendes:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

Fügen Sie für dieses Tutorial die Trigger zu den Tabellen `Products`, `Categories`und `Suppliers` hinzu. Wir sehen uns die Befehlszeilen Syntax in Schritt 3 an.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inapp_data"></a>Schritt 2: verweisen auf eine Microsoft SQL Server 2005 Express Edition-Datenbank in`App_Data`

Das `aspnet_regsql.exe`-Befehlszeilenprogramm erfordert den Datenbank-und Servernamen, um die erforderliche Abruf Infrastruktur hinzuzufügen. Aber wie lautet der Datenbank-und Servername für eine Microsoft SQL Server 2005 Express-Datenbank, die sich im Ordner "`App_Data`" befindet? Anstatt die Namen der Datenbank und des Servers zu ermitteln, habe ich festgestellt, dass der einfachste Ansatz darin besteht, die Datenbank an die `localhost\SQLExpress` Daten Bank Instanz anzufügen und die Daten mithilfe [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx)umzubenennen. Wenn eine der Vollversionen von SQL Server 2005 auf Ihrem Computer installiert ist, haben Sie wahrscheinlich bereits SQL Server Management Studio auf dem Computer installiert. Wenn Sie nur über die Express Edition verfügen, können Sie die kostenlose [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)herunterladen.

Beginnen Sie mit dem Schließen von Visual Studio. Öffnen Sie als nächstes SQL Server Management Studio, und wählen Sie mithilfe der Windows-Authentifizierung eine Verbindung mit dem `localhost\SQLExpress` Server aus.

![Anfügen an den localhost\SQLExpress-Server](using-sql-cache-dependencies-vb/_static/image1.gif)

**Abbildung 1**: Anfügen an den `localhost\SQLExpress` Server

Nach dem Herstellen einer Verbindung mit dem Server zeigt Management Studio den Server an und enthält Unterordner für die Datenbanken, Sicherheit usw. Klicken Sie mit der rechten Maustaste auf den Ordner Datenbanken, und wählen Sie die Option anhängen. Dadurch wird das Dialogfeld Datenbanken anfügen angezeigt (siehe Abbildung 2). Klicken Sie auf die Schaltfläche hinzufügen, und wählen Sie im Ordner `App_Data` der Webanwendung den Ordner `NORTHWND.MDF` Datenbank aus.

[![das Northwnd anfügen. MDF-Datenbank aus dem Ordner "App_Data"](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Abbildung 2**: Anfügen der `NORTHWND.MDF` Datenbank aus dem Ordner "`App_Data`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image2.png))

Dadurch wird die Datenbank dem Ordner Datenbanken hinzugefügt. Der Datenbankname ist möglicherweise der vollständige Pfad zur Datenbankdatei oder der vollständige Pfad, dem eine [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)vorangesteht ist. Um zu vermeiden, dass Sie diesen langwierigen Datenbanknamen bei Verwendung des Befehlszeilen Tools ASPNET\_RegSql. exe eingeben müssen, benennen Sie die Datenbank in einen benutzerfreundlicheren Namen um, indem Sie mit der rechten Maustaste auf die soeben angefügte Datenbank klicken und Umbenennen auswählen. Ich habe meine Datenbank in "datatutorials" umbenannt.

![Benennen Sie die angefügte Datenbank in einen benutzerfreundlicheren Namen um.](using-sql-cache-dependencies-vb/_static/image3.gif)

**Abbildung 3**: Umbenennen der angefügten Datenbank in einen benutzerfreundlicheren Namen

## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Schritt 3: Hinzufügen der Abruf Infrastruktur zur Northwind-Datenbank

Nachdem wir nun die `NORTHWND.MDF` Datenbank aus dem Ordner "`App_Data`" angefügt haben, können wir die Abruf Infrastruktur hinzufügen. Wenn Sie die Datenbank in datatutorials umbenannt haben, führen Sie die folgenden vier Befehle aus:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Nachdem Sie diese vier Befehle ausgeführt haben, klicken Sie in Management Studio mit der rechten Maustaste auf den Datenbanknamen, navigieren Sie zum Untermenü Tasks, und wählen Sie trennen aus. Schließen Sie dann Management Studio, und öffnen Sie Visual Studio erneut.

Nachdem Visual Studio erneut geöffnet wurde, führen Sie einen Drilldown in die Datenbank durch Server-Explorer aus. Beachten Sie die neue Tabelle (`AspNet_SqlCacheTablesForChangeNotification`), die neuen gespeicherten Prozeduren und die Trigger in den Tabellen "`Products`", "`Categories`" und "`Suppliers`".

![Die Datenbank enthält jetzt die erforderliche Abruf Infrastruktur.](using-sql-cache-dependencies-vb/_static/image4.gif)

**Abbildung 4**: die Datenbank enthält jetzt die erforderliche Abruf Infrastruktur

## <a name="step-4-configuring-the-polling-service"></a>Schritt 4: Konfigurieren des Abruf dienstanweises

Nachdem Sie die erforderlichen Tabellen, Trigger und gespeicherten Prozeduren in der Datenbank erstellt haben, ist der letzte Schritt die Konfiguration des Abruf Dienstanbieter, der durch `Web.config` durch Angabe der zu verwendenden Datenbanken und der Abruf Häufigkeit in Millisekunden erreicht wird. Das folgende Markup fragt die Northwind-Datenbank einmal pro Sekunde ab.

[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

Der `name` Wert im `<add>`-Element (northwinddb) ordnet einen lesbaren Namen einer bestimmten Datenbank zu. Beim Arbeiten mit SQL-Cache Abhängigkeiten müssen wir auf den hier definierten Datenbanknamen sowie auf die Tabelle verweisen, auf der die zwischengespeicherten Daten basieren. Wir sehen, wie Sie die `SqlCacheDependency`-Klasse verwenden, um zwischengespeicherten Daten in Schritt 6 Programm gesteuert SQL-Cache Abhängigkeiten zuzuordnen.

Nachdem eine SQL-Cache Abhängigkeit hergestellt wurde, stellt das Abrufsystem eine Verbindung mit den Datenbanken her, die in den `<databases>` Elementen alle `pollTime` Millisekunden definiert sind, und führt die gespeicherte Prozedur `AspNet_SqlCachePollingStoredProcedure` aus. Diese gespeicherte Prozedur, die in Schritt 3 mithilfe des Befehlszeilen Tools `aspnet_regsql.exe` hinzugefügt wurde, gibt die `tableName`-und `changeId` Werte für jeden Datensatz in `AspNet_SqlCacheTablesForChangeNotification`zurück. Veraltete SQL-Cache Abhängigkeiten werden aus dem Cache entfernt.

Die `pollTime` Einstellung führt zu einem Kompromiss zwischen Leistung und Daten Veraltung. Ein kleiner `pollTime` Wert erhöht die Anzahl der Anforderungen an die Datenbank, aber es werden schneller veraltete Daten aus dem Cache entfernt. Ein größerer `pollTime` Wert reduziert die Anzahl der Datenbankanforderungen, erhöht jedoch die Verzögerung zwischen dem Zeitpunkt, zu dem die Back-End-Daten geändert werden, und dem Entfernen der zugehörigen Cache Elemente. Glücklicherweise führt die Daten Bank Anforderung eine einfache gespeicherte Prozedur aus, die nur wenige Zeilen aus einer einfachen, einfachen Tabelle zurückgibt. Experimentieren Sie jedoch mit unterschiedlichen `pollTime` Werten, um ein optimales Gleichgewicht zwischen Datenbankzugriff und Daten Veraltung für Ihre Anwendung zu finden. Der kleinste `pollTime` zulässige Wert ist 500.

> [!NOTE]
> Im obigen Beispiel wird ein einzelner `pollTime` Wert im `<sqlCacheDependency>`-Element bereitstellt, aber Sie können optional den `pollTime` Wert im `<add>`-Element angeben. Dies ist hilfreich, wenn Sie mehrere Datenbanken angegeben haben und die Abruf Häufigkeit pro Datenbank anpassen möchten.

## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Schritt 5: Deklaratives arbeiten mit SQL-Cache Abhängigkeiten

In den Schritten 1 bis 4 haben wir uns mit dem Einrichten der erforderlichen Datenbankinfrastruktur und der Konfiguration des Abruf Systems beschäftigt. Mit dieser Infrastruktur können jetzt dem Daten Cache Elemente mit einer zugehörigen SQL-Cache Abhängigkeit mithilfe Programm gesteuerter oder deklarativer Verfahren hinzugefügt werden. In diesem Schritt untersuchen wir die deklarative Arbeit mit SQL-Cache Abhängigkeiten. In Schritt 6 betrachten wir die programmgesteuerte Vorgehensweise.

Im Tutorial zum zwischen [Speichern von Daten mit dem ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) -Tutorial wurden die deklarativen zwischen Speicherungs Funktionen von ObjectDataSource untersucht. Wenn Sie einfach die `EnableCaching`-Eigenschaft auf `True` und die `CacheDuration`-Eigenschaft auf ein bestimmtes Zeitintervall festlegen, werden die Daten, die vom zugrunde liegenden-Objekt für das angegebene Intervall zurückgegeben werden, von ObjectDataSource automatisch zwischengespeichert. Von ObjectDataSource können auch mindestens eine SQL-Cache Abhängigkeit verwendet werden.

Um die deklarative Verwendung von SQL-Cache Abhängigkeiten zu veranschaulichen, öffnen Sie die Seite `SqlCacheDependencies.aspx` im Ordner `Caching`, und ziehen Sie eine GridView aus der Toolbox auf den Designer. Legen Sie die `ID` GridView s auf `ProductsDeclarative` fest, und wählen Sie aus dem smarttagtag aus, dass es an eine neue ObjectDataSource mit dem Namen `ProductsDataSourceDeclarative`gebunden werden soll.

[![erstellen Sie eine neue ObjectDataSource namens productdatasourcedeclarative.](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource mit dem Namen "`ProductsDataSourceDeclarative`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image4.png))

Konfigurieren Sie ObjectDataSource so, dass die `ProductsBLL`-Klasse verwendet wird, und legen Sie die Dropdown Liste auf der Registerkarte auswählen auf `GetProducts()`fest. Wählen Sie auf der Registerkarte Update die `UpdateProduct` Überladung mit drei Eingabe Parametern aus: `productName`, `unitPrice`und `productID`. Legen Sie die Dropdown Liste auf der Registerkarte Einfügen und löschen auf (keine) fest.

[![die UpdateProduct-Überladung mit drei Eingabe Parametern verwenden](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Abbildung 6**: Verwenden der UpdateProduct-Überladung mit drei Eingabe Parametern ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image6.png))

[![die Dropdown Liste für die Registerkarten einfügen und löschen auf (keine) festlegen.](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Abbildung 7**: Festlegen der Dropdown Liste auf (keine) für die Registerkarten "Einfügen" und "Löschen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image8.png))

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen erstellt Visual Studio boundfields und checkboxfields in der GridView für jedes der Datenfelder. Entfernen Sie alle Felder, jedoch `ProductName`, `CategoryName`und `UnitPrice`, und formatieren Sie diese Felder wie angezeigt. Aktivieren Sie aus dem GridView s-Smarttag die Kontrollkästchen Paging aktivieren, Sortierung aktivieren und Bearbeiten aktivieren. In Visual Studio wird die Eigenschaft ObjectDataSource s `OldValuesParameterFormatString` auf `original_{0}`festgelegt. Damit die Funktion zum Bearbeiten von GridView-Funktionen ordnungsgemäß funktioniert, entfernen Sie diese Eigenschaft vollständig aus der deklarativen Syntax, oder legen Sie Sie auf den Standardwert zurück, `{0}`.

Fügen Sie schließlich über der GridView ein Label-websteuer Element hinzu, und legen Sie dessen `ID`-Eigenschaft auf `ODSEvents` und dessen `EnableViewState`-Eigenschaft auf `False`fest. Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup der Seite in etwa wie folgt aussehen. Beachten Sie, dass ich einige ästhetische Anpassungen an den GridView-Feldern vorgenommen habe, die nicht erforderlich sind, um die SQL-Cache-Abhängigkeits Funktionalität zu veranschaulichen.

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Erstellen Sie als nächstes einen Ereignishandler für das Ereignis "ObjectDataSource s `Selecting`", und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Beachten Sie, dass das ObjectDataSource s-`Selecting` Ereignis nur beim Abrufen von Daten aus dem zugrunde liegenden-Objekt ausgelöst wird. Wenn die ObjectDataSource auf die Daten aus dem eigenen Cache zugreift, wird dieses Ereignis nicht ausgelöst.

Besuchen Sie diese Seite nun über einen Browser. Da wir noch keine Zwischenspeicherung implementieren müssen, sollte die Seite jedes Mal, wenn Sie das Raster bearbeiten, Sortieren oder bearbeiten, den Text anzeigen. Wählen Sie dann Ereignis ausgelöst aus, wie in Abbildung 8 gezeigt.

[![das Ereignis zum Auswählen von ObjectDataSource s bei jedem Pager, bearbeiten oder Sortieren der GridView ausgelöst wird.](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Abbildung 8**: das Ereignis "ObjectDataSource s `Selecting`" wird jedes Mal ausgelöst, wenn die GridView per Pager, bearbeitet oder sortiert wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image10.png)).

Wie im Tutorial zwischen [Speichern von Daten mit dem ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) -Tutorial gezeigt, wird durch das Festlegen der `EnableCaching`-Eigenschaft auf `True` bewirkt, dass ObjectDataSource seine Daten für die Dauer zwischenspeichert, die von der `CacheDuration`-Eigenschaft angegeben wird. ObjectDataSource verfügt auch über eine [`SqlCacheDependency`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), die den zwischengespeicherten Daten mithilfe des folgenden Musters eine oder mehrere SQL-Cache Abhängigkeiten hinzufügt:

[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Dabei ist *DatabaseName* der Name der Datenbank, wie im `name`-Attribut des `<add>`-Elements in `Web.config`angegeben, und *TableName* ist der Name der Datenbanktabelle. Um z. b. eine ObjectDataSource zu erstellen, die Daten unbegrenzt auf der Grundlage einer SQL-Cache Abhängigkeit von der Northwind s-`Products` Tabelle speichert, legen Sie die Eigenschaft ObjectDataSource s `EnableCaching` auf `True` und deren `SqlCacheDependency`-Eigenschaft auf northwinddb: Products fest.

> [!NOTE]
> Sie können eine SQL-Cache Abhängigkeit *und* einen zeitbasierten Ablauf verwenden, indem Sie `EnableCaching` auf `True`festlegen, auf das Zeitintervall `CacheDuration` und `SqlCacheDependency` auf die Datenbank und den Tabellennamen. Die ObjectDataSource entfernt die Daten, wenn der zeitbasierte Ablauf erreicht wird, oder wenn das Abrufsystem feststellt, dass sich die zugrunde liegenden Datenbankdaten geändert haben, je nachdem, welcher Vorgang zuerst ausgeführt wird.

Die GridView in `SqlCacheDependencies.aspx` zeigt Daten aus zwei Tabellen an, `Products` und `Categories` (das `CategoryName` Feld Product s wird über eine `JOIN` auf `Categories`abgerufen). Daher möchten wir zwei SQL-Cache Abhängigkeiten angeben: northwinddb: Products; Northwinddb: Kategorien.

[![Konfigurieren von ObjectDataSource zur Unterstützung der Zwischenspeicherung mithilfe von SQL-Cache Abhängigkeiten von Produkten und Kategorien](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Abbildung 9**: Konfigurieren von ObjectDataSource zur Unterstützung der Zwischenspeicherung mithilfe von SQL-Cache Abhängigkeiten auf `Products` und `Categories` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image12.png))

Nach dem Konfigurieren von ObjectDataSource zur Unterstützung der Zwischenspeicherung überprüfen Sie die Seite über einen Browser. Der Text, der auswählt, sollte beim ersten Besuch der Seite angezeigt werden, sollte jedoch beim Paging, beim Sortieren oder beim Klicken auf die Schaltflächen "Bearbeiten" oder "Abbrechen" entfernt werden. Dies liegt daran, dass nach dem Laden der Daten in den ObjectDataSource s-Cache diese beibehalten werden, bis die `Products`-oder `Categories` Tabellen geändert werden oder die Daten über die GridView aktualisiert werden.

Öffnen Sie nach dem Paging durch das Raster und dem Hinweis, dass der Text ausgelöste Ereignis Auslösung nicht mehr angezeigt wird. Öffnen Sie ein neues Browserfenster, und navigieren Sie im Abschnitt bearbeiten, einfügen und löschen (`~/EditInsertDelete/Basics.aspx`) zum Grundlagen-Tutorial. Aktualisieren Sie den Namen oder den Preis eines Produkts. Zeigen Sie dann in das erste Browserfenster eine andere Datenseite an, Sortieren Sie das Raster, oder klicken Sie auf die Schaltfläche zum Bearbeiten von Zeilen. Diesmal sollte das ausgelöste Ereignis erneut angezeigt werden, da die zugrunde liegenden Datenbankdaten geändert wurden (siehe Abbildung 10). Wenn der Text nicht angezeigt wird, warten Sie einen Moment, und versuchen Sie es noch mal. Beachten Sie, dass der Abruf Dienst alle `pollTime` Millisekunden auf Änderungen an der `Products` Tabelle prüft. es gibt also eine Verzögerung zwischen dem Zeitpunkt, zu dem die zugrunde liegenden Daten aktualisiert werden, und dem Zeitpunkt, zu dem die zwischengespeicherten Daten entfernt werden.

[![Ändern der Products-Tabelle werden die zwischengespeicherten Produktdaten entfernt.](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Abbildung 10**: Ändern der Products-Tabelle entfernt die zwischengespeicherten Produktdaten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image14.png))

## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Schritt 6: Programm gesteuertes arbeiten mit der`SqlCacheDependency`-Klasse

Im Tutorial zum zwischen [Speichern von Daten in der Architektur](caching-data-in-the-architecture-vb.md) wurden die Vorteile der Verwendung einer separaten Cache Schicht in der Architektur behandelt, anstatt das Zwischenspeichern eng mit der ObjectDataSource zu koppeln. In diesem Tutorial haben wir eine `ProductsCL` Klasse erstellt, um die programmgesteuerte Arbeit mit dem Daten Cache zu veranschaulichen. Verwenden Sie die `SqlCacheDependency`-Klasse, um SQL-Cache Abhängigkeiten in der zwischen Speicherungs Ebene zu verwenden.

Beim Abrufsystem muss ein `SqlCacheDependency` Objekt einer bestimmten Datenbank und einem Tabellen Paar zugeordnet werden. Der folgende Code erstellt z. b. ein `SqlCacheDependency` Objekt, das auf der Northwind-Datenbank s `Products` Tabelle basiert:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Die beiden Eingabeparameter für den `SqlCacheDependency` s-Konstruktor sind die Datenbank bzw. die Tabellennamen. Wie bei der ObjectDataSource s `SqlCacheDependency`-Eigenschaft entspricht der verwendete Datenbankname dem Wert, der im `name`-Attribut des `<add>`-Elements in `Web.config`angegeben ist. Der Tabellenname ist der tatsächliche Name der Datenbanktabelle.

Um einem Element, das dem Daten Cache hinzugefügt wurde, eine `SqlCacheDependency` zuzuordnen, verwenden Sie eine der `Insert` Methoden Überladungen, die eine Abhängigkeit annehmen. Der folgende Code fügt dem Daten Cache für eine unbestimmte Dauer einen *Wert* hinzu, ordnet ihn jedoch einer `SqlCacheDependency` in der `Products` Tabelle zu. Kurz gesagt verbleiben *Werte* im Cache, bis Sie aufgrund von Arbeitsspeicher Einschränkungen entfernt werden oder das Abrufsystem festgestellt hat, dass die `Products` Tabelle seit der Zwischenspeicherung geändert wurde.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

Die `ProductsCL` Klasse der zwischen Speicherungs Schicht speichert aktuell Daten aus der `Products` Tabelle mithilfe eines zeitbasierten Ablaufs von 60 Sekunden. Aktualisieren Sie diese Klasse, sodass stattdessen SQL-Cache Abhängigkeiten verwendet werden. Die `ProductsCL` Class s `AddCacheItem`-Methode, die für das Hinzufügen der Daten zum Cache zuständig ist, enthält derzeit den folgenden Code:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Aktualisieren Sie diesen Code, sodass anstelle der `MasterCacheKeyArray` Cache Abhängigkeit ein `SqlCacheDependency` Objekt verwendet wird:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Um diese Funktionalität zu testen, fügen Sie der Seite unterhalb der vorhandenen `ProductsDeclarative` GridView eine GridView hinzu. Legen Sie die neue GridView s-`ID` auf `ProductsProgrammatic` fest, und binden Sie Sie über das Smarttag an eine neue ObjectDataSource mit dem Namen `ProductsDataSourceProgrammatic`. Konfigurieren Sie ObjectDataSource für die Verwendung der `ProductsCL`-Klasse, und legen Sie die Dropdown Listen auf den Registerkarten auswählen und Aktualisieren auf `GetProducts` bzw. `UpdateProduct`fest.

[![Konfigurieren von ObjectDataSource für die Verwendung der productscl-Klasse](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Abbildung 11**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsCL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image16.png))

[![in der Dropdown Liste "Registerkarte auswählen" die Methode "GetProducts" auswählen](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Abbildung 12**: Auswählen der `GetProducts`-Methode aus der Dropdown Liste "Registerkarte auswählen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image18.png))

[![wählen Sie die UpdateProduct-Methode aus der Dropdown Liste der Registerkarte "Aktualisieren" aus.](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Abbildung 13**: Auswählen der UpdateProduct-Methode aus der Dropdown Liste der Registerkarte "Aktualisieren" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-sql-cache-dependencies-vb/_static/image20.png))

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen erstellt Visual Studio boundfields und checkboxfields in der GridView für jedes der Datenfelder. Entfernen Sie wie bei der ersten GridView, die dieser Seite hinzugefügt wurde, alle Felder, jedoch `ProductName`, `CategoryName`und `UnitPrice`, und formatieren Sie diese Felder wie gewünscht. Aktivieren Sie aus dem GridView s-Smarttag die Kontrollkästchen Paging aktivieren, Sortierung aktivieren und Bearbeiten aktivieren. Wie bei der `ProductsDataSourceDeclarative` ObjectDataSource legt Visual Studio die `ProductsDataSourceProgrammatic` ObjectDataSource-Eigenschaft `OldValuesParameterFormatString` auf `original_{0}`fest. Damit die Funktion zum Bearbeiten von GridView-Funktionen ordnungsgemäß funktioniert, legen Sie diese Eigenschaft auf `{0}` zurück (oder entfernen Sie die Eigenschafts Zuweisung aus der deklarativen Syntax vollständig).

Nachdem Sie diese Aufgaben abgeschlossen haben, sollten das resultierende GridView-und ObjectDataSource-Markup wie folgt aussehen:

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Um die SQL-Cache Abhängigkeit in der zwischen Speicherungs Ebene zu testen, legen Sie einen Haltepunkt in der `ProductCL`-Klasse `AddCacheItem` Methode fest, und starten Sie das Debugging. Wenn Sie `SqlCacheDependencies.aspx`zum ersten Mal besuchen, sollte der Haltepunkt getroffen werden, wenn die Daten zum ersten Mal angefordert und im Cache abgelegt werden. Wechseln Sie als nächstes zu einer anderen Seite in der GridView, oder sortieren Sie eine der Spalten. Dies bewirkt, dass die GridView Ihre Daten anweist, die Daten jedoch im Cache gefunden werden, da die `Products` Datenbanktabelle nicht geändert wurde. Wenn die Daten wiederholt nicht im Cache gefunden werden, stellen Sie sicher, dass genügend Arbeitsspeicher auf dem Computer verfügbar ist, und wiederholen Sie den Vorgang.

Öffnen Sie nach dem Paging durch einige Seiten der GridView ein zweites Browserfenster, und navigieren Sie im Abschnitt bearbeiten, einfügen und löschen (`~/EditInsertDelete/Basics.aspx`) zum Grundlagen-Tutorial. Aktualisieren Sie einen Datensatz in der Tabelle Products, und zeigen Sie dann im ersten Browserfenster eine neue Seite an, oder klicken Sie auf einen der Sortier Header.

In diesem Szenario wird eine der beiden folgenden Punkte angezeigt: entweder wird der Breakpoint gedrückt, und es wird angegeben, dass die zwischengespeicherten Daten aufgrund der Änderung in der Datenbank entfernt wurden. oder der Breakpoint wird nicht gedrückt, was bedeutet, dass `SqlCacheDependencies.aspx` jetzt veraltete Daten anzeigt. Wenn der Haltepunkt nicht gedrückt wird, liegt dies wahrscheinlich daran, dass der Abruf Dienst noch nicht ausgelöst wurde, da die Daten geändert wurden. Beachten Sie, dass der Abruf Dienst alle `pollTime` Millisekunden auf Änderungen an der `Products` Tabelle prüft. es gibt also eine Verzögerung zwischen dem Zeitpunkt, zu dem die zugrunde liegenden Daten aktualisiert werden, und dem Zeitpunkt, zu dem die zwischengespeicherten Daten entfernt werden.

> [!NOTE]
> Diese Verzögerung wird eher angezeigt, wenn eines der Produkte über die GridView in `SqlCacheDependencies.aspx`bearbeitet wird. Im Tutorial zum zwischen [Speichern von Daten in der Architektur](caching-data-in-the-architecture-vb.md) haben wir die `MasterCacheKeyArray` Cache Abhängigkeit hinzugefügt, um sicherzustellen, dass die Daten, die über die `ProductsCL` Class s `UpdateProduct`-Methode bearbeitet wurden, aus dem Cache entfernt wurden. Wir haben diese Cache Abhängigkeit jedoch ersetzt, wenn wir die `AddCacheItem` Methode weiter oben in diesem Schritt geändert haben. Daher zeigt die `ProductsCL` Klasse die zwischengespeicherten Daten so lange an, bis das Abrufsystem die Änderung an der `Products` Tabelle notiert. Wir sehen uns an, wie Sie die `MasterCacheKeyArray` Cache Abhängigkeit in Schritt 7 erneut einführen können.

## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Schritt 7: Zuordnen mehrerer Abhängigkeiten zu einem zwischengespeicherten Element

Beachten Sie, dass die `MasterCacheKeyArray` Cache Abhängigkeit verwendet wird, um sicherzustellen, dass *alle* produktbezogenen Daten aus dem Cache entfernt werden, wenn ein einzelnes Element, das in ihr verknüpft ist, aktualisiert wird. Beispielsweise speichert die `GetProductsByCategoryID(categoryID)`-Methode `ProductsDataTables` Instanzen für jeden eindeutigen *CategoryID* -Wert zwischen. Wenn eines dieser Objekte entfernt wird, gewährleistet die `MasterCacheKeyArray` Cache Abhängigkeit, dass die anderen ebenfalls entfernt werden. Ohne diese Cache Abhängigkeit ist es möglich, dass andere zwischengespeicherte Produktdaten veraltet sind, wenn die zwischengespeicherten Daten geändert werden. Folglich ist es wichtig, dass die `MasterCacheKeyArray` Cache Abhängigkeit bei Verwendung von SQL-Cache Abhängigkeiten beibehalten wird. Die `Insert`-Methode des Daten Caches ermöglicht jedoch nur ein einzelnes Abhängigkeits Objekt.

Außerdem müssen bei der Arbeit mit SQL-Cache Abhängigkeiten möglicherweise mehrere Datenbanktabellen als Abhängigkeiten verknüpft werden. Beispielsweise enthält die `ProductsDataTable`, die in der `ProductsCL`-Klasse zwischengespeichert ist, die Kategorien-und Lieferanten Namen für jedes Produkt, aber die `AddCacheItem`-Methode verwendet nur eine Abhängigkeit von `Products`. Wenn der Benutzer den Namen einer Kategorie oder eines Lieferanten aktualisiert, verbleiben die zwischengespeicherten Produktdaten in dieser Situation im Cache und sind veraltet. Daher möchten wir, dass die zwischengespeicherten Produktdaten nicht nur von der `Products` Tabelle, sondern auch von den Tabellen `Categories` und `Suppliers` abhängig sind.

Die [`AggregateCacheDependency`-Klasse](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) bietet eine Möglichkeit zum Zuordnen mehrerer Abhängigkeiten zu einem Cache Element. Beginnen Sie mit dem Erstellen einer `AggregateCacheDependency`-Instanz. Fügen Sie als nächstes den Satz von Abhängigkeiten hinzu, indem Sie die `AggregateCacheDependency` s `Add`-Methode verwenden. Wenn Sie das Element anschließend in den Daten Cache einfügen, übergeben Sie die `AggregateCacheDependency`-Instanz. Wenn sich *eine* der Abhängigkeiten der `AggregateCacheDependency` Instanz ändert, wird das zwischengespeicherte Element entfernt.

Der folgende Code zeigt den aktualisierten Code für die `ProductsCL`-Klasse `AddCacheItem`-Methode. Die-Methode erstellt die `MasterCacheKeyArray` Cache Abhängigkeit zusammen mit `SqlCacheDependency`-Objekten für die Tabellen `Products`, `Categories`und `Suppliers`. Diese werden alle in einem `AggregateCacheDependency` Objekt mit dem Namen `aggregateDependencies`kombiniert, das dann an die `Insert`-Methode weitergegeben wird.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Testen Sie diesen neuen Code. Änderungen an den Tabellen `Products`, `Categories`oder `Suppliers` bewirken nun, dass die zwischengespeicherten Daten entfernt werden. Außerdem wird die `MasterCacheKeyArray` Cache Abhängigkeit entfernt, wenn die `ProductsCL`-Klasse `UpdateProduct` Methode, die beim Bearbeiten eines Produkts über die GridView aufgerufen wird, die Cache Abhängigkeit entfernt, was bewirkt, dass die zwischengespeicherten `ProductsDataTable` entfernt werden und die Daten bei der nächsten Anforderung erneut abgerufen werden.

> [!NOTE]
> SQL-Cache Abhängigkeiten können auch mit der [Ausgabe](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx)Zwischenspeicherung verwendet werden. Eine Demonstration dieser Funktionalität finden Sie unter Verwenden von [ASP.net Output Caching with SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).

## <a name="summary"></a>Zusammenfassung

Beim Zwischenspeichern von Datenbankdaten verbleiben die Daten idealerweise im Cache, bis Sie in der Datenbank geändert werden. Mit ASP.NET 2,0 können SQL-Cache Abhängigkeiten in deklarativen und programmgesteuerten Szenarien erstellt und verwendet werden. Eine der Herausforderungen bei diesem Ansatz besteht darin, herauszufinden, wann die Daten geändert wurden. Die vollständigen Versionen von Microsoft SQL Server 2005 bieten Benachrichtigungsfunktionen, die eine Anwendung Benachrichtigen können, wenn sich ein Abfrageergebnis geändert hat. Für die Express Edition von SQL Server 2005 und älteren Versionen von SQL Server muss stattdessen ein Abrufsystem verwendet werden. Glücklicherweise ist das Einrichten der erforderlichen Abruf Infrastruktur recht unkompliziert.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Verwenden von Abfrage Benachrichtigungen in Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Erstellen einer Abfrage Benachrichtigung](https://msdn.microsoft.com/library/ms188669.aspx)
- [Caching in ASP.net mit der `SqlCacheDependency`-Klasse](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server Registrierungs Tool (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Übersicht über `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Marko Rangel, Teresa Murphy und Hilton giesreviewer. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Previous](caching-data-at-application-startup-vb.md)
