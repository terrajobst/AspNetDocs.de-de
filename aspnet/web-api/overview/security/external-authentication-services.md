---
uid: web-api/overview/security/external-authentication-services
title: Externe Authentifizierungsdienste mit ASP.net-Web-API (C#) | Microsoft-Dokumentation
author: rmcmurray
description: Beschreibt die Verwendung externer Authentifizierungsdienste in ASP.net-Web-API.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447417"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="b40c4-103">Externe Authentifizierungsdienste mit ASP.net-Web-API (C#)</span><span class="sxs-lookup"><span data-stu-id="b40c4-103">External Authentication Services with ASP.NET Web API (C#)</span></span>

<span data-ttu-id="b40c4-104">Visual Studio 2017 und ASP.NET 4.7.2 erweitern Sie die Sicherheitsoptionen für [Single-Page-Anwendungen (Single Page Applications](../../../single-page-application/index.md) , Spa) und Web- [API](../../index.md) -Dienste für die Integration in externe Authentifizierungsdienste. dazu gehören verschiedene OAuth-/OpenID-und Social Media-Authentifizierungsdienste: Microsoft-Konten, Twitter, Facebook und Google.</span><span class="sxs-lookup"><span data-stu-id="b40c4-104">Visual Studio 2017 and ASP.NET 4.7.2 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>  

### <a name="in-this-walkthrough"></a><span data-ttu-id="b40c4-105">In dieser exemplarischen Vorgehensweise</span><span class="sxs-lookup"><span data-stu-id="b40c4-105">In this Walkthrough</span></span>

- [<span data-ttu-id="b40c4-106">Verwenden externer Authentifizierungsdienste</span><span class="sxs-lookup"><span data-stu-id="b40c4-106">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="b40c4-107">Erstellen der beispielweb Anwendung</span><span class="sxs-lookup"><span data-stu-id="b40c4-107">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="b40c4-108">Aktivieren der Facebook-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-108">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="b40c4-109">Aktivieren der Google-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-109">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="b40c4-110">Aktivieren der Microsoft-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-110">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="b40c4-111">Aktivieren der Twitter-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-111">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="b40c4-112">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="b40c4-112">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="b40c4-113">Kombinieren externer Authentifizierungsdienste</span><span class="sxs-lookup"><span data-stu-id="b40c4-113">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="b40c4-114">Konfigurieren von IIS Express für die Verwendung eines voll qualifizierten Domänen Namens</span><span class="sxs-lookup"><span data-stu-id="b40c4-114">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="b40c4-115">Abrufen der Anwendungseinstellungen für die Microsoft-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-115">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="b40c4-116">Optional: lokale Registrierung deaktivieren</span><span class="sxs-lookup"><span data-stu-id="b40c4-116">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="b40c4-117">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="b40c4-117">Prerequisites</span></span>

<span data-ttu-id="b40c4-118">Um die Beispiele in dieser exemplarischen Vorgehensweise befolgen zu können, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b40c4-118">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="b40c4-119">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b40c4-119">Visual Studio 2017</span></span>
- <span data-ttu-id="b40c4-120">Ein Entwicklerkonto mit dem Anwendungs Bezeichner und dem geheimen Schlüssel für einen der folgenden Social Media-Authentifizierungsdienste:</span><span class="sxs-lookup"><span data-stu-id="b40c4-120">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

  - <span data-ttu-id="b40c4-121">Microsoft-Konten ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="b40c4-121">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
  - <span data-ttu-id="b40c4-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="b40c4-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
  - <span data-ttu-id="b40c4-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="b40c4-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>
  - <span data-ttu-id="b40c4-124">Google ([https://developers.google.com/](https://developers.google.com))</span><span class="sxs-lookup"><span data-stu-id="b40c4-124">Google ([https://developers.google.com/](https://developers.google.com))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="b40c4-125">Verwenden externer Authentifizierungsdienste</span><span class="sxs-lookup"><span data-stu-id="b40c4-125">Using External Authentication Services</span></span>

<span data-ttu-id="b40c4-126">Die Fülle externer Authentifizierungsdienste, die derzeit für Webentwickler verfügbar sind, trägt dazu bei, die Entwicklungszeit beim Erstellen neuer Webanwendungen zu verkürzen.</span><span class="sxs-lookup"><span data-stu-id="b40c4-126">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="b40c4-127">Webbenutzer verfügen in der Regel über mehrere Konten für beliebte Webdienste und Social Media-Websites. Wenn also eine Webanwendung die Authentifizierungsdienste von einem externen Webdienst oder einer Website für soziale Medien implementiert, spart Sie die Entwicklungszeit, die hätte die Erstellung einer Authentifizierungs Implementierung aufgewendet.</span><span class="sxs-lookup"><span data-stu-id="b40c4-127">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="b40c4-128">Wenn Sie einen externen Authentifizierungsdienst verwenden, müssen die Endbenutzer kein weiteres Konto für Ihre Webanwendung erstellen, sondern auch einen anderen Benutzernamen und ein Kennwort speichern.</span><span class="sxs-lookup"><span data-stu-id="b40c4-128">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="b40c4-129">In der Vergangenheit hatten Entwickler zwei Möglichkeiten: Erstellen Sie Ihre eigene Authentifizierungs Implementierung, oder erfahren Sie, wie Sie einen externen Authentifizierungsdienst in Ihre Anwendungen integrieren.</span><span class="sxs-lookup"><span data-stu-id="b40c4-129">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="b40c4-130">Das folgende Diagramm veranschaulicht auf der grundlegendsten Ebene einen einfachen Anforderungs Fluss für einen Benutzer-Agent (Webbrowser), der Informationen aus einer Webanwendung anfordert, die für die Verwendung eines externen Authentifizierungs Diensts konfiguriert ist:</span><span class="sxs-lookup"><span data-stu-id="b40c4-130">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

<span data-ttu-id="b40c4-131">Im vorangehenden Diagramm führt der Benutzer-Agent (oder Webbrowser in diesem Beispiel) eine Anforderung an eine Webanwendung aus, die den Webbrowser an einen externen Authentifizierungsdienst umleitet.</span><span class="sxs-lookup"><span data-stu-id="b40c4-131">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="b40c4-132">Der Benutzer-Agent sendet seine Anmelde Informationen an den externen Authentifizierungsdienst, und wenn der Benutzer-Agent erfolgreich authentifiziert wurde, leitet der externe Authentifizierungsdienst den Benutzer-Agent zur ursprünglichen Webanwendung mit einer Form von Token weiter, die das der Benutzer-Agent sendet an die Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="b40c4-132">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="b40c4-133">Die Webanwendung verwendet das Token, um zu überprüfen, ob der Benutzer-Agent erfolgreich vom externen Authentifizierungsdienst authentifiziert wurde, und die Webanwendung kann das Token verwenden, um weitere Informationen über den Benutzer-Agent zu sammeln.</span><span class="sxs-lookup"><span data-stu-id="b40c4-133">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="b40c4-134">Nachdem die Anwendung die Informationen des Benutzer-Agents verarbeitet hat, gibt die Webanwendung die entsprechende Antwort an den Benutzer-Agent zurück, basierend auf den Autorisierungs Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="b40c4-134">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="b40c4-135">Im zweiten Beispiel wird der Benutzer-Agent mit der Webanwendung und dem externen autorisierungsserver verhandelt, und die Webanwendung führt zusätzliche Kommunikation mit dem externen autorisierungsserver durch, um zusätzliche Informationen über den Benutzer abzurufen. Büros</span><span class="sxs-lookup"><span data-stu-id="b40c4-135">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

<span data-ttu-id="b40c4-136">Visual Studio 2017 und ASP.NET 4.7.2 vereinfachen Sie die Integration mit externen Authentifizierungsdiensten für Entwickler, indem Sie integrierte Integration für die folgenden Authentifizierungsdienste bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="b40c4-136">Visual Studio 2017 and ASP.NET 4.7.2 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="b40c4-137">Facebook</span><span class="sxs-lookup"><span data-stu-id="b40c4-137">Facebook</span></span>
- <span data-ttu-id="b40c4-138">Google</span><span class="sxs-lookup"><span data-stu-id="b40c4-138">Google</span></span>
- <span data-ttu-id="b40c4-139">Microsoft-Konten (Windows Live ID-Konten)</span><span class="sxs-lookup"><span data-stu-id="b40c4-139">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="b40c4-140">Twitter</span><span class="sxs-lookup"><span data-stu-id="b40c4-140">Twitter</span></span>

<span data-ttu-id="b40c4-141">In den Beispielen in dieser exemplarischen Vorgehensweise wird veranschaulicht, wie die einzelnen unterstützten externen Authentifizierungsdienste mithilfe der neuen ASP.NET-Webanwendungs Vorlage konfiguriert werden, die mit Visual Studio 2017 ausgeliefert wird.</span><span class="sxs-lookup"><span data-stu-id="b40c4-141">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2017.</span></span>

> [!NOTE]
> <span data-ttu-id="b40c4-142">Gegebenenfalls müssen Sie Ihren FQDN den Einstellungen für den externen Authentifizierungsdienst hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b40c4-142">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="b40c4-143">Diese Anforderung basiert auf Sicherheitseinschränkungen für einige externe Authentifizierungsdienste, die erfordern, dass der FQDN in ihren Anwendungseinstellungen mit dem FQDN übereinstimmt, der von ihren Clients verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b40c4-143">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="b40c4-144">(Die Schritte hierfür sind für jeden externen Authentifizierungsdienst sehr unterschiedlich. Sie müssen sich die Dokumentation für jeden externen Authentifizierungsdienst ansehen, um festzustellen, ob dies erforderlich ist und wie diese Einstellungen konfiguriert werden.) Wenn Sie IIS Express für die Verwendung eines FQDN zum Testen dieser Umgebung konfigurieren müssen, lesen Sie den Abschnitt [Konfigurieren IIS Express, um einen voll qualifizierten Domänen Namen zu](#FQDN) verwenden, weiter unten in dieser exemplarischen Vorgehensweise.</span><span class="sxs-lookup"><span data-stu-id="b40c4-144">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a><span data-ttu-id="b40c4-145">Erstellen einer beispielweb Anwendung</span><span class="sxs-lookup"><span data-stu-id="b40c4-145">Create a Sample Web Application</span></span>

<span data-ttu-id="b40c4-146">Die folgenden Schritte führen Sie durch das Erstellen einer Beispielanwendung mithilfe der Vorlage ASP.NET-Webanwendung. diese Beispielanwendung wird später in dieser exemplarischen Vorgehensweise für jeden der externen Authentifizierungsdienste verwendet.</span><span class="sxs-lookup"><span data-stu-id="b40c4-146">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="b40c4-147">Starten Sie Visual Studio 2017, und wählen Sie auf der Start Seite die Option **Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="b40c4-147">Start Visual Studio 2017 and select **New Project** from the Start page.</span></span> <span data-ttu-id="b40c4-148">Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="b40c4-148">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

<span data-ttu-id="b40c4-149">Wenn das Dialogfeld **Neues Projekt** angezeigt wird, klicken Sie auf **installiert** , und erweitern Sie **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="b40c4-149">When the **New Project** dialog box is displayed, select **Installed** and expand **Visual C#**.</span></span> <span data-ttu-id="b40c4-150">Wählen Sie unter **Visualisierung C#** die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="b40c4-150">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="b40c4-151">Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET Webanwendung (.NET Framework)** aus.</span><span class="sxs-lookup"><span data-stu-id="b40c4-151">In the list of project templates, select **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="b40c4-152">Geben Sie einen Namen für das Projekt ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b40c4-152">Enter a name for your project and click **OK**.</span></span>

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

<span data-ttu-id="b40c4-153">Wenn das **neue ASP.net-Projekt** angezeigt wird, wählen Sie die Vorlage für die **Einzelseiten Anwendung** aus, und klicken Sie auf **Projekt erstellen**.</span><span class="sxs-lookup"><span data-stu-id="b40c4-153">When the **New ASP.NET Project** is displayed, select the **Single Page Application** template and click **Create Project**.</span></span>

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

<span data-ttu-id="b40c4-154">Warten Sie, bis Visual Studio 2017 Ihr Projekt erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="b40c4-154">Wait as Visual Studio 2017 creates your project.</span></span>

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

<span data-ttu-id="b40c4-155">Wenn Visual Studio 2017 die Erstellung des Projekts abgeschlossen hat, öffnen Sie die Datei *Startup.auth.cs* , die sich im Ordner **App\_Start** befindet.</span><span class="sxs-lookup"><span data-stu-id="b40c4-155">When Visual Studio 2017 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="b40c4-156">Wenn Sie das Projekt erstmalig erstellen, ist keiner der externen Authentifizierungsdienste in der *Startup.auth.cs* -Datei aktiviert; Im folgenden wird veranschaulicht, wie Ihr Code aussehen könnte, und in den Abschnitten, in denen Sie einen externen Authentifizierungsdienst und relevante Einstellungen aktivieren, um Microsoft-Konten, Twitter, Facebook oder die Google-Authentifizierung mit Ihrer ASP.NET-Anwendung zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="b40c4-156">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="b40c4-157">Wenn Sie F5 drücken, um Ihre Webanwendung zu erstellen und zu debuggen, wird ein Anmeldebildschirm angezeigt, auf dem Sie sehen, dass keine externen Authentifizierungsdienste definiert wurden.</span><span class="sxs-lookup"><span data-stu-id="b40c4-157">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

<span data-ttu-id="b40c4-158">In den folgenden Abschnitten erfahren Sie, wie Sie die einzelnen externen Authentifizierungsdienste aktivieren, die mit ASP.net in Visual Studio 2017 bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="b40c4-158">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2017.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="b40c4-159">Aktivieren der Facebook-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-159">Enabling Facebook authentication</span></span>

<span data-ttu-id="b40c4-160">Für die Verwendung der Facebook-Authentifizierung müssen Sie ein Facebook-Entwicklerkonto erstellen, und Ihr Projekt erfordert eine Anwendungs-ID und einen geheimen Schlüssel von Facebook, damit Sie funktioniert.</span><span class="sxs-lookup"><span data-stu-id="b40c4-160">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="b40c4-161">Weitere Informationen zum Erstellen eines Facebook-Entwickler Kontos und zum Abrufen der Anwendungs-ID und des geheimen Schlüssels finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="b40c4-161">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="b40c4-162">Nachdem Sie Ihre Anwendungs-ID und den geheimen Schlüssel erhalten haben, führen Sie die folgenden Schritte aus, um die Facebook-Authentifizierung für Ihre Webanwendung zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="b40c4-162">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="b40c4-163">Wenn das Projekt in Visual Studio 2017 geöffnet ist, öffnen Sie die Datei *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="b40c4-163">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="b40c4-164">Suchen Sie den Abschnitt Facebook-Authentifizierung im Code:</span><span class="sxs-lookup"><span data-stu-id="b40c4-164">Locate the Facebook authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="b40c4-165">Entfernen Sie die &quot;//&quot; Zeichen, um die Auskommentierung der markierten Codezeilen aufzuheben, und fügen Sie dann die Anwendungs-ID und den geheimen Schlüssel hinzu.</span><span class="sxs-lookup"><span data-stu-id="b40c4-165">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="b40c4-166">Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:</span><span class="sxs-lookup"><span data-stu-id="b40c4-166">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="b40c4-167">Wenn Sie F5 drücken, um die Webanwendung in Ihrem Webbrowser zu öffnen, sehen Sie, dass Facebook als externer Authentifizierungsdienst definiert wurde:</span><span class="sxs-lookup"><span data-stu-id="b40c4-167">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. <span data-ttu-id="b40c4-168">Wenn Sie auf die **Facebook** -Schaltfläche klicken, wird der Browser zur Facebook-Anmeldeseite umgeleitet:</span><span class="sxs-lookup"><span data-stu-id="b40c4-168">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. <span data-ttu-id="b40c4-169">Nachdem Sie Ihre Facebook-Anmelde Informationen eingegeben und auf **Anmelden**klicken, wird der Webbrowser zurück an Ihre Webanwendung umgeleitet, der Sie zur Eingabe des **Benutzernamens** auffordert, den Sie Ihrem Facebook-Konto zuordnen möchten:</span><span class="sxs-lookup"><span data-stu-id="b40c4-169">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. <span data-ttu-id="b40c4-170">Nachdem Sie Ihren Benutzernamen eingegeben und auf die Schaltfläche **registrieren** geklickt haben, zeigt Ihre Webanwendung die Standard **Startseite** für Ihr Facebook-Konto an:</span><span class="sxs-lookup"><span data-stu-id="b40c4-170">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="b40c4-171">Aktivieren der Google-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-171">Enabling Google Authentication</span></span>

<span data-ttu-id="b40c4-172">Wenn Sie die Google-Authentifizierung verwenden, müssen Sie ein Google Developer-Konto erstellen, und Ihr Projekt erfordert eine Anwendungs-ID und einen geheimen Schlüssel von Google, damit Sie funktioniert.</span><span class="sxs-lookup"><span data-stu-id="b40c4-172">Using Google authentication requires you to create a Google developer account, and your project will require an application ID and secret key from Google in order to function.</span></span> <span data-ttu-id="b40c4-173">Weitere Informationen zum Erstellen eines Google-Entwickler Kontos und zum Abrufen der Anwendungs-ID und des geheimen Schlüssels finden Sie unter [https://developers.google.com](https://developers.google.com).</span><span class="sxs-lookup"><span data-stu-id="b40c4-173">For information about creating a Google developer account and obtaining your application ID and secret key, see [https://developers.google.com](https://developers.google.com).</span></span>

<span data-ttu-id="b40c4-174">Um die Google-Authentifizierung für Ihre Webanwendung zu aktivieren, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="b40c4-174">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="b40c4-175">Wenn das Projekt in Visual Studio 2017 geöffnet ist, öffnen Sie die Datei *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="b40c4-175">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="b40c4-176">Suchen Sie den Abschnitt zur Google-Authentifizierung im Code:</span><span class="sxs-lookup"><span data-stu-id="b40c4-176">Locate the Google authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="b40c4-177">Entfernen Sie die &quot;//&quot; Zeichen, um die Auskommentierung der markierten Codezeilen aufzuheben, und fügen Sie dann die Anwendungs-ID und den geheimen Schlüssel hinzu.</span><span class="sxs-lookup"><span data-stu-id="b40c4-177">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="b40c4-178">Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:</span><span class="sxs-lookup"><span data-stu-id="b40c4-178">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="b40c4-179">Wenn Sie F5 drücken, um die Webanwendung in Ihrem Webbrowser zu öffnen, sehen Sie, dass Google als externer Authentifizierungsdienst definiert wurde:</span><span class="sxs-lookup"><span data-stu-id="b40c4-179">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. <span data-ttu-id="b40c4-180">Wenn Sie auf die **Google** -Schaltfläche klicken, wird der Browser an die Google-Anmeldeseite umgeleitet:</span><span class="sxs-lookup"><span data-stu-id="b40c4-180">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. <span data-ttu-id="b40c4-181">Nachdem Sie Ihre Google-Anmelde Informationen eingegeben und auf **Anmelden**klicken, werden Sie von Google aufgefordert, sicherzustellen, dass Ihre Webanwendung über Berechtigungen für den Zugriff auf Ihr Google-Konto verfügt:</span><span class="sxs-lookup"><span data-stu-id="b40c4-181">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. <span data-ttu-id="b40c4-182">Wenn Sie auf **annehmen**klicken, wird der Webbrowser zurück an Ihre Webanwendung umgeleitet, der Sie zur Eingabe des **Benutzernamens** auffordert, den Sie Ihrem Google-Konto zuordnen möchten:</span><span class="sxs-lookup"><span data-stu-id="b40c4-182">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. <span data-ttu-id="b40c4-183">Nachdem Sie Ihren Benutzernamen eingegeben und auf die Schaltfläche **registrieren** geklickt haben, zeigt Ihre Webanwendung die Standard **Startseite** für Ihr Google-Konto an:</span><span class="sxs-lookup"><span data-stu-id="b40c4-183">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="b40c4-184">Aktivieren der Microsoft-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-184">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="b40c4-185">Die Microsoft-Authentifizierung erfordert, dass Sie ein Entwicklerkonto erstellen, und erfordert eine Client-ID und einen geheimen Client Schlüssel, damit Sie funktionieren.</span><span class="sxs-lookup"><span data-stu-id="b40c4-185">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="b40c4-186">Weitere Informationen zum Erstellen eines Microsoft-Entwickler Kontos und zum Abrufen der Client-ID und des geheimen Client Schlüssels finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span><span class="sxs-lookup"><span data-stu-id="b40c4-186">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="b40c4-187">Nachdem Sie den consumerschlüssel und das Kunden Geheimnis erhalten haben, führen Sie die folgenden Schritte aus, um die Microsoft-Authentifizierung für Ihre Webanwendung zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="b40c4-187">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="b40c4-188">Wenn das Projekt in Visual Studio 2017 geöffnet ist, öffnen Sie die Datei *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="b40c4-188">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="b40c4-189">Suchen Sie den Microsoft-Authentifizierungs Abschnitt des Codes:</span><span class="sxs-lookup"><span data-stu-id="b40c4-189">Locate the Microsoft authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="b40c4-190">Entfernen Sie die &quot;//&quot; Zeichen, um die Auskommentierung der markierten Codezeilen aufzuheben, und fügen Sie dann die Client-ID und den geheimen Client Schlüssel hinzu.</span><span class="sxs-lookup"><span data-stu-id="b40c4-190">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="b40c4-191">Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:</span><span class="sxs-lookup"><span data-stu-id="b40c4-191">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="b40c4-192">Wenn Sie F5 drücken, um die Webanwendung in Ihrem Webbrowser zu öffnen, sehen Sie, dass Microsoft als externer Authentifizierungsdienst definiert wurde:</span><span class="sxs-lookup"><span data-stu-id="b40c4-192">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. <span data-ttu-id="b40c4-193">Wenn Sie auf die **Microsoft** -Schaltfläche klicken, wird der Browser auf die Microsoft-Anmeldeseite umgeleitet:</span><span class="sxs-lookup"><span data-stu-id="b40c4-193">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. <span data-ttu-id="b40c4-194">Nachdem Sie Ihre Microsoft-Anmelde Informationen eingegeben und auf **Anmelden**klicken, werden Sie aufgefordert, zu überprüfen, ob Ihre Webanwendung über Berechtigungen für den Zugriff auf Ihre Microsoft-Konto verfügt:</span><span class="sxs-lookup"><span data-stu-id="b40c4-194">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. <span data-ttu-id="b40c4-195">Wenn Sie auf **Ja**klicken, wird der Webbrowser zurück an Ihre Webanwendung umgeleitet, in der Sie aufgefordert werden, den **Benutzernamen** einzugeben, den Sie mit Ihrem Microsoft-Konto verknüpfen möchten:</span><span class="sxs-lookup"><span data-stu-id="b40c4-195">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. <span data-ttu-id="b40c4-196">Nachdem Sie Ihren Benutzernamen eingegeben und auf die Schaltfläche **registrieren** geklickt haben, zeigt Ihre Webanwendung die Standard **Startseite** für Ihre Microsoft-Konto an:</span><span class="sxs-lookup"><span data-stu-id="b40c4-196">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="b40c4-197">Aktivieren der Twitter-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-197">Enabling Twitter Authentication</span></span>

<span data-ttu-id="b40c4-198">Die Twitter-Authentifizierung erfordert, dass Sie ein Entwicklerkonto erstellen, und erfordert einen consumerschlüssel und einen geheimen Kundenschlüssel, damit Sie funktionieren.</span><span class="sxs-lookup"><span data-stu-id="b40c4-198">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="b40c4-199">Weitere Informationen zum Erstellen eines Twitter-Entwickler Kontos und zum Abrufen des consumerschlüssels und des geheimen Kunden Schlüssels finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="b40c4-199">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="b40c4-200">Nachdem Sie den consumerschlüssel und das Kunden Geheimnis erhalten haben, führen Sie die folgenden Schritte aus, um die Twitter-Authentifizierung für Ihre Webanwendung zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="b40c4-200">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="b40c4-201">Wenn das Projekt in Visual Studio 2017 geöffnet ist, öffnen Sie die Datei *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="b40c4-201">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="b40c4-202">Suchen Sie den Abschnitt Twitter-Authentifizierung im Code:</span><span class="sxs-lookup"><span data-stu-id="b40c4-202">Locate the Twitter authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="b40c4-203">Entfernen Sie die &quot;//&quot; Zeichen, um die Auskommentierung der markierten Codezeilen aufzuheben, und fügen Sie dann Ihren consumerschlüssel und Ihr consumergeheimnis hinzu.</span><span class="sxs-lookup"><span data-stu-id="b40c4-203">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="b40c4-204">Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:</span><span class="sxs-lookup"><span data-stu-id="b40c4-204">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="b40c4-205">Wenn Sie F5 drücken, um die Webanwendung in Ihrem Webbrowser zu öffnen, sehen Sie, dass Twitter als externer Authentifizierungsdienst definiert wurde:</span><span class="sxs-lookup"><span data-stu-id="b40c4-205">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. <span data-ttu-id="b40c4-206">Wenn Sie auf die Schaltfläche **Twitter** klicken, wird der Browser zur Twitter-Anmeldeseite umgeleitet:</span><span class="sxs-lookup"><span data-stu-id="b40c4-206">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. <span data-ttu-id="b40c4-207">Nachdem Sie Ihre Twitter-Anmelde Informationen eingegeben und auf " **App autorisieren**" klicken, wird der Webbrowser zurück an Ihre Webanwendung umgeleitet, der Sie zur Eingabe des **Benutzernamens** auffordert, den Sie Ihrem Twitter-Konto zuordnen möchten:</span><span class="sxs-lookup"><span data-stu-id="b40c4-207">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. <span data-ttu-id="b40c4-208">Nachdem Sie Ihren Benutzernamen eingegeben und auf die Schaltfläche **registrieren** geklickt haben, zeigt Ihre Webanwendung die Standard **Startseite** für Ihr Twitter-Konto an:</span><span class="sxs-lookup"><span data-stu-id="b40c4-208">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="b40c4-209">Zusätzliche Informationen</span><span class="sxs-lookup"><span data-stu-id="b40c4-209">Additional Information</span></span>

<span data-ttu-id="b40c4-210">Weitere Informationen zum Erstellen von Anwendungen, die OAuth und OpenID verwenden, finden Sie unter den folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="b40c4-210">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="b40c4-211">Kombinieren externer Authentifizierungsdienste</span><span class="sxs-lookup"><span data-stu-id="b40c4-211">Combining External Authentication Services</span></span>

<span data-ttu-id="b40c4-212">Um mehr Flexibilität zu erreichen, können Sie mehrere externe Authentifizierungsdienste gleichzeitig definieren. Dies ermöglicht es den Benutzern Ihrer Webanwendung, ein Konto von einem der aktivierten externen Authentifizierungsdienste zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="b40c4-212">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="b40c4-213">Konfigurieren von IIS Express für die Verwendung eines voll qualifizierten Domänen Namens</span><span class="sxs-lookup"><span data-stu-id="b40c4-213">Configure IIS Express to use a fully qualified domain name</span></span>

<span data-ttu-id="b40c4-214">Einige externe Authentifizierungs Anbieter unterstützen das Testen der Anwendung nicht, indem Sie eine HTTP-Adresse wie `http://localhost:port/`verwenden.</span><span class="sxs-lookup"><span data-stu-id="b40c4-214">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="b40c4-215">Um dieses Problem zu umgehen, können Sie der Hostdatei eine statische vollständig qualifizierte Domänen Namen Zuordnung (FQDN) hinzufügen und ihre Projektoptionen in Visual Studio 2017 so konfigurieren, dass der FQDN zum Testen/Debuggen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b40c4-215">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2017 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="b40c4-216">Führen Sie dazu die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="b40c4-216">To do so, use the following steps:</span></span>

- <span data-ttu-id="b40c4-217">Fügen Sie einen statischen voll qualifizierten Namen für die Hostdatei hinzu:</span><span class="sxs-lookup"><span data-stu-id="b40c4-217">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="b40c4-218">Öffnen Sie in Windows eine Eingabeaufforderung mit erhöhten Rechten.</span><span class="sxs-lookup"><span data-stu-id="b40c4-218">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="b40c4-219">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="b40c4-219">Type the following command:</span></span>

      <span data-ttu-id="b40c4-220"><kbd>Editor%windir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="b40c4-220"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="b40c4-221">Fügen Sie der Datei Hosts einen Eintrag wie den folgenden hinzu:</span><span class="sxs-lookup"><span data-stu-id="b40c4-221">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="b40c4-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="b40c4-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="b40c4-223">Speichern und schließen Sie die Hostdatei.</span><span class="sxs-lookup"><span data-stu-id="b40c4-223">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="b40c4-224">Konfigurieren Sie das Visual Studio-Projekt für die Verwendung des FQDN:</span><span class="sxs-lookup"><span data-stu-id="b40c4-224">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="b40c4-225">Wenn das Projekt in Visual Studio 2017 geöffnet ist, klicken Sie auf das Menü **Projekt** , und wählen Sie dann die Eigenschaften Ihres Projekts aus.</span><span class="sxs-lookup"><span data-stu-id="b40c4-225">When your project is open in Visual Studio 2017, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="b40c4-226">Sie können z. b. **WebApplication1-Eigenschaften**auswählen.</span><span class="sxs-lookup"><span data-stu-id="b40c4-226">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="b40c4-227">Wählen Sie die Registerkarte **Web** aus.</span><span class="sxs-lookup"><span data-stu-id="b40c4-227">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="b40c4-228">Geben Sie Ihren voll qualifizierten Namen für die <strong>Projekt-URL</strong>ein.</span><span class="sxs-lookup"><span data-stu-id="b40c4-228">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="b40c4-229">Beispielsweise können Sie <kbd><http://www.wingtiptoys.com></kbd> eingeben, wenn dies die FQDN-Zuordnung ist, die Sie der Hostdatei hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="b40c4-229">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="b40c4-230">Konfigurieren Sie IIS Express, um den FQDN für Ihre Anwendung zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="b40c4-230">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="b40c4-231">Öffnen Sie in Windows eine Eingabeaufforderung mit erhöhten Rechten.</span><span class="sxs-lookup"><span data-stu-id="b40c4-231">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="b40c4-232">Geben Sie den folgenden Befehl ein, um zum IIS Express Ordner zu wechseln:</span><span class="sxs-lookup"><span data-stu-id="b40c4-232">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="b40c4-233"><kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="b40c4-233"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="b40c4-234">Geben Sie den folgenden Befehl ein, um den FQDN Ihrer Anwendung hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="b40c4-234">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="b40c4-235"><kbd>Appcmd. exe set config-section: System. applicationHost/Sites/+&quot;[Name = ' WebApplication1 ']. Bindungen. [Protocol = ' http ', bindingInformation = ' \*: 80: www. wingtiptoys. com ']&quot;/Commit: Apphost</kbd></span><span class="sxs-lookup"><span data-stu-id="b40c4-235"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="b40c4-236">Dabei ist **WebApplication1** der Name Ihres Projekts, und **bindingInformation** enthält die Portnummer und den FQDN, die Sie für die Tests verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="b40c4-236">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="b40c4-237">Abrufen der Anwendungseinstellungen für die Microsoft-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b40c4-237">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="b40c4-238">Das Verknüpfen einer Anwendung mit Windows Live für die Microsoft-Authentifizierung ist ein einfacher Prozess.</span><span class="sxs-lookup"><span data-stu-id="b40c4-238">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="b40c4-239">Wenn Sie eine Anwendung nicht bereits mit Windows Live verknüpft haben, können Sie die folgenden Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="b40c4-239">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="b40c4-240">Navigieren Sie zu [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) , und geben Sie den Microsoft-Konto Namen und das Kennwort ein, **Wenn Sie dazu**aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="b40c4-240">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. <span data-ttu-id="b40c4-241">Wählen Sie **app hinzufügen** aus, geben Sie den Namen der Anwendung ein, wenn Sie dazu aufgefordert werden, und klicken Sie auf **Erstellen**:</span><span class="sxs-lookup"><span data-stu-id="b40c4-241">Select **Add an app** and enter the name of your application when prompted, and then click **Create**:</span></span>

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. <span data-ttu-id="b40c4-242">Wählen Sie Ihre APP unter **Name** aus, und die zugehörige Anwendungseigenschaften Seite wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b40c4-242">Select your app under **Name** and its application properties page appears.</span></span>

4. <span data-ttu-id="b40c4-243">Geben Sie die Umleitungs Domäne für die Anwendung ein.</span><span class="sxs-lookup"><span data-stu-id="b40c4-243">Enter the redirect domain for your application.</span></span> <span data-ttu-id="b40c4-244">Kopieren Sie die **Anwendungs-ID** , und klicken Sie unter **Anwendungs Geheimnisse**auf **Kennwort generieren**.</span><span class="sxs-lookup"><span data-stu-id="b40c4-244">Copy the **Application ID** and, under **Application Secrets**, select **Generate Password**.</span></span> <span data-ttu-id="b40c4-245">Kopieren Sie das angezeigte Kennwort.</span><span class="sxs-lookup"><span data-stu-id="b40c4-245">Copy the password that appears.</span></span> <span data-ttu-id="b40c4-246">Die Anwendungs-ID und das Kennwort sind Ihre Client-ID und Ihr geheimer Client Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="b40c4-246">The application ID and password are your client ID and client secret.</span></span> <span data-ttu-id="b40c4-247">Wählen Sie **OK** und dann **Speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="b40c4-247">Select **Ok** and then **Save**.</span></span>

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="b40c4-248">Optional: lokale Registrierung deaktivieren</span><span class="sxs-lookup"><span data-stu-id="b40c4-248">Optional: Disable Local Registration</span></span>

<span data-ttu-id="b40c4-249">Die aktuelle ASP.net lokale Registrierungsfunktion verhindert nicht, dass automatisierte Programme (Bots) Mitgliedskonten erstellen. beispielsweise mithilfe einer bot-Prävention und Validierungstechnologie wie [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span><span class="sxs-lookup"><span data-stu-id="b40c4-249">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="b40c4-250">Aus diesem Grund sollten Sie das lokale Anmeldeformular und den Registrierungs Link auf der Anmeldeseite entfernen.</span><span class="sxs-lookup"><span data-stu-id="b40c4-250">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="b40c4-251">Öffnen Sie hierzu die Seite " *\_Login. cshtml* " in Ihrem Projekt, und kommentieren Sie dann die Zeilen für den lokalen Anmeldebereich und den Registrierungs Link aus.</span><span class="sxs-lookup"><span data-stu-id="b40c4-251">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="b40c4-252">Die resultierende Seite sollte wie im folgenden Codebeispiel aussehen:</span><span class="sxs-lookup"><span data-stu-id="b40c4-252">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="b40c4-253">Nachdem der lokale Anmeldebereich und der Registrierungs Link deaktiviert wurden, werden auf der Anmeldeseite nur die externen Authentifizierungs Anbieter angezeigt, die Sie aktiviert haben:</span><span class="sxs-lookup"><span data-stu-id="b40c4-253">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
