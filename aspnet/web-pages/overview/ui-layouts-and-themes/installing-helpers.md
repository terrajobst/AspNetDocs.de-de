---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installieren eines Hilfsobjekts in einer ASP.net Web Pages-(Razor-) Site | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird beschrieben, wie ein Hilfsprogramm auf einer ASP.net Web Pages-Website (Razor) installiert wird. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup für pro...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518643"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Installieren eines Hilfsobjekts in einer ASP.net Web Pages-(Razor-) Site

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie ein Hilfsprogramm auf einer ASP.net Web Pages-Website (Razor) installiert wird. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup enthält, *um eine Aufgabe* auszuführen, die vielleicht mühsam oder komplex ist.
> 
> Sie lernen Folgendes:
> 
> - Installieren eines Hilfsprogramms in einer mit webmatrix 3 erstellten Website.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Webmatrix 3

## <a name="overview-of-helpers"></a>Übersicht über Hilfsprogramme

Einige Aufgaben, die häufig auf Webseiten ausgeführt werden sollen, erfordern viel Code oder erfordern zusätzliches wissen. Beispiele hierfür sind das Anzeigen eines Diagramms für Daten. Verschieben einer Twitter-Schaltfläche "Follow" auf einer Seite Senden von e-Mails von Ihrer Website Zuschneiden oder Ändern der Größe von Bildern Verwenden von PayPal für Ihre Website. Um dies zu vereinfachen, können *Sie mit ASP.net Web Pages Hilfsprogramme verwenden.* Hilfsprogramme sind Komponenten, die Sie für einen-Standort installieren und mit denen Sie typische Aufgaben durchführen können, indem Sie nur eine Zeile oder zwei Razor-Codes verwenden.

In ASP.net Web Pages sind einige hilfshilfen integriert. Viele Hilfsprogramme sind jedoch in Paketen (Add-Ins) verfügbar, die mit dem nuget-Paket-Manager bereitgestellt werden. Mit nuget können Sie ein Paket für die Installation auswählen. Anschließend werden alle Details der Installation behandelt.

## <a name="installing-a-helper-in-webmatrix-3"></a>Installieren eines Hilfsprogramms in webmatrix 3

1. Klicken Sie in webmatrix 3 auf die Schaltfläche **nuget** .

    ![Dialogfeld "nuget-Katalog" in webmatrix](installing-helpers/_static/image1.png)
2. Der nuget-Paket-Manager wird gestartet, und die verfügbaren Pakete werden angezeigt. Geben Sie im Suchfeld ein Schlüsselwort für das Hilfsprogramm ein, das Sie installieren möchten.

    ![Dialogfeld "nuget-Katalog" in webmatrix](installing-helpers/_static/image2.png)
3. Wählen Sie das Paket aus, und klicken Sie auf **Installieren**. Klicken Sie bei der Frage, ob Sie das Paket installieren möchten, auf **Ja** , und geben Sie an, dass Sie die Bedingungen akzeptieren.

     Wenn Sie das Hilfsprogramm zum ersten Mal installiert haben, erstellt nuget für den Code, der das Hilfsprogramm bildet, Ordner auf Ihrer Website.
4. Wenn Sie ein Hilfsprogramm deinstallieren möchten, klicken Sie auf die Schaltfläche **Galerie** , klicken Sie auf die Registerkarte **installiert** , und wählen Sie das zu deinstallierende

## <a name="installing-the-twitter-helper"></a>Installieren des Twitter-Hilfsprogramms

Die neueste Version der Twitter-API ist nicht mit dem Twitter-Hilfsprogramm kompatibel, das Sie über nuget installieren. Informationen zum Einrichten des Twitter-Hilfsprogramms in Ihrem Projekt finden Sie stattdessen im Twitter-Hilfsprogramm [mit webmatrix](twitter-helper.md) .

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Einführung in die Grundlagen der ASP.net Web Pages 2-Programmierung](../getting-started/introducing-razor-syntax-c.md)

[Twitter-Hilfsprogramm mit webmatrix](twitter-helper.md)
