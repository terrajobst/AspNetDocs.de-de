---
uid: signalr/overview/older-versions/dependency-injection
title: Abhängigkeitsinjektion in signalr 1. x | Microsoft-Dokumentation
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: de838ab6b3a299eb1e5ebeb9fa3c583478ce3e56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431541"
---
# <a name="dependency-injection-in-signalr-1x"></a>Abhängigkeitsinjektion in SignalR 1.x

von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Die Abhängigkeitsinjektion ist eine Möglichkeit, hart codierte Abhängigkeiten zwischen Objekten zu entfernen. Dadurch wird das Ersetzen der Abhängigkeiten eines Objekts vereinfacht, entweder zum Testen (mit Pseudo Objekten) oder zum Ändern des Lauf Zeit Verhaltens. In diesem Tutorial wird gezeigt, wie Sie eine Abhängigkeitsinjektion für signalr-Hubs ausführen. Außerdem wird gezeigt, wie IOC-Container mit signalr verwendet werden. Ein IOC-Container ist ein allgemeines Framework für die Abhängigkeitsinjektion.

## <a name="what-is-dependency-injection"></a>Was ist eine Abhängigkeitsinjektion?

Überspringen Sie diesen Abschnitt, wenn Sie bereits mit der Abhängigkeitsinjektion vertraut sind.

Die *Abhängigkeitsinjektion* (di) ist ein Muster, in dem Objekte nicht für das Erstellen eigener Abhängigkeiten verantwortlich sind. Im folgenden finden Sie ein einfaches Beispiel zum motivieren von di. Angenommen, Sie verfügen über ein Objekt, das Nachrichten protokollieren muss. Sie können eine Protokollierungs Schnittstelle definieren:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

In Ihrem-Objekt können Sie eine `ILogger` erstellen, um Meldungen zu protokollieren:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Dies funktioniert, ist aber nicht das beste Design. Wenn Sie `FileLogger` durch eine andere `ILogger` Implementierung ersetzen möchten, müssen Sie `SomeComponent`ändern. Unter der Annahme, dass viele andere Objekte `FileLogger`verwenden, müssen Sie alle ändern. Wenn Sie sich entscheiden, `FileLogger` einen Singleton zu erstellen, müssen Sie auch Änderungen in der gesamten Anwendung vornehmen.

Ein besserer Ansatz besteht darin, eine `ILogger` in das Objekt einzufügen, z. –. mithilfe eines Konstruktorarguments:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Nun ist das Objekt nicht für die Auswahl der zu verwendenden `ILogger` verantwortlich. Sie können `ILogger`-Implementierungen wechseln, ohne die Objekte zu ändern, die von ihm abhängen.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Dieses Muster wird als [Konstruktorinjektion](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)bezeichnet. Ein weiteres Muster ist Setter Injection, bei dem Sie die Abhängigkeit über eine Setter-Methode oder-Eigenschaft festlegen.

## <a name="simple-dependency-injection-in-signalr"></a>Einfache Abhängigkeitsinjektion in signalr

Sehen Sie sich die Chat-Anwendung aus dem Tutorial zu den ersten Schritten [mit signalr](../getting-started/tutorial-getting-started-with-signalr.md)an. Hier ist die Hub-Klasse aus der Anwendung:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Angenommen, Sie möchten Chat Nachrichten auf dem Server speichern, bevor Sie Sie senden. Sie können eine Schnittstelle definieren, die diese Funktionalität abstrahiert, und die Schnittstelle mit di in die `ChatHub` Klasse einfügen.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Das einzige Problem besteht darin, dass eine signalr-Anwendung keine Hubs direkt erstellt. Sie werden von signalr für Sie erstellt. Standardmäßig erwartet signalr, dass eine Hub-Klasse einen Parameter losen Konstruktor hat. Sie können jedoch problemlos eine Funktion registrieren, um Hub-Instanzen zu erstellen, und diese Funktion verwenden, um di auszuführen. Registrieren Sie die Funktion, indem Sie **globalhost. DependencyResolver. Register**aufrufen.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Nun ruft signalr diese anonyme Funktion immer dann auf, wenn eine `ChatHub` Instanz erstellt werden muss.

## <a name="ioc-containers"></a>IOC-Container

Der vorherige Code eignet sich gut für einfache Fälle. Aber trotzdem mussten Sie Folgendes schreiben:

[!code-css[Main](dependency-injection/samples/sample8.css)]

In einer komplexen Anwendung mit vielen Abhängigkeiten müssen Sie möglicherweise einen großen Teil dieses "Verdrahtungs Codes" schreiben. Dieser Code kann schwierig zu verwalten sein, insbesondere, wenn Abhängigkeiten eingebettet sind. Der Komponenten Test ist auch schwierig.

Eine Lösung ist die Verwendung eines IOC-Containers. Ein IOC-Container ist eine Softwarekomponente, die für die Verwaltung von Abhängigkeiten zuständig ist. Sie registrieren Typen beim Container und verwenden dann den Container, um Objekte zu erstellen. Der Container ermittelt automatisch die Abhängigkeitsbeziehungen. Viele IOC-Container ermöglichen es Ihnen außerdem, Dinge wie Objekt Lebensdauer und Bereich zu steuern.

> [!NOTE]
> "IOC" steht für "Inversion of Control", bei dem es sich um ein allgemeines Muster handelt, bei dem ein Framework Anwendungscode aufruft. Ein IOC-Container erstellt Ihre Objekte für Sie, was die übliche Ablauf Steuerung "Rück kehrt".

## <a name="using-ioc-containers-in-signalr"></a>Verwenden von IOC-Containern in signalr

Die Chat-Anwendung ist wahrscheinlich zu einfach für den Vorteil eines IOC-Containers. Sehen wir uns stattdessen das [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) -Beispiel an.

Das StockTicker-Beispiel definiert zwei Hauptklassen:

- `StockTickerHub`: die Hub-Klasse, die Clientverbindungen verwaltet.
- `StockTicker`: ein Singleton mit Aktienkursen, der in regelmäßigen Abständen aktualisiert wird.

`StockTickerHub` enthält einen Verweis auf die `StockTicker` Singleton, während `StockTicker` einen Verweis auf den **ihubconnectioncontext** für die `StockTickerHub`enthält. Diese Schnittstelle wird verwendet, um mit `StockTickerHub` Instanzen zu kommunizieren. (Weitere Informationen finden Sie unter [Server Broadcast mit ASP.net signalr](index.md).)

Wir können einen IOC-Container verwenden, um diese Abhängigkeiten etwas zu lösen. Vereinfachen Sie zunächst die Klassen `StockTickerHub` und `StockTicker`. Im folgenden Code werden die Teile, die wir nicht benötigen, auskommentiert.

Entfernen Sie den Parameter losen Konstruktor aus `StockTicker`. Stattdessen verwenden wir immer di zum Erstellen des Hubs.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Entfernen Sie für StockTicker die Singleton-Instanz. Später verwenden wir den IOC-Container, um die StockTicker-Lebensdauer zu steuern. Legen Sie außerdem den Konstruktor als öffentlich.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Als nächstes können wir den Code umgestalten, indem wir eine Schnittstelle für `StockTicker`erstellen. Wir verwenden diese Schnittstelle, um die `StockTickerHub` von der `StockTicker`-Klasse zu entkoppeln.

Visual Studio macht diese Art von Umgestaltung einfach. Öffnen Sie die Datei StockTicker.cs, klicken Sie mit der rechten Maustaste auf die Deklaration der `StockTicker`-Klasse, und wählen Sie **umgestalten** ... **Schnittstelle extrahieren**.

![](dependency-injection/_static/image1.png)

Klicken Sie im Dialogfeld **Schnittstelle extrahieren** auf **Alles auswählen**. Behalten Sie die restlichen Standardwerte bei. Klicken Sie auf **OK**.

![](dependency-injection/_static/image2.png)

Visual Studio erstellt eine neue Schnittstelle mit dem Namen `IStockTicker`und ändert `StockTicker`, sodass Sie von `IStockTicker`abgeleitet werden.

Öffnen Sie die Datei IStockTicker.cs, und ändern Sie die Schnittstelle in **Public**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

Ändern Sie in der `StockTickerHub`-Klasse die beiden Instanzen von `StockTicker` in `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Das Erstellen einer `IStockTicker` Schnittstelle ist nicht zwingend notwendig, aber ich wollte zeigen, wie di die Kopplung zwischen Komponenten in Ihrer Anwendung reduzieren kann.

## <a name="add-the-ninject-library"></a>Hinzufügen der Ninject-Bibliothek

Es gibt viele Open-Source-IoC-Container für .net. Für dieses Tutorial verwende ich [Ninject](http://www.ninject.org/). (Weitere beliebte Bibliotheken sind [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)und [StructureMap](http://docs.structuremap.net).)

Verwenden Sie den nuget-Paket-Manager zum Installieren der [Ninject-Bibliothek](https://nuget.org/packages/Ninject/3.0.1.10). Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager** > Paket-Manager- **Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Ersetzen des signalr-Abhängigkeits Konflikt Lösers

Um Ninject innerhalb von signalr zu verwenden, erstellen Sie eine Klasse, die von **defaultdependencyresolver**abgeleitet ist.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Diese Klasse überschreibt die **GetService** -Methode und die **GetServices** -Methode von **defaultdependencyresolver**. Signalr ruft diese Methoden auf, um verschiedene Objekte zur Laufzeit zu erstellen, einschließlich hubinstanzen, sowie verschiedene Dienste, die intern von signalr verwendet werden.

- Die **GetService** -Methode erstellt eine einzelne Instanz eines Typs. Überschreiben Sie diese Methode, um die **TryGet** -Methode des Ninject-Kernels aufzurufen. Wenn diese Methode NULL zurückgibt, greifen Sie auf den Standard Konflikt Löser zurück.
- Die **GetServices** -Methode erstellt eine Auflistung von Objekten eines angegebenen Typs. Überschreiben Sie diese Methode, um die Ergebnisse von Ninject mit den Ergebnissen des standardresolvers zu verketten.

## <a name="configure-ninject-bindings"></a>Konfigurieren von Ninject-Bindungen

Nun verwenden wir Ninject, um Typbindungen zu deklarieren.

Öffnen Sie die Datei RegisterHubs.cs. Erstellen Sie in der `RegisterHubs.Start`-Methode den Ninject-Container, der Ninject den *Kernel*aufruft.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Erstellen Sie eine Instanz des benutzerdefinierten Abhängigkeits Konflikt Lösers:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Erstellen Sie eine Bindung für `IStockTicker` wie folgt:

[!code-html[Main](dependency-injection/samples/sample17.html)]

In diesem Code werden zwei Dinge aufgeführt. Wenn die Anwendung eine `IStockTicker`benötigt, sollte der Kernel zunächst eine Instanz von `StockTicker`erstellen. Zweitens sollte die `StockTicker`-Klasse als Singleton-Objekt erstellt werden. Ninject erstellt eine Instanz des-Objekts und gibt für jede Anforderung dieselbe Instanz zurück.

Erstellen Sie wie folgt eine Bindung für " **ihubconnectioncontext** ":

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Dieser Code erstellt eine anonyme Funktion, die eine **ihubconnection**zurückgibt. Die Methode " **accessinjetedinto** " weist Ninject an, diese Funktion nur beim Erstellen von `IStockTicker` Instanzen zu verwenden. Der Grund hierfür ist, dass signalr eine interne **ihubconnectioncontext** -Instanz erstellt und nicht überschreiben soll, wie Sie von signalr erstellt werden. Diese Funktion gilt nur für unsere `StockTicker`-Klasse.

Übergeben Sie den Abhängigkeits Konflikt Löser an die **maphubs** -Methode:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Jetzt verwendet signalr den in **maphubs**angegebenen Konflikt Löser anstelle des Standard Konflikt Lösers.

Hier ist das komplette Codelisting für `RegisterHubs.Start`.

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Drücken Sie F5, um die StockTicker-Anwendung in Visual Studio auszuführen. Navigieren Sie im Browserfenster zu `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Die Anwendung verfügt über genau die gleiche Funktionalität wie zuvor. (Eine Beschreibung finden Sie unter [Server Broadcast mit ASP.net signalr](index.md).) Das Verhalten wurde nicht geändert. der Code wurde einfach getestet, gewartet und entwickelt.
