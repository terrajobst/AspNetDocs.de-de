---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: ASP.NET MVC 5-App mit zweistufiger SMS-und e-Mail-Authentifizierung | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Web-App mit zweistufiger Authentifizierung erstellen. Erstellen Sie eine sichere ASP.NET MVC 5-Web-App mit...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457595"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="61933-104">ASP.NET MVC 5-App mit zweistufiger Authentifizierung per SMS und E-Mail</span><span class="sxs-lookup"><span data-stu-id="61933-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>

<span data-ttu-id="61933-105">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="61933-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="61933-106">In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Web-App mit zweistufiger Authentifizierung erstellen.</span><span class="sxs-lookup"><span data-stu-id="61933-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="61933-107">[Erstellen Sie eine sichere ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-Bestätigung und Kenn Wort](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) Zurücksetzung, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="61933-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="61933-108">Sie können die abgeschlossene Anwendung [hier](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)herunterladen.</span><span class="sxs-lookup"><span data-stu-id="61933-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="61933-109">Der Download enthält debughilfsprogramme, mit denen Sie e-Mail-Bestätigung und SMS testen können, ohne eine e-Mail oder einen SMS-Anbieter einzurichten.</span><span class="sxs-lookup"><span data-stu-id="61933-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="61933-110">Dieses Tutorial wurde von [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ) verfasst.</span><span class="sxs-lookup"><span data-stu-id="61933-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

- [<span data-ttu-id="61933-111">Erstellen einer ASP.NET-MVC-App</span><span class="sxs-lookup"><span data-stu-id="61933-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="61933-112">Einrichten von SMS für die zweistufige Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="61933-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="61933-113">Aktivieren der zweistufigen Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="61933-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="61933-114">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="61933-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="61933-115">Erstellen einer ASP.NET-MVC-App</span><span class="sxs-lookup"><span data-stu-id="61933-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="61933-116">Beginnen Sie mit der Installation und Ausführung von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="61933-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="61933-117">Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.</span><span class="sxs-lookup"><span data-stu-id="61933-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="61933-118">Warnung: [Erstellen Sie eine Secure ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-Bestätigung und Kenn Wort](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) Zurücksetzung, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="61933-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="61933-119">Sie müssen [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher installieren, um dieses Tutorial abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="61933-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="61933-120">Erstellen Sie ein neues ASP.NET-Webprojekt, und wählen Sie die MVC-Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="61933-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="61933-121">Web Forms auch ASP.net Identity unterstützt, können Sie ähnliche Schritte in einer Web Forms-app ausführen.</span><span class="sxs-lookup"><span data-stu-id="61933-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="61933-122">Überlassen Sie die Standard Authentifizierung als **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="61933-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="61933-123">Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert.</span><span class="sxs-lookup"><span data-stu-id="61933-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="61933-124">Später in diesem Tutorial wird die Bereitstellung in Azure durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="61933-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="61933-125">Sie können [ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="61933-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="61933-126">Legen Sie fest, dass das [Projekt SSL verwendet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="61933-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="61933-127">Einrichten von SMS für die zweistufige Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="61933-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="61933-128">Dieses Tutorial enthält Anweisungen zur Verwendung von twilio oder ASPSMS, aber Sie können auch einen beliebigen anderen SMS-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="61933-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="61933-129">**Erstellen eines Benutzerkontos mit einem SMS-Anbieter**</span><span class="sxs-lookup"><span data-stu-id="61933-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="61933-130">Erstellen Sie ein [twilio](https://www.twilio.com/try-twilio) -oder [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) -Konto.</span><span class="sxs-lookup"><span data-stu-id="61933-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="61933-131">**Installieren zusätzlicher Pakete oder Hinzufügen von Dienst verweisen**</span><span class="sxs-lookup"><span data-stu-id="61933-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="61933-132">Twilio</span><span class="sxs-lookup"><span data-stu-id="61933-132">Twilio:</span></span>  
   <span data-ttu-id="61933-133">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="61933-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="61933-134">Aspsms:</span><span class="sxs-lookup"><span data-stu-id="61933-134">ASPSMS:</span></span>  
   <span data-ttu-id="61933-135">Der folgende Dienst Verweis muss hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="61933-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="61933-136">Adresse:</span><span class="sxs-lookup"><span data-stu-id="61933-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="61933-137">Namespace:</span><span class="sxs-lookup"><span data-stu-id="61933-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="61933-138">**Ermitteln der Benutzer Anmelde Informationen für den SMS-Anbieter**</span><span class="sxs-lookup"><span data-stu-id="61933-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="61933-139">Twilio</span><span class="sxs-lookup"><span data-stu-id="61933-139">Twilio:</span></span>  
   <span data-ttu-id="61933-140">Kopieren Sie die **Konto-SID** und das Authentifizierungs **Token**auf der Registerkarte **Dashboard** Ihres twilio-Kontos.</span><span class="sxs-lookup"><span data-stu-id="61933-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="61933-141">Aspsms:</span><span class="sxs-lookup"><span data-stu-id="61933-141">ASPSMS:</span></span>  
   <span data-ttu-id="61933-142">Navigieren Sie in Ihren Kontoeinstellungen zu **UserKey** , und kopieren Sie Sie mit Ihrem selbst definierten **Kennwort**.</span><span class="sxs-lookup"><span data-stu-id="61933-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="61933-143">Diese Werte werden später in der Datei " *Web. config* " in den Schlüsseln `"SMSAccountIdentification"` und `"SMSAccountPassword"` gespeichert.</span><span class="sxs-lookup"><span data-stu-id="61933-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="61933-144">**Angeben von SenderID/Absender**</span><span class="sxs-lookup"><span data-stu-id="61933-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="61933-145">Twilio</span><span class="sxs-lookup"><span data-stu-id="61933-145">Twilio:</span></span>  
   <span data-ttu-id="61933-146">Kopieren Sie Ihre twilio-Telefonnummer auf der Registerkarte **Zahlen** .</span><span class="sxs-lookup"><span data-stu-id="61933-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="61933-147">Aspsms:</span><span class="sxs-lookup"><span data-stu-id="61933-147">ASPSMS:</span></span>  
   <span data-ttu-id="61933-148">Entsperren Sie einen oder mehrere Originatoren im Menü **Unlock** -Absender, oder wählen Sie einen alphanumerischen Absender aus (wird nicht von allen Netzwerken unterstützt).</span><span class="sxs-lookup"><span data-stu-id="61933-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="61933-149">Dieser Wert wird später in der Datei " *Web. config* " im Schlüssel `"SMSAccountFrom"` gespeichert.</span><span class="sxs-lookup"><span data-stu-id="61933-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="61933-150">**Übertragen der Anmelde Informationen für den SMS-Anbieter**</span><span class="sxs-lookup"><span data-stu-id="61933-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="61933-151">Stellen Sie die Anmelde Informationen und die Absender Telefonnummer für die APP zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="61933-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="61933-152">Um dies zu gewährleisten, werden diese Werte in der Datei " *Web. config* " gespeichert.</span><span class="sxs-lookup"><span data-stu-id="61933-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="61933-153">Wenn die Bereitstellung in Azure durchzuführen ist, können wir die Werte im Abschnitt " **App-Einstellungen** " auf der Registerkarte "Website konfigurieren" sicher speichern.</span><span class="sxs-lookup"><span data-stu-id="61933-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="61933-154">Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode.</span><span class="sxs-lookup"><span data-stu-id="61933-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="61933-155">Das Konto und die Anmelde Informationen werden dem obigen Code hinzugefügt, um das Beispiel einfach zu halten.</span><span class="sxs-lookup"><span data-stu-id="61933-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="61933-156">Weitere Informationen finden [Sie unter Bewährte Methoden für die Bereitstellung von Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)</span><span class="sxs-lookup"><span data-stu-id="61933-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="61933-157">**Implementierung der Datenübertragung an den SMS-Anbieter**</span><span class="sxs-lookup"><span data-stu-id="61933-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="61933-158">Konfigurieren Sie die `SmsService`-Klasse in der *App\_start\identityconfig.cs* -Datei.</span><span class="sxs-lookup"><span data-stu-id="61933-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="61933-159">Aktivieren Sie je nach verwendetem SMS-Anbieter entweder den **twilio** -oder den **ASPSMS** -Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="61933-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="61933-160">Aktualisieren Sie die Razor-Ansicht *views\manage\index.cshtml* : (Hinweis: Entfernen Sie nicht nur die Kommentare im beendenden Code, und verwenden Sie den folgenden Code.)</span><span class="sxs-lookup"><span data-stu-id="61933-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="61933-161">Überprüfen Sie, ob die `EnableTwoFactorAuthentication`-und `DisableTwoFactorAuthentication` Aktionsmethoden in der `ManageController` über das[[validateantiforgerytoken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) -Attribut verfügen:</span><span class="sxs-lookup"><span data-stu-id="61933-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="61933-162">Führen Sie die APP aus, und melden Sie sich mit dem zuvor registrierten Konto an.</span><span class="sxs-lookup"><span data-stu-id="61933-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="61933-163">Klicken Sie auf Ihre Benutzer-ID, die die `Index` Aktionsmethode in `Manage` Controller aktiviert.</span><span class="sxs-lookup"><span data-stu-id="61933-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="61933-164">Klicken Sie auf Hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="61933-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="61933-165">Die `AddPhoneNumber` Aktionsmethode zeigt ein Dialogfeld an, in dem Sie eine Telefonnummer eingeben können, die SMS-Nachrichten empfangen kann.</span><span class="sxs-lookup"><span data-stu-id="61933-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="61933-166">In wenigen Sekunden erhalten Sie eine Textnachricht mit dem Überprüfungs Code.</span><span class="sxs-lookup"><span data-stu-id="61933-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="61933-167">Geben Sie ihn ein, **und drücken Sie die Eingabe**</span><span class="sxs-lookup"><span data-stu-id="61933-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="61933-168">In der Ansicht verwalten wird angezeigt, dass Ihre Telefonnummer hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="61933-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="61933-169">Zwei-Faktor-Authentifizierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="61933-169">Enable two-factor authentication</span></span>

<span data-ttu-id="61933-170">In der von der Vorlage generierten App müssen Sie die Benutzeroberfläche verwenden, um die zweistufige Authentifizierung (2FA) zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="61933-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="61933-171">Um 2FA zu aktivieren, klicken Sie auf der Navigationsleiste auf Ihre Benutzer-ID (e-Mail-Alias).</span><span class="sxs-lookup"><span data-stu-id="61933-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="61933-172">Klicken Sie auf 2 Fa aktivieren.</span><span class="sxs-lookup"><span data-stu-id="61933-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="61933-173">Melden Sie sich ab, und melden Sie sich erneut an.</span><span class="sxs-lookup"><span data-stu-id="61933-173">Logout, then log back in.</span></span> <span data-ttu-id="61933-174">Wenn Sie e-Mail aktiviert haben (siehe mein [Vorheriges Tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail für 2FA auswählen.</span><span class="sxs-lookup"><span data-stu-id="61933-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="61933-175">Die Seite Code überprüfen wird angezeigt, auf der Sie den Code (von SMS oder e-Mail) eingeben können.</span><span class="sxs-lookup"><span data-stu-id="61933-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="61933-176">Wenn Sie auf das Kontrollkästchen **diesen Browser speichern** klicken, werden Sie von der Verwendung von 2FA für die Anmeldung aufgefordert, wenn Sie den Browser und das Gerät verwenden, auf dem Sie das Kontrollkästchen aktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="61933-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="61933-177">Solange böswillige Benutzer nicht auf Ihr Gerät zugreifen können, wird durch das Aktivieren von 2FA und das Klicken auf den Benutzer, der **sich in diesem Browser** befindet, ein bequemer Zugriff auf das Kennwort bereitgestellt, während gleichzeitig ein starker 2FA-Schutz für den gesamten Zugriff von nicht vertrauenswürdigen Geräten beibehalten wird</span><span class="sxs-lookup"><span data-stu-id="61933-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="61933-178">Sie können auf allen privaten Geräten dazu, die Sie regelmäßig verwenden.</span><span class="sxs-lookup"><span data-stu-id="61933-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="61933-179">Dieses Tutorial enthält eine kurze Einführung in die Aktivierung von 2FA für eine neue ASP.NET MVC-app.</span><span class="sxs-lookup"><span data-stu-id="61933-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="61933-180">In meinem Tutorial wird die [zweistufige Authentifizierung mit SMS und e-Mail mit ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) ausführlich zum Code hinter dem Beispiel behandelt.</span><span class="sxs-lookup"><span data-stu-id="61933-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="61933-181">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="61933-181">Additional Resources</span></span>

- <span data-ttu-id="61933-182">[Zweistufige Authentifizierung mit SMS und e-Mail mit ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Detaillierte Informationen zur zweistufigen Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="61933-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="61933-183">Links zu ASP.net Identity empfohlenen Ressourcen</span><span class="sxs-lookup"><span data-stu-id="61933-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="61933-184">[Konto Bestätigung und Kenn Wort Wiederherstellung mit ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Weitere Informationen zur Kenn Wort Wiederherstellung und zur Konto Bestätigung.</span><span class="sxs-lookup"><span data-stu-id="61933-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="61933-185">[MVC 5-App mit Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-App mit Facebook-und Google OAuth 2-Autorisierung schreiben.</span><span class="sxs-lookup"><span data-stu-id="61933-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="61933-186">Außerdem wird gezeigt, wie Sie der Identitätsdatenbank zusätzliche Daten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="61933-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="61933-187">Stellen [Sie eine sichere ASP.NET MVC-App mit Mitgliedschaft, OAuth und SQL-Datenbank im Azure-Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)bereit.</span><span class="sxs-lookup"><span data-stu-id="61933-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="61933-188">In diesem Tutorial wird die Azure-Bereitstellung hinzugefügt, die Sicherheit Ihrer APP mit Rollen, die Verwendung der Mitgliedschafts-API zum Hinzufügen von Benutzern und Rollen sowie zusätzliche Sicherheitsfeatures erläutert.</span><span class="sxs-lookup"><span data-stu-id="61933-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="61933-189">Erstellen einer Google-App für OAuth 2 und Verbinden der APP mit dem Projekt</span><span class="sxs-lookup"><span data-stu-id="61933-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="61933-190">Erstellen der app in Facebook und Verbinden der APP mit dem Projekt</span><span class="sxs-lookup"><span data-stu-id="61933-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="61933-191">Einrichten von SSL im Projekt</span><span class="sxs-lookup"><span data-stu-id="61933-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="61933-192">Einrichten der C# Entwicklungsumgebung für und ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="61933-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
