---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.net Web Forms verbindungsresilienz und Befehls Abfang | Microsoft-Dokumentation
author: Erikre
description: In diesem Tutorial wird beschrieben, wie Sie eine Beispielanwendung so ändern, dass Verbindungs Resilienz und Befehls Abfang Funktion unterstützt
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 95f0b5635c12d5ef88622e5766c1278c6570dd4d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484233"
---
# <a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Verbindungsresilienz von ASP.NET Web Forms und Abfangen von Befehlen

von [Erik Reitan](https://github.com/Erikre)

In diesem Tutorial ändern Sie die Wingtip Toys-Beispielanwendung, um die verbindungsresilienz und die Befehls Abfang Funktion zu unterstützen. Durch die Aktivierung der verbindungsresilienz führt die Wingtip Toys-Beispielanwendung automatisch eine Wiederholung von Daten aufrufen aus, wenn vorübergehende Fehler auftreten, die typisch für eine cloudumgebung sind. Außerdem fängt die Wingtip Toys-Beispielanwendung durch Implementieren der Befehls Abfang Funktion alle SQL-Abfragen ab, die an die Datenbank gesendet werden, um Sie zu protokollieren oder zu ändern.

> [!NOTE] 
> 
> Dieses Web Forms Tutorial basiert auf dem folgenden MVC-Tutorial von Tom Dykstra:  
> [Verbindungsresilienz und Befehls Abfang Funktion mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Bereitstellen der verbindungsresilienz
- Implementierung der Befehls Abfang Funktion

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich, dass die folgende Software auf Ihrem Computer installiert ist, bevor Sie beginnen:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für das Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Der .NET Framework wird automatisch installiert.
- Das Wingtip Toys-Beispiel Projekt, damit Sie die in diesem Tutorial erwähnte Funktionalität im Wingtip Toys-Projekt implementieren können. Der folgende Link enthält Download Details:

    - [Getting Started with ASP.NET 4.5.1 Web Forms-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Bevor Sie dieses Tutorial durcharbeiten, sollten Sie die entsprechenden tutorialreihe in den ersten Schritten [mit ASP.NET 4,5 Web Forms und Visual Studio 2013 über](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)prüfen. In der tutorialreihe erfahren Sie, wie Sie mit dem **wingtiptoys** -Projekt und-Code vertraut werden.

## <a name="connection-resiliency"></a>Verbindungsresilienz

Wenn Sie eine Anwendung in Windows Azure bereitstellen möchten, sollten Sie die Datenbank in **Windows** **Azure SQL-Datenbank**, einem clouddatenbankdienst, bereitstellen. Vorübergehende Verbindungsfehler treten in der Regel häufiger auf, wenn Sie eine Verbindung mit einem clouddatenbankdienst herstellen, als wenn der Webserver und der Datenbankserver direkt zusammen im selben Rechenzentrum verbunden sind. Selbst wenn ein Cloud-Webserver und ein clouddatenbankdienst im selben Rechenzentrum gehostet werden, gibt es mehr Netzwerkverbindungen zwischen Ihnen, die Probleme verursachen können, wie z. b. Load Balancer.

Außerdem wird ein clouddienst in der Regel von anderen Benutzern gemeinsam genutzt, was bedeutet, dass die Reaktionsfähigkeit von Ihnen beeinträchtigt werden kann. Der Zugriff auf die Datenbank unterliegt möglicherweise einer Drosselung. Eine Drosselung bedeutet, dass der Datenbankdienst Ausnahmen auslöst, wenn Sie versuchen, häufiger darauf zuzugreifen, als in Ihrem *Vereinbarung zum Servicelevel* (SLA) zulässig ist.

Viele oder die meisten Verbindungsprobleme, die beim Zugriff auf einen clouddienst auftreten, sind vorübergehend, d. h., Sie lösen sich in kurzer Zeit selbst auf. Wenn Sie also einen Daten Bank Vorgang ausprobieren und einen Fehlertyp abrufen, der in der Regel vorübergehend ist, können Sie den Vorgang nach einer kurzen Wartezeit wiederholen, und der Vorgang ist möglicherweise erfolgreich. Sie können Ihren Benutzern eine viel bessere Benutzerfreundlichkeit bieten, wenn Sie vorübergehende Fehler behandeln, indem Sie automatisch versuchen, die meisten von Ihnen für den Kunden unsichtbar zu machen. Mit dem Feature "verbindungsresilienz" in Entity Framework 6 wird der Prozess der Wiederholung von fehlgeschlagenen SQL-Abfragen automatisiert.

Das Feature für die verbindungsresilienz muss für einen bestimmten Datenbankdienst Ordnungs entsprechend konfiguriert werden:

1. Es muss wissen, welche Ausnahmen wahrscheinlich vorübergehend sind. Sie möchten Fehler erneut versuchen, die durch einen vorübergehenden Verlust der Netzwerk Konnektivität verursacht wurden, nicht jedoch durch Programmfehler verursachte Fehler.
2. Es muss ein entsprechender Zeitraum zwischen den Wiederholungen eines fehlgeschlagenen Vorgangs gewartet werden. Sie können länger zwischen den Wiederholungen für einen Batch Prozess warten, als für eine Online Webseite, auf der ein Benutzer auf eine Antwort wartet.
3. Es muss eine geeignete Anzahl von Wiederholungs versuchen durchführen, bevor es zurückgibt. Möglicherweise möchten Sie in einem Batch Prozess, den Sie in einer Online Anwendung verwenden, mehrmals versuchen, den Vorgang zu wiederholen.

Sie können diese Einstellungen für jede Datenbankumgebung, die von einem Entity Framework-Anbieter unterstützt wird, manuell konfigurieren.

Zum Aktivieren der verbindungsresilienz müssen Sie lediglich eine Klasse in der Assembly erstellen, die von der `DbConfiguration`-Klasse abgeleitet ist, und in dieser Klasse die Ausführungs Strategie der SQL-Datenbank festlegen, die in Entity Framework ein anderer Begriff für die Wiederholungs Richtlinie ist.

### <a name="implementing-connection-resiliency"></a>Implementieren der verbindungsresilienz

1. Laden Sie die [wingtiptoys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) Sample Web Forms-Anwendung in Visual Studio herunter, und öffnen Sie Sie.
2. Fügen Sie im Ordner *Logic* der Anwendung **wingtiptoys** eine Klassendatei mit dem Namen *WingtipToysConfiguration.cs*hinzu.
3. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Der Entity Framework führt den gefundenen Code automatisch in einer Klasse aus, die von `DbConfiguration`abgeleitet ist. Sie können die `DbConfiguration`-Klasse verwenden, um Konfigurationsaufgaben im Code auszuführen, die Sie andernfalls in der Datei " *Web. config" ausführen* würden. Weitere Informationen finden Sie unter " [EntityFramework-Code basierte Konfiguration](https://msdn.microsoft.com/data/jj680699)".

1. Öffnen Sie im Ordner " *Logic* " die Datei *AddProducts.cs* .
2. Fügen Sie eine `using` Anweisung für `System.Data.Entity.Infrastructure` hinzu, wie in gelb hervorgehoben:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Fügen Sie der `AddProduct`-Methode einen `catch`-Block hinzu, damit die `RetryLimitExceededException` protokolliert wird, wie in gelb hervorgehoben:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Wenn Sie die `RetryLimitExceededException` Ausnahme hinzufügen, können Sie eine bessere Protokollierung bereitstellen oder dem Benutzer eine Fehlermeldung anzeigen lassen, wo der Vorgang wiederholt werden kann. Wenn Sie die `RetryLimitExceededException` Ausnahme abfangen, wurden die einzigen Fehler, die wahrscheinlich vorübergehend waren, bereits mehrmals ausprobiert und sind mehrmals fehlgeschlagen. Die tatsächlich zurückgegebene Ausnahme wird in der `RetryLimitExceededException` Ausnahme umschließt. Außerdem haben Sie auch einen allgemeinen catch-Block hinzugefügt. Weitere Informationen zur `RetryLimitExceededException` Ausnahme finden Sie unter [Entity Framework verbindungsresilienz/Wiederholungs Logik](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Befehls Abfang Funktion

Nachdem Sie nun eine Wiederholungs Richtlinie aktiviert haben, wie testen Sie, ob Sie erwartungsgemäß funktioniert? Es ist nicht so einfach, einen vorübergehenden Fehler zu erzwingen, insbesondere wenn Sie lokal ausgeführt werden, und es wäre besonders schwierig, tatsächliche vorübergehende Fehler in einen automatisierten Komponenten Test zu integrieren. Um das Feature für die Verbindungs Resilienz zu testen, benötigen Sie eine Möglichkeit, Abfragen abzufangen, die von Entity Framework an SQL Server gesendet werden, und die SQL Server Antwort durch einen Ausnahmetyp zu ersetzen, der normalerweise vorübergehend ist

Sie können die Abfrage Abfang Funktion auch verwenden, um eine bewährte Vorgehensweise für cloudanwendungen zu implementieren: Protokollieren Sie die Latenzzeit und den Erfolg oder Misserfolg aller Aufrufe externer Dienste wie z. b. Datenbankdienste.

In diesem Abschnitt des Tutorials verwenden Sie die [*Abfang Funktion*](https://msdn.microsoft.com/data/dn469464) des Entity Framework für die Protokollierung und zum simulieren vorübergehender Fehler.

### <a name="create-a-logging-interface-and-class"></a>Erstellen einer Protokollierungs Schnittstelle und-Klasse

Eine bewährte Vorgehensweise für die Protokollierung besteht darin, eine [`interface`](https://msdn.microsoft.com/library/ms173156.aspx) zu verwenden, anstatt hart Codierungs Aufrufe an `System.Diagnostics.Trace` oder eine Protokollierungs Klasse durchzuführen. Dadurch ist es einfacher, den Protokollierungs Mechanismus zu einem späteren Zeitpunkt zu ändern, wenn Sie dies jemals tun müssen. In diesem Abschnitt erstellen Sie also die Protokollierungs Schnittstelle und eine Klasse, um Sie zu implementieren.

Basierend auf dem oben beschriebenen Verfahren haben Sie die **wingtiptoys** -Beispielanwendung in Visual Studio heruntergeladen und geöffnet.

1. Erstellen Sie im Projekt **wingtiptoys** einen Ordner, und nennen Sie ihn *Logging*.
2. Erstellen Sie im Ordner " *Logging* " eine Klassendatei mit dem Namen *ILogger.cs* , und ersetzen Sie den Standard Code durch den folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   Die-Schnittstelle stellt drei Ablauf Verfolgungs Ebenen bereit, um die relative Wichtigkeit von Protokollen anzugeben, und eine, die zum Bereitstellen von Latenz Informationen für externe Dienst Aufrufe wie Datenbankabfragen entworfen wurde Die Protokollierungs Methoden verfügen über über Ladungen, die es Ihnen ermöglichen, eine Ausnahme zu übergeben. Dies ist so, dass Ausnahme Informationen einschließlich Stapel Überwachung und innerer Ausnahmen von der Klasse, die die Schnittstelle implementiert, zuverlässig protokolliert werden, anstatt sich darauf zu verlassen, dass Sie in jedem Protokollierungs Methodenaufrufe in der gesamten Anwendung ausgeführt werden.  
  
   Mit den `TraceApi`-Methoden können Sie die Latenz der einzelnen Aufrufe eines externen Dienstanbieter (z. b. SQL-Datenbank) nachverfolgen.
3. Erstellen Sie im Ordner " *Logging* " eine Klassendatei mit dem Namen *Logger.cs* , und ersetzen Sie den Standard Code durch den folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Die-Implementierung verwendet `System.Diagnostics`, um die Ablauf Verfolgung durchzuführen. Dies ist eine integrierte Funktion von .net, mit der Ablauf Verfolgungs Informationen leicht generiert und verwendet werden können. Es gibt viele &quot;Listener&quot; die Sie mit `System.Diagnostics` Ablauf Verfolgung verwenden können, z.b. zum Schreiben von Protokollen in Dateien oder zum Schreiben der Daten in den BLOB-Speicher in Windows Azure. Weitere Informationen finden Sie unter Problembehandlung für [Windows Azure-Websites in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)unter den Optionen und Links zu anderen Ressourcen. In diesem Tutorial sehen Sie sich nur die Protokolle im Visual Studio- **Ausgabe** Fenster an.

In einer Produktionsanwendung sollten Sie die Verwendung von anderen Ablauf Verfolgungs Frameworks als `System.Diagnostics`in Erwägung gezogen, und die `ILogger`-Schnittstelle macht es relativ einfach, zu einem anderen Ablauf Verfolgungs Mechanismus zu wechseln, wenn Sie sich dafür entscheiden.

### <a name="create-interceptor-classes"></a>Erstellen von Interceptor Klassen

Im nächsten Schritt erstellen Sie die Klassen, die der Entity Framework jedes Mal aufruft, wenn er eine Abfrage an die Datenbank sendet, eine zum simulieren vorübergehender Fehler und eine für die Protokollierung. Diese Interceptor Klassen müssen von der `DbCommandInterceptor`-Klasse abgeleitet werden. Darin schreiben Sie Methoden Überschreibungen, die automatisch aufgerufen werden, wenn die Abfrage gerade ausgeführt wird. In diesen Methoden können Sie die Abfrage überprüfen oder protokollieren, die an die Datenbank gesendet wird, und Sie können die Abfrage ändern, bevor Sie an die Datenbank gesendet wird. Sie können auch einen Entity Framework selbst zurückgeben, ohne die Abfrage an die Datenbank zu übergeben.

1. Um die Interceptor Klasse zu erstellen, die alle SQL-Abfragen protokolliert, bevor Sie an die Datenbank gesendet werden, erstellen Sie eine Klassendatei mit dem Namen *InterceptorLogging.cs* im Ordner " *Logic* ", und ersetzen Sie den Standard Code durch den folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Für erfolgreiche Abfragen oder Befehle schreibt dieser Code ein Informations Protokoll mit Latenz Informationen. Bei Ausnahmen wird ein Fehlerprotokoll erstellt.
2. Um die Interceptor Klasse zu erstellen, die bei der Eingabe &quot;Throw-&quot; im Textfeld **Name** auf der Seite mit dem Namen " *adminpage. aspx*" einen Fehler verursacht, erstellen Sie eine Klassendatei mit dem Namen " *InterceptorTransientErrors.cs* " im Ordner " *Logic* ", und ersetzen Sie den Standard Code durch den folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Dieser Code überschreibt nur die `ReaderExecuting`-Methode, die für Abfragen aufgerufen wird, die mehrere Daten Zeilen zurückgeben können. Wenn Sie die verbindungsresilienz für andere Abfrage Typen überprüfen möchten, können Sie auch die `NonQueryExecuting`-und `ScalarExecuting` Methoden überschreiben, wie der Protokollierungs Interceptor dies tut.  
  
   Melden Sie sich später als "admin" an, und wählen Sie den Link **Admin** in der oberen Navigationsleiste aus. Fügen Sie dann auf der Seite " *adminpage. aspx* " ein Produkt mit dem Namen &quot;Throw&quot;hinzu. Der Code erstellt eine Pseudo-SQL-Datenbank-Ausnahme für die Fehlernummer 20, einen Typ, der in der Regel vorübergehend ist. Andere Fehlernummern, die zurzeit als vorübergehend erkannt werden, sind 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 und 40613. Diese können jedoch in neuen Versionen der SQL-Datenbank geändert werden. Das Produkt wird in "transikoterrorexample" umbenannt, das Sie im Code der *InterceptorTransientErrors.cs* -Datei nachverfolgen können.  
  
   Der Code gibt die Ausnahme zurück, die Entity Framework wird, anstatt die Abfrage zu ausführen und die Ergebnisse zurückzugeben. Die vorübergehende Ausnahme wird *viermal* zurückgegeben, und dann wird der Code auf das normale Verfahren zum übergeben der Abfrage an die Datenbank zurückgesetzt.

    Da alles protokolliert wird, können Sie sehen, dass Entity Framework versucht, die Abfrage vier Mal auszuführen, bevor Sie schließlich erfolgreich ist, und der einzige Unterschied in der Anwendung besteht darin, dass es länger dauert, bis eine Seite mit Abfrage Ergebnissen gerengt wird.  
  
   Die Häufigkeit, mit der das Entity Framework wiederholt wird, ist konfigurierbar. der Code gibt viermal an, da dies der Standardwert für die Ausführungs Richtlinie der SQL-Datenbank ist. Wenn Sie die Ausführungs Richtlinie ändern, ändern Sie auch den Code hier, der angibt, wie oft vorübergehende Fehler generiert werden. Sie können den Code auch so ändern, dass weitere Ausnahmen generiert werden, damit Entity Framework die `RetryLimitExceededException` Ausnahme auslöst.
3. Fügen Sie in *Global. asax*die folgenden using-Anweisungen hinzu:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Fügen Sie dann die markierten Zeilen der `Application_Start`-Methode hinzu:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Diese Codezeilen bewirken, dass der Interceptor Code ausgeführt wird, wenn Entity Framework Abfragen an die Datenbank sendet. Beachten Sie, dass Sie separate Interceptor Klassen für vorübergehende Fehler Simulationen und-Protokollierung erstellt haben, die Sie unabhängig aktivieren und deaktivieren können.   
  
 Sie können Interceptors mit der `DbInterception.Add`-Methode an beliebiger Stelle im Code hinzufügen. Er muss nicht in der `Application_Start`-Methode enthalten sein. Wenn Sie in der `Application_Start`-Methode keine Interceptors hinzugefügt haben, sollten Sie die Klasse mit dem Namen *WingtipToysConfiguration.cs* aktualisieren oder hinzufügen und den obigen Code am Ende des Konstruktors der `WingtipToysConfiguration` Klasse platzieren.

Wenn Sie diesen Code einfügen, achten Sie darauf, `DbInterception.Add` für denselben Interceptor mehrmals auszuführen, oder Sie erhalten zusätzliche Interceptor Instanzen. Wenn Sie z. b. den Protokollierungs Interceptor zweimal hinzufügen, werden für jede SQL-Abfrage zwei Protokolle angezeigt.

Interceptors werden in der Reihenfolge der Registrierung ausgeführt (die Reihenfolge, in der die `DbInterception.Add` Methode aufgerufen wird). Je nachdem, was Sie im Interceptor tun, kann die Reihenfolge von Bedeutung sein. Beispielsweise kann ein Interceptor den SQL-Befehl ändern, der in der `CommandText`-Eigenschaft abgerufen wird. Wenn der SQL-Befehl geändert wird, erhält der nächste Interceptor den geänderten SQL-Befehl, nicht den ursprünglichen SQL-Befehl.

Sie haben den Code für die vorübergehende Fehlersimulation so geschrieben, dass Sie vorübergehende Fehler verursachen können, indem Sie einen anderen Wert in der Benutzeroberfläche eingeben. Als Alternative können Sie den Interceptor Code schreiben, um immer die Sequenz vorübergehender Ausnahmen zu generieren, ohne einen bestimmten Parameterwert zu überprüfen. Sie können dann den Interceptor nur dann hinzufügen, wenn Sie vorübergehende Fehler generieren möchten. Wenn Sie dies jedoch tun, fügen Sie den Interceptor erst hinzu, wenn die Daten Bank Initialisierung abgeschlossen ist. Führen Sie also mindestens einen Daten Bank Vorgang aus, z. b. eine Abfrage für eine ihrer Entitätenmengen, bevor Sie mit der Erstellung vorübergehender Fehler beginnen. Der Entity Framework führt während der Daten Bank Initialisierung mehrere Abfragen aus, und Sie werden nicht in einer Transaktion ausgeführt, sodass Fehler während der Initialisierung dazu führen können, dass der Kontext in einen inkonsistenten Zustand wechselt.

## <a name="test-logging-and-connection-resiliency"></a>Test Protokollierung und verbindungsresilienz

1. Drücken Sie in Visual Studio **F5** , um die Anwendung im Debugmodus auszuführen, und melden Sie sich dann als "admin" mithilfe von "PA $ $Word" als Kennwort an.
2. Wählen Sie oben in der Navigationsleiste **Admin** aus.
3. Geben Sie ein neues Produkt mit dem Namen "throw" mit der entsprechenden Beschreibung, dem Preis und der Bilddatei ein.
4. Klicken Sie auf die Schaltfläche **Produkt hinzufügen** .  
   Sie werden feststellen, dass der Browser einige Sekunden lang nicht reagiert, während Entity Framework die Abfrage mehrmals wiederholt. Der erste Wiederholungsversuch erfolgt sehr schnell, und die Wartezeit wird vor jedem weiteren Wiederholungsversuch erhöht. Dieser Prozess, der vor jedem Wiederholungsversuch länger gewartet wird, wird als *exponentieller Backoff* bezeichnet.
5. Warten Sie, bis die Seite nicht mehr versucht, zu laden.
6. Halten Sie das Projekt an, und überprüfen Sie das Visual Studio- **Ausgabe** Fenster, um die Ablauf Verfolgungs Ausgabe anzuzeigen. Sie finden das Fenster **Ausgabe** , indem Sie **Debuggen** -&gt; **Windows** - -&gt; **Ausgabe**auswählen. Möglicherweise müssen Sie einen Bildlauf nach mehreren anderen Protokollen durchführen  
  
   Beachten Sie, dass Sie die eigentlichen SQL-Abfragen sehen können, die an die Datenbank gesendet werden. Es werden einige anfängliche Abfragen und Befehle angezeigt, die Entity Framework, um zu beginnen und die Datenbankversion und die Migrations Verlaufs Tabelle zu überprüfen.   
    ![Ausgabefenster](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Beachten Sie, dass Sie diesen Test nur wiederholen können, wenn Sie die Anwendung abbrechen und neu starten. Wenn Sie in der Lage sein sollen, die Verbindungs Resilienz mehrmals in einer einzelnen Anwendung auszuführen, können Sie Code schreiben, um den Fehler Zählers in `InterceptorTransientErrors` zurückzusetzen.
7. Um den Unterschied der Ausführungs Strategie (Wiederholungs Richtlinie) anzuzeigen, kommentieren Sie den `SetExecutionStrategy` Zeile in der Datei *WingtipToysConfiguration.cs* im Ordner *Logic* aus, führen Sie die **Administrator** Seite erneut im Debugmodus aus, und fügen Sie das Produkt mit dem Namen &quot;Throw&quot; erneut hinzu.  
  
   Dieses Mal hält der Debugger bei der ersten generierten Ausnahme sofort an, wenn versucht wird, die Abfrage zum ersten Mal auszuführen.  
    ![Debuggen: Details anzeigen](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Heben Sie die Auskommentierung der `SetExecutionStrategy` Zeile in der Datei *WingtipToysConfiguration.cs* auf.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie eine Web Forms Beispielanwendung ändern, um die Verbindungs Resilienz und die Befehls Abfang Funktion zu unterstützen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die verbindungsresilienz und die Befehls Abfang Funktion in ASP.net Web Forms überprüft haben, lesen Sie die ASP.net Web Forms Thema [Asynchronous Methods in ASP.NET 4,5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). In diesem Thema werden die Grundlagen des Aufbaus einer asynchronen ASP.net Web Forms-Anwendung mit Visual Studio erläutert.
