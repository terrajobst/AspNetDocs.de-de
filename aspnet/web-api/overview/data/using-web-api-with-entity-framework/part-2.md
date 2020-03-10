---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Hinzufügen von Modellen und Controllern | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449187"
---
# <a name="add-models-and-controllers"></a>Hinzufügen von Modellen und Controllern

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie Modellklassen hinzu, die die Daten Bank Entitäten definieren. Anschließend werden Sie Web-API-Controller hinzufügen, die CRUD-Vorgänge für diese Entitäten ausführen.

## <a name="add-model-classes"></a>Hinzufügen von Modellklassen

In diesem Tutorial erstellen wir die Datenbank mithilfe des "Code First"-Ansatzes für Entity Framework (EF). Mit Code First schreiben C# Sie Klassen, die Datenbanktabellen entsprechen, und EF erstellt die Datenbank. (Weitere Informationen finden Sie unter [Entity Framework-Entwicklungsansätzen](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Zunächst definieren wir unsere Domänen Objekte als POCOS (Plain-Old CLR Objects). Wir erstellen die folgenden POCOS:

- Autor
- Buch

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle. Wählen Sie **Hinzufügen**und dann **Klasse**aus. Geben Sie der Klassen den Namen `Author`.

![](part-2/_static/image1.png)

Ersetzen Sie den gesamten Code in Author.cs durch den folgenden Code.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Fügen Sie eine weitere Klasse mit dem Namen `Book`mit dem folgenden Code hinzu.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework werden diese Modelle zum Erstellen von Datenbanktabellen verwenden. Für jedes Modell wird die `Id`-Eigenschaft zur Primärschlüssel Spalte der Datenbanktabelle.

In der Book-Klasse definiert der `AuthorId` einen Fremdschlüssel in der `Author` Tabelle. (Aus Gründen der Einfachheit gehe ich davon aus, dass jedes Buch über einen einzigen Autor verfügt.) Die Book-Klasse enthält auch eine Navigations Eigenschaft für die zugehörigen `Author`. Mit der-Navigations Eigenschaft können Sie auf die zugehörigen `Author` im Code zugreifen. Weitere Informationen zu Navigations Eigenschaften finden Sie in Teil 4, [Behandeln von Entitäts Beziehungen](part-4.md).

## <a name="add-web-api-controllers"></a>Web-API-Controller hinzufügen

In diesem Abschnitt fügen wir Web-API-Controller hinzu, die CRUD-Vorgänge unterstützen (erstellen, lesen, aktualisieren und löschen). Die Controller verwenden Entity Framework, um mit der Datenbankebene zu kommunizieren.

Zuerst können Sie die Datei Controller/valuescontroller. cs löschen. Diese Datei enthält einen Beispiel-Web-API-Controller, den Sie für dieses Tutorial jedoch nicht benötigen.

![](part-2/_static/image2.png)

Erstellen Sie als nächstes das Projekt. Das Web-API-Gerüst verwendet Reflektion, um nach den Modellklassen zu suchen. Daher muss die kompilierte Assembly verwendet werden.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller. Wählen Sie **Hinzufügen**und dann **Controller**aus.

![](part-2/_static/image3.png)

Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option "Web-API 2-Controller mit Aktionen unter Verwendung Entity Framework" aus. Klicken Sie auf **Hinzufügen**.

![](part-2/_static/image4.png)

Gehen Sie im Dialogfeld **Controller hinzufügen** wie folgt vor:

1. Wählen Sie in der Dropdown Liste **Modell Klasse** die `Author` Klasse aus. (Wenn Sie in der Dropdown Liste nicht angezeigt wird, stellen Sie sicher, dass Sie das Projekt erstellt haben.)
2. Aktivieren Sie die Option "Async-Controller Aktionen verwenden".
3. Belassen Sie den Controller Namen als &quot;authorscontroller&quot;.
4. Klicken Sie neben **Datenkontext Klasse**auf die Schaltfläche Plus (+).

![](part-2/_static/image5.png)

Überlassen Sie im Dialogfeld **neuer Datenkontext** den Standardnamen, und klicken Sie auf **Hinzufügen**.

![](part-2/_static/image6.png)

Klicken Sie zum Vervollständigen des Dialog Felds **Controller hinzu** fügen auf **Hinzufügen** . Im Dialogfeld werden zwei Klassen zu Ihrem Projekt hinzugefügt:

- `AuthorsController` definiert einen Web-API-Controller. Der Controller implementiert die Rest-API, die von Clients zum Durchführen von CRUD-Vorgängen für die Liste der Autoren verwendet wird.
- `BookServiceContext` verwaltet Entitäts Objekte zur Laufzeit. dazu gehören das Auffüllen von Objekten mit Daten aus einer Datenbank, die Änderungs Nachverfolgung und das Beibehalten von Daten in der Datenbank. Er erbt von `DbContext`.

![](part-2/_static/image7.png)

Erstellen Sie an diesem Punkt das Projekt erneut. Durchlaufen Sie nun die gleichen Schritte, um einen API-Controller für `Book` Entitäten hinzuzufügen. Wählen Sie diesmal `Book` für die Modell Klasse aus, und wählen Sie die vorhandene `BookServiceContext` Klasse für die Datenkontext Klasse aus. (Erstellen Sie keinen neuen Datenkontext.) Klicken Sie auf **Hinzufügen** , um den Controller hinzuzufügen.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Zurück](part-1.md)
> [Weiter](part-3.md)
