---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Gewusst wie: Erstellen eines benutzerdefinierten HTML-Hilfsprogramms für eine MVC-Anwendung | Microsoft-Dokumentation'
author: rick-anderson
description: In diesem Video zeigt Chris Pels, wie ein benutzerdefiniertes htmlhelper erstellt wird, das im Standardsatz in einer MVC-Anwendung nicht verfügbar ist. Zuerst ein Beispiel für eine MVC-Anwendung...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 60953243d3038667e4f729b1394e68f0c9d7c178
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450477"
---
# <a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="58e18-105">Gewusst wie: Erstellen eines benutzerdefinierten HTML-Hilfsprogramms für eine MVC-Anwendung</span><span class="sxs-lookup"><span data-stu-id="58e18-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>

<span data-ttu-id="58e18-106">von [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="58e18-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="58e18-107">In diesem Video zeigt Chris Pels, wie ein benutzerdefiniertes htmlhelper erstellt wird, das im Standardsatz in einer MVC-Anwendung nicht verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="58e18-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="58e18-108">Zuerst wird eine MVC-Beispielanwendung mit einem Demo Controller und einer Ansicht erstellt, um das benutzerdefinierte htmlhelper zu testen.</span><span class="sxs-lookup"><span data-stu-id="58e18-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="58e18-109">Als nächstes wird ein Modul mit einer öffentlichen Funktion erstellt, bei der es sich um eine Erweiterungsmethode handelt, die die Implementierung des benutzerdefinierten htmlhelper darstellt.</span><span class="sxs-lookup"><span data-stu-id="58e18-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="58e18-110">Das benutzerdefinierte Hilfsprogramm dient zum Erstellen von `<img>`-Tags in einer Seite und empfängt mehrere eingehende Parameter, einschließlich der ID, URL und des Alternativ Texts für das imagetag.</span><span class="sxs-lookup"><span data-stu-id="58e18-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="58e18-111">Die Logik wird dann der-Funktion zum Zurückgeben des abgeschlossenen `<img>`-Tags mit den angegebenen Informationen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="58e18-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="58e18-112">Anschließend wird das benutzerdefinierte htmlhelper auf der Seite Demo zum Anzeigen eines Bilds verwendet.</span><span class="sxs-lookup"><span data-stu-id="58e18-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="58e18-113">Schließlich wird das benutzerdefinierte htmlhelper erweitert und umfasst mehrere konstruktorüberschreibungen, die die Flexibilität erleichtern, unterschiedliche `<img>` Tags zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="58e18-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="58e18-114">&#9654;Video ansehen (18 Minuten)</span><span class="sxs-lookup"><span data-stu-id="58e18-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="58e18-115">[Zurück](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Weiter](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="58e18-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
