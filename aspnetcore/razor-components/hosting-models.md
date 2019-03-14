---
title: Hostingmodelle Razor-Komponenten
author: guardrex
description: Verstehen der clientseitigen Blazor und ASP.NET Core Razor Serverkomponenten Hostingmodelle.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/hosting-models
ms.openlocfilehash: d1e0c472d7d10eeb4cef0da735cf703c98dd1645
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042437"
---
# <a name="razor-components-hosting-models"></a>Hostingmodelle Razor-Komponenten

Durch [Daniel Roth](https://github.com/danroth27)

Razor-Komponenten ist ein Webframework, das für die clientseitige Ausführung vorgesehen im Browser auf eine WebAssembly basierenden .NET Common Language Runtime (*Blazor*) oder serverseitige in ASP.NET Core (*ASP.NET Core-Razor-Komponenten*). Unabhängig von dem Modell, die app und die Komponente Hostingmodelle *unverändert*. Dieser Artikel beschreibt die verfügbaren Hostingmodelle.

## <a name="client-side-hosting-model"></a>Clientseitiges Hostingmodell

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Das Dienstprinzipale Hostingmodell für Blazor ist ausgeführten Client-Seite im Browser. In diesem Modell werden die Blazor-app, ihre Abhängigkeiten und die .NET Runtime an den Browser heruntergeladen. Die App wird direkt im UI-Thread des Browsers ausgeführt. Alle Aktualisierungen der Benutzeroberfläche und Behandlung von Ereignissen tritt innerhalb des gleichen Prozesses. Die app-Ressourcen können als statische Dateien, die mit den Webserver bevorzugt wird bereitgestellt werden (finden Sie unter [hosten und Bereitstellen von](xref:host-and-deploy/razor-components/index)).

![Blazor clientseitig: Die Blazor-Anwendung, die in einem UI-Thread im Browser ausgeführt werden.](hosting-models/_static/client-side.png)

Verwenden Sie zum Erstellen einer Blazor-app, die mit das Hostingmodell für die clientseitige der **Blazor** oder **Blazor (ASP.NET Core gehostet)** -Projektvorlagen (`blazor` oder `blazorhosted` Vorlage, wenn Sie die verwenden[Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl an einer Eingabeaufforderung). Die enthaltenen *blazor.webassembly.js* Skript behandelt:

* Herunterladen von .NET Runtime, die app und die dazugehörigen Abhängigkeiten.
* Die Initialisierung der Runtime, um die app ausführen.

Das Hostingmodell für die clientseitige bietet mehrere Vorteile. Die clientseitige Blazor:

* Verfügt über keine serverseitige-Abhängigkeit von .NET.
* Verfügt über eine umfangreiche interaktive Benutzeroberfläche.
* Vollständig nutzt die Clientressourcen und Fähigkeiten.
* Abladungen Arbeit vom Server an den Client.
* Unterstützt die offline-Szenarien.

Einige Nachteile der clientseitigen hosten. Die clientseitige Blazor:

* Wird die app auf die Funktionen des Browsers beschränkt.
* Erfordert, kann der Clienthardware und Software (z. B. WebAssembly unterstützt).
* Verfügt über eine größere Downloadgröße und länger Ladevorgang der app.
* Verfügt über weniger Reife der .NET Common Language Runtime und toolunterstützung (z. B. Einschränkungen in .NET Standard-Support und Debuggen).

Visual Studio enthält die **Blazor (ASP.NET Core gehostet)** Projektvorlage zum Erstellen einer Blazor-app, die auf WebAssembly ausgeführt wird, und klicken Sie auf eine ASP.NET Core-Server gehostet wird. Die ASP.NET Core-app ist ein separater Prozess aber dient die Blazor-app-Clients. Die clientseitige Blazor-app kann über das Netzwerk mithilfe von Web-API-Aufrufe oder SignalR-Verbindungen mit dem Server interagieren.

> [!IMPORTANT]
> Wenn eine clientseitige Blazor-app von einer ASP.NET Core-app, die als IIS-Sub-app gehostet bedient wird, deaktivieren Sie die geerbte Handler von ASP.NET Core-Modul. Legen Sie die app-Basispfad der Blazor App *"Index.HTML"* Datei an den IIS-Alias verwendet, wenn die untergeordneten app in IIS zu konfigurieren.
>
> Weitere Informationen finden Sie unter [App-Basispfad](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="server-side-hosting-model"></a>Serverseitiges Hostingmodell

In das ASP.NET Core-Razor-Komponenten serverseitige hosting-Modell wird die app auf dem Server in einer ASP.NET Core-app ausgeführt. Benutzeroberflächenupdates, Ereignisbehandlung und JavaScript-Aufrufe werden über eine SignalR-Verbindung verarbeitet.

![ASP.NET Core-Razor-Komponenten Serverseitig: Der Browser interagiert mit der app, die (gehostet in einer ASP.NET Core-app) auf dem Server über eine SignalR-Verbindung.](hosting-models/_static/server-side.png)

Verwenden Sie zum Erstellen einer Razor-Komponenten-app, die mit das Hostingmodell für die serverseitige der **Blazor (serverseitig in ASP.NET Core)** Vorlage (`blazorserver` Verwendung [Dotnet neue](/dotnet/core/tools/dotnet-new) an einer Eingabeaufforderung). Eine ASP.NET Core-app hostet die serverseitige app Razor-Komponenten und richtet den SignalR-Endpunkt, in dem Verbinden von Clients. Die ASP.NET Core-app verweist auf der app `Startup` Klasse hinzufügen:

* Serverseitige Komponenten der Razor-Dienste.
* Die app die Anforderungspipeline.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

Die *blazor.server.js* Skript&dagger; stellt die Clientverbindung her. Es ist der Zuständigkeit der app zum beibehalten und Wiederherstellen von app-Status nach Bedarf (z. B. im Falle einer Netzwerkverbindung verloren).

Das Hostingmodell für die serverseitige bietet mehrere Vorteile:

* Können Sie zum Schreiben der gesamten Apps mit .NET und C# mithilfe des Komponentenmodells.
* Bietet eine umfassende interaktive Verhalten und vermeidet unnötige Seite wird aktualisiert.
* Verfügt über eine erheblich kleinere app-Größe als eine clientseitige Blazor-app aus, und viel schneller geladen.
* Komponente Logik kann vollständig von Serverfunktionen, einschließlich der Verwendung von .NET Core kompatible APIs nutzen.
* Wird mit .NET Core auf dem Server ausgeführt, sodass es sich bei vorhandenen .NET Tools, z. B. Debuggen, wie erwartet funktioniert.
* Funktioniert mit thin Clients (z. B. Browser, WebAssembly und die Ressourcengruppe nicht unterstützen, Geräten eingeschränkt).

Einige Nachteile: serverseitige hosten:

* Weist auf höhere Latenz: Jedes Eingreifen des Benutzers umfasst ein Netzwerkhop.
* Bietet keine Unterstützung für offline: Wenn die Clientverbindung fehlschlägt, funktioniert die app nicht mehr.
* Skalierbarkeit ist reduziert werden: Der Server muss mehrere Clientverbindungen verwalten und Clientstatus behandeln.
* Erfordert einen ASP.NET Core-Server, auf die app dient. Bereitstellung ohne Server (z. B. von einem CDN) ist nicht möglich.

&dagger;Die *blazor.server.js* Skript wird in folgendem Pfad veröffentlicht: *"bin" / {Debuggen | Das Release} / {ZIELFRAMEWORK} /publish/ {ANWENDUNGSNAME}. App/Dist/_framework*.
