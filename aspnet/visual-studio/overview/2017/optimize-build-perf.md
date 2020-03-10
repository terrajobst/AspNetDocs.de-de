---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimieren der Buildleistung für die Projektmappe
author: AngelosP
description: Optimieren der Buildleistung für die Projektmappe
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504975"
---
# <a name="optimize-build-performance-for-solution"></a>Optimieren der Buildleistung für die Projektmappe

Visual Studio 2017 15,8 oder höher enthält ein Menü Element: **Build** > **ASP.NET-Kompilierung** > Optimieren der **Buildleistung für die Lösung**.

![Screenshot des neuen Menü Elements](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET kompiliert seine Sichten zur Laufzeit, was bedeutet, dass ein ASP.net-Projekt eine Kopie des Compilers enthält. Wenn jedoch die Kopie des Compilers auf einem Entwickler Computer nicht mit der Visual Studio-Kopie identisch ist, wird die Buildleistung in der Reihenfolge von 1-3 Sekunden pro inkrementellem Build beeinträchtigt. Mit dieser Funktion wird die Kopie des Compilers Ihres Projekts so aktualisiert, dass Sie mit Visual Studio identisch ist, was normalerweise inkrementelle Builds beschleunigt.

**Dies gilt nur für ASP.NET Framework 4.7.1 oder spätere Projekte, es gilt nicht für ASP.net Core.**
