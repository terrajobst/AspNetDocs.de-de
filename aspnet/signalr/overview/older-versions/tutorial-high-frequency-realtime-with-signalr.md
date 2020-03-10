---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Hochfrequenz in Echtzeit mit signalr 1. x | Microsoft-Dokumentation
author: bradygaster
description: In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr verwendet, um hoch frequentes Messaging-Funktionen bereitzustellen. Hoch frequentes Messaging in...
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: e37e63a410952ec170cbbaadeeb54eae7e82b3b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505713"
---
# <a name="high-frequency-realtime-with-signalr-1x"></a>Häufiges Senden von Echtzeitnachrichten mit SignalR 1.x

von [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr verwendet, um hoch frequentes Messaging-Funktionen bereitzustellen. Hoch frequentes Messaging bedeutet in diesem Fall Updates, die mit fester Geschwindigkeit gesendet werden. im Fall dieser Anwendung werden bis zu 10 Nachrichten pro Sekunde angezeigt.
> 
> Die Anwendung, die Sie in diesem Tutorial erstellen, zeigt eine Form an, die Benutzer ziehen können. Die Position der Form in allen anderen verbundenen Browsern wird dann aktualisiert, um die Position der gezogenen Form mithilfe von zeitgesteuerten Updates abzugleichen.
> 
> Die in diesem Tutorial eingeführten Konzepte enthalten Anwendungen in Echtzeit-spielen und anderen Simulationsanwendungen.
> 
> Kommentare zu diesem Tutorial sind willkommen. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com)veröffentlichen.

## <a name="overview"></a>Übersicht

In diesem Tutorial wird veranschaulicht, wie eine Anwendung erstellt wird, die den Status eines Objekts in Echtzeit mit anderen Browsern teilt. Die Anwendung, die wir erstellen, wird als "muveshape" bezeichnet. Auf der Seite "Seite" wird ein HTML-div-Element angezeigt, das der Benutzer ziehen kann. Wenn der Benutzer das div-Format zieht, wird die neue Position an den Server gesendet, die dann alle anderen verbundenen Clients anweist, die Position der Form entsprechend zu aktualisieren.

![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Die in diesem Tutorial erstellte Anwendung basiert auf einer Demo von Damian Edwards. Ein Video, das diese Demo enthält, finden Sie [hier](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Das Tutorial zeigt zunächst, wie signalr-Nachrichten von jedem Ereignis gesendet werden, das ausgelöst wird, wenn die Form gezogen wird. Jeder verbundene Client aktualisiert dann die Position der lokalen Version der Form, wenn eine Nachricht empfangen wird.

Obwohl die Anwendung mit dieser Methode funktioniert, ist dies kein empfohlenes Programmiermodell, da es keine Obergrenze für die Anzahl der gesendeten Nachrichten gäbe, sodass die Clients und der Server mit Nachrichten überlastet werden könnten und die Leistung beeinträchtigt wird. . Die angezeigte Animation auf dem Client würde ebenfalls getrennt werden, da die Form sofort von jeder Methode verschoben würde, anstatt reibungslos an jeden neuen Speicherort zu wechseln. In den nachfolgenden Abschnitten des Lernprogramms wird veranschaulicht, wie Sie eine Timer-Funktion erstellen, die die maximale Rate einschränkt, mit der Nachrichten vom Client oder Server gesendet werden, und wie Sie die Form reibungslos zwischen den Standorten verschieben. Die endgültige Version der Anwendung, die in diesem Tutorial erstellt wurde, kann aus der [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)heruntergeladen werden.

Dieses Tutorial enthält die folgenden Abschnitte:

- [Erforderliche Komponenten](#prerequisites)
- [Erstellen des Projekts](#createtheproject)
- [Fügen Sie die nuget-Pakete ASP.net signalr und jQuery. UI hinzu.](#nugetpackages)
- [Erstellen der Basisanwendung](#baseapp)
- [Hinzufügen der Client Schleife](#clientloop)
- [Hinzufügen der Server Schleife](#serverloop)
- [Hinzufügen von Smooth Animation auf dem Client](#animation)
- [Weitere Schritte](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial erfordert Visual Studio 2012 oder Visual Studio 2010. Wenn Visual Studio 2010 verwendet wird, verwendet das Projekt .NET Framework 4 anstelle von .NET Framework 4,5.

Wenn Sie Visual Studio 2012 verwenden, empfiehlt es sich, das [ASP.net and Web Tools 2012,2-Update](https://go.microsoft.com/fwlink/?LinkId=282650)zu installieren. Dieses Update enthält neue Features, wie z. b. Verbesserungen bei der Veröffentlichung, neuen Funktionen und neuen Vorlagen.

Wenn Sie über Visual Studio 2010 verfügen, stellen Sie sicher, dass [nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert ist.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Erstellen eines Projekts

In diesem Abschnitt erstellen wir das Projekt in Visual Studio.

1. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
2. Erweitern **C#** Sie im Dialogfeld **Neues Projekt** unter **Vorlagen** , und wählen Sie **Web**aus.
3. Wählen Sie die Vorlage **ASP.net Empty Web Application** *aus, benennen Sie das Projekt mit*dem Namen "", und klicken Sie auf **OK**.

    ![Erstellen des neuen Projekts](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Hinzufügen der signalr-und jQuery. UI-nuget-Pakete

Sie können einem Projekt signalr-Funktionen hinzufügen, indem Sie ein nuget-Paket installieren. In diesem Tutorial wird auch das Paket "jQuery. UI" verwendet, um zuzulassen, dass die Form gezogen und animiert wird.

1. Klicken Sie auf **Tools | Nuget-Paket-Manager | Paket-Manager-Konsole**.
2. Geben Sie den folgenden Befehl in den Paket-Manager ein.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Das signalr-Paket installiert eine Reihe von anderen nuget-Paketen als Abhängigkeiten. Wenn die Installation abgeschlossen ist, verfügen Sie über alle Server-und Client Komponenten, die für die Verwendung von signalr in einer ASP.NET-Anwendung erforderlich sind.
3. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein, um die jQuery-und jQuery. UI-Pakete zu installieren.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Erstellen der Basisanwendung

In diesem Abschnitt erstellen wir eine Browser Anwendung, die während jedes Maus Verschiebungs Ereignisses den Speicherort der Form an den Server sendet. Der Server überträgt diese Informationen dann an alle anderen verbundenen Clients, sobald Sie empfangen werden. Diese Anwendung wird in späteren Abschnitten erweitert.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**, **Klasse...** aus. Nennen Sie die Klasse " **hveshapeer Hub** ", und klicken Sie auf **Hinzufügen**
2. Ersetzen Sie den Code in der neuen Klasse " **hveshapeer Hub** " durch den folgenden Code.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    Die `MoveShapeHub`-Klasse oben ist eine Implementierung eines signalr-Hubs. Wie im Lernprogramm für die ersten Schritte [mit signalr](index.md) hat der Hub eine Methode, die von den Clients direkt aufgerufen wird. In diesem Fall sendet der Client ein Objekt, das die neuen X-und Y-Koordinaten der Form enthält, an den Server, der dann an alle anderen verbundenen Clients gesendet wird. Signalr serialisiert dieses Objekt automatisch mithilfe von JSON.

    Das-Objekt, das an den Client gesendet wird (`ShapeModel`), enthält Elemente, die die Position der Form speichern. Die-Version des-Objekts auf dem Server enthält auch ein Element, mit dem Sie nachverfolgen können, welche Client Daten gespeichert werden, sodass einem bestimmten Client keine eigenen Daten gesendet werden. Dieser Member verwendet das `JsonIgnore`-Attribut, um zu verhindern, dass er serialisiert und an den Client gesendet wird.
3. Als nächstes richten wir den Hub ein, wenn die Anwendung gestartet wird. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen. Globale Anwendungsklasse**. Akzeptieren Sie den Standardnamen *Global* , und klicken Sie auf **OK**.

    ![Globale Anwendungsklasse hinzufügen](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Fügen Sie die folgende `using`-Anweisung nach den bereitgestellten **using** -Anweisungen in der Global.asax.cs-Klasse hinzu.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Fügen Sie in der `Application_Start`-Methode der Global-Klasse die folgende Codezeile hinzu, um die Standardroute für signalr zu registrieren.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Die Datei "Global. asax" sollte wie folgt aussehen:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Als Nächstes fügen wir den Client hinzu. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen. Neues Element**. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **HTML-Seite**aus. Geben Sie der Seite einen passenden Namen ( **z.b. "default. html**"), und klicken Sie auf **Hinzufügen**.
7. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die soeben erstellte Seite, und klicken Sie dann auf **als Start Seite festlegen**.
8. Ersetzen Sie den Standard Code in der HTML-Seite durch den folgenden Code Ausschnitt.

    > [!NOTE]
    > Überprüfen Sie, ob die unten aufgeführten Skript Verweise den Paketen entsprechen, die dem Projekt im Ordner "Scripts" hinzugefügt wurden In Visual Studio 2010 Stimmen die Version von jQuery und signalr, die dem Projekt hinzugefügt wurde, möglicherweise nicht mit den unten aufgeführten Versionsnummern ab.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Der obige HTML-und JavaScript-Code erstellt eine rote div namens Shape, aktiviert das Zieh Verhalten der Form mithilfe der jQuery-Bibliothek und verwendet das `drag`-Ereignis der Form, um die Position der Form an den Server zu senden.
9. Starten Sie die Anwendung, indem Sie F5 drücken. Kopieren Sie die URL der Seite, und fügen Sie Sie in ein zweites Browserfenster ein. Ziehen Sie die Form in eines der Browserfenster. die Form im anderen Browserfenster sollte verschoben werden.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Hinzufügen der Client Schleife

Da der Speicherort der Form bei jedem Maus Verschiebungs Ereignis eine unnötige Menge an Netzwerk Datenverkehr erzeugt, müssen die Nachrichten vom Client gedrosselt werden. Wir verwenden die JavaScript-`setInterval`-Funktion, um eine Schleife einzurichten, die neue Positionsinformationen mit fester Rate an den Server sendet. Diese Schleife ist eine sehr einfache Darstellung einer "Spiel Schleife", einer wiederholt aufgerufenen Funktion, die die gesamte Funktionalität eines Spiels oder einer anderen Simulation antreibt.

1. Aktualisieren Sie den Client Code in der HTML-Seite entsprechend dem folgenden Code Ausschnitt.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    Mit dem obigen Update wird die `updateServerModel`-Funktion hinzugefügt, die in einer festgelegten Häufigkeit aufgerufen wird. Diese Funktion sendet die Positionsdaten immer dann an den Server, wenn das `moved`-Flag angibt, dass neue Positionsdaten zum Senden vorhanden sind.
2. Starten Sie die Anwendung, indem Sie F5 drücken. Kopieren Sie die URL der Seite, und fügen Sie Sie in ein zweites Browserfenster ein. Ziehen Sie die Form in eines der Browserfenster. die Form im anderen Browserfenster sollte verschoben werden. Da die Anzahl der Nachrichten, die an den Server gesendet werden, gedrosselt wird, wird die Animation nicht so reibungslos wie im vorherigen Abschnitt angezeigt.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Hinzufügen der Server Schleife

In der aktuellen Anwendung werden Nachrichten, die vom Server an den Client gesendet werden, so oft wie Sie empfangen. Dies stellt ein ähnliches Problem dar, das auf dem Client aufgetreten ist. Nachrichten können häufiger gesendet werden, als Sie benötigt werden, und die Verbindung könnte als Ergebnis überflutet werden. In diesem Abschnitt wird beschrieben, wie Sie den Server aktualisieren, um einen Timer zu implementieren, der die Rate der ausgehenden Nachrichten drosselt.

1. Ersetzen Sie den Inhalt von `MoveShapeHub.cs` durch den folgenden Code Ausschnitt.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Der obige Code erweitert den Client, um die `Broadcaster`-Klasse hinzuzufügen, die die ausgehenden Nachrichten mit der `Timer`-Klasse von .NET Framework drosselt.

    Da der Hub selbst vorübergehendes ist (er wird jedes Mal erstellt, wenn er benötigt wird), wird der `Broadcaster` als Singleton erstellt. Verzögerte Initialisierung (eingeführt in .NET 4) wird verwendet, um die Erstellung zu verzögern, bis Sie benötigt wird. Dadurch wird sichergestellt, dass die erste Hub-Instanz vollständig erstellt wird, bevor der Timer gestartet wird.

    Der Aufruf der `UpdateShape` Funktion der Clients wird dann aus der `UpdateModel`-Methode des Hubs verschoben, sodass er nicht mehr sofort aufgerufen wird, wenn eingehende Nachrichten empfangen werden. Stattdessen werden die Nachrichten an die Clients mit einer Rate von 25 Aufrufen pro Sekunde gesendet, die vom `_broadcastLoop` Timer innerhalb der `Broadcaster`-Klasse verwaltet werden.

    Anstatt die Client Methode direkt vom Hub aus aufrufen zu können, muss die `Broadcaster`-Klasse einen Verweis auf den aktuellen betriebshub (`_hubContext`) mithilfe des `GlobalHost`abrufen.
2. Starten Sie die Anwendung, indem Sie F5 drücken. Kopieren Sie die URL der Seite, und fügen Sie Sie in ein zweites Browserfenster ein. Ziehen Sie die Form in eines der Browserfenster. die Form im anderen Browserfenster sollte verschoben werden. Es gibt keinen sichtbaren Unterschied im Browser aus dem vorherigen Abschnitt, aber die Anzahl der Nachrichten, die an den Client gesendet werden, wird gedrosselt.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Hinzufügen von Smooth Animation auf dem Client

Die Anwendung ist fast fertig, aber wir könnten in der Bewegung der Form auf dem Client eine weitere Verbesserung vornehmen, da Sie als Reaktion auf Server Nachrichten verschoben wird. Anstatt die Position der Form auf den neuen Speicherort festzulegen, den der Server erhält, verwenden wir die `animate`-Funktion der jQuery-UI-Bibliothek, um die Form zwischen Ihrer aktuellen und neuen Position reibungslos zu verschieben.

1. Aktualisieren Sie die `updateShape`-Methode des Clients so, dass Sie wie der hervorgehobene Code unten aussieht:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Der obige Code verschiebt die Form vom alten Speicherort in den neuen, der vom Server über den Verlauf des Animations Intervalls (in diesem Fall 100 Millisekunden) angegeben wird. Jede vorherige Animation, die auf der Form ausgeführt wird, wird vor dem Start der neuen Animation gelöscht.
2. Starten Sie die Anwendung, indem Sie F5 drücken. Kopieren Sie die URL der Seite, und fügen Sie Sie in ein zweites Browserfenster ein. Ziehen Sie die Form in eines der Browserfenster. die Form im anderen Browserfenster sollte verschoben werden. Die Bewegung der Form im anderen Fenster sollte weniger ruckartig aussehen, da die Bewegung im Laufe der Zeit interpoliert wird, anstatt einmal pro eingehender Nachricht festgelegt zu werden.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Weitere Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine signalr-Anwendung programmieren, die hoch Häufigkeits Meldungen zwischen Clients und Servern sendet. Dieses Kommunikations Paradigma eignet sich für die Entwicklung von Online spielen und anderen Simulationen, wie z. b. [das mit signalr erstellte shootr-Spiel](http://shootr.signalr.net).

Die in diesem Tutorial erstellte komplette Anwendung kann aus der [Code Galerie](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)heruntergeladen werden.

Weitere Informationen zu signalr-Entwicklungskonzepten finden Sie auf den folgenden Websites für signalr-Quellcode und-Ressourcen:

- [Signalr-Projekt](http://signalr.net)
- [Signalr GitHub und Beispiele](https://github.com/SignalR/SignalR)
- [Signalr-wiki](https://github.com/SignalR/SignalR/wiki)
