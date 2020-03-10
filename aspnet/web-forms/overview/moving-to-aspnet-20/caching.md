---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Caching | Microsoft-Dokumentation
author: microsoft
description: Ein Verständnis der Zwischenspeicherung ist wichtig für eine leistungsstarke ASP.NET-Anwendung. ASP.NET 1. x bietet drei verschiedene Optionen für die Zwischenspeicherung. Ausgabe Zwischenspeicherung,...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f0b021ca6ca151544dd9fb0587ed9e0cf14ff65
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464955"
---
# <a name="caching"></a>Caching

von [Microsoft](https://github.com/microsoft)

> Ein Verständnis der Zwischenspeicherung ist wichtig für eine leistungsstarke ASP.NET-Anwendung. ASP.NET 1. x bietet drei verschiedene Optionen für die Zwischenspeicherung. Ausgabe Zwischenspeicherung, FragmentCaching und Cache-API.

Ein Verständnis der Zwischenspeicherung ist wichtig für eine leistungsstarke ASP.NET-Anwendung. ASP.NET 1. x bietet drei verschiedene Optionen für die Zwischenspeicherung. Ausgabe Zwischenspeicherung, FragmentCaching und Cache-API. ASP.NET 2,0 bietet alle drei Methoden, bietet jedoch einige bedeutende zusätzliche Features. Es gibt mehrere neue Cache Abhängigkeiten. Entwickler haben jetzt auch die Möglichkeit, benutzerdefinierte Cache Abhängigkeiten zu erstellen. Die Konfiguration der Zwischenspeicherung wurde in ASP.NET 2,0 ebenfalls erheblich verbessert.

## <a name="new-features"></a>Neue Funktionen

## <a name="cache-profiles"></a>Cache profile

Mit Cache Profilen können Entwickler bestimmte Cache Einstellungen definieren, die dann auf einzelne Seiten angewendet werden können. Wenn Sie z. b. einige Seiten haben, die nach 12 Stunden aus dem Cache ablaufen sollen, können Sie problemlos ein Cache Profil erstellen, das auf diese Seiten angewendet werden kann. Zum Hinzufügen eines neuen Cache Profils verwenden Sie den Abschnitt &lt;outputCacheSettings&gt; in der Konfigurationsdatei. Im folgenden finden Sie beispielsweise die Konfiguration eines Cache Profils namens *twoday* , mit dem eine Cache Dauer von 12 Stunden konfiguriert wird.

[!code-xml[Main](caching/samples/sample1.xml)]

Um dieses Cache Profil auf eine bestimmte Seite anzuwenden, verwenden Sie das CacheProfile-Attribut der @ OutputCache-Direktive, wie unten dargestellt:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Benutzerdefinierte Cache Abhängigkeiten

ASP.NET 1. x-Entwickler riefen für benutzerdefinierte Cache Abhängigkeiten auf. In ASP.NET 1. x war die cachedepensklasse versiegelt, wodurch Entwickler daran gehindert wurden, ihre eigenen Klassen davon abzuleiten. In ASP.NET 2,0 wird diese Einschränkung entfernt, und Entwickler können eigene benutzerdefinierte Cache Abhängigkeiten entwickeln. Die cacheabhängigkeits-Klasse ermöglicht das Erstellen einer benutzerdefinierten Cache Abhängigkeit auf der Grundlage von Dateien, Verzeichnissen oder Cache Schlüsseln.

Der folgende Code erstellt z. b. eine neue benutzerdefinierte Cache Abhängigkeit auf der Grundlage einer Datei namens "stuff. xml" im Stammverzeichnis der Webanwendung:

[!code-csharp[Main](caching/samples/sample3.cs)]

In diesem Szenario wird das zwischengespeicherte Element für ungültig erklärt, wenn sich die Datei "stuff. xml" ändert.

Es ist auch möglich, eine benutzerdefinierte Cache Abhängigkeit mithilfe von Cache Schlüsseln zu erstellen. Wenn diese Methode verwendet wird, werden die zwischengespeicherten Daten durch das Entfernen des Cache Schlüssels ungültig. Dies wird anhand des folgenden Beispiels veranschaulicht:

[!code-csharp[Main](caching/samples/sample4.cs)]

Um das oben eingefügte Element für ungültig zu erklären, entfernen Sie einfach das in den Cache eingefügte Element, das als Cache Schlüssel fungiert.

[!code-csharp[Main](caching/samples/sample5.cs)]

Beachten Sie, dass der Schlüssel des Elements, das als Cache Schlüssel fungiert, mit dem Wert identisch sein muss, der dem Array von Cache Schlüsseln hinzugefügt wurde.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Abruf basierte SQL-Cache Abhängigkeiten (auch als Tabellen basierte Abhängigkeiten bezeichnet)

SQL Server 7 und 2000 verwenden Sie das Abruf basierte Modell für SQL-Cache Abhängigkeiten. Das Abruf basierte Modell verwendet einen Trigger für eine Datenbanktabelle, die ausgelöst wird, wenn sich Daten in der Tabelle ändern. Dieser auslöst aktualisiert ein **changeid-** Feld in der Benachrichtigungs Tabelle, das ASP.net regelmäßig überprüft. Wenn das **changeid-** Feld aktualisiert wurde, weiß ASP.net, dass sich die Daten geändert haben, und die zwischengespeicherten Daten werden für ungültig erklärt.

> [!NOTE]
> SQL Server 2005 kann auch das Abruf basierte Modell verwenden, aber da das Abruf basierte Modell nicht das effizienteste Modell ist, empfiehlt es sich, ein Abfrage basiertes Modell (später erläutert) mit SQL Server 2005 zu verwenden.

Damit eine SQL-Cache Abhängigkeit, die das Abruf basierte Modell verwendet, ordnungsgemäß funktioniert, müssen die-Tabellen Benachrichtigungen aktiviert haben. Dies kann Programm gesteuert mithilfe der SqlCacheDependencyAdmin-Klasse oder mithilfe des ASPNET\_RegSql. exe-Hilfsprogramms erreicht werden.

Mit der folgenden Befehlszeile wird die Products-Tabelle in der Northwind-Datenbank registriert, die sich auf einer SQL Server Instanz namens *dBASE* für die SQL-Cache Abhängigkeit befindet.

[!code-console[Main](caching/samples/sample6.cmd)]

Im folgenden finden Sie eine Erläuterung der Befehls Zeilenschalter, die im obigen Befehl verwendet werden:

| **Befehls Zeilenschalter** | **Zweck** |
| --- | --- |
| -S *server* | Gibt den Servernamen an. |
| -Ed | Gibt an, dass die Datenbank für die SQL-Cache Abhängigkeit aktiviert werden soll. |
| -d *Datenbank\_Name* | Gibt den Datenbanknamen an, der für die SQL-Cache Abhängigkeit aktiviert werden soll. |
| -E | Gibt an, dass ASPNET\_RegSql beim Herstellen einer Verbindung mit der Datenbank die Windows-Authentifizierung verwenden soll. |
| -et | Gibt an, dass eine Datenbanktabelle für die SQL-Cache Abhängigkeit aktiviert wird. |
| -t *\_Name der Tabelle* | Gibt den Namen der Datenbanktabelle an, die für die SQL-Cache Abhängigkeit aktiviert werden soll. |

> [!NOTE]
> Es stehen weitere Switches für ASPNET\_RegSql. exe zur Verfügung. Eine komplette Liste erhalten Sie, wenn Sie ASPNET\_RegSql. exe ausführen: von der Befehlszeile aus.

Wenn dieser Befehl ausgeführt wird, werden die folgenden Änderungen an der SQL Server Datenbank vorgenommen:

- Eine **ASPNET-\_sqlcachetablesforchangenotifizierungstabelle** wird hinzugefügt. Diese Tabelle enthält eine Zeile für jede Tabelle in der Datenbank, für die eine SQL-Cache Abhängigkeit aktiviert wurde.
- Die folgenden gespeicherten Prozeduren werden in der-Datenbank erstellt:

| ASPNET\_sqlcachepollingstoredprocedure | Fragt die ASPNET-\_Tabelle "sqlcachetablesforchangenotifitifitifitifitifitifitifitifitifitifitifitifitifitifi" ab und gibt alle Tabellen zurück, die für die SQL-Cache Abhängigkeit aktiviert sind Diese gespeicherte Prozedur wird für Abfragen verwendet, um zu bestimmen, ob sich die Daten geändert haben. |
| --- | --- |
| ASPNET\_sqlcachequeryregisteredtablesstoredprocedure | Gibt alle für die SQL-Cache Abhängigkeit aktivierten Tabellen zurück, indem die ASPNET-\_sqlcachetablesforchangenotifizierungstabelle abgefragt wird und alle für SQL-Cache Abhängigkeit aktivierten Tabellen zurückgegeben werden. |
| ASPNET\_sqlcacheregistertablestoredprocedure | Registriert eine Tabelle für die SQL-Cache Abhängigkeit, indem der erforderliche Eintrag in der Benachrichtigungs Tabelle hinzugefügt und der-Cache hinzugefügt wird. |
| ASPNET\_sqlcacheunregistertablestoredprocedure | Hebt die Registrierung einer Tabelle für die SQL-Cache Abhängigkeit auf, indem der Eintrag in der Benachrichtigungs Tabelle entfernt und der-Cache entfernt wird. |
| ASPNET\_sqlcacheupdatechangeidstoredprocedure | Aktualisiert die Benachrichtigungs Tabelle durch Erhöhen der changeid für die geänderte Tabelle. ASP.NET verwendet diesen Wert, um zu bestimmen, ob sich die Daten geändert haben. Wie unten angegeben, wird diese gespeicherte Prozedur von dem beim Aktivieren der Tabelle erstellten-ausgelöst. |

- Für die Tabelle wird ein SQL Server-  **_\_namens Table Name_\_ASPNET\_sqlcachenogations\_ausgelöst** . Dieser Triggers führt die ASPNET-\_sqlcacheupdatechangeidstoredprocedure aus, wenn eine INSERT-, Update-oder DELETE-Anweisung für die Tabelle ausgeführt wird.
- Der Datenbank wird eine SQL Server Rolle namens **ASPNET\_changenozierung\_receivenotificationsonlyaccess** hinzugefügt.

Die **ASPNET-\_changenotifizierung\_receivenotificationsonlyaccess** SQL Server Rolle verfügt über exec-Berechtigungen für die ASPNET-\_sqlcachepollingstoredprocedure. Damit das Abruf Modell ordnungsgemäß funktioniert, müssen Sie das Prozess Konto der Rolle "ASPNET\_changenotifizierung\_receivenotificationsonlyaccess" hinzufügen. Das Tool ASPNET\_RegSql. exe führt dies nicht für Sie aus.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Konfigurieren von Abruf basierten SQL-Cache Abhängigkeiten

Zum Konfigurieren von Abruf basierten SQL-Cache Abhängigkeiten sind mehrere Schritte erforderlich. Der erste Schritt besteht darin, die Datenbank und die Tabelle wie oben beschrieben zu aktivieren. Nachdem dieser Schritt ausgeführt wurde, sieht der Rest der Konfiguration wie folgt aus:

- Konfigurieren der ASP.NET-Konfigurationsdatei.
- Konfigurieren von sqlcacheabhängigkeiten

### <a name="configuring-the-aspnet-configuration-file"></a>Konfigurieren der ASP.NET-Konfigurationsdatei

Zusätzlich zum Hinzufügen einer Verbindungs Zeichenfolge, wie in einem vorherigen Modul erläutert, müssen Sie auch ein &lt;Cache&gt;-Element mit einem &lt;sqlcachedepen&gt;-Element konfigurieren, wie unten dargestellt:

[!code-xml[Main](caching/samples/sample7.xml)]

Diese Konfiguration ermöglicht eine SQL-Cache Abhängigkeit von der *Pubs* -Datenbank. Beachten Sie, dass das pollTime-Attribut im &lt;sqlcachedepen&gt;-Element standardmäßig 60000 Millisekunden oder 1 Minute beträgt. (Dieser Wert darf nicht kleiner als 500 Millisekunden sein.) In diesem Beispiel fügt das &lt;Add&gt;-Elements eine neue Datenbank hinzu und überschreibt die pollTime-Einstellung auf 9 Millionen Millisekunden.

#### <a name="configuring-the-sqlcachedependency"></a>Konfigurieren von sqlcacheabhängigkeiten

Der nächste Schritt ist das Konfigurieren von sqlcacheabhängigkeit. Die einfachste Möglichkeit, dies zu erreichen, besteht darin, den Wert für das sqldirective-Attribut in der @ outcache-Direktive wie folgt anzugeben:

[!code-aspx[Main](caching/samples/sample8.aspx)]

In der obigen @ OutputCache-Direktive wird eine SQL-Cache Abhängigkeit für die *Autoren* Tabelle in der *Pubs* -Datenbank konfiguriert. Mehrere Abhängigkeiten können konfiguriert werden, indem Sie durch ein Semikolon wie folgt getrennt werden:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Eine andere Methode zum Konfigurieren von sqlcacheabhängigkeiten besteht darin, dies Programm gesteuert zu tun. Mit dem folgenden Code wird eine neue SQL-Cache Abhängigkeit von der Tabelle " *Authors* " in der *Pubs* -Datenbank erstellt.

[!code-csharp[Main](caching/samples/sample10.cs)]

Einer der Vorteile der programmgesteuerten Definition der SQL-Cache Abhängigkeit ist, dass Sie alle auftretenden Ausnahmen behandeln können. Wenn Sie z. b. versuchen, eine SQL-Cache Abhängigkeit für eine Datenbank zu definieren, die nicht für eine Benachrichtigung aktiviert wurde, wird eine **DatabaseNotEnabledForNotificationException** -Ausnahme ausgelöst. In diesem Fall können Sie versuchen, die Datenbank für Benachrichtigungen zu aktivieren, indem Sie die **SqlCacheDependencyAdmin. EnableNotifications** -Methode aufrufen und ihr den Datenbanknamen übergeben.

Wenn Sie versuchen, eine SQL-Cache Abhängigkeit für eine Tabelle zu definieren, die nicht für eine Benachrichtigung aktiviert wurde, wird eine **TableNotEnabledForNotificationException** ausgelöst. Sie können dann die Methode **SqlCacheDependencyAdmin. enabletablefornotification** aufzurufen, indem Sie den Datenbanknamen und den Tabellennamen übergeben.

Im folgenden Codebeispiel wird veranschaulicht, wie die Ausnahmebehandlung beim Konfigurieren einer SQL-Cache Abhängigkeit ordnungsgemäß konfiguriert wird.

[!code-csharp[Main](caching/samples/sample11.cs)]

Weitere Informationen: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Abfrage basierte SQL-Cache Abhängigkeiten (nur SQL Server 2005)

Wenn Sie SQL Server 2005 für die SQL-Cache Abhängigkeit verwenden, ist das Abruf basierte Modell nicht erforderlich. Bei Verwendung mit SQL Server 2005 kommunizieren SQL-Cache Abhängigkeiten direkt über SQL-Verbindungen mit der SQL Server Instanz (es ist keine weitere Konfiguration erforderlich) mithilfe von SQL Server 2005-Abfrage Benachrichtigungen.

Die einfachste Möglichkeit, Abfrage basierte Benachrichtigungen zu aktivieren, besteht darin, die deklarative Durchsetzung des **sqlcachedepen-Attributs** des Datenquellen Objekts auf **CommandNotification** zu aktivieren und das Attribut **EnableCaching** auf **true**festzulegen. Mit dieser Methode ist kein Code erforderlich. Wenn sich das Ergebnis eines Befehls, der für die Datenquelle ausgeführt wird, ändert, werden die Cache Daten ungültig.

Im folgenden Beispiel wird ein Datenquellen-Steuerelement für die SQL-Cache Abhängigkeit konfiguriert:

[!code-aspx[Main](caching/samples/sample12.aspx)]

Wenn in diesem Fall die im **SelectCommand** angegebene Abfrage ein anderes Ergebnis als ursprünglich zurückgibt, werden die zwischengespeicherten Ergebnisse für ungültig erklärt.

Sie können auch angeben, dass alle Datenquellen für SQL-Cache Abhängigkeiten aktiviert werden sollen, indem Sie das **sqlqueue** -Attribut der **@ OutputCache** -Direktive auf **CommandNotification**festlegen. Dies wird im folgenden Beispiel veranschaulicht.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Weitere Informationen zu Abfrage Benachrichtigungen in SQL Server 2005 finden Sie unter SQL Server-Onlinedokumentation.

Eine weitere Methode zum Konfigurieren einer Abfrage basierten SQL-Cache Abhängigkeit ist die programmgesteuerte Verwendung der sqlcachedepensklasse. Im folgenden Codebeispiel wird veranschaulicht, wie dies erreicht wird.

[!code-csharp[Main](caching/samples/sample14.cs)]

Weitere Informationen: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Ersetzung nach dem Cache

Das Zwischenspeichern einer Seite kann die Leistung einer Webanwendung erheblich steigern. In einigen Fällen ist es jedoch erforderlich, dass der größte Teil der Seite zwischengespeichert wird und einige Fragmente auf der Seite dynamisch sind. Wenn Sie z. b. eine Seite mit Nachrichten Geschichten erstellen, die für festgelegte Zeiträume vollständig statisch ist, können Sie festlegen, dass die gesamte Seite zwischengespeichert wird. Wenn Sie ein gedrehtes Werbebanner einschließen möchten, das bei jeder Seiten Anforderung geändert wurde, muss der Teil der Seite, der die Ankündigung enthält, dynamisch sein. Um das Zwischenspeichern einer Seite, aber das dynamische Ersetzen von Inhalten zu ermöglichen, können Sie die ASP.net-Ersetzung nach dem Cache verwenden. Bei der Ersetzung nach dem Cache wird die gesamte Seite als Ausgabe zwischengespeichert, wobei bestimmte Teile als vom Caching ausgenommen gekennzeichnet sind. Im Beispiel der Werbebanner ermöglicht Ihnen das AdRotator-Steuerelement, die Ersetzung nach dem Cache zu nutzen, sodass für jeden Benutzer und jede Seiten Aktualisierung dynamische Werbeeinblendungen erstellt werden.

Es gibt drei Möglichkeiten, die Ersetzung nach dem Cache zu implementieren:

- Deklarativ, mithilfe des Steuer Elements Ersetzung.
- Programm gesteuert mit der Steuerelement-API für die Ersetzung.
- Implizit, mithilfe des AdRotator-Steuer Elements.

### <a name="substitution-control"></a>Ersetzung-Steuerelement

Das ASP.net Substitution-Steuerelement gibt einen Abschnitt einer zwischengespeicherten Seite an, der dynamisch erstellt wird, anstatt zwischengespeichert zu werden. Sie platzieren ein Ersetzungs Steuerelement an der Position auf der Seite, auf der der dynamische Inhalt angezeigt werden soll. Zur Laufzeit ruft das Steuerelement Ersetzung eine Methode auf, die Sie mit der MethodName-Eigenschaft angeben. Die Methode muss eine Zeichenfolge zurückgeben, die dann den Inhalt des Ersetzungs Steuer Elements ersetzt. Die-Methode muss eine statische Methode auf der enthaltenden Seite oder dem UserControl-Steuerelement sein. Die Verwendung des Ersetzungs Steuer Elements bewirkt, dass die Client seitige Cache Fähigkeit in Server Cache Fähigkeit geändert wird, sodass die Seite nicht auf dem Client zwischengespeichert wird. Dadurch wird sichergestellt, dass in zukünftigen Anforderungen der Seite die-Methode erneut aufgerufen wird, um dynamischen Inhalt zu generieren.

### <a name="substitution-api"></a>Ersetzungs-API

Um dynamischen Inhalt für eine zwischengespeicherte Seite Programm gesteuert zu erstellen, können Sie die Methode " [Write tesubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) " in Ihrem Seitencode aufrufen und dabei den Namen einer Methode als Parameter übergeben. Die Methode, die die Erstellung des dynamischen Inhalts behandelt, nimmt einen einzelnen [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) -Parameter an und gibt eine Zeichenfolge zurück. Die Rückgabe Zeichenfolge ist der Inhalt, der an der angegebenen Position ersetzt wird. Ein Vorteil des Aufrufs der Methode "Write tesubstitution" anstelle der deklarativen Verwendung des Ersetzungs Steuer Elements besteht darin, dass Sie eine Methode eines beliebigen Objekts aufrufen können, anstatt eine statische Methode der Seite oder des UserControl-Objekts aufzurufen.

Das Aufrufen der Methode "Write tesubstitution" bewirkt, dass die Client seitige Cache Fähigkeit in Server Cache Fähigkeit geändert wird, sodass die Seite nicht auf dem Client zwischengespeichert wird. Dadurch wird sichergestellt, dass in zukünftigen Anforderungen der Seite die-Methode erneut aufgerufen wird, um dynamischen Inhalt zu generieren.

### <a name="adrotator-control"></a>AdRotator-Steuerelement

Das Server Steuerelement AdRotator implementiert intern die Unterstützung für die Ersetzung nach dem Cache. Wenn Sie ein AdRotator-Steuerelement auf Ihrer Seite platzieren, werden für jede Anforderung eindeutige Ankündigungen angezeigt, unabhängig davon, ob die übergeordnete Seite zwischengespeichert wird. Folglich wird eine Seite, die ein AdRotator-Steuerelement enthält, nur serverseitig zwischengespeichert.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy-Klasse

Die ControlCachePolicy-Klasse ermöglicht die programmgesteuerte Steuerung des Fragmentcaches mithilfe von Benutzer Steuerelementen. ASP.net bettet Benutzer Steuerelemente in einer [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) -Instanz ein. Die BasePartialCachingControl-Klasse stellt ein Benutzer Steuerelement dar, für das das Ausgabe Caching aktiviert ist.

Wenn Sie auf die [BasePartialCachingControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) -Eigenschaft eines [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) -Steuer Elements zugreifen, erhalten Sie immer ein gültiges ControlCachePolicy-Objekt. Wenn Sie jedoch auf die [UserControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) -Eigenschaft eines [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) -Steuer Elements zugreifen, erhalten Sie nur dann ein gültiges ControlCachePolicy-Objekt, wenn das Benutzer Steuerelement bereits von einem BasePartialCachingControl-Steuerelement umschließt ist. Wenn Sie nicht umschließt ist, löst das von der-Eigenschaft zurückgegebene ControlCachePolicy-Objekt Ausnahmen aus, wenn Sie versuchen, Sie zu bearbeiten, da ihr kein BasePartialCachingControl-Element zugeordnet ist. Überprüfen Sie die [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) -Eigenschaft, um zu bestimmen, ob eine UserControl-Instanz Caching ohne das Erzeugen von Ausnahmen unterstützt

Die Verwendung der ControlCachePolicy-Klasse ist eine von mehreren Möglichkeiten zum Aktivieren der Ausgabe Zwischenspeicherung. In der folgenden Liste werden Methoden beschrieben, mit denen Sie die Ausgabe Zwischenspeicherung aktivieren können:

- Verwenden Sie die [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) -Direktive, um die Ausgabe Zwischenspeicherung in deklarativen Szenarien zu aktivieren.
- Verwenden Sie das [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) -Attribut, um das Zwischenspeichern für ein Benutzer Steuerelement in einer Code Behind-Datei zu aktivieren.
- Verwenden Sie die ControlCachePolicy-Klasse, um Cache Einstellungen in programmgesteuerten Szenarien anzugeben, in denen Sie mit den BasePartialCachingControl-Instanzen arbeiten, die mithilfe einer der vorherigen Methoden zwischengespeichert und mithilfe der [System. Web. UI. TemplateControl. LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) -Methode dynamisch geladen wurden.

Eine ControlCachePolicy-Instanz kann nur zwischen den init-und PreRender-Phasen des Steuerungs Lebenszyklus geändert werden. Wenn Sie ein ControlCachePolicy-Objekt nach der PreRender-Phase ändern, löst ASP.net eine Ausnahme aus, da alle Änderungen, die nach dem Rendern des Steuer Elements vorgenommen werden, die Cache Einstellungen nicht beeinflussen können (ein Steuerelement wird während der Renderingphase zwischengespeichert Schließlich ist eine Benutzer Steuerelement Instanz (und damit auch Ihr ControlCachePolicy-Objekt) nur für die programmgesteuerte Bearbeitung verfügbar, wenn Sie tatsächlich gerendert wird.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Änderungen an der cachingkonfiguration-das &lt;Caching&gt;-Element

Es gibt mehrere Änderungen an der cachingkonfiguration in ASP.NET 2,0. Das &lt;Caching&gt;-Elements ist neu in ASP.NET 2,0 und ermöglicht es Ihnen, Änderungen an der Cache Konfiguration in der Konfigurationsdatei vorzunehmen. Die folgenden Attribute sind verfügbar.

| **Element** | **Beschreibung** |
| --- | --- |
| **Speichern** | Optionales Element. Definiert globale Anwendungscache Einstellungen. |
| **OutputCache** | Optionales Element. Gibt Anwendungs weite Ausgabe Cache Einstellungen an. |
| **outputCacheSettings** | Optionales Element. Gibt Ausgabe Cache Einstellungen an, die auf Seiten in der Anwendung angewendet werden können. |
| **SqlCacheDependency** | Optionales Element. Konfiguriert die SQL-Cacheabhängigkeiten für eine ASP.NET-Anwendung. |

### <a name="the-ltcachegt-element"></a>Das &lt;Cache&gt; Element

Die folgenden Attribute sind im &lt;Cache&gt;-Elements verfügbar:

| **Attribut** | **Beschreibung** |
| --- | --- |
| **DisableMemoryCollection** | Optionales **Boolean** -Attribut. Ruft einen Wert ab oder legt einen Wert fest, der angibt, ob die Cache Speicher Auflistung deaktiviert ist, wenn der Computer nicht über genügend Arbeitsspeicher verfügt. |
| **disableablauf** | Optionales **Boolean** -Attribut. Ruft einen Wert ab, der angibt, ob die Cache Ablaufzeit deaktiviert ist Wenn diese Option deaktiviert ist, laufen die zwischengespeicherten Elemente nicht ab, und es erfolgt keine Hintergrund Löschung abgelaufener Cache Elemente. |
| **PrivateBytesLimit** | Optionales **Int64** -Attribut. Ruft einen Wert ab oder legt einen Wert fest, der die maximale Größe der privaten Bytes einer Anwendung angibt, bevor der Cache abgelaufene Elemente geleert und versucht, Arbeitsspeicher freizugeben. Dieser Grenzwert schließt sowohl den vom Cache genutzten Speicher als auch den normalen Arbeitsspeicher Aufwand aus der laufenden Anwendung ein. Eine Einstellung von NULL gibt an, dass ASP.net eine eigene Heuristik verwendet, um zu bestimmen, wann der Speicher freigegeben werden soll. |
| **"prozagephysicalmemoryusedlimit"** | Optionales **Int32** -Attribut. Ruft einen Wert ab oder legt einen Wert fest, der den maximalen Prozentsatz des physischen Speichers eines Computers angibt, der von einer Anwendung genutzt werden kann, bevor der Cache abgelaufene Elemente geleert und versucht, Arbeitsspeicher freizugeben. diese Speicherauslastung enthält sowohl den vom Cache verwendeten Speicher. als normale Speicherauslastung der laufenden Anwendung. Eine Einstellung von NULL gibt an, dass ASP.net eine eigene Heuristik verwendet, um zu bestimmen, wann der Speicher freigegeben werden soll. |
| **PrivateBytesPollTime** | Optionales **TimeSpan** -Attribut. Ruft einen Wert ab oder legt einen Wert fest, der das Zeitintervall zwischen dem Abrufen der Arbeitsspeicher Auslastung der privaten Bytes der Anwendung angibt. |

### <a name="the-ltoutputcachegt-element"></a>Das &lt;OutputCache-&gt; Element

Die folgenden Attribute sind für das &lt;OutputCache-&gt;-Element verfügbar.

|       <strong>Attribut</strong>        |                                                                                                                                                                                                                                                       <strong>Beschreibung</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>EnableOutputCache</strong>    |                                                                                                                                                          Optionales <strong>Boolean</strong> -Attribut. Aktiviert bzw. deaktiviert den Seiten Ausgabe Cache. Wenn diese Option deaktiviert ist, werden keine Seiten unabhängig von den programmgesteuerten oder deklarativen Einstellungen zwischengespeichert. Der Standardwert lautet <strong>true</strong>.                                                                                                                                                           |
|  <strong>EnableFragmentCache</strong>   |                                                Optionales <strong>Boolean</strong> -Attribut. Aktiviert bzw. deaktiviert den Cache des Anwendungs Fragments. Wenn diese Option deaktiviert ist, werden unabhängig von der verwendeten [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) -Direktive oder dem verwendeten Cache Profil keine Seiten zwischengespeichert. Schließt einen Cache-Control-Header ein, der angibt, dass Upstreamproxyserver und Browser Clients nicht versuchen sollten, die Seiten Ausgabe zwischenzuspeichern. Der Standardwert ist <strong>false</strong>.                                                 |
| <strong>SendCacheControlHeader</strong> |                                                                                                                                                      Optionales <strong>Boolean</strong> -Attribut. Ruft einen Wert ab, der angibt, ob der Header " <strong>Cache-Control: Private</strong> " standardmäßig vom Ausgabe Cache Modul gesendet wird, oder legt diesen fest. Der Standardwert ist <strong>false</strong>.                                                                                                                                                      |
|      <strong>OmitVaryStar</strong>      | Optionales <strong>Boolean</strong> -Attribut. Aktiviert bzw. deaktiviert das Senden eines HTTP-Headers "<strong>Vary: \</strong ><em>" in der Antwort. Mit der Standardeinstellung false wird ein "</em>* Vary: \*<strong>"-Header für Ausgabe zwischengespeicherte Seiten gesendet. Wenn der Vary-Header gesendet wird, können unterschiedliche Versionen basierend auf dem im Vary-Header angegebenen Cache zwischengespeichert werden. Beispielsweise <em>variieren: Benutzer-Agents</em> speichern verschiedene Versionen einer Seite auf der Grundlage des Benutzer-Agents, der die Anforderung ausgibt. Der Standardwert ist * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Das &lt;outputCacheSettings-&gt; Element

Das &lt;outputCacheSettings-&gt; Element ermöglicht die Erstellung von Cache Profilen, wie zuvor beschrieben. Das einzige untergeordnete Element für das &lt;outputCacheSettings-&gt; Element ist das &lt;outputCacheProfiles&gt;-Element für die Konfiguration von Cache Profilen.

### <a name="the-ltsqlcachedependencygt-element"></a>Das &lt;sqlcachedepen&gt;-Element

Die folgenden Attribute sind für das &lt;sqlcachedepen&gt;-Element verfügbar.

| **Attribut** | **Beschreibung** |
| --- | --- |
| **enabled** | Erforderliches **Boolesches** Attribut. Gibt an, ob Änderungen abgerufen werden. |
| **PollTime** | Optionales **Int32** -Attribut. Legt die Häufigkeit fest, mit der sqlcacheabhängigkeiten die Datenbanktabelle auf Änderungen abfragt. Dieser Wert entspricht der Anzahl von Millisekunden zwischen aufeinander folgenden pollingen. Der Wert kann nicht auf weniger als 500 Millisekunden festgelegt werden. Der Standardwert ist 1 Minute. |

### <a name="more-information"></a>Weitere Informationen

Es gibt einige zusätzliche Informationen, die Sie hinsichtlich der Cache Konfiguration beachten sollten.

- Wenn der Grenzwert für private Bytes des Arbeitsprozesses nicht festgelegt ist, verwendet der Cache eine der folgenden Einschränkungen: 

    - x86 2 GB: 800mb oder 60% physischer RAM, je nachdem, welcher Wert kleiner ist
    - x86 3 GB: 1800 MB oder 60% physischer RAM, je nachdem, welcher Wert kleiner ist
    - x64:1 Terabyte oder 60% physischer RAM, je nachdem, welcher Wert kleiner ist
- Wenn sowohl der Grenzwert für private Bytes des Arbeitsprozesses als auch der &lt;Cache PrivateBytesLimit/&gt; festgelegt sind, verwendet der Cache den minimalen der beiden Werte.
- Wie in 1. x, werden Cache Einträge gelöscht und GC aufgerufen. Sammeln Sie aus zwei Gründen: 

    - Der Grenzwert für private Bytes liegt sehr nahe.
    - Der verfügbare Arbeitsspeicher beträgt fast oder weniger als 10%.
- Sie können Trim und Cache für wenig verfügbaren Arbeitsspeicher effektiv deaktivieren, indem Sie &lt;Cache prozagephysicalmemoryuselimit/&gt; auf 100 festlegen.
- Im Gegensatz zu 1. x hält 2,0 die Trim-und Collect-Aufrufe an, wenn der letzte GC. Die Sammlung hat die privaten Bytes oder die Größe der verwalteten Heaps nicht um mehr als 1% des (Cache) Arbeitsspeicher Limits reduziert.

## <a name="lab1-custom-cache-dependencies"></a>Lab1: benutzerdefinierte Cache Abhängigkeiten

1. Erstellen Sie eine neue Website.
2. Fügen Sie eine neue XML-Datei mit dem Namen Cache. XML hinzu, und speichern Sie Sie im Stammverzeichnis der Webanwendung.
3. Fügen Sie der Seite\_Load-Methode im Code Behind von default. aspx den folgenden Code hinzu: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Fügen Sie am Anfang der Datei "default. aspx" in der Quell Ansicht Folgendes hinzu: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Durchsuchen Sie "default. aspx". Was sagt die Zeit?
6. Aktualisieren Sie den Browser. Was sagt die Zeit?
7. Öffnen Sie Cache. XML, und fügen Sie den folgenden Code hinzu: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Speichern Sie "Cache. xml".
9. Aktualisieren Sie Ihren Browser. Was sagt die Zeit?
10. Erläutern Sie, warum die Zeit aktualisiert wurde, statt die zuvor zwischengespeicherten Werte anzuzeigen:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Lab 2: Verwenden von Abruf basierten Cache Abhängigkeiten

In diesem Lab wird das Projekt verwendet, das Sie im vorherigen Modul erstellt haben, das die Bearbeitung von Daten in der Northwind-Datenbank über ein GridView-und DetailsView-Steuerelement ermöglicht.

1. Öffnen Sie das Projekt in Visual Studio 2005.
2. Führen Sie das ASPNET\_RegSql-Hilfsprogramm für die Northwind-Datenbank aus, um die Datenbank und die Products-Tabelle zu aktivieren. Verwenden Sie den folgenden Befehl an einer Visual Studio-Eingabeaufforderung: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Fügen Sie der Datei "Web. config" Folgendes hinzu: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Fügen Sie ein neues Webformular mit dem Namen "ShowData. aspx" hinzu.
5. Fügen Sie die folgende @ OutputCache-Direktive zur ShowData. aspx-Seite hinzu: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Fügen Sie den folgenden Code auf der Seite\_Laden von "ShowData. aspx" hinzu: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Fügen Sie "ShowData. aspx" ein neues SqlDataSource-Steuerelement hinzu, und konfigurieren Sie es für die Verwendung der Datenbankverbindung Northwind. Klicken Sie auf Weiter.
8. Aktivieren Sie die Kontrollkästchen ProductName und ProductID, und klicken Sie auf Weiter.
9. Klicken Sie auf Fertigstellen.
10. Fügen Sie der ShowData. aspx-Seite eine neue GridView hinzu.
11. Wählen Sie in der Dropdown Liste SqlDataSource1 aus.
12. Speichern und Durchsuchen Sie "ShowData. aspx". Notieren Sie sich die angezeigte Zeit.
