---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Erstellen einer Secure ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-BestätigungC#und Kenn Wort Zurücksetzung () | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Web-App mit e-Mail-Bestätigung und Kenn Wort Zurücksetzung mithilfe des ASP.net Identity Mitgliedschafts Systems erstellen Ihre Zertifizierungsstelle...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 6169c972ad0f4ee2079d3638c54a5accc4b8b3de
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519348"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="c1d5f-104">Erstellen einer sicheren ASP.NET MVC 5-Web-App mit Anmeldung, E-Mail-Bestätigung und Kennwortzurücksetzung (C#)</span><span class="sxs-lookup"><span data-stu-id="c1d5f-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>

<span data-ttu-id="c1d5f-105">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c1d5f-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c1d5f-106">In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Web-App mit e-Mail-Bestätigung und Kenn Wort Zurücksetzung mithilfe des ASP.net Identity Mitgliedschafts Systems erstellen</span><span class="sxs-lookup"><span data-stu-id="c1d5f-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span>

<span data-ttu-id="c1d5f-107">Eine aktualisierte Version dieses Tutorials, das .net Core verwendet, finden Sie [Unterkonto Bestätigung und Kenn Wort Wiederherstellung in ASP.net Core](/aspnet/core/security/authentication/accconfirm).</span><span class="sxs-lookup"><span data-stu-id="c1d5f-107">For an updated version of this tutorial that uses .NET Core, see [Account confirmation and password recovery in ASP.NET Core](/aspnet/core/security/authentication/accconfirm).</span></span>

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="c1d5f-108">Erstellen einer ASP.NET-MVC-App</span><span class="sxs-lookup"><span data-stu-id="c1d5f-108">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="c1d5f-109">Beginnen Sie mit der Installation und Ausführung von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="c1d5f-109">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="c1d5f-110">Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-110">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="c1d5f-111">Warnung: Sie müssen [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher installieren, um dieses Tutorial abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-111">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="c1d5f-112">Erstellen Sie ein neues ASP.NET-Webprojekt, und wählen Sie die MVC-Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-112">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="c1d5f-113">Web Forms auch ASP.net Identity unterstützt, können Sie ähnliche Schritte in einer Web Forms-app ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-113">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="c1d5f-114">Überlassen Sie die Standard Authentifizierung als **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-114">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="c1d5f-115">Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-115">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="c1d5f-116">Später in diesem Tutorial wird die Bereitstellung in Azure durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-116">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="c1d5f-117">Sie können [ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="c1d5f-117">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="c1d5f-118">Legen Sie fest, dass das [Projekt SSL verwendet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="c1d5f-118">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="c1d5f-119">Führen Sie die APP aus, klicken Sie auf den Link **registrieren** , und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-119">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="c1d5f-120">An diesem Punkt wird nur die e-Mail-Überprüfung mit dem [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) -Attribut durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-120">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="c1d5f-121">Navigieren Sie in Server-Explorer zu **Daten connections\defaultconnection\tables\aspnettusers**, klicken Sie mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**aus.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-121">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="c1d5f-122">Die folgende Abbildung zeigt das `AspNetUsers` Schema:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-122">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="c1d5f-123">Klicken Sie mit der rechten Maustaste auf die Tabelle **aspnettusers** , und wählen Sie **Tabellendaten anzeigen**aus.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-123">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="c1d5f-124">An diesem Punkt wurde die e-Mail nicht bestätigt.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-124">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="c1d5f-125">Klicken Sie auf die Zeile, und wählen Sie löschen aus.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-125">Click on the row and select delete.</span></span> <span data-ttu-id="c1d5f-126">Sie fügen diese e-Mail im nächsten Schritt erneut hinzu und senden eine Bestätigungs-e-Mail.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-126">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="c1d5f-127">Bestätigung per e-Mail</span><span class="sxs-lookup"><span data-stu-id="c1d5f-127">Email confirmation</span></span>

<span data-ttu-id="c1d5f-128">Es wird empfohlen, die e-Mail-Adresse einer neuen Benutzerregistrierung zu bestätigen, um sicherzustellen, dass Sie nicht von einer anderen Person angenommen werden (d. h., Sie haben sich nicht bei der e-Mail einer anderen Person registriert).</span><span class="sxs-lookup"><span data-stu-id="c1d5f-128">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="c1d5f-129">Angenommen, Sie hatten ein Diskussionsforum, Sie sollten verhindern, dass sich `"bob@example.com"` als `"joe@contoso.com"`registrieren.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-129">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="c1d5f-130">Ohne e-Mail-Bestätigung können `"joe@contoso.com"` unerwünschte e-Mails von Ihrer APP erhalten.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-130">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="c1d5f-131">Wenn Bob versehentlich als `"bib@example.com"` registriert ist und es nicht bemerkt hat, wäre er nicht in der Lage, die Kenn Wort Wiederherstellung zu verwenden, da die APP nicht über die richtige e-Mail verfügt.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-131">Suppose Bob accidentally registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="c1d5f-132">E-Mail-Bestätigung bietet nur eingeschränkten Schutz vor Bots und bietet keinen Schutz vor ermittelten Spammern. Sie verfügen über viele funktionierende e-Mail-Aliase, die Sie zum Registrieren verwenden können.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-132">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="c1d5f-133">In der Regel möchten Sie, dass neue Benutzerdaten auf Ihrer Website veröffentlichen, bevor Sie per e-Mail, SMS-SMS oder einem anderen Mechanismus bestätigt wurden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-133">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="c1d5f-134">In den folgenden Abschnitten aktivieren wir die e-Mail-Bestätigung und ändern den Code, um zu verhindern, dass sich neu registrierte Benutzer anmelden, bis Ihre e-Mail-Nachricht bestätigt wird.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-134">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="c1d5f-135">Sendgrid anschließen</span><span class="sxs-lookup"><span data-stu-id="c1d5f-135">Hook up SendGrid</span></span>

<span data-ttu-id="c1d5f-136">Die Anweisungen in diesem Abschnitt sind nicht aktuell.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-136">The instructions in this section are not current.</span></span> <span data-ttu-id="c1d5f-137">Aktualisierte Anweisungen finden Sie unter [Konfigurieren eines sendgrid-e-Mail-Anbieters](/aspnet/core/security/authentication/accconfirm#configure-email-provider)</span><span class="sxs-lookup"><span data-stu-id="c1d5f-137">See [Configure SendGrid email provider](/aspnet/core/security/authentication/accconfirm#configure-email-provider) for updated instructions.</span></span>

<span data-ttu-id="c1d5f-138">Obwohl in diesem Tutorial nur das Hinzufügen von e-Mail-Benachrichtigungen über [sendgrid](http://sendgrid.com/)veranschaulicht wird, können Sie e-Mails mithilfe von SMTP und anderen Mechanismen (siehe [zusätzliche Ressourcen](#addRes)) senden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="c1d5f-139">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="c1d5f-140">Wechseln Sie zur [Azure sendgrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) -Registrierungsseite, und registrieren Sie sich für ein kostenloses sendgrid-Konto.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="c1d5f-141">Konfigurieren Sie sendgrid, indem Sie Code wie den folgenden in *App_Start/identityconfig.cs*hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="c1d5f-142">Sie müssen Folgendes hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="c1d5f-143">Um dieses Beispiel einfach zu halten, speichern wir die App-Einstellungen in der Datei " *Web. config* ":</span><span class="sxs-lookup"><span data-stu-id="c1d5f-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="c1d5f-144">Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="c1d5f-145">Das Konto und die Anmelde Informationen werden in der appSetting gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="c1d5f-146">In Azure können diese Werte auf der Registerkarte **[Konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** des Azure-Portal sicher gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="c1d5f-147">Weitere Informationen finden [Sie unter Bewährte Methoden für die Bereitstellung von Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)</span><span class="sxs-lookup"><span data-stu-id="c1d5f-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>

### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="c1d5f-148">E-Mail-Bestätigung im Konto Controller aktivieren</span><span class="sxs-lookup"><span data-stu-id="c1d5f-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="c1d5f-149">Vergewissern Sie sich, dass die Datei " *views\account\confirmemail.cshtml* " eine korrekte Razor-Syntax aufweist.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="c1d5f-150">(Das @-Zeichen in der ersten Zeile ist möglicherweise nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="c1d5f-151">)</span><span class="sxs-lookup"><span data-stu-id="c1d5f-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="c1d5f-152">Führen Sie die APP aus, und klicken Sie auf den Link registrieren.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-152">Run the app and click the Register link.</span></span> <span data-ttu-id="c1d5f-153">Nachdem Sie das Registrierungsformular eingereicht haben, sind Sie angemeldet.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="c1d5f-154">Überprüfen Sie Ihr e-Mail-Konto, und klicken Sie auf den Link, um Ihre e-Mail</span><span class="sxs-lookup"><span data-stu-id="c1d5f-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="c1d5f-155">E-Mail-Bestätigung vor der Anmeldung erforderlich</span><span class="sxs-lookup"><span data-stu-id="c1d5f-155">Require email confirmation before log in</span></span>

<span data-ttu-id="c1d5f-156">Wenn ein Benutzer das Registrierungsformular abschließt, werden Sie zurzeit angemeldet.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="c1d5f-157">Sie möchten Ihre e-Mail in der Regel bestätigen, bevor Sie Sie in protokollieren.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="c1d5f-158">Im folgenden Abschnitt ändern wir den Code so, dass neue Benutzer eine bestätigte e-Mail-Nachricht erhalten müssen, bevor Sie angemeldet (authentifiziert) werden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="c1d5f-159">Aktualisieren Sie die `HttpPost Register`-Methode mit den folgenden hervorgehobenen Änderungen:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="c1d5f-160">Wenn Sie die `SignInAsync`-Methode auskommentieren, wird der Benutzer nicht durch die Registrierung angemeldet.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="c1d5f-161">Die `TempData["ViewBagLink"] = callbackUrl;` Zeile kann verwendet werden, um [die APP zu Debuggen und die](#dbg) Registrierung ohne e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="c1d5f-162">`ViewBag.Message` wird verwendet, um die Bestätigungs Anweisungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="c1d5f-163">Das [Beispiel zum Herunterladen](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) enthält Code zum Testen der e-Mail-Bestätigung ohne e-Mail-Einrichtung und kann auch zum Debuggen der Anwendung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="c1d5f-164">Erstellen Sie eine `Views\Shared\Info.cshtml` Datei, und fügen Sie das folgende Razor-Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="c1d5f-165">Fügen Sie das [Attribut autorisieren](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) der `Contact` Aktionsmethode des Home-Controllers hinzu.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="c1d5f-166">Sie können auf den **Kontakt** Link klicken, um zu überprüfen, ob anonyme Benutzer keinen Zugriff haben und authentifizierte Benutzer Zugriff haben.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="c1d5f-167">Sie müssen auch die `HttpPost Login` Aktionsmethode aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="c1d5f-168">Aktualisieren Sie die Ansicht *views\shared\error.cshtml* , um die Fehlermeldung anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="c1d5f-169">Löschen Sie alle Konten in der Tabelle " **aspnettusers** ", die den zu testenden e-Mail-Alias enthalten.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="c1d5f-170">Führen Sie die APP aus, und vergewissern Sie sich, dass Sie sich erst anmelden können, wenn Sie Ihre e-Mail</span><span class="sxs-lookup"><span data-stu-id="c1d5f-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="c1d5f-171">Wenn Sie Ihre e-Mail-Adresse bestätigt haben, klicken Sie auf den Link **Kontakt** .</span><span class="sxs-lookup"><span data-stu-id="c1d5f-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="c1d5f-172">Kenn Wort Wiederherstellung/zurück Setzung</span><span class="sxs-lookup"><span data-stu-id="c1d5f-172">Password recovery/reset</span></span>

<span data-ttu-id="c1d5f-173">Entfernen Sie die Kommentarzeichen aus der `HttpPost ForgotPassword` Aktionsmethode im Konto Controller:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="c1d5f-174">Entfernen Sie die Kommentarzeichen aus dem `ForgotPassword` [Action Link](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in der Razor-Ansichts Datei " *views\account\login.cshtml* ":</span><span class="sxs-lookup"><span data-stu-id="c1d5f-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="c1d5f-175">Auf der Anmeldeseite wird jetzt ein Link zum Zurücksetzen des Kennworts angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="c1d5f-176">Link zum erneuten Senden von e-Mail</span><span class="sxs-lookup"><span data-stu-id="c1d5f-176">Resend email confirmation link</span></span>

<span data-ttu-id="c1d5f-177">Nachdem ein Benutzer ein neues lokales Konto erstellt hat, sendet er einen Bestätigungslink per e-Mail, bevor er sich anmelden kann.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="c1d5f-178">Wenn der Benutzer die Bestätigungs-e-Mail versehentlich löscht oder die e-Mail nie eintrifft, muss der Bestätigungslink erneut gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-178">If the user accidentally deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="c1d5f-179">Die folgenden Codeänderungen zeigen, wie Sie diese aktivieren.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="c1d5f-180">Fügen Sie am Ende der Datei " *controllers\accountcontroller.cs* " die folgende Hilfsmethode hinzu:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="c1d5f-181">Aktualisieren Sie die Register-Methode, um das neue Hilfsprogramm zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="c1d5f-182">Aktualisieren Sie die Anmelde Methode, um das Kennwort erneut zu senden, wenn das Benutzerkonto nicht bestätigt wurde:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c1d5f-183">Kombinieren von sozialen und lokalen Anmeldekonten</span><span class="sxs-lookup"><span data-stu-id="c1d5f-183">Combine social and local login accounts</span></span>

<span data-ttu-id="c1d5f-184">Sie können lokale und soziale Konten kombinieren, indem Sie auf Ihren e-Mail-Link klicken.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c1d5f-185">In der folgenden Reihenfolge **RickAndMSFT@gmail.com** zuerst als lokale Anmeldung erstellt, aber Sie können das Konto zuerst als ein soziales Protokoll erstellen und dann einen lokalen Anmelde Namen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="c1d5f-186">Klicken Sie auf den Link **Verwalten** .</span><span class="sxs-lookup"><span data-stu-id="c1d5f-186">Click on the **Manage** link.</span></span> <span data-ttu-id="c1d5f-187">Notieren Sie sich die **externen Anmeldungen: 0** , die diesem Konto zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="c1d5f-188">Klicken Sie auf den Link zu einem anderen Dienst Protokoll, und akzeptieren Sie die APP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="c1d5f-189">Die beiden Konten wurden kombiniert, Sie können sich mit jedem Konto anmelden.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="c1d5f-190">Möglicherweise möchten Sie, dass Ihre Benutzer lokale Konten hinzufügen, falls Ihr soziales Protokoll im Authentifizierungsdienst ausgefallen ist, oder wahrscheinlich, dass Sie den Zugriff auf Ihr Social Media-Konto verloren haben.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="c1d5f-191">In der folgenden Abbildung ist Tom ein soziales Protokoll in (das Sie über die **externen Anmeldungen sehen können: 1** auf der Seite angezeigt).</span><span class="sxs-lookup"><span data-stu-id="c1d5f-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="c1d5f-192">Wenn Sie auf " **Kennwort auswählen** " klicken, können Sie ein lokales Protokoll hinzufügen, das demselben Konto zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="c1d5f-193">E-Mail-Bestätigung ausführlicher</span><span class="sxs-lookup"><span data-stu-id="c1d5f-193">Email confirmation in more depth</span></span>

<span data-ttu-id="c1d5f-194">Die [Bestätigung und Kenn Wort wiederASP.net Identity Herstellung](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) für das Tutorial in diesem Thema enthält weitere Details.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="c1d5f-195">Debuggen der APP</span><span class="sxs-lookup"><span data-stu-id="c1d5f-195">Debugging the app</span></span>

<span data-ttu-id="c1d5f-196">Wenn Sie keine e-Mail erhalten, die den Link enthält:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="c1d5f-197">Überprüfen Sie Ihren Junk-oder Spam Ordner.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="c1d5f-198">Melden Sie sich bei Ihrem sendgrid-Konto an, und klicken Sie auf den [Link Email Activity](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="c1d5f-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="c1d5f-199">Wenn Sie den Überprüfungs Link ohne e-Mail testen möchten, laden Sie das [vollständige Beispiel](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)herunter.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="c1d5f-200">Der Bestätigungslink und die Bestätigungs Codes werden auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="c1d5f-201">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c1d5f-201">Additional Resources</span></span>

- [<span data-ttu-id="c1d5f-202">Links zu ASP.net Identity empfohlenen Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c1d5f-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="c1d5f-203">[Konto Bestätigung und Kenn Wort Wiederherstellung mit ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Weitere Informationen zur Kenn Wort Wiederherstellung und zur Konto Bestätigung.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="c1d5f-204">[MVC 5-App mit Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-App mit Facebook-und Google OAuth 2-Autorisierung schreiben.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="c1d5f-205">Außerdem wird gezeigt, wie Sie der Identitätsdatenbank zusätzliche Daten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="c1d5f-206">Stellen [Sie eine sichere ASP.NET MVC-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)bereit.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="c1d5f-207">In diesem Tutorial wird die Azure-Bereitstellung hinzugefügt, die Sicherheit Ihrer APP mit Rollen, die Verwendung der Mitgliedschafts-API zum Hinzufügen von Benutzern und Rollen sowie zusätzliche Sicherheitsfeatures erläutert.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="c1d5f-208">Erstellen einer Google-App für OAuth 2 und Verbinden der APP mit dem Projekt</span><span class="sxs-lookup"><span data-stu-id="c1d5f-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="c1d5f-209">Erstellen der app in Facebook und Verbinden der APP mit dem Projekt</span><span class="sxs-lookup"><span data-stu-id="c1d5f-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="c1d5f-210">Einrichten von SSL im Projekt</span><span class="sxs-lookup"><span data-stu-id="c1d5f-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
