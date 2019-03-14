---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044077"
---
## <a name="add-initial-migration-and-update-the-database"></a>Anfängliche Migration hinzufügen und Aktualisieren der Datenbank

* Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Verzeichnis Projekts. (Das Verzeichnis mit dem *"Startup.cs"* Datei).

* Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET Core](/dotnet/core/tools/index) ist eine plattformübergreifende Implementierung von .NET. Hier ist, was mit diesen Befehlen tun:

  * [Dotnet-Restore](/dotnet/core/tools/dotnet-restore): Die NuGet-Pakete, die im angegebenen Downloads der *csproj* Datei.
  * `dotnet ef migrations add Initial` Führt den Entity Framework .NET Core-CLI-Befehl Migrationen und die ursprüngliche Migration erstellt. Der Parameter nach "hinzufügen" ist ein Name, der Sie die Migration zuweisen. Hier benennen die Migration "Initial" Sie da es sich um die anfängliche Datenbankmigration handelt. Dieser Vorgang erstellt die *Datenmigrationen / /\<Datum und Uhrzeit > _Initial.cs* -Datei mit der migrationsbefehle zum Hinzufügen der *Film* Tabelle in der Datenbank.
  * `dotnet ef database update`  Aktualisiert die Datenbank mit der Migration, die wir gerade erstellt haben.

Sie erfahren im nächsten Tutorial zu der Datenbank und die Verbindungszeichenfolge-Zeichenfolge. Erfahren Sie über Änderungen des Datenmodells in der [Hinzufügen eines Felds](xref:tutorials/first-mvc-app/new-field) Tutorial.
