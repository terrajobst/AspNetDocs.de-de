---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Teil 2: Datenzugriffs Ebene | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. In Teil 2 wird das Hinzufügen der Datenzugriffs Ebene behandelt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78462651"
---
# <a name="part-2-data-access-layer"></a>Teil 2: Datenzugriffs Ebene

von [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen. Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. In Teil 2 wird das Hinzufügen der Datenzugriffs Ebene behandelt.

## <a id="_Toc260221668"></a>Hinzufügen der Datenzugriffs Ebene

Unsere eCommerce-Anwendung hängt von zwei Datenbanken ab.

Für Kundeninformationen verwenden wir die ASP.NET-Standard Mitgliedschafts Datenbank. Für den Warenkorb und den Produktkatalog implementieren wir wie folgt eine SQL Express-Datenbank.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Wenn Sie die Datenbank (Commerce. mdf) im App-\_Datenordner der Anwendung erstellt haben, können wir mit dem Erstellen unserer Datenzugriffs Ebene mit dem .NET-Entity Framework fortfahren.

Erstellen Sie einen Ordner mit dem Namen "Data\_Access", und klicken Sie mit der rechten Maustaste auf diesen Ordner, und wählen Sie "Neues Element hinzufügen" aus.

Geben Sie im Element "installierte Vorlagen" den Eintrag "ADO.NET Entity Data Model" ein, und klicken Sie dann auf "EDM\_Commerce. edmx", und klicken Sie auf die Schaltfläche "hinzufügen".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Wählen Sie "aus Datenbank generieren" aus.

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Speichern und erstellen Sie.

Nun können wir unsere erste Funktion hinzufügen – ein Menü für die Produktkategorie.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-1.md)
> [Weiter](tailspin-spyworks-part-3.md)
