---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037397"
---
Der generierte Code für die Identität Datenbank erfordert [Entity Framework Core-Migrationen](/ef/core/managing-schemas/migrations/). Erstellen Sie eine Migration aus, und aktualisieren Sie die Datenbank. Führen Sie z. B. die folgenden Befehle ein:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

In der Visual Studio **-Paket-Manager-Konsole**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

Der "CreateIdentitySchema"-Name-Parameter für die `Add-Migration` Befehl ist willkürlich. `"CreateIdentitySchema"` Beschreibt die Migration.
