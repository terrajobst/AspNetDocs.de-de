---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: Protokollieren von Fehler Details mit ASP.NET Health Monitoring (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Das System Überwachungssystem von Microsoft bietet eine einfache und anpassbare Möglichkeit zum Protokollieren verschiedener Webanwendungen, einschließlich nicht behandelter Ausnahmen. Dieses Tutorial führt Sie durch...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: f57aca41771adfd9a7c7f38da1916db9197262da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78507597"
---
# <a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Protokollieren von Fehlerdetails mit der ASP.NET-Systemüberwachung (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Das System Überwachungssystem von Microsoft bietet eine einfache und anpassbare Möglichkeit zum Protokollieren verschiedener Webanwendungen, einschließlich nicht behandelter Ausnahmen. In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie das System Überwachungssystem einrichten, um nicht behandelte Ausnahmen in einer Datenbank zu protokollieren und Entwickler über eine e-Mail zu benachrichtigen.

## <a name="introduction"></a>Einführung

Die Protokollierung ist ein nützliches Tool für die Überwachung der Integrität einer bereitgestellten Anwendung und für die Diagnose eventuell auftretende Probleme. Es ist besonders wichtig, Fehler zu protokollieren, die in einer bereitgestellten Anwendung auftreten, damit Sie behoben werden können. Das `Error` Ereignis wird immer dann ausgelöst, wenn eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung auftritt. im [vorherigen Tutorial](processing-unhandled-exceptions-vb.md) wurde gezeigt, wie Sie einen Entwickler über einen Fehler benachrichtigen und seine Details protokollieren können, indem Sie einen Ereignishandler für das `Error`-Ereignis erstellen. Das Erstellen eines `Error` Ereignis Handlers zum Protokollieren der Fehlerdetails und Benachrichtigen eines Entwicklers ist jedoch unnötig, da diese Aufgabe von ASP ausgeführt werden kann. *System Überwachungssystem*von net.

Das System Überwachungssystem wurde in ASP.NET 2,0 eingeführt und dient zur Überwachung der Integrität einer bereitgestellten ASP.NET-Anwendung, indem Ereignisse protokolliert werden, die während der Anwendungs-oder Anforderungs Lebensdauer auftreten. Die vom Integritäts Überwachungssystem protokollierten Ereignisse werden als Integritäts *Überwachungs Ereignisse* oder *Webereignisse*bezeichnet und umfassen Folgendes:

- Anwendungs Lebensdauer-Ereignisse, z. b. Wenn eine Anwendung gestartet oder angehalten wird
- Sicherheitsereignisse, einschließlich fehlgeschlagener Anmeldeversuche und fehlerhafter URL-Autorisierungs Anforderungen
- Anwendungsfehler, einschließlich nicht behandelter Ausnahmen, Ausnahmen bei der Ansichts Zustands Überprüfung, Ausnahmen bei der Anforderungs Validierung und Kompilierungsfehler, unter anderem Fehlertypen.

Wenn ein Integritäts Überwachungs Ereignis ausgelöst wird, kann es auf einer beliebigen Anzahl von angegebenen *Protokoll Quellen*protokolliert werden. Das System Überwachungssystem wird mit Protokoll Quellen ausgeliefert, die Webereignisse in einer Microsoft SQL Server Datenbank, im Windows-Ereignisprotokoll oder über eine e-Mail-Nachricht protokollieren. Sie können auch eigene Protokoll Quellen erstellen.

Die Ereignisse, die das System Überwachungssystem zusammen mit den verwendeten Protokoll Quellen protokolliert, werden in `Web.config`definiert. Mit einigen Konfigurations Markup Zeilen können Sie die Systemüberwachung verwenden, um alle nicht behandelten Ausnahmen in einer Datenbank zu protokollieren und Sie per e-Mail über die Ausnahme zu benachrichtigen.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Überprüfen der Konfiguration des System Überwachungssystems

Das Verhalten des Integritäts Überwachungssystems wird durch die Konfigurationsinformationen definiert, die sich im [`<healthMonitoring>`-Element](https://msdn.microsoft.com/library/2fwh2ss9.aspx) in `Web.config`befinden. Dieser Konfigurations Abschnitt definiert unter anderem die folgenden drei wichtigen Informationen:

1. Die Integritäts Überwachungs Ereignisse, die, wenn Sie ausgelöst werden, protokolliert werden sollen.
2. Die Protokoll Quellen und
3. Wie jedes in (1) definierte Integritäts Überwachungs Ereignis den in (2) definierten Protokoll Quellen zugeordnet wird.

Diese Informationen werden durch drei untergeordnete Konfigurationselemente angegeben: [`<eventMappings>`](https://msdn.microsoft.com/library/yc5yk01w.aspx), [`<providers>`](https://msdn.microsoft.com/library/zaa41kz1.aspx)bzw. [`<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx).

Die Standard Konfigurationsinformationen für die Systemüberwachung finden Sie in der `Web.config`-Datei in `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` Ordner. Diese Standard Konfigurationsinformationen, bei denen ein Markup aus Gründen der Kürze entfernt wurde, sind unten dargestellt:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

Die relevanten Integritäts Überwachungs Ereignisse werden im `<eventMappings>`-Element definiert, das einen benutzerfreundlichen Namen für eine Klasse von Integritäts Überwachungs Ereignissen liefert. Im obigen Markup weist das `<eventMappings>`-Element den benutzerfreundlichen Namen "All Errors" den Integritäts Überwachungs Ereignissen vom Typ `WebBaseErrorEvent` und den Namen "Fehler Überwachungen" für Integritäts Überwachungs Ereignisse des Typs `WebFailureAuditEvent`zu.

Das `<providers>`-Element definiert die Protokoll Quellen und gibt Ihnen einen benutzerfreundlichen Namen und die Angabe beliebiger Protokoll Quellen spezifischer Konfigurationsinformationen. Das erste `<add>`-Element definiert den Event LogProvider-Anbieter, der die angegebenen Integritäts Überwachungs Ereignisse mithilfe der `EventLogWebEventProvider`-Klasse protokolliert. Die `EventLogWebEventProvider`-Klasse protokolliert das Ereignis im Windows-Ereignisprotokoll. Mit dem zweiten `<add>`-Element wird der Anbieter "SqlWebEventProvider" definiert, der Ereignisse über die `SqlWebEventProvider`-Klasse in einer Microsoft SQL Server Datenbank protokolliert. Die Konfiguration "SqlWebEventProvider" gibt die Verbindungs Zeichenfolge (`connectionStringName`) der Datenbank unter anderen Konfigurationsoptionen an.

Das `<rules>`-Element ordnet die im `<eventMappings>`-Element angegebenen Ereignisse den Protokoll Quellen im `<providers>`-Element zu. Standardmäßig protokollieren ASP.NET-Webanwendungen alle nicht behandelten Ausnahmen und Überwachungs Fehler im Windows-Ereignisprotokoll.

## <a name="logging-events-to-a-database"></a>Protokollieren von Ereignissen in einer Datenbank

Die Standardkonfiguration des Integritäts Überwachungssystems kann auf Webanwendungen für Webanwendungen angepasst werden, indem ein `<healthMonitoring>` Abschnitt zur `Web.config` Datei der Anwendung hinzugefügt wird. Sie können zusätzliche Elemente in die Abschnitte `<eventMappings>`, `<providers>`und `<rules>` einschließen, indem Sie das `<add>`-Element verwenden. Um eine Einstellung aus der Standardkonfiguration zu entfernen, verwenden Sie das `<remove>`-Element, oder verwenden Sie `<clear />`, um alle Standardwerte aus einem dieser Abschnitte zu entfernen. Wir konfigurieren die Webanwendung "Book Reviews" so, dass alle nicht behandelten Ausnahmen in einer Microsoft SQL Server Datenbank mithilfe der `SqlWebEventProvider`-Klasse protokolliert werden.

Die `SqlWebEventProvider`-Klasse ist Teil des System Überwachungssystems und protokolliert ein Integritäts Überwachungs Ereignis für eine angegebene SQL Server Datenbank. Die `SqlWebEventProvider`-Klasse erwartet, dass die angegebene Datenbank eine gespeicherte Prozedur mit dem Namen `aspnet_WebEvent_LogEvent`enthält. Dieser gespeicherten Prozedur werden die Details des Ereignisses und das Speichern der Ereignis Details übermittelt. Die gute Nachricht ist, dass Sie diese gespeicherte Prozedur und die Tabelle nicht zum Speichern der Ereignis Details erstellen müssen. Sie können diese Objekte mit dem `aspnet_regsql.exe` Tool zur Datenbank hinzufügen.

> [!NOTE]
> Das `aspnet_regsql.exe` Tool wurde im Abschnitt [ *Konfigurieren einer Website erläutert, die Anwendungsdienste Tutorial verwendet,* ](configuring-a-website-that-uses-application-services-vb.md) wenn wir die Unterstützung für ASP hinzugefügt haben. NET-Anwendungsdienste. Folglich enthält die Website der Website zur Buch Überprüfung bereits die gespeicherte Prozedur `aspnet_WebEvent_LogEvent`, die die Ereignis Informationen in einer Tabelle mit dem Namen `aspnet_WebEvent_Events`speichert.

Nachdem Sie die erforderliche gespeicherte Prozedur und Tabelle zur Datenbank hinzugefügt haben, müssen Sie nur noch die Integritäts Überwachung anweisen, alle nicht behandelten Ausnahmen in der Datenbank zu protokollieren. Fügen Sie hierzu der `Web.config` Datei Ihrer Website das folgende Markup hinzu:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Das obige Integritäts Überwachungs-Konfigurations Markup verwendet `<clear />` Elemente, um die vordefinierte Konfigurationsinformationen zur Integritäts Überwachung aus den Abschnitten `<eventMappings>`, `<providers>`und `<rules>` zu löschen. Anschließend wird jedem dieser Abschnitte ein einziger Eintrag hinzugefügt.

- Das `<eventMappings>`-Element definiert ein einzelnes Integritäts Überwachungs Ereignis namens "All Errors", das immer dann ausgelöst wird, wenn eine nicht behandelte Ausnahme auftritt.
- Das `<providers>`-Element definiert eine einzelne Protokoll Quelle mit dem Namen "SqlWebEventProvider", die die `SqlWebEventProvider`-Klasse verwendet. Das `connectionStringName`-Attribut wurde auf "reviewsconnectionstring" festgelegt, d. h. der Name der Verbindungs Zeichenfolge, die im `<connectionStrings>` Abschnitt definiert ist.
- Zum Schluss gibt das &lt;Rules&gt;-Element an, dass, wenn ein "alle Fehler"-Ereignis auftritt, mit dem Anbieter "SqlWebEventProvider" protokolliert werden soll.

Diese Konfigurationsinformationen weisen das System Überwachungssystem an, alle nicht behandelten Ausnahmen in der Datenbank der Buch Reviews zu protokollieren.

> [!NOTE]
> Das `WebBaseErrorEvent` Ereignis wird nur für Server Fehler ausgelöst. Sie wird nicht für HTTP-Fehler ausgelöst, z. b. eine Anforderung einer nicht gefundenen ASP.NET-Ressource. Dies unterscheidet sich vom Verhalten des `Error` Ereignisses der `HttpApplication` Klasse, das sowohl für Server-als auch für HTTP-Fehler ausgelöst wird.

Um das System Überwachungssystem in Aktion zu sehen, besuchen Sie die Website, und generieren Sie einen Laufzeitfehler, indem Sie `Genre.aspx?ID=foo`besuchen. Die entsprechende Fehlerseite sollte angezeigt werden: entweder der Bildschirm Ausnahme Details gelb (beim lokalen Besuch) oder die benutzerdefinierte Fehlerseite (beim Besuch der Website in der Produktion). Im Hintergrund protokolliert das System Überwachungssystem die Fehlerinformationen in der Datenbank. In der `aspnet_WebEvent_Events` Tabelle sollte ein Datensatz vorhanden sein (siehe **Abbildung 1**). Dieser Datensatz enthält Informationen zum Laufzeitfehler, der soeben aufgetreten ist.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Abbildung 1**: die Fehler Details wurden in der `aspnet_WebEvent_Events` Tabelle protokolliert.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Anzeigen des Fehler Protokolls auf einer Webseite

Mit der aktuellen Konfiguration der Website protokolliert das System Überwachungssystem alle nicht behandelten Ausnahmen in der Datenbank. Die Systemüberwachung bietet jedoch keinen Mechanismus, um das Fehlerprotokoll über eine Webseite anzuzeigen. Sie können jedoch eine ASP.NET-Seite erstellen, auf der diese Informationen aus der Datenbank angezeigt werden. (Wie wir sehen werden, können Sie die Fehlerdetails in einer e-Mail-Nachricht an Sie senden.)

Wenn Sie eine solche Seite erstellen, stellen Sie sicher, dass Sie Maßnahmen ergreifen, damit nur autorisierte Benutzer die Fehlerdetails anzeigen können. Wenn Ihre Website bereits Benutzerkonten verwendet, können Sie URL-Autorisierungs Regeln verwenden, um den Zugriff auf die Seite auf bestimmte Benutzer oder Rollen einzuschränken. Weitere Informationen zum gewähren oder Einschränken des Zugriffs auf Webseiten basierend auf dem angemeldeten Benutzer finden Sie in den Tutorials zu den Sicherheitsprogrammen der [Website](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Im nachfolgenden Tutorial wird ein alternatives Fehler Protokollierungs-und Benachrichtigungssystem mit dem Namen ELMAH behandelt. ELMAH enthält einen integrierten Mechanismus zum Anzeigen des Fehler Protokolls sowohl auf einer Webseite als auch als RSS-Feed.

## <a name="logging-events-to-email"></a>Protokollieren von Ereignissen in e-Mail

Das System Überwachungssystem enthält einen Protokoll Quellen Anbieter, der ein Ereignis in einer e-Mail-Nachricht protokolliert. Die Protokoll Quelle enthält dieselben Informationen, die in der-Datenbank im e-Mail-Nachrichtentext protokolliert werden. Mit dieser Protokoll Quelle können Sie einen Entwickler benachrichtigen, wenn ein bestimmtes Integritäts Überwachungs Ereignis auftritt.

Aktualisieren Sie die Konfiguration der Website "Book Reviews", damit wir immer dann eine e-Mail erhalten, wenn eine Ausnahme auftritt. Um dies zu erreichen, müssen Sie drei Aufgaben ausführen:

1. Konfigurieren Sie die ASP.NET-Webanwendung, um e-Mails zu senden. Dies wird erreicht, indem angegeben wird, wie e-Mail-Nachrichten über das `<system.net>` Konfigurationselement gesendet werden. Weitere Informationen zum Senden von e-Mail-Nachrichten in einer ASP.NET-Anwendung finden Sie unter [Senden von e-Mails in ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) und häufig gestellte Fragen zu [System .net. Mail](http://systemnetmail.com/).
2. Registrieren Sie den e-Mail-Protokoll Quellen Anbieter im `<providers>`-Element, und
3. Fügen Sie dem `<rules>`-Element einen Eintrag hinzu, der dem in Schritt (2) hinzugefügten Protokoll Quellen Anbieter das Ereignis "alle Fehler" zuordnet.

Das System Überwachungssystem enthält zwei Klassen von e-Mail-Protokoll Quellen Anbietern: `SimpleMailWebEventProvider` und `TemplatedMailWebEventProvider`. Die [`SimpleMailWebEventProvider`-Klasse](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) sendet eine nur-Text-e-Mail-Nachricht, die die Ereignis Details enthält und wenig Anpassungen des e-Mail-Texts ermöglicht. Mit der [`TemplatedMailWebEventProvider`-Klasse](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) geben Sie eine ASP.NET-Seite an, deren gerendertes Markup als Text für die e-Mail-Nachricht verwendet wird. Die [`TemplatedMailWebEventProvider`-Klasse](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) ermöglicht Ihnen eine viel bessere Kontrolle über den Inhalt und das Format der e-Mail-Nachricht, erfordert jedoch etwas mehr Vorarbeit, da Sie die ASP.NET-Seite erstellen müssen, die den Nachrichtentext der e-Mail generiert. Dieses Tutorial konzentriert sich auf die Verwendung der `SimpleMailWebEventProvider`-Klasse.

Aktualisieren Sie das `<providers>`-Element des Integritäts Überwachungssystems in der `Web.config`-Datei, um eine Protokoll Quelle für die `SimpleMailWebEventProvider`-Klasse einzuschließen:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

Im obigen Markup wird die `SimpleMailWebEventProvider`-Klasse als Protokoll Quellen Anbieter verwendet und dem anzeigen Amen "emailwebeventprovider" zugewiesen. Darüber hinaus enthält das `<add>`-Attribut zusätzliche Konfigurationsoptionen, wie z. b. die an-und die-Adresse der e-Mail-Nachricht.

Wenn die e-Mail-Protokoll Quelle definiert ist, besteht lediglich die Anweisung, das System Überwachungssystem anzuweisen, diese Quelle zur Protokollierung nicht behandelter Ausnahmen zu verwenden. Dies wird erreicht, indem im `<rules>` Abschnitt eine neue Regel hinzugefügt wird:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

Der Abschnitt "`<rules>`" enthält jetzt zwei Regeln. Der erste mit dem Namen "alle Fehler in e-Mail" sendet alle nicht behandelten Ausnahmen an die Protokoll Quelle "emailwebeventprovider". Diese Regel hat den Effekt, dass Details zu Fehlern auf der Website an die angegebene Adresse gesendet werden. Mit der Regel "alle Fehler in der Datenbank" werden die Fehlerdetails in der Datenbank des Standorts protokolliert. Folglich werden bei jedem Auftreten einer nicht behandelten Ausnahme auf der Website die Details in der Datenbank protokolliert und an die angegebene e-Mail-Adresse gesendet.

**Abbildung 2** zeigt die e-Mail, die von der `SimpleMailWebEventProvider`-Klasse beim Besuch `Genre.aspx?ID=foo`generiert wurde.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Abbildung 2**: die Fehler Details werden in einer e-Mail-Nachricht gesendet.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Zusammenfassung

Das ASP.NET-System Überwachungssystem ist so konzipiert, dass Administratoren die Integrität einer bereitgestellten Webanwendung überwachen können. Ereignisse der Integritäts Überwachung werden ausgelöst, wenn bestimmte Aktionen ausgeführt werden, z. b. wenn die Anwendung beendet wird, wenn sich ein Benutzer erfolgreich bei der Website anmeldet oder wenn eine nicht behandelte Ausnahme auftritt. Diese Ereignisse können auf einer beliebigen Anzahl von Protokoll Quellen protokolliert werden. In diesem Tutorial wurde gezeigt, wie die Details von nicht behandelten Ausnahmen in einer Datenbank und per e-Mail protokolliert werden.

Dieses Tutorial konzentriert sich auf die Verwendung der Integritäts Überwachung zur Protokollierung nicht behandelter Ausnahmen. Beachten Sie jedoch, dass die Integritäts Überwachung so konzipiert ist, dass die Gesamt Integrität einer bereitgestellten ASP.NET-Anwendung gemessen wird, und umfasst eine Vielzahl von Integritäts Überwachungs Ereignissen und Protokoll Quellen Hier untersucht. Darüber hinaus können Sie Ihre eigenen Integritäts Überwachungs Ereignisse und-Protokoll Quellen erstellen, wenn dies erforderlich ist. Wenn Sie mehr über die Integritäts Überwachung erfahren möchten, sollten Sie sich zunächst mit der häufig gestellten Fragen zur System [Überwachung](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)von [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)vertraut machen. Weitere Informationen finden Sie unter Gewusst [wie: Verwenden der Integritäts Überwachung in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx).

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.net Übersicht über die Integritäts Überwachung](https://msdn.microsoft.com/library/bb398933.aspx)
- [Konfigurieren und Anpassen des System Überwachungssystems von ASP.net](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [FAQ: Integritäts Überwachung in ASP.NET 2,0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Gewusst wie: Senden von e-Mails für Integritäts Überwachungs Benachrichtigungen](https://msdn.microsoft.com/library/ms227553.aspx)
- [Vorgehensweise: Verwenden der Integritäts Überwachung in ASP.net](https://msdn.microsoft.com/library/ms998306.aspx)
- [Integritäts Überwachung in ASP.net](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Zurück](processing-unhandled-exceptions-vb.md)
> [Weiter](logging-error-details-with-elmah-vb.md)
