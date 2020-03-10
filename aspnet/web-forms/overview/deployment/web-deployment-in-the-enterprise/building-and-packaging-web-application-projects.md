---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Entwickeln und Verpacken von Webanwendungs Projekten | Microsoft-Dokumentation
author: jrjlee
description: Wenn Sie ein Webanwendungs Projekt in einer Remote Serverumgebung bereitstellen möchten, besteht die erste Aufgabe darin, das Projekt zu erstellen und eine Webbereitstellungs-Packa zu generieren...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 1d0ee0264ce6461d7b0159f1a44de4de31e2d079
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463227"
---
# <a name="building-and-packaging-web-application-projects"></a>Erstellen von Webanwendungsprojekten und Paketerstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie ein Webanwendungs Projekt in einer Remote Serverumgebung bereitstellen möchten, besteht die erste Aufgabe darin, das Projekt zu erstellen und ein Webbereitstellungs Paket zu generieren. In diesem Thema wird beschrieben, wie der Buildprozess für Webanwendungs Projekte funktioniert. Insbesondere wird Folgendes erläutert:
> 
> - Gibt an, wie die Webpublishing Pipeline (WPP) den Buildprozess erweitert, um die Bereitstellungs Funktionalität einzubeziehen.
> - Wie das Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) Ihre Webanwendung in ein Bereitstellungs Paket umwandelt.
> - Wie der Build-und Verpackungsprozess funktioniert und welche Dateien erstellt werden.

In Visual Studio 2010 wird der Build-und Bereitstellungs Prozess für Webanwendungs Projekte von WPP unterstützt. Das WPP bietet eine Reihe von Microsoft-Build-Engine-Zielen (MSBuild), die die Funktionalität von MSBuild erweitern und die Integration in Web deploy ermöglichen. In Visual Studio können Sie diese erweiterte Funktionalität auf den Eigenschaften Seiten für das Webanwendungs Projekt sehen. Auf der **Webseite "Verpacken/veröffentlichen** " können Sie zusammen mit der Seite " **SQL Verpacken/veröffentlichen** " konfigurieren, wie das Webanwendungs Projekt für die Bereitstellung verpackt wird, wenn der Buildprozess beendet ist.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Funktionsweise von WPP

Wenn Sie sich die Projektdatei für ein C#-basiertes Webanwendungs Projekt ansehen, sehen Sie, dass zwei Targets-Dateien importiert werden.

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]

Die erste **Import** Anweisung ist für alle visuellen C# Projekte üblich. Diese Datei, *Microsoft. CSharp. targets*, enthält Ziele und Aufgaben, die für Visual C#spezifisch sind. Beispielsweise wird hier C# die Compileraufgabe (**csc**) aufgerufen. Mit der Datei " *Microsoft. CSharp. targets* " wird wiederum die Datei " *Microsoft. Common. targets* " importiert. Definiert Ziele, die allen Projekten gemeinsam sind, wie z. b. **Erstellen**, **neu**erstellen, **Ausführen**, **Kompilieren**und **Bereinigen**. Die zweite **Import** Anweisung ist spezifisch für Webanwendungs Projekte. Mit der Datei " *Microsoft. WebApplication. targets* " wird wiederum die Datei " *Microsoft. Web. Publishing. targets* " importiert. Die Datei " *Microsoft. Web. Publishing. targets* " *ist* im Wesentlichen die WPP. Sie definiert Ziele wie " **Package** " und " **msdeploypublish**", die Web deploy aufrufen, um verschiedene Bereitstellungs Aufgaben abzuschließen.

Um zu verstehen, wie diese zusätzlichen Ziele verwendet werden, öffnen Sie in der Beispiellösung "Contact Manager" die Datei " *Publish. proj* ", und sehen Sie sich das Ziel " **buildprojects** " an.

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]

Dieses Ziel verwendet die **MSBuild** -Aufgabe, um verschiedene Projekte zu erstellen. Beachten Sie die Eigenschaften **deployonbuild** und **deploytarget** :

- Die **deployonbuild = true** -Eigenschaft bedeutet im Wesentlichen, dass ein zusätzliches Ziel ausgeführt werden soll, wenn der Build erfolgreich abgeschlossen wird.
- Die Eigenschaft **deploytarget** identifiziert den Namen des Ziels, das Sie ausführen möchten, wenn die Eigenschaft **deployonbuild** gleich **true**ist. In diesem Fall geben Sie an, dass MSBuild das **Paketziel** nach dem Erstellen des Projekts ausführen soll.

Das **Paket** Ziel ist in der Datei " *Microsoft. Web. Publishing. targets* " definiert. Im wesentlichen nimmt dieses Ziel die Buildausgabe des Webanwendungs Projekts an und wandelt es in ein Webbereitstellungs Paket, das auf einem IIS-Webserver veröffentlicht werden kann.

> [!NOTE]
> Zum Anzeigen einer Projektdatei (z. <em>b. "ContactManager. MVC. csproj</em>") in Visual Studio 2010 müssen Sie zuerst das Projekt aus der Projekt Mappe entladen. Klicken Sie im <strong>Projektmappen-Explorer</strong> Fenster mit der rechten Maustaste auf den Projekt Knoten, und klicken Sie dann auf <strong>Projekt entladen</strong>. Klicken Sie erneut mit der rechten Maustaste auf den Projekt Knoten, und klicken Sie dann auf <strong>Bearbeiten</strong><em>[Projektdatei]</em>). Die Projektdatei wird im unformatierten XML-Format geöffnet. Denken Sie daran, das Projekt erneut zu laden, wenn Sie fertig sind.  
> Weitere Informationen zu MSBuild-Zielen, Tasks und <strong>Import</strong> -Anweisungen finden Sie Untergrund Legendes [zur Projektdatei](understanding-the-project-file.md). Eine ausführlichere Einführung in Projektdateien und WPP finden Sie unter [in der Microsoft-Build-Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) von Sayed Ibrahim Hashimi und William Bartholomew, ISBN: 978-0-7356-4524-0.

## <a name="what-is-a-web-deployment-package"></a>Was ist ein Webbereitstellungs Paket?

Wenn Sie ein Webanwendungs Projekt erstellen und bereitstellen, entweder mithilfe von Visual Studio 2010 oder direkt mithilfe von MSBuild, ist das Endergebnis normalerweise ein *Webbereitstellungs Paket*. Das Webbereitstellungs Paket ist eine ZIP-Datei. Sie enthält alles, was IIS und Web deploy benötigen, um Ihre Webanwendung neu zu erstellen, einschließlich:

- Die kompilierte Ausgabe Ihrer Webanwendung, einschließlich Inhalt, Ressourcen Dateien, Konfigurationsdateien, JavaScript-und Cascading Stylesheets (CSS)-Ressourcen usw.
- Assemblys für Ihr Webanwendungs Projekt und für alle referenzierten Projekte in der Projekt Mappe.
- SQL-Skripts zum Generieren von Datenbanken, die Sie mit Ihrer Webanwendung bereitstellen.

Nachdem das Webbereitstellungs Paket generiert wurde, können Sie es auf verschiedene Weise auf einem IIS-Webserver veröffentlichen. Beispielsweise können Sie diese Remote bereitstellen, indem Sie den Web deploy Remote-Agent-Dienst oder den Web deploy Handler auf dem Zielweb Server als Ziel verwenden, oder Sie können den IIS-Manager verwenden, um das Paket manuell auf den Zielweb Server zu importieren. Weitere Informationen zu diesen Ansätzen für die Bereitstellung finden Sie unter [auswählen des richtigen Ansatzes für die Webbereitstellung](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Wie funktioniert der Buildprozess?

Dies zeigt, was geschieht, wenn Sie ein Webanwendungs Projekt erstellen und packen:

![](building-and-packaging-web-application-projects/_static/image2.png)

Wenn Sie ein Webanwendungs Projekt erstellen, generiert der Buildprozess eine Datei mit dem Namen *[Project Name]. Sourcemanifest. XML*. Dies ist zusammen mit der Projektdatei und der Buildausgabe *. Die Datei sourcemanifest. XML* gibt Web Deploy an, was im Webbereitstellungs Paket enthalten sein muss. Mithilfe dieser Eingaben generiert Web deploy ein Webbereitstellungs Paket mit dem Namen *[Project Name]. zip*.

Neben dem Webbereitstellungs Paket generiert der Buildprozess zwei Dateien, die Ihnen bei der Verwendung des Pakets helfen können:

- Die *. Deployment. cmd* -Datei enthält einen Satz parametrisierter Web deploy-Befehle (msdeployment. exe), mit denen das Webbereitstellungs Paket auf einem IIS-Remote Webserver veröffentlicht wird. Das Ausführen der Datei ". Bereitstellungs- *cmd* " mit den entsprechenden Parametern bietet in der Regel eine schnellere und einfachere Alternative zum manuellen Erstellen der Befehle "msbereitstellungs. exe".
- Die Datei " *SetParameters. XML* " enthält einen Satz von Parameterwerten für den Befehl "msbereitstellungs. exe". Diese Werte umfassen Eigenschaften wie den Namen der IIS-Webanwendung, für die Sie das Paket bereitstellen möchten, die Werte von Dienst Endpunkten und Verbindungs Zeichenfolgen, die in der Datei " *Web. config* " definiert sind, sowie alle Bereitstellungs Eigenschaftswerte, die auf den Projekteigenschaften Seiten definiert sind.

Die Datei " *SetParameters. XML* " ist der Schlüssel für die Verwaltung des Bereitstellungs Prozesses. Diese Datei wird dynamisch entsprechend dem Inhalt des Webanwendungs Projekts generiert. Wenn Sie z. b. der Datei " *Web. config* " eine Verbindungs Zeichenfolge hinzufügen, wird die Verbindungs Zeichenfolge vom Buildprozess automatisch erkannt, die Bereitstellung entsprechend parametrisiert und ein Eintrag in der Datei " *SetParameters. XML* " erstellt, damit Sie die Verbindungs Zeichenfolge im Rahmen des Bereitstellungs Prozesses ändern können. Im nächsten Thema, [Konfigurieren von Parametern für die Webpaket Bereitstellung](configuring-parameters-for-web-package-deployment.md), wird die Rolle dieser Datei ausführlicher erläutert, und es werden die verschiedenen Möglichkeiten beschrieben, wie Sie Sie während der Erstellung und Bereitstellung ändern können.

> [!NOTE]
> In Visual Studio 2010 wird die Vorkompilierung der Seiten in einer Webanwendung vor der Paket Erstellung von WPP nicht unterstützt. Die nächste Version von Visual Studio und die WPP enthalten die Möglichkeit, eine Webanwendung als Paket Option vorkompilieren zu können.

## <a name="conclusion"></a>Zusammenfassung

Dieses Thema bietet einen Überblick über den Build-und Verpackungsprozess für Webanwendungs Projekte in Visual Studio 2010. Es wurde beschrieben, wie Sie mit WPP Web deploy Befehle von MSBuild aufrufen können, und es wurde erläutert, wie der Build-und Verpackungsprozess funktioniert.

Wenn Sie ein Webbereitstellungs Paket erstellt haben, ist der nächste Schritt die Bereitstellung. Weitere Informationen hierzu finden Sie unter [Konfigurieren von Parametern für die Webpaket Bereitstellung und bereit](configuring-parameters-for-web-package-deployment.md) stellen von [Webpaketen](deploying-web-packages.md).

## <a name="further-reading"></a>Weitere nützliche Informationen

In den nächsten Themen dieses Tutorials, [Konfigurieren von Parametern für die Webpaket Bereitstellung](configuring-parameters-for-web-package-deployment.md) und Bereitstellen von [Webpaketen](deploying-web-packages.md)finden Sie Anleitungen zur Verwendung des erstellten Webpakets. Das abschließende Tutorial in dieser Reihe, [Advanced Unternehmensweb Bereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), bietet Anleitungen zum Anpassen und behandeln von Problemen bei der Paket Erstellung.

Eine ausführlichere Einführung in Projektdateien und WPP finden Sie unter [in der Microsoft-Build-Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) von Sayed Ibrahim Hashimi und William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](understanding-the-build-process.md)
> [Weiter](configuring-parameters-for-web-package-deployment.md)
