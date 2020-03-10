---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Teil 8: abschließende Seiten, Ausnahmebehandlung und Schlussfolgerung | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 8 fügt eine Kontaktseite, eine Seite und eine Ausnahme hinzu...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474339"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a>Teil 8: abschließende Seiten, Ausnahmebehandlung und Schlussfolgerung

von [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen. Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 8 fügt eine Kontaktseite, Informationen zur Seite und Ausnahmebehandlung hinzu. Dies ist das Ende der Reihe.

## <a id="_Toc260221680"></a>Kontaktseite (e-Mail senden von ASP.net)

Erstellen Sie eine neue Seite mit dem Namen contactus. aspx.

Erstellen Sie mit dem Designer das folgende Formular, und verwenden Sie dabei eine besondere Notiz, um den toolkitscriptmanager und das Editor-Steuerelement aus dem AjaxControlToolkit einzubeziehen. erforderlich.

![](tailspin-spyworks-part-8/_static/image1.jpg)

Doppelklicken Sie auf die Schaltfläche "Senden", um einen Click-Ereignishandler in der Code Behind-Datei zu generieren, und implementieren Sie eine Methode, um die Kontaktinformationen als e-Mail zu senden.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Dieser Code erfordert, dass Ihre Web. config-Datei einen Eintrag im Konfigurations Abschnitt enthält, der den SMTP-Server angibt, der zum Senden von e-Mails verwendet werden soll.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Info-Seite

Erstellen Sie eine Seite mit dem Namen "aboutus. aspx", und fügen Sie den gewünschten Inhalt hinzu.

## <a id="_Toc260221682"></a>Globaler Ausnahme Handler

Schließlich haben wir in der gesamten Anwendung Ausnahmen ausgelöst, und es gibt unvorhersehbare Umstände, die ebenfalls zu unbehandelten Ausnahmen in unserer Webanwendung führen.

Wir möchten niemals, dass eine nicht behandelte Ausnahme für den Besucher einer Website angezeigt wird.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Abgesehen davon, dass es sich nicht um eine schreckliche Benutzer Darstellung handelt, kann es auch zu Sicherheitsproblemen kommen.

Um dieses Problem zu beheben, wird ein globaler Ausnahmehandler implementiert.

Öffnen Sie hierzu die Datei Global. asax, und notieren Sie sich den folgenden vorab generierten Ereignishandler.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Fügen Sie Code hinzu, um den Anwendungs\_Fehlerhandler wie folgt zu implementieren.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Fügen Sie der Projekt Mappe dann eine Seite mit dem Namen Error. aspx hinzu, und fügen Sie diesen Markup Ausschnitt hinzu.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Extrahieren Sie nun auf der Seite\_Load-Ereignishandler die Fehlermeldungen aus dem Anforderungs Objekt.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Abschluss

Wir haben gesehen, dass ASP.net WebForms das Erstellen einer ausgereiften Website mit Datenbankzugriff, Mitgliedschaft, AJAX usw. erleichtert. ziemlich schnell.

Hoffentlich haben Sie in diesem Tutorial die Tools erhalten, die Sie benötigen, um mit dem Aufbau eigener ASP.net WebForms-Anwendungen zu beginnen.

> [!div class="step-by-step"]
> [Previous](tailspin-spyworks-part-7.md)
