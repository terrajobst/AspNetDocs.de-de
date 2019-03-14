---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimieren der Buildleistung für die Projektmappe
author: AngelosP
description: Optimieren der Buildleistung für die Projektmappe
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050507"
---
# <a name="optimize-build-performance-for-solution"></a>Optimieren der Buildleistung für die Projektmappe

Visual Studio 2017 15.8 oder höheren Versionen gehören ein Menüelements: **Erstellen Sie** > **ASP.NET-Kompilierung** > **Buildleistung Projektmappenverzeichnis**.

![Screenshot des neuen Menüelements](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET kompiliert die Ansichten zur Laufzeit, was bedeutet, dass ein ASP.NET-Projekt mit sich eine Kopie der Compiler führt. Jedoch auf einem Computer des Entwicklers bei der die Kopie des Compilers nicht mit Visual Studio Kopie übereinstimmt Buildleistung Größenordnung von 1 bis 3 Sekunden pro inkrementeller Build betroffen ist. Dieses Feature wird aktualisiert, Ihres Projekts Kopieren des Compilers mit Visual Studio übereinstimmen, inkrementelle Builds in der Regel beschleunigt wird.

**Dies gilt für ASP.NET Framework 4.7.1 oder höher nur Projekte, es gilt nicht für ASP.NET Core.**
