---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Erstellen einer benutzerdefinierten Routen EinschränkungC#() | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther zeigt, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können. Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 98d5839e3d2623665770ccc5689c28f9eb29c8f6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486825"
---
# <a name="creating-a-custom-route-constraint-c"></a>Erstellen einer benutzerdefinierten Routeneinschränkung (C#)

von [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther zeigt, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können. Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird, wenn eine Browser Anforderung von einem Remote Computer aus erfolgt.

In diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können. Mit einer benutzerdefinierten Routen Einschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung stimmt überein.

In diesem Tutorial erstellen wir eine localhost-Routen Einschränkung. Die localhost-Routen Einschränkung entspricht nur Anforderungen, die vom lokalen Computer ausgeführt wurden. Remote Anforderungen aus dem Internet stimmen nicht überein.

Sie implementieren eine benutzerdefinierte Routen Einschränkung, indem Sie die irouteeinschränkung-Schnittstelle implementieren. Dies ist eine sehr einfache Schnittstelle, die eine einzelne Methode beschreibt:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Die-Methode gibt einen booleschen Wert zurück. Wenn Sie false zurückgeben, entspricht die der Einschränkung zugeordnete Route nicht der Browser Anforderung.

Die localhost-Einschränkung ist in der Liste 1 enthalten.

**Codebeispiel 1-LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Die Einschränkung in der Auflistung 1 nutzt die IsLocal-Eigenschaft, die von der HttpRequest-Klasse verfügbar gemacht wird. Diese Eigenschaft gibt true zurück, wenn die IP-Adresse der Anforderung 127.0.0.1 ist oder wenn die IP-Adresse der Anforderung mit der IP-Adresse des Servers identisch ist.

Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global. asax" definiert ist. Die Datei Global. asax in der Liste 2 verwendet die localhost-Einschränkung, um zu verhindern, dass eine Administrator Seite von einer Administrator Seite angefordert wird, es sei denn, Sie stellen die Anforderung vom lokalen Server Eine Anforderung für/admin/DeleteAll schlägt beispielsweise fehl, wenn Sie von einem Remote Server aus ausgeführt wird.

**Codebeispiel 2: Global. asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Die localhost-Einschränkung wird in der Definition der admin-Route verwendet. Diese Route wird nicht mit einer Remote Browser Anforderung abgeglichen. Beachten Sie jedoch, dass andere in "Global. asax" definierte Routen mit derselben Anforderung übereinstimmen könnten. Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route eine Anforderung abstimmt und nicht alle Routen, die in der Datei "Global. asax" definiert sind.

Beachten Sie, dass die Standardroute aus der Datei Global. asax in der Liste 2 auskommentiert wurde. Wenn Sie die Standardroute einschließen, entspricht die Standardroute den Anforderungen des Admin-Controllers. In diesem Fall können Remote Benutzer weiterhin Aktionen des Admin-Controllers aufrufen, auch wenn Ihre Anforderungen nicht der admin-Route entsprechen.

> [!div class="step-by-step"]
> [Zurück](creating-a-route-constraint-cs.md)
> [Weiter](asp-net-mvc-controller-overview-vb.md)
