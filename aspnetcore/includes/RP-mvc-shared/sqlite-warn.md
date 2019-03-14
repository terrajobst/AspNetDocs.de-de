---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048217"
---

> [!NOTE]
> <span data-ttu-id="8e19a-101">Viele Schemaänderungsvorgänge werden vom EF Core SQLite-Anbieter nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8e19a-101">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="8e19a-102">Zum Beispiel wird Hinzufügen einer Spalten unterstützt, wogegen Entfernen einer Spalte nicht unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="8e19a-102">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="8e19a-103">Wenn eine Migration, zum Entfernen einer Spalte erstellt wird, die `ef migrations add` Befehl ist erfolgreich, aber die `ef database update` Befehl schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="8e19a-103">If a migration is created to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="8e19a-104">Einige dieser Einschränkungen können überwunden werden, indem Sie manuell schreiben migrationscode, um eine tabellenneuerstellung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="8e19a-104">Some of these limitations can be overcome by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="8e19a-105">Eine tabellenneuerstellung umfasst:</span><span class="sxs-lookup"><span data-stu-id="8e19a-105">A table rebuild involves:</span></span>

>* <span data-ttu-id="8e19a-106">Umbenennen der vorhandenen Tabelle.</span><span class="sxs-lookup"><span data-stu-id="8e19a-106">Renaming the existing table.</span></span>
>* <span data-ttu-id="8e19a-107">Erstellen einer neuen Tabelle an.</span><span class="sxs-lookup"><span data-stu-id="8e19a-107">Creating a new table.</span></span>
>* <span data-ttu-id="8e19a-108">Kopieren von Daten aus der alten Tabelle in die neue Tabelle.</span><span class="sxs-lookup"><span data-stu-id="8e19a-108">Copying data from the old table to the new table.</span></span>
>* <span data-ttu-id="8e19a-109">Löschen die alte Tabelle.</span><span class="sxs-lookup"><span data-stu-id="8e19a-109">Dropping the old table.</span></span>

<span data-ttu-id="8e19a-110">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="8e19a-110">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="8e19a-111">SQLite EF Core-Datenbank-Anbieter-Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="8e19a-111">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="8e19a-112">Anpassen des Migrationscodes</span><span class="sxs-lookup"><span data-stu-id="8e19a-112">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="8e19a-113">Datenseeding</span><span class="sxs-lookup"><span data-stu-id="8e19a-113">Data seeding</span></span>](/ef/core/modeling/data-seeding)