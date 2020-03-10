---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: In diesem Video zeigt Chris Pels, wie der Zustand von einem oder mehreren Objekten in einem Benutzer Steuerelement beibehalten wird. Zuerst wird ein Benutzer Steuerelement erstellt, das die Fähigkeiten darstellt...
ms.author: riande
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: c87bd6c5c993a1bde8f8a84f6d53b431e54541d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516273"
---
# <a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Gewusst wie]: beibehalten des Zustands eines Benutzer Steuer Elements während eines Postbacks
[How Do I]: Persist the State of a User Control During a Postback

<span data-ttu-id="88f3a-104">von [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="88f3a-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="88f3a-105">In diesem Video zeigt Chris Pels, wie der Zustand von einem oder mehreren Objekten in einem Benutzer Steuerelement beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="88f3a-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="88f3a-106">Zuerst wird ein Benutzer Steuerelement erstellt, das die Fähigkeit eines Benutzers darstellt, Filterkriterien für eine Suche anzugeben.</span><span class="sxs-lookup"><span data-stu-id="88f3a-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="88f3a-107">Außerdem wird eine Begleit Filterklasse erstellt, um die Filter Informationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="88f3a-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="88f3a-108">Dem Filter Steuerelement werden mehrere Benutzeroberflächen Elemente sowie einige Methoden und Eigenschaften hinzugefügt, um die aktuellen Filter Informationen in der Filterklassen Instanz zu speichern.</span><span class="sxs-lookup"><span data-stu-id="88f3a-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="88f3a-109">Im nächsten Schritt wird die Persistenz des Benutzer Steuer Elements mithilfe der registerrequirements scontrolstate-Methode und der zugehörigen Save/restore-Methoden implementiert.</span><span class="sxs-lookup"><span data-stu-id="88f3a-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="88f3a-110">Diese Methoden speichern die Instanz der Filter-Klasse und der zugehörigen Daten während der Seiten Postbacks.</span><span class="sxs-lookup"><span data-stu-id="88f3a-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="88f3a-111">Schließlich wird erläutert, wie mehrere Objekte in der Implementierung des Steuerungs Zustands gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="88f3a-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="88f3a-112">&#9654;Video ansehen (23 Minuten)</span><span class="sxs-lookup"><span data-stu-id="88f3a-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
