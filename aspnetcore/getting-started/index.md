---
title: Erste Schritte mit ASP.NET Core
author: rick-anderson
description: 'Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird.'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="2b252-103">Tutorial: Erste Schritte mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b252-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="2b252-104">Dieses Tutorial zeigt, wie die .NET Core-Befehlszeilenschnittstelle verwendet wird, um eine ASP.NET Core-Web-App zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b252-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="2b252-105">Sie lernen, die folgende Aufgaben auszuführen:</span><span class="sxs-lookup"><span data-stu-id="2b252-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2b252-106">Erstellen Sie ein Web-App-Projekt.</span><span class="sxs-lookup"><span data-stu-id="2b252-106">Create a web app project.</span></span>
> * <span data-ttu-id="2b252-107">Aktivieren Sie lokales HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2b252-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="2b252-108">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="2b252-108">Run the app.</span></span>
> * <span data-ttu-id="2b252-109">Bearbeiten Sie eine Razor-Seite.</span><span class="sxs-lookup"><span data-stu-id="2b252-109">Edit a Razor page.</span></span>

<span data-ttu-id="2b252-110">Am Schluss werden Sie eine funktionierende Web-App auf Ihrem lokalen Computer besitzen.</span><span class="sxs-lookup"><span data-stu-id="2b252-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web-App-Startseite](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="2b252-112">Vorraussetzungen</span><span class="sxs-lookup"><span data-stu-id="2b252-112">Prerequisites</span></span>

* [<span data-ttu-id="2b252-113">.NET Core 2.2 SDK</span><span class="sxs-lookup"><span data-stu-id="2b252-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="2b252-114">Erstellen Sie ein Web-App-Projekt.</span><span class="sxs-lookup"><span data-stu-id="2b252-114">Create a web app project</span></span>

<span data-ttu-id="2b252-115">Öffnen Sie eine Befehlsshell, und geben Sie den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="2b252-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="2b252-116">Aktivieren Sie lokales HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2b252-116">Enable local HTTPS</span></span>

<span data-ttu-id="2b252-117">Vertrauen Sie dem HTTPS-Entwicklungszertifikat:</span><span class="sxs-lookup"><span data-stu-id="2b252-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2b252-118">Windows</span><span class="sxs-lookup"><span data-stu-id="2b252-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="2b252-119">Über den vorherigen Befehl wird der folgende Dialog angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2b252-119">The preceding command displays the following dialog:</span></span>

![Dialogfeld „Sicherheitswarnung“](~/getting-started/_static/cert.png)

<span data-ttu-id="2b252-121">Klicken Sie auf **Ja**, wenn Sie zustimmen möchten, dass das Entwicklungszertifikat vertrauenswürdig ist.</span><span class="sxs-lookup"><span data-stu-id="2b252-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="2b252-122">macOS</span><span class="sxs-lookup"><span data-stu-id="2b252-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="2b252-123">Über den vorherigen Befehl wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2b252-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="2b252-124">*Trusting the HTTPS development certificate was requested. Wenn das Zertifikat nicht bereits vertrauenswürdig, ist führen wir den folgenden Befehl aus:*  `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="2b252-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>
 
<span data-ttu-id="2b252-125">Dieser Befehl fordert Sie möglicherweise zur Eingabe Ihres Kennworts auf, um das Zertifikat für die Systemkeychain zu installieren.</span><span class="sxs-lookup"><span data-stu-id="2b252-125">This command command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="2b252-126">Geben Sie Ihr Kennwort ein, wenn Sie die Vertrauenswürdigkeit des Entwicklungszertifikats bestätigen möchten.</span><span class="sxs-lookup"><span data-stu-id="2b252-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2b252-127">Linux</span><span class="sxs-lookup"><span data-stu-id="2b252-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="2b252-128">Weitere Informationen zum Bestätigen der Vertrauenswürdigkeit eines HTTPS-Entwicklungszertifikats finden Sie in der Dokumentation zu Ihrer Linux-Distribution.</span><span class="sxs-lookup"><span data-stu-id="2b252-128">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="2b252-129">Weitere Informationen finden Sie unter [Festlegen des ASP.NET Core-HTTPS-Entwicklungszertifikat als vertrauenswürdig](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="2b252-129">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2b252-130">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="2b252-130">Run the app</span></span>

<span data-ttu-id="2b252-131">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="2b252-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="2b252-132">Nachdem die Befehlsshell angibt, dass die App gestartet wurde, wechseln Sie zu [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="2b252-132">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="2b252-133">Klicken Sie auf **Accept** (Akzeptieren), um die Datenschutz- und Cookierichtlinie zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="2b252-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="2b252-134">Diese App bewahrt keine personenbezogenen Informationen auf.</span><span class="sxs-lookup"><span data-stu-id="2b252-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="2b252-135">Bearbeiten einer Razor-Seite</span><span class="sxs-lookup"><span data-stu-id="2b252-135">Edit a Razor page</span></span>

<span data-ttu-id="2b252-136">Öffnen Sie *Pages/Index.cshtml*, und ändern Sie die Seite mit dem folgenden hervorgehobenen Markup:</span><span class="sxs-lookup"><span data-stu-id="2b252-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="2b252-137">Navigieren Sie zu [https://localhost:5001](https://localhost:5001), und überprüfen Sie, ob die Änderungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2b252-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b252-138">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="2b252-138">Next steps</span></span>

<span data-ttu-id="2b252-139">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="2b252-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2b252-140">Erstellen Sie ein Web-App-Projekt.</span><span class="sxs-lookup"><span data-stu-id="2b252-140">Create a web app project.</span></span>
> * <span data-ttu-id="2b252-141">Aktivieren Sie lokales HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2b252-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="2b252-142">Führen Sie das Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="2b252-142">Run the project.</span></span>
> * <span data-ttu-id="2b252-143">Führen Sie eine Änderung durch.</span><span class="sxs-lookup"><span data-stu-id="2b252-143">Make a change.</span></span>

<span data-ttu-id="2b252-144">Um mehr über ASP.NET Core zu lernen, machen Sie sich mit dem empfohlenen Lernpfad in der Einführung vertraut:</span><span class="sxs-lookup"><span data-stu-id="2b252-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
