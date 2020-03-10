---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Neues Element zur Datenbank hinzufügen | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448971"
---
# <a name="add-a-new-item-to-the-database"></a><span data-ttu-id="1a287-102">Hinzufügen eines neuen Elements zur Datenbank</span><span class="sxs-lookup"><span data-stu-id="1a287-102">Add a New Item to the Database</span></span>

<span data-ttu-id="1a287-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1a287-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1a287-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="1a287-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="1a287-105">In diesem Abschnitt fügen Sie Benutzern die Möglichkeit hinzu, ein neues Buch zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1a287-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="1a287-106">Fügen Sie in "App. js" dem Ansichts Modell folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="1a287-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="1a287-107">Ersetzen Sie in der Datei "index. cshtml" das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="1a287-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="1a287-108">Durch:</span><span class="sxs-lookup"><span data-stu-id="1a287-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="1a287-109">Dieses Markup erstellt ein Formular zum Übermitteln eines neuen Autors.</span><span class="sxs-lookup"><span data-stu-id="1a287-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="1a287-110">Die Werte für die Dropdown Liste Autor sind an die `authors` Observable im Ansichts Modell gebunden.</span><span class="sxs-lookup"><span data-stu-id="1a287-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="1a287-111">Für andere Formular Eingaben sind die Werte an die `newBook`-Eigenschaft des Ansichts Modells gebunden.</span><span class="sxs-lookup"><span data-stu-id="1a287-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="1a287-112">Der Sende Handler im Formular ist an die `addBook`-Funktion gebunden:</span><span class="sxs-lookup"><span data-stu-id="1a287-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="1a287-113">Die `addBook`-Funktion liest die aktuellen Werte der Daten gebundenen Formular Eingaben, um ein JSON-Objekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1a287-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="1a287-114">Anschließend wird das JSON-Objekt an `/api/books`übermittelt.</span><span class="sxs-lookup"><span data-stu-id="1a287-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1a287-115">[Zurück](part-8.md)
> [Weiter](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="1a287-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
