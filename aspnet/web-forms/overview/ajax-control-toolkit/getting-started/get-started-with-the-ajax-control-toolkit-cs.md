---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Beginnen Sie mit dem AJAX Control Toolkit (C#) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie alles, was Sie wissen müssen, um mit dem AJAX Control Toolkit zu beginnen.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 7478b090ec52778572d70065983de6be8bdb4e6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504087"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="f6755-103">Erste Schritte mit dem AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="f6755-103">Get Started with the AJAX Control Toolkit (C#)</span></span>

<span data-ttu-id="f6755-104">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f6755-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f6755-105">Erfahren Sie alles, was Sie wissen müssen, um mit dem AJAX Control Toolkit zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="f6755-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>

<span data-ttu-id="f6755-106">Das AJAX Control Toolkit enthält mehr als 30 kostenlose Steuerelemente, die Sie in Ihren ASP.NET-Anwendungen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="f6755-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="f6755-107">In diesem Tutorial erfahren Sie, wie Sie das AJAX Control Toolkit herunterladen und die Toolkit-Steuerelemente zu Ihrer Visual Studio-/Visual Web Developer Express-Toolbox hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f6755-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="f6755-108">Herunterladen des AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="f6755-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="f6755-109">Das [AJAX Control Toolkit](http://devexpress.com/act) ist ein Open Source-Projekt, das von den Mitgliedern der ASP.net-Community und des ASP.NET-Teams entwickelt wurde.</span><span class="sxs-lookup"><span data-stu-id="f6755-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 

<span data-ttu-id="f6755-110">[![das AJAX Control Toolkit herunterladen](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f6755-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="f6755-111">**Abbildung 01**: Herunterladen des AJAX Control Toolkit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f6755-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>

<span data-ttu-id="f6755-112">Nachdem Sie die Datei heruntergeladen haben, müssen Sie die Blockierung der Datei entsperren.</span><span class="sxs-lookup"><span data-stu-id="f6755-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="f6755-113">Klicken Sie mit der rechten Maustaste auf die Datei, wählen Sie Eigenschaften aus, und klicken Sie **auf die Schaltfläche entsperren (** siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="f6755-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>

<span data-ttu-id="f6755-114">[Aufheben der Blockierung der ZIP-Datei für das AJAX Control Toolkit ![](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f6755-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="f6755-115">**Abbildung 02**: Aufheben der Blockierung der AJAX Control Toolkit-ZIP-Datei ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f6755-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>

<span data-ttu-id="f6755-116">Nach dem Entsperren der Datei können Sie die Datei entpacken: Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie die Menüoption **Alle extrahieren** aus.</span><span class="sxs-lookup"><span data-stu-id="f6755-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="f6755-117">Nun können wir das Toolkit der Visual Studio-/Visual Web Developer-Toolbox hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f6755-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="f6755-118">Hinzufügen des AJAX Control Toolkit zur Toolbox</span><span class="sxs-lookup"><span data-stu-id="f6755-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="f6755-119">Die einfachste Möglichkeit, das AJAX Control Toolkit zu verwenden, besteht darin, das Toolkit Ihrer Visual Studio-/Visual Web Developer-Toolbox hinzuzufügen (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="f6755-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="f6755-120">Auf diese Weise können Sie einfach ein Toolkit-Steuerelement auf eine Seite ziehen, wenn Sie es verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="f6755-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>

<span data-ttu-id="f6755-121">[![AJAX Control Toolkit wird in Toolbox angezeigt.](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f6755-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="f6755-122">**Abbildung 03**: AJAX Control Toolkit wird in der Toolbox angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f6755-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>

<span data-ttu-id="f6755-123">Zunächst müssen Sie der Toolbox eine Registerkarte des AJAX-Steuerelement-Toolkits hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f6755-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="f6755-124">Führen Sie folgende Schritte durch:</span><span class="sxs-lookup"><span data-stu-id="f6755-124">Follow these steps.</span></span>

1. <span data-ttu-id="f6755-125">Erstellen Sie eine neue ASP.NET-Website, indem Sie die Menü Optionsdatei, neue Website auswählen.</span><span class="sxs-lookup"><span data-stu-id="f6755-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="f6755-126">Doppelklicken Sie im Fenster Projektmappen-Explorer auf die Datei default. aspx, um die Datei im Editor zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f6755-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="f6755-127">Klicken Sie mit der rechten Maustaste unterhalb der Registerkarte Allgemein auf die Toolbox, und wählen Sie die Menüoption **Registerkarte hinzufügen** (siehe Abbildung 4)</span><span class="sxs-lookup"><span data-stu-id="f6755-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="f6755-128">Geben Sie eine neue Registerkarte mit dem Namen AJAX Control Toolkit ein.</span><span class="sxs-lookup"><span data-stu-id="f6755-128">Enter a new tab named AJAX Control Toolkit.</span></span>

<span data-ttu-id="f6755-129">[![Hinzufügen einer neuen Registerkarte](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f6755-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="f6755-130">**Abbildung 04**: Hinzufügen einer neuen Registerkarte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="f6755-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>

<span data-ttu-id="f6755-131">Als nächstes müssen Sie die AJAX Control Toolkit-Steuerelemente zur neuen Registerkarte hinzufügen. führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="f6755-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="f6755-132">Klicken Sie mit der rechten Maustaste unterhalb der Registerkarte AJAX Control Toolkit, und wählen Sie die Menüoption **Elemente auswählen (siehe Abbildung 5)** .</span><span class="sxs-lookup"><span data-stu-id="f6755-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="f6755-133">Navigieren Sie zu dem Speicherort, an dem Sie das AJAX Control Toolkit entzippt haben, und wählen Sie die Assembly "AjaxControlToolkit. dll"</span><span class="sxs-lookup"><span data-stu-id="f6755-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>

<span data-ttu-id="f6755-134">[![Elemente auswählen, die der Toolbox hinzugefügt werden sollen.](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f6755-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="f6755-135">**Abbildung 05**: Auswählen der Elemente, die der Toolbox hinzugefügt werden sollen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="f6755-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>

<span data-ttu-id="f6755-136">Nachdem Sie diese Schritte ausgeführt haben, werden alle Toolkit-Steuerelemente in der Toolbox angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f6755-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="f6755-137">Aktualisieren auf eine neue Version des Toolkits</span><span class="sxs-lookup"><span data-stu-id="f6755-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="f6755-138">Wenn Sie eine ältere Version des Toolkits verwendet haben und jetzt zu einer späteren Version wechseln müssen, werden die empfohlenen Schritte ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="f6755-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="f6755-139">Binärdateien: Löschen Sie die alte Version der Datei "AjaxControlToolkit. dll" aus dem Ordner "bin" der Website.</span><span class="sxs-lookup"><span data-stu-id="f6755-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="f6755-140">Toolbox Items-löschen Sie die Registerkarte "AJAX Control Toolkit", und befolgen Sie die obigen Schritte, um die Registerkarte mit der neuen Version der "AjaxControlToolkit. dll"-Assembly neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6755-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f6755-141">Weiter</span><span class="sxs-lookup"><span data-stu-id="f6755-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
