---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Erstellen einer benutzerdefinierten Routen Einschränkung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther zeigt, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können. Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2330708cf4a28180ce8a05f4696bf7a7a32092d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486789"
---
# <a name="creating-a-custom-route-constraint-vb"></a>Erstellen einer benutzerdefinierten Routeneinschränkung (VB)

von [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther zeigt, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können. Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird, wenn eine Browser Anforderung von einem Remote Computer aus erfolgt.

In diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können. Mit einer benutzerdefinierten Routen Einschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung stimmt überein.

In diesem Tutorial erstellen wir eine localhost-Routen Einschränkung. Die localhost-Routen Einschränkung entspricht nur Anforderungen, die vom lokalen Computer ausgeführt wurden. Remote Anforderungen aus dem Internet stimmen nicht überein.

Sie implementieren eine benutzerdefinierte Routen Einschränkung, indem Sie die irouteeinschränkung-Schnittstelle implementieren. Dies ist eine sehr einfache Schnittstelle, die eine einzelne Methode beschreibt:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Die-Methode gibt einen booleschen Wert zurück. Wenn Sie false zurückgeben, entspricht die der Einschränkung zugeordnete Route nicht der Browser Anforderung.

Die localhost-Einschränkung ist in der Liste 1 enthalten.

**Codebeispiel 1: localhosteinschränkung. vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Die Einschränkung in der Auflistung 1 nutzt die IsLocal-Eigenschaft, die von der HttpRequest-Klasse verfügbar gemacht wird. Diese Eigenschaft gibt true zurück, wenn die IP-Adresse der Anforderung 127.0.0.1 ist oder wenn die IP-Adresse der Anforderung mit der IP-Adresse des Servers identisch ist.

Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global. asax" definiert ist. Die Datei Global. asax in der Liste 2 verwendet die localhost-Einschränkung, um zu verhindern, dass eine Administrator Seite von einer Administrator Seite angefordert wird, es sei denn, Sie stellen die Anforderung vom lokalen Server Eine Anforderung für/admin/DeleteAll schlägt beispielsweise fehl, wenn Sie von einem Remote Server aus ausgeführt wird.

**Codebeispiel 2: Global. asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Die localhost-Einschränkung wird in der Definition der admin-Route verwendet. Diese Route wird nicht mit einer Remote Browser Anforderung abgeglichen. Beachten Sie jedoch, dass andere in "Global. asax" definierte Routen mit derselben Anforderung übereinstimmen könnten. Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route eine Anforderung abstimmt und nicht alle Routen, die in der Datei "Global. asax" definiert sind.

Beachten Sie, dass die Standardroute aus der Datei Global. asax in der Liste 2 auskommentiert wurde. Wenn Sie die Standardroute einschließen, entspricht die Standardroute den Anforderungen des Admin-Controllers. In diesem Fall können Remote Benutzer weiterhin Aktionen des Admin-Controllers aufrufen, auch wenn Ihre Anforderungen nicht der admin-Route entsprechen.

> [!div class="step-by-step"]
> [Previous](creating-a-route-constraint-vb.md)
