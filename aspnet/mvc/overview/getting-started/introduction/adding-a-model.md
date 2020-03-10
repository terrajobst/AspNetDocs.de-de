---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Hinzufügen eines Modells | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: c5525cfe940cadff5113c63eb0508d15697b5606
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499083"
---
# <a name="adding-a-model"></a>Hinzufügen eines Modells

von [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

In diesem Abschnitt fügen Sie einige Klassen zum Verwalten von Filmen in einer-Datenbank hinzu. Diese Klassen sind das &quot;Modell&quot; Teil der ASP.NET MVC-app.

Sie verwenden eine .NET Framework Datenzugriffs Technologie, die als [Entity Framework](https://docs.microsoft.com/ef/) bezeichnet wird, um diese Modellklassen zu definieren und mit Ihnen zu arbeiten. Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklungsparadigma namens *Code First*. Code First ermöglicht es Ihnen, Modell Objekte zu erstellen, indem einfache Klassen geschrieben werden. (Diese werden auch als poco-Klassen bezeichnet, von &quot;Plain-Old CLR-Objekten.&quot;) Anschließend können Sie die Datenbank dynamisch von ihren Klassen erstellen lassen, wodurch ein sehr sauberer und schneller Entwicklungs Workflow ermöglicht wird. Wenn Sie die Datenbank zuerst erstellen müssen, finden Sie in diesem Tutorial Weitere Informationen zur MVC-und EF-App-Entwicklung. Anschließend können Sie das Tutorial "Tom fzmakens [ASP.net Gerüstbau](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) " befolgen, das den Ansatz der Datenbank behandelt.

## <a name="adding-model-classes"></a>Hinzufügen von Modellklassen

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie **Hinzufügen**und dann **Klasse**aus.

![](adding-a-model/_static/image1.png)

Geben Sie den *Klassen* Namen &quot;Movie&quot;ein.

Fügen Sie der `Movie`-Klasse die folgenden fünf Eigenschaften hinzu:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Wir verwenden die `Movie`-Klasse, um Filme in einer Datenbank darzustellen. Jede Instanz eines `Movie` Objekts entspricht einer Zeile in einer Datenbanktabelle, und jede Eigenschaft der `Movie` Klasse wird einer Spalte in der Tabelle zugeordnet.

Hinweis: um "System. Data. Entity" und die zugehörige Klasse zu verwenden, müssen Sie das [Entity Framework nuget-Paket](https://www.nuget.org/packages/EntityFramework/)installieren. Weitere Anweisungen finden Sie unter dem Link.

Fügen Sie in der gleichen Datei die folgende `MovieDBContext`-Klasse hinzu:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

Die `MovieDBContext`-Klasse stellt den Entity Framework Movie-Daten Bank Kontext dar, der das Abrufen, speichern und Aktualisieren von `Movie`-Klassen Instanzen in einer Datenbank übernimmt. Die `MovieDBContext` wird von der `DbContext` Basisklasse abgeleitet, die vom Entity Framework bereitgestellt wird.

Um auf `DbContext` und `DbSet`verweisen zu können, müssen Sie am Anfang der Datei die folgende `using`-Anweisung hinzufügen:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Fügen Sie hierzu manuell die using-Anweisung hinzu, oder bewegen Sie den Mauszeiger über die roten Wellenlinien, klicken Sie auf `Show potential fixes` und klicken Sie dann auf `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Hinweis: mehrere nicht verwendete `using` Anweisungen wurden entfernt. In Visual Studio werden nicht verwendete Abhängigkeiten als grau angezeigt. Sie können nicht verwendete Abhängigkeiten entfernen, indem Sie auf die grauen Abhängigkeiten zeigen, auf `Show potential fixes` klicken und auf nicht verwendete using-Direktiven **Entfernen klicken.**

![](adding-a-model/_static/image3.png)

Schließlich haben wir ein Modell hinzugefügt (M in MVC). Im nächsten Abschnitt Arbeiten Sie mit der Datenbank-Verbindungs Zeichenfolge.

> [!div class="step-by-step"]
> [Zurück](adding-a-view.md)
> [Weiter](creating-a-connection-string.md)
