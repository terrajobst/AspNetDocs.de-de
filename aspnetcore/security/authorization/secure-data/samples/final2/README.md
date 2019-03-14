---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049107"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="0113d-101">Wie Sie Build/Datenbeispiel sichere Benutzer ausführen.</span><span class="sxs-lookup"><span data-stu-id="0113d-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="0113d-102">Legen Sie das Kennwort, mit dem Geheimnis-Manager-Tool:</span><span class="sxs-lookup"><span data-stu-id="0113d-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="0113d-103">Die Datenbank zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="0113d-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="0113d-104">Aktivieren von HTTPS im Projekt</span><span class="sxs-lookup"><span data-stu-id="0113d-104">Enable HTTPS in the project</span></span>
