---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Gewusst wie:] Verwenden Sie die Eigenschaft "reponse. Filter", um HTML in einer ASP.NET-Seite zu ersetzen | Microsoft-Dokumentation'
author: rick-anderson
description: In diesem Video zeigt Chris Pels, wie Sie die Eigenschaft "reponse. Filter" verwenden, um den HTML-Code abzufangen und zu ändern, der an eine Seite gesendet wird. Zuerst wird eine Beispielseite erstellt...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78487995"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Gewusst wie:] Verwenden Sie die Eigenschaft "reponse. Filter", um HTML auf einer ASP.NET-Seite zu ersetzen.

von [Chris Pels](https://twitter.com/chrispels)

In diesem Video zeigt Chris Pels, wie Sie die Eigenschaft "reponse. Filter" verwenden, um den HTML-Code abzufangen und zu ändern, der an eine Seite gesendet wird. Zuerst wird eine Beispielseite mit einem einfachen Text erstellt. Anschließend wird eine benutzerdefinierte Streamklasse erstellt, die als Ersatz Datenstrom für den aktuellen Stream dient, der an den Browser des Benutzers gesendet wird. In dieser benutzerdefinierten Streamklasse wird der Inhalt der Seite aus dem Stream abgerufen, geändert und dann in den Antwortstream geschrieben. In dieser benutzerdefinierten Streamklasse wird die Write-Methode angepasst, um den HTML-Code im basisantwortstream zu ersetzen, wodurch geändert wird, was an den Browser des Benutzers gesendet wird. Schließlich wird die neue Stream-Klasse der Response. Filter-Eigenschaft in der Seite\_Load-Ereignis zugewiesen, wodurch der Mechanismus zum Ändern des Seiteninhalts bereitgestellt wird.

[&#9654;Video ansehen (13 Minuten)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
