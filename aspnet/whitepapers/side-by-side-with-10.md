---
uid: whitepapers/side-by-side-with-10
title: ASP.net parallele Ausführung von .NET Framework 1,0 und 1,1 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, wie Sie .NET 1,0 und .NET 1,1 auf Ihrem Computer installieren, sodass eine ASP.NET-Webanwendung unter beiden Versionen von Fram ausgeführt werden kann...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513831"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="bb4aa-103">ASP.NET – Parallele Ausführung von .NET Framework 1.0 und 1.1</span><span class="sxs-lookup"><span data-stu-id="bb4aa-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>

> <span data-ttu-id="bb4aa-104">In diesem Whitepaper wird beschrieben, wie Sie .NET 1,0 und .NET 1,1 auf Ihrem Computer installieren, sodass eine ASP.NET-Webanwendung in einer der beiden Versionen des Frameworks ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="bb4aa-105">Gilt für ASP.NET 1,0 und ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="bb4aa-106">In ASP.net werden Anwendungen so genannten, dass Sie parallel ausgeführt werden, wenn Sie auf demselben Computer installiert werden, aber unterschiedliche Versionen des .NET Framework verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="bb4aa-107">Im folgenden Thema wird beschrieben, wie ASP.NET-Anwendungen für die parallele Ausführung konfiguriert werden, und es werden ausführliche Schritte für Folgendes bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="bb4aa-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="bb4aa-108">Beibehalten der Zuordnung Ihrer Webanwendung zur .NET Framework Version 1,0 während der Installation</span><span class="sxs-lookup"><span data-stu-id="bb4aa-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="bb4aa-109">Ordnen Sie eine Webanwendung einer bestimmten Version des .NET Framework</span><span class="sxs-lookup"><span data-stu-id="bb4aa-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="bb4aa-110">Ermitteln der Version der .NET Framework, die von einer Website verwendet wird</span><span class="sxs-lookup"><span data-stu-id="bb4aa-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="bb4aa-111">Wenn eine Komponente oder eine Anwendung auf einem Computer aktualisiert wird, wird die ältere Version traditionell entfernt und durch die neuere Version ersetzt.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="bb4aa-112">Wenn die neue Version nicht mit der vorherigen Version kompatibel ist, unterbricht dies normalerweise andere Anwendungen, die die Komponente oder Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="bb4aa-113">Der .NET Framework bietet Unterstützung für die parallele Ausführung, sodass mehrere Versionen einer Assembly oder Anwendung gleichzeitig auf demselben Computer installiert werden können.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="bb4aa-114">Da mehrere Versionen gleichzeitig installiert werden können, können verwaltete Anwendungen auswählen, welche Version verwendet werden soll, ohne dass Anwendungen betroffen sind, die eine andere Version verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="bb4aa-115">Standardmäßig werden bei der Installation der .NET Framework Version 1,1 alle vorhandenen ASP.NET-Anwendungen automatisch neu konfiguriert, um die neueste Version der .NET Framework zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="bb4aa-116">Wenn Sie nicht möchten, dass Ihre ASP.NET-Anwendungen standardmäßig .NET Framework 1,1, klicken Sie [hier](#1) , um zu erfahren, wie Sie dies während der Installation verhindern können.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="bb4aa-117">Wenn Sie Ihren Webserver auf .NET Framework 1,1 aktualisieren und eine oder mehrere Webanwendungen .NET Framework 1,0 ausführen möchten, müssen Sie die Skript Zuordnung Internetinformationsdienste (IIS) aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="bb4aa-118">Die Skript Zuordnung ist der Mechanismus zum Zuordnen der ASPX-Dateierweiterung für eine bestimmte Webanwendung zu einer Version der .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="bb4aa-119">Klicken Sie [hier](#2) , um zu erfahren, wie eine Webanwendung einer bestimmten Version des .NET Framework zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="bb4aa-120">Mit dem Internet Information Manager oder dem ASP.NET IIS-Registrierungs Tool (ASPNET\_regiis. exe) können Sie feststellen, welche .NET Framework Version eine bestimmte Webanwendung ausgeführt hat.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="bb4aa-121">Klicken Sie [hier](#3) , um zu erfahren, wie Sie die Version der .NET Framework finden, die von einer Website verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="bb4aa-122">Bei der Migration zu .NET Framework 1,1 ist eine Überlegungen zum Import zu beachten, dass jede Version des .NET Framework eine eigene Datei "Machine. config" verwendet.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="bb4aa-123">Wenn ein Webadministrator Änderungen an der Datei "Machine. config" vorgenommen hat, müssen diese Änderungen daher in die Datei ".NET Framework 1,1 Machine. config" migriert werden.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="bb4aa-124">Beibehalten der Zuordnung Ihrer Webanwendung zu .NET Framework 1,0 während der Installation</span><span class="sxs-lookup"><span data-stu-id="bb4aa-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="bb4aa-125">Standardmäßig werden alle vorhandenen ASP.NET-Anwendungen während der Installation automatisch neu konfiguriert, um die neuere Version des .NET Framework zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="bb4aa-126">Mithilfe der neueren Version des .NET Framework können Anwendungen die Verbesserungen und neuen Features in der neuen Version in vollem Umfang nutzen.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="bb4aa-127">Gleichzeitig kann der Webadministrator, der die genaue Kontrolle über die aktualisierten Anwendungen hätte, die automatische Neuzuordnung aller vorhandenen ASP.NET-Anwendungen während der Installation des .NET Framework verhindern.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="bb4aa-128">Um die automatische Neuzuordnung der gesamten ASP.NET-Anwendung zur neueren Version des .NET Framework zu verhindern, kann der Webadministrator die Befehlszeilenoption/noaspupgrade mit dem Setup Programm Dotnetfx. exe verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="bb4aa-129">**So verhindern Sie die Neuzuordnung der ASP.NET-Anwendung zu einer neueren Version**</span><span class="sxs-lookup"><span data-stu-id="bb4aa-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="bb4aa-130">Navigieren Sie zu **Start**.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-130">Go to **Start**.</span></span>
2. <span data-ttu-id="bb4aa-131">Klicken Sie auf **Ausführen**.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-131">Click on **run**.</span></span>
3. <span data-ttu-id="bb4aa-132">Geben Sie **cmd** ein.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-132">Type **cmd**.</span></span>
4. <span data-ttu-id="bb4aa-133">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="bb4aa-134">Geben Sie an der Eingabeaufforderung die folgende Zeile ein, um die Installation des .NET Framework zu starten: **Dotnetfx. exe/c: "Install/noaspupgrade?** .</span><span class="sxs-lookup"><span data-stu-id="bb4aa-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="bb4aa-135">Klicken Sie im Microsoft .NET Framework 1,1-Setup auf **Ja** .</span><span class="sxs-lookup"><span data-stu-id="bb4aa-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="bb4aa-136">Dadurch wird der Setup Vorgang des .NET Framework 1,1 gestartet.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="bb4aa-137">Ordnen Sie eine Webanwendung einer bestimmten Version des .NET Framework</span><span class="sxs-lookup"><span data-stu-id="bb4aa-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="bb4aa-138">Jede Version der .NET Framework enthält eine Version des ASP.NET IIS-Registrierungs Tools (ASPNET\_regiis. exe).</span><span class="sxs-lookup"><span data-stu-id="bb4aa-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="bb4aa-139">Mit diesem Tool können Administratoren angeben, dass eine Webanwendung unter einer bestimmten Version des .NET Framework ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="bb4aa-140">Dies wird als Zuordnung einer Webanwendung zu einer Version der .NET Framework bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="bb4aa-141">Administratoren müssen die ASPNET-\_regiis. exe auswählen, die der Version des .NET Framework entspricht, der der Webanwendung zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="bb4aa-142">Ein Administrator, der beispielsweise angeben möchte, dass eine Website .NET Framework 1,1 verwendet, muss die ASPNET-\_regiis. exe verwenden, die mit .NET Framework 1,1 ausgestattet ist.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="bb4aa-143">Die ASPNET-\_regiis. exe für Version 1,0 befindet sich unter:</span><span class="sxs-lookup"><span data-stu-id="bb4aa-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="bb4aa-144">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="bb4aa-144">C:\WINDOWS\Microsoft.NET\Framework\\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="bb4aa-145">Die ASPNET-\_regiis. exe für Version 1, 1 befindet sich unter:</span><span class="sxs-lookup"><span data-stu-id="bb4aa-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="bb4aa-146">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="bb4aa-146">C:\WINDOWS\Microsoft.NET\Framework\\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="bb4aa-147">Die ASPNET-\_regiis. exe bietet zwei Optionen für die Skript Zuordnung für eine Webanwendung:</span><span class="sxs-lookup"><span data-stu-id="bb4aa-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="bb4aa-148">**-s** legt die Skript Zuordnung im Pfad und in den zugehörigen untergeordneten Verzeichnissen fest.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="bb4aa-149">**-SN** legt die Skript Zuordnung nur im Pfad fest.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="bb4aa-150">Der Pfad definiert den IIS-Metadatenpfad der Webanwendung, der in Form von W3SVC/root/{websitenreber}/{Application\_Name} definiert ist.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="bb4aa-151">Für eine Webanwendung namens "Portal" unter der Standard Website lautet der Metabasispfad beispielsweise W3SVC/1/root/Portal.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="bb4aa-152">Hinweis Sie können auch ein Tool mit dem Namen Metabase Editor verwenden, um den Metabasispfad zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="bb4aa-153">Sie können dieses Tool von der Microsoft-Support-Website unter [https://support.microsoft.com/default.aspx?scid=kb; en-US; 232068](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068) herunterladen.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="bb4aa-154">Führen Sie ASPNET\_regiis. exe-s W3SVC/1/root/Portal aus, um die Portal-IIS-Skript Map und deren unter Anwendung zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="bb4aa-155">Führen Sie ASPNET\_regiis. exe-SN W3SVC/1/root/Portal aus, um die IIS-Skript Zuordnung des Portals zu aktualisieren, ohne dass sich dies auf die Anwendungen in den Unterverzeichnissen des Portals auswirkt.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="bb4aa-156">Suchen der .NET Framework Version, die eine Webanwendung verwendet</span><span class="sxs-lookup"><span data-stu-id="bb4aa-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="bb4aa-157">Ein Administrator kann den Internet Service Manager verwenden, um zu ermitteln, welche Version des .NET Framework eine Website ausführt.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="bb4aa-158">Unterschiedliche Betriebssystemversionen starten das Internet Service Manager anders.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="bb4aa-159">Führen Sie die unten aufgeführten Schritte aus, um den Dienst-Manager zu starten.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="bb4aa-160">**So starten Sie Internet Service Manager**</span><span class="sxs-lookup"><span data-stu-id="bb4aa-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="bb4aa-161">Navigieren Sie zu **Start**.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-161">Go to **Start**.</span></span>
2. <span data-ttu-id="bb4aa-162">Klicken Sie auf **Ausführen**.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-162">Click on **run**.</span></span>
3. <span data-ttu-id="bb4aa-163">Geben Sie **inetmgr**ein.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="bb4aa-164">Wählen Sie im Internet Service Manager die Webanwendung aus, deren Version der .NET Framework Sie wissen möchten.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="bb4aa-165">Klicken Sie mit der rechten Maustaste auf die Webanwendung, und klicken Sie auf **Eigenschaften.**</span><span class="sxs-lookup"><span data-stu-id="bb4aa-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="bb4aa-166">Wählen Sie im Eigenschaften Fenster die Option **Konfiguration aus.**</span><span class="sxs-lookup"><span data-stu-id="bb4aa-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="bb4aa-167">Wählen Sie in der Tabelle Anwendungs Zuordnung die Option **. aspx**aus, und klicken Sie auf **Bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="bb4aa-168">Überprüfen Sie im Textfeld **ausführbare** Datei das Versions Verzeichnis, indem Sie einen Bildlauf ausführen.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="bb4aa-169">Wenn das Versions Verzeichnis v. 1.1.4322 ist, wird die Anwendung .NET Framework 1,1 zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="bb4aa-170">Wenn das Versions Verzeichnis also v 1.0.3705 ist, wird die Anwendung .NET Framework 1,0 zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="bb4aa-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
