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
# <a name="aspnet-webhooks-receivers"></a>ASP.net-webhooks-Empfänger

Das Empfangen von webhooks hängt davon ab, wer der Absender ist. Manchmal gibt es zusätzliche Schritte zum Registrieren eines webhooks, bei dem überprüft wird, ob der Abonnent wirklich lauscht. Einige webhooks bieten ein Push-to-Pull-Modell, bei dem die HTTP POST-Anforderung nur einen Verweis auf die Ereignis Informationen enthält, die dann unabhängig voneinander abgerufen werden sollen. Häufig variiert das Sicherheitsmodell erheblich.

Der Zweck Microsoft ASP.net webhooks besteht darin, die API einfacher und konfiter zu gestalten, ohne viel Zeit dafür zu aufwenden, wie eine bestimmte Variante von webhooks behandelt werden muss.

Ein webhook-Empfänger ist für das akzeptieren und Überprüfen von webhooks von einem bestimmten Absender verantwortlich. Ein webhook-Empfänger kann eine beliebige Anzahl von webhooks unterstützen, die jeweils über eine eigene Konfiguration verfügen. Beispielsweise kann der GitHub-webhook-Empfänger webhooks aus einer beliebigen Anzahl von GitHub-Repository akzeptieren.

## <a name="webhook-receiver-uris"></a>Webhook-Empfänger-URIs

Wenn Sie Microsoft ASP.net webhooks installieren, erhalten Sie einen allgemeinen webhook-Controller, der webhook-Anforderungen von einer offen enden Anzahl von Diensten akzeptiert. Wenn eine Anforderung eingeht, wird der entsprechende Empfänger ausgewählt, den Sie für die Verarbeitung eines bestimmten webhook-Absenders installiert haben.

Der URI dieses Controllers ist der webhook-URI, den Sie mit dem Dienst registrieren, und hat folgendes Format:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Aus Sicherheitsgründen erfordern viele webhook-Empfänger, dass der URI ein *https* -URI ist, und in einigen Fällen muss Sie auch einen zusätzlichen Abfrage Parameter enthalten, der verwendet wird, um zu erzwingen, dass nur die beabsichtigte Partei webhooks an den oben genannten URI senden kann.

Die `<receiver>` Komponente ist der Name des Empfängers, z. b. `github` oder `slack`.

" *{ID}* " ist ein optionaler Bezeichner, der zum Identifizieren einer bestimmten webhook-Empfänger Konfiguration verwendet werden kann. Dies kann zum Registrieren von N-webhooks bei einem bestimmten Empfänger verwendet werden. Beispielsweise können die folgenden drei URIs verwendet werden, um sich für drei unabhängige webhooks zu registrieren:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Installieren eines webhook-Empfängers

Zum Empfangen von webhooks mithilfe von Microsoft ASP.net webhooks installieren Sie zuerst das nuget-Paket für den Webhostinganbieter oder die Anbieter, von denen Sie webhooks erhalten möchten. Die nuget-Pakete heißen " [Microsoft. Aspnet. webhooks. Receiver. *](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) ", wobei der letzte Teil den unterstützten Dienst angibt. Beispiel:

[Microsoft. Aspnet. webhooks. Empfängers. GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) bietet Unterstützung für den Empfang von webhooks von GitHub und [Microsoft. Aspnet. webhooks. Empfängers. Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) bietet Unterstützung für den Empfang von webhooks, die von ASP.net-webhooks generiert werden.

Standardmäßig können Sie Unterstützung für Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, trello und WordPress finden, aber es ist möglich, eine beliebige Anzahl anderer Anbieter zu unterstützen.

## <a name="configuring-a-webhook-receiver"></a>Konfigurieren eines webhook-Empfängers

Webhook-Empfänger werden über die ganze Zahl [iwebhostokreceiverconfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) konfiguriert, und bestimmte Implementierungen dieser Schnittstelle können mithilfe eines beliebigen Modells für die Abhängigkeitsinjektion registriert werden. Die Standard Implementierung verwendet Anwendungseinstellungen, die entweder in der Datei "Web. config" festgelegt werden können, oder wenn Sie Azure-Web-Apps verwenden, kann über das [Azure-Portal](https://portal.azure.com/)festgelegt werden.

![App-Einstellungen in Azure](_static/AzureAppSettings.png)

Das Format für Anwendungs Einstellungs Schlüssel lautet wie folgt:

```
MS_WebHookReceiverSecret_<receiver>
```

Der Wert ist eine durch Trennzeichen getrennte Liste von Werten, die den *{ID}* -Werten entsprechen, für die webhooks registriert wurden, z. b.:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Initialisieren eines webhook-Empfängers

Webhook-Empfänger werden initialisiert, indem Sie registriert werden, in der Regel in der statischen *webapiconfig* -Klasse, z. b.:

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
