---
ms.openlocfilehash: eb2f83504129ae2b19ff5d85a2bc7d90293b43d6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043397"
---
* "Startup.cs": ["Startup"-Klasse](xref:fundamentals/startup) -Klasse, konfiguriert die Anforderungspipeline, der alle Anforderungen an die Anwendung behandelt.
* "Program.cs": [Programmieren von Klasse](xref:fundamentals/index) , enthält der Haupteinstiegspunkt der Anwendung.
* firstapp.csproj: [Projektdatei](/dotnet/articles/core/preview3/tools/csproj) MSBuild-Projektdateiformat für ASP.NET Core-Anwendungen. Projekt-zu-Projekt Verweise enthält, NuGet-Verweise und andere Projekt verknüpfte Elemente.
* appsettings.json / appsettings.Development.json : Konfigurationsdatei für Basis-app-Umgebung. [Finden Sie unter Konfiguration](xref:fundamentals/configuration/index).
* bower.json : Bower-Paket-Abhängigkeiten für das Projekt.
* ".bowerrc": Bower-Konfigurationsdatei die definiert, wo Sie die Komponenten zu installieren, wenn Bower die Objekte heruntergeladen.
* bundleconfig.JSON: Konfigurationsdateien zu bündeln und Minimieren der Front-End-JavaScript- und CSS-Objekte.
* Ansichten: Enthält die Razor-Ansichten. Ansichten sind die Komponenten, die die Benutzeroberfläche der App anzeigen. Im Allgemeinen werden die Modelldaten auf dieser Benutzeroberfläche angezeigt.
* Domänencontroller: Enthält MVC-Controller, anfänglich *"HomeController.cs"*. Controller sind Klassen, die Browseranforderungen verarbeiten.
* "Wwwroot": Stammordner der Web-Anwendung.

Weitere Informationen finden Sie unter [das MVC-Muster](xref:mvc/overview).
