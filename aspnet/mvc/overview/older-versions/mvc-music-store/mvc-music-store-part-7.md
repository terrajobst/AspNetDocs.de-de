---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Teil 7: Mitgliedschaft und Autorisierung | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 7 werden die Mitgliedschaft und die Autorisierung behandelt.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433467"
---
# <a name="part-7-membership-and-authorization"></a><span data-ttu-id="6a46b-104">Teil 7: Mitgliedschaft und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="6a46b-104">Part 7: Membership and Authorization</span></span>

<span data-ttu-id="6a46b-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6a46b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6a46b-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6a46b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6a46b-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="6a46b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6a46b-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="6a46b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6a46b-109">In Teil 7 werden die Mitgliedschaft und die Autorisierung behandelt.</span><span class="sxs-lookup"><span data-stu-id="6a46b-109">Part 7 covers Membership and Authorization.</span></span>

<span data-ttu-id="6a46b-110">Der Store Manager-Controller ist zurzeit für alle Benutzer zugänglich, die unsere Website besuchen.</span><span class="sxs-lookup"><span data-stu-id="6a46b-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="6a46b-111">Ändern wir dies, um die Berechtigung für Website Administratoren einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="6a46b-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="6a46b-112">Hinzufügen der AccountController-und-Sichten</span><span class="sxs-lookup"><span data-stu-id="6a46b-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="6a46b-113">Ein Unterschied zwischen der vollständigen ASP.NET MVC 3-Webanwendungs Vorlage und der Vorlage für eine leere ASP.NET MVC 3-Webanwendung besteht darin, dass die leere Vorlage keinen Konto Controller enthält.</span><span class="sxs-lookup"><span data-stu-id="6a46b-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="6a46b-114">Wir fügen einen Konto Controller hinzu, indem wir einige Dateien aus einer neuen ASP.NET MVC-Anwendung kopieren, die aus der Vorlage Full ASP.NET MVC 3-Webanwendung erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="6a46b-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="6a46b-115">Erstellen Sie eine neue ASP.NET MVC-Anwendung mithilfe der Vorlage Full ASP.NET MVC 3-Webanwendung, und kopieren Sie die folgenden Dateien in die gleichen Verzeichnisse in unserem Projekt:</span><span class="sxs-lookup"><span data-stu-id="6a46b-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="6a46b-116">Kopieren Sie AccountController.cs in das Verzeichnis Controller.</span><span class="sxs-lookup"><span data-stu-id="6a46b-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="6a46b-117">Accountmodels in das Modell Verzeichnis kopieren</span><span class="sxs-lookup"><span data-stu-id="6a46b-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="6a46b-118">Erstellen Sie im Verzeichnis "Views" ein Konto Verzeichnis, und kopieren Sie alle vier Sichten in</span><span class="sxs-lookup"><span data-stu-id="6a46b-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="6a46b-119">Ändern Sie den Namespace für die Controller-und Modellklassen, damit Sie mit dem mvcmusicstore beginnen.</span><span class="sxs-lookup"><span data-stu-id="6a46b-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="6a46b-120">Die AccountController-Klasse sollte den mvcmusicstore. Controllers-Namespace verwenden, und die accountmodels-Klasse sollte den mvcmusicstore. Models-Namespace verwenden.</span><span class="sxs-lookup"><span data-stu-id="6a46b-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="6a46b-121">*Hinweis: diese Dateien sind auch im Download MvcMusicStore-Assets. zip verfügbar, von dem aus wir unsere Website Entwurfs Dateien zu Beginn des Tutorials kopiert haben. Die Mitgliedschafts Dateien befinden sich im Code Verzeichnis.*</span><span class="sxs-lookup"><span data-stu-id="6a46b-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="6a46b-122">Die aktualisierte Lösung sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="6a46b-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="6a46b-123">Hinzufügen eines Administrators mit der ASP.NET-Konfigurations Website</span><span class="sxs-lookup"><span data-stu-id="6a46b-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="6a46b-124">Bevor wir auf unserer Website eine Autorisierung benötigen, müssen wir einen Benutzer mit Zugriff erstellen.</span><span class="sxs-lookup"><span data-stu-id="6a46b-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="6a46b-125">Die einfachste Möglichkeit zum Erstellen eines Benutzers besteht darin, die integrierte ASP.NET-Konfigurations Website zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="6a46b-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="6a46b-126">Starten Sie die ASP.NET-Konfigurations Website, indem Sie auf das Symbol im Projektmappen-Explorer klicken.</span><span class="sxs-lookup"><span data-stu-id="6a46b-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="6a46b-127">Dadurch wird eine Konfigurations Website gestartet.</span><span class="sxs-lookup"><span data-stu-id="6a46b-127">This launches a configuration website.</span></span> <span data-ttu-id="6a46b-128">Klicken Sie im Startbildschirm auf die Registerkarte Sicherheit, und klicken Sie dann auf den Link "Rollen aktivieren" in der Mitte des Bildschirms.</span><span class="sxs-lookup"><span data-stu-id="6a46b-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="6a46b-129">Klicken Sie auf den Link "Rollen erstellen oder verwalten".</span><span class="sxs-lookup"><span data-stu-id="6a46b-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="6a46b-130">Geben Sie "Administrator" als Rollenname ein, und klicken Sie auf die Schaltfläche Rolle hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6a46b-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="6a46b-131">Klicken Sie auf die Schaltfläche zurück, und klicken Sie auf der linken Seite auf den Link Benutzer erstellen.</span><span class="sxs-lookup"><span data-stu-id="6a46b-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="6a46b-132">Füllen Sie die Felder für die Benutzerinformationen auf der linken Seite mit den folgenden Informationen aus:</span><span class="sxs-lookup"><span data-stu-id="6a46b-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="6a46b-133">**Feld**</span><span class="sxs-lookup"><span data-stu-id="6a46b-133">**Field**</span></span> | <span data-ttu-id="6a46b-134">**Wert**</span><span class="sxs-lookup"><span data-stu-id="6a46b-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="6a46b-135">**Benutzername**</span><span class="sxs-lookup"><span data-stu-id="6a46b-135">**User Name**</span></span> | <span data-ttu-id="6a46b-136">Administrator</span><span class="sxs-lookup"><span data-stu-id="6a46b-136">Administrator</span></span> |
| <span data-ttu-id="6a46b-137">**Kennwort**</span><span class="sxs-lookup"><span data-stu-id="6a46b-137">**Password**</span></span> | <span data-ttu-id="6a46b-138">password123!</span><span class="sxs-lookup"><span data-stu-id="6a46b-138">password123!</span></span> |
| <span data-ttu-id="6a46b-139">**Kennwort bestätigen**</span><span class="sxs-lookup"><span data-stu-id="6a46b-139">**Confirm Password**</span></span> | <span data-ttu-id="6a46b-140">password123!</span><span class="sxs-lookup"><span data-stu-id="6a46b-140">password123!</span></span> |
| <span data-ttu-id="6a46b-141">**E-Mail**</span><span class="sxs-lookup"><span data-stu-id="6a46b-141">**E-mail**</span></span> | <span data-ttu-id="6a46b-142">(eine beliebige e-Mail-Adresse funktioniert)</span><span class="sxs-lookup"><span data-stu-id="6a46b-142">(any email address will work)</span></span> |
| <span data-ttu-id="6a46b-143">**Sicherheitsfrage**</span><span class="sxs-lookup"><span data-stu-id="6a46b-143">**Security Question**</span></span> | <span data-ttu-id="6a46b-144">(je nachdem, was Sie möchten)</span><span class="sxs-lookup"><span data-stu-id="6a46b-144">(whatever you like)</span></span> |
| <span data-ttu-id="6a46b-145">**Sicherheitsantwort**</span><span class="sxs-lookup"><span data-stu-id="6a46b-145">**Security Answer**</span></span> | <span data-ttu-id="6a46b-146">(je nachdem, was Sie möchten)</span><span class="sxs-lookup"><span data-stu-id="6a46b-146">(whatever you like)</span></span> |

<span data-ttu-id="6a46b-147">*Hinweis: Sie können natürlich auch ein beliebiges Kennwort verwenden. Die Standardeinstellungen für die Kenn Wort Sicherheit erfordern ein Kennwort mit einer Länge von 7 Zeichen und ein nicht alphanumerisches Zeichen.*</span><span class="sxs-lookup"><span data-stu-id="6a46b-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="6a46b-148">Wählen Sie die Administrator Rolle für diesen Benutzer aus, und klicken Sie auf die Schaltfläche Benutzer erstellen.</span><span class="sxs-lookup"><span data-stu-id="6a46b-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="6a46b-149">An diesem Punkt sollte eine Meldung angezeigt werden, die besagt, dass der Benutzer erfolgreich erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="6a46b-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="6a46b-150">Sie können das Browserfenster jetzt schließen.</span><span class="sxs-lookup"><span data-stu-id="6a46b-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="6a46b-151">Rollenbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="6a46b-151">Role-based Authorization</span></span>

<span data-ttu-id="6a46b-152">Nun können wir den Zugriff auf den storemanagercontroller mithilfe des Attributs [autorisieren] einschränken und angeben, dass der Benutzer in der Administrator Rolle sein muss, um auf eine beliebige Controller Aktion in der Klasse zugreifen zu können.</span><span class="sxs-lookup"><span data-stu-id="6a46b-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="6a46b-153">*Hinweis: das Attribut [autorisieren] kann auf bestimmte Aktionsmethoden und auf Controller Klassenebene platziert werden.*</span><span class="sxs-lookup"><span data-stu-id="6a46b-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="6a46b-154">Wenn Sie jetzt zu/StoreManager navigieren, wird ein Dialogfeld für die Anmeldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="6a46b-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="6a46b-155">Nachdem Sie sich mit unserem neuen Administrator Konto angemeldet haben, können wir wie zuvor zum Bildschirm zum Bearbeiten des Albums wechseln.</span><span class="sxs-lookup"><span data-stu-id="6a46b-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6a46b-156">[Zurück](mvc-music-store-part-6.md)
> [Weiter](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="6a46b-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
