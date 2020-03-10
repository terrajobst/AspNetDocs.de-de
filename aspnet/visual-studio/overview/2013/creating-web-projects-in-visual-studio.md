---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Erstellen von ASP.NET-Webprojekten in Visual Studio 2013 | Microsoft-Dokumentation
author: tdykstra
description: In diesem Thema werden die Optionen zum Erstellen von ASP.NET-Webprojekten in Visual Studio 2013 mit Update 3 erläutert. hier finden Sie einige der neuen Features für die Webentwicklung.
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447201"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Erstellen von ASP.NET-Webprojekten in Visual Studio 2013

von [Tom Dykstra](https://github.com/tdykstra)

> In diesem Thema werden die Optionen zum Erstellen von ASP.NET-Webprojekten in Visual Studio 2013 mit Update 3 erläutert.
> 
> Im folgenden finden Sie einige der neuen Features für die Webentwicklung im Vergleich zu früheren Versionen von Visual Studio:
> 
> - Eine einfache Benutzeroberfläche zum Erstellen von Projekten, die [Unterstützung für mehrere ASP.NET-Frameworks](#add) (Web Forms, MVC und die Web-API) bieten.
> - [ASP.net Identity](#indauth)ein neues ASP.NET-Mitgliedschaftssystem, das in allen ASP.NET-Frameworks identisch funktioniert und mit anderen Webhosting-Software als IIS funktioniert.
> - Verwendung von [Bootstrap](#bootstrap) , um reaktionsschnelle Design-und Designfunktionen bereitzustellen.
> - Neue Features für Web Forms, die nur für MVC angeboten werden, z. b. die [Automatische Testprojekt Erstellung](#testproj) und eine [Intranetsite-Vorlage](#winauth).
> 
> Weitere Informationen zum Erstellen von Webprojekten für Azure Cloud Services oder Azure Mobile Services finden Sie unter Erstellen von [Azure-Cloud Services und ASP.net](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) und [Erstellen einer Bestenlisten-App mit Azure Mobile Services .net](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)-Back-End.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Voraussetzungen

Dieser Artikel bezieht sich auf [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) , auf denen [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installiert ist.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Webanwendungs Projekte und Website Projekte

ASP.net bietet Ihnen die Wahl zwischen zwei Arten von Webprojekten: *Webanwendungs Projekte* und *Website Projekten*. Wir empfehlen Webanwendungs Projekte für die neue Entwicklung, und dieser Artikel bezieht sich nur auf Webanwendungs Projekte. Weitere Informationen finden Sie unter [Webanwendungs Projekte im Vergleich zu Website Projekten in Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) auf der MSDN-Website.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Übersicht über das Erstellen von Webanwendungs Projekten

Die folgenden Schritte zeigen, wie Sie ein Webprojekt erstellen:

1. Klicken Sie auf der **Start** Seite oder im Menü **Datei** auf **Neues Projekt** .
2. Klicken Sie im Dialogfeld **Neues Projekt** im linken Bereich auf **Web** und im mittleren Bereich auf **ASP.NET Webanwendung** .

    ![Dialogfeld "Neues Projekt"](creating-web-projects-in-visual-studio/_static/image1.png)

    Sie können im linken Bereich **Cloud** auswählen, um einen [Azure-clouddienst](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile Service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)oder [Azure webjob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs)zu erstellen. Diese Vorlagen werden in diesem Thema nicht behandelt.
3. Klicken Sie im rechten Bereich auf das Kontrollkästchen **Application Insights zu Projekt hinzufügen** , wenn Sie die Integritäts-und Verwendungs Überwachung für Ihre Anwendung verwenden möchten. Weitere Informationen finden Sie unter [Leistungsüberwachung in Webanwendungen](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Geben Sie Projekt **Name**, **Speicherort**und andere Optionen an, und klicken Sie dann auf **OK**.

    Das Dialogfeld **Neues ASP.net-Projekt** wird angezeigt.

    ![Dialogfeld "Neues Projekt"](creating-web-projects-in-visual-studio/_static/image2.png)
5. Klicken Sie auf eine Vorlage.

    ![Vorlage auswählen](creating-web-projects-in-visual-studio/_static/image3.png)
6. Wenn Sie Unterstützung für zusätzliche Frameworks hinzufügen möchten, die nicht in der Vorlage enthalten sind, klicken Sie auf das entsprechende Kontrollkästchen. (Im gezeigten Beispiel könnten Sie MVC und/oder die Web-API zu einem Web Forms Projekt hinzufügen.)

    ![Frameworks hinzufügen](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Wenn Sie ein Komponenten Testprojekt hinzufügen möchten, klicken Sie auf Komponenten **Tests hinzufügen**.

    ![Hinzufügen von Komponententests](creating-web-projects-in-visual-studio/_static/image5.png)
8. Wenn Sie eine andere Authentifizierungsmethode als die standardmäßig von der Vorlage bereitgestellte Authentifizierungsmethode wünschen, klicken Sie auf **Authentifizierung ändern**.

    ![Authentifizierungs Schaltfläche Konfigurieren](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Dialogfeld zur Authentifizierung konfigurieren](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Erstellen einer Web-App oder eines virtuellen Computers in Azure

Visual Studio enthält Funktionen, die die Arbeit mit Azure-Diensten für das Hosting von Webanwendungen erleichtern. Sie können z. b. alle folgenden Aktionen direkt aus der Visual Studio-IDE ausführen:

- Erstellen und verwalten Sie Web-Apps oder virtuelle Computer, die Ihre Anwendung über das Internet verfügbar machen.
- Anzeigen von Protokollen, die von der Anwendung erstellt wurden, während Sie in der Cloud ausgeführt wird.
- Wird im Debugmodus Remote ausgeführt, während die Anwendung in der Cloud ausgeführt wird.
- Anzeigen und Verwalten anderer Azure-Dienste wie SQL-Datenbanken.

Sie können [ein Azure-Konto erstellen](https://www.windowsazure.com/pricing/free-trial/) , das grundlegende Dienste wie z. b. Web-Apps kostenlos umfasst. Wenn Sie MSDN-Abonnent sind, können Sie die [Vorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) , mit denen Sie monatliche Gutschriften für zusätzliche Azure-Dienste erhalten. 

Standardmäßig können Sie im Dialogfeld **Neues ASP.net-Projekt** eine Web-App oder einen virtuellen Computer für ein neues Webprojekt erstellen. Wenn Sie keine neue Web-App oder einen neuen virtuellen Computer erstellen möchten, deaktivieren Sie das Kontrollkästchen **Host in der Cloud** .

![Erstellen von Remote Ressourcen](creating-web-projects-in-visual-studio/_static/image8.png)

Die Beschriftung des Kontrollkästchens kann **in der Cloud gehostet** werden oder **Remote Ressourcen erstellen**, und in beiden Fällen ist der Effekt identisch. Wenn Sie das Kontrollkästchen aktiviert lassen, erstellt Visual Studio standardmäßig eine Web-App in Azure App Service. Wenn Sie möchten, können Sie das Dropdown Feld verwenden, um die **virtuelle Maschine** zu ändern. Wenn Sie noch nicht bei Azure angemeldet sind, werden Sie zur Eingabe von Azure-Anmelde Informationen aufgefordert. Nachdem Sie sich angemeldet haben, können Sie mit einem Dialogfeld die Ressourcen konfigurieren, die von Visual Studio für Ihr Projekt erstellt werden. Die folgende Abbildung zeigt das Dialogfeld für eine Web-App. Wenn Sie einen virtuellen Computer erstellen, werden verschiedene Optionen angezeigt.

![Konfigurieren von Azure-App Einstellungen](creating-web-projects-in-visual-studio/_static/image9.png)

Weitere Informationen zur Verwendung dieses Prozesses zum Erstellen von Azure-Ressourcen finden Sie unter Erstellen von Azure [-Ressourcen mit Azure und ASP.net](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) und [Erstellen eines virtuellen Computers für eine Website mit Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Im restlichen Teil dieses Artikels finden Sie weitere Informationen zu den verfügbaren Vorlagen und deren Optionen. Der Artikel führt auch Bootstrap ein, das Layout-und Design-Framework, das in den Vorlagen verwendet wird.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 von Webprojekt Vorlagen

Visual Studio verwendet Vorlagen zum Erstellen von Webprojekten. Mit einer Projektvorlage können Dateien und Ordner im neuen Projekt erstellt, nuget-Pakete installiert und Beispielcode für eine rudimentäre Arbeits Anwendung bereitgestellt werden. Vorlagen implementieren die neuesten Webstandards und dienen dazu, bewährte Methoden für die Verwendung von ASP.NET-Technologien zu veranschaulichen und Ihnen einen schnellen Einstieg in die Erstellung Ihrer eigenen Anwendung zu verschaffen.

Visual Studio 2013 bietet die folgenden Optionen für Webprojekt Vorlagen für Projekte, die auf .NET 4,5 oder spätere Versionen von .NET Framework ausgerichtet sind:

- [Leere Vorlage](#empty)
- [Web Forms Vorlage](#wf)
- [MVC-Vorlage](#mvc)
- [Web-API-Vorlage](#webapi)
- [Vorlage für Einzelseiten Anwendungen](#spa)
- [Azure Mobile Service-Vorlage](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012-Vorlagen](#vs2012)

Sie können auch eine Visual Studio-Erweiterung installieren, die eine [Facebook-Vorlage](#facebook)bereitstellt.

Weitere Informationen zum Erstellen von Projekten für .NET 4 finden Sie unter [Visual Studio 2012-Vorlagen](#vs2012) weiter unten in diesem Thema.

Weitere Informationen zum Erstellen von ASP.NET-Anwendungen für mobile Clients finden Sie [unter Mobile Support in ASP.net](../../../mobile/overview.md).

<a id="empty"></a>
### <a name="empty-template"></a>Leere Vorlage

Mit der leeren Vorlage werden die minimalen minimalen Ordner und Dateien für eine ASP.net-Web-App bereitstellt, z. b. eine Projektdatei ( *. csproj* oder). *VBPROJ*) und eine *Web. config* -Datei. Sie können Unterstützung für Web Forms, MVC und/oder die Web-API hinzufügen, indem Sie die Kontrollkästchen unter **Ordner hinzufügen und Kern Verweise für:** verwenden.

Für die leere Vorlage sind keine Authentifizierungs Optionen verfügbar. Die Authentifizierungs Funktionalität wird in Beispielanwendungen implementiert, und die leere Vorlage erstellt keine Beispielanwendung.

<a id="wf"></a>
### <a name="web-forms-template"></a>Web Forms Vorlage

Das Web Forms-Framework bietet die folgenden Features, mit denen Sie schnell Websites erstellen können, die in der Benutzeroberfläche und den Datenzugriffs Features umfangreich sind:

- Ein WYSIWYG-Designer in Visual Studio.
- Server Steuerelemente, die HTML-Code darstellen und durch Festlegen von Eigenschaften und Stilen angepasst werden können.
- Eine umfangreiche Palette von Steuerelementen für den Datenzugriff und die Datenanzeige.
- Ein Ereignis Modell, das Ereignisse verfügbar macht, die Sie genauso programmieren können wie eine Client Anwendung wie WPF.
- Automatische Beibehaltung des Zustands (Daten) zwischen HTTP-Anforderungen.

Im Allgemeinen erfordert das Erstellen einer Web Forms Anwendung weniger Programmieraufwand als das Erstellen der gleichen Anwendung mithilfe des ASP.NET-MVC-Frameworks. Web Forms ist jedoch nicht nur für die schnelle Anwendungsentwicklung vorgesehen. Es gibt viele komplexe kommerzielle Anwendungen und Frameworks, die auf Web Forms basieren.

Da auf einer Web Forms Seite und den Steuerelementen auf der Seite automatisch ein Großteil des Markups generiert wird, das an den Browser gesendet wird, haben Sie nicht die fein abgestimmte Kontrolle über den HTML-Code, den ASP.NET MVC bietet. Das deklarative Modell zum Konfigurieren von Seiten und Steuerelementen minimiert die Menge an Code, den Sie schreiben müssen, blendet jedoch ein Teil des Verhaltens von HTML und http aus. Beispielsweise ist es nicht immer möglich, genau anzugeben, welches Markup von einem Steuerelement generiert werden kann.

Das Web Forms Framework eignet sich nicht so einfach wie ASP.NET MVC für Musterbasierte Entwicklungsverfahren, wie z. b. die [Test gesteuerte Entwicklung](http://en.wikipedia.org/wiki/Test-driven_development), die [Trennung von Anliegen](http://en.wikipedia.org/wiki/Separation_of_concerns), die [Inversion der Kontrolle](http://en.wikipedia.org/wiki/Inversion_of_control)und die [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection). Wenn Sie Code schreiben möchten, der auf diese Weise faktorisiert ist, können Sie Es ist nicht so automatisch wie das ASP.NET-MVC-Framework. Das [ASP.net Web Forms MVP](http://webformsmvp.com/) -Projekt zeigt einen Ansatz, der die Trennung von Anliegen und der Prüfbarkeit erleichtert und gleichzeitig die schnelle Entwicklung aufrecht erhält, die für die Bereitstellung von Web Forms entwickelt wurde Microsoft SharePoint basiert auf Web Forms MVP.

Die Web Forms Vorlage erstellt eine Beispiel Web Forms Anwendung, die [Bootstrap](#bootstrap) verwendet, um reaktionsschnelle Design-und Design Features bereitzustellen. Die folgende Abbildung zeigt die Startseite.

![Startseite der Web Forms Vorlagen-App](creating-web-projects-in-visual-studio/_static/image10.png)

Weitere Informationen zu Web Forms finden Sie unter [ASP.net Web Forms](https://asp.net/web-forms). Informationen dazu, was die Web Forms Vorlage für Sie erledigt, finden Sie unter [Erstellen einer grundlegenden Web Forms Anwendung mit Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC-Vorlage

ASP.NET MVC wurde entwickelt, um Musterbasierte Entwicklungsverfahren zu vereinfachen, wie z. b. die [Test gesteuerte Entwicklung](http://en.wikipedia.org/wiki/Test-driven_development), die [Trennung von](http://en.wikipedia.org/wiki/Separation_of_concerns)Aufgaben, die [Inversion der Kontrolle](http://en.wikipedia.org/wiki/Inversion_of_control)und die [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection). Das Framework fördert die Trennung der Geschäftslogik Schicht einer Webanwendung von der Darstellungs Schicht. Durch die Aufteilung der Anwendung in Modelle (M), Ansichten (V) und Controller (C) kann ASP.NET MVC das Verwalten von Komplexität in größeren Anwendungen vereinfachen.

Mit ASP.NET MVC arbeiten Sie mehr direkt mit HTML und HTTP als in Web Forms. Beispielsweise kann Web Forms den Status zwischen HTTP-Anforderungen automatisch beibehalten, aber Sie müssen explizit in MVC codieren. Der Vorteil des MVC-Modells besteht darin, dass es Ihnen ermöglicht, die genaue Kontrolle über das Verhalten Ihrer Anwendung und deren Verhalten in der Weboberfläche zu nehmen. Der Nachteil ist, dass Sie mehr Code schreiben müssen.

MVC wurde als erweiterbar konzipiert und bietet Leistungs Entwicklern die Möglichkeit, das Framework für Ihre Anwendungsanforderungen anzupassen. Außerdem ist der ASP.NET MVC-Quellcode unter einer OSI-Lizenz verfügbar.

Die MVC-Vorlage erstellt eine MVC 5-Beispielanwendung, die [Bootstrap](#bootstrap) verwendet, um reaktionsschnelle Design-und Design Features bereitzustellen. Die folgende Abbildung zeigt die Startseite.

![MVC-Beispielanwendung](creating-web-projects-in-visual-studio/_static/image11.png)

Weitere Informationen zu MVC finden Sie unter [ASP.NET MVC](https://asp.net/mvc). Weitere Informationen zum Auswählen der MVC 4-Vorlage finden Sie unter [Visual Studio 2012-Vorlagen](#vs2012) weiter unten in diesem Artikel.

<a id="webapi"></a>
### <a name="web-api-template"></a>Web-API-Vorlage

Die Web-API-Vorlage erstellt einen beispielweb Dienst, der auf der Web-API basiert, einschließlich API-Hilfeseiten, die auf MVC basieren.

ASP.net-Web-API ist ein Framework, das das Erstellen von http-Diensten erleichtert, die eine breite Palette von Clients erreichen, einschließlich Browsern und mobilen Geräten. ASP.net-Web-API ist eine ideale Plattform zum Aufbauen von Rest-Diensten auf dem .NET Framework.

Die Web-API-Vorlage erstellt einen beispielweb Dienst. Die folgenden Abbildungen zeigen Beispiel Hilfeseiten.

![Web-API-Hilfeseite](creating-web-projects-in-visual-studio/_static/image12.png)

![Web-API-Hilfeseite für Get-API](creating-web-projects-in-visual-studio/_static/image13.png)

Weitere Informationen zur Web-API finden Sie unter [ASP.net-Web-API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Vorlage für Einzelseiten Anwendungen

Die Vorlage für die Einzelseiten Anwendung (Single Page Application, Spa) erstellt eine Beispielanwendung, die JavaScript, HTML 5 und [knockoutjs](http://knockoutjs.com/) auf dem Client und ASP.net-Web-API auf dem Server verwendet.

Die einzige Authentifizierungs Option für die Spa-Vorlage sind [einzelne Benutzerkonten](#indauth).

Die folgende Abbildung zeigt den ursprünglichen Zustand der Beispielanwendung, die von der Spa-Vorlage erstellt wird.

![Spa-Beispielanwendung](creating-web-projects-in-visual-studio/_static/image14.png)

Informationen zum Erstellen einer Anwendung mithilfe der Spa-Vorlage finden Sie unter [Web-API-externe Authentifizierungsdienste](../../../web-api/overview/security/external-authentication-services.md).

Weitere Informationen zu ASP.net Single-Page-Anwendungen und zu zusätzlichen Spa-Vorlagen, die andere JavaScript-Frameworks als "knockoutjs" verwenden, finden Sie in den folgenden Ressourcen:

- [ASP.net Single-Page-Anwendung](../../../single-page-application/index.md).
- [Grundlegendes zu Sicherheits Features in der Spa-Vorlage für VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Single-Page-Anwendungen: Erstellen von modernen, reaktionsfähigen Web-Apps mit ASP.net](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook-Vorlage

Sie können eine [Visual Studio-Erweiterung installieren, die eine Facebook-Vorlage bereitstellt](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Mit dieser Vorlage wird eine Beispielanwendung erstellt, die auf der Facebook-Website ausgeführt werden soll. Es basiert auf ASP.NET MVC und verwendet die Web-API für echt Zeit Aktualisierungs Funktionen.

Für die Facebook-Vorlage sind keine Authentifizierungs Optionen verfügbar, da Facebook-Anwendungen auf der Facebook-Website ausgeführt werden und von der Authentifizierung von Facebook abhängig sind.

Weitere Informationen zu ASP.net-Facebook-Anwendungen finden Sie unter [Aktualisieren der MVC-Facebook-API](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012-Vorlagen

Das Dialogfeld Visual Studio 2013-Webprojekt Erstellung bietet keinen Zugriff auf einige Vorlagen, die in Visual Studio 2012 verfügbar waren. Wenn Sie eine dieser Vorlagen verwenden möchten, können Sie im linken Bereich des Dialog Felds "Neues Projekt" von Visual Studio auf den Knoten "Visual Studio 2012" klicken.

![Visual Studio 2012-Vorlagen](creating-web-projects-in-visual-studio/_static/image15.png)

Mit dem Knoten " **Visual Studio 2012** " können Sie die folgenden Webvorlagen auswählen, auf die Sie in der Standardliste mit Vorlagen für Visual Studio 2013 keinen Zugriff haben:

- ASP.NET MVC 4-Webanwendung
- Webanwendung für ASP.NET Dynamic Data Entities
- ASP.NET AJAX-Server Steuerelement
- ASP.NET AJAX-Server Steuerelement-Extender
- ASP.NET-Server Steuerelement

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap in den Visual Studio 2013-Webprojekt Vorlagen

Die Visual Studio 2013-Projektvorlagen verwenden [Bootstrap](http://getbootstrap.com/), ein Layout und Design-Framework, das von Twitter erstellt wurde. Bootstrap verwendet CSS3, um reaktionsfähiges Design bereitzustellen. Dies bedeutet, dass Layouts dynamisch an verschiedene Browserfenster Größen angepasst werden können. Beispielsweise sieht die von der Web Forms Vorlage erstellte Startseite in einem breiten Browserfenster wie in der folgenden Abbildung aus:

![Startseite der Web Forms Vorlagen-App](creating-web-projects-in-visual-studio/_static/image16.png)

Vergrößern Sie das Fenster, und die horizontal angeordneten Spalten werden in vertikale Anordnung verschoben:

![Bootstrap-vertikale Spalten Anordnung](creating-web-projects-in-visual-studio/_static/image17.png)

Schränken Sie das Fenster ein, und das horizontale obere Menü wird zu einem Symbol, auf das Sie klicken können, um zu einem vertikal ausgerichteten Menü zu erweitern:

![Bootstrap-Menü Symbol](creating-web-projects-in-visual-studio/_static/image18.png)

![Bootstrap (vertikal) Menü](creating-web-projects-in-visual-studio/_static/image19.png)

Sie können auch die Funktion des Bootstrap-Features verwenden, um auf einfache Weise eine Änderung im Aussehen und Verhalten der Anwendung zu beeinflussen. Beispielsweise können Sie die folgenden Schritte ausführen, um das Design zu ändern.

1. Wechseln Sie in Ihrem Browser zu [http://Bootswatch.com](http://Bootswatch.com), wählen Sie ein Design aus, und klicken Sie dann auf **herunterladen**. (Hiermit wird " *Bootstrap. min. CSS* " standardmäßig heruntergeladen. Wenn Sie den CSS-Code untersuchen möchten, können Sie " *Bootstrap. CSS* " anstelle der minisierten Version herunterladen.)
2. Kopieren Sie den Inhalt der heruntergeladenen CSS-Datei.
3. Erstellen Sie in Visual Studio im *Inhalts* Ordner eine neue **Stylesheet** -Datei mit dem Namen *Bootstrap-Theme. CSS* , und fügen Sie den heruntergeladenen CSS-Code in die Datei ein.
4. Öffnen Sie *App\_Start/Bundle. config* , und ändern Sie die Datei *Bootstrap. CSS* in *Bootstrap-Theme. CSS*.

Führen Sie das Projekt erneut aus, und die Anwendung verfügt über ein neues Aussehen. Die folgende Abbildung zeigt die Auswirkung des Amelia-Designs:

![Bootstrap Amelia-Design](creating-web-projects-in-visual-studio/_static/image20.png)

Viele Bootstrap-Designs sind verfügbar, sowohl für kostenlose als auch für Premium-Versionen. Bootstrap bietet auch eine Vielzahl von Benutzeroberflächen Komponenten, z. b. [Dropdown](http://twitter.github.io/bootstrap/components.html#dropdowns)Felder, [Schaltflächen Gruppen](http://twitter.github.io/bootstrap/components.html#buttonGroups)und [Symbole](http://twitter.github.io/bootstrap/base-css.html#images). Weitere Informationen zu Bootstrap finden Sie auf [der Bootstrap-Website](http://twitter.github.io/bootstrap/).

Wenn Sie den Web Forms-Designer in Visual Studio verwenden, beachten Sie, dass der Designer CSS3 nicht unterstützt, sodass nicht alle Auswirkungen von Bootstrap-Designs oder reaktionsfähigen Layoutänderungen angezeigt werden. Die Web Forms Seiten werden jedoch ordnungsgemäß angezeigt, wenn Sie in einem Browser angezeigt werden.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Zusätzliche Unterstützung für zusätzliche Frameworks

Wenn Sie eine Vorlage auswählen, wird das Kontrollkästchen für die von der Vorlage verwendeten Frameworks automatisch ausgewählt. Wenn Sie z. b. die **Web Forms** Vorlage auswählen, wird das Kontrollkästchen **Web Forms** aktiviert, und Sie können es nicht löschen.

![Vorlage auswählen](creating-web-projects-in-visual-studio/_static/image21.png)

![Frameworks hinzufügen](creating-web-projects-in-visual-studio/_static/image22.png)

Sie können das Kontrollkästchen für ein Framework aktivieren, das nicht in der Vorlage enthalten ist, um Unterstützung für dieses Framework hinzuzufügen, wenn das Projekt erstellt wird. Wenn Sie z. b. die Verwendung von Web Forms *. aspx* -Seiten aktivieren möchten, wenn Sie die MVC-Vorlage ausgewählt haben, aktivieren Sie das Kontrollkästchen **Web Forms** . Wenn Sie MVC auch aktivieren möchten, wenn Sie die Web Forms Vorlage verwenden, klicken Sie auf das Kontrollkästchen **MVC** . Das Hinzufügen eines Frameworks ermöglicht Entwurfszeit-und Laufzeitunterstützung. Wenn Sie z. b. MVC-Unterstützung zu einem Web Forms Projekt hinzufügen, können Sie Controller und Ansichten mit Gerüstbau versehen.

Wenn Sie Web Forms und MVC in einem Projekt kombinieren und [friendly URLs](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) in Web Forms aktivieren, treten möglicherweise unerwartete Routing Probleme auf, bei denen eine URL mehrere mögliche Ziele hat. Die Routen, die zuerst definiert werden, haben Vorrang. Wenn Sie z. b. einen `Home` Controller und eine *Home. aspx* -Seite haben, wird die `http://contoso.com/home`-URL zu *Home. aspx* , wenn Sie die `EnableFriendlyUrls`-Methode aufrufen, bevor Sie die `MapRoute`-Methode in *RouteConfig.cs*aufrufen. andernfalls wird die gleiche URL zur Standardansicht für den `Home` Controller, wenn Sie `MapRoute` vor `EnableFriendlyUrls`aufrufen.

Durch das Hinzufügen eines Frameworks werden keine Beispiel Anwendungsfunktionen hinzugefügt. Wenn Sie z. b. Web Forms Unterstützung hinzufügen, wenn Sie die MVC-Vorlage ausgewählt haben, wird keine " *default. aspx* "-Startseiten Datei erstellt. Nur die Ordner, Dateien und Verweise, die zur Unterstützung des Frameworks erforderlich sind, werden hinzugefügt. Aus diesem Grund werden durch das Hinzufügen von Frameworks keine Authentifizierungs Optionen geändert, die von Code in Beispielanwendungen implementiert werden, die von den Vorlagen erstellt wurden. Wenn Sie z. b. die leere Vorlage auswählen und Web Forms-oder MVC-Unterstützung hinzufügen, wird die Schaltfläche **Authentifizierung konfigurieren** weiterhin deaktiviert.

In den folgenden Abschnitten werden die Auswirkungen der einzelnen Kontrollkästchen kurz beschrieben.

### <a name="add-web-forms-support"></a>Web Forms Unterstützung hinzufügen

Erstellt leere *App-\_Daten* -und *Modell* Ordner und eine *Global. asax* -Datei. Diese werden bereits von allen anderen Vorlagen als der leeren Vorlage erstellt, sodass das Aktivieren des Kontrollkästchens Web Forms für andere Vorlagen keinen Unterschied macht.

Mit der Web Forms-Vorlage werden standardmäßig benutzerfreundliche URLs aktiviert, aber wenn Sie anderen Vorlagen Web Forms Unterstützung hinzufügen, indem Sie das Kontrollkästchen Web Forms aktivieren, werden die angezeigten URLs nicht automatisch aktiviert.

### <a name="add-mvc-support"></a>MVC-Unterstützung hinzufügen

Installiert nuget-Pakete für MVC, Razor und Webseiten, erstellt leere *App\_Daten*, *Controller*, *Modelle*und *Ansichten* Ordner, erstellt *App\_Start* Ordner mit *RouteConfig.cs* File und erstellt die Datei " *Global. asax* ".

### <a name="add-web-api-support"></a>Unterstützung für Web-API

Installiert WebAPI-und newtonsoft. JSON-nuget-Pakete, erstellt leere *App-\_Daten*-, *Controller*-und *Modell* Ordner, erstellt *App\_Start* Ordners mit *WebApiConfig.cs* File und erstellt die Datei " *Global. asax* ".

<a id="auth"></a>
## <a name="authentication-methods"></a>Authentifizierungsmethoden

Visual Studio 2013 bietet verschiedene Authentifizierungs Optionen für die Web Forms-, MVC-und Web-API-Vorlagen:

- [Keine Authentifizierung](#noauth)
- [Einzelne Benutzerkonten](#indauth) (ASP.net Identity, ehemals ASP.net Membership)
- [Organisations Konten](#orgauth) (Windows Server Active Directory oder Azure Active Directory)
- [Windows-Authentifizierung](#winauth) (Intranet)

![Dialogfeld zur Authentifizierung konfigurieren](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Keine Authentifizierung

Wenn Sie **keine Authentifizierung**auswählen, enthält die Beispielanwendung keine Webseiten zum Anmelden, keine Benutzeroberfläche, die angibt, wer angemeldet ist, keine Entitäts Klassen für eine Mitgliedschafts Datenbank und keine Verbindungs Zeichenfolge für eine Mitgliedschafts Datenbank.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Einzelne Benutzerkonten

Wenn Sie **einzelne Benutzerkonten**auswählen, wird die Beispielanwendung für die Verwendung von ASP.net Identity (früher als ASP.NET Mitgliedschaft bezeichnet) für die Benutzerauthentifizierung konfiguriert. ASP.net Identity ermöglicht es einem Benutzer, ein Konto zu registrieren, indem er einen Benutzernamen und ein Kennwort auf der Website erstellt oder sich mit Anbietern von sozialen Netzwerken wie Facebook, Google, Microsoft-Konto oder Twitter anmeldet. Der Standarddaten Speicher für Benutzerprofile in ASP.net Identity ist eine SQL Server localdb-Datenbank, die Sie für die Bereitstellung in SQL Server oder Azure SQL-Datenbank für den Produktionsstandort bereitstellen können.

In Visual Studio 2013 diese Features identisch mit denen in Visual Studio 2012, aber der zugrunde liegende Code für das ASP.NET-Mitgliedschaftssystem wurde umgeschrieben. Zu den Vorteilen der neuen Codebasis zählen folgende:

- Das neue Mitgliedschaftssystem basiert auf [owin](http://owin.org/) und nicht auf dem ASP.net Forms Authentication-Modul. Dies bedeutet, dass Sie denselben Authentifizierungsmechanismus verwenden können, unabhängig davon, ob Sie Web Forms oder MVC in IIS verwenden oder ob Sie Web-API oder signalr selbst Hosting haben.
- Die neue Mitgliedschafts Datenbank wird von Entity Framework Code First verwaltet, und alle Tabellen werden durch Entitäts Klassen dargestellt, die Sie ändern können. Dies bedeutet, dass Sie das Datenbankschema und die Profil bezogene Webbenutzer Oberfläche problemlos an Ihre eigenen Anforderungen anpassen können, und Sie können Ihre Updates problemlos mithilfe Code First-Migrationen bereitstellen.

Das neue Mitgliedschaftssystem wird automatisch in den neuen Vorlagen implementiert und kann in jedem Projekt, das .NET 4,5 oder höher als Ziel hat, manuell implementiert werden.

ASP.net Identity ist eine gute Wahl, wenn Sie eine Internet Website erstellen, die in erster Linie externe Kunden ist. Wenn Ihre Organisation Active Directory oder Office 365 verwendet und Sie ein Projekt erstellen möchten, das einmaliges Anmelden für Mitarbeiter und Geschäftspartner ermöglicht, ist die Option **Organisations Konten** möglicherweise eine bessere Wahl.

Weitere Informationen zur Option individuelle Benutzerkonten finden Sie in den folgenden Ressourcen:

- [www.ASP.net/Identity](../../../identity/index.md). Dokumentation zu ASP.net Identity auf der ASP.NET-Website.
- [Erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Anmeldung](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Außerdem wird gezeigt, wie Benutzerprofil Daten angepasst werden.
- [Web-API-externe Authentifizierungsdienste](../../../web-api/overview/security/external-authentication-services.md)
- [Hinzufügen externer Anmeldungen zu Ihrer ASP.NET-Anwendung in Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Organisationskonten

Wenn Sie **Organisations Konten**auswählen, wird die Beispielanwendung für die Verwendung von Windows Identity Foundation (WIF) für die Authentifizierung auf der Grundlage von Benutzerkonten in Azure Active Directory (Azure AD einschließlich Office 365) oder Windows Server Active Directory konfiguriert. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Organisations Konto-Authentifizierungs Optionen](#orgauthoptions) .

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows-Authentifizierung

Wenn Sie die **Windows-Authentifizierung**auswählen, wird die Beispielanwendung so konfiguriert, dass das Windows-Authentifizierungs-IIS-Modul für die Authentifizierung verwendet wird. Die Anwendung zeigt die Domäne und die Benutzer-ID des Active Directory-oder lokalen Computer Kontos an, das bei Windows angemeldet ist, aber keine Benutzerregistrierung oder Anmelde Benutzeroberfläche enthält. Diese Option ist für Intranetwebsites vorgesehen.

Alternativ können Sie eine Intranetsite erstellen, die die AD-Authentifizierung verwendet, indem Sie die [Option lokal unter Organisations Konten](#orgauthonprem)auswählen. Die Option lokal verwendet Windows Identity Foundation (WIF) anstelle des Windows-Authentifizierungs Moduls. Zum Einrichten der lokalen Option sind einige zusätzliche Schritte erforderlich, aber WIF aktiviert Features, die im Windows-Authentifizierungs Modul nicht verfügbar sind. Beispielsweise können Sie mit WIF den Anwendungs Zugriff in Active Directory konfigurieren und Verzeichnis Daten Abfragen.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Authentifizierungs Optionen für das Organisations Konto

Im Dialogfeld **Authentifizierung konfigurieren** erhalten Sie mehrere Optionen für Azure Active Directory (Azure AD, einschließlich Office 365) oder Windows Server Active Directory (AD)-Konto Authentifizierung:

- [Cloud-Single Organization](#orgauthsingle) (Azure AD oder AD mithilfe der Verzeichnisintegration mit Azure AD)
- [Cloud-Multi-Organization](#orgauthmulti) (Azure AD oder AD mithilfe der Verzeichnisintegration mit Azure AD)
- [Lokal](#orgauthonprem) (AD)

Wenn Sie eine der Azure AD Optionen ausprobieren möchten, aber noch kein Konto haben, [Klicken Sie hier, um sich für ein Azure AD Konto anzumelden](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Wenn Sie eine der Azure AD Optionen auswählen, benötigt das Projekt eine Datenbank, und Sie müssen sich bei einem globalen Administrator Konto für Ihren Azure AD-Mandanten anmelden. Geben Sie den Namen und das Kennwort für ein Organisations Konto (z. b. admin@contoso.onmicrosoft.com) ein, das über Administrator Berechtigungen für Ihren Azure AD-Mandanten verfügt.
> 
> **Geben Sie die Anmelde Informationen für ein Microsoft-Konto (z. b. contoso@hotmail.com) im Anmelde Dialogfeld nicht ein.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Authentifizierung mit einer einzelnen Organisation in der Cloud

![Authentifizierung mit einer einzelnen Organisation](creating-web-projects-in-visual-studio/_static/image24.png)

Wählen Sie diese Option aus, wenn Sie die Authentifizierung für Benutzerkonten aktivieren möchten, die in [einem Azure AD Mandanten](https://technet.microsoft.com/library/jj573650.aspx)definiert sind. Beispielsweise ist die Website contoso.com und wird den Mitarbeitern des Unternehmens "Unternehmen", die sich im contoso.onmicrosoft.com-Mandanten befinden, zur Verfügung gestellt. Sie können Azure AD nicht konfigurieren, um Benutzern von anderen Mandanten den Zugriff auf die Anwendung zu gestatten.

#### <a name="domain"></a>Domain

Geben Sie die Azure AD Domäne ein, in der Sie die Anwendung einrichten möchten, z. b.: `contoso.onmicrosoft.com`. Wenn Sie über eine [benutzerdefinierte Domäne](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/)verfügen, z. b. `contoso.com` anstelle `contoso.onmicrosoft.com`, können Sie diese hier eingeben.

#### <a name="access-level"></a>Zugriffsebene

Wenn die Anwendung Verzeichnisinformationen mithilfe der Graph-API Abfragen oder aktualisieren muss, wählen Sie **einmaliges Anmelden, Verzeichnis Daten** oder **einmaliges Anmelden, lesen und Schreiben von Verzeichnis Daten**aus. Wählen Sie andernfalls **einmaliges Anmelden aus**. Weitere Informationen finden Sie unter [Anwendungs Zugriffsebenen](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) und [Verwenden der Graph-API, um Azure AD abzufragen](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Anwendungs-ID-URI

Standardmäßig erstellt die Vorlage einen Anwendungs-ID-URI für Sie, indem der Projektname an die Azure AD Domäne angehängt wird. Wenn der Projektname beispielsweise `Example` und die Domäne `contoso.onmicrosoft.com`ist, wird der Anwendungs-ID-URI `https://contoso.onmicrosoft.com/Example`. Wenn Sie den Anwendungs-ID-URI manuell angeben möchten, erweitern Sie den Abschnitt **Weitere Optionen** , und geben Sie den Anwendungs-ID-URI in das Textfeld ein. Der Anwendungs-ID-URI muss mit `https://`beginnen.

Standardmäßig ist eine Anwendung, die bereits in Azure AD bereitgestellt wurde, denselben Anwendungs-ID-URI wie der, den Visual Studio für das Projekt verwendet, und das Projekt wird mit der vorhandenen Anwendung verbunden, anstatt eine neue bereitzustellen. Wenn in diesem Fall eine neue Anwendung bereitgestellt werden soll, deaktivieren Sie das Kontrollkästchen **Anwendungs Eintrag überschreiben, wenn bereits ein solcher ID vorhanden** ist.

Wenn das Kontrollkästchen über **Schreiben** deaktiviert ist und Visual Studio eine vorhandene Anwendung mit demselben Anwendungs-ID-URI findet, wird ein neuer URI erstellt, indem eine Zahl an den URI angehängt wird, der verwendet werden soll. Angenommen, der Projektname ist `Example`. Sie lassen das Textfeld leer. deaktivieren Sie das Kontrollkästchen über **Schreiben** , und der Azure AD Mandant verfügt bereits über eine Anwendung mit dem URI `https://contoso.onmicrosoft.com/Example`. In diesem Fall wird eine neue Anwendung mit einem Anwendungs-ID-URI wie `https://contoso.onmicrosoft.com/Example_20130619330903`bereitgestellt.

#### <a name="provisioning-the-application-in-azure-ad"></a>Bereitstellen der Anwendung in Azure AD

Um die Anwendung in Azure AD bereitzustellen oder das Projekt mit einer vorhandenen Anwendung zu verbinden, benötigt Visual Studio die Anmelde Informationen eines globalen Administrators für die Domäne. Wenn Sie im Dialogfeld **Authentifizierung konfigurieren** auf **OK** klicken, werden Sie zur Eingabe des Benutzernamens und des Kennworts für einen globalen Administrator der von Ihnen angegebenen Domäne aufgefordert. Wenn Sie später im Dialogfeld **Neues ASP.net-Projekt** auf **Projekt erstellen** klicken, stellt Visual Studio die Anwendung in Azure AD bereit. Beachten Sie, dass Visual Studio im Rahmen dieses Prozesses die geheimen Client Schlüsselwerte in die Datei "Web. config" einbettet, die ein Jahr nach der Erstellung abläuft.

Informationen zum Erstellen von Anwendungen, die die Authentifizierung mit **einer einzelnen Organisation** verwenden, finden Sie in den folgenden Ressourcen:

- [Azure-Authentifizierung](../2012/windows-azure-authentication.md)
- [Hinzufügen einer Anmeldung zu einer Webanwendung mithilfe von Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Entwickeln von ASP.NET-Apps mit Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Sichern von ASP.net-Web-API mit Azure AD und Microsoft owin-Komponenten](https://msdn.microsoft.com/magazine/dn463788.aspx)

Die Tutorials wurden für Visual Studio 2013 noch nicht aktualisiert. einige der Tutorials, die Sie manuell ausführen, werden in Visual Studio 2013 automatisiert.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud-Multi-Organization-Authentifizierung

![Authentifizierung mit mehreren Organisationen](creating-web-projects-in-visual-studio/_static/image25.png)

Wählen Sie diese Option aus, wenn Sie die Authentifizierung für Benutzerkonten aktivieren möchten, die in [mehreren Azure AD Mandanten](https://technet.microsoft.com/library/jj573650.aspx)definiert sind. Beispielsweise ist die Website contoso.com und wird den Mitarbeitern des Unternehmens "Unternehmen", die sich im contoso.onmicrosoft.com-Mandanten befinden, und der Mitarbeiter des Fabrikam-Unternehmens, die sich im fabrikam.onmicrosoft.com-Mandanten befinden, zur Verfügung gestellt.

Die Einstellungen, die Sie eingeben, und der Schritt für die Anwendungs Bereitstellung ähneln der [Authentifizierung in einer Organisation](#orgauthsingle).

Weitere Informationen zum Erstellen von Anwendungen, die die Authentifizierung in der **Cloud mit mehreren Organisationen** verwenden, finden Sie in den folgenden Ressourcen:

- [Einfache Web-App-Integration mit Azure Active Directory, ASP.net &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) im Active Directory Teamblog.
- [Entwickeln von mehr Instanzen fähigen Webanwendungen mit Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) Tutorial. Das Tutorial wurde für Visual Studio 2013 noch nicht aktualisiert. einige der Schritte, die Sie in diesem Tutorial manuell durchführen, werden in Visual Studio 2013 automatisiert.
- [Sie müssen sich mit ihrer eigenen ASP.net-App für mehrere Organisationen registrieren, bevor Sie sich anmelden können](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog von Vittorio Bertocci, in dem erläutert wird, wie ein gängiges Problem gelöst werden kann, das beim Erstellen eines Projekts mit Authentifizierung mit mehreren Organisationen auftritt.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Lokale Organisations Authentifizierung

![Lokale Organisations Authentifizierung](creating-web-projects-in-visual-studio/_static/image26.png)

Wählen Sie diese Option aus, wenn Sie die Authentifizierung für Benutzerkonten aktivieren möchten, die in Windows Server Active Directory (AD) definiert sind, und Sie Azure AD nicht verwenden möchten. Sie können diese Option verwenden, um eine Intranetsite oder eine Internet Website zu erstellen. Verwenden Sie für eine Internet Site Active Directory-Verbunddienste (AD FS) (ADFS), um den Zugriff auf AD bereitzustellen. Weitere Informationen finden Sie unter [Verwenden der lokalen Organisations Authentifizierungs Option (ADFS) mit ASP.net in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Als Alternative können Sie für eine Intranetsite anstelle dieser Option die Option [Windows-Authentifizierung](#winauth) auswählen. Für die Windows-Authentifizierungs Option müssen Sie keine Metadaten-Dokument-URL angeben. Die Windows-Authentifizierung bietet Ihnen jedoch nicht die Möglichkeit, den Anwendungs Zugriff in Active Directory zu steuern oder Verzeichnis Daten abzufragen.

#### <a name="on-premises-authority"></a>Lokale Autorität

Geben Sie eine URL ein, die auf das Metadatendokument verweist. Das Metadatendokument enthält die Koordinaten der Zertifizierungsstelle. Diese Koordinaten werden von der Anwendung verwendet, um den Webanmeldungs Fluss zu steuern.

#### <a name="application-id-uri"></a>Anwendungs-ID-URI

Geben Sie einen eindeutigen URI an, mit dem AD diese Anwendung identifizieren kann, oder lassen Sie das Feld leer, damit Visual Studio eine solche Anwendung erstellen kann.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

Dieses Dokument bietet einige grundlegende Hilfe zum Erstellen eines neuen ASP.net-Webprojekts in Visual Studio 2013. Weitere Informationen zur Verwendung von für Visual Studio für die Webentwicklung finden Sie unter [https://www.asp.net/visual-studio/](../../index.md).
