---
ms.openlocfilehash: 568fa161b27e554fd8b474670b8f9a7ec2f39f45
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047127"
---
Die folgende Tabelle zeigt die Details der Parameter des Codegenerators von ASP.NET:

| Parameter               | Beschreibung|
| ----------------- | ------------ |
| -m  | Der Name des Modells. |
| -dc  | Der Datenkontext. |
| -udl | Verwendung des Standardlayouts. |
| --relativeFolderPath | Der relative Ausgabeordnerpfad zur Erstellung der Ansichten. |
| --useDefaultLayout | Das Standardlayout sollte für die Ansichten verwendet werden. |
| --referenceScriptLibraries | Fügt `_ValidationScriptsPartial` zu den Seiten „Create“ und „Edit“ hinzu. |

Verwenden Sie den `h`-Switch um Hilfe für den `aspnet-codegenerator controller`-Befehl zu erhalten:

```console
dotnet aspnet-codegenerator controller -h
```