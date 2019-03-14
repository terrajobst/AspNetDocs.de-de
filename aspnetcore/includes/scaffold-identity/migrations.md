---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037397"
---
<span data-ttu-id="764a5-101">Der generierte Code für die Identität Datenbank erfordert [Entity Framework Core-Migrationen](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="764a5-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="764a5-102">Erstellen Sie eine Migration aus, und aktualisieren Sie die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="764a5-102">Create a migration and update the database.</span></span> <span data-ttu-id="764a5-103">Führen Sie z. B. die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="764a5-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="764a5-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="764a5-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="764a5-105">In der Visual Studio **-Paket-Manager-Konsole**:</span><span class="sxs-lookup"><span data-stu-id="764a5-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="764a5-106">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="764a5-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="764a5-107">Der "CreateIdentitySchema"-Name-Parameter für die `Add-Migration` Befehl ist willkürlich.</span><span class="sxs-lookup"><span data-stu-id="764a5-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="764a5-108">`"CreateIdentitySchema"` Beschreibt die Migration.</span><span class="sxs-lookup"><span data-stu-id="764a5-108">`"CreateIdentitySchema"` describes the migration.</span></span>
