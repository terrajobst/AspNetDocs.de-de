---
uid: whitepapers/request-validation
title: Anforderungs Validierung-verhindern von Skript Angriffen | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument wird das Anforderungs Überprüfungs Feature von ASP.net beschrieben, bei dem die Anwendung standardmäßig daran gehindert wird, nicht codierten HTML-Inhaltstyp zu verarbeiten...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520581"
---
# <a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="49da0-103">Anforderungsvalidierung – Schutz vor Angriffen durch Skripteinschleusung</span><span class="sxs-lookup"><span data-stu-id="49da0-103">Request Validation - Preventing Script Attacks</span></span>

> <span data-ttu-id="49da0-104">In diesem Dokument wird das Anforderungs Überprüfungs Feature von ASP.net beschrieben, wobei die Anwendung standardmäßig daran gehindert wird, nicht codierten HTML-Inhalt zu verarbeiten, der an den Server gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="49da0-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="49da0-105">Diese Anforderungs Validierungsfunktion kann deaktiviert werden, wenn die Anwendung für die sichere Verarbeitung von HTML-Daten konzipiert wurde.</span><span class="sxs-lookup"><span data-stu-id="49da0-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="49da0-106">Gilt für ASP.NET 1,1 und ASP.NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="49da0-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>

<span data-ttu-id="49da0-107">Das Anforderungsüberprüfungsfeature von ASP.NET ist seit Version 1.1 verfügbar und verhindert, dass der Server Inhalte mit nicht codierter HTML akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="49da0-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="49da0-108">Dieses Feature dient zur Abwehr von Skripteinschleusungsangriffen, bei denen Clientskriptcode oder HTML unbewusst an einen Server übermittelt, gespeichert und anschließend anderen Benutzern angezeigt werden kann.</span><span class="sxs-lookup"><span data-stu-id="49da0-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="49da0-109">Es wird weiterhin dringend empfohlen, sämtliche Eingabedaten zu überprüfen und gegebenenfalls als HTML zu codieren.</span><span class="sxs-lookup"><span data-stu-id="49da0-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="49da0-110">Beispielsweise erstellen Sie eine Webseite, die die e-Mail-Adresse eines Benutzers anfordert und diese e-Mail-Adresse dann in einer Datenbank speichert.</span><span class="sxs-lookup"><span data-stu-id="49da0-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="49da0-111">Wenn der Benutzer &lt;Skript&gt;Warnung ("Hello from Script")&lt;/Script&gt; anstelle einer gültigen e-Mail-Adresse eingibt, kann dieses Skript ausgeführt werden, wenn der Inhalt nicht ordnungsgemäß codiert wurde.</span><span class="sxs-lookup"><span data-stu-id="49da0-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="49da0-112">Das Anforderungs Validierungs Feature von ASP.net verhindert, dass dies geschieht.</span><span class="sxs-lookup"><span data-stu-id="49da0-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="49da0-113">Warum diese Funktion nützlich ist</span><span class="sxs-lookup"><span data-stu-id="49da0-113">Why this feature is useful</span></span>

<span data-ttu-id="49da0-114">Viele Websites sind nicht darauf aufmerksam, dass Sie für einfache Skript einschleusungs Angriffe offen sind.</span><span class="sxs-lookup"><span data-stu-id="49da0-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="49da0-115">Unabhängig davon, ob sich der Zweck dieser Angriffe auf die Website durch Anzeigen von HTML oder das Ausführen eines Client Skripts zum Umleiten des Benutzers an die Website eines Hackers auswirken soll, sind Skript einschleusungs Angriffe ein Problem, mit dem sich Webentwickler auseinandersetzen müssen.</span><span class="sxs-lookup"><span data-stu-id="49da0-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="49da0-116">Skript einschleusungs Angriffe sind für alle Webentwickler von Bedeutung, unabhängig davon, ob Sie ASP.net, ASP oder andere Webentwicklungs Technologien verwenden.</span><span class="sxs-lookup"><span data-stu-id="49da0-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="49da0-117">Das ASP.net Request Validation-Feature verhindert diese Angriffe proaktiv, indem es nicht zulässt, dass nicht codierter HTML-Inhalt vom Server verarbeitet wird, es sei denn, der Entwickler beschließt, diesen Inhalt zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="49da0-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="49da0-118">Was Sie erwartet: Fehlerseite</span><span class="sxs-lookup"><span data-stu-id="49da0-118">What to expect: Error Page</span></span>

<span data-ttu-id="49da0-119">Der nachfolgende Screenshot zeigt einen Beispielcode für ASP.net:</span><span class="sxs-lookup"><span data-stu-id="49da0-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="49da0-120">Das Ausführen dieses Codes führt zu einer einfachen Seite, die es Ihnen ermöglicht, Text in das Textfeld einzugeben, auf die Schaltfläche zu klicken und den Text im Label-Steuerelement anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="49da0-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="49da0-121">Wenn jedoch JavaScript, z. b. `<script>alert("hello!")</script>` eingegeben und gesendet werden, wird eine Ausnahme ausgelöst:</span><span class="sxs-lookup"><span data-stu-id="49da0-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="49da0-122">In der Fehlermeldung wird angegeben, dass ein potenziell gefährlicher Request. Form-Wert erkannt wurde, und es werden weitere Details in der Beschreibung angezeigt, um genau zu erkennen, was passiert ist und wie das Verhalten geändert werden kann.</span><span class="sxs-lookup"><span data-stu-id="49da0-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="49da0-123">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="49da0-123">For example:</span></span>

<span data-ttu-id="49da0-124">Die Anforderungs Validierung hat einen potenziell gefährlichen Client Eingabe Wert festgestellt, und die Verarbeitung der Anforderung wurde abgebrochen.</span><span class="sxs-lookup"><span data-stu-id="49da0-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="49da0-125">Dieser Wert kann auf einen Versuch hindeuten, die Sicherheit Ihrer Anwendung zu kompromittieren, z. b. ein Site übergreifender Scripting-Angriff.</span><span class="sxs-lookup"><span data-stu-id="49da0-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="49da0-126">Sie können die Anforderungs Validierung deaktivieren, indem Sie `validateRequest=false` in der Page-Direktive oder im Konfigurations Abschnitt festlegen.</span><span class="sxs-lookup"><span data-stu-id="49da0-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="49da0-127">Es wird jedoch dringend empfohlen, dass die Anwendung in diesem Fall explizit alle Eingaben überprüft.</span><span class="sxs-lookup"><span data-stu-id="49da0-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="49da0-128">Deaktivieren der Anforderungs Validierung auf einer Seite</span><span class="sxs-lookup"><span data-stu-id="49da0-128">Disabling request validation on a page</span></span>

<span data-ttu-id="49da0-129">Um die Anforderungs Validierung auf einer Seite zu deaktivieren, müssen Sie das `validateRequest`-Attribut der Page-Direktive auf `false`festlegen:</span><span class="sxs-lookup"><span data-stu-id="49da0-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="49da0-130">Wenn die Anforderungs Validierung deaktiviert ist, kann der Inhalt an eine Seite übermittelt werden. Es liegt in der Verantwortung des Seiten Entwicklers, sicherzustellen, dass Inhalte ordnungsgemäß codiert oder verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="49da0-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="49da0-131">Deaktivieren der Anforderungs Validierung für Ihre Anwendung</span><span class="sxs-lookup"><span data-stu-id="49da0-131">Disabling request validation for your application</span></span>

<span data-ttu-id="49da0-132">Um die Anforderungs Validierung für die Anwendung zu deaktivieren, müssen Sie eine Web. config-Datei für Ihre Anwendung ändern oder erstellen und das ValidateRequest-Attribut des `<pages />` Abschnitts auf `false`festlegen:</span><span class="sxs-lookup"><span data-stu-id="49da0-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="49da0-133">Wenn Sie die Anforderungs Validierung für alle Anwendungen auf dem Server deaktivieren möchten, können Sie diese Änderung an der Datei Machine. config vornehmen.</span><span class="sxs-lookup"><span data-stu-id="49da0-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="49da0-134">Wenn die Anforderungs Validierung deaktiviert ist, kann der Inhalt an die Anwendung übermittelt werden. Es liegt in der Verantwortung des Anwendungs Entwicklers sicherzustellen, dass Inhalte ordnungsgemäß codiert oder verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="49da0-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="49da0-135">Der folgende Code wird geändert, um die Anforderungs Validierung zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="49da0-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="49da0-136">Wenn das folgende JavaScript in das Textfeld eingegeben wurde `<script>alert("hello!")</script>` lautet das Ergebnis wie folgt:</span><span class="sxs-lookup"><span data-stu-id="49da0-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="49da0-137">Um dies zu verhindern, muss der Inhalt bei deaktivierter Anforderungs Validierung in HTML-Code codiert werden.</span><span class="sxs-lookup"><span data-stu-id="49da0-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="49da0-138">HTML-codieren von Inhalten</span><span class="sxs-lookup"><span data-stu-id="49da0-138">How to HTML encode content</span></span>

<span data-ttu-id="49da0-139">Wenn Sie die Anforderungs Validierung deaktiviert haben, empfiehlt es sich, Inhalte zu codieren, die für die zukünftige Verwendung gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="49da0-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="49da0-140">Bei der HTML-Codierung werden alle '&lt;' oder '&gt;' (in Verbindung mit mehreren anderen Symbolen) automatisch durch die entsprechende HTML-codierte Darstellung ersetzt.</span><span class="sxs-lookup"><span data-stu-id="49da0-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="49da0-141">Beispielsweise wird '&lt;' durch '&amp;lt; ' ersetzt, und '&gt;' wird durch '&amp;gt; ' ersetzt.</span><span class="sxs-lookup"><span data-stu-id="49da0-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="49da0-142">Browser verwenden diese speziellen Codes, um "&lt;" oder "&gt;" im Browser anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="49da0-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="49da0-143">Der Inhalt kann auf dem Server auf einfache Weise auf dem Server mit der `Server.HtmlEncode(string)`-API codiert werden.</span><span class="sxs-lookup"><span data-stu-id="49da0-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="49da0-144">Der Inhalt kann auch einfach HTML-decodiert werden, d. h. mit der `Server.HtmlDecode(string)`-Methode auf Standard-HTML zurückgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="49da0-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="49da0-145">Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="49da0-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
