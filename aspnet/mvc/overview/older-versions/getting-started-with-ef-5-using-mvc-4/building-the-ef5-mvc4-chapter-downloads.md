---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Building the Chapter Downloads for the EF 5 MVC 4 Tutorials | Microsoft-Dokumentation
author: Rick-Anderson
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 10237af40e3914b65e5181f17555697e86adea4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485121"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Building the Chapter Downloads for the EF 5 MVC 4 Tutorials

von [Rick Anderson](https://twitter.com/RickAndMSFT)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

## <a name="building-the-chapter-downloads"></a>Durchführen von Downloads der einzelnen Kapitel

1. Herunterladen und Entpacken der ZIP-Datei für das Projekt. Im entzippten Downloadpaket finden Sie zusätzliche ZIP-Dateien, eine für den Abschluss der einzelnen Kapitel.
2. Klicken Sie mit der rechten Maustaste auf die gewünschte Zip-Datei, klicken Sie auf **Eigenschaften**und dann **auf die Schaltfläche entsperren.**  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Entzippen Sie die Datei.
4. Doppelklicken Sie auf die Datei *CUX. sln* , um Visual Studio zu starten.
5. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**und dann auf Paket-Manager- **Konsole**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. Klicken Sie in der Paket-Manager-Konsole (PMC) auf **Wiederherstellen**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Beenden Sie Visual Studio.
8. Starten Sie Visual Studio neu, und öffnen Sie die Projektmappendatei, die Sie im obigen Schritt geschlossen haben
9. Geben Sie in der Paket-Manager-Konsole (PMC) den `Update-Database` Befehl ein:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Möglicherweise wird die folgende Fehlermeldung angezeigt:  
    >   
    >  *Der Begriff "Update-Database" wird nicht als Name eines Cmdlets, einer Funktion, einer Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder überprüfen Sie, ob der Pfad korrekt ist, und versuchen Sie es noch mal.*  
    > Beenden Sie, und starten Sie Visual Studio neu.

    Jede Migration wird ausgeführt, und die Seed-Methode wird ausgeführt. Sie können jetzt die app ausführen.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Previous](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
