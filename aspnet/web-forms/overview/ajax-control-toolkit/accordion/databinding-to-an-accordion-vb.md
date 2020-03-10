---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Datenbindung an ein Akkordeon (VB) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen. Panels werden normalerweise als w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: bfb31940c0395c7ed1d5d471fb8fb686b66c59ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498027"
---
# <a name="databinding-to-an-accordion-vb"></a><span data-ttu-id="bba91-104">Datenbindung an Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="bba91-104">Databinding to an Accordion (VB)</span></span>

<span data-ttu-id="bba91-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bba91-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bba91-106">[Code herunterladen](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bba91-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span></span>

> <span data-ttu-id="bba91-107">Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="bba91-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="bba91-108">Panels werden in der Regel auf der Seite selbst deklariert, aber das Binden an eine Datenquelle bietet mehr Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="bba91-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="bba91-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bba91-109">Overview</span></span>

<span data-ttu-id="bba91-110">Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="bba91-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="bba91-111">Panels werden in der Regel auf der Seite selbst deklariert, aber das Binden an eine Datenquelle bietet mehr Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="bba91-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="bba91-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="bba91-112">Steps</span></span>

<span data-ttu-id="bba91-113">Zuerst ist eine Datenquelle erforderlich.</span><span class="sxs-lookup"><span data-stu-id="bba91-113">First of all, a data source is required.</span></span> <span data-ttu-id="bba91-114">In diesem Beispiel werden die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition verwendet.</span><span class="sxs-lookup"><span data-stu-id="bba91-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="bba91-115">Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich Express Edition) und ist auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)verfügbar.</span><span class="sxs-lookup"><span data-stu-id="bba91-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="bba91-116">Die AdventureWorks-Datenbank ist Teil der SQL Server 2005-Beispiele und-Beispiel Datenbanken (Download bei [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="bba91-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="bba91-117">Die einfachste Möglichkeit, die Datenbank einzurichten, ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und das Anfügen der `AdventureWorks.mdf` Datenbankdatei.</span><span class="sxs-lookup"><span data-stu-id="bba91-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="bba91-118">Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup.</span><span class="sxs-lookup"><span data-stu-id="bba91-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="bba91-119">Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.</span><span class="sxs-lookup"><span data-stu-id="bba91-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="bba91-120">Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):</span><span class="sxs-lookup"><span data-stu-id="bba91-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

<span data-ttu-id="bba91-121">Fügen Sie dann der Seite eine Datenquelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="bba91-121">Then, add a data source to the page.</span></span> <span data-ttu-id="bba91-122">Um eine begrenzte Menge von Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank aus.</span><span class="sxs-lookup"><span data-stu-id="bba91-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="bba91-123">Wenn Sie den Visual Studio-Assistenten verwenden, um die Datenquelle zu erstellen, beachten Sie, dass ein Fehler in der aktuellen Version dem Tabellennamen (`Vendor`) nicht mit `Purchasing`vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="bba91-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="bba91-124">Das folgende Markup zeigt die korrekte Syntax:</span><span class="sxs-lookup"><span data-stu-id="bba91-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

<span data-ttu-id="bba91-125">Merken Sie sich den Namen (ID) der Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="bba91-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="bba91-126">Diese sehr Identifikation muss dann in der `DataSourceID`-Eigenschaft des Accordion-Steuer Elements verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="bba91-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

<span data-ttu-id="bba91-127">Innerhalb des Accordion-Steuer Elements können Sie Vorlagen für verschiedene Teile des Steuer Elements bereitstellen, einschließlich des Headers (`<HeaderTemplate>`) und des Inhalts (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="bba91-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="bba91-128">Geben Sie in diesen Elementen einfach die Daten aus der Datenquelle mit der `DataBinder.Eval()`-Methode aus:</span><span class="sxs-lookup"><span data-stu-id="bba91-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

<span data-ttu-id="bba91-129">Wenn die Seite geladen wird, muss die Datenquelle mit diesem serverseitigen Code an das Akkordeon gebunden werden:</span><span class="sxs-lookup"><span data-stu-id="bba91-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

<span data-ttu-id="bba91-130">Um dieses Beispiel zu schließen, müssen Sie die beiden CSS-Klassen definieren, auf die im Steuerelement "Accordion" verwiesen wird (in den Eigenschaften `HeaderCssClass` und `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="bba91-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="bba91-131">Fügen Sie das folgende Markup in den `<head>` Abschnitt der Seite ein:</span><span class="sxs-lookup"><span data-stu-id="bba91-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]

<span data-ttu-id="bba91-132">[![die Daten im Akkordeon direkt aus der Datenquelle stammen.](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bba91-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span></span>

<span data-ttu-id="bba91-133">Die Daten im Akkordeon stammen direkt aus der Datenquelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](databinding-to-an-accordion-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="bba91-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bba91-134">[Zurück](dynamically-adding-an-accordion-pane-cs.md)
> [Weiter](dynamically-adding-an-accordion-pane-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bba91-134">[Previous](dynamically-adding-an-accordion-pane-cs.md)
[Next](dynamically-adding-an-accordion-pane-vb.md)</span></span>
