---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051957"
---
> [!WARNING]
> Aus Sicherheitsgründen müssen Sie Daten von `GET`-Anforderungen in die Seitenmodelleigenschaften einbinden. Überprüfen Sie die Benutzereingaben, bevor Sie sie den Eigenschaften zuordnen. Die Entscheidung für diese Bindung von `GET` ist von Vorteil, wenn Sie auf Szenarios eingehen, die von Abfragezeichenfolgen oder von Werten für Routen abhängig sind.
>
> Legen Sie die `SupportsGet`-Eigenschaft des [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute)-Attributs auf `true`: `[BindProperty(SupportsGet = true)]` fest, um eine Eigenschaft an `GET`-Anforderungen zu binden.
