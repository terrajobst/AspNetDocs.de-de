---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048217"
---

> [!NOTE]
> Viele Schemaänderungsvorgänge werden vom EF Core SQLite-Anbieter nicht unterstützt. Zum Beispiel wird Hinzufügen einer Spalten unterstützt, wogegen Entfernen einer Spalte nicht unterstützt wird. Wenn eine Migration, zum Entfernen einer Spalte erstellt wird, die `ef migrations add` Befehl ist erfolgreich, aber die `ef database update` Befehl schlägt fehl. Einige dieser Einschränkungen können überwunden werden, indem Sie manuell schreiben migrationscode, um eine tabellenneuerstellung auszuführen. Eine tabellenneuerstellung umfasst:

>* Umbenennen der vorhandenen Tabelle.
>* Erstellen einer neuen Tabelle an.
>* Kopieren von Daten aus der alten Tabelle in die neue Tabelle.
>* Löschen die alte Tabelle.

Weitere Informationen finden Sie in den folgenden Ressourcen:
> * [SQLite EF Core-Datenbank-Anbieter-Einschränkungen](/ef/core/providers/sqlite/limitations)
> * [Anpassen des Migrationscodes](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Datenseeding](/ef/core/modeling/data-seeding)