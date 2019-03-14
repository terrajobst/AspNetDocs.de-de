---
ms.openlocfilehash: 025a244812a50709bfd48de9f47a234a547c53e2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047417"
---
<!-- THIS INCLUDE USED BY MVC AND RP --> Fügen Sie der `Movie`-Klasse die folgenden Eigenschaften hinzu:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

Die `Movie`-Klasse enthält:

* Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.
* `[DataType(DataType.Date)]`:  Das [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter)-Attribut gibt den Typ der Daten (Date) an. Mit diesem Attribut:

  * Es ist nicht erforderlich, dass der Benutzer Zeitinformationen in das Datumsfeld eingibt.
  * Nur das Datum wird angezeigt, keine Zeitinformationen.

[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) werden in einem späteren Tutorial behandelt.