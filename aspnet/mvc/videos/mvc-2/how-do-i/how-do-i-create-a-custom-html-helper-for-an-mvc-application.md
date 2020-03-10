---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Gewusst wie: Erstellen eines benutzerdefinierten HTML-Hilfsprogramms für eine MVC-Anwendung | Microsoft-Dokumentation'
author: rick-anderson
description: In diesem Video zeigt Chris Pels, wie ein benutzerdefiniertes htmlhelper erstellt wird, das im Standardsatz in einer MVC-Anwendung nicht verfügbar ist. Zuerst ein Beispiel für eine MVC-Anwendung...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 60953243d3038667e4f729b1394e68f0c9d7c178
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450477"
---
# <a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a>Gewusst wie: Erstellen eines benutzerdefinierten HTML-Hilfsprogramms für eine MVC-Anwendung

von [Chris Pels](https://twitter.com/chrispels)

In diesem Video zeigt Chris Pels, wie ein benutzerdefiniertes htmlhelper erstellt wird, das im Standardsatz in einer MVC-Anwendung nicht verfügbar ist. Zuerst wird eine MVC-Beispielanwendung mit einem Demo Controller und einer Ansicht erstellt, um das benutzerdefinierte htmlhelper zu testen. Als nächstes wird ein Modul mit einer öffentlichen Funktion erstellt, bei der es sich um eine Erweiterungsmethode handelt, die die Implementierung des benutzerdefinierten htmlhelper darstellt. Das benutzerdefinierte Hilfsprogramm dient zum Erstellen von `<img>`-Tags in einer Seite und empfängt mehrere eingehende Parameter, einschließlich der ID, URL und des Alternativ Texts für das imagetag. Die Logik wird dann der-Funktion zum Zurückgeben des abgeschlossenen `<img>`-Tags mit den angegebenen Informationen hinzugefügt. Anschließend wird das benutzerdefinierte htmlhelper auf der Seite Demo zum Anzeigen eines Bilds verwendet. Schließlich wird das benutzerdefinierte htmlhelper erweitert und umfasst mehrere konstruktorüberschreibungen, die die Flexibilität erleichtern, unterschiedliche `<img>` Tags zu erstellen.

[&#9654;Video ansehen (18 Minuten)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> [Zurück](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Weiter](how-do-i-work-with-model-binders-in-an-mvc-application.md)
