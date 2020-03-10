---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Lebenszyklus einer ASP.NET MVC 5-Anwendung | Microsoft-Dokumentation
author: cephalin
description: Laden Sie ein PDF-Dokument herunter, das den Lebenszyklus einer ASP.NET MVC 5-Anwendung zeichnet. Dieses Lebenszyklus Dokument bietet einen Überblick über den MVC-Lebenszyklus...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: f4a9b3fb61552b070db11fba617b5627fcd71cd5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470325"
---
# <a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Lebenszyklus einer ASP.NET MVC 5-Anwendung

von [Cephas Lin](https://github.com/cephalin)

[PDF-Dokument herunterladen](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Hier können Sie ein PDF-Dokument herunterladen, das den Lebenszyklus jeder ASP.NET MVC 5-Anwendung für den Empfang der HTTP-Anforderung zum Senden der HTTP-Antwort an den Client aufzeichnet. Es ist sowohl als Schulungs Tool für Benutzer konzipiert, die mit ASP.NET MVC noch nicht vertraut sind, als auch als Referenz für diejenigen, die einen Drilldown in bestimmte Aspekte der Anwendung ausführen müssen. Das PDF-Dokument verfügt über die folgenden Features:

- Relevante [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) -Phasen, die Ihnen helfen zu verstehen, wo sich MVC in den [ASP.net-Anwendungslebenszyklus](https://msdn.microsoft.com/library/bb470252.aspx)integriert.
- Eine allgemeine Übersicht über den MVC-Anwendungslebenszyklus, in dem Sie die wichtigsten Phasen verstehen können, die jede MVC-Anwendung in der Pipeline für die Anforderungs Verarbeitung durchläuft.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Eine Detailansicht, die einen Drilldown in die Details der Pipeline für die Anforderungs Verarbeitung zeigt. Sie können die allgemeine Ansicht und die Detailansicht vergleichen, um zu sehen, wie die Lebenszyklus Details in den verschiedenen Phasen gesammelt werden. [Laden Sie PDF herunter](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) , um eine größere Ansicht anzuzeigen.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Platzierung und Zweck aller über schreibbaren Methoden auf dem [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) Objekt in der Pipeline für die Anforderungs Verarbeitung. Sie müssen möglicherweise eine Methode außer Kraft setzen, aber es ist wichtig, dass Sie Ihre Rolle im Anwendungslebenszyklus verstehen, damit Sie Code in der richtigen Lebenszyklusphase für die gewünschte Auswirkung schreiben können.
- In aufgebaufenen Diagrammen wird gezeigt, wie die einzelnen Filtertypen (Authentifizierung, Autorisierung, Aktion und Ergebnis) aufgerufen werden.
- Verknüpfen Sie einen nützlichen Artikel oder Blog aus den einzelnen Point of Interest in der Detailansicht.

## <a name="next-steps"></a>Nächste Schritte

Erfüllt dieses Dokument ihren Bedarf? Wir freuen uns über Ihr Feedback. Wenn Sie Fragen zum ASP.NET MVC-Lebenszyklus in Ihrer Anwendung haben, sind [StackOverflow](http://stackoverflow.com/help) -und [ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx) hervorragend geeignet. Folgen Sie [mir](https://twitter.com/Cephas_MSFT) auf Twitter, damit Sie Updates zu den neuesten Tutorials erhalten.
