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
# <a name="aspnet-webhooks-overview"></a>Übersicht über ASP.net webhooks

Webhooks ist ein einfaches http-Muster, das ein einfaches Pub/Sub-Modell zum Verbinden von Web-APIs und Saas-Diensten bereitstellt. Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung in Form einer HTTP POST-Anforderung an registrierte Abonnenten gesendet. Die Post-Anforderung enthält Informationen zum Ereignis, das es dem Empfänger ermöglicht, entsprechend zu agieren.

Aus Gründen der Einfachheit sind webhooks bereits durch eine große Anzahl von Diensten verfügbar, darunter [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [trello](http://www.trello.com/)und viele. Weitere. Ein webhook kann z. b. angeben, dass eine Datei in [Dropbox](http://dropbox.com/)geändert wurde oder für eine Codeänderung in GitHub ein Commit ausgeführt wurde oder eine Zahlung in [PayPal](http://www.paypal.com/)initiiert wurde oder eine Karte in [trello](http://www.trello.com/)erstellt wurde. Die Möglichkeiten sind unendlich.

Microsoft ASP.net webhooks erleichtert das Senden und empfangen von webhooks als Teil Ihrer ASP.NET-Anwendung:

* Auf der Empfangsseite wird ein gängiges Modell zum empfangen und Verarbeiten von webhooks von einer beliebigen Anzahl von webhostinganbietern bereitstellt. Es ist standardmäßig mit Unterstützung für [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) und [Zendesk](https://www.zendesk.com/) , aber es ist einfach, Unterstützung für weitere Funktionen hinzuzufügen.

* Auf der sendenden Seite bietet Sie Unterstützung für das Verwalten und Speichern von Abonnements sowie das Senden von Ereignis Benachrichtigungen an die richtige Gruppe von Abonnenten. Dies ermöglicht es Ihnen, einen eigenen Satz von Ereignissen zu definieren, den Abonnenten abonnieren können, und diese Benachrichtigen, wenn ein Problem auftritt.

Die beiden Teile können abhängig von Ihrem Szenario unabhängig voneinander verwendet werden. Wenn Sie nur webhooks von anderen Diensten empfangen müssen, können Sie nur den Empfänger Teil verwenden. Wenn Sie für andere Benutzer nur webhooks verfügbar machen möchten, können Sie dies tun.

Der Code zielt auf ASP.net-Web-API 2 und ASP.NET MVC 5 ab und steht als [OSS auf GitHub](https://github.com/aspnet/WebHooks)zur Verfügung.

## <a name="webhooks-overview"></a>Übersicht über webhooks

Webhooks ist ein Muster, das bedeutet, dass es sich auf die Verwendung von Service to Service, aber die grundlegende Idee unterscheidet. Sie können sich webhooks als einfaches Pub/Sub-Modell vorstellen, in dem ein Benutzer Ereignisse abonnieren kann, die an anderer Stelle ausgeführt werden. Die Ereignis Benachrichtigungen werden als HTTP POST-Anforderungen weitergegeben, die Informationen über das Ereignis selbst enthalten.

In der Regel enthält die HTTP POST-Anforderung ein JSON-Objekt oder HTML-Formulardaten, die vom Absender des webhooks bestimmt werden, einschließlich Informationen zu dem Ereignis, das den webhook auslöst. Beispielsweise sieht ein webhook-Post-Anforderungs Text von [GitHub](http://www.github.com/) wie folgt aus, weil ein neues Problem in einem bestimmten Repository geöffnet wird:

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

Um sicherzustellen, dass der webhook tatsächlich vom vorgesehenen Absender ist, wird die Post-Anforderung auf irgendeine Weise gesichert und dann vom Empfänger überprüft. [GitHub-webhooks](https://developer.github.com/webhooks/) enthalten beispielsweise einen *X-Hub-Signature-http-* Header mit einem Hash des Anforderungs Texts, der von der Empfänger Implementierung geprüft wird, damit Sie sich keine Gedanken machen müssen.

Der webhook-Flow wird in der Regel in etwa wie folgt aussehen:

* Der Absender des webhooks macht Ereignisse verfügbar, die von einem Client abonniert werden können. Die Ereignisse beschreiben Observable-Änderungen am System, z. b. Wenn ein neues Datenelement eingefügt, ein Prozess abgeschlossen wurde oder etwas anderes.

* Der webhook-Empfänger abonniert durch Registrieren eines webhooks, der aus vier Dingen besteht:

     1. Ein URI für den Speicherort, an dem die Ereignis Benachrichtigung in Form einer HTTP POST-Anforderung gepostet werden soll.

     2. Eine Reihe von Filtern, die die spezifischen Ereignisse beschreiben, für die der webhook ausgelöst werden soll.

     3. Ein geheimer Schlüssel, der zum Signieren der HTTP POST-Anforderung verwendet wird.

     4. Zusätzliche Daten, die in die HTTP POST-Anforderung eingeschlossen werden sollen. Dies kann z. b. zusätzliche HTTP-Header Felder oder-Eigenschaften sein, die im Text der HTTP POST-Anforderung enthalten sind.

* Sobald ein Ereignis eintritt, werden die übereinstimmenden webhook-Registrierungen gefunden und HTTP POST-Anforderungen übermittelt. In der Regel wird die Generierung der HTTP POST-Anforderungen mehrmals wiederholt, wenn der Empfänger aus irgendeinem Grund nicht antwortet oder die HTTP POST-Anforderung zu einer Fehler Antwort führt.

## <a name="webhooks-processing-pipeline"></a>Webhooks-Verarbeitungs Pipeline

Die Microsoft ASP.net webhooks-Verarbeitungs Pipeline für eingehende webhooks sieht wie folgt aus:

![ASP.net webhooks-Verarbeitungs Pipeline](_static/WebHookReceivers.png)

Dabei handelt es sich um die beiden wichtigsten Konzepte von *Empfängern* und *Handlern*:

* *Empfänger* sind dafür verantwortlich, die besondere webhook-Konfiguration von einem bestimmten Absender aus zu verarbeiten und Sicherheitsüberprüfungen zu erzwingen, um sicherzustellen, dass die webhook-Anforderung tatsächlich von dem vorgesehenen Absender abgeleitet ist.

* *Handler* werden in der Regel von Benutzercode ausgeführt, der den jeweiligen webhook verarbeitet.

In den folgenden Knoten werden diese Konzepte ausführlicher beschrieben.
