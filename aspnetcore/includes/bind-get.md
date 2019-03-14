---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051957"
---
> [!WARNING]
> <span data-ttu-id="becdd-101">Aus Sicherheitsgründen müssen Sie Daten von `GET`-Anforderungen in die Seitenmodelleigenschaften einbinden.</span><span class="sxs-lookup"><span data-stu-id="becdd-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="becdd-102">Überprüfen Sie die Benutzereingaben, bevor Sie sie den Eigenschaften zuordnen.</span><span class="sxs-lookup"><span data-stu-id="becdd-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="becdd-103">Die Entscheidung für diese Bindung von `GET` ist von Vorteil, wenn Sie auf Szenarios eingehen, die von Abfragezeichenfolgen oder von Werten für Routen abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="becdd-103">Opting in to `GET` binding is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="becdd-104">Legen Sie die `SupportsGet`-Eigenschaft des [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute)-Attributs auf `true`: `[BindProperty(SupportsGet = true)]` fest, um eine Eigenschaft an `GET`-Anforderungen zu binden.</span><span class="sxs-lookup"><span data-stu-id="becdd-104">To bind a property on `GET` requests, set the [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>
