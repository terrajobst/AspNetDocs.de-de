---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Erstellen von benutzerdefiniertenC#Routen () | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie einer ASP.NET MVC-Anwendung benutzerdefinierte Routen hinzufügen. In diesem Tutorial erfahren Sie, wie Sie die Standardrouten Tabelle in der Datei "Global. asax" ändern.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486723"
---
# <a name="creating-custom-routes-c"></a>Erstellen von benutzerdefinierten Routen (C#)

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie einer ASP.NET MVC-Anwendung benutzerdefinierte Routen hinzufügen. In diesem Tutorial erfahren Sie, wie Sie die Standardrouten Tabelle in der Datei "Global. asax" ändern.

In diesem Tutorial erfahren Sie, wie Sie eine benutzerdefinierte Route zu einer ASP.NET MVC-Anwendung hinzufügen. Sie erfahren, wie Sie die Standardrouten Tabelle in der Datei "Global. asax" mit einer benutzerdefinierten Route ändern.

Für viele einfache ASP.NET-MVC-Anwendungen funktioniert die Standardrouten Tabelle problemlos. Allerdings können Sie feststellen, dass Sie über spezielle Routing Anforderungen verfügen. In diesem Fall können Sie eine benutzerdefinierte Route erstellen.

Stellen Sie sich beispielsweise vor, dass Sie eine Blog Anwendung entwickeln. Möglicherweise möchten Sie eingehende Anforderungen verarbeiten, die wie folgt aussehen:

/Archive/12-25-2009

Wenn ein Benutzer diese Anforderung eingibt, möchten Sie den Blogeintrag zurückgeben, der dem Datum 12/25/2009 entspricht. Um diese Art von Anforderung zu verarbeiten, müssen Sie eine benutzerdefinierte Route erstellen.

Die Datei "Global. asax" in der Liste 1 enthält eine neue benutzerdefinierte Route mit dem Namen "Blog", die Anforderungen behandelt, die wie/Archive/*Entry Date*aussehen.

**Codebeispiel 1: Global. asax (mit benutzerdefinierter Route)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

Die Reihenfolge der Routen, die Sie der Routentabelle hinzufügen, ist wichtig. Unsere neue benutzerdefinierte Blog Route wird vor der vorhandenen Standardroute hinzugefügt. Wenn Sie die Reihenfolge umkehren, wird die Standardroute immer anstelle der benutzerdefinierten Route aufgerufen.

Die benutzerdefinierte Blog Route stimmt mit allen Anforderungen überein, die mit/Archive/. beginnen. Dies entspricht allen folgenden URLs:

- /Archive/12-25-2009

- /Archive/10-6-2004

- /Archive/apple

Die benutzerdefinierte Route ordnet die eingehende Anforderung einem Controller namens Archive zu und ruft die Aktion Entry () auf. Wenn die Entry ()-Methode aufgerufen wird, wird das Eintrags Datum als Parameter namens entrydate übergeben.

Sie können die benutzerdefinierte Blog Route mit dem Controller in der Liste 2 verwenden.

**Codebeispiel 2: ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Beachten Sie, dass die Entry ()-Methode in der Liste 2 einen Parameter vom Typ DateTime akzeptiert. Das MVC-Framework ist intelligent genug, um das Eingangsdatum automatisch von der URL in einen DateTime-Wert zu konvertieren. Wenn der Parameter Entry Date von der URL nicht in einen DateTime-Wert konvertiert werden kann, wird ein Fehler ausgelöst (siehe Abbildung 1).

**Abbildung 1: Fehler beim Umrechnen des Parameters**

[![des Dialog Felds "Neues Projekt"](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Abbildung 01**: Fehler beim Umrechnen des Parameters ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-custom-routes-cs/_static/image2.png))

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierte Route erstellen können. Sie haben gelernt, wie Sie der Routing Tabelle in der Datei "Global. asax", die Blogeinträge darstellt, eine benutzerdefinierte Route hinzufügen. Wir haben erläutert, wie Anforderungen für Blogeinträge einem Controller namens archivecontroller und einer Controller Aktion namens Entry () zugeordnet werden.

> [!div class="step-by-step"]
> [Zurück](aspnet-mvc-controllers-overview-cs.md)
> [Weiter](creating-a-route-constraint-cs.md)
