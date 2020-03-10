---
uid: webhooks/receiving/receivers
title: ASP.net webhooks-Empfänger | Microsoft-Dokumentation
author: rick-anderson
description: ASP.net-webhooks-Empfänger
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: d771a588b23abcd7b1b33e694af17b219683fc48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463461"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="e42f7-103">ASP.net-webhooks-Empfänger</span><span class="sxs-lookup"><span data-stu-id="e42f7-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="e42f7-104">Das Empfangen von webhooks hängt davon ab, wer der Absender ist.</span><span class="sxs-lookup"><span data-stu-id="e42f7-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="e42f7-105">Manchmal gibt es zusätzliche Schritte zum Registrieren eines webhooks, bei dem überprüft wird, ob der Abonnent wirklich lauscht.</span><span class="sxs-lookup"><span data-stu-id="e42f7-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="e42f7-106">Einige webhooks bieten ein Push-to-Pull-Modell, bei dem die HTTP POST-Anforderung nur einen Verweis auf die Ereignis Informationen enthält, die dann unabhängig voneinander abgerufen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e42f7-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="e42f7-107">Häufig variiert das Sicherheitsmodell erheblich.</span><span class="sxs-lookup"><span data-stu-id="e42f7-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="e42f7-108">Der Zweck Microsoft ASP.net webhooks besteht darin, die API einfacher und konfiter zu gestalten, ohne viel Zeit dafür zu aufwenden, wie eine bestimmte Variante von webhooks behandelt werden muss.</span><span class="sxs-lookup"><span data-stu-id="e42f7-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="e42f7-109">Ein webhook-Empfänger ist für das akzeptieren und Überprüfen von webhooks von einem bestimmten Absender verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="e42f7-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="e42f7-110">Ein webhook-Empfänger kann eine beliebige Anzahl von webhooks unterstützen, die jeweils über eine eigene Konfiguration verfügen.</span><span class="sxs-lookup"><span data-stu-id="e42f7-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="e42f7-111">Beispielsweise kann der GitHub-webhook-Empfänger webhooks aus einer beliebigen Anzahl von GitHub-Repository akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="e42f7-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="e42f7-112">Webhook-Empfänger-URIs</span><span class="sxs-lookup"><span data-stu-id="e42f7-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="e42f7-113">Wenn Sie Microsoft ASP.net webhooks installieren, erhalten Sie einen allgemeinen webhook-Controller, der webhook-Anforderungen von einer offen enden Anzahl von Diensten akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="e42f7-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="e42f7-114">Wenn eine Anforderung eingeht, wird der entsprechende Empfänger ausgewählt, den Sie für die Verarbeitung eines bestimmten webhook-Absenders installiert haben.</span><span class="sxs-lookup"><span data-stu-id="e42f7-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="e42f7-115">Der URI dieses Controllers ist der webhook-URI, den Sie mit dem Dienst registrieren, und hat folgendes Format:</span><span class="sxs-lookup"><span data-stu-id="e42f7-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="e42f7-116">Aus Sicherheitsgründen erfordern viele webhook-Empfänger, dass der URI ein *https* -URI ist, und in einigen Fällen muss Sie auch einen zusätzlichen Abfrage Parameter enthalten, der verwendet wird, um zu erzwingen, dass nur die beabsichtigte Partei webhooks an den oben genannten URI senden kann.</span><span class="sxs-lookup"><span data-stu-id="e42f7-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="e42f7-117">Die `<receiver>` Komponente ist der Name des Empfängers, z. b. `github` oder `slack`.</span><span class="sxs-lookup"><span data-stu-id="e42f7-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="e42f7-118">" *{ID}* " ist ein optionaler Bezeichner, der zum Identifizieren einer bestimmten webhook-Empfänger Konfiguration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="e42f7-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="e42f7-119">Dies kann zum Registrieren von N-webhooks bei einem bestimmten Empfänger verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e42f7-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="e42f7-120">Beispielsweise können die folgenden drei URIs verwendet werden, um sich für drei unabhängige webhooks zu registrieren:</span><span class="sxs-lookup"><span data-stu-id="e42f7-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="e42f7-121">Installieren eines webhook-Empfängers</span><span class="sxs-lookup"><span data-stu-id="e42f7-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="e42f7-122">Zum Empfangen von webhooks mithilfe von Microsoft ASP.net webhooks installieren Sie zuerst das nuget-Paket für den Webhostinganbieter oder die Anbieter, von denen Sie webhooks erhalten möchten.</span><span class="sxs-lookup"><span data-stu-id="e42f7-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="e42f7-123">Die nuget-Pakete heißen " [Microsoft. Aspnet. webhooks. Receiver. \*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) ", wobei der letzte Teil den unterstützten Dienst angibt.</span><span class="sxs-lookup"><span data-stu-id="e42f7-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="e42f7-124">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e42f7-124">For example</span></span>

<span data-ttu-id="e42f7-125">[Microsoft. Aspnet. webhooks. Empfängers. GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) bietet Unterstützung für den Empfang von webhooks von GitHub und [Microsoft. Aspnet. webhooks. Empfängers. Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) bietet Unterstützung für den Empfang von webhooks, die von ASP.net-webhooks generiert werden.</span><span class="sxs-lookup"><span data-stu-id="e42f7-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="e42f7-126">Standardmäßig können Sie Unterstützung für Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, trello und WordPress finden, aber es ist möglich, eine beliebige Anzahl anderer Anbieter zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e42f7-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="e42f7-127">Konfigurieren eines webhook-Empfängers</span><span class="sxs-lookup"><span data-stu-id="e42f7-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="e42f7-128">Webhook-Empfänger werden über die ganze Zahl [iwebhostokreceiverconfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) konfiguriert, und bestimmte Implementierungen dieser Schnittstelle können mithilfe eines beliebigen Modells für die Abhängigkeitsinjektion registriert werden.</span><span class="sxs-lookup"><span data-stu-id="e42f7-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="e42f7-129">Die Standard Implementierung verwendet Anwendungseinstellungen, die entweder in der Datei "Web. config" festgelegt werden können, oder wenn Sie Azure-Web-Apps verwenden, kann über das [Azure-Portal](https://portal.azure.com/)festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="e42f7-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![App-Einstellungen in Azure](_static/AzureAppSettings.png)

<span data-ttu-id="e42f7-131">Das Format für Anwendungs Einstellungs Schlüssel lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="e42f7-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="e42f7-132">Der Wert ist eine durch Trennzeichen getrennte Liste von Werten, die den *{ID}* -Werten entsprechen, für die webhooks registriert wurden, z. b.:</span><span class="sxs-lookup"><span data-stu-id="e42f7-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="e42f7-133">Initialisieren eines webhook-Empfängers</span><span class="sxs-lookup"><span data-stu-id="e42f7-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="e42f7-134">Webhook-Empfänger werden initialisiert, indem Sie registriert werden, in der Regel in der statischen *webapiconfig* -Klasse, z. b.:</span><span class="sxs-lookup"><span data-stu-id="e42f7-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
