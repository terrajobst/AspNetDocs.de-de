---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188717"
---
> <span data-ttu-id="5700c-101">Einige Befehle werden nicht unterstützt, wenn die app SQLite als Identitätsspeicher für Daten verwendet.</span><span class="sxs-lookup"><span data-stu-id="5700c-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="5700c-102">Aufgrund von Einschränkungen in die Datenbank-Engine `Alter` Befehle der folgenden Ausnahme:</span><span class="sxs-lookup"><span data-stu-id="5700c-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="5700c-103">"System.NotSupportedException: SQLite unterstützt diesen Migrationsvorgang nicht."</span><span class="sxs-lookup"><span data-stu-id="5700c-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="5700c-104">Können dies umgehen führen Sie Code First-Migrationen in der Datenbank so ändern Sie die Tabellen aus.</span><span class="sxs-lookup"><span data-stu-id="5700c-104">As a work around, run Code First migrations on the database to change the tables.</span></span>
