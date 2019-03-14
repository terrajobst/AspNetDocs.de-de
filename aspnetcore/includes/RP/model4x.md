---
ms.openlocfilehash: d875717d480fe234ac5a328493fcec6a268e37a8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039517"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="ff1a1-101">Aufbauen des Filmmodells</span><span class="sxs-lookup"><span data-stu-id="ff1a1-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="ff1a1-102">Führen Sie über die Befehlszeile im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien) folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="ff1a1-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="ff1a1-103">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="ff1a1-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="ff1a1-104">Öffnen Sie eine Befehlsshell im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien).</span><span class="sxs-lookup"><span data-stu-id="ff1a1-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
