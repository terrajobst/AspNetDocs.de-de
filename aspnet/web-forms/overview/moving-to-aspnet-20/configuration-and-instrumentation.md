---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Konfiguration und Instrumentation | Microsoft-Dokumentation
author: microsoft
description: In ASP.NET 2,0 sind wesentliche Änderungen an der Konfiguration und Instrumentation vorhanden. Die neue ASP.NET-Konfigurations-API ermöglicht die Änderung von Konfigurationsänderungen...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: cd5bedce5459e8cf8e72df8de69ebd82f2d97789
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78507879"
---
# <a name="configuration-and-instrumentation"></a>Konfiguration und Instrumentierung

von [Microsoft](https://github.com/microsoft)

> In ASP.NET 2,0 sind wesentliche Änderungen an der Konfiguration und Instrumentation vorhanden. Die neue ASP.NET-Konfigurations-API ermöglicht die programmgesteuerte Konfiguration von Konfigurationsänderungen. Außerdem sind viele neue Konfigurationseinstellungen für neue Konfigurationen und Instrumentation vorhanden.

In ASP.NET 2,0 sind wesentliche Änderungen an der Konfiguration und Instrumentation vorhanden. Die neue ASP.NET-Konfigurations-API ermöglicht die programmgesteuerte Konfiguration von Konfigurationsänderungen. Außerdem sind viele neue Konfigurationseinstellungen für neue Konfigurationen und Instrumentation vorhanden.

In diesem Modul erörtern wir ASP.net die Konfigurations-API, da Sie sich auf das Lesen aus und Schreiben in ASP.NET-Konfigurationsdateien bezieht. Außerdem wird die ASP.net-Instrumentation behandelt. Außerdem werden die in der ASP.net-Ablauf Verfolgung verfügbaren neuen Features behandelt.

## <a name="aspnet-configuration-api"></a>ASP.NET-Konfigurations-API

Die ASP.NET-Konfigurations-API ermöglicht das entwickeln, bereitstellen und Verwalten von Anwendungs Konfigurationsdaten mithilfe einer einzelnen Programmierschnittstelle. Mithilfe der-Konfigurations-API können Sie alle ASP.NET-Konfigurationen Programm gesteuert entwickeln und ändern, ohne den XML-Code in den Konfigurationsdateien direkt bearbeiten zu müssen. Außerdem können Sie die Konfigurations-API in den von Ihnen entwickelten Konsolen Anwendungen und-Skripts, in webbasierten Verwaltungs Tools und in Microsoft Management Console (MMC)-Snap-Ins verwenden.

Die folgenden beiden Konfigurations Verwaltungs Tools verwenden die-Konfigurations-API und sind in der .NET Framework Version 2,0 enthalten:

- Das MMC-Snap-in ASP.net, das die Konfigurations-API verwendet, um administrative Aufgaben zu vereinfachen, bietet eine integrierte Ansicht der lokalen Konfigurationsdaten von allen Ebenen der Konfigurations Hierarchie.
- Das Websiteverwaltungs-Tool, das es Ihnen ermöglicht, Konfigurationseinstellungen für lokale und Remote Anwendungen, einschließlich gehosteter Websites, zu verwalten.

Die ASP.NET-Konfigurations-API besteht aus einem Satz von ASP.NET-Verwaltungs Objekten, mit denen Sie Websites und Anwendungen Programm gesteuert konfigurieren können. Verwaltungs Objekte werden als .NET Framework-Klassenbibliothek implementiert. Mit dem Programmiermodell der Konfigurations-API können Sie Code Konsistenz und Zuverlässigkeit sicherstellen, indem Sie Datentypen zur Kompilierzeit erzwingen. Um die Verwaltung von Anwendungs Konfigurationen zu vereinfachen, können Sie mithilfe der Konfigurations-API Daten anzeigen, die von allen Punkten in der Konfigurations Hierarchie als einzelne Sammlung zusammengeführt werden, anstatt die Daten als separate Auflistungen von anderen anzuzeigen. Konfigurationsdateien. Außerdem können Sie mithilfe der Konfigurations-API vollständige Anwendungs Konfigurationen bearbeiten, ohne den XML-Code in den Konfigurationsdateien direkt bearbeiten zu müssen. Schließlich vereinfacht die API Konfigurationsaufgaben durch die Unterstützung von Verwaltungs Tools, wie z. b. dem Websiteverwaltungs-Tool. Die Konfigurations-API vereinfacht die Bereitstellung, indem die Erstellung von Konfigurationsdateien auf einem Computer und das Ausführen von Konfigurations Skripts auf mehreren Computern unterstützt wird.

> [!NOTE]
> Die Konfigurations-API unterstützt die Erstellung von IIS-Anwendungen nicht.

## <a name="working-with-local-and-remote-configuration-settings"></a>Arbeiten mit lokalen und Remote Konfigurationseinstellungen

Ein Konfigurationsobjekt stellt die zusammengeführte Ansicht der Konfigurationseinstellungen dar, die für eine bestimmte physische Entität, z. b. einen Computer, oder für eine logische Entität (z. b. eine Anwendung oder eine Website) gelten. Die angegebene logische Entität kann auf dem lokalen Computer oder auf einem Remote Server vorhanden sein. Wenn keine Konfigurationsdatei für eine angegebene Entität vorhanden ist, stellt das Konfigurationsobjekt die Standard Konfigurationseinstellungen dar, die in der Datei Machine. config definiert sind.

Sie können ein Konfigurationsobjekt mithilfe einer der geöffneten Konfigurations Methoden aus den folgenden Klassen erhalten:

1. Die ConfigurationManager-Klasse, wenn die Entität eine Client Anwendung ist.
2. Die WebConfigurationManager-Klasse, wenn die Entität eine Webanwendung ist.

Diese Methoden geben ein Konfigurationsobjekt zurück, das wiederum die erforderlichen Methoden und Eigenschaften bereitstellt, um die zugrunde liegenden Konfigurationsdateien zu verarbeiten. Sie können auf diese Dateien zum Lesen oder Schreiben zugreifen.

### <a name="reading"></a>Lesen

Zum Lesen von Konfigurationsinformationen verwenden Sie die GetSection-Methode oder die GetSectionGroup-Methode. Der Benutzer oder der Prozess, der liest, muss über Leseberechtigungen für alle Konfigurationsdateien in der Hierarchie verfügen.

> [!NOTE]
> Wenn Sie eine statische GetSection-Methode verwenden, die einen path-Parameter annimmt, muss der path-Parameter auf die Anwendung verweisen, in der der Code ausgeführt wird. Andernfalls wird der-Parameter ignoriert, und die Konfigurationsinformationen für die aktuell laufende Anwendung werden zurückgegeben.

### <a name="writing"></a>Schreiben

Sie verwenden eine der Save-Methoden, um Konfigurationsinformationen zu schreiben. Der Benutzer oder der Prozess, der schreibt, muss über Schreibberechtigungen für die Konfigurationsdatei und das Verzeichnis auf der aktuellen Konfigurations Hierarchieebene sowie über Leseberechtigungen für alle Konfigurationsdateien in der Hierarchie verfügen.

Um eine Konfigurationsdatei zu generieren, die die geerbten Konfigurationseinstellungen für eine angegebene Entität darstellt, verwenden Sie eine der folgenden Methoden für die Speicherkonfiguration:

1. Die Save-Methode zum Erstellen einer neuen Konfigurationsdatei.
2. Die SaveAs-Methode, um eine neue Konfigurationsdatei an einem anderen Speicherort zu generieren.

## <a name="configuration-classes-and-namespaces"></a>Konfigurations Klassen und-Namespaces

Viele Konfigurations Klassen und-Methoden sind einander ähnlich. In der folgenden Tabelle werden die am häufigsten verwendeten Konfigurations Klassen und-Namespaces beschrieben.

| **Konfigurations Klasse oder Namespace** | **Beschreibung** |
| --- | --- |
| [System. Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) -Namespace | Enthält die Haupt Konfigurations Klassen für alle .NET Framework Anwendungen. Abschnitts Handler-Klassen werden zum Abrufen von Konfigurationsdaten für einen Abschnitt von Methoden wie GetSection und GetSectionGroup verwendet. Diese beiden Methoden sind nicht statisch. |
| System. Configuration. Configuration-Klasse | Stellt einen Satz von Konfigurationsdaten für einen Computer, eine Anwendung, ein Webverzeichnis oder eine andere Ressource dar. Diese Klasse enthält nützliche Methoden, z. b. GetSection und GetSectionGroup, zum Aktualisieren von Konfigurationseinstellungen und zum Abrufen von Verweisen auf Abschnitte und Abschnitts Gruppen. Diese Klasse wird als Rückgabetyp für Methoden verwendet, die Entwurfszeit-Konfigurationsdaten abrufen, z. b. die Methoden der WebConfigurationManager-Klasse und der ConfigurationManager-Klasse. |
| System. Web. Configuration-Namespace | Enthält die Abschnitts Handlerklassen für die ASP.NET-Konfigurations Abschnitte, die unter [ASP.NET-Konfigurationseinstellungen](https://msdn.microsoft.com/library/b5ysx397.aspx)definiert sind. Abschnitts Handler-Klassen werden zum Abrufen von Konfigurationsdaten für einen Abschnitt von Methoden wie GetSection und GetSectionGroup verwendet. |
| System.Web.Configuration.WebConfigurationManager class | Bietet nützliche Methoden zum Abrufen von Verweisen auf Lauf Zeit-und Entwurfszeit-Konfigurationseinstellungen. Diese Methoden verwenden die System. Configuration. Configuration-Klasse als Rückgabetyp. Sie können die statische GetSection-Methode dieser Klasse oder die nicht statische GetSection-Methode der System. Configuration. ConfigurationManager-Klasse austauschbar verwenden. Bei Webanwendungs Konfigurationen wird die System. Web. Configuration. WebConfigurationManager-Klasse anstelle der System. Configuration. ConfigurationManager-Klasse empfohlen. |
| [System. Configuration. Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) -Namespace | Bietet eine Möglichkeit, den Konfigurations Anbieter anzupassen und zu erweitern. Dies ist die Basisklasse für alle Anbieter Klassen im Konfigurationssystem. |
| [System. Web. Management](https://msdn.microsoft.com/library/system.web.management.aspx) -Namespace | Enthält Klassen und Schnittstellen zum Verwalten und Überwachen der Integrität von Webanwendungen. Streng genommen gilt dieser Namespace nicht als Teil der Konfigurations-API. Beispielsweise wird die Ablauf Verfolgung und das Auslösen von Ereignissen durch die Klassen in diesem Namespace erreicht. |
| [System. Management. Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) -Namespace | Stellt die Klassen bereit, die für die Instrumentierung von Anwendungen erforderlich sind, um Ihre Verwaltungsinformationen und-Ereignisse über Windows-Verwaltungsinstrumentation (WMI) für potenzielle Consumer verfügbar zu machen. ASP.net die Integritäts Überwachung verwendet WMI zum Übermitteln von Ereignissen. Streng genommen gilt dieser Namespace nicht als Teil der Konfigurations-API. |

## <a name="reading-from-aspnet-configuration-files"></a>Lesen aus ASP.NET-Konfigurationsdateien

Die WebConfigurationManager-Klasse ist die Kern Klasse zum Lesen von ASP.NET-Konfigurationsdateien. Es gibt im Wesentlichen drei Schritte zum Lesen von ASP.NET-Konfigurationsdateien:

1. Verwenden Sie die OpenWebConfiguration-Methode, um ein Konfigurationsobjekt zu erhalten.
2. Sie erhalten einen Verweis auf den gewünschten Abschnitt in der Konfigurationsdatei.
3. Lesen Sie die gewünschten Informationen aus der Konfigurationsdatei.

Das Konfigurationsobjekt stellt keine bestimmte Konfigurationsdatei dar. Stattdessen handelt es sich um eine zusammengeführte Ansicht der Konfiguration eines Computers, einer Anwendung oder einer Website. Im folgenden Codebeispiel wird ein Konfigurationsobjekt instanziiert, das die Konfiguration einer Webanwendung mit dem Namen *productinfo*darstellt.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Beachten Sie Folgendes: Wenn der/ProductInfo-Pfad nicht vorhanden ist, gibt der obige Code die Standardkonfiguration zurück, wie in der Datei Machine. config angegeben.

Sobald Sie über das Konfigurationsobjekt verfügen, können Sie die GetSection-Methode oder die GetSectionGroup-Methode verwenden, um einen Drilldown in die Konfigurationseinstellungen auszuführen. Im folgenden Beispiel wird ein Verweis auf die Identitätswechsel Einstellungen für die oben aufgeführte productinfo-Anwendung abgerufen:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Schreiben in ASP.NET-Konfigurationsdateien

Wie beim Lesen aus Konfigurationsdateien ist die WebConfigurationManager-Klasse der Kern zum Schreiben in ASP.NET-Konfigurationsdateien. Es gibt auch drei Schritte zum Schreiben in ASP.NET-Konfigurationsdateien.

1. Verwenden Sie die OpenWebConfiguration-Methode, um ein Konfigurationsobjekt zu erhalten.
2. Sie erhalten einen Verweis auf den gewünschten Abschnitt in der Konfigurationsdatei.
3. Schreiben Sie die gewünschten Informationen mithilfe der Save-oder SaveAs-Methode aus der Konfigurationsdatei.

Der folgende Code ändert das **Debug** -Attribut des &lt;Kompilierungs&gt; Elements in false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Wenn dieser Code ausgeführt wird, wird das Attribut **Debug** der &lt;Kompilierung&gt; Element für die Datei Web. config der *webapp* auf false festgelegt.

## <a name="systemwebmanagement-namespace"></a>System. Web. Management-Namespace

Der System. Web. Management-Namespace stellt die Klassen und Schnittstellen zum Verwalten und Überwachen der Integrität von ASP.NET-Anwendungen bereit.

Die Protokollierung erfolgt durch Definieren einer Regel, die Ereignisse einem Anbieter zuordnet. Die Regel definiert den Typ der Ereignisse, die an den Anbieter gesendet werden. Die folgenden Basis Ereignisse sind für die Protokollierung verfügbar:

| **WebBaseEvent** | Die Basisereignis Klasse für alle Ereignisse. Enthält die erforderlichen Eigenschaften für alle Ereignisse, z. b. den Ereignis Code, den Ereignis Detail Code, das Datum und die Uhrzeit, zu der das Ereignis ausgelöst wurde, die Sequenznummer, die Ereignismeldung und Ereignis Details. |
| --- | --- |
| **WebManagementEvent** | Die Basisereignis Klasse für Verwaltungs Ereignisse, z. b. Anwendungs Lebensdauer, Anforderungs-, Fehler-und Überwachungs Ereignisse. |
| **WebHeartbeatEvent** | Das Ereignis, das von der Anwendung in regelmäßigen Abständen generiert wird, um nützliche Lauf Zeit Zustandsinformationen aufzuzeichnen. |
| **WebAuditEvent** | Die Basisklasse für Sicherheits Überwachungs Ereignisse, die zum Markieren von Bedingungen verwendet werden, wie z. b. Autorisierungs Fehler, Entschlüsselungs Fehler *usw.* |
| **WebRequestEvent** | Die Basisklasse für alle Informations Anforderungs Ereignisse. |
| **WebBaseErrorEvent** | Die Basisklasse für alle Ereignisse, die Fehlerzustände angeben. |

Mit den verfügbaren Anbieter Typen können Sie die Ereignis Ausgabe an Ereignisanzeige, SQL Server, Windows-Verwaltungsinstrumentation (WMI) und an die e-Mail senden. Die vorkonfigurierten Anbieter und Ereignis Zuordnungen verringern den Arbeitsaufwand, der erforderlich ist, um die Protokollierung von Ereignis Ausgaben zu erhalten.

ASP.NET 2,0 verwendet den Ereignisprotokoll Anbieter, der standardmäßig verwendet wird, um Ereignisse auf der Grundlage von Anwendungs Domänen zu protokollieren, die gestartet und beendet werden, sowie zum Protokollieren von nicht behandelten Ausnahmen. Dadurch können einige der grundlegenden Szenarien abgedeckt werden. Angenommen, die Anwendung löst eine Ausnahme aus, aber der Benutzer speichert den Fehler nicht, und Sie können ihn nicht reproduzieren. Mit der Standard Ereignisprotokoll-Regel können Sie die Ausnahme und Stapel Informationen erfassen, um eine bessere Vorstellung davon zu erhalten, welche Art von Fehler aufgetreten ist. Ein weiteres Beispiel gilt, wenn die Anwendung den Sitzungszustand verliert. In diesem Fall können Sie das Ereignisprotokoll überprüfen, um zu ermitteln, ob die Anwendungsdomäne wieder verwendet wird und warum die Anwendungsdomäne an erster Stelle angehalten wurde.

Außerdem ist das System Überwachungssystem erweiterbar. Beispielsweise können Sie benutzerdefinierte Webanwendungen definieren, Sie in der Anwendung auslösen und dann eine Regel definieren, um die Ereignis Informationen an einen Anbieter wie Ihre e-Mail zu senden. Dies ermöglicht es Ihnen, ihre Instrumentierung problemlos mit den Integritäts Überwachungs Anbietern zu verknüpfen. Als weiteres Beispiel können Sie jedes Mal ein Ereignis auslösen, wenn eine Bestellung verarbeitet wird, und eine Regel einrichten, die jedes Ereignis an die SQL Server Datenbank sendet. Sie können auch ein Ereignis auslösen, wenn sich ein Benutzer nicht mehrmals in einer Zeile anmelden kann, und das Ereignis so einrichten, dass die e-Mail-basierten Anbieter verwendet werden.

Die Konfiguration für die Standardanbieter und-Ereignisse wird in der globalen Web. config-Datei gespeichert. In der globalen Datei Web. config werden alle webbasierten Einstellungen gespeichert, die in der Datei Machine. config in ASP.net 1X gespeichert wurden. Die globale Datei Web. config befindet sich im folgenden Verzeichnis:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

Der Abschnitt &lt;healthMonitoring&gt; der globalen Web. config-Datei enthält Standard Konfigurationseinstellungen. Sie können diese Einstellung überschreiben oder eigene Einstellungen konfigurieren, indem Sie den Abschnitt &lt;healthMonitoring&gt; in der Datei "Web. config" für Ihre Anwendung implementieren.

Der Abschnitt &lt;healthMonitoring&gt; der globalen Web. config-Datei enthält die folgenden Elemente:

| **providers** | Enthält Anbieter, die für die Ereignisanzeige, WMI und SQL Server eingerichtet sind. |
| --- | --- |
| **eventMappings** | Enthält Zuordnungen für die verschiedenen webbase-Klassen. Sie können diese Liste erweitern, wenn Sie eine eigene Ereignisklasse generieren. Durch das Erstellen einer eigenen Ereignisklasse erhalten Sie eine präzisere Granularität für die Anbieter, an die Sie Informationen senden. Beispielsweise können Sie nicht behandelte Ausnahmen so konfigurieren, dass Sie an SQL Server gesendet werden, während Sie Ihre eigenen benutzerdefinierten Ereignisse an eine e-Mail senden. |
| **Werks** | Verknüpft die eventMappings mit dem Anbieter. |
| **Pufferung** | Wird mit SQL Server und e-Mail-Anbietern verwendet, um zu bestimmen, wie oft die Ereignisse an den Anbieter geleert werden. |

Im folgenden finden Sie ein Codebeispiel aus der globalen Web. config-Datei.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Vorgehensweise beim Speichern von Ereignissen in Ereignisanzeige

Wie bereits erwähnt, wird der Anbieter für das Protokollieren von Ereignissen in der Ereignisanzeige für Sie in der globalen Web. config-Datei konfiguriert. Standardmäßig werden alle Ereignisse, die auf **WebBaseErrorEvent** und **WebFailureAuditEvent** basieren, protokolliert. Sie können zusätzliche Regeln hinzufügen, um zusätzliche Informationen im Ereignisprotokoll zu protokollieren. Wenn Sie z. b. alle Ereignisse protokollieren möchten (*d. h.* jedes Ereignis, das auf **WebBaseEvent**basiert), können Sie der Datei "Web. config" die folgende Regel hinzufügen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Diese Regel verknüpft die Ereignis Zuordnung **alle Ereignisse** mit dem Ereignisprotokoll Anbieter. Sowohl EventMapping als auch der Anbieter sind in der globalen Web. config-Datei enthalten.

## <a name="how-to-store-events-to-sql-server"></a>Vorgehensweise beim Speichern von Ereignissen in SQL Server

Diese Methode verwendet die **aspnetdb** -Datenbank, die vom ASPNET\_RegSql. exe-Tool generiert wird. Der Standardanbieter verwendet die LocalSqlServer-Verbindungs Zeichenfolge, die entweder eine dateibasierte Datenbank im App-\_Datenordner oder die lokale SqlExpress-Instanz von SQL Server verwendet. Sowohl die LocalSqlServer-Verbindungs Zeichenfolge als auch der SqlProvider werden in der globalen Web. config-Datei konfiguriert.

Die LocalSqlServer-Verbindungs Zeichenfolge in der globalen Web. config-Datei sieht wie folgt aus:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Wenn Sie eine andere SQL Server-Instanz verwenden möchten, müssen Sie das ASPNET\_RegSql. exe-Tool verwenden, das Sie in der Datei%windir%\Microsoft.NET\Framework\v2.0. finden.\* Ordner. Verwenden Sie das ASPNET\_RegSql. exe-Tool, um eine benutzerdefinierte **aspnetdb** -Datenbank auf der SQL Server Instanz zu generieren. Fügen Sie anschließend die Verbindungs Zeichenfolge zur Anwendungs Konfigurationsdatei hinzu, und fügen Sie dann mithilfe der neuen Verbindungs Zeichenfolge einen Anbieter hinzu. Nachdem Sie die **aspnetdb** -Datenbank erstellt haben, müssen Sie eine Regel festlegen, um eine Ereignis Zuordnung mit dem SqlProvider zu verknüpfen.

Unabhängig davon, ob Sie den standardmäßigen SqlProvider verwenden oder einen eigenen Anbieter konfigurieren, müssen Sie eine Regel hinzufügen, die den Anbieter mit einer Ereignis Zuordnung verknüpft. Die folgende Regel verknüpft den neuen Anbieter, den Sie zuvor erstellt haben, mit der Ereignis Zuordnung **alle Ereignisse** . Mit dieser Regel werden alle Ereignisse auf der Grundlage von **WebBaseEvent** protokolliert und an den mysqlwebeventprovider gesendet, von dem die Verbindungs Zeichenfolge myaspnetdb verwendet wird. Mit dem folgenden Code wird eine Regel hinzugefügt, mit der der Anbieter mit einer Ereignis Zuordnung verknüpft wird:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Wenn Sie nur Fehler an SQL Server senden möchten, können Sie die folgende Regel hinzufügen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Weiterleiten von Ereignissen an WMI

Sie können die Ereignisse auch an WMI weiterleiten. Der WMI-Anbieter wird standardmäßig in der globalen Web. config-Datei konfiguriert.

Im folgenden Codebeispiel wird eine Regel hinzugefügt, um die Ereignisse an WMI weiterzuleiten:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Sie müssen eine Regel hinzufügen, um dem Anbieter eine Ereignis Zuordnung zuzuordnen, sowie eine WMI-Listeneranwendung, die auf die Ereignisse lauschen soll. Im folgenden Codebeispiel wird eine Regel hinzugefügt, um den WMI-Anbieter mit der Ereignis Zuordnung **alle Ereignisse** zu verknüpfen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Weiterleiten von Ereignissen an e-Mail

Sie können Ereignisse auch an eine e-Mail weiterleiten. Achten Sie darauf, welche Ereignis Regeln Sie Ihrem e-Mail-Anbieter zuordnen, da Sie versehentlich viele Informationen an Sie senden können, die für SQL Server oder das Ereignisprotokoll besser geeignet sind. Es gibt zwei e-Mail-Anbieter: SimpleMailWebEventProvider und TemplatedMailWebEventProvider. Jede verfügt über die gleichen Konfigurations Attribute, mit Ausnahme der Attribute "Template" und "detailedTemplateErrors", die beide nur auf "TemplatedMailWebEventProvider" verfügbar sind.

> [!NOTE]
> Keiner dieser e-Mail-Anbieter ist für Sie konfiguriert. Sie müssen Sie der Datei "Web. config" hinzufügen.

Der Hauptunterschied zwischen diesen beiden e-Mail-Anbietern besteht darin, dass SimpleMailWebEventProvider e-Mails in einer generischen Vorlage sendet, die nicht geändert werden kann. Mit der Beispieldatei Web. config wird dieser e-Mail-Anbieter mithilfe der folgenden Regel der Liste der konfigurierten Anbieter hinzugefügt:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Die folgende Regel wird ebenfalls hinzugefügt, um den e-Mail-Anbieter mit der Ereignis Zuordnung **alle Ereignisse** zu verknüpfen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2,0-Ablauf Verfolgung

Es gibt drei wesentliche Verbesserungen bei der Ablauf Verfolgung in ASP.NET 2,0.

1. Integrierte Ablauf Verfolgungs Funktionen
2. Programm gesteuerter Zugriff auf Ablauf Verfolgungs Meldungen
3. Verbesserte Ablauf Verfolgung auf Anwendungsebene

## <a name="integrated-tracing-functionality"></a>Integrierte Ablauf Verfolgungs Funktionen

Sie können jetzt von der System. Diagnostics. Trace-Klasse ausgegebene Nachrichten an die ASP.net-Ablauf Verfolgungs Ausgabe weiterleiten und von ASP.net Tracing ausgegebene Nachrichten an System. Diagnostics. Trace weiterleiten. Sie können ASP.net Instrumentierungs Ereignisse auch an System. Diagnostics. Trace weiterleiten. Diese Funktion wird durch das neue Attribut " **Write-dediagnosticstrace** " des &lt;Trace&gt;-Elements bereitgestellt. Wenn dieser boolesche Wert true ist, werden ASP.net-Ablauf Verfolgungs Meldungen an die System. Diagnostics-Ablauf Verfolgungs Infrastruktur weitergeleitet, damit Sie von allen Listener verwendet werden, die zum Anzeigen von Ablauf Verfolgungs Meldungen registriert sind.

## <a name="programmatic-access-to-trace-messages"></a>Programm gesteuerter Zugriff auf Ablauf Verfolgungs Meldungen

ASP.NET 2,0 ermöglicht den programmgesteuerten Zugriff auf alle Ablauf Verfolgungs Meldungen über die **TraceContextRecord** -Klasse und die **TraceRecords** -Auflistung. Der effizienteste Weg für den Zugriff auf Ablauf Verfolgungs Nachrichten besteht darin, einen **TraceContextEventHandler** -Delegaten (ebenfalls neu in ASP.NET 2,0) zu registrieren, um das neue **traceabgeschlossene** -Ereignis zu behandeln. Anschließend können Sie die Ablauf Verfolgungs Meldungen nach Belieben durchlaufen.

Dies wird im folgenden Codebeispiel veranschaulicht:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Im obigen Beispiel durchführe ich die TraceRecords-Auflistung und schreibe dann jede Nachricht in den Antwortstream.

## <a name="improved-application-level-tracing"></a>Verbesserte Ablauf Verfolgung auf Anwendungsebene

Die Ablauf Verfolgung auf Anwendungsebene wird durch die Einführung des neuen " **mustrecent** "-Attributs des &lt;Trace&gt;-Elements verbessert. Dieses Attribut gibt an, ob die letzte Ablauf Verfolgungs Ausgabe auf Anwendungsebene angezeigt wird und ältere Ablauf Verfolgungs Daten, die über die durch requestLimit angezeigten Grenzen hinausgehen, verworfen werden. Wenn der Wert false ist, werden Ablauf Verfolgungs Daten für Anforderungen angezeigt, bis das requestLimit-Attribut erreicht ist.

## <a name="aspnet-command-line-tools"></a>ASP.net-Befehlszeilen Tools

Es gibt mehrere Befehlszeilen Tools, die die Konfiguration von ASP.NET unterstützen. ASP.NET-Entwickler sollten mit dem ASPNET-\_regiis. exe-Tool vertraut sein. ASP.NET 2,0 stellt drei weitere Befehlszeilen Tools bereit, um die Konfiguration zu unterstützen.

Die folgenden Befehlszeilen Tools sind verfügbar:

| **Tool** | **Verwenden Sie** |
| --- | --- |
| **ASPNET-\_regiis. exe** | Ermöglicht die Registrierung von ASP.net mit IIS. Es gibt zwei Versionen dieser Tools, die mit ASP.NET 2,0, einer für 32-Bit-Systeme (im frameworkordner) und einem für 64-Bit-Systeme (im Ordner Framework64) ausgeliefert werden. Die 64-Bit-Version wird nicht auf einem 32-Bit-Betriebssystem installiert. |
| **ASPNET\_RegSql. exe** | Das Registrierungs Tool ASP.NET SQL Server wird zum Erstellen einer Microsoft SQL Server Datenbank verwendet, die von den SQL Server Anbietern in ASP.NET verwendet wird, oder zum Hinzufügen oder Entfernen von Optionen zu einer vorhandenen Datenbank. Die Datei ASPNET\_RegSql. exe befindet sich auf dem Webserver im Ordner [Laufwerk:] \WINDOWS\Microsoft.NET\Framework\versionNumber. |
| **ASPNET\_regbrowsers. exe** | Das ASP.net-Browser Registrierungs Tool analysiert und kompiliert alle systemweiten Browser Definitionen in eine Assembly und installiert die Assembly im globalen Assemblycache. Das Tool verwendet die Browser Definitions Dateien (. Browser Dateien) aus dem .NET Framework Browser-Unterverzeichnis. Das Tool befindet sich im Verzeichnis "%SystemRoot%\Microsoft.net\framework\version\". |
| **ASPNET\_Compiler. exe** | Mit dem ASP.net-Kompilierungs Tool können Sie eine ASP.NET-Webanwendung entweder direkt oder für die Bereitstellung an einem Ziel Speicherort (z. b. auf einem Produktionsserver) kompilieren. Die direkte Kompilierung unterstützt die Leistung der Anwendung, da Endbenutzer bei der Kompilierung der Anwendung keine Verzögerung bei der ersten Anforderung an die Anwendung erhalten. |

Da das Tool "ASPNET\_regiis. exe" nicht neu in ASP.NET 2,0 ist, wird es hier nicht erörtert.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>ASP.NET SQL Server Registrierungs Tool-ASPNET\_RegSql. exe

Sie können verschiedene Arten von Optionen mithilfe des Registrierungs Tools ASP.NET SQL Server festlegen. Sie können eine SQL-Verbindung angeben und angeben, welche ASP.NET-Anwendungsdienste SQL Server verwenden, um Informationen zu verwalten, anzugeben, welche Datenbank oder Tabelle für die SQL-Cache Abhängigkeit verwendet wird, und wie Sie die Unterstützung für das Speichern von Prozeduren und Sitzungs Zuständen SQL Server verwenden.

Mehrere ASP.NET-Anwendungsdienste greifen auf einen Anbieter zurück, um das Speichern und Abrufen von Daten aus einer Datenquelle zu verwalten. Jeder Anbieter ist für die Datenquelle spezifisch. ASP.NET enthält einen SQL Server Anbieter für die folgenden ASP.NET-Features:

- Mitgliedschaft (die [sqlmembership shipprovider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -Klasse).
- Rollen Verwaltung (die [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -Klasse).
- Profile (die [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) -Klasse).
- Webparts Personalisierung (die [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) -Klasse).
- Webereignisse (die [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) -Klasse).

Wenn Sie ASP.NET installieren, enthält die Datei Machine. config für Ihren Server Konfigurationselemente, die SQL Server Anbieter für die einzelnen ASP.NET-Features angeben, die von einem Anbieter abhängig sind. Diese Anbieter sind standardmäßig so konfiguriert, dass Sie eine Verbindung mit einer lokalen Benutzer Instanz von SQL Server Express 2005 herstellen. Wenn Sie die von den Anbietern verwendete Standard Verbindungs Zeichenfolge ändern, müssen Sie vor der Verwendung von ASP.NET-Funktionen, die in der Computerkonfiguration konfiguriert wurden, die SQL Server Datenbank und die Datenbankelemente für das ausgewählte Feature mithilfe von ASPNET\_RegSql. exe installieren. Wenn die Datenbank, die Sie mit dem SQL-Registrierungs Tool angeben, nicht bereits vorhanden ist (aspnetdb ist die Standarddatenbank, wenn keine in der Befehlszeile angegeben ist), muss der aktuelle Benutzer über Rechte zum Erstellen von Datenbanken in SQL Server und zum Erstellen des Schemas verfügen. innerhalb einer Datenbank.

### <a name="sql-cache-dependency"></a>SQL-Cacheabhängigkeit

Eine erweiterte Funktion der ASP.net-Ausgabe Zwischenspeicherung ist die SQL-Cache Abhängigkeit. Die SQL-Cache Abhängigkeit unterstützt zwei unterschiedliche Betriebsmodi: eine, die eine ASP.NET-Implementierung von Tabellen Abruf verwendet, und ein zweiter Modus, in dem die Abfrage Benachrichtigungsfunktionen von SQL Server 2005 verwendet werden. Das SQL-Registrierungs Tool kann verwendet werden, um den Tabellen Abruf Modus des Vorgangs zu konfigurieren.

### <a name="session-state"></a>Sitzungszustand

Standardmäßig werden die Sitzungs Zustands Werte und-Informationen im Arbeitsspeicher innerhalb des ASP.NET-Prozesses gespeichert. Alternativ können Sie Sitzungsdaten in einer SQL Server Datenbank speichern, wo Sie von mehreren Webservern gemeinsam genutzt werden kann. Wenn die Datenbank, die Sie für den Sitzungszustand mit dem SQL-Registrierungs Tool angeben, nicht bereits vorhanden ist, muss der aktuelle Benutzer über Rechte zum Erstellen von Datenbanken in SQL Server und zum Erstellen von Schema Elementen innerhalb einer Datenbank verfügen. Wenn die Datenbank vorhanden ist, muss der aktuelle Benutzer über Rechte zum Erstellen von Schema Elementen in der vorhandenen Datenbank verfügen.

Um die Sitzungs Zustands Datenbank auf SQL Server zu installieren, führen Sie das ASPNET\_RegSql. exe-Tool aus, und geben Sie mit dem Befehl die folgenden Informationen an:

- Der Name der SQL Server Instanz, wobei die Option **-S** verwendet wird.
- Die Anmelde Informationen für ein Konto, das über die Berechtigung zum Erstellen einer Datenbank auf einem Computer mit SQL Server verfügt. Verwenden Sie die Option " **-E** ", um den derzeit angemeldeten Benutzer zu verwenden, oder verwenden Sie die Option " **-U** ", um eine Benutzer-ID zusammen mit der Option " **-P** " anzugeben und ein Kennwort anzugeben.
- Die Befehlszeilenoption **-ssadd** zum Hinzufügen der Sitzungs Zustands Datenbank.

Standardmäßig können Sie das ASPNET\_RegSql. exe-Tool nicht verwenden, um die Sitzungs Zustands Datenbank auf einem Computer zu installieren, auf dem SQL Server 2005 Express Edition ausgeführt wird.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>Das ASP.net-Browser Registrierungs Tool-ASPNET\_regbrowsers. exe

In ASP.NET Version 1,1 enthielt die Datei Machine. config einen Abschnitt mit dem Namen &lt;browserCaps&gt;. Dieser Abschnitt enthielt eine Reihe von XML-Einträgen, die die Konfigurationen für verschiedene Browser auf Grundlage eines regulären Ausdrucks definiert haben. Für ASP.net, Version 2,0, eine neue. Die Browser Datei definiert die Parameter eines bestimmten Browsers mithilfe von XML-Einträgen. Informationen zu einem neuen Browser Fügen Sie hinzu, indem Sie eine neue hinzufügen. Browser Datei in den Ordner, der sich unter%SystemRoot%\Microsoft.NET\Framework\Version\CONFIG\Browsers auf dem System befindet.

Da eine Anwendung nicht jedes Mal, wenn Sie Browserinformationen erfordert, eine config-Datei liest, können Sie eine neue erstellen. Browser Datei, und führen Sie ASPNET\_regbrowsers. exe aus, um die erforderlichen Änderungen der Assembly hinzuzufügen. Dadurch kann der Server sofort auf die neuen Browserinformationen zugreifen, sodass Sie Ihre Anwendungen nicht herunterfahren müssen, um die Informationen zu erhalten. Eine Anwendung kann über die Browser-Eigenschaft des aktuellen HttpRequest-Objekts auf Browserfunktionen zugreifen.

Die folgenden Optionen sind verfügbar, wenn Sie ASPNET\_regbrowser. exe ausführen:

| **Option** | **Beschreibung** |
| --- | --- |
| **-?** | Zeigt den Hilfe Text ASPNET\_regbbrowsers. exe im Befehlsfenster an. |
| **-i** | Erstellt die Assembly für Lauf Zeit Browserfunktionen und installiert Sie im globalen Assemblycache. |
| **-u** | Deinstalliert die Assembly der Lauf Zeit Browserfunktionen aus dem globalen Assemblycache. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>Das ASP.net-Kompilierungs Tool-ASPNET\_Compiler. exe

Das ASP.net-Kompilierungs Tool kann auf zwei Arten verwendet werden: für die direkte Kompilierung und Kompilierung für die Bereitstellung, bei der ein Ziel Ausgabeverzeichnis angegeben wird.

### <a name="compiling-an-application-in-place"></a>[Kompilieren einer Anwendung an Ort und Stelle](https://msdn.microsoft.com/library/ms229863.aspx)

Mit dem ASP.net-Kompilierungs Tool kann eine Anwendung direkt kompiliert werden, d. h. es imitiert das Verhalten von mehreren Anforderungen an die Anwendung und führt so zu einer regulären Kompilierung. Benutzer einer vorkompilierten Website erhalten keine Verzögerung, die durch Kompilieren der Seite bei der ersten Anforderung verursacht wird.

Wenn Sie einen Standort vorkompilieren, gelten die folgenden Elemente:

- Der Standort behält seine Dateien und die Verzeichnisstruktur bei.
- Sie müssen über Compiler für alle Programmiersprachen verfügen, die vom-Standort auf dem Server verwendet werden.
- Wenn bei einer Datei die Kompilierung fehlschlägt, schlägt die Kompilierung des gesamten Standorts fehl

Sie können eine Anwendung auch direkt neu kompilieren, nachdem Sie Ihr neue Quelldateien hinzugefügt haben. Das Tool kompiliert nur die neuen oder geänderten Dateien, sofern Sie nicht die Option **-c** einschließen.

> [!NOTE]
> Bei der Kompilierung einer Anwendung, die eine in einer Anwendung enthaltene Anwendung enthält, wird die nicht kompilierte Anwendung nicht kompiliert. Die Anwendung muss separat kompiliert werden.

### <a name="compiling-an-application-for-deployment"></a>[Kompilieren einer Anwendung für die Bereitstellung](https://msdn.microsoft.com/library/ms229863.aspx)

Sie kompilieren eine Anwendung für die Bereitstellung (Kompilierung an einem Zielort), indem Sie den targetDir-Parameter angeben. TARGETDIR kann der endgültige Speicherort für die Webanwendung sein, oder die kompilierte Anwendung kann weiter bereitgestellt werden. Mithilfe der Option **-u** wird die Anwendung so kompiliert, dass Sie Änderungen an bestimmten Dateien in der kompilierten Anwendung vornehmen können, ohne Sie neu kompilieren zu müssen. ASPNET\_Compiler. exe unterscheidet zwischen statischen und dynamischen Dateitypen und behandelt diese anders, wenn die resultierende Anwendung erstellt wird.

- Statische Dateitypen sind solche, die nicht über einen zugeordneten Compiler oder Buildanbieter verfügen, z. b. Dateien, deren benannte Erweiterungen wie CSS, GIF, htm, HTML, JPG, JS usw. aufweisen. Diese Dateien werden einfach an den Ziel Speicherort kopiert, wobei ihre relativen Positionen in der Verzeichnisstruktur beibehalten werden.
- Dynamische Dateitypen sind solche, die über einen zugeordneten Compiler oder Buildanbieter verfügen, einschließlich Dateien mit ASP.net-spezifischen Dateinamen Erweiterungen wie. asax, ASCX, ashx, aspx,. Browser,. Master usw. Das ASP.net-Kompilierungs Tool generiert Assemblys aus diesen Dateien. Wenn die Option **-u** ausgelassen wird, erstellt das Tool auch Dateien mit der Dateinamenerweiterung. Kompiliert, mit dem die ursprünglichen Quelldateien der Assembly zugeordnet werden. Um sicherzustellen, dass die Verzeichnisstruktur der Anwendungs Quelle beibehalten wird, generiert das Tool Platzhalter Dateien an den entsprechenden Speicherorten in der Zielanwendung.

Sie müssen die Option **-u** verwenden, um anzugeben, dass der Inhalt der kompilierten Anwendung geändert werden kann. Andernfalls werden nachfolgende Änderungen ignoriert, oder es treten Laufzeitfehler auf.

In der folgenden Tabelle wird beschrieben, wie das ASP.net-Kompilierungs Tool verschiedene Dateitypen behandelt, wenn die Option **-u** eingeschlossen ist.

| **Dateityp** | **Compileraktion** |
| --- | --- |
| . ascx,. aspx,. Master | Diese Dateien werden in Markup und Quellcode aufgeteilt, das sowohl Code-Behind-Dateien als auch Code enthält, der in &lt;Script runat = "Server"-&gt; Elemente eingeschlossen ist. Der Quellcode wird in Assemblys kompiliert, deren Namen von einem Hash Algorithmus abgeleitet sind, und die Assemblys werden in das Verzeichnis "bin" eingefügt. Alle Inline Codes, d. h. Code, der zwischen den **&lt;%** und **%&gt;** eckigen Klammern eingeschlossen ist, sind in Markup enthalten und nicht kompiliert. Es werden neue Dateien mit dem gleichen Namen wie die Quelldateien erstellt, die das Markup enthalten und in den entsprechenden Ausgabe Verzeichnissen abgelegt werden. |
| ashx, asmx | Diese Dateien werden nicht kompiliert und in die Ausgabe Verzeichnisse verschoben und nicht kompiliert. Wenn der Handlercode kompiliert werden soll, platzieren Sie den Code in den Quell Code Dateien im Code Verzeichnis der APP\_. |
| . cs,. vb,. jsl,. cpp (ohne Code-Behind-Dateien für die zuvor aufgeführten Dateitypen) | Diese Dateien werden kompiliert und als Ressource in Assemblys eingeschlossen, die auf Sie verweisen. Quelldateien werden nicht in das Ausgabeverzeichnis kopiert. Wenn auf eine Codedatei nicht verwiesen wird, wird Sie nicht kompiliert. |
| Benutzerdefinierte Dateitypen | Diese Dateien werden nicht kompiliert. Diese Dateien werden in die entsprechenden Ausgabe Verzeichnisse kopiert. |
| Quell Code Dateien in der APP\_Unterverzeichnis "Code" | Diese Dateien werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. |
| RESX-und resources-Dateien im Unterverzeichnis der APP\_globalresources | Diese Dateien werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. Im Hauptausgabe Verzeichnis wird keine app\_globalresources-Unterverzeichnis erstellt, und im Quellverzeichnis werden keine RESX-oder resources-Dateien in die Ausgabe Verzeichnisse kopiert. |
| RESX-und resources-Dateien in der APP\_Unterverzeichnis "localresources" | Diese Dateien werden nicht kompiliert und in die entsprechenden Ausgabe Verzeichnisse kopiert. |
| . Skin-Dateien im Unterverzeichnis der APP-\_Designs | Die Skin-Dateien und die statischen Designdateien werden nicht kompiliert und in die entsprechenden Ausgabe Verzeichnisse kopiert. |
| . Browser Web. config statische Dateitypen Assemblys sind bereits im bin-Verzeichnis vorhanden. | Diese Dateien werden unverändert in die Ausgabe Verzeichnisse kopiert. |

In der folgenden Tabelle wird beschrieben, wie das ASP.net-Kompilierungs Tool verschiedene Dateitypen behandelt, wenn die Option **-u** ausgelassen wird.

| **Dateityp** | **Compileraktion** |
| --- | --- |
| . aspx,. asmx,. ashx,. Master | Diese Dateien werden in Markup und Quellcode aufgeteilt, das sowohl Code-Behind-Dateien als auch Code enthält, der in &lt;Script runat = "Server"-&gt; Elemente eingeschlossen ist. Der Quellcode wird in Assemblys mit Namen kompiliert, die von einem Hash Algorithmus abgeleitet werden. Die resultierenden Assemblys werden in das Verzeichnis "bin" eingefügt. Alle Inline Codes, d. h. Code, der zwischen den **&lt;%** und **%&gt;** eckigen Klammern eingeschlossen ist, sind in Markup enthalten und nicht kompiliert. Der Compiler erstellt neue Dateien, die das Markup mit dem gleichen Namen wie die Quelldateien enthalten. Diese resultierenden Dateien werden in das Verzeichnis "bin" eingefügt. Der Compiler erstellt auch Dateien mit dem gleichen Namen wie die Quelldateien, jedoch mit der Erweiterung. Kompiliert, die Mapping-Informationen enthalten. Die. Kompilierte Dateien werden in den Ausgabe Verzeichnissen abgelegt, die dem ursprünglichen Speicherort der Quelldateien entsprechen. |
| .ascx | Diese Dateien werden in Markup und Quellcode aufgeteilt. Der Quellcode wird in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt, wobei die Namen von einem Hash Algorithmus abgeleitet werden. Es werden keine Markup Dateien generiert. |
| . cs,. vb,. jsl,. cpp (ohne Code-Behind-Dateien für die zuvor aufgeführten Dateitypen) | Der Quellcode, auf den von den Assemblys verwiesen wird, die aus ASCX-, ashx-oder ASPX-Dateien generiert werden, werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. Es werden keine Quelldateien kopiert. |
| Benutzerdefinierte Dateitypen | Diese Dateien werden wie dynamische Dateien kompiliert. Abhängig vom Typ der Datei, auf der Sie basieren, kann der Compiler die Mapping-Dateien in den Ausgabe Verzeichnissen platzieren. |
| Dateien in der APP\_Unterverzeichnis "Code" | Quell Code Dateien in diesem Unterverzeichnis werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. |
| Dateien in der APP\_globalresources-Unterverzeichnis | Diese Dateien werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. Im Hauptausgabe Verzeichnis wird keine app\_globalresources-Unterverzeichnis erstellt. Wenn in der Konfigurationsdatei AppliesTo = "All" angegeben wird, werden RESX-und resources-Dateien in die Ausgabe Verzeichnisse kopiert. Sie werden nicht kopiert, wenn von einem [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx)auf Sie verwiesen wird. |
| RESX-und resources-Dateien in der APP\_Unterverzeichnis "localresources" | Diese Dateien werden in Assemblys mit eindeutigen Namen kompiliert und in das Verzeichnis "bin" eingefügt. Keine RESX-oder Resource-Dateien werden in die Ausgabe Verzeichnisse kopiert. |
| . Skin-Dateien im Unterverzeichnis der APP-\_Designs | Designs werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. Stubdateien werden für. Skin-Dateien erstellt und im entsprechenden Ausgabeverzeichnis abgelegt. Statische Dateien (z. b. CSS) werden in die Ausgabe Verzeichnisse kopiert. |
| . Browser Web. config statische Dateitypen Assemblys sind bereits im bin-Verzeichnis vorhanden. | Diese Dateien werden unverändert in das Ausgabeverzeichnis kopiert. |

### <a name="fixed-assembly-names"></a>[Assemblynamen fester](https://msdn.microsoft.com/library/ms229863.aspx##)

In einigen Szenarien, z. b. beim Bereitstellen einer Webanwendung mit der MSI-Windows Installer, müssen konsistente Dateinamen und Inhalte verwendet werden, sowie konsistente Verzeichnisstrukturen zum Identifizieren von Assemblys oder Konfigurationseinstellungen für Updates. In diesen Fällen können Sie mit der Option **-fixednames** angeben, dass das ASP.net-Kompilierungs Tool eine Assembly für jede Quelldatei kompilieren soll, statt die zu verwenden, in der mehrere Seiten in Assemblys kompiliert werden. Dies kann zu einer großen Anzahl von Assemblys führen, daher sollten Sie diese Option mit Vorsicht verwenden, wenn Sie sich mit der Skalierbarkeit befassen.

### <a name="strong-name-compilation"></a>[Kompilierung mit starkem Namen](https://msdn.microsoft.com/library/ms229863.aspx##)

Die Optionen **-APTCA**, **-Delta Sign**, **-keycontainer** und **-keyfile** werden bereitgestellt, sodass Sie ASPNET\_Compiler. exe verwenden können, um Assemblys mit starkem Namen zu erstellen, ohne das [Strong Name-Tool (Sn. exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) separat zu verwenden. Diese Optionen entsprechen " **allowpartiallytreuhänder dcallersattribute**", " **assemblydelta-signattribute**", " **AssemblyKeyNameAttribute**" und " **AssemblyKeyFileAttribute**".

Die Erörterung dieser Attribute liegt außerhalb des Umfangs dieses Kurses.

## <a name="labs"></a>Labs

Jedes der folgenden Labs basiert auf den vorherigen Labs. Sie müssen Sie in der richtigen Reihenfolge ausführen.

## <a name="lab-1-using-the-configuration-api"></a>Lab 1: Verwenden der Konfigurations-API

1. Erstellen Sie eine neue Website mit dem Namen *mod9lab*.
2. Fügen Sie der Website eine neue Webkonfigurationsdatei hinzu.
3. Fügen Sie der Datei "Web. config" Folgendes hinzu:

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Dadurch wird sichergestellt, dass Sie über die Berechtigung zum Speichern von Änderungen in der Datei "Web. config" verfügen.

1. Fügen Sie "default. aspx" ein neues Label-Steuerelement hinzu, und ändern Sie die ID in **lbldebugstatus**.
2. Fügen Sie "default. aspx" ein neues Button-Steuerelement hinzu.
3. Ändern Sie die ID des Schaltflächen-Steuer Elements in **btntoggledebug** und den Text zum Umschalten des **Debugstatus**.
4. Öffnen Sie die Code Ansicht für die Code-Behind-Datei von "default. aspx", und fügen Sie eine **using** -Anweisung für " **System. Web. Configuration** " wie folgt hinzu:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Fügen Sie der-Klasse zwei private Variablen und eine Seite\_Init-Methode hinzu, wie unten dargestellt:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Fügen Sie den folgenden Code zu Seiten\_laden hinzu:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Speichern und Durchsuchen Sie "default. aspx". Beachten Sie, dass das Label-Steuerelement den aktuellen Debugstatus anzeigt.
2. Doppelklicken Sie im Designer auf das Schaltflächen-Steuerelement, und fügen Sie den folgenden Code zum Click-Ereignis für das Schaltflächen-Steuerelement hinzu:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Speichern und Durchsuchen Sie Default. aspx, und klicken Sie auf die Schaltfläche.
2. Öffnen Sie die Datei "Web. config" nach jedem Klick auf die Schaltfläche, und beobachten Sie das Attribut **Debug** im Abschnitt &lt;Kompilierung&gt;.

## <a name="lab-2-logging-application-restarts"></a>Lab 2: Protokollieren von Anwendungs Neustarts

In dieser Übungseinheit erstellen Sie Code, mit dem Sie die Protokollierung von anwendungsshutdowns, Startups und Neukompilierungen in der Ereignisanzeige umschalten können.

1. Fügen Sie "default. aspx" eine DropDownList hinzu, und ändern Sie die ID in "ddllogappevents".
2. Legen Sie die Eigenschaft **AutoPostBack** für DropDownList auf **true**fest.
3. Fügen Sie der Items-Sammlung für DropDownList drei Elemente hinzu. Legen Sie den **Text** für das erste Element auf *Wert* und den Wert-1. Legen Sie den **Text** und den **Wert** des zweiten Elements auf **true** und den **Text** und den **Wert** des dritten Elements auf **false**.
4. Fügen Sie "default. aspx" eine neue Bezeichnung hinzu. Ändern Sie die ID in **lbllogappevents**.
5. Öffnen Sie die Code Behind-Ansicht für "default. aspx", und fügen Sie eine neue Deklaration für eine Variable vom Typ "HealthMonitoringSection" hinzu, wie unten dargestellt:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Fügen Sie dem vorhandenen Code in der Seite\_init den folgenden Code hinzu:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Doppelklicken Sie auf DropDownList, und fügen Sie den folgenden Code zum SelectedIndexChanged-Ereignis hinzu:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Durchsuchen Sie "default. aspx".
2. Legen Sie die Dropdown Liste auf **false**fest.
3. Löschen Sie das Anwendungsprotokoll in der Ereignisanzeige.
4. Klicken Sie auf die Schaltfläche, um das debug-Attribut für die Anwendung zu ändern.
5. Aktualisieren Sie das Anwendungsprotokoll in der Ereignisanzeige. 

    1. Wurden Ereignisse protokolliert?
    2. Warum oder warum?
6. Legen Sie die Dropdown Liste auf **true fest.**
7. Klicken Sie auf die Schaltfläche, um das debug-Attribut für die Anwendung zu wechseln.
8. Aktualisieren Sie den Anwendungs Anmelde Namen des Ereignisanzeige. 

    1. Wurden Ereignisse protokolliert?
    2. Was ist der Grund für das Herunterfahren der APP?
9. Experimentieren Sie mit dem Aktivieren und Deaktivieren der Protokollierung, und sehen Sie sich die Änderungen an der Datei "Web. config" an.

## <a name="more-information"></a>Weitere Informationen:

Das ASP.NET 2.0-Anbieter Modell ermöglicht es Ihnen, eigene Anbieter für die nicht nur Anwendungs Instrumentation zu erstellen, sondern auch für viele andere Verwendungszwecke, wie z. b. Mitgliedschaft, Profile usw. Ausführliche Informationen zum Schreiben eines benutzerdefinierten Anbieters, um Anwendungs Ereignisse in eine Textdatei zu protokollieren, finden Sie unter [diesem Link](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
