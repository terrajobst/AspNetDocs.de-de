---
ms.openlocfilehash: 5ed24cd8a7e880a496d0c0295f7c1fb07f0e1032
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57035987"
---
Die folgende Tabelle zeigt die Details der Parameter des Codegenerators von ASP.NET:

| Parameter               | Beschreibung|
| ----------------- | ------------ |
| -m  | Der Name des Modells. |
| -dc  | Die `DbContext`-Klasse, die verwendet werden soll. |
| -udl | Verwendung des Standardlayouts. |
| -outDir | Der relative Ausgabeordnerpfad zur Erstellung der Ansichten. |
| --referenceScriptLibraries | Fügt `_ValidationScriptsPartial` zu den Seiten „Create“ und „Edit“ hinzu. |

Verwenden Sie den `h`-Switch um Hilfe für den `aspnet-codegenerator razorpage`-Befehl zu erhalten:

```console
dotnet aspnet-codegenerator razorpage -h
```
