---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044077"
---
## <a name="add-initial-migration-and-update-the-database"></a><span data-ttu-id="c97df-101">Anfängliche Migration hinzufügen und Aktualisieren der Datenbank</span><span class="sxs-lookup"><span data-stu-id="c97df-101">Add initial migration and update the database</span></span>

* <span data-ttu-id="c97df-102">Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Verzeichnis Projekts.</span><span class="sxs-lookup"><span data-stu-id="c97df-102">Open a command prompt and navigate to the project directory.</span></span> <span data-ttu-id="c97df-103">(Das Verzeichnis mit dem *"Startup.cs"* Datei).</span><span class="sxs-lookup"><span data-stu-id="c97df-103">(The directory containing the *Startup.cs* file).</span></span>

* <span data-ttu-id="c97df-104">Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="c97df-104">Run the following commands in the command prompt:</span></span>

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  <span data-ttu-id="c97df-105">[.NET Core](/dotnet/core/tools/index) ist eine plattformübergreifende Implementierung von .NET.</span><span class="sxs-lookup"><span data-stu-id="c97df-105">[.NET Core](/dotnet/core/tools/index) is a cross-platform implementation of .NET.</span></span> <span data-ttu-id="c97df-106">Hier ist, was mit diesen Befehlen tun:</span><span class="sxs-lookup"><span data-stu-id="c97df-106">Here is what these commands do:</span></span>

  * <span data-ttu-id="c97df-107">[Dotnet-Restore](/dotnet/core/tools/dotnet-restore): Die NuGet-Pakete, die im angegebenen Downloads der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="c97df-107">[dotnet restore](/dotnet/core/tools/dotnet-restore): Downloads the NuGet packages specified in the *.csproj* file.</span></span>
  * <span data-ttu-id="c97df-108">`dotnet ef migrations add Initial` Führt den Entity Framework .NET Core-CLI-Befehl Migrationen und die ursprüngliche Migration erstellt.</span><span class="sxs-lookup"><span data-stu-id="c97df-108">`dotnet ef migrations add Initial` Runs the Entity Framework .NET Core CLI migrations command and creates the initial migration.</span></span> <span data-ttu-id="c97df-109">Der Parameter nach "hinzufügen" ist ein Name, der Sie die Migration zuweisen.</span><span class="sxs-lookup"><span data-stu-id="c97df-109">The parameter after "add" is a name that you assign to the migration.</span></span> <span data-ttu-id="c97df-110">Hier benennen die Migration "Initial" Sie da es sich um die anfängliche Datenbankmigration handelt.</span><span class="sxs-lookup"><span data-stu-id="c97df-110">Here you're naming the migration "Initial" because it's the initial database migration.</span></span> <span data-ttu-id="c97df-111">Dieser Vorgang erstellt die *Datenmigrationen / /\<Datum und Uhrzeit > _Initial.cs* -Datei mit der migrationsbefehle zum Hinzufügen der *Film* Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c97df-111">This operation creates the *Data/Migrations/\<date-time>_Initial.cs* file containing the migration commands to add the *Movie* table to the database.</span></span>
  * <span data-ttu-id="c97df-112">`dotnet ef database update`  Aktualisiert die Datenbank mit der Migration, die wir gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="c97df-112">`dotnet ef database update`  Updates the database with the migration we just created.</span></span>

<span data-ttu-id="c97df-113">Sie erfahren im nächsten Tutorial zu der Datenbank und die Verbindungszeichenfolge-Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="c97df-113">You'll learn about the database and connection string in the next tutorial.</span></span> <span data-ttu-id="c97df-114">Erfahren Sie über Änderungen des Datenmodells in der [Hinzufügen eines Felds](xref:tutorials/first-mvc-app/new-field) Tutorial.</span><span class="sxs-lookup"><span data-stu-id="c97df-114">You'll learn about data model changes in the [Add a field](xref:tutorials/first-mvc-app/new-field) tutorial.</span></span>
