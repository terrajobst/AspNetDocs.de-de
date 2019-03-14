---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188717"
---
> Einige Befehle werden nicht unterstützt, wenn die app SQLite als Identitätsspeicher für Daten verwendet. Aufgrund von Einschränkungen in die Datenbank-Engine `Alter` Befehle der folgenden Ausnahme:
>
> "System.NotSupportedException: SQLite unterstützt diesen Migrationsvorgang nicht." 
>
> Können dies umgehen führen Sie Code First-Migrationen in der Datenbank so ändern Sie die Tabellen aus.
