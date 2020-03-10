---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Datenquellen-Steuerelemente | Microsoft-Dokumentation
author: microsoft
description: Das DataGrid-Steuerelement in ASP.NET 1. x hat eine große Verbesserung beim Datenzugriff in Webanwendungen gekennzeichnet. Dies war jedoch nicht so benutzerfreundlich wie möglich....
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: a2e2cfbec3e5aebf42a2de30bab7d45b4b610298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519351"
---
# <a name="data-source-controls"></a>Datenquellen-Steuerelemente

von [Microsoft](https://github.com/microsoft)

> Das DataGrid-Steuerelement in ASP.NET 1. x hat eine große Verbesserung beim Datenzugriff in Webanwendungen gekennzeichnet. Dies war jedoch nicht so benutzerfreundlich wie möglich. Es war immer noch eine beträchtliche Menge an Code erforderlich, um sehr nützliche Funktionen zu erhalten. Dies ist das Modell in allen Datenzugriffs Bestrebungen in 1. x.

Das DataGrid-Steuerelement in ASP.NET 1. x hat eine große Verbesserung beim Datenzugriff in Webanwendungen gekennzeichnet. Dies war jedoch nicht so benutzerfreundlich wie möglich. Es war immer noch eine beträchtliche Menge an Code erforderlich, um sehr nützliche Funktionen zu erhalten. Dies ist das Modell in allen Datenzugriffs Bestrebungen in 1. x.

ASP.NET 2,0 adressiert dies mit Datenquellen-Steuerelementen. Die Datenquellen-Steuerelemente in ASP.NET 2,0 stellen Entwicklern ein deklaratives Modell zum Abrufen von Daten, Anzeigen von Daten und Bearbeiten von Daten bereit. Der Zweck der Datenquellen Steuerelemente besteht darin, eine konsistente Darstellung von Daten für Daten gebundene Steuerelemente bereitzustellen, unabhängig von der Quelle dieser Daten. Das Herzstück der Datenquellen-Steuerelemente in ASP.NET 2,0 ist die abstrakte Klasse DataSourceControl. Die DataSourceControl-Klasse stellt eine Basis Implementierung der IDataSource-Schnittstelle und der IListSource-Schnittstelle bereit, die es Ihnen ermöglicht, das Datenquellen-Steuerelement als Datenquelle für ein Daten gebundenes Steuerelement zuzuweisen (über die neue DataSourceID-Eigenschaft). später erläutert) und die darin enthaltenen Daten als Liste verfügbar machen. Jede Liste der Daten aus einem Datenquellen-Steuerelement wird als DataSourceView-Objekt verfügbar gemacht. Der Zugriff auf die DataSourceView-Instanzen wird von der IDataSource-Schnittstelle bereitgestellt. Beispielsweise gibt die GetViewNames-Methode eine ICollection zurück, die es Ihnen ermöglicht, die einem bestimmten Datenquellen Steuerelement zugeordneten DataSourceViews aufzulisten, und mit der GetView-Methode können Sie über den Namen auf eine bestimmte DataSourceView-Instanz zugreifen.

Datenquellen-Steuerelemente haben keine Benutzeroberfläche. Sie werden als Server Steuerelemente implementiert, sodass Sie deklarative Syntax unterstützen können, sodass Sie bei Bedarf auf den Seiten Zustand zugreifen können. Datenquellen-Steuerelemente erzeugen kein HTML-Markup für den Client.

> [!NOTE]
> Wie Sie später sehen werden, gibt es auch Cache Vorteile, die durch die Verwendung von Datenquellen-Steuerelementen erzielt werden.

## <a name="storing-connection-strings"></a>Speichert Verbindungs Zeichenfolgen

Bevor wir uns mit der Konfiguration von Datenquellen-Steuerelementen befassen, sollten wir eine neue Funktion in ASP.NET 2,0 für Verbindungs Zeichenfolgen abdecken. ASP.NET 2,0 führt einen neuen Abschnitt in der Konfigurationsdatei ein, mit dem Sie problemlos Verbindungs Zeichenfolgen speichern können, die zur Laufzeit dynamisch lesbar sind. Der Abschnitt "&lt;connectionStrings&gt;" erleichtert das Speichern von Verbindungs Zeichenfolgen.

Der folgende Code Ausschnitt fügt eine neue Verbindungs Zeichenfolge hinzu.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Wie im Abschnitt "&lt;appSettings&gt;" wird der Abschnitt "&lt;connectionStrings&gt;" außerhalb des &lt;Abschnitts "System. Web" in der Konfigurationsdatei angezeigt.&gt;

Wenn Sie diese Verbindungs Zeichenfolge verwenden möchten, können Sie die folgende Syntax verwenden, wenn Sie das ConnectionString-Attribut eines Server Steuer Elements festlegen.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

Der Abschnitt "&lt;connectionStrings&gt;" kann ebenfalls verschlüsselt werden, damit vertrauliche Informationen nicht verfügbar gemacht werden. Diese Fähigkeit wird in einem späteren Modul behandelt.

## <a name="caching-data-sources"></a>Zwischenspeichern von Datenquellen

Jede DataSourceControl stellt vier Eigenschaften zum Konfigurieren der Zwischenspeicherung bereit. EnableCaching, CacheDuration, CacheExpirationPolicy und cachekeyabhängigkeit.

## <a name="enablecaching"></a>EnableCaching

EnableCaching ist eine boolesche Eigenschaft, die bestimmt, ob das Zwischenspeichern für das Datenquellen-Steuerelement aktiviert ist.

## <a name="cacheduration-property"></a>CacheDuration (Eigenschaft)

Die CacheDuration-Eigenschaft legt die Anzahl von Sekunden fest, die der Cache gültig bleibt. Wenn diese Eigenschaft auf **0** festgelegt wird, bleibt der Cache gültig, bis er explizit für ungültig erklärt wird.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy (Eigenschaft)

Die CacheExpirationPolicy-Eigenschaft kann auf **absolut** oder **gleitend**festgelegt werden. Das Festlegen auf absolute bedeutet, dass die maximale Zeitspanne, für die die Daten zwischengespeichert werden, die in der CacheDuration-Eigenschaft angegebene Anzahl von Sekunden ist. Wenn Sie die Einstellung auf gleitenden Wert festlegen, wird die Ablaufzeit zurückgesetzt, wenn jeder Vorgang durchgeführt wird.

## <a name="cachekeydependency-property"></a>Cachekeydepengseigenschaft

Wenn ein Zeichen folgen Wert für die cachekeydepengseigenschaft angegeben wird, richtet ASP.net eine neue Cache Abhängigkeit basierend auf dieser Zeichenfolge ein. Auf diese Weise können Sie den Cache explizit für ungültig erklären, indem Sie einfach cachekeyabhängigkeit ändern oder entfernen.

**Wichtig**: Wenn der Identitätswechsel aktiviert ist und der Zugriff auf die Datenquelle und/oder den Inhalt von Daten auf der Client Identität basiert, empfiehlt es sich, das Caching durch Festlegen von "EnableCaching" auf "false" zu deaktivieren. Wenn das Caching in diesem Szenario aktiviert ist, und ein anderer Benutzer als der Benutzer, der die Daten ursprünglich angefordert hat, wird die Autorisierung für die Datenquelle nicht erzwungen. Die Daten werden einfach aus dem Cache bereitgestellt.

## <a name="the-sqldatasource-control"></a>Das SqlDataSource-Steuerelement

Das SqlDataSource-Steuerelement ermöglicht Entwicklern den Zugriff auf Daten, die in einer relationalen Datenbank gespeichert sind, die ADO.NET unterstützt. Der System. Data. SqlClient-Anbieter kann für den Zugriff auf eine SQL Server Datenbank, den System. Data. OleDb-Anbieter, den System. Data. ODBC-Anbieter oder den System. Data. OracleClient-Anbieter für den Zugriff auf Oracle verwendet werden. Daher wird die SqlDataSource sicherlich nicht nur für den Zugriff auf Daten in einer SQL Server Datenbank verwendet.

Um SqlDataSource zu verwenden, geben Sie einfach einen Wert für die ConnectionString-Eigenschaft an, und geben Sie einen SQL-Befehl oder eine gespeicherte Prozedur an. Das SqlDataSource-Steuerelement kümmert sich um die Arbeit mit der zugrunde liegenden ADO.NET-Architektur. Sie öffnet die Verbindung, fragt die Datenquelle ab oder führt die gespeicherte Prozedur aus, gibt die Daten zurück und schließt dann die Verbindung für Sie.

> [!NOTE]
> Da die DataSourceControl-Klasse die Verbindung automatisch für Sie schließt, sollte Sie die Anzahl von Kunden aufrufen verringern, die durch das Verlust von Datenbankverbindungen generiert werden.

Der folgende Code Ausschnitt bindet ein DropDownList-Steuerelement mithilfe der Verbindungs Zeichenfolge, die in der Konfigurationsdatei gespeichert ist, an ein SqlDataSource-Steuerelement, wie oben gezeigt.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Wie oben gezeigt, gibt die DataSourceMode-Eigenschaft von SqlDataSource den Modus für die Datenquelle an. Im obigen Beispiel wird DataSourceMode auf DataReader festgelegt. In diesem Fall gibt SqlDataSource ein IDataReader-Objekt zurück, indem ein Vorwärts Cursor und ein Schreib geschützter Cursor verwendet werden. Der angegebene Objekttyp, der zurückgegeben wird, wird vom verwendeten Anbieter gesteuert. In diesem Fall verwende ich den System. Data. SqlClient-Anbieter, wie im Abschnitt &lt;connectionStrings&gt; der Datei "Web. config" angegeben. Daher ist das zurückgegebene Objekt vom Typ SqlDataReader. Wenn Sie einen DataSourceMode-Wert von DataSet angeben, können die Daten in einem Dataset auf dem Server gespeichert werden. Mit diesem Modus können Sie Features wie Sortieren, Paging usw. hinzufügen. Wenn ich die Datenbindung von SqlDataSource an ein GridView-Steuerelement vorgenommen hätte, hätte ich den datasetmodus ausgewählt. Im Fall einer Dropdown List ist der DataReader-Modus jedoch die richtige Wahl.

> [!NOTE]
> Beim Zwischenspeichern einer SqlDataSource oder einer AccessDataSource muss die DataSourceMode-Eigenschaft auf DataSet festgelegt werden. Eine Ausnahme tritt auf, wenn Sie die Zwischenspeicherung mit einem DataSourceMode-DataReader aktivieren.

## <a name="sqldatasource-properties"></a>SqlDataSource-Eigenschaften

Im folgenden finden Sie einige der Eigenschaften des SqlDataSource-Steuer Elements.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Ein boolescher Wert, der angibt, ob ein SELECT-Befehl abgebrochen wird, wenn einer der Parameter NULL ist. Der Standardwert lautet „true“.

### <a name="conflictdetection"></a>ConflictDetection

In einer Situation, in der mehrere Benutzer eine Datenquelle gleichzeitig aktualisieren, bestimmt die Eigenschaft conflicterkennungs das Verhalten des SqlDataSource-Steuer Elements. Diese Eigenschaft wird zu einem der Werte der ConflictOptions-Enumeration ausgewertet. Diese Werte sind **CompareAllValues** und **overschreitechanges**. Wenn der Wert auf Overwrite-changes festgelegt ist, überschreibt die letzte Person, die Daten in die Datenquelle schreibt, alle vorherigen Änderungen. Wenn jedoch die Eigenschaft conflicterkennungs auf CompareAllValues festgelegt ist, werden Parameter für die Spalten erstellt, die von SelectCommand zurückgegeben werden, und die Parameter werden ebenfalls erstellt, um die ursprünglichen Werte in jeder dieser Spalten zu speichern, sodass SqlDataSource bestimmen Sie, ob die Werte seit der Ausführung von SelectCommand geändert wurden.

### <a name="deletecommand"></a>DeleteCommand

Ruft die beim Löschen von Zeilen aus der Datenbank verwendete SQL-Zeichenfolge ab oder legt diese fest. Dabei kann es sich entweder um eine SQL-Abfrage oder einen Namen einer gespeicherten Prozedur handeln.

### <a name="deletecommandtype"></a>DeleteCommandType

Legt den Typ des DELETE-Befehls fest, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Gibt die Parameter zurück, die vom DeleteCommand des SqlDataSourceView-Objekts verwendet werden, das dem SqlDataSource-Steuerelement zugeordnet ist.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Diese Eigenschaft wird verwendet, um das Format der ursprünglichen Wert Parameter in Fällen anzugeben, in denen die Eigenschaft conflicterkennungs auf CompareAllValues festgelegt ist. Der Standardwert ist {0} was bedeutet, dass die ursprünglichen Wert Parameter denselben Namen wie der ursprüngliche Parameter haben. Anders ausgedrückt: Wenn der Feldname Mitarbeiter-ID ist, wäre der ursprüngliche value-Parameter @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Legt die SQL-Zeichenfolge fest, die zum Abrufen von Daten aus der Datenbank verwendet wird. Dabei kann es sich entweder um eine SQL-Abfrage oder einen Namen einer gespeicherten Prozedur handeln.

### <a name="selectcommandtype"></a>SelectCommandType

Legt den Typ des Select-Befehls fest, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Gibt die Parameter zurück, die vom SelectCommand des SqlDataSourceView-Objekts verwendet werden, das dem SqlDataSource-Steuerelement zugeordnet ist.

### <a name="sortparametername"></a>SortParameterName

Ruft den Namen des Parameters einer gespeicherten Prozedur ab, der beim Sortieren von Daten verwendet wird, die vom Datenquellen Steuerelement abgerufen werden, oder legt diesen fest Nur gültig, wenn SelectCommandType auf StoredProcedure festgelegt ist.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Eine durch Semikolons getrennte Zeichenfolge, die die Datenbanken und Tabellen angibt, die in einer SQL Server Cache Abhängigkeit verwendet werden. (SQL-Cache Abhängigkeiten werden in einem späteren Modul erläutert.)

### <a name="updatecommand"></a>UpdateCommand

Legt die SQL-Zeichenfolge fest, die beim Aktualisieren von Daten in der Datenbank verwendet wird. Dabei kann es sich entweder um eine SQL-Abfrage oder einen Namen einer gespeicherten Prozedur handeln.

### <a name="updatecommandtype"></a>UpdateCommandType

Legt den Typ des Aktualisierungs Befehls fest, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Gibt die Parameter zurück, die vom UpdateCommand des SqlDataSourceView-Objekts verwendet werden, das dem SqlDataSource-Steuerelement zugeordnet ist.

## <a name="the-accessdatasource-control"></a>Das AccessDataSource-Steuerelement

Das AccessDataSource-Steuerelement wird von der SqlDataSource-Klasse abgeleitet und für die Datenbindung an eine Microsoft Access-Datenbank verwendet. Die ConnectionString-Eigenschaft für das AccessDataSource-Steuerelement ist eine schreibgeschützte Eigenschaft. Anstatt die ConnectionString-Eigenschaft zu verwenden, wird die DataFile-Eigenschaft verwendet, um auf die Access-Datenbank zu verweisen, wie unten gezeigt.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

Die AccessDataSource legt den ProviderName der Basis-SqlDataSource immer auf System. Data. OleDb fest und stellt mithilfe des Microsoft. Jet. OleDb. 4.0-OLE DB Anbieters eine Verbindung mit der Datenbank her. Sie können das AccessDataSource-Steuerelement nicht verwenden, um eine Verbindung mit einer Datenbank mit Kenn Wort geschütztem Zugriff herzustellen. Wenn Sie eine Verbindung mit einer Kenn Wort geschützten Datenbank herstellen müssen, sollten Sie das SqlDataSource-Steuerelement verwenden.

> [!NOTE]
> Access-Datenbanken, die auf der Website gespeichert werden, sollten im Datenverzeichnis der APP\_abgelegt werden. ASP.net lässt nicht zu, dass Dateien in diesem Verzeichnis durchsucht werden. Sie müssen dem Prozess Konto beim Verwenden von Access-Datenbanken Lese-und Schreibberechtigungen für das App\_-Datenverzeichnis erteilen.

## <a name="the-xmldatasource-control"></a>Das XmlDataSource-Steuerelement

Die XmlDataSource wird für die Datenbindung von XML-Daten an Daten gebundene Steuerelemente verwendet. Sie können mithilfe der DataFile-Eigenschaft an eine XML-Datei binden, oder Sie können mithilfe der Data-Eigenschaft eine Bindung an eine XML-Zeichenfolge herstellen. Die XmlDataSource macht XML-Attribute als bindbare Felder verfügbar. In Fällen, in denen Sie an Werte binden müssen, die nicht als Attribute dargestellt werden, müssen Sie eine XSL-Transformation verwenden. Sie können auch XPath-Ausdrücke verwenden, um XML-Daten zu filtern.

Beachten Sie die folgende XML-Datei:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Beachten Sie, dass XmlDataSource die XPath-Eigenschaft *Personen/Person* verwendet, um nur die &lt;Person&gt; Knoten zu filtern. Die Dropdown List-Eigenschaft bindet dann mithilfe der DataTextField-Eigenschaft an das LastName-Attribut.

Obwohl das XmlDataSource-Steuerelement hauptsächlich für die Datenbindung an schreibgeschützte XML-Daten verwendet wird, ist es möglich, die XML-Datendatei zu bearbeiten. Beachten Sie, dass in solchen Fällen das automatische einfügen, aktualisieren und Löschen von Informationen in der XML-Datei nicht automatisch erfolgt, wie dies bei anderen Datenquellen-Steuerelementen der Fall ist. Stattdessen müssen Sie Code schreiben, um die Daten mithilfe der folgenden Methoden des XmlDataSource-Steuer Elements manuell zu bearbeiten.

### <a name="getxmldocument"></a>GetXmlDocument

Ruft ein XmlDocument-Objekt ab, das den XML-Code enthält, der von der XmlDataSource abgerufen wurde.

### <a name="save"></a>Speichern

Speichert das in-Memory-XmlDocument wieder in der Datenquelle.

Es ist wichtig zu wissen, dass die Save-Methode nur funktioniert, wenn die folgenden beiden Bedingungen erfüllt sind:

1. XmlDataSource verwendet die DataFile-Eigenschaft zum Binden an eine XML-Datei anstelle der Data-Eigenschaft, um Sie an XML-Daten im Arbeitsspeicher zu binden.
2. Es wird keine Transformation über die Transform-oder TransformFile-Eigenschaft angegeben.

Beachten Sie auch, dass die Save-Methode unerwartete Ergebnisse erzielen kann, wenn Sie von mehreren Benutzern gleichzeitig aufgerufen wird.

## <a name="the-objectdatasource-control"></a>Das ObjectDataSource-Steuerelement

Die Datenquellen Steuerelemente, die wir bis zu diesem Punkt abgedeckt haben, sind hervorragende Optionen für Anwendungen mit zwei Ebenen, bei denen das Datenquellen-Steuerelement direkt mit dem Datenspeicher kommuniziert. Viele reale Anwendungen sind jedoch Anwendungen mit mehreren Ebenen, bei denen ein Datenquellen-Steuerelement möglicherweise mit einem Geschäftsobjekt kommunizieren muss, das wiederum mit der Datenschicht kommuniziert. In diesen Fällen füllt die ObjectDataSource die Rechnung gut aus. Die ObjectDataSource funktioniert in Verbindung mit einem-Quell Objekt. Das ObjectDataSource-Steuerelement erstellt eine Instanz des Quell Objekts, ruft die angegebene Methode auf und gibt die Objektinstanz innerhalb des Gültigkeits Bereichs einer einzelnen Anforderung frei, wenn das Objekt Instanzmethoden anstelle von statischen Methoden (die in Visual Basic freigegeben ist) enthält. Daher muss Ihr Objekt zustandslos sein. Das heißt, das Objekt sollte alle erforderlichen Ressourcen innerhalb der Spanne einer einzelnen Anforderung abrufen und freigeben. Sie können steuern, wie das Quell Objekt erstellt wird, indem Sie das ObjectCreating-Ereignis des ObjectDataSource-Steuer Elements verarbeiten. Sie können eine Instanz des Quell Objekts erstellen und dann die ObjectInstance-Eigenschaft der ObjectDataSourceEventArgs-Klasse auf diese Instanz festlegen. Das ObjectDataSource-Steuerelement verwendet die-Instanz, die im ObjectCreating-Ereignis erstellt wird, anstatt eine eigene Instanz zu erstellen.

Wenn das Quell Objekt für ein ObjectDataSource-Steuerelement öffentliche statische Methoden (Shared in Visual Basic) verfügbar macht, die aufgerufen werden können, um Daten abzurufen und zu ändern, Ruft ein ObjectDataSource-Steuerelement diese Methoden direkt auf. Wenn ein ObjectDataSource-Steuerelement eine Instanz des Quell Objekts erstellen muss, um Methodenaufrufe durchführen zu können, muss das Objekt einen öffentlichen Konstruktor enthalten, der keine Parameter annimmt. Das ObjectDataSource-Steuerelement ruft diesen Konstruktor auf, wenn eine neue Instanz des Quell Objekts erstellt wird.

Wenn das Quell Objekt keinen öffentlichen Konstruktor ohne Parameter enthält, können Sie eine Instanz des Quell Objekts erstellen, das vom ObjectDataSource-Steuerelement im ObjectCreating-Ereignis verwendet wird.

## <a name="specifying-object-methods"></a>Angeben von Objektmethoden

Das Quell Objekt für ein ObjectDataSource-Steuerelement kann eine beliebige Anzahl von Methoden enthalten, die zum auswählen, einfügen, aktualisieren oder Löschen von Daten verwendet werden. Diese Methoden werden vom ObjectDataSource-Steuerelement basierend auf dem Namen der Methode aufgerufen, wie in der SelectMethod, InsertMethod, UpdateMethod oder DeleteMethod-Eigenschaft des ObjectDataSource-Steuer Elements angegeben. Das Quell Objekt kann auch eine optionale SelectCount-Methode enthalten, die vom ObjectDataSource-Steuerelement mithilfe der selectzählmethod-Eigenschaft identifiziert wird, die die Gesamtzahl der Objekte in der Datenquelle zurückgibt. Das ObjectDataSource-Steuerelement ruft die SelectCount-Methode auf, nachdem eine Select-Methode aufgerufen wurde, um die Gesamtanzahl der Datensätze in der Datenquelle abzurufen, die beim Paging verwendet werden sollen.

## <a name="lab-using-data-source-controls"></a>Lab mithilfe von Datenquellen-Steuerelementen

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Übung 1: Anzeigen von Daten mit dem SqlDataSource-Steuerelement

In der folgenden Übung wird das SqlDataSource-Steuerelement verwendet, um eine Verbindung mit der Northwind-Datenbank herzustellen. Dabei wird davon ausgegangen, dass Sie Zugriff auf die Northwind-Datenbank in einer SQL Server 2000-Instanz haben.

1. Erstellen Sie eine neue ASP.NET-Website.
2. Fügen Sie eine neue Datei "Web. config" hinzu.

    1. Klicken Sie mit der rechten Maustaste auf das Projekt in Projektmappen-Explorer und dann auf Neues Element hinzufügen.
    2. Wählen Sie in der Liste der Vorlagen Webkonfigurationsdatei aus, und klicken Sie auf Hinzufügen
3. Bearbeiten Sie den &lt;connectionStrings-&gt; Abschnitt wie folgt: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Wechseln Sie zur Code Ansicht, und fügen Sie dem &lt;ASP: SqlDataSource-&gt; Steuerelement wie folgt ein ConnectionString-Attribut und ein SelectCommand-Attribut hinzu: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Fügen Sie in Designansicht ein neues GridView-Steuerelement hinzu.
6. Wählen Sie im Menü GridView-Aufgaben in der Dropdown Liste Datenquelle auswählen die Option SqlDataSource1 aus.
7. Klicken Sie mit der rechten Maustaste auf Default. aspx, und wählen Sie im Menü die Option in Browser anzeigen aus. Wenn Sie zum Speichern aufgefordert werden, klicken Sie auf Ja.
8. Die GridView zeigt die Daten aus der Tabelle "Products" an.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Übung 2: Bearbeiten von Daten mit dem SqlDataSource-Steuerelement

In der folgenden Übung wird veranschaulicht, wie ein Dropdown List-Steuerelement mithilfe der deklarativen Syntax an Daten gebunden wird und wie Sie die im DropDownList-Steuerelement dargestellten Daten bearbeiten können.

1. Löschen Sie in Designansicht das GridView-Steuerelement aus "default. aspx". 

    **Wichtig**: belassen Sie das SqlDataSource-Steuerelement auf der Seite.
2. Fügen Sie "default. aspx" ein DropDownList-Steuerelement hinzu.
3. Wechseln Sie zur Quell Ansicht.
4. Fügen Sie das Attribut "DataSourceID", "DataTextField" und "DataValueField" dem &lt;ASP: DropDownList-&gt; Steuerelement wie folgt hinzu: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Speichern Sie die Datei default. aspx, und zeigen Sie Sie im Browser an. Beachten Sie, dass DropDownList alle Produkte aus der Northwind-Datenbank enthält.
6. Schließen Sie den Browser.
7. Fügen Sie in der Quell Ansicht von default. aspx unter dem DropDownList-Steuerelement ein neues TextBox-Steuerelement hinzu. Ändern Sie die ID-Eigenschaft des Textfelds in txtproductname.
8. Fügen Sie unter dem TextBox-Steuerelement ein neues Schaltflächen-Steuerelement hinzu. Ändern Sie die ID-Eigenschaft der Schaltfläche in btnUpdate und die Text-Eigenschaft, um den **Produktnamen zu aktualisieren**.
9. Fügen Sie in der Quell Ansicht von "default. aspx" dem SqlDataSource-Tag wie folgt eine UpdateCommand-Eigenschaft und zwei neue UpdateParameters hinzu: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Beachten Sie, dass dieser Code zwei Update Parameter (ProductName und ProductID) enthält. Diese Parameter werden der Text-Eigenschaft des Textfelds txtproductname und der SelectedValue-Eigenschaft der Dropdown Liste ddlproducts zugeordnet.
10. Wechseln Sie zu Designansicht, und doppelklicken Sie auf das Schaltflächen-Steuerelement, um einen Ereignishandler hinzuzufügen.
11. Fügen Sie den folgenden Code zu btnUpdate hinzu,\_klicken Sie auf Code: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Klicken Sie mit der rechten Maustaste auf Default. aspx, und wählen Sie die Anzeige im Browser aus. Klicken Sie auf Ja, wenn Sie zum Speichern aller Änderungen aufgefordert werden.
13. ASP.NET 2,0 partielle Klassen ermöglichen die Kompilierung zur Laufzeit. Es ist nicht erforderlich, eine Anwendung zu erstellen, um zu sehen, dass Codeänderungen wirksam werden.
14. Wählen Sie ein Produkt aus der Dropdown Liste aus.
15. Geben Sie einen neuen Namen für das ausgewählte Produkt in das Textfeld ein, und klicken Sie dann auf die Schaltfläche Aktualisieren.
16. Der Produktname wird in der Datenbank aktualisiert.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Übung 3 mit dem ObjectDataSource-Steuerelement

In dieser Übung wird veranschaulicht, wie das ObjectDataSource-Steuerelement und ein Quell Objekt für die Interaktion mit der Northwind-Datenbank verwendet werden.

1. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie auf Neues Element hinzufügen.
2. Wählen Sie Web Form in der Liste Vorlagen aus. Ändern Sie den Namen in Object. aspx, und klicken Sie auf hinzufügen.
3. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie auf Neues Element hinzufügen.
4. Wählen Sie in der Liste Vorlagen die Option Klasse aus. Ändern Sie den Namen der Klasse in NorthwindData.cs, und klicken Sie auf hinzufügen.
5. Klicken Sie auf Ja, wenn Sie dazu aufgefordert werden, die Klasse dem App-\_Code Ordner hinzuzufügen.
6. Fügen Sie der Datei NorthwindData.cs den folgenden Code hinzu: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Fügen Sie der Quell Ansicht von Object. aspx den folgenden Code hinzu: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Speichern Sie alle Dateien, und Durchsuchen Sie Object. aspx.
9. Interagieren Sie mit der-Schnittstelle, indem Sie Details anzeigen, Mitarbeiter bearbeiten, Mitarbeiter hinzufügen und Mitarbeiter löschen.
