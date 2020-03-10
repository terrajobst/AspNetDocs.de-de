---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Verwenden eines CAPTCHA-Diensts, um zu verhindern, dass Bots Ihre ASP.net Web Razor-Website verwenden | Microsoft-Dokumentation
author: microsoft
description: In diesem Artikel wird erläutert, wie Sie mit "reCAPTCHA (a Security Measure)" verhindern, dass automatisierte Programme (Bots) Aufgaben in einem ASP.net Web Pages (Razor) ausführen...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440193"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="78a3d-103">Verwenden eines CAPTCHA-Diensts, um zu verhindern, dass Bots Ihre ASP.net Web Razor-Website verwenden</span><span class="sxs-lookup"><span data-stu-id="78a3d-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>

<span data-ttu-id="78a3d-104">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="78a3d-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="78a3d-105">In diesem Artikel wird erläutert, wie Sie mit "reCAPTCHA" (einer Sicherheitsmaßnahme) verhindern, dass automatisierte Programme (Bots) Aufgaben auf einer ASP.net Web Pages-Website (Razor) ausführen.</span><span class="sxs-lookup"><span data-stu-id="78a3d-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="78a3d-106">**Lernen Sie Folgendes:**</span><span class="sxs-lookup"><span data-stu-id="78a3d-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="78a3d-107">Vorgehensweise beim Hinzufügen eines CAPTCHA-Tests zu Ihrer Website.</span><span class="sxs-lookup"><span data-stu-id="78a3d-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="78a3d-108">Dies sind die ASP.NET-Funktionen, die im Artikel eingeführt wurden:</span><span class="sxs-lookup"><span data-stu-id="78a3d-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="78a3d-109">Das `ReCaptcha`-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="78a3d-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="78a3d-110">Die Informationen in diesem Artikel gelten für ASP.net Web Pages 1,0 und Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="78a3d-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>

## <a name="about-captchas"></a><span data-ttu-id="78a3d-111">Informationen zu Captchas</span><span class="sxs-lookup"><span data-stu-id="78a3d-111">About CAPTCHAs</span></span>

<span data-ttu-id="78a3d-112">Jedes Mal, wenn Sie die Registrierung an Ihrer Website gestatten oder sogar einfach einen Namen und eine URL eingeben (z.b. für einen Blog Kommentar), erhalten Sie möglicherweise eine Flut von gefälschten Namen.</span><span class="sxs-lookup"><span data-stu-id="78a3d-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="78a3d-113">Diese sind häufig von automatisierten Programmen (Bots) übrig, die versuchen, URLs auf jeder Website zu verlassen, die Sie finden.</span><span class="sxs-lookup"><span data-stu-id="78a3d-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="78a3d-114">(Eine gängige Motivation besteht darin, die URLs von Produkten für den Verkauf zu veröffentlichen.)</span><span class="sxs-lookup"><span data-stu-id="78a3d-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="78a3d-115">Sie können sicherstellen, dass ein Benutzer echte Person und kein Computerprogramm ist, indem Sie eine *CAPTCHA* verwenden, um Benutzer zu überprüfen, wenn Sie Ihren Namen und Ihre Website registrieren oder anderweitig eingeben.</span><span class="sxs-lookup"><span data-stu-id="78a3d-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="78a3d-116">CAPTCHA steht für vollständig automatisierte öffentliche Tests, um Computer und Menschen voneinander zu informieren.</span><span class="sxs-lookup"><span data-stu-id="78a3d-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="78a3d-117">Ein CAPTCHA ist ein *Challenge-Response-* Test, bei dem der Benutzer aufgefordert wird, etwas zu tun, das für eine Person einfach ist, aber für ein automatisiertes Programm schwierig ist.</span><span class="sxs-lookup"><span data-stu-id="78a3d-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="78a3d-118">Der häufigste Typ von CAPTCHA ist ein Typ, in dem Sie einige verzerrte Buchstaben sehen und aufgefordert werden, Sie einzugeben.</span><span class="sxs-lookup"><span data-stu-id="78a3d-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="78a3d-119">(Die Verzerrung soll es für Bots schwierig machen, die Buchstaben zu entschlüsseln.)</span><span class="sxs-lookup"><span data-stu-id="78a3d-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="78a3d-120">Hinzufügen eines "reCAPTCHA"-Tests</span><span class="sxs-lookup"><span data-stu-id="78a3d-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="78a3d-121">In ASP.NET-Seiten können Sie mithilfe des `ReCaptcha`-Hilfsprogramms einen CAPTCHA-Test, der auf dem "reCAPTCHA"-Dienst ([http://recaptcha.net](http://recaptcha.net)) basiert, darstellen.</span><span class="sxs-lookup"><span data-stu-id="78a3d-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="78a3d-122">Das `ReCaptcha`-Hilfsprogramm zeigt ein Bild mit zwei verzerrten Wörtern an, die Benutzer vor dem Validieren der Seite ordnungsgemäß eingeben müssen.</span><span class="sxs-lookup"><span data-stu-id="78a3d-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="78a3d-123">Die Benutzer Antwort wird vom reCAPTCHA.net-Dienst überprüft.</span><span class="sxs-lookup"><span data-stu-id="78a3d-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="78a3d-124">Registrieren Sie Ihre Website unter reCAPTCHA.net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="78a3d-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="78a3d-125">Wenn Sie die Registrierung abgeschlossen haben, erhalten Sie einen öffentlichen Schlüssel und einen privaten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="78a3d-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="78a3d-126">Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.</span><span class="sxs-lookup"><span data-stu-id="78a3d-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="78a3d-127">Wenn Sie nicht bereits über eine *\_AppStart. cshtml* -Datei verfügen, erstellen Sie im Stamm Ordner einer Website eine Datei mit dem Namen *\_AppStart. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78a3d-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="78a3d-128">Fügen Sie die folgenden `Recaptcha` Hilfsobjekte in der *\_AppStart. cshtml* -Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="78a3d-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="78a3d-129">Legen Sie die Eigenschaften "`PublicKey`" und "`PrivateKey`" mithilfe ihrer eigenen öffentlichen und privaten Schlüssel fest.</span><span class="sxs-lookup"><span data-stu-id="78a3d-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="78a3d-130">Speichern Sie die Datei *\_AppStart. cshtml* , und schließen Sie Sie.</span><span class="sxs-lookup"><span data-stu-id="78a3d-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="78a3d-131">Erstellen Sie im Stamm Ordner einer Website eine neue Seite mit dem Namen " *reCAPTCHA. cshtml*".</span><span class="sxs-lookup"><span data-stu-id="78a3d-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="78a3d-132">Ersetzen Sie den vorhandenen Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="78a3d-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="78a3d-133">Führen Sie die Seite " *reCAPTCHA. cshtml* " in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="78a3d-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="78a3d-134">Wenn der `PrivateKey` Wert gültig ist, zeigt die Seite das Steuerelement "reCAPTCHA" und eine Schaltfläche an.</span><span class="sxs-lookup"><span data-stu-id="78a3d-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="78a3d-135">Wenn Sie die Schlüssel nicht global in *\_AppStart. html*festgelegt haben, wird auf der Seite ein Fehler angezeigt.</span><span class="sxs-lookup"><span data-stu-id="78a3d-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="78a3d-136">Geben Sie die Wörter für den Test ein.</span><span class="sxs-lookup"><span data-stu-id="78a3d-136">Enter the words for the test.</span></span> <span data-ttu-id="78a3d-137">Wenn Sie den "reCAPTCHA"-Test bestanden haben, wird eine entsprechende Meldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="78a3d-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="78a3d-138">Andernfalls wird eine Fehlermeldung angezeigt, und das Steuerelement "reCAPTCHA" wird erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="78a3d-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="78a3d-139">Wenn sich der Computer in einer Domäne befindet, die den Proxy Server verwendet, müssen Sie möglicherweise das `defaultproxy`-Element der Datei " *Web. config* " konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="78a3d-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="78a3d-140">Das folgende Beispiel zeigt eine *Web. config* -Datei mit dem-`defaultproxy`-Element, das so konfiguriert ist, dass der-Dienst für die Zusammenarbeit aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="78a3d-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="78a3d-141">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="78a3d-141">Additional Resources</span></span>

- [<span data-ttu-id="78a3d-142">Anpassen des Standort weiten Verhaltens für ASP.net Web Pages Websites</span><span class="sxs-lookup"><span data-stu-id="78a3d-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="78a3d-143">Website "reCAPTCHA"</span><span class="sxs-lookup"><span data-stu-id="78a3d-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
