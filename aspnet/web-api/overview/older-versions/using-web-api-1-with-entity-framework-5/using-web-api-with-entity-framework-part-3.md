---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Teil 3: Erstellen eines Administrator Controllers | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600042"
---
# <a name="part-3-creating-an-admin-controller"></a>Teil 3: Erstellen eines Administrator Controllers

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Hinzufügen eines Administrator Controllers

In diesem Abschnitt wird ein Web-API-Controller hinzugefügt, der CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) für Produkte unterstützt. Der Controller verwendet Entity Framework, um mit der Datenbankebene zu kommunizieren. Nur Administratoren können diesen Controller verwenden. Kunden greifen über einen anderen Controller auf die Produkte zu.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller. Wählen Sie **Hinzufügen** und dann **Controller**aus.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

Benennen Sie den Controller im Dialogfeld **Controller hinzufügen** `AdminController`. Wählen Sie unter **Vorlage**&quot;API-Controller mit Lese-/Schreibaktionen aus, indem Sie Entity Framework&quot;verwenden. Wählen Sie unter **Modell Klasse**die Option "Product (productstore. Models)" aus. Wählen Sie unter **Datenkontext**die Option "&lt;neuer Datenkontext&gt;" aus.

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Wenn in der Dropdown-Dropdown-Dropdown- **Klasse** keine Modellklassen angezeigt werden, stellen Sie sicher, dass das Projekt kompiliert wurde. Entity Framework die Reflektion verwendet, muss die kompilierte Assembly verwendet werden.

Wenn Sie "&lt;neuen Datenkontext&gt;" auswählen, wird das Dialogfeld " **Neues Datenkontext** " geöffnet. Benennen Sie den Datenkontext `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Klicken Sie auf **OK** , um das Dialogfeld **neuer Datenkontext** zu schließen. Klicken Sie im Dialogfeld **Controller hinzufügen** auf **Hinzufügen**.

Das Projekt wurde dem Projekt hinzugefügt:

- Eine Klasse mit dem Namen `OrdersContext`, die von **dbcontext**abgeleitet ist. Diese Klasse stellt den Klebstoff zwischen den poco-Modellen und der-Datenbank bereit.
- Ein Web-API-Controller mit dem Namen `AdminController`. Dieser Controller unterstützt CRUD-Vorgänge auf `Product` Instanzen. Die `OrdersContext`-Klasse wird für die Kommunikation mit Entity Framework verwendet.
- Eine neue Daten bankverbindungs Zeichenfolge in der Datei "Web. config".

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Öffnen Sie die Datei OrdersContext.cs. Beachten Sie, dass der-Konstruktor den Namen der Daten bankverbindungs Zeichenfolge angibt. Dieser Name verweist auf die Verbindungs Zeichenfolge, die der Datei "Web. config" hinzugefügt wurde.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Fügen Sie der Klasse `OrdersContext` die folgenden Eigenschaften hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Ein **dbset** stellt einen Satz von Entitäten dar, die abgefragt werden können. Im folgenden finden Sie die komplette Auflistung für die `OrdersContext`-Klasse:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

Die `AdminController`-Klasse definiert fünf Methoden, die grundlegende CRUD-Funktionen implementieren. Jede Methode entspricht einem URI, den der Client aufrufen kann:

| Controller-Methode | Beschreibung | URI | HTTP-Methode |
| --- | --- | --- | --- |
| GetProducts | Ruft alle Produkte ab. | API/Produkte | GET |
| GetProduct | Sucht nach einem Produkt anhand der ID. | API/Produkte/*ID* | GET |
| Putproduct | Aktualisiert ein Produkt. | API/Produkte/*ID* | PUT |
| Postproduct | Erstellt ein neues Produkt. | API/Produkte | POST |
| DeleteProduct | Löscht ein Produkt. | API/Produkte/*ID* | LÖSCHEN |

Jede Methode ruft `OrdersContext` auf, um die Datenbank abzufragen. Die Methoden, die den Auflistungs-(Put-, Post-und DELETE-) Befehl ändern, `db.SaveChanges`, um die Änderungen in der Datenbank beizubehalten. Controller werden pro HTTP-Anforderung erstellt und dann verworfen, sodass Änderungen persistent gespeichert werden müssen, bevor eine Methode zurückgegeben wird.

## <a name="add-a-database-initializer"></a>Datenbankinitialisierer hinzufügen

Entity Framework verfügt über ein nettes Feature, mit dem Sie die Datenbank beim Start auffüllen und die Datenbank automatisch neu erstellen können, wenn sich die Modelle ändern. Diese Funktion ist während der Entwicklung nützlich, da Sie immer einige Testdaten haben, auch wenn Sie die Modelle ändern.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle, und erstellen Sie eine neue Klasse mit dem Namen `OrdersContextInitializer`. Fügen Sie die folgende Implementierung ein:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Durch die Vererbung von der **dropkreatedatabaseifmodelchanges** -Klasse wird Entity Framework, die Datenbank zu löschen, wenn die Modellklassen geändert werden. Wenn Entity Framework die Datenbank erstellt (oder neu erstellt), ruft Sie die **Seed** -Methode auf, um die Tabellen aufzufüllen. Wir verwenden die **Seed** -Methode, um einige Beispiel Produkte sowie eine Beispiel Reihenfolge hinzuzufügen.

Diese Funktion eignet sich hervorragend für Tests, aber verwenden Sie nicht die **dropkreatedatabaseifmodelchanges** -Klasse in der Produktion,, weil Sie die Daten verlieren könnten, wenn jemand eine Modell Klasse ändert.

Öffnen Sie als nächstes Global. asax, und fügen Sie den folgenden Code in die **Anwendungs\_Start** -Methode ein:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Senden einer Anforderung an den Controller

An diesem Punkt haben wir keinen Client Code geschrieben, aber Sie können die Web-API mithilfe eines Webbrowsers oder eines HTTP-Debugtools wie z. b. " [fddler](http://www.fiddler2.com/fiddler2/)" aufrufen. Drücken Sie in Visual Studio F5, um das Debuggen zu starten. Der Webbrowser wird geöffnet, um `http://localhost:*portnum*/`, wobei *portnum* eine Portnummer ist.

Senden Sie eine HTTP-Anforderung an "`http://localhost:*portnum*/api/admin`. Die erste Anforderung kann langsam sein, da Entity Framework die Datenbank erstellen und darauf basieren muss. Die Antwort sollte etwa wie folgt aussehen:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-2.md)
> [Weiter](using-web-api-with-entity-framework-part-4.md)
