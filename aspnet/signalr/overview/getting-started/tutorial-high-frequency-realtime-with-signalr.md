---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: Erstellen einer Hochfrequenz-Echt Zeit-App mit signalr 2 | Microsoft-Dokumentation'
author: bradygaster
description: In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr verwendet, um hoch frequentes Messaging-Funktionen bereitzustellen.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450117"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Tutorial: Erstellen einer Hochfrequenz-Echt Zeit-App mit signalr 2

In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um hoch frequentes Messaging-Funktionen bereitzustellen. In diesem Fall bedeutet "hoch frequentes Messaging", dass der Server Updates mit fester Geschwindigkeit sendet. Sie senden bis zu 10 Nachrichten pro Sekunde.

Die von Ihnen erstellte Anwendung zeigt eine Form an, die Benutzer ziehen können. Der Server aktualisiert die Position der Form in allen verbundenen Browsern entsprechend der Position der gezogenen Form mithilfe von zeitgesteuerten Updates.

Die in diesem Tutorial eingeführten Konzepte enthalten Anwendungen in Echtzeit-spielen und anderen Simulationsanwendungen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten des Projekts
> * Erstellen der Basisanwendung
> * Beim Starten der APP dem Hub zuordnen
> * Hinzufügen des Clients
> * Ausführen der App
> * Hinzufügen der Client Schleife
> * Hinzufügen der Server Schleife
> * Smooth Animation hinzufügen

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**

## <a name="set-up-the-project"></a>Einrichten des Projekts

In diesem Abschnitt erstellen Sie das Projekt in Visual Studio 2017.

In diesem Abschnitt wird gezeigt, wie Sie Visual Studio 2017 verwenden, um eine leere ASP.NET-Webanwendung zu erstellen und die signalr-und jQuery. UI-Bibliotheken hinzuzufügen.

1. Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.

    ![Web erstellen](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. Lassen Sie im Fenster **neue ASP.NET-Webanwendung-haphapdemo** die Option **leer** , und wählen Sie **OK**aus.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.

1. Wählen Sie unter **Neues Element hinzufügen-kuveshapeer Demo**die Option **installiert** > **Visual C#**  > **Web** > **signalr** aus, und wählen Sie dann **signalr Hub-Klasse (v2)** aus.

1. Nennen Sie die Klasse " *hveshapeer Hub* ", und fügen Sie Sie dem Projekt hinzu.

    In diesem Schritt wird die *MoveShapeHub.cs* -Klassendatei erstellt. Gleichzeitig werden dem Projekt ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die signalr unterstützen.

1. Klicken Sie auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.

1. Führen Sie in der **Paket-Manager-Konsole**den folgenden Befehl aus:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    Mit dem Befehl wird die jQuery-UI-Bibliothek installiert. Sie verwenden Sie zum Animieren der Form.

1. Erweitern Sie in **Projektmappen-Explorer**den Knoten Skripts.

    ![Skript Bibliotheks Verweise](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Skript Bibliotheken für jQuery, jqueryui und signalr sind im Projekt sichtbar.

## <a name="create-the-base-application"></a>Erstellen der Basisanwendung

In diesem Abschnitt erstellen Sie eine Browser Anwendung. Die APP sendet den Speicherort der Form während jedes Maus Verschiebungs Ereignisses an den Server. Der Server überträgt diese Informationen in Echtzeit an alle anderen verbundenen Clients. In späteren Abschnitten erfahren Sie mehr über diese Anwendung.

1. Öffnen Sie die Datei *MoveShapeHub.cs* .

1. Ersetzen Sie den Code in der Datei *MoveShapeHub.cs* durch den folgenden Code:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Speichern Sie die Datei .

Die `MoveShapeHub`-Klasse ist eine Implementierung eines signalr-Hubs. Wie im Lernprogramm für die ersten Schritte [mit signalr](tutorial-getting-started-with-signalr.md) hat der Hub eine Methode, die von den Clients direkt aufgerufen wird. In diesem Fall sendet der Client ein Objekt mit den neuen X-und Y-Koordinaten der Form an den Server. Diese Koordinaten werden an alle anderen verbundenen Clients übertragen. Signalr serialisiert dieses Objekt automatisch mithilfe von JSON.

Die APP sendet das `ShapeModel`-Objekt an den Client. Sie enthält Member, um die Position der Form zu speichern. Die-Version des-Objekts auf dem Server verfügt auch über einen Member, um zu verfolgen, welche Client Daten gespeichert werden. Dieses Objekt verhindert, dass der Server die Daten eines Clients zurück an sich selbst sendet. Dieser Member verwendet das `JsonIgnore`-Attribut, um zu verhindern, dass die Anwendung die Daten serialisiert und an den Client zurücksendet.

## <a name="map-to-the-hub-when-app-starts"></a>Beim Starten der APP dem Hub zuordnen

Als nächstes richten Sie die Zuordnung zum Hub ein, wenn die Anwendung gestartet wird. In signalr 2 wird durch das Hinzufügen einer owin-Startklasse die Zuordnung erstellt.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.

1. Wählen Sie in **Neues Element hinzufügen-muveshapeer Demo** die Option **installiert** > **Visual C#**  > **Web** aus, und wählen Sie dann **owin-Startklasse**aus.

1. Benennen Sie die Klasse *Startup* , und wählen Sie **OK**aus.

1. Ersetzen Sie den Standard Code in der Datei *Startup.cs* durch den folgenden Code:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

Die owin-Startklasse ruft `MapSignalR` auf, wenn die APP die `Configuration`-Methode ausführt. Die APP fügt die Klasse dem Startprozess von owin mithilfe des `OwinStartup` Assembly-Attributs hinzu.

## <a name="add-the-client"></a>Hinzufügen des Clients

Fügen Sie die HTML-Seite für den Client hinzu.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **HTML-Seite** **Hinzufügen** .

1. Benennen Sie die Seite **Standard** , und wählen Sie **OK**aus.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *default. html* , und wählen Sie **als Start Seite festlegen**aus.

1. Ersetzen Sie den Standard Code in der Datei " *default. html* " durch den folgenden Code:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. Erweitern Sie in **Projektmappen-Explorer**den Eintrag **Skripts**.

    Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.

    > [!IMPORTANT]
    > Der Paket-Manager installiert eine spätere Version der signalr-Skripts.

1. Aktualisieren Sie die Skript Verweise im Codeblock, damit Sie den Versionen der Skriptdateien im Projekt entsprechen.

Dieser HTML-und JavaScript-Code erstellt eine rote `div` namens `shape`. Er aktiviert das Zieh Verhalten der Form mithilfe der jQuery-Bibliothek und verwendet das `drag`-Ereignis, um die Position der Form an den Server zu senden.

## <a name="run-the-app"></a>Ausführen der App

Sie können die app ausführen, um Sie zu bearbeiten. Wenn Sie die Form um ein Browserfenster ziehen, bewegt sich auch die Form in den anderen Browsern.

1. Aktivieren Sie auf der Symbolleiste das **Skript Debugging** , und wählen Sie dann die Schaltfläche wiedergeben aus, um die Anwendung im Debugmodus auszuführen.

    ![Screenshot des Benutzers zum Aktivieren des Debugmodus und zum Auswählen von Play.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    In der oberen rechten Ecke wird ein Browserfenster mit der roten Form geöffnet.

1. Kopieren Sie die URL der Seite.

1. Öffnen Sie einen anderen Browser, und fügen Sie die URL in die Adressleiste ein.

1. Ziehen Sie die Form in eines der Browserfenster. Die Form im anderen Browserfenster folgt.

Obwohl die Anwendung mit dieser Methode funktioniert, ist Sie kein empfohlenes Programmiermodell. Es gibt keine Obergrenze für die Anzahl der gesendeten Nachrichten. Folglich werden die Clients und der Server mit den Nachrichten und der Leistung beeinträchtigt. Außerdem zeigt die APP eine disjoinanimation auf dem Client an. Diese ruckartig-Animation wird durchgeführt, da die Form sofort von jeder Methode bewegt wird. Es ist besser, wenn die Form reibungslos an jeden neuen Speicherort verschoben wird. Als Nächstes erfahren Sie, wie Sie diese Probleme beheben.

## <a name="add-the-client-loop"></a>Hinzufügen der Client Schleife

Wenn Sie den Speicherort der Form bei jedem Maus Verschiebungs Ereignis senden, entsteht eine unnötige Menge an Netzwerk Datenverkehr. Die APP muss die Nachrichten vom Client drosseln.

Verwenden Sie die JavaScript-`setInterval`-Funktion, um eine-Schleife einzurichten, die neue Positionsinformationen mit fester Rate an den Server sendet. Diese Schleife ist eine grundlegende Darstellung einer "Game-Schleife". Es handelt sich um eine wiederholt aufgerufene Funktion, die die gesamte Funktionalität eines Spiels steuert.

1. Ersetzen Sie den Client Code in der Datei " *default. html* " durch den folgenden Code:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Sie müssen die Skript Verweise erneut ersetzen. Sie müssen den Versionen der Skripts im Projekt entsprechen.

    Mit diesem neuen Code wird die `updateServerModel`-Funktion hinzugefügt. Sie wird in einer bestimmten Häufigkeit aufgerufen. Die-Funktion sendet die Positionsdaten immer dann an den Server, wenn das `moved`-Flag angibt, dass neue Positionsdaten gesendet werden sollen.

1. Wiedergabe Schaltfläche zum Starten der Anwendung

1. Kopieren Sie die URL der Seite.

1. Öffnen Sie einen anderen Browser, und fügen Sie die URL in die Adressleiste ein.

1. Ziehen Sie die Form in eines der Browserfenster. Die Form im anderen Browserfenster folgt.

Da die APP die Anzahl der Nachrichten drosselt, die an den Server gesendet werden, wird die Animation zuerst nicht als glatt angezeigt.

## <a name="add-the-server-loop"></a>Hinzufügen der Server Schleife

In der aktuellen Anwendung werden Nachrichten, die vom Server an den Client gesendet werden, so oft wie Sie empfangen. Der Netzwerk Datenverkehr weist ein ähnliches Problem auf wie auf dem Client.

Die APP kann Nachrichten häufiger als benötigt senden. Die Verbindung kann daher überflutet werden. In diesem Abschnitt wird beschrieben, wie Sie den Server aktualisieren, um einen Zeitgeber hinzuzufügen, der die Rate der ausgehenden Nachrichten drosselt.

1. Ersetzen Sie den Inhalt `MoveShapeHub.cs` durch den folgenden Code:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Wählen Sie die Wiedergabe Schaltfläche, um die Anwendung zu starten.

1. Kopieren Sie die URL der Seite.

1. Öffnen Sie einen anderen Browser, und fügen Sie die URL in die Adressleiste ein.

1. Ziehen Sie die Form in eines der Browserfenster.

Dieser Code erweitert den Client, um die `Broadcaster`-Klasse hinzuzufügen. Die neue Klasse schränkt die ausgehenden Nachrichten mit der `Timer`-Klasse von .NET Framework ein.

Es ist gut zu lernen, dass der Hub selbst Transitory ist. Es wird jedes Mal erstellt, wenn es benötigt wird. Daher erstellt die APP den `Broadcaster` als Singleton. Sie verwendet die verzögerte Initialisierung, um die Erstellung des `Broadcaster`zu verzögern, bis Sie benötigt wird. Dadurch wird sichergestellt, dass die APP die erste Hub-Instanz vollständig erstellt, bevor der Timer gestartet wird.

Der Aufrufe der `UpdateShape` Funktion der Clients wird dann aus der `UpdateModel`-Methode des Hubs verschoben. Er wird nicht mehr sofort aufgerufen, wenn die APP eingehende Nachrichten empfängt. Stattdessen sendet die APP die Nachrichten mit einer Rate von 25 Aufrufen pro Sekunde an die Clients. Der Prozess wird vom `_broadcastLoop` Timer innerhalb der `Broadcaster`-Klasse verwaltet.

Anstatt die Client Methode direkt vom Hub aufzurufenden zu lassen, muss die `Broadcaster` Klasse einen Verweis auf den aktuell Betriebs `_hubContext` Hub abrufen. Der Verweis wird mit dem `GlobalHost`abgerufen.

## <a name="add-smooth-animation"></a>Smooth Animation hinzufügen

Die Anwendung ist fast fertig, aber wir könnten eine Verbesserung vornehmen. Die app verschiebt die Form auf dem Client als Antwort auf Server Meldungen. Anstatt die Position der Form auf den neuen Speicherort festzulegen, den der Server erhält, verwenden Sie die `animate`-Funktion der jQuery-UI-Bibliothek. Sie kann die Form zwischen der aktuellen und der neuen Position reibungslos verschieben.

1. Aktualisieren Sie die `updateShape`-Methode des Clients in der Datei " *default. html* " so, dass Sie wie der hervorgehobene Code aussieht:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Wählen Sie die Wiedergabe Schaltfläche, um die Anwendung zu starten.

1. Kopieren Sie die URL der Seite.

1. Öffnen Sie einen anderen Browser, und fügen Sie die URL in die Adressleiste ein.

1. Ziehen Sie die Form in eines der Browserfenster.

Die Bewegung der Form im anderen Fenster wird weniger ruckartig angezeigt. Die APP interpoliert die Bewegung im Laufe der Zeit, anstatt Sie einmal pro eingehender Nachricht festzulegen.

Mit diesem Code wird die Form von der alten an die neue Position verschoben. Der Server gibt die Position der Form im Verlauf des Animations Intervalls an. In diesem Fall ist dies 100 Millisekunden. Die APP löscht jede vorherige Animation, die auf der Form ausgeführt wird, bevor die neue Animation gestartet wird.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Das Kommunikations Paradigma, das Sie soeben kennengelernt haben, eignet sich für die Entwicklung von Online spielen und anderen Simulationen, wie z. b. [mit signalr erstellten shootr](https://shootr.azurewebsites.net/)-spielen.

Weitere Informationen zu signalr finden Sie in den folgenden Ressourcen:

* [Signalr-Projekt](http://signalr.net)

* [Signalr GitHub und Beispiele](https://github.com/SignalR/SignalR)

* [Signalr-wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten des Projekts
> * Die Basisanwendung wurde erstellt.
> * Wird dem Hub beim Starten der APP zugeordnet
> * Hinzugefügter Client
> * Die APP wurde ausgeführt.
> * Die Client Schleife wurde hinzugefügt.
> * Die Server Schleife wurde hinzugefügt.
> * Smooth Animation hinzugefügt

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um Server Broadcast Funktionen bereitzustellen.
> [!div class="nextstepaction"]
> [Signalr 2 und Server Übertragung](tutorial-server-broadcast-with-signalr.md)