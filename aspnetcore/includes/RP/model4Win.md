---
ms.openlocfilehash: 85fa9f8b18dc7dfb3e9d37703549dc5c28dc7a4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062597"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Aufbauen des Filmmodells

* Führen Sie über die Befehlszeile im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien) folgenden Befehl aus:

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Der zuvor beschriebene Fehler tritt auf, wenn Sie sich im falschen Verzeichnis befinden. Öffnen Sie eine Befehlsshell im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien), und führen Sie den zuvor aufgeführten Befehl aus.

Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Beenden Sie Visual Studio, und führen Sie den Befehl erneut aus.
