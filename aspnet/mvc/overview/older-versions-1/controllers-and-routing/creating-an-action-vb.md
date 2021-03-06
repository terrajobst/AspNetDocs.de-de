---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Erstellen einer Aktion (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie einem ASP.NET-MVC-Controller eine neue Aktion hinzufügen. Erfahren Sie mehr über die Anforderungen für eine Methode, die eine Aktion sein soll.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: b1b53bea899deecef203551b23c087944e3990ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470157"
---
# <a name="creating-an-action-vb"></a>Erstellen einer Aktion (VB)

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie einem ASP.NET-MVC-Controller eine neue Aktion hinzufügen. Erfahren Sie mehr über die Anforderungen für eine Methode, die eine Aktion sein soll.

In diesem Tutorial wird erläutert, wie Sie eine neue Controller Aktion erstellen können. Sie erfahren mehr über die Anforderungen einer Aktionsmethode. Außerdem erfahren Sie, wie Sie verhindern können, dass eine Methode als Aktion verfügbar gemacht wird.

## <a name="adding-an-action-to-a-controller"></a>Hinzufügen einer Aktion zu einem Controller

Sie fügen einem Controller eine neue Aktion hinzu, indem Sie dem Controller eine neue Methode hinzufügen. Der Controller in der Liste 1 enthält z. b. eine Aktion mit dem Namen Index () und eine Aktion mit dem Namen "SayHello ()". Beide Methoden werden als Aktionen verfügbar gemacht.

**Codebeispiel 1-controllers\homecontroller.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Um dem Universum als Aktion zur Verfügung gestellt zu werden, muss eine Methode bestimmte Anforderungen erfüllen:

- Die Methode muss öffentlich sein.
- Die Methode kann keine statische Methode sein.
- Die Methode kann keine Erweiterungsmethode sein.
- Bei der Methode kann es sich nicht um einen Konstruktor, einen Getter oder einen Setter handeln.
- Die-Methode kann keine offenen generischen Typen aufweisen.
- Die-Methode ist keine Methode der Controller Basisklasse.
- Die-Methode kann keine **ref** -oder **out** -Parameter enthalten.

Beachten Sie, dass es für den Rückgabetyp einer Controller Aktion keine Einschränkungen gibt. Eine Controller Aktion kann eine Zeichenfolge, einen DateTime-Wert, eine Instanz der Zufalls Klasse oder "void" zurückgeben. Das ASP.NET-MVC-Framework konvertiert jeden Rückgabetyp, der kein Aktions Ergebnis ist, in eine Zeichenfolge und stellt die Zeichenfolge im Browser dar.

Wenn Sie einem Controller eine Methode hinzufügen, die diese Anforderungen nicht verletzt, wird die Methode als Controller Aktion verfügbar gemacht. Seien Sie vorsichtig. Eine Controller Aktion kann von allen Personen aufgerufen werden, die mit dem Internet verbunden sind. Erstellen Sie z. b. eine deletemywebsite ()-Controller Aktion nicht.

## <a name="preventing-a-public-method-from-being-invoked"></a>Verhindern, dass eine öffentliche Methode aufgerufen wird

Wenn Sie eine öffentliche Methode in einer Controller Klasse erstellen müssen und die Methode nicht als Controller Aktion verfügbar machen möchten, können Sie verhindern, dass die Methode mithilfe des &lt;NonAction&gt;-Attributs aufgerufen wird. Der Controller in der Liste 2 enthält z. b. eine öffentliche Methode mit dem Namen companysecrets (), die mit dem &lt;NonAction&gt;-Attribut ergänzt wird.

**Codebeispiel 2: controllers\workcontroller.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Wenn Sie versuchen, die controanysecrets ()-Controller Aktion aufzurufen, indem Sie/Work/CompanySecrets in die Adressleiste Ihres Browsers eingeben, erhalten Sie die Fehlermeldung in Abbildung 1.

[![Aufrufen einer NonAction-Methode](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Abbildung 01**: Aufrufen einer nicht Aktionsmethode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Zurück](creating-a-controller-vb.md)
> [Weiter](aspnet-mvc-controllers-overview-cs.md)
