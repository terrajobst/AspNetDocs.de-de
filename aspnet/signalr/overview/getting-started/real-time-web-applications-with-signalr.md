---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Praktische Übungseinheit: Echt Zeit Webanwendungen mit signalr | Microsoft-Dokumentation'
author: bradygaster
description: In Echt Zeit Webanwendungen ist es möglich, serverseitige Inhalte in Echtzeit auf die verbundenen Clients zu übersetzen. Für ASP.NET-Entwickler, ASP...
ms.author: bradyg
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 9e39fd3f2fc9d4e791002450085215096c222fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431667"
---
# <a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Praktische Übungseinheit: Echt Zeit Webanwendungen mit signalr

vom [Web Camps-Team](https://twitter.com/webcamps)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Herunterladen von Web Camps Trainingskit, Version vom Oktober 2015](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b)

> In Echt Zeit Webanwendungen ist es möglich, serverseitige Inhalte in Echtzeit auf die verbundenen Clients zu übersetzen. Für ASP.NET-Entwickler ist **ASP.net signalr** eine Bibliothek zum Hinzufügen von Echt Zeit Webfunktionen zu Ihren Anwendungen. Es nutzt mehrere Transporte und wählt automatisch den am besten verfügbaren Transport aus, wenn der am besten verfügbare Client und Server verwendet wird. Es nutzt **WebSocket**, eine HTML5-API, die eine bidirektionale Kommunikation zwischen dem Browser und dem Server ermöglicht.
> 
> **Signalr** bietet auch eine einfache API auf hoher Ebene für die Durchführung von Server-zu-Client-RPC (JavaScript-Funktionen in den Browsern Ihrer Clients aus Server seitigem .NET-Code) in Ihrer ASP.NET-Anwendung sowie nützliche Hooks für die Verbindungs Verwaltung, z. b. Verbindungs-/Trennungs Ereignisse, Gruppierungs Verbindungen und Autorisierung.
> 
> **Signalr** ist eine Abstraktion über einige der Transporte, die für die Echt Zeit Arbeit zwischen Client und Server erforderlich sind. Eine **signalr** -Verbindung wird als http gestartet und anschließend zu einer **WebSocket** -Verbindung herauf gestuft, falls verfügbar. **WebSocket** ist der ideale Transport für **signalr**, da der Server Speicher optimal genutzt wird, die geringste Latenz aufweist und die meisten zugrunde liegenden Features (z. b. Vollduplex Kommunikation zwischen Client und Server) vorhanden sind. es gelten jedoch auch die strengsten Anforderungen: **WebSocket** erfordert, dass der Server **Windows Server 2012** oder **Windows 8**zusammen mit **.NET Framework 4,5**verwendet. Wenn diese Anforderungen nicht erfüllt werden, versucht **signalr** , andere Transporte zu verwenden, um die Verbindungen herzustellen (z. b. *AJAX Long*-Abruf).
> 
> Die **signalr** -API enthält zwei Modelle für die Kommunikation zwischen Clients und Servern: **persistente Verbindungen** und **Hubs**. Eine **Verbindung** stellt einen einfachen Endpunkt zum Senden von Nachrichten mit einem einzelnen Empfänger, gruppierten oder Broadcast Nachrichten dar. Ein **Hub** ist eine hochstufige Pipeline, die auf der Verbindungs-API basiert und es dem Client und dem Server ermöglicht, Methoden untereinander direkt aufzurufen.
> 
> ![Signalr-Architektur](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps Training Kit (Version vom Oktober 2015) enthalten, das unter [https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b)verfügbar ist.  Beachten Sie, dass der installerlink auf dieser Seite nicht mehr funktioniert. Verwenden Sie stattdessen einen der Links im Abschnitt Assets.

<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Senden von Benachrichtigungen vom Server an den Client mithilfe von signalr.
- Scale Out Sie die signalr-Anwendung mithilfe **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zuerst Ihre Umgebung einrichten.

1. Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie zum **Quell** Ordner des Labs.
2. Klicken Sie mit der rechten Maustaste auf **Setup. cmd** , und wählen Sie **als Administrator ausführen** aus, um den Setup Vorgang zu starten, mit dem die Umgebung konfiguriert wird, und die Visual Studio-Code Ausschnitte für dieses Lab
3. Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion, um fortzufahren.

> [!NOTE]
> Vergewissern Sie sich, dass Sie vor dem Ausführen des Setups alle Abhängigkeiten für dieses Lab geprüft haben.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden der Code Ausschnitte

Im gesamten Lab-Dokument werden Sie angewiesen, Code Blöcke einzufügen. Der größte Teil dieses Codes wird zur einfacheren Verwendung als Visual Studio Code Ausschnitte bereitgestellt, auf die Sie in Visual Studio 2013 zugreifen können, um das manuelle Hinzufügen zu vermeiden.

> [!NOTE]
> Jede Übung wird von einer Start Lösung begleitet, die sich im Ordner " **Begin** " der Übung befindet und Ihnen ermöglicht, die einzelnen Übungen unabhängig von den anderen zu befolgen. Beachten Sie, dass die während einer Übung hinzugefügten Code Ausschnitte in diesen Projektmappen fehlen und möglicherweise erst nach Abschluss der Übung funktionieren. Innerhalb des Quellcodes für eine Übung finden Sie auch einen **endordner** mit einer Visual Studio-Projekt Mappe mit dem Code, der aus der Ausführung der Schritte in der entsprechenden Übung resultiert. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, während Sie diese praktische Übungseinheit durcharbeiten.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit umfasst die folgenden Übungen:

1. [Arbeiten mit Echtzeitdaten mithilfe von signalr](#Exercise1)
2. [Horizontales Skalieren mithilfe SQL Server](#Exercise2)

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungs Sammlungen auswählen. Jede vordefinierte Sammlung ist so konzipiert, dass Sie einem bestimmten Entwicklungsstil entspricht, und bestimmt Fensterlayouts, das Editor-Verhalten, IntelliSense-Code Ausschnitte und Dialogfeld Optionen. In den Prozeduren in dieser Übungseinheit werden die Aktionen beschrieben, die zum Ausführen einer bestimmten Aufgabe in Visual Studio bei Verwendung der **allgemeinen Entwicklungs Einstellungs** Auflistung erforderlich sind. Wenn Sie eine andere Einstellungs Sammlung für Ihre Entwicklungsumgebung auswählen, kann es Unterschiede in den Schritten geben, die Sie berücksichtigen sollten.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Übung 1: Arbeiten mit Echtzeitdaten mithilfe von signalr

Obwohl Chat häufig als Beispiel verwendet wird, können Sie mit Echtzeit-Webfunktionen viel mehr tun. Jedes Mal, wenn ein Benutzer eine Webseite aktualisiert, um neue Daten anzuzeigen, oder die Seite einen langfristigen AJAX-Abruf zum Abrufen neuer Daten implementiert, können Sie signalr verwenden.

Signalr unterstützt **Serverpush** -oder **Broadcast** Funktionen. die Verbindungs Verwaltung wird automatisch verarbeitet. Bei klassischen http-Verbindungen für die Client-/Serverkommunikation wird die Verbindung für jede Anforderung wieder hergestellt, aber signalr stellt eine permanente Verbindung zwischen dem Client und dem Server bereit. In signalr ruft der Servercode einen Client Code im Browser mithilfe von Remote Prozedur aufrufen (RPC) auf, anstatt das Anforderungs Antwort Modell, das wir heute kennen.

In dieser Übung konfigurieren Sie die **Geek Quiz** -Anwendung so, dass Sie signalr verwendet, um das Statistik Dashboard mit den aktualisierten Metriken anzuzeigen, ohne die gesamte Seite aktualisieren zu müssen.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Aufgabe 1 – Durchsuchen der Seite "Geek Quiz Statistics"

In dieser Aufgabe durchlaufen Sie die Anwendung und überprüfen, wie die Statistikseite angezeigt wird, und wie Sie die Aktualisierung der Informationen verbessern können.

1. Öffnen Sie **Visual Studio Express 2013 für das Web** , und öffnen Sie die Projekt Mappe " **geekquiz. sln** ", die sich im Ordner " **source\ex1-workingwithrealtimedata\begin** " befindet.
2. Drücken Sie **F5** , um die Projekt Mappe auszuführen. Die **Anmelde** Seite sollte im Browser angezeigt werden.

    ![Ausführen der Lösung](real-time-web-applications-with-signalr/_static/image2.png "Ausführen der Lösung")

    *Ausführen der Lösung*
3. Klicken Sie in der oberen rechten Ecke der Seite auf **registrieren** , um einen neuen Benutzer in der Anwendung zu erstellen.

    ![Link registrieren](real-time-web-applications-with-signalr/_static/image3.png "Link registrieren")

    *Link registrieren*
4. Geben Sie auf der **Register** Seite einen **Benutzernamen** und ein **Kennwort**ein, und klicken Sie dann auf **registrieren**.

    ![Registrieren eines Benutzers](real-time-web-applications-with-signalr/_static/image4.png "Registrieren eines Benutzers")

    *Registrieren eines Benutzers*
5. Die Anwendung registriert das neue Konto, und der Benutzer wird authentifiziert und auf die Startseite umgeleitet, auf der die erste Quiz Frage angezeigt wird.
6. Öffnen Sie die Seite **Statistik** in einem neuen Fenster, und legen Sie die Seite **Start** Seite und **Statistik** nebeneinander ab.

    ![Parallele Fenster](real-time-web-applications-with-signalr/_static/image5.png "Nebeneinander Fenster")

    *Parallele Fenster*
7. Beantworten Sie die Frage auf der **Start** Seite, indem Sie auf eine der Optionen klicken.

    ![Beantworten einer Frage](real-time-web-applications-with-signalr/_static/image6.png "Beantworten einer Frage")

    *Beantworten einer Frage*
8. Nachdem Sie auf eine der Schaltflächen geklickt haben, sollte die Antwort angezeigt werden.

    ![Frage richtig beantwortet](real-time-web-applications-with-signalr/_static/image7.png "Frage richtig beantwortet")

    *Frage richtig beantwortet*
9. Beachten Sie, dass die auf der Seite Statistik angegebenen Informationen veraltet sind. Aktualisieren Sie die Seite, um die aktualisierten Ergebnisse anzuzeigen.

    ![Statistik (Seite)](real-time-web-applications-with-signalr/_static/image8.png "Statistik (Seite)")

    *Statistik (Seite)*
10. Wechseln Sie zurück zu Visual Studio, und fahren Sie das Debugging fort.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Aufgabe 2 – Hinzufügen von signalr zum Geek-Quiz zum Anzeigen von Online Diagrammen

In dieser Aufgabe fügen Sie signalr der Lösung hinzu und senden Updates automatisch an die Clients, wenn eine neue Antwort an den Server gesendet wird.

1. Wählen Sie **im Menü Extras** in Visual Studio den **nuget-Paket-Manager**aus, und klicken Sie dann auf Paket-Manager- **Konsole**.
2. Führen Sie im Fenster der **Paket-Manager-Konsole** den folgenden Befehl aus:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Signalr-Paketinstallation](real-time-web-applications-with-signalr/_static/image9.png "Signalr-Paketinstallation")

    *Signalr-Paketinstallation*

   > [!NOTE]
   > Bei der Installation von **signalr** nuget-Paketen Version 2.0.2 aus einer neuen MVC 5-Anwendung müssen Sie **owin** -Pakete manuell auf Version 2.0.1 (oder höher) aktualisieren, bevor Sie signalr installieren. Hierzu können Sie das folgende Skript in der Paket-Manager- **Konsole**ausführen:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > In einer zukünftigen Version von signalr werden owin-Abhängigkeiten automatisch aktualisiert.
3. Erweitern Sie in **Projektmappen-Explorer**den Ordner **Skripts** , und beachten Sie, dass die signalr *js* -Dateien der Projekt Mappe hinzugefügt wurden.

    ![Signalr-JavaScript-Verweise](real-time-web-applications-with-signalr/_static/image10.png "Signalr-JavaScript-Verweise")

    *Signalr-JavaScript-Verweise*
4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das **geekquiz** -Projekt, wählen Sie | **neuen Ordner** **Hinzufügen** aus, und nennen Sie es **Hubs**.
5. Klicken Sie mit der rechten Maustaste auf den Ordner **Hubs** , **und wählen Sie Neues Element**.

    ![Neues Element hinzufügen](real-time-web-applications-with-signalr/_static/image11.png "Neues Element hinzufügen")

    *Neues Element hinzufügen*
6. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die **Visualisierung C# aus. Web | Signalr** -Knoten im linken Bereich Wählen Sie **signalr Hub-Klasse (v2)** im mittleren Bereich aus, benennen Sie die Datei **StatisticsHub.cs** , und klicken Sie auf **Hinzufügen**.

    ![Dialogfeld "Neues Element hinzufügen"](real-time-web-applications-with-signalr/_static/image12.png "Dialogfeld "Neues Element hinzufügen"")

    *Dialogfeld "Neues Element hinzufügen"*
7. Ersetzen Sie den Code in der **statisticshub** -Klasse durch den folgenden Code.

    (Code Ausschnitt- *realtimesignalr-EX1-statisticshubclass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Öffnen Sie **Startup.cs** , und fügen Sie am Ende der **Konfigurations** Methode die folgende Zeile hinzu.

    (Code Ausschnitt- *realtimesignalr-EX1-mapsignalr*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Öffnen Sie im Ordner **Dienste** die Seite **StatisticsService.cs** , und fügen Sie die folgenden using-Direktiven hinzu.

    (Code Ausschnitt- *realtimesignalr-EX1-usingdirektiven*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Um verbundene Clients über Updates zu benachrichtigen, rufen Sie zuerst ein **Kontext** Objekt für die aktuelle Verbindung ab. Das **Hub** -Objekt enthält Methoden zum Senden von Nachrichten an einen einzelnen Client oder zum Übertragen an alle verbundenen Clients. Fügen Sie der **statisticsservice** -Klasse die folgende Methode hinzu, um die Statistikdaten zu übertragen.

    (Code Ausschnitt- *realtimesignalr-EX1-notifyupdatesmethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Im obigen Code verwenden Sie einen beliebigen Methodennamen, um eine Funktion auf dem Client aufzurufen (d.h. *updatestatistik*). Der von Ihnen angegebene Methodenname wird als dynamisches Objekt interpretiert, was bedeutet, dass keine IntelliSense-oder Kompilierzeit Validierung dafür vorliegt. Der Ausdruck wird zur Laufzeit ausgewertet. Wenn der Methodenaufrufe ausgeführt wird, sendet signalr den Methodennamen und die Parameterwerte an den Client. Wenn der Client über eine Methode verfügt, die mit dem Namen übereinstimmt, wird diese Methode aufgerufen, und die Parameterwerte werden an Sie übergeben. Wenn keine übereinstimmende Methode auf dem Client gefunden wird, wird kein Fehler ausgelöst. Weitere Informationen finden Sie unter [ASP.net signalr Hubs API Guide](../guide-to-the-api/hubs-api-guide-server.md).
11. Öffnen Sie die Seite **TriviaController.cs** im Ordner **Controllers** , und fügen Sie die folgenden using-Direktiven hinzu.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Fügen Sie der **Post** -Aktionsmethode den folgenden markierten Code hinzu.

    (Code Ausschnitt- *realtimesignalr-EX1-notifyupdatesaufrufen*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Öffnen Sie die Seite " **Statistics. cshtml** " in den **Ansichten | Basisordner.** Suchen Sie **den Abschnitt Skripts,** und fügen Sie am Anfang des Abschnitts die folgenden Skript Verweise hinzu.

    (Code Ausschnitt- *realtimesignalr-EX1-signalrscriptreferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Wenn Sie signalr und andere Skript Bibliotheken zu Ihrem Visual Studio-Projekt hinzufügen, installiert der Paket-Manager möglicherweise eine Version der signalr-Skriptdatei, die aktueller als die in diesem Thema gezeigte Version ist. Stellen Sie sicher, dass der Skript Verweis im Code mit der Version der Skript Bibliothek übereinstimmt, die in Ihrem Projekt installiert ist.
14. Fügen Sie den folgenden hervorgehobenen Code hinzu, um eine Verbindung zwischen dem Client und dem signalr-Hub herzustellen und die Statistikdaten zu aktualisieren, wenn eine neue Nachricht vom Hub empfangen wird.

    (Code Ausschnitt- *realtimesignalr-EX1-signalrclientcode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    In diesem Code erstellen Sie einen Hub-Proxy und registrieren einen Ereignishandler, um auf vom Server gesendete Nachrichten zu lauschen. In diesem Fall lauschen Sie auf Nachrichten, die über die *UpdateStatistics* -Methode gesendet werden.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Aufgabe 3 – Ausführen der Lösung

In dieser Aufgabe führen Sie die Lösung aus, um zu überprüfen, ob die Statistik Ansicht automatisch mithilfe von signalr aktualisiert wird, nachdem eine neue Frage beantwortet wurde.

1. Drücken Sie **F5** , um die Projekt Mappe auszuführen.

    > [!NOTE]
    > Wenn Sie nicht bereits bei der Anwendung angemeldet sind, melden Sie sich mit dem Benutzer an, den Sie in Aufgabe 1 erstellt haben.
2. Öffnen Sie die Seite **Statistik** in einem neuen Fenster, und legen Sie die Seite **Start** Seite und **Statistik** parallel wie in Aufgabe 1 ab.
3. Beantworten Sie die Frage auf der **Start** Seite, indem Sie auf eine der Optionen klicken.

    ![Beantworten einer anderen Frage](real-time-web-applications-with-signalr/_static/image13.png "Beantworten einer anderen Frage")

    *Beantworten einer anderen Frage*
4. Nachdem Sie auf eine der Schaltflächen geklickt haben, sollte die Antwort angezeigt werden. Beachten Sie, dass die Statistik Informationen auf der Seite automatisch aktualisiert werden, nachdem Sie die Frage mit den aktualisierten Informationen beantwortet haben, ohne die gesamte Seite aktualisieren zu müssen.

    ![Statistikseite nach der Antwort aktualisiert](real-time-web-applications-with-signalr/_static/image14.png "Statistikseite nach der Antwort aktualisiert")

    *Statistikseite nach der Antwort aktualisiert*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Übung 2: horizontales hochskalieren mithilfe SQL Server

Beim *skalieren* einer Webanwendung können Sie im Allgemeinen zwischen den Optionen zum horizontalen hoch-und herunter *skalieren* wählen. Zentrales *hochskalieren* bedeutet die Verwendung eines größeren Servers mit mehr Ressourcen (CPU, RAM usw.), während das horizontale hoch *skalieren* bedeutet, dass weitere Server zur Bewältigung der Auslastung hinzugefügt werden. Das Problem bei letzterem besteht darin, dass die Clients an verschiedene Server weitergeleitet werden können. Ein Client, der mit einem Server verbunden ist, empfängt keine Nachrichten, die von einem anderen Server gesendet werden.

Sie können diese Probleme beheben, indem Sie eine Komponente mit dem Namen *Rückwand*zum Weiterleiten von Nachrichten zwischen Servern verwenden. Wenn eine Rückwand aktiviert ist, sendet jede Anwendungs Instanz Nachrichten an die Rückwand, und die Rückwand leitet Sie an die anderen Anwendungs Instanzen weiter.

Es gibt derzeit drei Typen von Backplane für signalr:

- **Windows-Azure Service Bus**. Service Bus ist eine Messaging Infrastruktur, mit der Komponenten lose gekoppelte Nachrichten senden können.
- **SQL Server**. Die SQL Server Rückwand schreibt Nachrichten in SQL-Tabellen. Die Rückwand verwendet Service Broker für effizientes Messaging. Es funktioniert jedoch auch, wenn Service Broker nicht aktiviert ist.
- **Redis**. Redis ist ein Schlüssel-Wert-Speicher im Arbeitsspeicher. Redis unterstützt ein Veröffentlichen/Abonnieren-Muster ("Pub/Sub") zum Senden von Nachrichten.

Jede Nachricht wird über einen Nachrichtenbus gesendet. Ein Nachrichtenbus implementiert die [imessagebus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) -Schnittstelle, die eine Veröffentlichung/Abonnement-Abstraktion bereitstellt. Die Rückstände funktionieren, indem Sie den standardmäßigen **imessagebus** durch einen Bus ersetzen, der für diese Backplane konzipiert ist.

Jede Serverinstanz stellt über den Bus eine Verbindung mit der Rückwand her. Wenn eine Nachricht gesendet wird, wird Sie an die Rückwand gesendet, und die Rückwand sendet Sie an jeden Server. Wenn ein Server eine Nachricht von der Backplane empfängt, wird die Nachricht im lokalen Cache gespeichert. Der Server übergibt dann Nachrichten aus seinem lokalen Cache an Clients.

Weitere Informationen zur Funktionsweise der signalr-Rückwand finden Sie in diesem [Artikel](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Es gibt einige Szenarien, in denen eine Rückwand zu einem Engpass werden kann. Im folgenden finden Sie einige typische signalr-Szenarien:
> 
> - [Server Broadcast](tutorial-server-broadcast-with-signalr.md) (z. b. Börsen Ticker): Backplane funktionieren für dieses Szenario gut, da der Server die Rate steuert, mit der Nachrichten gesendet werden.
> - [Client-zu-Client](tutorial-getting-started-with-signalr.md) (z. b. Chat): in diesem Szenario kann die Rückwand einen Engpass darstellen, wenn die Anzahl der Nachrichten mit der Anzahl der Clients skaliert wird. Das heißt, wenn die Rate der Nachrichten proportional zunimmt, wenn mehr Clients beitreten.
> - [Hochfrequenz in Echtzeit](tutorial-high-frequency-realtime-with-signalr.md) (z. b. Echt Zeit Spiele): für dieses Szenario wird keine Rückwand empfohlen.

In dieser Übung verwenden Sie **SQL Server** , um Nachrichten über die **Geek Quiz** -Anwendung zu verteilen. Sie führen diese Aufgaben auf einem einzelnen Testcomputer aus, um zu erfahren, wie Sie die Konfiguration einrichten, aber um die vollständige Wirkung zu erzielen, müssen Sie die signalr-Anwendung auf zwei oder mehr Servern bereitstellen. Sie müssen auch SQL Server auf einem der-Server oder auf einem separaten dedizierten Server installieren.

![Scale Out mithilfe SQL Server Diagramms](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Aufgabe 1: Grundlegendes zum Szenario

In dieser Aufgabe führen Sie zwei Instanzen eines Geek- **Quiz** aus, die mehrere IIS-Instanzen auf dem lokalen Computer simulieren. In diesem Szenario wird das Update nicht auf der Seite Statistik der zweiten Instanz benachrichtigt, wenn Sie auf eine Anwendung mit trivialen Fragen Antworten. Diese Simulation ähnelt einer Umgebung, in der Ihre Anwendung auf mehreren Instanzen bereitgestellt wird, und die Verwendung eines Load Balancers, um mit Ihnen zu kommunizieren.

1. Öffnen Sie die Projekt Mappe " **BEGIN. sln** ", die sich im Ordner " **Source/EX2-scalingoutwithsqlserver/BEGIN** " befindet. Nach **Server-Explorer** dem Laden bemerken Sie, dass die Projekt Mappe über zwei Projekte mit identischen Strukturen, aber mit unterschiedlichen Namen verfügt. Dadurch wird die Ausführung von zwei Instanzen der gleichen Anwendung auf dem lokalen Computer simuliert.

    ![Starten der Projekt Mappe simulieren von 2 Instanzen eines Geek-Quiz](real-time-web-applications-with-signalr/_static/image16.png "Starten der Projekt Mappe simulieren von 2 Instanzen eines Geek-Quiz")

    *Starten der Projekt Mappe simulieren von 2 Instanzen eines Geek-Quiz*
2. Öffnen Sie die Eigenschaften Seite der Projekt Mappe, indem Sie mit der rechten Maustaste auf den Projektmappenknoten klicken und **Eigenschaften**auswählen Wählen Sie unter **Startprojekt**die Option **mehrere Start Projekte** aus, und ändern Sie den **Aktions** Wert für beide Projekte in *Start*.

    ![Starten von mehreren Projekten](real-time-web-applications-with-signalr/_static/image17.png "Starten von mehreren Projekten")

    *Starten von mehreren Projekten*
3. Drücken Sie **F5** , um die Projekt Mappe auszuführen. Die Anwendung startet zwei Instanzen von " **Geek Quiz** " in unterschiedlichen Ports und simuliert mehrere Instanzen der gleichen Anwendung. Heften Sie einen der Browser auf der linken Seite an, und klicken Sie auf der rechten Seite des Bildschirms. Melden Sie sich mit Ihren Anmelde Informationen an, oder registrieren Sie einen neuen Benutzer. Wenn Sie angemeldet sind, behalten Sie die Seite Trivia auf der linken Seite, und wechseln Sie im Browser auf der rechten Seite zur Seite **Statistik** .

    ![Geek-Quiz nebeneinander](real-time-web-applications-with-signalr/_static/image18.png)

    *Geek-Quiz nebeneinander*

    ![Geek Quiz in anderen Ports](real-time-web-applications-with-signalr/_static/image19.png)

    *Geek Quiz in anderen Ports*
4. Beginnen Sie mit der Beantwortung von Fragen im linken Browser. Sie werden feststellen, dass die Seite **Statistik** im rechten Browser nicht aktualisiert wird. Der Grund hierfür ist, dass **signalr** einen lokalen Cache verwendet, um Nachrichten auf Clients zu verteilen. in diesem Szenario werden mehrere Instanzen simuliert, sodass der Cache nicht für Sie freigegeben wird. Sie können überprüfen, ob **signalr** funktioniert, indem Sie die gleichen Schritte testen, aber eine einzelne APP verwenden. In den folgenden Aufgaben konfigurieren Sie eine Rückwand, um die Nachrichten über mehrere Instanzen hinweg zu replizieren.
5. Wechseln Sie zurück zu Visual Studio, und fahren Sie das Debugging fort.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Aufgabe 2 – Erstellen der SQL Server Backplane

In dieser Aufgabe erstellen Sie eine Datenbank, die als Rückwand für die Anwendung " **Geek Quiz** " fungieren soll. Verwenden Sie **SQL Server-Objekt-Explorer** , um den Server zu durchsuchen und die Datenbank zu initialisieren. Außerdem aktivieren Sie die **Service Broker**.

1. Öffnen Sie in **Visual Studio**die Menü **Ansicht** , und wählen Sie **SQL Server-Objekt-Explorer**aus.
2. Stellen Sie eine Verbindung mit ihrer localdb-Instanz her, indem Sie mit der rechten Maustaste auf den Knoten **SQL Server** klicken und **SQL Server hinzufügen** auswählen.

    ![Hinzufügen einer SQL Server Instanz](real-time-web-applications-with-signalr/_static/image20.png "Hinzufügen einer SQL Server Instanz")

    *Hinzufügen einer SQL Server Instanz zu SQL Server-Objekt-Explorer*
3. Legen Sie den **Servernamen** auf *(localdb) \v11.0* fest, und lassen Sie **Windows-Authentifizierung** als Authentifizierungsmodus fest. Klicken Sie auf **Verbinden** , um den Vorgang fortzusetzen.

    ![Herstellen einer Verbindung mit localdb](real-time-web-applications-with-signalr/_static/image21.png "Herstellen einer Verbindung mit localdb")

    *Herstellen einer Verbindung mit localdb*
4. Nachdem Sie nun eine Verbindung mit ihrer localdb-Instanz hergestellt haben, müssen Sie eine Datenbank erstellen, die die SQL Server Rückwand für signalr darstellt. Klicken Sie hierzu mit der rechten Maustaste auf den Knoten **Datenbanken** , und wählen Sie **neue Datenbank hinzufügen**aus.

    ![Hinzufügen einer neuen Datenbank](real-time-web-applications-with-signalr/_static/image22.png "Hinzufügen einer neuen Datenbank")

    *Hinzufügen einer neuen Datenbank*
5. Legen Sie den Datenbanknamen auf *signalr* fest, und klicken Sie zum Erstellen auf **OK**

    ![Erstellen der signalr-Datenbank](real-time-web-applications-with-signalr/_static/image23.png "Erstellen der signalr-Datenbank")

    *Erstellen der signalr-Datenbank*

    > [!NOTE]
    > Sie können einen beliebigen Namen für die Datenbank auswählen.
6. Um Updates effizienter von der Backplane zu erhalten, wird empfohlen, Service Broker für die Datenbank zu aktivieren. Service Broker bietet native Unterstützung für Messaging und queuingin SQL Server. Die Rückwand funktioniert auch ohne Service Broker. Öffnen Sie eine neue Abfrage, indem Sie mit der rechten Maustaste auf die Datenbank klicken und **neue Abfrage**auswählen.

    ![Öffnen einer neuen Abfrage](real-time-web-applications-with-signalr/_static/image24.png "Öffnen einer neuen Abfrage")

    *Öffnen einer neuen Abfrage*
7. Um zu überprüfen, ob Service Broker aktiviert ist, Fragen Sie die Spalte **is\_Broker\_aktivierte** Spalte in der **sys. Datenbanken** -Katalog Sicht ab. Führen Sie das folgende Skript im zuletzt geöffneten Abfragefenster aus.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Abfragen des Service Broker Status](real-time-web-applications-with-signalr/_static/image25.png "Abfragen des Service Broker Status")

    *Abfragen des Service Broker Status*
8. Wenn der Wert von **\_Broker\_aktivierte** Spalte in der Datenbank &quot;0&quot;ist, verwenden Sie den folgenden Befehl, um ihn zu aktivieren. Ersetzen Sie **&lt;Your-Database&gt;** durch den Namen, den Sie beim Erstellen der Datenbank festgelegt haben (z. b.: signalr).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Aktivieren von Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Aktivieren von Service Broker")

    *Aktivieren von Service Broker*

    > [!NOTE]
    > Wenn für diese Abfrage ein Deadlock auftritt, stellen Sie sicher, dass keine Anwendungen mit der Datenbank verbunden sind.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Aufgabe 3 – Konfigurieren der signalr-Anwendung

In dieser Aufgabe konfigurieren Sie das **Geek-Quiz** , um eine Verbindung mit der SQL Server Backplane herzustellen. Fügen Sie zunächst das nuget-Paket **signalr. SqlServer** hinzu, und legen Sie die Verbindungs Zeichenfolge auf die Rückwand-Datenbank fest.

1. Öffnen Sie die **Paket-Manager-Konsole** über die **Tools** > **nuget-Paket-Manager**. Stellen Sie sicher, dass **geekquiz** Project in der Dropdown Liste **Standard Projekt** ausgewählt ist. Geben Sie den folgenden Befehl ein, um das nuget-Paket **Microsoft. Aspnet. signalr. SqlServer** zu installieren.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Wiederholen Sie den vorherigen Schritt, aber dieses Mal für Project **GeekQuiz2**.
3. Um die SQL Server Backplane zu konfigurieren, öffnen Sie die Datei **Startup.cs** des **geekquiz** -Projekts, und fügen Sie der **configure** -Methode den folgenden Code hinzu. Ersetzen Sie **&lt;Your-Database&gt;** durch den Datenbanknamen, den Sie beim Erstellen der SQL Server Backplane verwendet haben. Wiederholen Sie diesen Schritt für das **GeekQuiz2** -Projekt.

    (Code Ausschnitt- *realtimesignalr-EX2-startupconfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Nachdem nun beide Projekte für die Verwendung der SQL Server Backplane konfiguriert sind, drücken Sie **F5** , um Sie gleichzeitig auszuführen.
5. Auch hier startet **Visual Studio** zwei Instanzen des **Geek Quiz** in unterschiedlichen Ports. Heften Sie einen der Browser auf der linken Seite und den anderen auf der rechten Seite des Bildschirms an, und melden Sie sich mit Ihren Anmelde Informationen an. Behalten Sie die Seite "Trivia" auf der linken Seite, und wechseln Sie zu **Statistik** pagein der richtigen Browser.
6. Beginnen Sie mit der Beantwortung von Fragen im linken Browser. Dieses Mal wird die **Statistik** Seite dank der Backplane aktualisiert. Wechseln Sie zwischen Anwendungen (**Statistiken** befinden sich jetzt auf der linken Seite und **Trivia** auf der rechten Seite), und wiederholen Sie den Test, um zu überprüfen, ob er für beide Instanzen funktioniert. Die Rückwand dient als frei gegebener Nachrichten *Cache* für jeden verbundenen Server, und jeder Server speichert die Nachrichten in seinem eigenen lokalen Cache, um Sie an verbundene Clients zu verteilen.
7. Wechseln Sie zurück zu Visual Studio, und fahren Sie das Debugging fort.
8. Die SQL Server Rückwand-Komponente generiert automatisch die erforderlichen Tabellen in der angegebenen Datenbank. Öffnen Sie im **SQL Server-Objekt-Explorer** Panel die Datenbank, die Sie für die Rückwand erstellt haben (z. b.: signalr), und erweitern Sie die zugehörigen Tabellen. Die folgenden Tabellen sollten angezeigt werden:

    ![Von der Backplane generierte Tabellen](real-time-web-applications-with-signalr/_static/image27.png)

    *Von der Backplane generierte Tabellen*
9. Klicken Sie mit der rechten Maustaste auf die Tabelle **signalr. Messages\_0** , und wählen Sie **Daten anzeigen**aus.

    ![Anzeigen der Tabelle der signalr-Backplane-Nachrichten](real-time-web-applications-with-signalr/_static/image28.png)

    *Anzeigen der Tabelle der signalr-Backplane-Nachrichten*
10. Sie können die verschiedenen Nachrichten sehen, die an den **Hub** gesendet werden, wenn Sie die trivialen Fragen beantworten. Die Rückwand verteilt diese Nachrichten an eine beliebige verbundene Instanz.

    ![Tabelle "Backplane Messages"](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabelle "Backplane Messages"*

---

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie erfahren, wie Sie **signalr** zu Ihrer Anwendung hinzufügen und mithilfe von **Hubs**Benachrichtigungen vom Server an ihre verbundenen Clients senden. Außerdem haben Sie erfahren, wie Sie Ihre Anwendung mithilfe einer *Rückwand* -Komponente horizontal hochskalieren, wenn Ihre Anwendung in mehreren IIS-Instanzen bereitgestellt wird.
