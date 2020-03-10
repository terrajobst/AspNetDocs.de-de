---
uid: whitepapers/aspnet-and-iis6
title: Ausführen von ASP.NET 1,1 mit IIS 6,0 | Microsoft-Dokumentation
author: rick-anderson
description: Obwohl Windows Server 2003 sowohl IIS 6,0 als auch ASP.NET 1,1 enthält, sind diese komponentenstandard mäßig deaktiviert. In diesem Whitepaper wird beschrieben, wie Sie IIS 6,0 aktivieren...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419847"
---
# <a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="f1d0a-104">Ausführen von ASP.NET 1.1 mit IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="f1d0a-104">Running ASP.NET 1.1 with IIS 6.0</span></span>

> <span data-ttu-id="f1d0a-105">Obwohl Windows Server 2003 sowohl IIS 6,0 als auch ASP.NET 1,1 enthält, sind diese komponentenstandard mäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="f1d0a-106">Dieses Whitepaper beschreibt die Aktivierung von IIS 6,0 und ASP.NET 1,1 und empfiehlt verschiedene Konfigurationseinstellungen, um die optimale Leistung von IIS und ASP.net zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="f1d0a-107">Gilt für ASP.NET 1,1 und IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>

<span data-ttu-id="f1d0a-108">ASP.NET 1,1 wird mit Windows Server 2003 ausgeliefert, das auch die neueste Version von Internet Information Server (IIS), Version 6,0, umfasst.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="f1d0a-109">IIS 6,0 und ASP.NET 1,1 sind so konzipiert, dass Sie nahtlos und ASP.net jetzt standardmäßig das neue IIS 6,0-workerprozessmodell verwendet.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="f1d0a-110">ASP.NET 1,1 wird nicht standardmäßig installiert.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="f1d0a-111">Im Gegensatz zu früheren Versionen der Server Betriebssysteme von Microsoft ist IIS (Internet Information Server) standardmäßig nicht aktiviert. und ist nicht ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="f1d0a-112">Es gibt zwei Optionen zum Aktivieren von IIS:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="f1d0a-113">Aktivieren von IIS, Option #1-Konfigurieren des Server-Assistenten</span><span class="sxs-lookup"><span data-stu-id="f1d0a-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="f1d0a-114">Windows Server 2003 enthält einen neuen Server "Server konfigurieren", mit dem Sie den Server im gewünschten Modus ordnungsgemäß konfigurieren können.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="f1d0a-115">Um den Assistenten zu starten, müssen Sie als Administrator angemeldet sein, um den Assistenten auszuführen: Programme | Verwaltungs Tools, und wählen Sie "Server konfigurieren" aus.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="f1d0a-116">Nachdem Sie die Option ausgewählt haben, wird der Startbildschirm des Assistenten zum Konfigurieren des Servers angezeigt:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="f1d0a-117">Klicken Sie auf "weiter &gt;":</span><span class="sxs-lookup"><span data-stu-id="f1d0a-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="f1d0a-118">Klicken Sie auf "weiter &gt;".</span><span class="sxs-lookup"><span data-stu-id="f1d0a-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="f1d0a-119">Auf diesem Bildschirm müssen Sie den Anwendungsserver (IIS, ASP.net) als Optionen für die Konfiguration auswählen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="f1d0a-120">Klicken Sie auf "weiter &gt;".</span><span class="sxs-lookup"><span data-stu-id="f1d0a-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="f1d0a-121">Nachdem Sie die Konfiguration des Servers als Anwendungsserver ausgewählt haben, wird dieser Bildschirm angezeigt, in dem Sie dazu aufgefordert werden, welche zusätzlichen Funktionen installiert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="f1d0a-122">Keine der beiden Optionen ist standardmäßig ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-122">Neither option is selected by default.</span></span> <span data-ttu-id="f1d0a-123">Um ASP.NET automatisch zu aktivieren, müssen Sie "ASP aktivieren" auswählen. NET '.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="f1d0a-124">Klicken Sie auf "weiter &gt;".</span><span class="sxs-lookup"><span data-stu-id="f1d0a-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="f1d0a-125">Auf diesem Bildschirm wird angezeigt, welche Optionen installiert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="f1d0a-126">Klicken Sie auf "weiter &gt;".</span><span class="sxs-lookup"><span data-stu-id="f1d0a-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="f1d0a-127">Dieser Bildschirm wird angezeigt, während die von Ihnen ausgewählten Optionen installiert werden.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="f1d0a-128">Es ist normal, dass andere Dialogfelder angezeigt werden, wenn Dienste installiert werden.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="f1d0a-129">Sie werden möglicherweise zusätzlich zur Eingabe des Speicher Orts der Windows 2003-Server-Installations-CD aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="f1d0a-130">Klicken Sie nach Abschluss des Vorgangs auf ' weiter &gt;'.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="f1d0a-131">Klicken Sie auf "Fertigstellen"-Windows Server 2003 ist nun für die Unterstützung von IIS 6,0 und ASP.NET 1,1 konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="f1d0a-132">Aktivieren von IIS, Option #2-Manuelles Konfigurieren von IIS und ASP.net</span><span class="sxs-lookup"><span data-stu-id="f1d0a-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="f1d0a-133">Wenn Sie den Serverkonfigurations-Assistenten nicht verwenden möchten, können Sie optional IIS 6,0 und ASP.NET 1,1 mithilfe von "Software" in der Systemsteuerung installieren.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="f1d0a-134">Öffnen Sie zunächst die Systemsteuerung:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="f1d0a-135">Klicken Sie anschließend auf "Windows-Komponenten hinzufügen/entfernen", um den Assistenten für Windows-Komponenten zu öffnen:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="f1d0a-136">Markieren Sie "Anwendungs Server", und klicken Sie darauf, und klicken Sie dann auf "Details?".</span><span class="sxs-lookup"><span data-stu-id="f1d0a-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="f1d0a-137">gedrückt</span><span class="sxs-lookup"><span data-stu-id="f1d0a-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="f1d0a-138">Um ASP.net zu installieren, aktivieren Sie "ASP. NET '.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="f1d0a-139">Klicken Sie auf "OK", um zum Assistenten für Windows-Komponenten zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="f1d0a-140">Klicken Sie im Assistenten für Windows-Komponenten auf "weiter &gt;", um die Installation zu starten:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="f1d0a-141">Es ist normal, dass andere Dialogfelder angezeigt werden, wenn Dienste installiert werden.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="f1d0a-142">Sie werden möglicherweise zusätzlich zur Eingabe des Speicher Orts der Windows 2003-Server-Installations-CD aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="f1d0a-143">Nach Abschluss der Installation wird der letzte Bildschirm des Assistenten für Windows-Komponenten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="f1d0a-144">IIS 6,0 und ASP.NET 1,1 sind nun konfiguriert und verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="f1d0a-145">Empfohlene Einstellungen</span><span class="sxs-lookup"><span data-stu-id="f1d0a-145">Recommended Settings</span></span>

<span data-ttu-id="f1d0a-146">Beim Ausführen von ASP.NET 1,1 mit IIS 6,0 gibt es mehrere Konfigurationseinstellungen, die empfohlen werden, um die optimale Leistung von ASP.net zu erzielen:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="f1d0a-147">Konfigurieren von Arbeitsspeicher Limits für Arbeitsprozesse</span><span class="sxs-lookup"><span data-stu-id="f1d0a-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="f1d0a-148">Konfigurieren der Wiederverwendung von Arbeitsprozessen</span><span class="sxs-lookup"><span data-stu-id="f1d0a-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="f1d0a-149">Konfigurieren von Arbeitsspeicher Limits für Arbeitsprozesse</span><span class="sxs-lookup"><span data-stu-id="f1d0a-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="f1d0a-150">Standardmäßig wird von IIS 6,0 keine Beschränkung für die Menge an Arbeitsspeicher festgelegt, die von IIS verwendet werden darf.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="f1d0a-151">ASP. Die Cache-Funktion des Netzes basiert auf einer Beschränkung des Arbeitsspeichers, sodass der Cache nicht verwendete Elemente proaktiv aus dem Arbeitsspeicher entfernen kann.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="f1d0a-152">Es wird empfohlen, dass Sie die Speicher Wiederverwendungs Funktion von IIS 6,0 konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="f1d0a-153">So konfigurieren Sie diesen geöffneten Internetinformationsdienste-Manager (Start | Programme | Verwaltungs Tools | Internetinformationsdienste).</span><span class="sxs-lookup"><span data-stu-id="f1d0a-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="f1d0a-154">Nachdem Sie geöffnet haben, erweitern Sie den Ordner "Anwendungs Pools":</span><span class="sxs-lookup"><span data-stu-id="f1d0a-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="f1d0a-155">Für jeden Anwendungs Pool:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="f1d0a-156">Klicken Sie mit der rechten Maustaste auf den Anwendungs Pool, z. b. "DefaultAppPool", und wählen Sie "Eigenschaften" aus:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="f1d0a-157">Aktivieren Sie als nächstes die Speicher Wiederverwendung, indem Sie entweder auf "Maximum used Memory (in Megabyte):" klicken.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="f1d0a-158">Der Wert darf nicht größer sein als der physische (nicht virtuelle) Arbeitsspeicher auf dem Server, eine gute Näherung ist 60% des physischen Speichers, d. h. für einen Server mit 512 MB physischem Arbeitsspeicher wählen Sie 310 aus.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="f1d0a-159">Es wird auch empfohlen, bei Verwendung eines 2-GB-Adressraums maximal 800mb zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="f1d0a-160">Wenn der Speicher Adressraum des Servers 3 GB beträgt, kann das maximale Arbeitsspeicher Limit für den Arbeitsprozess maximal 1, 800 MB betragen:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="f1d0a-161">Klicken Sie auf "übernehmen" und "OK", um das Eigenschaften Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="f1d0a-162">Wiederholen Sie diesen Schritt für alle verfügbaren Anwendungs Pools.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="f1d0a-163">Konfigurieren von workerwieder</span><span class="sxs-lookup"><span data-stu-id="f1d0a-163">Configuring worker recycling</span></span>

<span data-ttu-id="f1d0a-164">IIS 6,0 ist standardmäßig so konfiguriert, dass der Arbeitsprozess alle 29 Stunden wieder verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="f1d0a-165">Dies ist ein wenig aggressiv für eine Anwendung, die ASP.NET ausgeführt wird. es wird empfohlen, die automatische Wiederverwendung des Arbeitsprozesses zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="f1d0a-166">Zum Deaktivieren der automatischen Wiederverwendung des Arbeitsprozesses öffnen Sie zunächst Internetinformationsdienste-Manager (Start | Programme | Verwaltungs Tools | Internetinformationsdienste).</span><span class="sxs-lookup"><span data-stu-id="f1d0a-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="f1d0a-167">Nachdem Sie geöffnet haben, erweitern Sie den Ordner "Anwendungs Pools":</span><span class="sxs-lookup"><span data-stu-id="f1d0a-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="f1d0a-168">Für jeden Anwendungs Pool:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-168">For each application pool:</span></span>

1. <span data-ttu-id="f1d0a-169">Klicken Sie mit der rechten Maustaste auf den Anwendungs Pool, z. b. "DefaultAppPool", und wählen Sie "Eigenschaften" aus:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="f1d0a-170">Deaktivieren Sie die Option "Arbeitsprozess wieder verwenden (in Minuten):":</span><span class="sxs-lookup"><span data-stu-id="f1d0a-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="f1d0a-171">Klicken Sie auf "übernehmen" und "OK", um das Eigenschaften Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="f1d0a-172">Wiederholen Sie diesen Schritt für alle verfügbaren Anwendungs Pools.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="f1d0a-173">Gewähren von Schreibzugriff auf das Dateisystem</span><span class="sxs-lookup"><span data-stu-id="f1d0a-173">Granting write access to the file system</span></span>

<span data-ttu-id="f1d0a-174">Wenn Ihre Anwendung Schreibzugriff auf das Dateisystem benötigt und Sie NTFS verwenden, müssen Sie eine Access Control Liste (ACL) für den Ordner oder die Datei ändern, um ASP.net Zugriff zu gewähren.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="f1d0a-175">Wenn Sie z. b. dem ersten geöffneten Explorer "c:\inetpub\wwwroot" ASP.net Schreibzugriff gewähren möchten, navigieren Sie zu dem Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="f1d0a-176">Klicken Sie dann mit der rechten Maustaste auf das Verzeichnis, z. b. "wwwroot", und wählen Sie Eigenschaften aus.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="f1d0a-177">Wählen Sie nach dem Öffnen des Dialog Felds Eigenschaften die Registerkarte "Sicherheit" aus:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="f1d0a-178">Das Verzeichnis "c:\inetpub\wwwroot\" ist ein spezielles Verzeichnis, in dem die spezielle IIS 6,0-Gruppe "IIS\_WPG" bereits lesen &amp; ausführen, Ordner Inhalt auflisten und Leseberechtigungen gewährt wurde.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="f1d0a-179">Um jedoch Schreibberechtigungen zu erteilen, müssen Sie auf das Kontrollkästchen Zulassen für Write (schreiben) klicken:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="f1d0a-180">IIS 6,0 verfügt jetzt über die Schreib Berechtigung für diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="f1d0a-181">Gehen Sie folgendermaßen vor, um Schreibberechtigungen für andere Ordner zu erteilen: Beachten Sie, dass Sie möglicherweise die IIS-\_WPG-Gruppe hinzufügen müssen, falls diese nicht bereits vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="f1d0a-182">Das Erteilen von Schreibberechtigungen für IIS\_WPG ermöglicht allen ASP.NET-Anwendungen das Schreiben in dieses Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="f1d0a-183">Unterstützung der integrierten Authentifizierung mit SQL Server</span><span class="sxs-lookup"><span data-stu-id="f1d0a-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="f1d0a-184">Die integrierte Authentifizierung ermöglicht SQL Server die Windows NT-Authentifizierung zum Überprüfen von SQL Server Anmeldekonten zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="f1d0a-185">Dadurch kann der Benutzer den Standard-SQL Server Anmeldeprozess umgehen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="f1d0a-186">Bei diesem Ansatz kann ein Netzwerk Benutzer auf eine SQL Server Datenbank zugreifen, ohne eine separate Anmelde-ID oder ein Kennwort anzugeben, da SQL Server die Benutzer-und Kenn Wort Informationen aus dem Windows NT-Netzwerk Sicherheitsprozess abruft.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="f1d0a-187">Die Auswahl integrierter Authentifizierung für ASP.NET-Anwendungen ist eine gute Wahl, da in der Verbindungs Zeichenfolge für Ihre Anwendung niemals Anmelde Informationen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="f1d0a-188">Die Verbindungs Zeichenfolge, die zum Herstellen einer Verbindung mit SQL verwendet wird, sieht stattdessen wie folgt</span><span class="sxs-lookup"><span data-stu-id="f1d0a-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="f1d0a-189">Diese Verbindungs Zeichenfolge weist SQL Server an, die Windows-Anmelde Informationen der Anwendung zu verwenden, die auf SQL Server zuzugreifen versucht.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="f1d0a-190">Im Fall von ASP.NET/IIS 6 wäre dies ein Konto in der IIS-\_WPG-Gruppe.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="f1d0a-191">Um die integrierte Authentifizierung zwischen SQL Server und ASP.net zu aktivieren, müssen Sie zunächst sicherstellen, dass SQL Server entweder für die integrierte Authentifizierung oder die Authentifizierung im gemischten Modus konfiguriert ist. Überprüfen Sie hierzu den DBA, um dies zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="f1d0a-192">Wenn sich SQL Server in einem dieser beiden Modi befindet, können Sie die integrierte Authentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="f1d0a-193">Öffnen Sie SQL Server Enterprise-Manager (Start | Programme | Microsoft SQL Server | Enterprise Manager), wählen Sie den entsprechenden Server aus, und erweitern Sie den Ordner Sicherheit:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="f1d0a-194">Wenn die Gruppe "builtint\iis\_WPG ' nicht aufgeführt ist, klicken Sie mit der rechten Maustaste auf Anmeldungen, und wählen Sie" neu Login"aus:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="f1d0a-195">Geben Sie im Textfeld "Name:" entweder "[Server/Domänen Name] \IIS\_WPG" ein, oder klicken Sie auf die Schaltfläche mit den Auslassungs Punkten, um die Benutzer-/Gruppenauswahl von Windows NT zu öffnen:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="f1d0a-196">Wählen Sie die IIS-\_WPG-Gruppe des aktuellen Computers aus, und klicken Sie auf "hinzufügen", um die Auswahl zu schließen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="f1d0a-197">Anschließend müssen Sie auch die Standarddatenbank und die Berechtigungen für den Zugriff auf die Datenbank festlegen.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="f1d0a-198">Um die Standarddatenbank festzulegen, wählen Sie in der Dropdown Liste aus, z. b. unter Northwind ist ausgewählt:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="f1d0a-199">Klicken Sie anschließend auf die Registerkarte Datenbankzugriff:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="f1d0a-200">Klicken Sie auf das Kontrollkästchen Zulassen für jede Datenbank, für die Sie den Zugriff zulassen möchten.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="f1d0a-201">Sie müssen auch Daten bankrollen auswählen und DB überprüfen\_der Besitzer sicherstellt, dass Ihre Anmeldung über alle erforderlichen Berechtigungen zum Verwalten und Verwenden der ausgewählten Datenbank verfügt.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="f1d0a-202">Klicken Sie auf OK, um das Eigenschafts Dialogfeld</span><span class="sxs-lookup"><span data-stu-id="f1d0a-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="f1d0a-203">Ihre ASP.NET-Anwendung ist nun so konfiguriert, dass die integrierte SQL Server Authentifizierung unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="f1d0a-204">ASP.NET 1,0 nicht im einheitlichen Modus von IIS 6,0 ausführen</span><span class="sxs-lookup"><span data-stu-id="f1d0a-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="f1d0a-205">ASP.NET 1,0 unter IIS 6,0 wird nur im IIS 5-Kompatibilitätsmodus unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f1d0a-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="f1d0a-206">Öffnen Sie Internetdienste-Manager, und klicken Sie mit der rechten Maustaste auf Websites, und wählen Sie Eigenschaften aus, um ASP.NET 1,0 für die im IIS 5,0-Kompatibilitätsmodus</span><span class="sxs-lookup"><span data-stu-id="f1d0a-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="f1d0a-207">Wechseln Sie zur Registerkarte Dienst, und überprüfen Sie? WWW-Dienst im IIS 5,0-Isolations Modus ausführen?:</span><span class="sxs-lookup"><span data-stu-id="f1d0a-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
