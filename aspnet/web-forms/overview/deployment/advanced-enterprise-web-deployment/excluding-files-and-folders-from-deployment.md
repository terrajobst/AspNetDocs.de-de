---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Ausschließen von Dateien und Ordnern aus der Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie Dateien und Ordner aus einem Webbereitstellungs Paket ausschließen können, wenn Sie ein Webanwendungs Projekt erstellen und packen.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: a262ce43d7199fb1015d54d0b7c213857c360946
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438417"
---
# <a name="excluding-files-and-folders-from-deployment"></a>Ausschließen von Dateien und Ordnern von der Bereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie Dateien und Ordner aus einem Webbereitstellungs Paket ausschließen können, wenn Sie ein Webanwendungs Projekt erstellen und packen.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="overview"></a>Übersicht

Wenn Sie in Visual Studio 2010 ein Webanwendungs Projekt erstellen, können Sie mithilfe der Webpublishing Pipeline (WPP) diesen Buildprozess erweitern, indem Sie die kompilierte Webanwendung in ein bereitstell bares Webpaket verpacken. Anschließend können Sie das Webbereitstellungs Tool (Web deploy) Internetinformationsdienste (IIS) verwenden, um dieses Webpaket auf einem IIS-Remote Webserver bereitzustellen, oder das Webpaket manuell über den IIS-Manager importieren. Dieser Verpackungsprozess wird in [Erstellung und Verpacken von Webanwendungs Projekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)erläutert.

Wie können Sie also steuern, was in Ihrem Webpaket enthalten ist? Die Projekteinstellungen in Visual Studio über die zugrunde liegende Projektdatei bieten eine ausreichende Kontrolle für viele Szenarien. In einigen Fällen möchten Sie jedoch möglicherweise den Inhalt Ihres Webpakets an bestimmte Ziel Umgebungen anpassen. Beispielsweise können Sie einen Ordner für Protokolldateien einschließen, wenn Sie die Anwendung in einer Testumgebung bereitstellen, aber den Ordner ausschließen, wenn Sie die Anwendung in einer Staging-oder Produktionsumgebung bereitstellen. In diesem Thema wird gezeigt, wie Sie dies tun.

## <a name="what-gets-included-by-default"></a>Was ist standardmäßig inbegriffen?

Wenn Sie die Eigenschaften des Webanwendungs Projekts in Visual Studio konfigurieren, können Sie in der Liste zu bereit zustellende Elemente auf der **Webseite zum Packen/veröffentlichen** angeben, welche **Elemente** Sie in das Webbereitstellungs Paket einschließen möchten. Standardmäßig ist dies nur für Dateien festgelegt, **die zum Ausführen der Anwendung erforderlich**sind.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Wenn Sie nur die Dateien auswählen, die **zum Ausführen dieser Anwendung benötigt**werden, wird von WPP versucht, die Dateien zu ermitteln, die dem Webpaket hinzugefügt werden sollen. Dies umfasst Folgendes:

- Alle Buildausgaben für das Projekt.
- Alle Dateien, die mit einer Buildaktion von **Content**gekennzeichnet sind.

> [!NOTE]
> Die Logik, die festlegt, welche Dateien eingeschlossen werden sollen, ist in dieser Datei enthalten:   
> *%ProgramFiles%\msbuild\microsoft\visualstudio\v10.0\web\ Microsoft. Web. Publishing. onlyfilestorunderapp. targets*

## <a name="excluding-specific-files-and-folders"></a>Ausschließen bestimmter Dateien und Ordner

In einigen Fällen benötigen Sie eine präzisere Kontrolle darüber, welche Dateien und Ordner bereitgestellt werden. Wenn Sie wissen, welche Dateien im Voraus ausgeschlossen werden sollen und der Ausschluss für alle Ziel Umgebungen gilt, können Sie die **Buildaktion** der einzelnen Dateien einfach auf **keine**festlegen.

**So schließen Sie bestimmte Dateien aus der Bereitstellung aus**

1. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **Eigenschaften**.
2. Wählen Sie im Fenster **Eigenschaften** in der **Zeile** Buildvorgang die Option **keine**aus.

Dieser Ansatz ist jedoch nicht immer praktisch. Beispielsweise möchten Sie möglicherweise variieren, welche Dateien und Ordner gemäß Ihrer Zielumgebung und außerhalb von Visual Studio enthalten sind. Betrachten Sie z. b. in der Beispiellösung Contact Manager den Inhalt des Projekts ContactManager. MVC:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Der interne Ordner enthält einige SQL-Skripts, die vom Entwickler zum Erstellen, löschen und Auffüllen lokaler Datenbanken zu Entwicklungszwecken verwendet werden. In diesem Ordner sollte nichts in einer Staging-oder Produktionsumgebung bereitgestellt werden.
- Der Ordner Scripts enthält mehrere JavaScript-Dateien. Viele dieser Dateien sind ausschließlich zum unterstützen des Debuggens oder zum Bereitstellen von IntelliSense in Visual Studio enthalten. Einige dieser Dateien sollten nicht in Staging-oder Produktionsumgebungen bereitgestellt werden. Sie möchten Sie jedoch möglicherweise in einer Entwickler Testumgebung bereitstellen, um die Problembehandlung zu vereinfachen.

Obwohl Sie die Projektdateien so bearbeiten könnten, dass bestimmte Dateien und Ordner ausgeschlossen werden, gibt es eine einfachere Methode. Das WPP umfasst einen Mechanismus zum Ausschließen von Dateien und Ordnern durch das Entwickeln von Element Listen mit dem Namen **excludefrompackagefolders** und **excludefrompackagefiles**. Sie können diesen Mechanismus erweitern, indem Sie eigene Elemente zu diesen Listen hinzufügen. Hierzu müssen Sie die folgenden Schritte ausführen:

1. Erstellen Sie eine benutzerdefinierte Projektdatei mit dem Namen *[Project Name]. WPP. targets* in demselben Ordner wie die Projektdatei.

    > [!NOTE]
    > Die *WPP. targets* -Datei muss sich im selben Ordner wie die Projektdatei&#x2014;der Webanwendung befinden. Beispiel: *ContactManager. MVC. csproj*&#x2014;und nicht im selben Ordner wie benutzerdefinierte Projektdateien, mit denen Sie den Build-und Bereitstellungs Prozess steuern.
2. Fügen Sie in der Datei *. WPP. targets* ein **ItemGroup** -Element hinzu.
3. Fügen Sie im Element **ItemGroup** die Elemente **excludefrompackagefolders** und **excludefrompackagefiles** hinzu, um bestimmte Dateien und Ordner nach Bedarf auszuschließen.

Dies ist die grundlegende Struktur dieser *WPP. targets* -Datei:

[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]

Beachten Sie, dass jedes Element ein Element-Metadatenelement mit dem Namen **fromtarget**enthält. Dies ist ein optionaler Wert, der den Buildprozess nicht beeinträchtigt. Er dient lediglich dazu, anzugeben, warum bestimmte Dateien oder Ordner ausgelassen wurden, wenn jemand die Buildprotokolle prüft.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Ausschließen von Dateien und Ordnern von einem Webpaket

Im nächsten Verfahren wird gezeigt, wie Sie eine *WPP. targets* -Datei zu einem Webanwendungs Projekt hinzufügen und wie Sie diese Datei verwenden, um bestimmte Dateien und Ordner aus dem Webpaket auszuschließen, wenn Sie das Projekt erstellen.

**So schließen Sie Dateien und Ordner von einem Webbereitstellungs Paket aus**

1. Öffnen Sie die Projekt Mappe in Visual Studio 2010.
2. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf den Webanwendungs Projekt Knoten (z. **b. ContactManager. MVC**), zeigen Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**.
3. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **XML-Datei** aus.
4. Geben Sie im Feld **Name den Namen** *[Project Name] * * *. WPP. targets** (z **. b. ContactManager. MVC. WPP. targets**) ein, und klicken Sie dann auf **Hinzufügen**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Wenn Sie dem Stamm Knoten eines Projekts ein neues Element hinzufügen, wird die Datei im selben Ordner wie die Projektdatei erstellt. Sie können dies überprüfen, indem Sie den Ordner in Windows-Explorer öffnen.
5. Fügen Sie in der-Datei ein **Project** -Element und ein **ItemGroup** -Element hinzu:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Wenn Sie Ordner aus dem Webpaket ausschließen möchten, fügen Sie dem **ItemGroup** -Element ein **excludefrompackagefolders** -Element hinzu:

   1. Geben Sie im **include** -Attribut eine durch Semikolons getrennte Liste der Ordner an, die Sie ausschließen möchten.
   2. Geben Sie im **fromtarget** -Metadatenelement einen sinnvollen Wert an, um anzugeben, warum die Ordner ausgeschlossen werden, wie z. b. den Namen der *WPP. targets* -Datei.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Wenn Sie Dateien aus dem Webpaket ausschließen möchten, fügen Sie dem **ItemGroup** -Element ein **excludefrompackagefiles** -Element hinzu:

   1. Geben Sie im **include** -Attribut eine durch Semikolons getrennte Liste der Dateien an, die Sie ausschließen möchten.
   2. Geben Sie im **fromtarget** -Metadatenelement einen sinnvollen Wert an, um anzugeben, warum die Dateien ausgeschlossen werden, wie z. b. den Namen der *WPP. targets* -Datei.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. Die Datei *[Project Name]. WPP. targets* sollte nun wie folgt aussehen:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Speichern und schließen Sie die Datei *[Project Name]. WPP. targets* .

Wenn Sie das nächste Mal das Webanwendungs Projekt erstellen und packen, wird die WPP *. targets* -Datei automatisch von WPP erkannt. Dateien und Ordner, die Sie angegeben haben, werden nicht in das Webpaket eingeschlossen.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie Sie beim Erstellen eines Webpakets bestimmte Dateien und Ordner ausschließen, indem Sie eine benutzerdefinierte *WPP. targets* -Datei im gleichen Ordner wie die Projektdatei Ihrer Webanwendung erstellen.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zur Verwendung von Projektdateien für benutzerdefinierte Microsoft-Build-Engine (MSBuild) zum Steuern des Bereitstellungs Prozesses finden Sie Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen zum Verpackungs-und Bereitstellungs Prozess finden Sie unter [Erstellung und Verpacken von Webanwendungs Projekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurieren von Parametern für die Webpaket Bereitstellung und bereit](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)stellen von [Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Zurück](deploying-membership-databases-to-enterprise-environments.md)
> [Weiter](taking-web-applications-offline-with-web-deploy.md)
