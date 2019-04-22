---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Gewusst wie:] Verwenden der bedingten UpdateMode von UpdatePanel? | Microsoft-Dokumentation'
author: JoeStagner
description: Das UpdatePanel von ASP.NET AJAX umfasst eine UpdateMode-Eigenschaft, die auf "Always" oder "Bedingt" festgelegt werden kann. Der Standardwert ist immer in diesem Fall wird die UpdatePan...
ms.author: riande
ms.date: 08/01/2007
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: c05d4f262d56dfba858443b830d72ff0520b65d7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59381418"
---
# <a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="ebc3e-105">[Gewusst wie:] Verwenden der bedingten UpdateMode von UpdatePanel?</span><span class="sxs-lookup"><span data-stu-id="ebc3e-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>

<span data-ttu-id="ebc3e-106">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="ebc3e-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="ebc3e-107">Das UpdatePanel von ASP.NET AJAX umfasst eine UpdateMode-Eigenschaft, die auf "Always" oder "Bedingt" festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="ebc3e-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="ebc3e-108">Der Standardwert ist immer in diesem Fall das UpdatePanel stets den Inhalt während eines asynchronen Postbacks aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="ebc3e-108">The default is Always, in which case the UpdatePanel will always update its content during an asynchronous postback.</span></span> <span data-ttu-id="ebc3e-109">In diesem Video erfahren wir, wie wir die UpdateMode auf bedingte, festlegen können in der Fall UpdatePanel nur den Inhalt aktualisiert wird, sobald unsere serverseitigen Code die Update-Methode aufruft.</span><span class="sxs-lookup"><span data-stu-id="ebc3e-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="ebc3e-110">Dadurch können Sie bedingten Logik in Ihrem C#- oder Visual Basic-Code verwenden, um festzustellen, ob das UpdatePanel seinen Inhalt während des aktuellen asynchronen Postbacks aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="ebc3e-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="ebc3e-111">&#9654;Sehen Sie sich Video (13 Minuten)</span><span class="sxs-lookup"><span data-stu-id="ebc3e-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="ebc3e-112">[Zurück](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [Weiter](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="ebc3e-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>
