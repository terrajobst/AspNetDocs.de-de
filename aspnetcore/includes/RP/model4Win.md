---
ms.openlocfilehash: 85fa9f8b18dc7dfb3e9d37703549dc5c28dc7a4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062597"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="26278-101">Aufbauen des Filmmodells</span><span class="sxs-lookup"><span data-stu-id="26278-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="26278-102">Führen Sie über die Befehlszeile im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien) folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="26278-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="26278-103">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="26278-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="26278-104">Der zuvor beschriebene Fehler tritt auf, wenn Sie sich im falschen Verzeichnis befinden.</span><span class="sxs-lookup"><span data-stu-id="26278-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="26278-105">Öffnen Sie eine Befehlsshell im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien), und führen Sie den zuvor aufgeführten Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="26278-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="26278-106">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="26278-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="26278-107">Beenden Sie Visual Studio, und führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="26278-107">Exit Visual Studio and run the command again.</span></span>
