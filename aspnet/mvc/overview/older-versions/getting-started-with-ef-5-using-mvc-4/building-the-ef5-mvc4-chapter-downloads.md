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
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="cdc12-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span><span class="sxs-lookup"><span data-stu-id="cdc12-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>

<span data-ttu-id="cdc12-104">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cdc12-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="cdc12-105">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="cdc12-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="cdc12-106">Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="cdc12-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="cdc12-107">Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="cdc12-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

## <a name="building-the-chapter-downloads"></a><span data-ttu-id="cdc12-108">Durchführen von Downloads der einzelnen Kapitel</span><span class="sxs-lookup"><span data-stu-id="cdc12-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="cdc12-109">Herunterladen und Entpacken der ZIP-Datei für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="cdc12-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="cdc12-110">Im entzippten Downloadpaket finden Sie zusätzliche ZIP-Dateien, eine für den Abschluss der einzelnen Kapitel.</span><span class="sxs-lookup"><span data-stu-id="cdc12-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="cdc12-111">Klicken Sie mit der rechten Maustaste auf die gewünschte Zip-Datei, klicken Sie auf **Eigenschaften**und dann **auf die Schaltfläche entsperren.**</span><span class="sxs-lookup"><span data-stu-id="cdc12-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="cdc12-112">Entzippen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="cdc12-112">Unzip the file.</span></span>
4. <span data-ttu-id="cdc12-113">Doppelklicken Sie auf die Datei *CUX. sln* , um Visual Studio zu starten.</span><span class="sxs-lookup"><span data-stu-id="cdc12-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="cdc12-114">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**und dann auf Paket-Manager- **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="cdc12-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="cdc12-115">Klicken Sie in der Paket-Manager-Konsole (PMC) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="cdc12-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="cdc12-116">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cdc12-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="cdc12-117">Starten Sie Visual Studio neu, und öffnen Sie die Projektmappendatei, die Sie im obigen Schritt geschlossen haben</span><span class="sxs-lookup"><span data-stu-id="cdc12-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="cdc12-118">Geben Sie in der Paket-Manager-Konsole (PMC) den `Update-Database` Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="cdc12-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="cdc12-119">Möglicherweise wird die folgende Fehlermeldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="cdc12-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="cdc12-120">*Der Begriff "Update-Database" wird nicht als Name eines Cmdlets, einer Funktion, einer Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder überprüfen Sie, ob der Pfad korrekt ist, und versuchen Sie es noch mal.*</span><span class="sxs-lookup"><span data-stu-id="cdc12-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="cdc12-121">Beenden Sie, und starten Sie Visual Studio neu.</span><span class="sxs-lookup"><span data-stu-id="cdc12-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="cdc12-122">Jede Migration wird ausgeführt, und die Seed-Methode wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cdc12-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="cdc12-123">Sie können jetzt die app ausführen.</span><span class="sxs-lookup"><span data-stu-id="cdc12-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="cdc12-124">Previous</span><span class="sxs-lookup"><span data-stu-id="cdc12-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
