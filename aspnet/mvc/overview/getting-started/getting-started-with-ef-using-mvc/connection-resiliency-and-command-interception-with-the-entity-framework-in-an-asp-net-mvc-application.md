---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Verwenden der verbindungsresilienz und Befehls Abfang Funktion mit EF in einer ASP.NET MVC-App'
author: tdykstra
description: In diesem Tutorial erfahren Sie, wie Sie verbindungsresilienz und Befehls Abfang Funktion verwenden. Sie sind zwei wichtige Features von Entity Framework 6.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 276266f8ae9df38529d44742ebe6ac0dc8e79727
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471399"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>Tutorial: Verwenden von verbindungsresilienz und Befehls Abfang Funktion mit Entity Framework in einer ASP.NET MVC-App

Bisher wurde die Anwendung lokal in IIS Express auf dem Entwicklungs Computer ausgeführt. Um eine echte Anwendung für andere Personen über das Internet zur Verfügung zu stellen, müssen Sie Sie für einen Webhostinganbieter bereitstellen, und Sie müssen die Datenbank auf einem Datenbankserver bereitstellen.

In diesem Tutorial erfahren Sie, wie Sie verbindungsresilienz und Befehls Abfang Funktion verwenden. Dabei handelt es sich um zwei wichtige Features von Entity Framework 6, die besonders wertvoll sind, wenn Sie in der cloudumgebung bereitstellen: verbindungsresilienz (automatische Wiederholungen bei vorübergehenden Fehlern) und Befehls Abfang Funktion (alle an die Datenbank gesendeten SQL-Abfragen erfassen). , um Sie zu protokollieren oder zu ändern.)

Dieses Tutorial zur Verbindungs Resilienz und zum Abfangen von Befehlen ist optional. Wenn Sie dieses Tutorial überspringen, müssen in den nachfolgenden Tutorials einige geringfügige Anpassungen vorgenommen werden.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Verbindungsresilienz aktivieren
> * Befehls Abfang Funktion aktivieren
> * Testen der neuen Konfiguration

## <a name="prerequisites"></a>Voraussetzungen

* [Sortieren, Filtern und Auslagern](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>Verbindungsresilienz aktivieren

Wenn Sie die Anwendung in Windows Azure bereitstellen, stellen Sie die Datenbank in Windows Azure SQL-Datenbank, einem clouddatenbankdienst, bereit. Vorübergehende Verbindungsfehler treten in der Regel häufiger auf, wenn Sie eine Verbindung mit einem clouddatenbankdienst herstellen, als wenn der Webserver und der Datenbankserver direkt zusammen im selben Rechenzentrum verbunden sind. Selbst wenn ein Cloud-Webserver und ein clouddatenbankdienst im selben Rechenzentrum gehostet werden, gibt es mehr Netzwerkverbindungen zwischen Ihnen, die Probleme verursachen können, wie z. b. Load Balancer.

Außerdem wird ein clouddienst in der Regel von anderen Benutzern gemeinsam genutzt, was bedeutet, dass die Reaktionsfähigkeit von Ihnen beeinträchtigt werden kann. Der Zugriff auf die Datenbank unterliegt möglicherweise einer Drosselung. Eine Drosselung bedeutet, dass der Datenbankdienst Ausnahmen auslöst, wenn Sie versuchen, häufiger darauf zuzugreifen, als in Ihrem Vereinbarung zum Servicelevel (SLA) zulässig ist.

Viele oder die meisten Verbindungsprobleme beim Zugriff auf einen clouddienst sind vorübergehend, d. h., Sie lösen sich in kurzer Zeit selbst auf. Wenn Sie also einen Daten Bank Vorgang ausprobieren und einen Fehlertyp abrufen, der in der Regel vorübergehend ist, können Sie den Vorgang nach einer kurzen Wartezeit wiederholen, und der Vorgang ist möglicherweise erfolgreich. Sie können Ihren Benutzern eine viel bessere Benutzerfreundlichkeit bieten, wenn Sie vorübergehende Fehler behandeln, indem Sie automatisch versuchen, die meisten von Ihnen für den Kunden unsichtbar zu machen. Mit dem Feature "verbindungsresilienz" in Entity Framework 6 wird der Prozess der Wiederholung von fehlgeschlagenen SQL-Abfragen automatisiert.

Das Feature für die verbindungsresilienz muss für einen bestimmten Datenbankdienst Ordnungs entsprechend konfiguriert werden:

- Es muss wissen, welche Ausnahmen wahrscheinlich vorübergehend sind. Sie möchten Fehler erneut versuchen, die durch einen vorübergehenden Verlust der Netzwerk Konnektivität verursacht wurden, nicht jedoch durch Programmfehler verursachte Fehler.
- Es muss ein entsprechender Zeitraum zwischen den Wiederholungen eines fehlgeschlagenen Vorgangs gewartet werden. Sie können länger zwischen den Wiederholungen für einen Batch Prozess warten, als für eine Online Webseite, auf der ein Benutzer auf eine Antwort wartet.
- Es muss eine geeignete Anzahl von Wiederholungs versuchen durchführen, bevor es zurückgibt. Möglicherweise möchten Sie in einem Batch Prozess, den Sie in einer Online Anwendung verwenden, mehrmals versuchen, den Vorgang zu wiederholen.

Sie können diese Einstellungen für jede Datenbankumgebung, die von einem Entity Framework-Anbieter unterstützt wird, manuell konfigurieren. Standardwerte, die in der Regel für eine Online Anwendung, die die Windows Azure SQL-Datenbank verwendet, gut funktionieren, wurden aber bereits für Sie konfiguriert. Dies sind die Einstellungen, die Sie für die Anwendung "" der Anwendung "der Anwendung" implementieren.

Zum Aktivieren der verbindungsresilienz müssen Sie lediglich eine Klasse in der Assembly erstellen, die von der [dbconfiguration](https://msdn.microsoft.com/data/jj680699.aspx) -Klasse abgeleitet ist, und in dieser Klasse die *Ausführungs Strategie*für die SQL-Datenbank festlegen, die in EF ein anderer Begriff für die *Wiederholungs Richtlinie*ist.

1. Fügen Sie im Ordner dal eine Klassendatei mit dem Namen *SchoolConfiguration.cs*hinzu.
2. Ersetzen Sie den Vorlagencode durch den folgenden Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Der Entity Framework führt den gefundenen Code automatisch in einer Klasse aus, die von `DbConfiguration`abgeleitet ist. Sie können die `DbConfiguration`-Klasse verwenden, um Konfigurationsaufgaben im Code auszuführen, die Sie andernfalls in der Datei " *Web. config" ausführen* würden. Weitere Informationen finden Sie unter " [EntityFramework-Code basierte Konfiguration](https://msdn.microsoft.com/data/jj680699)".
3. Fügen Sie in *StudentController.cs*eine `using`-Anweisung für `System.Data.Entity.Infrastructure`hinzu.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Ändern Sie alle `catch` Blöcke, die `DataException` Ausnahmen abfangen, sodass Sie stattdessen `RetryLimitExceededException` Ausnahmen abfangen. Beispiel:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Sie haben `DataException` verwendet, um Fehler zu identifizieren, die möglicherweise flüchtig sind, um eine benutzerfreundliche "try again"-Meldung zu geben. Nachdem Sie jedoch eine Wiederholungs Richtlinie aktiviert haben, sind die einzigen Fehler, die wahrscheinlich vorübergehend waren, bereits aufgetreten und sind mehrmals fehlgeschlagen, und die tatsächlich zurückgegebene Ausnahme wird in der `RetryLimitExceededException` Ausnahme umschließt.

Weitere Informationen finden Sie unter [Entity Framework verbindungsresilienz/Wiederholungs Logik](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Befehls Abfang Funktion aktivieren

Nachdem Sie nun eine Wiederholungs Richtlinie aktiviert haben, wie testen Sie, ob Sie erwartungsgemäß funktioniert? Es ist nicht so einfach, einen vorübergehenden Fehler zu erzwingen, insbesondere wenn Sie lokal ausgeführt werden, und es wäre besonders schwierig, tatsächliche vorübergehende Fehler in einen automatisierten Komponenten Test zu integrieren. Um das Feature für die Verbindungs Resilienz zu testen, benötigen Sie eine Möglichkeit, Abfragen abzufangen, die von Entity Framework an SQL Server gesendet werden, und die SQL Server Antwort durch einen Ausnahmetyp zu ersetzen, der normalerweise vorübergehend ist

Sie können die Abfrage Abfang Funktion auch verwenden, um eine bewährte Vorgehensweise für cloudanwendungen zu implementieren: [Protokollieren Sie die Latenzzeit und den Erfolg oder Misserfolg aller Aufrufe externer Dienste](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) wie z. b. Datenbankdienste. EF6 bietet eine [dedizierte Protokollierungs-API](https://msdn.microsoft.com/data/dn469464) , mit der die Protokollierung vereinfacht werden kann. in diesem Abschnitt des Tutorials erfahren Sie jedoch, wie Sie die [Abfang Funktion](https://msdn.microsoft.com/data/dn469464) der Entity Framework direkt für die Protokollierung und die Simulation vorübergehender Fehler verwenden können.

### <a name="create-a-logging-interface-and-class"></a>Erstellen einer Protokollierungs Schnittstelle und-Klasse

Eine [bewährte Vorgehensweise für die Protokollierung](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) besteht darin, eine Schnittstelle zu verwenden, statt hart Codierungs Aufrufe von System. Diagnostics. Trace oder einer Protokollierungs Klasse durchzuführen. Dadurch ist es einfacher, den Protokollierungs Mechanismus zu einem späteren Zeitpunkt zu ändern, wenn Sie dies jemals tun müssen. In diesem Abschnitt erstellen Sie also die Protokollierungs Schnittstelle und eine Klasse, um Sie zu implementieren./p >

1. Erstellen Sie einen Ordner im Projekt, und nennen Sie ihn *Logging*.
2. Erstellen Sie im Ordner " *Logging* " eine Klassendatei mit dem Namen *ILogger.cs*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Die-Schnittstelle stellt drei Ablauf Verfolgungs Ebenen bereit, um die relative Wichtigkeit von Protokollen anzugeben, und eine, die zum Bereitstellen von Latenz Informationen für externe Dienst Aufrufe wie Datenbankabfragen entworfen wurde Die Protokollierungs Methoden verfügen über über Ladungen, die es Ihnen ermöglichen, eine Ausnahme zu übergeben. Dies ist so, dass Ausnahme Informationen einschließlich Stapel Überwachung und innerer Ausnahmen von der Klasse, die die Schnittstelle implementiert, zuverlässig protokolliert werden, anstatt sich darauf zu verlassen, dass Sie in jedem Protokollierungs Methodenaufrufe in der gesamten Anwendung ausgeführt werden.

    Mit den traceapi-Methoden können Sie die Latenz der einzelnen Aufrufe eines externen Dienstanbieter (z. b. SQL-Datenbank) nachverfolgen.
3. Erstellen Sie im Ordner " *Logging* " eine Klassendatei mit dem Namen *Logger.cs*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Die-Implementierung verwendet System. Diagnostics, um die Ablauf Verfolgung durchzuführen. Dies ist eine integrierte Funktion von .net, mit der Ablauf Verfolgungs Informationen leicht generiert und verwendet werden können. Es gibt viele "Listener", die Sie mit der System. Diagnostics-Ablauf Verfolgung verwenden können, z. b. zum Schreiben von Protokollen in Dateien oder zum Schreiben der Daten in den BLOB-Speicher in Azure. Weitere Informationen zur Problembehandlung von Azure-Websites [in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)finden Sie unter einige der Optionen und Links zu anderen Ressourcen. In diesem Tutorial sehen Sie sich nur die Protokolle im **Ausgabe** Fenster von Visual Studio an.

    In einer Produktionsanwendung sollten Sie die Ablauf Verfolgung von Paketen außer System. Diagnostics in Erwägung gezogen, und die ILogger-Schnittstelle macht es relativ einfach, zu einem anderen Ablauf Verfolgungs Mechanismus zu wechseln, wenn Sie sich dafür entscheiden.

### <a name="create-interceptor-classes"></a>Erstellen von Interceptor Klassen

Im nächsten Schritt erstellen Sie die Klassen, die der Entity Framework jedes Mal aufruft, wenn er eine Abfrage an die Datenbank sendet, eine zum simulieren vorübergehender Fehler und eine für die Protokollierung. Diese Interceptor Klassen müssen von der `DbCommandInterceptor`-Klasse abgeleitet werden. Darin schreiben Sie Methoden Überschreibungen, die automatisch aufgerufen werden, wenn die Abfrage ausgeführt wird. In diesen Methoden können Sie die Abfrage überprüfen oder protokollieren, die an die Datenbank gesendet wird, und Sie können die Abfrage ändern, bevor Sie an die Datenbank gesendet wird. Sie können auch einen Entity Framework selbst zurückgeben, ohne die Abfrage an die Datenbank zu übergeben.

1. Um die Interceptor Klasse zu erstellen, die jede SQL-Abfrage protokolliert, die an die Datenbank gesendet wird, erstellen Sie eine Klassendatei mit dem Namen *SchoolInterceptorLogging.cs* im Ordner *dal* , und ersetzen Sie den Vorlagen Code durch den folgenden Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Für erfolgreiche Abfragen oder Befehle schreibt dieser Code ein Informations Protokoll mit Latenz Informationen. Bei Ausnahmen wird ein Fehlerprotokoll erstellt.
2. Erstellen Sie eine Klassendatei mit dem Namen *SchoolInterceptorTransientErrors.cs* im Ordner *dal* , und ersetzen Sie den Vorlagen Code durch den folgenden Code, um die Interceptor Klasse zu erstellen, die bei der Eingabe von "throw" in das **Suchfeld** einen vorübergehenden Fehler verursacht:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Dieser Code überschreibt nur die `ReaderExecuting`-Methode, die für Abfragen aufgerufen wird, die mehrere Daten Zeilen zurückgeben können. Wenn Sie die verbindungsresilienz für andere Abfrage Typen überprüfen möchten, können Sie auch die `NonQueryExecuting`-und `ScalarExecuting` Methoden überschreiben, wie der Protokollierungs Interceptor dies tut.

    Wenn Sie die Seite Student ausführen und "throw" als Such Zeichenfolge eingeben, erstellt dieser Code eine Pseudo-SQL-Daten Bank Ausnahme für die Fehlernummer 20, einen Typ, der in der Regel vorübergehend ist. Andere Fehlernummern, die zurzeit als vorübergehend erkannt werden, sind 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 und 40613. Diese können jedoch in neuen Versionen der SQL-Datenbank geändert werden.

    Der Code gibt die Ausnahme zurück, die Entity Framework wird, anstatt die Abfrage zu ausführen und die Abfrageergebnisse zurückzugeben. Die vorübergehende Ausnahme wird viermal zurückgegeben, und dann wird der Code auf das normale Verfahren zum übergeben der Abfrage an die Datenbank zurückgesetzt.

    Da alles protokolliert wird, können Sie sehen, dass Entity Framework versucht, die Abfrage vier Mal auszuführen, bevor Sie schließlich erfolgreich ist, und der einzige Unterschied in der Anwendung besteht darin, dass es länger dauert, bis eine Seite mit Abfrage Ergebnissen gerengt wird.

    Die Häufigkeit, mit der das Entity Framework wiederholt wird, ist konfigurierbar. der Code gibt viermal an, da dies der Standardwert für die Ausführungs Richtlinie der SQL-Datenbank ist. Wenn Sie die Ausführungs Richtlinie ändern, ändern Sie auch den Code hier, der angibt, wie oft vorübergehende Fehler generiert werden. Sie können den Code auch so ändern, dass weitere Ausnahmen generiert werden, damit Entity Framework die `RetryLimitExceededException` Ausnahme auslöst.

    Der Wert, den Sie im Suchfeld eingeben, befindet sich in `command.Parameters[0]` und `command.Parameters[1]` (eine wird für den Vornamen und eine für den Nachnamen verwendet). Wenn der Wert "% throw%" gefunden wird, wird "throw" in diesen Parametern durch "an" ersetzt, sodass einige Schüler und Studenten gefunden und zurückgegeben werden.

    Dies ist nur eine bequeme Methode zum Testen der verbindungsresilienz, basierend auf der Änderung von Eingaben in die Benutzeroberfläche der Anwendung. Sie können auch Code schreiben, der vorübergehende Fehler für alle Abfragen oder Updates generiert, wie weiter unten in den Kommentaren zur *dbintercep. Add* -Methode erläutert.
3. Fügen Sie in *Global. asax*die folgenden `using`-Anweisungen hinzu:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Fügen Sie die markierten Zeilen der `Application_Start`-Methode hinzu:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Diese Codezeilen bewirken, dass der Interceptor Code ausgeführt wird, wenn Entity Framework Abfragen an die Datenbank sendet. Beachten Sie, dass Sie separate Interceptor Klassen für vorübergehende Fehler Simulationen und-Protokollierung erstellt haben, die Sie unabhängig aktivieren und deaktivieren können.

    Sie können Interceptors mit der `DbInterception.Add`-Methode an beliebiger Stelle im Code hinzufügen. Er muss nicht in der `Application_Start`-Methode enthalten sein. Eine weitere Möglichkeit besteht darin, diesen Code in der zuvor erstellten dbconfiguration-Klasse zu platzieren, um die Ausführungs Richtlinie zu konfigurieren.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Wenn Sie diesen Code einfügen, achten Sie darauf, `DbInterception.Add` für denselben Interceptor mehrmals auszuführen, oder Sie erhalten zusätzliche Interceptor Instanzen. Wenn Sie z. b. den Protokollierungs Interceptor zweimal hinzufügen, werden für jede SQL-Abfrage zwei Protokolle angezeigt.

    Interceptors werden in der Reihenfolge der Registrierung ausgeführt (die Reihenfolge, in der die `DbInterception.Add` Methode aufgerufen wird). Je nachdem, was Sie im Interceptor tun, kann die Reihenfolge von Bedeutung sein. Beispielsweise kann ein Interceptor den SQL-Befehl ändern, der in der `CommandText`-Eigenschaft abgerufen wird. Wenn der SQL-Befehl geändert wird, erhält der nächste Interceptor den geänderten SQL-Befehl, nicht den ursprünglichen SQL-Befehl.

    Sie haben den Code für die vorübergehende Fehlersimulation so geschrieben, dass Sie vorübergehende Fehler verursachen können, indem Sie einen anderen Wert in der Benutzeroberfläche eingeben. Als Alternative können Sie den Interceptor Code schreiben, um immer die Sequenz vorübergehender Ausnahmen zu generieren, ohne einen bestimmten Parameterwert zu überprüfen. Sie können dann den Interceptor nur dann hinzufügen, wenn Sie vorübergehende Fehler generieren möchten. Wenn Sie dies jedoch tun, fügen Sie den Interceptor erst hinzu, wenn die Daten Bank Initialisierung abgeschlossen ist. Führen Sie also mindestens einen Daten Bank Vorgang aus, z. b. eine Abfrage für eine ihrer Entitätenmengen, bevor Sie mit der Erstellung vorübergehender Fehler beginnen. Der Entity Framework führt während der Daten Bank Initialisierung mehrere Abfragen aus, und Sie werden nicht in einer Transaktion ausgeführt, sodass Fehler während der Initialisierung dazu führen können, dass der Kontext in einen inkonsistenten Zustand wechselt.

## <a name="test-the-new-configuration"></a>Testen der neuen Konfiguration

1. Drücken Sie **F5** , um die Anwendung im Debugmodus auszuführen, und klicken Sie dann auf die Registerkarte **Studenten** .
2. Sehen Sie sich das **Ausgabe** Fenster von Visual Studio an, um die Ablauf Verfolgungs Ausgabe anzuzeigen. Möglicherweise müssen Sie in einigen JavaScript-Fehlern einen Bildlauf nach oben durchführen, um die von ihrer Protokollierung geschriebenen Protokolle zu erhalten.

    Beachten Sie, dass Sie die eigentlichen SQL-Abfragen sehen können, die an die Datenbank gesendet werden. Sie sehen einige erste Abfragen und Befehle, die Entity Framework, um zu beginnen, und überprüfen die Datenbankversion und die Migrations Verlaufs Tabelle (im nächsten Tutorial erfahren Sie mehr über Migrationen). Und Sie sehen eine Abfrage für das Paging, um herauszufinden, wie viele Studenten vorhanden sind, und schließlich wird die Abfrage angezeigt, mit der die Studenten Daten abgerufen werden.

    ![Protokollierung für normale Abfrage](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Geben Sie auf der Seite **Studenten** "throw" als Such Zeichenfolge ein, und klicken Sie auf **Suchen**.

    ![Such Zeichenfolge auslösen](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Sie werden feststellen, dass der Browser einige Sekunden lang nicht reagiert, während Entity Framework die Abfrage mehrmals wiederholt. Der erste Wiederholungsversuch erfolgt sehr schnell, und der Warte Vorgang vor jeder zusätzlichen Wiederholung nimmt zu. Dieser Prozess, der vor jedem Wiederholungsversuch länger gewartet wird, wird als *exponentieller Backoff*bezeichnet.

    Wenn die Seite angezeigt wird, die Schüler/Studenten mit "an" in ihren Namen anzeigt, sehen Sie sich das Fenster Ausgabe an, und Sie sehen, dass die gleiche Abfrage fünfmal versucht wurde, wobei die ersten vier Male vorübergehende Ausnahmen zurückgeben. Für jeden vorübergehenden Fehler wird das Protokoll angezeigt, das Sie beim Erzeugen des vorübergehenden Fehlers in der `SchoolInterceptorTransientErrors`-Klasse schreiben ("Rückgabe vorübergehenden Fehler für den Befehl..."), und das Protokoll wird geschrieben, wenn `SchoolInterceptorLogging` die Ausnahme abruft.

    ![Protokollierungs Ausgabe mit Wiederholungen](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Da Sie eine Such Zeichenfolge eingegeben haben, wird die Abfrage, die die Studenten Daten zurückgibt, parametrisiert:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Sie protokollieren nicht den Wert der Parameter, aber Sie könnten dies tun. Wenn Sie die Parameterwerte anzeigen möchten, können Sie Protokollierungs Code schreiben, um Parameterwerte aus der `Parameters`-Eigenschaft des `DbCommand`-Objekts zu erhalten, das Sie in den Interceptor Methoden erhalten.

    Beachten Sie, dass Sie diesen Test nur wiederholen können, wenn Sie die Anwendung abbrechen und neu starten. Wenn Sie in der Lage sein sollen, die Verbindungs Resilienz mehrmals in einer einzelnen Anwendung auszuführen, können Sie Code schreiben, um den Fehler Zählers in `SchoolInterceptorTransientErrors`zurückzusetzen.
4. Um den Unterschied der Ausführungs Strategie (Wiederholungs Richtlinie) anzuzeigen, kommentieren Sie die `SetExecutionStrategy` Zeile in *SchoolConfiguration.cs*aus, führen Sie die Seite "Studenten" erneut im Debugmodus aus, und suchen Sie erneut nach "throw".

    Dieses Mal hält der Debugger bei der ersten generierten Ausnahme sofort an, wenn versucht wird, die Abfrage zum ersten Mal auszuführen.

    ![Dummy-Ausnahme](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Heben Sie die Auskommentierung der *texecutionstrategy* -Zeile in *SchoolConfiguration.cs*auf.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Verbindungsresilienz aktiviert
> * Befehls Abfang Funktion aktiviert
> * Die neue Konfiguration wurde getestet.

Im nächsten Artikel erfahren Sie mehr über Code First Migrationen und die Azure-Bereitstellung.
> [!div class="nextstepaction"]
> [Code First Migrationen und Azure-Bereitstellung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
