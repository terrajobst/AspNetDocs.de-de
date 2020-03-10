---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Gewusst wie:] Verwenden Sie die Eigenschaft "reponse. Filter", um HTML in einer ASP.NET-Seite zu ersetzen | Microsoft-Dokumentation'
author: rick-anderson
description: In diesem Video zeigt Chris Pels, wie Sie die Eigenschaft "reponse. Filter" verwenden, um den HTML-Code abzufangen und zu ändern, der an eine Seite gesendet wird. Zuerst wird eine Beispielseite erstellt...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78487995"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="fbb8b-104">[Gewusst wie:] Verwenden Sie die Eigenschaft "reponse. Filter", um HTML auf einer ASP.NET-Seite zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="fbb8b-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>

<span data-ttu-id="fbb8b-105">von [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="fbb8b-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="fbb8b-106">In diesem Video zeigt Chris Pels, wie Sie die Eigenschaft "reponse. Filter" verwenden, um den HTML-Code abzufangen und zu ändern, der an eine Seite gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="fbb8b-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="fbb8b-107">Zuerst wird eine Beispielseite mit einem einfachen Text erstellt.</span><span class="sxs-lookup"><span data-stu-id="fbb8b-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="fbb8b-108">Anschließend wird eine benutzerdefinierte Streamklasse erstellt, die als Ersatz Datenstrom für den aktuellen Stream dient, der an den Browser des Benutzers gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="fbb8b-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="fbb8b-109">In dieser benutzerdefinierten Streamklasse wird der Inhalt der Seite aus dem Stream abgerufen, geändert und dann in den Antwortstream geschrieben.</span><span class="sxs-lookup"><span data-stu-id="fbb8b-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="fbb8b-110">In dieser benutzerdefinierten Streamklasse wird die Write-Methode angepasst, um den HTML-Code im basisantwortstream zu ersetzen, wodurch geändert wird, was an den Browser des Benutzers gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="fbb8b-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="fbb8b-111">Schließlich wird die neue Stream-Klasse der Response. Filter-Eigenschaft in der Seite\_Load-Ereignis zugewiesen, wodurch der Mechanismus zum Ändern des Seiteninhalts bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="fbb8b-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="fbb8b-112">&#9654;Video ansehen (13 Minuten)</span><span class="sxs-lookup"><span data-stu-id="fbb8b-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
