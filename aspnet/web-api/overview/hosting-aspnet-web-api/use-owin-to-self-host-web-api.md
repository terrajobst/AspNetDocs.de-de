---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Verwenden von owin für die Self-Host-ASP.net-Web-API-ASP.NET 4. x
author: rick-anderson
description: Tutorial mit Code, in dem gezeigt wird, wie ASP.net-Web-API in einer Konsolenanwendung gehostet wird.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448329"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a>Verwenden von owin für Self-Host-ASP.net-Web-API 

> In diesem Tutorial wird gezeigt, wie Sie ASP.net-Web-API in einer Konsolenanwendung hosten, indem Sie owin zum Selbsthosten des Web-API-Frameworks verwenden.
>
> [Open Web Interface for .net](http://owin.org) (owin) definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen. Owin entkoppelt die Webanwendung vom Server, wodurch owin ideal für das selbst Hosting einer Webanwendung in Ihrem eigenen Prozess außerhalb von IIS geeignet ist.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - Web-API 5.2.7

> [!NOTE]
> Den gesamten Quellcode für dieses Tutorial finden Sie unter [GitHub.com/ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).

## <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Klicken Sie im Menü **Datei** auf **neu**, und wählen Sie dann **Projekt**aus. Wählen **Sie unter** **C#Visual**den **Windows-Desktop** aus, und wählen Sie dann **Konsolen-app (.NET Framework)** aus. Nennen Sie das Projekt "owinselfhostsample", und wählen Sie **OK**aus.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Hinzufügen der Web-API und der owin-Pakete

Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Dadurch wird das WebAPI owin SelfHost-Paket und alle erforderlichen owin-Pakete installiert.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Web-API für Self-Host konfigurieren

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie / **Klasse** **Hinzufügen** , um eine neue Klasse hinzuzufügen. Geben Sie der Klassen den Namen `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Ersetzen Sie den gesamten Code in dieser Datei durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Hinzufügen eines Web-API-Controllers

Fügen Sie als nächstes eine Web-API-Controller Klasse hinzu. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie / **Klasse** **Hinzufügen** , um eine neue Klasse hinzuzufügen. Geben Sie der Klassen den Namen `ValuesController`.

Ersetzen Sie den gesamten Code in dieser Datei durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Starten Sie den owin-Host, und stellen Sie eine Anforderung mit httpclient her.

Ersetzen Sie den gesamten Code in der Program.cs-Datei durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Drücken Sie in Visual Studio F5, um die Anwendung auszuführen. Die Ausgabe sollte wie folgt aussehen:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Übersicht über das Katana-Projekt](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hosten von ASP.net-Web-API in einer Azure-workerrolle](host-aspnet-web-api-in-an-azure-worker-role.md)
