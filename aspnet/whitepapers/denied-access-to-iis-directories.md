---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.net verweigerten Zugriff auf IIS-Verzeichnisse | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an Ihre ASP.NET-Anwendung den Fehler "der Zugriff auf das directname-Verzeichnis wurde verweigert" zurückgegeben wird. Fehler bei s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518571"
---
# <a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="97dda-104">ASP.NET – Verweigerung des Zugriffs auf IIS-Verzeichnisse</span><span class="sxs-lookup"><span data-stu-id="97dda-104">ASP.NET Denied Access to IIS Directories</span></span>

> <span data-ttu-id="97dda-105">In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an Ihre ASP.NET-Anwendung den Fehler "der Zugriff auf das *directname* -Verzeichnis wurde verweigert" zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="97dda-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="97dda-106">Fehler beim Starten der Überwachung von Verzeichnisänderungen. "</span><span class="sxs-lookup"><span data-stu-id="97dda-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="97dda-107">Gilt für ASP.NET 1,0 und ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="97dda-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="97dda-108">ASP.net V1 RTM wird nun mit einem Windows-Konto mit weniger Berechtigungen ausgeführt, das als "ASPNET"-Konto auf einem lokalen Computer registriert ist.</span><span class="sxs-lookup"><span data-stu-id="97dda-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="97dda-109">Bei einigen gesperrten Systemen verfügt dieses Konto möglicherweise nicht standardmäßig über Lese Sicherheits Zugriff auf die Inhaltsverzeichnisse einer Website, das Stammverzeichnis der Anwendung oder das Stammverzeichnis der Website.</span><span class="sxs-lookup"><span data-stu-id="97dda-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="97dda-110">In diesem Fall erhalten Sie den folgenden Fehler, wenn Sie Seiten von einer bestimmten Webanwendung anfordern:</span><span class="sxs-lookup"><span data-stu-id="97dda-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="97dda-111">Um dieses Problem zu beheben, müssen Sie die Sicherheits Berechtigungen für die entsprechenden Verzeichnisse ändern.</span><span class="sxs-lookup"><span data-stu-id="97dda-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="97dda-112">ASP.net erfordert insbesondere Lese-, Ausführungs-und Listen Zugriff für das ASPNET-Konto für das Website Stammverzeichnis (z. b. "c:\inetpub\wwwroot" oder ein beliebiges alternatives Website Verzeichnis, das Sie möglicherweise in IIS konfiguriert haben), das Inhaltsverzeichnis und das Stammverzeichnis der Anwendung. um Änderungen an Konfigurationsdateien zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="97dda-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="97dda-113">Der Anwendungs Stamm entspricht dem Ordner Pfad, der dem virtuellen Anwendungsverzeichnis im IIS-Verwaltungs Tool (inetmgr) zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="97dda-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="97dda-114">Sehen Sie sich beispielsweise die folgende Anwendungs Hierarchie unter dem Ordner "wwwroot" an.</span><span class="sxs-lookup"><span data-stu-id="97dda-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="97dda-115">In diesem Beispiel benötigt das ASPNET-Konto die oben definierten Leseberechtigungen für den Inhalt im Verzeichnis "MyApp" und im Verzeichnis "wwwroot".</span><span class="sxs-lookup"><span data-stu-id="97dda-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="97dda-116">Eine einzelne geerbte ACL für den Stamm Ordner kann optional auch für beide Verzeichnisse verwendet werden, wenn Sie sich in der Liste befinden.</span><span class="sxs-lookup"><span data-stu-id="97dda-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="97dda-117">Um Berechtigungen zu einem Verzeichnis hinzuzufügen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="97dda-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="97dda-118">Navigieren Sie im Windows-Explorer zum Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="97dda-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="97dda-119">Klicken Sie mit der rechten Maustaste auf den Verzeichnis Ordner, und wählen Sie "Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="97dda-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="97dda-120">Navigieren Sie im Eigenschaften Dialogfeld zur Registerkarte "Sicherheit".</span><span class="sxs-lookup"><span data-stu-id="97dda-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="97dda-121">Klicken Sie auf die Schaltfläche "hinzufügen", und geben Sie den Computernamen gefolgt vom Namen des ASPNET-Kontos ein.</span><span class="sxs-lookup"><span data-stu-id="97dda-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="97dda-122">Geben Sie beispielsweise auf einem Computer mit dem Namen "WebDev" webdev\aspnet ein, und drücken Sie "OK".</span><span class="sxs-lookup"><span data-stu-id="97dda-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="97dda-123">Stellen Sie sicher, dass das ASPNET-Konto die Kontrollkästchen "lesen &amp; ausführen", "Ordnerinhalte auflisten" und "lesen" aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="97dda-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="97dda-124">OK, um das Dialogfeld zu schließen und die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="97dda-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="97dda-125">Wenn gewünscht, können diese Änderungen mithilfe von Skripts oder dem Tool "Cacls. exe", das mit Windows ausgeliefert wird, automatisiert werden.</span><span class="sxs-lookup"><span data-stu-id="97dda-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="97dda-126">Weitere Informationen zum ASPNET-Konto finden Sie im [Dokument](https://go.microsoft.com/fwlink/?LinkId=5828)mit häufig gestellten Fragen.</span><span class="sxs-lookup"><span data-stu-id="97dda-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="97dda-127">Wenn eine bestimmte Webanwendung Schreib-oder Änderungs Berechtigungen für einen bestimmten Ordner oder eine bestimmte Datei hat, kann dies durch Befolgen desselben Verfahrens und Überprüfen der Kontrollkästchen "schreiben" und/oder "ändern" erfolgen.</span><span class="sxs-lookup"><span data-stu-id="97dda-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="97dda-128">Auf Computern, die allen Benutzern oder der Benutzergruppe Lesezugriff auf diese Verzeichnisse (Standardkonfiguration) gewähren, werden keine Probleme gefunden, und die oben aufgeführten Schritte sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="97dda-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
