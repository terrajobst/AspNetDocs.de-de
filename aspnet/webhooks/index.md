---
uid: webhooks/index
title: Übersicht über ASP.net webhooks | Microsoft-Dokumentation
author: rick-anderson
description: Eine Einführung in ASP.net webhooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: aa65a20e1af16d58533e37fafc77ac246e0fe327
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000733"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="2c6bc-103">Übersicht über ASP.net webhooks</span><span class="sxs-lookup"><span data-stu-id="2c6bc-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="2c6bc-104">Webhooks ist ein einfaches http-Muster, das ein einfaches Pub/Sub-Modell zum Verbinden von Web-APIs und Saas-Diensten bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="2c6bc-105">Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung in Form einer HTTP POST-Anforderung an registrierte Abonnenten gesendet.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="2c6bc-106">Die Post-Anforderung enthält Informationen zum Ereignis, das es dem Empfänger ermöglicht, entsprechend zu agieren.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="2c6bc-107">Aus Gründen der Einfachheit sind webhooks bereits durch eine große Anzahl von Diensten verfügbar, darunter [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [trello](http://www.trello.com/)und viele. Weitere.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="2c6bc-108">Ein webhook kann z. b. angeben, dass eine Datei in [Dropbox](http://dropbox.com/)geändert wurde oder für eine Codeänderung in GitHub ein Commit ausgeführt wurde oder eine Zahlung in [PayPal](http://www.paypal.com/)initiiert wurde oder eine Karte in [trello](http://www.trello.com/)erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="2c6bc-109">Die Möglichkeiten sind unendlich.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-109">The possibilities are endless!</span></span>

<span data-ttu-id="2c6bc-110">Microsoft ASP.net webhooks erleichtert das Senden und empfangen von webhooks als Teil Ihrer ASP.NET-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="2c6bc-111">Auf der Empfangsseite wird ein gängiges Modell zum empfangen und Verarbeiten von webhooks von einer beliebigen Anzahl von webhostinganbietern bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="2c6bc-112">Es ist standardmäßig mit Unterstützung für [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) und [Zendesk](https://www.zendesk.com/) , aber es ist einfach, Unterstützung für weitere Funktionen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="2c6bc-113">Auf der sendenden Seite bietet Sie Unterstützung für das Verwalten und Speichern von Abonnements sowie das Senden von Ereignis Benachrichtigungen an die richtige Gruppe von Abonnenten.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="2c6bc-114">Dies ermöglicht es Ihnen, einen eigenen Satz von Ereignissen zu definieren, den Abonnenten abonnieren können, und diese Benachrichtigen, wenn ein Problem auftritt.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="2c6bc-115">Die beiden Teile können abhängig von Ihrem Szenario unabhängig voneinander verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="2c6bc-116">Wenn Sie nur webhooks von anderen Diensten empfangen müssen, können Sie nur den Empfänger Teil verwenden. Wenn Sie für andere Benutzer nur webhooks verfügbar machen möchten, können Sie dies tun.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="2c6bc-117">Der Code zielt auf ASP.net-Web-API 2 und ASP.NET MVC 5 ab und steht als [OSS auf GitHub](https://github.com/aspnet/WebHooks)zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="2c6bc-118">Übersicht über webhooks</span><span class="sxs-lookup"><span data-stu-id="2c6bc-118">WebHooks Overview</span></span>

<span data-ttu-id="2c6bc-119">Webhooks ist ein Muster, das bedeutet, dass es sich auf die Verwendung von Service to Service, aber die grundlegende Idee unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="2c6bc-120">Sie können sich webhooks als einfaches Pub/Sub-Modell vorstellen, in dem ein Benutzer Ereignisse abonnieren kann, die an anderer Stelle ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="2c6bc-121">Die Ereignis Benachrichtigungen werden als HTTP POST-Anforderungen weitergegeben, die Informationen über das Ereignis selbst enthalten.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="2c6bc-122">In der Regel enthält die HTTP POST-Anforderung ein JSON-Objekt oder HTML-Formulardaten, die vom Absender des webhooks bestimmt werden, einschließlich Informationen zu dem Ereignis, das den webhook auslöst.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="2c6bc-123">Beispielsweise sieht ein webhook-Post-Anforderungs Text von [GitHub](http://www.github.com/) wie folgt aus, weil ein neues Problem in einem bestimmten Repository geöffnet wird:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-123">For example, a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="2c6bc-124">Um sicherzustellen, dass der webhook tatsächlich vom vorgesehenen Absender ist, wird die Post-Anforderung auf irgendeine Weise gesichert und dann vom Empfänger überprüft.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="2c6bc-125">[GitHub-webhooks](https://developer.github.com/webhooks/) enthalten beispielsweise einen *X-Hub-Signature-http-* Header mit einem Hash des Anforderungs Texts, der von der Empfänger Implementierung geprüft wird, damit Sie sich keine Gedanken machen müssen.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="2c6bc-126">Der webhook-Flow wird in der Regel in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="2c6bc-127">Der Absender des webhooks macht Ereignisse verfügbar, die von einem Client abonniert werden können.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="2c6bc-128">Die Ereignisse beschreiben Observable-Änderungen am System, z. b. Wenn ein neues Datenelement eingefügt, ein Prozess abgeschlossen wurde oder etwas anderes.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="2c6bc-129">Der webhook-Empfänger abonniert durch Registrieren eines webhooks, der aus vier Dingen besteht:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="2c6bc-130">Ein URI für den Speicherort, an dem die Ereignis Benachrichtigung in Form einer HTTP POST-Anforderung gepostet werden soll.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="2c6bc-131">Eine Reihe von Filtern, die die spezifischen Ereignisse beschreiben, für die der webhook ausgelöst werden soll.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="2c6bc-132">Ein geheimer Schlüssel, der zum Signieren der HTTP POST-Anforderung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="2c6bc-133">Zusätzliche Daten, die in die HTTP POST-Anforderung eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="2c6bc-134">Dies kann z. b. zusätzliche HTTP-Header Felder oder-Eigenschaften sein, die im Text der HTTP POST-Anforderung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="2c6bc-135">Sobald ein Ereignis eintritt, werden die übereinstimmenden webhook-Registrierungen gefunden und HTTP POST-Anforderungen übermittelt.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="2c6bc-136">In der Regel wird die Generierung der HTTP POST-Anforderungen mehrmals wiederholt, wenn der Empfänger aus irgendeinem Grund nicht antwortet oder die HTTP POST-Anforderung zu einer Fehler Antwort führt.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="2c6bc-137">Webhooks-Verarbeitungs Pipeline</span><span class="sxs-lookup"><span data-stu-id="2c6bc-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="2c6bc-138">Die Microsoft ASP.net webhooks-Verarbeitungs Pipeline für eingehende webhooks sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.net webhooks-Verarbeitungs Pipeline](_static/WebHookReceivers.png)

<span data-ttu-id="2c6bc-140">Dabei handelt es sich um die beiden wichtigsten Konzepte von *Empfängern* und *Handlern*:</span><span class="sxs-lookup"><span data-stu-id="2c6bc-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="2c6bc-141">*Empfänger* sind dafür verantwortlich, die besondere webhook-Konfiguration von einem bestimmten Absender aus zu verarbeiten und Sicherheitsüberprüfungen zu erzwingen, um sicherzustellen, dass die webhook-Anforderung tatsächlich von dem vorgesehenen Absender abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="2c6bc-142">*Handler* werden in der Regel von Benutzercode ausgeführt, der den jeweiligen webhook verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="2c6bc-143">In den folgenden Knoten werden diese Konzepte ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="2c6bc-143">In the following nodes these concepts are described in more details.</span></span>
