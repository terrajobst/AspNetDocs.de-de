---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Einführung in ASP.NET MVC 3 (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 24f7de303ef7f5a457bd509ecc6bd0e3be7e3d9d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456320"
---
# <a name="intro-to-aspnet-mvc-3-vb"></a>Einführung zu ASP.NET MVC 3 (VB)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:
> 
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit VB.NET-Quellcode ist für dieses Thema verfügbar. [Laden Sie die VB.NET-Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wechseln Sie C#ggf. zur [ C# Version](../cs/intro-to-aspnet-mvc-3.md) dieses Tutorials.

Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:

- [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)

Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Ein Visual Web Developer-Projekt mit VB-Quellcode ist für dieses Thema verfügbar. [Laden Sie die VB-Version hier herunter](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Wenn Sie CSharp bevorzugen, wechseln Sie zur [CSharp-Version](../cs/intro-to-aspnet-mvc-3.md) dieses Tutorials.

## <a name="what-youll-build"></a>Sie lernen Folgendes

Sie implementieren eine einfache Movie-Listing-Anwendung, die das Erstellen, bearbeiten und Auflisten von Filmen aus einer Datenbank unterstützt. Im folgenden finden Sie zwei Screenshots der Anwendung, die Sie erstellen. Sie enthält eine Seite, auf der eine Liste der Filme aus einer Datenbank angezeigt wird:

[![von "movieswithvarioussm"](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Mithilfe der Anwendung können Sie außerdem Filme hinzufügen, bearbeiten und löschen sowie Details zu einzelnen Personen anzeigen. Alle Dateneingabe Szenarien umfassen die Validierung, um sicherzustellen, dass die in der Datenbank gespeicherten Daten korrekt sind.

[!["kreateformso"](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Erlernte Fertigkeiten

Folgendes können Sie lernen:

- Erstellen eines neuen ASP.NET MVC-Projekts
- Erstellen einer neuen Datenbank mit Entity Framework Code First
- Erstellen von ASP.NET-MVC-Controllern und-Ansichten
- Abrufen und Anzeigen von Daten
- Vorgehensweise beim Bearbeiten von Daten und Aktivieren der Datenvalidierung

## <a name="getting-started"></a>Erste Schritte

Führen Sie zunächst Visual Web Developer 2010 Express ("VWD" für kurze Zeit) aus, und wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus.

Visual Web Developer ist eine IDE oder eine integrierte Entwicklungsumgebung. Ebenso wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE zum Erstellen von Anwendungen. In Visual Web Developer gibt es eine Symbolleiste am oberen Rand der verschiedenen Optionen, die Ihnen zur Verfügung stehen. Es gibt auch ein Menü, das eine weitere Möglichkeit bietet, Aufgaben in der IDE auszuführen. (Wenn Sie z. b. auf der **Start** Seite die Option **Neues Projekt** auswählen, können Sie das Menü verwenden und **Datei** &gt; **Neues Projekt**auswählen.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Sie können Anwendungen erstellen, indem Sie entweder Visual Basic oder Visual C# als Programmiersprache verwenden. Wählen Sie auf der linken Seite Visual Basic aus, und wählen Sie dann **ASP.NET MVC 3-Webanwendung**aus. Nennen Sie das Projekt "mvcmovie", und klicken Sie dann auf **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

Wählen Sie im Dialogfeld **Neues ASP.NET MVC 3-Projekt** die Option **Internet Anwendung**aus. Belassen Sie **Razor** als Standard Ansichts-Engine.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Klicken Sie auf **OK**. Visual Web Developer hat eine Standardvorlage für das ASP.NET MVC-Projekt verwendet, das Sie gerade erstellt haben, sodass Sie momentan über eine funktionierende Anwendung verfügen, ohne etwas zu tun! Dies ist eine einfache "Hallo Welt!" Das Projekt ist ein guter Ausgangspunkt, um Ihre Anwendung zu starten.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Wählen Sie im Menü **Debuggen** die Option **Debugging starten**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Beachten Sie, dass die Tastenkombination zum Starten des Debuggens F5 ist.

F5 bewirkt, dass Visual Web Developer einen entwicklungsweb Server startet und die Webanwendung ausgeführt wird. Anschließend wird von vwd ein Browser gestartet und die Startseite der Anwendung geöffnet. Beachten Sie, dass die Adressleiste des Browsers `localhost` und nicht wie `example.com`anzeigt. Der Grund hierfür ist, dass `localhost` immer auf Ihren eigenen lokalen Computer verweist. in diesem Fall wird die Anwendung ausgeführt, die Sie soeben erstellt haben. Wenn VWD ein Webprojekt ausführt, wird ein zufälliger Port für das Projekt verwendet. In der folgenden Abbildung ist die zufällige Portnummer 43246. In Ihrem Projekt wird wahrscheinlich eine andere Portnummer verwendet.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Diese Standardvorlage enthält standardmäßig zwei zu besuchende Seiten und eine einfache Anmeldeseite. Ändern Sie die Funktionsweise dieser Anwendung, und erfahren Sie etwas über ASP.NET MVC im Prozess. Schließen Sie Ihren Browser, und ändern Sie den Code.

> [!div class="step-by-step"]
> [Weiter](adding-a-controller.md)
