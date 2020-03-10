---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validieren mit einer Dienst Ebene (C#) | Microsoft-Dokumentation
author: StephenWalther
description: Erfahren Sie, wie Sie Ihre Validierungs Logik aus Ihren Controller Aktionen und in eine separate Dienst Schicht verschieben. In diesem Tutorial erläutert Stephen Walther, wie Sie...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: da1a1c9cc79a452eb0d7597810e86f7bcf6cd179
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436605"
---
# <a name="validating-with-a-service-layer-c"></a>Überprüfen mit einer Dienstschicht (C#)

von [Stephen Walther](https://github.com/StephenWalther)

> Erfahren Sie, wie Sie Ihre Validierungs Logik aus Ihren Controller Aktionen und in eine separate Dienst Schicht verschieben. In diesem Tutorial erläutert Stephen Walther, wie Sie eine deutliche Trennung der Belange gewährleisten können, indem Sie die Dienst Ebene von ihrer Controller Schicht isolieren.

Ziel dieses Tutorials ist es, eine Methode zur Durchführung der Validierung in einer ASP.NET MVC-Anwendung zu beschreiben. In diesem Tutorial erfahren Sie, wie Sie Ihre Validierungs Logik aus ihren Controllern und in eine separate Dienst Schicht verschieben.

## <a name="separating-concerns"></a>Trennung von Belangen

Wenn Sie eine ASP.NET MVC-Anwendung erstellen, sollten Sie Ihre Daten Bank Logik nicht in den Controller Aktionen platzieren. Durch die Kombination von Datenbank und Controller Logik wird die Verwaltung Ihrer Anwendung im Laufe der Zeit erschwert. Es wird empfohlen, dass Sie die gesamte Daten Bank Logik in einer separaten Repository-Schicht platzieren.

Beispielsweise enthält die Auflistung 1 ein einfaches Repository namens productrepository. Das produktrepository enthält den gesamten Datenzugriffs Code für die Anwendung. Die Auflistung enthält auch die iproductrepository-Schnittstelle, die das produktrepository implementiert.

**Codebeispiel 1: models\productrepositoriy.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Der Controller in der Liste 2 verwendet die Repository-Ebene sowohl in der Index ()-als auch der Create ()-Aktion. Beachten Sie, dass dieser Controller keine Daten Bank Logik enthält. Durch das Erstellen einer Repository-Ebene können Sie eine saubere Trennung der Belange gewährleisten. Controller sind für Anwendungs Fluss-Steuerungslogik zuständig, und das Repository ist für die Datenzugriffs Logik verantwortlich.

**Codebeispiel 2: controllers\productcontroller.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Erstellen einer Dienst Ebene

Daher gehört die Logik der Anwendungs Fluss Steuerung zu einem Controller, und die Datenzugriffs Logik gehört zu einem Repository. Wo platzieren Sie die Validierungs Logik in diesem Fall? Eine Möglichkeit besteht darin, die Validierungs Logik in eine *Dienst Ebene*zu platzieren.

Eine Dienst Ebene ist eine zusätzliche Ebene in einer ASP.NET MVC-Anwendung, die die Kommunikation zwischen einem Controller und einer Repository-Schicht vermittelt. Die Dienst Ebene enthält Geschäftslogik. Insbesondere enthält Sie Validierungs Logik.

Beispielsweise verfügt die Produkt Dienst Schicht in der Liste 3 über eine Methode "kreateproduct ()". Die Methode "Methode ()" Ruft die Methode "validateproduct ()" auf, um ein neues Produkt zu validieren, bevor das Produkt an das produktrepository übergeben wird.

**Codebeispiel 3: models\productservice.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Der Produkt Controller wurde in der Liste 4 aktualisiert, sodass anstelle der Repository-Ebene die Dienst Ebene verwendet wird. Die Controller Schicht spricht mit der Dienst Ebene. Die Dienst Ebene spricht mit der Repository-Ebene. Jede Ebene hat eine separate Verantwortung.

**Codebeispiel 4-controllers\productcontroller.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Beachten Sie, dass der Product Service im Product Controller-Konstruktor erstellt wird. Wenn der Product Service erstellt wird, wird das Modell Zustands Wörterbuch an den Dienst übermittelt. Der Product Service verwendet den Modell Status, um Validierungs Fehlermeldungen an den Controller zurückzuleiten.

## <a name="decoupling-the-service-layer"></a>Entkoppeln der Dienst Ebene

Die Controller-und Dienst Ebenen konnten nicht in einer Hinsicht isoliert werden. Die Controller-und Dienst Ebenen kommunizieren über den Modell Zustand. Anders ausgedrückt: die Dienst Schicht hat eine Abhängigkeit von einer bestimmten Funktion des ASP.NET-MVC-Frameworks.

Wir möchten die Dienst Ebene so weit wie möglich von unserer Controller Schicht isolieren. Theoretisch sollten wir die Dienst Schicht mit allen Anwendungs Typen und nicht nur mit einer ASP.NET MVC-Anwendung verwenden können. Beispielsweise möchten wir in Zukunft möglicherweise ein WPF-Front-End für die Anwendung erstellen. Wir sollten eine Möglichkeit finden, die Abhängigkeit vom ASP.NET MVC-Modell Status von unserer Dienst Schicht zu entfernen.

In der Liste 5 wurde die Dienst Schicht so aktualisiert, dass Sie nicht mehr den Modell Status verwendet. Stattdessen wird eine beliebige Klasse verwendet, die die ivalidationdictionary-Schnittstelle implementiert.

**Auflistung 5-models\productservice.cs (entkoppelt)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

Die ivalidationdictionary-Schnittstelle ist in der Liste 6 definiert. Diese einfache Schnittstelle verfügt über eine einzelne-Methode und eine einzelne-Eigenschaft.

**Codebeispiel 6: models\ivalidationditionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

Die Klasse in der Auflistung 7 namens der modelstatewrapper-Klasse implementiert die ivalidationdictionary-Schnittstelle. Sie können die modelstatewrapper-Klasse instanziieren, indem Sie ein Modell Zustands Wörterbuch an den-Konstruktor übergeben.

**Codebeispiel 7: models\modelstatewrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Schließlich verwendet der aktualisierte Controller in der Auflistung 8 den modelstatewrapper, wenn die Dienst Ebene im Konstruktor erstellt wird.

**Auflisten von 8-controllers\productcontroller.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

Mithilfe der ivalidationdictionary-Schnittstelle und der modelstatewrapper-Klasse können wir unsere Dienst Schicht vollständig von unserer Controller Schicht isolieren. Die Dienst Ebene ist nicht mehr vom Modell Zustand abhängig. Sie können eine beliebige Klasse übergeben, die die ivalidationdictionary-Schnittstelle implementiert, an die Dienst Ebene. Eine WPF-Anwendung kann z. b. die ivalidationdictionary-Schnittstelle mit einer einfachen Auflistungs Klasse implementieren.

## <a name="summary"></a>Zusammenfassung

Ziel dieses Tutorials war es, einen Ansatz für die Durchführung der Validierung in einer ASP.NET MVC-Anwendung zu erörtern. In diesem Tutorial haben Sie erfahren, wie Sie Ihre gesamte Validierungs Logik aus ihren Controllern und in eine separate Dienst Schicht verschieben. Außerdem haben Sie erfahren, wie Sie die Dienst Ebene von ihrer Controller Schicht isolieren, indem Sie eine modelstatewrapper-Klasse erstellen.

> [!div class="step-by-step"]
> [Zurück](validating-with-the-idataerrorinfo-interface-cs.md)
> [Weiter](validation-with-the-data-annotation-validators-cs.md)
