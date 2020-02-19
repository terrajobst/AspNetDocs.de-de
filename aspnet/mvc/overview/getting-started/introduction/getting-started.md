---
uid: mvc/overview/getting-started/introduction/getting-started
title: Einstieg in ASP.NET MVC 5 | Microsoft-Dokumentation
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: c74daa37f68dda641cae97d3b0c19718f62d474d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456386"
---
# <a name="getting-started-with-aspnet-mvc-5"></a>Einstieg in ASP.NET MVC 5

von [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC 5-Web-App mit [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Der endgültige Quellcode für das Tutorial befindet sich auf [GitHub](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Dieses Tutorial wurde von [Scott Guthrie](https://weblogs.asp.net/scottgu/) (Twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (Twitter: [@shanselman](https://twitter.com/shanselman) ) und [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ) verfasst.

Sie benötigen ein Azure-Konto, um diese APP in Azure bereitzustellen:

- Sie können [ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) . Sie erhalten Gutschriften, die Sie verwenden können, um kostenpflichtige Azure-Dienste zu testen. selbst nach ihrer Nutzung können Sie das Konto behalten und kostenlose Azure-Dienste nutzen.
- Sie können Ihre [Vorteile für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können.

## <a name="get-started"></a>Erste Schritte

Beginnen Sie mit der [Installation von Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Öffnen Sie dann Visual Studio.

Visual Studio ist eine IDE oder eine integrierte Entwicklungsumgebung. Ebenso wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE zum Erstellen von Anwendungen. In Visual Studio gibt es unten eine Liste mit verschiedenen Optionen, die Ihnen zur Verfügung stehen. Es gibt auch ein Menü, das eine weitere Möglichkeit bietet, Aufgaben in der IDE auszuführen. Anstatt auf der **Start Seite**auf " **Neues Projekt** " zu klicken, können Sie z. b. die Menüleiste verwenden und **Datei** > **Neues Projekt**auswählen.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Erstellen Ihrer ersten App

Wählen Sie auf der **Start Seite**die Option **Neues Projekt**aus. Wählen Sie im Dialogfeld **Neues Projekt** auf der linken Seite die Kategorie **Visualisierung C#**  und dann **Web**aus, und wählen Sie dann die Projektvorlage **ASP.NET-Webanwendung (.NET Framework)** aus. Nennen Sie das Projekt "mvcmovie", und wählen Sie dann **OK**aus.

![](getting-started/_static/image2.png)

Wählen Sie im Dialogfeld **New ASP.NET Webanwendung** die Option **MVC** aus, und klicken Sie dann auf **OK**.

![](getting-started/_static/image3.png)

Visual Studio hat eine Standardvorlage für das ASP.NET MVC-Projekt verwendet, das Sie gerade erstellt haben, sodass Sie momentan über eine funktionierende Anwendung verfügen, ohne etwas zu tun! Dies ist eine einfache "Hallo Welt!" Das Projekt ist ein guter Ausgangspunkt, um Ihre Anwendung zu starten.

![](getting-started/_static/image4.png)

Drücken Sie **F5**, um das Debuggen zu starten. Wenn Sie **F5**drücken, startet Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre Web-App aus. In Visual Studio wird dann ein Browser gestartet und die Startseite der Anwendung geöffnet. Beachten Sie, dass die Adressleiste des Browsers `localhost:port#` und nicht wie `example.com`anzeigt. Der Grund hierfür ist, dass `localhost` immer auf Ihren eigenen lokalen Computer verweist. in diesem Fall wird die Anwendung ausgeführt, die Sie soeben erstellt haben. Wenn Visual Studio ein Webprojekt ausführt, wird ein zufälliger Port für den Webserver verwendet. In der folgenden Abbildung ist die Portnummer 1234. Wenn Sie die Anwendung ausführen, wird eine andere Portnummer angezeigt.

![](getting-started/_static/image5.png)

Diese Standardvorlage ermöglicht Ihnen `Home`, `Contact`und `About` Seiten. In der folgenden Abbildung werden die Links **zu** **Start**, Info und **Kontakt** nicht angezeigt. Abhängig von der Größe Ihres Browserfensters müssen Sie möglicherweise auf das Navigations Symbol klicken, um diese Links anzuzeigen.

![](getting-started/_static/image6.png)

Die Anwendung bietet auch Unterstützung für die Registrierung und Anmeldung. Der nächste Schritt besteht darin, die Funktionsweise dieser Anwendung zu ändern und etwas über ASP.NET MVC zu erfahren. Schließen Sie die ASP.NET MVC-Anwendung, und ändern Sie den Code.

Eine Liste der aktuellen Tutorials finden Sie in den [empfohlenen MVC-Artikeln](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Diese APP wird in Azure ausgeführt.

Möchten Sie sehen, dass die fertige Site als Live-Web-App ausgeführt wird? Sie können eine vollständige Version der APP für Ihr Azure-Konto bereitstellen, indem Sie einfach auf die folgende Schaltfläche klicken.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Sie benötigen ein Azure-Konto, um diese Lösung in Azure bereitzustellen. Wenn Sie noch nicht über ein Konto verfügen, verwenden Sie eine der folgenden Optionen, um eines zu erstellen:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) . Sie erhalten Gutschriften, die Sie verwenden können, um kostenpflichtige Azure-Dienste zu testen. selbst nach ihrer Nutzung können Sie das Konto behalten und kostenlose Azure-Dienste nutzen.
- [Aktivieren von Visual Studio-Abonnenten Vorteilen](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) : Ihr Visual Studio-Abonnement bietet Ihnen jeden Monat ein Guthaben, das Sie für kostenpflichtige Azure-Dienste nutzen können.

> [!div class="step-by-step"]
> [Weiter](adding-a-controller.md)
