---
title: Debuggen von Razor-Komponenten
author: guardrex
description: Informationen Sie zum Debuggen von apps für Blazor und Razor-Komponenten.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034117"
---
# <a name="debug-razor-components"></a>Debuggen von Razor-Komponenten

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Razor-Komponenten verfügt über einige *sehr frühen* Unterstützung für das Debuggen von clientseitigen Blazor apps auf WebAssembly in Chrome ausgeführt wird. Während dieses ersten Debugunterstützung sehr begrenzter und unpoliertem ist, wird der grundlegende Debuginfrastruktur zusammenkommen.

So debuggen Sie eine clientseitige Blazor-app in Chrome:

* Erstellen Sie eine app Blazor in `Debug` Configuration (Standard für nicht veröffentlichte apps).
* Führen Sie die Blazor-app in Chrome (Version 70 oder höher).
* Wählen Sie mit dem Tastaturfokus auf der app (nicht in die Dev-Werkzeuge, die Sie möglicherweise eine weniger verwirrend Debugleistung erzielen Sie geschlossen werden soll) die folgende Blazor-spezifische Tastenkombination auswählen:
  * `Shift+Alt+D` unter Windows/Linux
  * `Shift+Cmd+D` unter macOS

Führen Sie Chrome mit aktiviertem Debuggen die app Blazor remote Debuggen. Wenn das Remotedebuggen deaktiviert ist, wird eine Fehlerseite von Chrome generiert. Die Fehlerseite enthält Anweisungen zum Ausführen von Chrome mit den Port zum Debuggen öffnen, damit der Blazor-debugging-Proxy für die app eine Verbindung herstellen kann. *Schließen Sie alle Instanzen von Chrome* und starten Sie Chrome erneut, wie beschrieben.

![Blazor Debuggen Fehler (Seite)](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

Wenn Chrome ausgeführt wird, mit aktiviertem Remotedebuggen, öffnet die Debuggen Tastenkombination eine neue Registerkarte "Debugger". Nach einer Weile die *Quellen* Registerkarte wird eine Liste der .NET-Assemblys in der app. Erweitern Sie jede Assembly, und suchen die *cs*/*.cshtml* Quelldateien für das Debuggen verfügbar. Legen Sie Haltepunkte, wechseln Sie zurück zur Registerkarte "der app", und die Haltepunkte werden erreicht. Nach einem Haltepunkt wird erreicht, Schritt für Schritt (`F10`) oder fortsetzen (`F8`) normalerweise.

![Blazor Debuggen](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

Blazor bietet eine debuggingproxy, implementiert die [Chrome DevTools-Protokoll](https://chromedevtools.github.io/devtools-protocol/) und das Protokoll mit erweitert. NET-spezifische Informationen. Beim Debuggen Tastenkombination gedrückt wird, verweist Blazor der Chrome DevTools auf dem Proxy. Der Proxy eine Verbindung herstellt, zu dem Browserfenster, Sie zum Debuggen suchen (daher Remotedebuggen aktiviert werden muss).

Sie Fragen sich vielleicht warum wir nur quellzuordnungen Browser nicht verwenden. Quellzuordnungen können den Browser, um kompilierte Dateien wieder den ursprünglichen Quelldateien zuordnen. Blazor ordnet jedoch nicht C# direkt mit JS/WASM (zumindest noch nicht). Stattdessen wird die Blazor IL-Interpretation innerhalb des Browsers, sodass quellzuordnungen nicht relevant sind.

Beachten Sie, dass die Debugger-Funktionen **sehr begrenzten**. Sie können derzeit nur:

* Haltepunkte setzen und entfernen.
* Schritt für Schritt durch den Code oder fortsetzen (`F8`).
* In der *"lokal"* anzeigen, sehen Sie sich die Werte von lokalen Variablen des Typs `int`, `string`, und `bool`.
* Finden Sie in der Aufrufliste, einschließlich Aufrufketten, die aus JavaScript in .NET und .NET zu JavaScript gesendet.

Sie *kann nicht*:

* Beobachten Sie die Werte, der alle lokalen Variablen, die keine `int`, `string`, oder `bool`.
* Beachten Sie die Werte der Klasseneigenschaften oder Feldern.
* Bewegen Sie den Mauszeiger über Variablen, um deren Werte finden Sie unter
* Auswerten von Ausdrücken in der Konsole.
* Schritt zwischen asynchronen Aufrufen.
* Führen Sie die meisten anderen gewöhnlichen Debugszenarien.

Entwicklung von weiteren Szenarien zu Debuggen ist ein Schwerpunkt hin zur fortlaufenden dem engineering-Team.

## <a name="troubleshooting-tip"></a>Tipp zur Problembehandlung

Wenn Sie Fehler ausführen, kann die folgenden Tipps helfen:

In der **Debugger** Registerkarte, öffnen Sie die Developer Tools in Ihrem Browser. Führen Sie in der Konsole `localStorage.clear()` alle Haltepunkte entfernen.
