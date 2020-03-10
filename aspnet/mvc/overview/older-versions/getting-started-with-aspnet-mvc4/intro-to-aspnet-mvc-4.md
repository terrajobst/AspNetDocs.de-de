---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Einführung in ASP.NET MVC 4 | Microsoft-Dokumentation
author: Rick-Anderson
description: Eine aktualisierte Version, wenn dieses Tutorial mithilfe von Visual Studio 2013 verfügbar ist. Im neuen Tutorial wird ASP.NET MVC 5 verwendet, das viele Verbesserungen gegenüber t bietet...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51709a9c6ddb39b8fcd1cd94cd08d530a595825a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485277"
---
# <a name="intro-to-aspnet-mvc-4"></a>Einführung zu ASP.NET MVC 4

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Eine aktualisierte Version, wenn dieses [Tutorial mithilfe von](../../getting-started/introduction/getting-started.md) [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)verfügbar ist. Im neuen Tutorial wird ASP.NET MVC 5 verwendet, das in diesem Tutorial viele Verbesserungen bietet.
>
> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC 4-Webanwendung mithilfe von Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) oder Visual Web Developer 2010 Express Service Pack 1. Visual Studio 2012 wird empfohlen. Sie müssen nichts installieren, um das Tutorial abzuschließen. Wenn Sie Visual Studio 2010 verwenden, müssen Sie die folgenden Komponenten installieren. Sie können alle Komponenten installieren, indem Sie auf die folgenden Links klicken:
>
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [WPI-Installer für ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie das [WPI-Installationsprogramm für ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) und die [Voraussetzungen für Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack) .
>
> Für dieses Thema steht ein Visual C# Web Developer-Projekt mit Quellcode zur Verfügung. [Laden Sie C# die Version herunter](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> In diesem Tutorial führen Sie die Anwendung in Visual Studio aus. Sie können die Anwendung auch über das Internet verfügbar machen, indem Sie Sie für einen Hostinganbieter bereitstellen. Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem [kostenlosen Windows Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)an. Informationen zum Bereitstellen eines Visual Studio-Webprojekts auf einer Windows Azure-Website finden Sie unter [Erstellen und Bereitstellen einer ASP.NET-Website und SQL-Datenbank mit Visual Studio](https://docs.microsoft.com/dotnet/azure/). In diesem Tutorial wird auch erläutert, wie Sie Entity Framework Code First-Migrationen zum Bereitstellen Ihrer SQL Server-Datenbank in Windows Azure SQL-Datenbank (ehemals SQL Azure) verwenden.
>
> Dieses Tutorial wurde von Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ) verfasst.

## <a name="what-youll-build"></a>Sie lernen Folgendes

> [!NOTE]
> Eine aktualisierte Version, wenn dieses [Tutorial mithilfe von](../../getting-started/introduction/getting-started.md) [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)verfügbar ist. Im neuen Tutorial wird ASP.NET MVC 5 verwendet, das in diesem Tutorial viele Verbesserungen bietet.

Sie implementieren eine einfache Movie-Listing-Anwendung, die das Erstellen, bearbeiten, durchsuchen und Auflisten von Filmen aus einer Datenbank unterstützt. Im folgenden finden Sie zwei Screenshots der Anwendung, die Sie erstellen. Sie enthält eine Seite, auf der eine Liste der Filme aus einer Datenbank angezeigt wird:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Mithilfe der Anwendung können Sie außerdem Filme hinzufügen, bearbeiten und löschen sowie Details zu einzelnen Personen anzeigen. Alle Dateneingabe Szenarien umfassen die Validierung, um sicherzustellen, dass die in der Datenbank gespeicherten Daten korrekt sind.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Erste Schritte

Starten Sie, indem Sie Visual Studio Express 2012 oder Visual Web Developer 2010 Express ausführen. Die meisten Bildschirmfotos in dieser Reihe verwenden Visual Studio Express 2012, aber Sie können dieses Tutorial mit Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 oder Visual Web Developer 2010 Express durchführen. Wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus.

Visual Studio ist eine IDE oder eine integrierte Entwicklungsumgebung. Ebenso wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE zum Erstellen von Anwendungen. In Visual Studio gibt es eine Symbolleiste am oberen Rand der verschiedenen Optionen, die Ihnen zur Verfügung stehen. Es gibt auch ein Menü, das eine weitere Möglichkeit bietet, Aufgaben in der IDE auszuführen. (Wenn Sie z. b. auf der **Start** Seite die Option **Neues Projekt** auswählen, können Sie das Menü verwenden und **Datei** &gt; **Neues Projekt**auswählen.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Sie können Anwendungen erstellen, indem Sie entweder Visual Basic C# oder Visual als Programmiersprache verwenden. Wählen Sie C# auf der linken Seite Visualisierung aus, und wählen Sie dann **ASP.NET MVC 4-Webanwendung**. Benennen Sie Ihr Projekt &quot;mvcmovie&quot; und klicken Sie dann auf **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Option **Internet Anwendung**aus. Belassen Sie **Razor** als Standard Ansichts-Engine.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Klicken Sie auf **OK**. Visual Studio hat eine Standardvorlage für das ASP.NET MVC-Projekt verwendet, das Sie gerade erstellt haben, sodass Sie momentan über eine funktionierende Anwendung verfügen, ohne etwas zu tun! Dies ist eine einfache &quot;Hallo Welt!&quot; Projekt, und es ist ein guter Ausgangspunkt, um Ihre Anwendung zu starten.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Wählen Sie im Menü **Debuggen** die Option **Debugging starten**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Beachten Sie, dass die Tastenkombination zum Starten des Debuggens F5 ist.

F5 bewirkt, dass Visual Studio IIS Express startet und die Webanwendung ausgeführt wird. In Visual Studio wird dann ein Browser gestartet und die Startseite der Anwendung geöffnet. Beachten Sie, dass die Adressleiste des Browsers `localhost` und nicht wie `example.com`anzeigt. Der Grund hierfür ist, dass `localhost` immer auf Ihren eigenen lokalen Computer verweist. in diesem Fall wird die Anwendung ausgeführt, die Sie soeben erstellt haben. Wenn Visual Studio ein Webprojekt ausführt, wird ein zufälliger Port für den Webserver verwendet. In der folgenden Abbildung ist die Portnummer 41788. Wenn Sie die Anwendung ausführen, wird wahrscheinlich eine andere Portnummer angezeigt.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Mit dieser Standardvorlage können Sie direkt auf die Seiten "Home", "Contact" und "about" klicken. Es bietet auch Unterstützung für die Registrierung und Anmeldung sowie Links zu Facebook und Twitter. Der nächste Schritt besteht darin, die Funktionsweise dieser Anwendung zu ändern und etwas über ASP.NET MVC zu erfahren. Schließen Sie Ihren Browser, und ändern Sie den Code.

> [!div class="step-by-step"]
> [Weiter](adding-a-controller.md)
