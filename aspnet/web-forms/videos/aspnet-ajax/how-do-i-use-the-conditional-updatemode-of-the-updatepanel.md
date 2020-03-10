---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Gewusst wie:] Verwenden Sie das bedingte Update Mode von UpdatePanel? | Microsoft-Dokumentation'
author: JoeStagner
description: Das ASP.NET AJAX-Update Panel enthält eine UpdateMode-Eigenschaft, die auf "Always" oder "Conditional" festgelegt werden kann. Der Standardwert lautet immer. in diesem Fall wird updatepan...
ms.author: riande
ms.date: 08/01/2007
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: c05d4f262d56dfba858443b830d72ff0520b65d7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510123"
---
# <a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="79c8e-105">[Gewusst wie:] Verwenden Sie das bedingte Update Mode von UpdatePanel?</span><span class="sxs-lookup"><span data-stu-id="79c8e-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>

<span data-ttu-id="79c8e-106">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="79c8e-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="79c8e-107">Das ASP.NET AJAX-Update Panel enthält eine UpdateMode-Eigenschaft, die auf "Always" oder "Conditional" festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="79c8e-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="79c8e-108">Der Standardwert ist immer. in diesem Fall aktualisiert Update Panel seinen Inhalt immer während eines asynchronen Postbacks.</span><span class="sxs-lookup"><span data-stu-id="79c8e-108">The default is Always, in which case the UpdatePanel will always update its content during an asynchronous postback.</span></span> <span data-ttu-id="79c8e-109">In diesem Video erfahren Sie, wie Sie UpdateMode auf Conditional festlegen. in diesem Fall aktualisiert Update Panel nur seinen Inhalt, wenn der serverseitige Code seine Update-Methode aufruft.</span><span class="sxs-lookup"><span data-stu-id="79c8e-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="79c8e-110">Auf diese Weise können Sie bedingte Logik in Ihrem C# -oder-Visual Basic Code verwenden, um zu bestimmen, ob Update Panel den Inhalt während des aktuellen asynchronen Postbacks aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="79c8e-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="79c8e-111">&#9654;Video ansehen (13 Minuten)</span><span class="sxs-lookup"><span data-stu-id="79c8e-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="79c8e-112">[Zurück](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [Weiter](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="79c8e-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>
