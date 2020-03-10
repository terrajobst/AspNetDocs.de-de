---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Problembehandlung beim Verpackungsprozess | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie ausführliche Informationen zum Verpackungsprozess sammeln können, indem Sie die enablepackageprocessloggingandassert-Eigenschaft im M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 8ad649dfff085a8774cc13c11d8a3e3d48277d66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509751"
---
# <a name="troubleshooting-the-packaging-process"></a>Behandeln von Problemen mit dem Paketerstellungsprozess

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie ausführliche Informationen zum Verpackungsprozess sammeln können, indem Sie die **enablepackageprocessloggingandassert** -Eigenschaft in der Microsoft-Build-Engine (MSBuild) verwenden.
> 
> Wenn Sie die **enablepackageprocessloggingandassert** -Eigenschaft auf **true**festlegen, führt MSBuild Folgendes aus:
> 
> - Fügen Sie den buildprotokollen Weitere Informationen zum Verpackungsprozess hinzu.
> - Protokollieren Sie Fehler unter bestimmten Bedingungen, z. b. Wenn in der Paketierungs Liste doppelte Dateien gefunden werden.
> - Erstellen Sie ein Protokoll Verzeichnis im Ordner *ProjectName*\_Paket, und verwenden Sie es, um Informationen zu den Dateien aufzuzeichnen, die Sie Verpacken.
> 
> Wenn der Paketierungs Prozess fehlschlägt oder Ihre Webbereitstellungs Pakete nicht die erwarteten Dateien enthalten, können Sie diese Informationen verwenden, um eine Problembehandlung für den Prozess auszuführen und die Ursache für den Fehler zu ermitteln.
> 
> > [!NOTE]
> > Die **enablepackageprocessloggingandassert** -Eigenschaft funktioniert nur, wenn Sie das Projekt mithilfe der **Debugkonfiguration** erstellen. Die-Eigenschaft wird in anderen Konfigurationen ignoriert.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Grundlegendes zur enablepackageprocessloggingandassert-Eigenschaft

[Erstellen und Verpacken von Webanwendungs Projekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) beschreiben, wie die Web Publishing Pipeline (WPP) einen Satz von MSBuild-Zielen bereitstellt, die die Funktionalität von MSBuild erweitern und die Integration mit dem Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) ermöglichen. Wenn Sie ein Webanwendungs Projekt Verpacken, rufen Sie WPP-Ziele auf.

Viele dieser WPP-Ziele enthalten bedingte Logik, die zusätzliche Informationen protokolliert, wenn die **enablepackageprocessloggingandassert** -Eigenschaft auf **true**festgelegt ist. Wenn Sie das **Paket** Ziel beispielsweise überprüfen, können Sie sehen, dass es ein zusätzliches Protokoll Verzeichnis erstellt und eine Liste von Dateien in eine Textdatei schreibt, wenn **enablepackageprocessloggingandassert** gleich **true**ist.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]

> [!NOTE]
> Die WPP-Ziele werden in der Datei " *Microsoft. Web. Publishing. targets* " im Ordner "% Program Files (x86)% \ msbuild\microsoft\visualstudio\v10.0\web" definiert. Sie können diese Datei öffnen und die Ziele in Visual Studio 2010 oder in einem beliebigen XML-Editor überprüfen. Achten Sie darauf, den Inhalt der Datei nicht zu ändern.

## <a name="enabling-the-additional-logging"></a>Aktivieren der zusätzlichen Protokollierung

Sie können einen Wert für die **enablepackageprocessloggingandassert** -Eigenschaft auf verschiedene Weise angeben, je nachdem, wie Sie das Projekt erstellen.

Wenn Sie das Projekt über die Befehlszeile erstellen, können Sie einen Wert für die **enablepackageprocessloggingandassert** -Eigenschaft als Befehlszeilenargument angeben:

[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]

Wenn Sie zum Erstellen Ihrer Projekte eine benutzerdefinierte Projektdatei verwenden, können Sie den **enablepackageprocessloggingandassert** -Wert in das **Properties** -Attribut der **MSBuild** -Aufgabe einschließen:

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]

Wenn Sie eine Team Foundation Server (TFS)-Builddefinition verwenden, um Ihre Projekte zu erstellen, können Sie einen Wert für die **enablepackageprocessloggingandassert** -Eigenschaft in der Zeile **MSBuild-Argumente** angeben:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Weitere Informationen zum Erstellen und Konfigurieren von Builddefinitionen finden Sie [unter Erstellen einer Builddefinition, die die Bereitstellung unterstützt](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Wenn Sie das Paket in jedem Build einschließen möchten, können Sie die Projektdatei für das Webanwendungs Projekt ändern, um die **enablepackageprocessloggingandassert** -Eigenschaft auf **true**festzulegen. Sie sollten die-Eigenschaft dem ersten **PropertyGroup** -Element in der CSPROJ-oder VBPROJ-Datei hinzufügen.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]

## <a name="reviewing-the-log-files"></a>Überprüfen der Protokolldateien

Wenn Sie ein Webanwendungs Projekt erstellen und packen, bei dem **enablepackageprocessloggingandassert** auf **true**festgelegt ist, erstellt MSBuild einen zusätzlichen Ordner mit dem Namen log im Ordner *ProjectName*\_Paket. Der Protokoll Ordner enthält verschiedene Dateien:

![](troubleshooting-the-packaging-process/_static/image2.png)

Die Liste der Dateien, die Sie sehen, variiert je nach den Dingen in Ihrem Projekt und Ihrem Buildprozess. Diese Dateien werden jedoch in der Regel verwendet, um die Liste der Dateien aufzuzeichnen, die von WPP für die Paket Erstellung in verschiedenen Phasen des Prozesses gesammelt werden:

- In der Datei " *preexcludepipelinecollectfilesphasefilelist. txt* " werden die Dateien aufgelistet, die von MSBuild für die Paket Erstellung gesammelt werden, bevor Dateien entfernt werden, die für den Ausschluss angegeben wurden.
- Die Datei " *afterexcludefilesfileslist. txt* " enthält die geänderte Datei Liste, nachdem alle Dateien entfernt wurden, die für den Ausschluss angegeben wurden.

    > [!NOTE]
    > Weitere Informationen zum Ausschließen von Dateien und Ordnern aus dem Verpackungsprozess finden Sie unter [Ausschließen von Dateien und Ordnern aus der Bereitstellung](excluding-files-and-folders-from-deployment.md).
- In der Datei " *aftertransformwebconfig. txt* " werden die Dateien aufgelistet, die für die Paket Erstellung gesammelt werden, nachdem *Web. config* -Transformationen ausgeführt wurden. In dieser Liste werden alle Konfigurations spezifischen *Web. config* -Transformations Dateien wie " *Web. Debug. config* " und " *Web. Release. config*" aus der Liste der Dateien für die Paket Erstellung ausgeschlossen. An ihrer Stelle ist eine einzelne transformierte *Web. config* -Datei enthalten.
- Die Datei " *postautoparameterizationwebconfigconnectionstrings. txt* " enthält die Liste der Dateien, nachdem die Verbindungs Zeichenfolgen in der Datei " *Web. config* " parametrisiert wurden. Dies ist der Prozess, mit dem Sie die Verbindungs Zeichenfolgen durch die richtigen Einstellungen für Ihre Zielumgebung ersetzen können, wenn Sie das Paket bereitstellen.
- Die Datei *prepackage. txt* enthält die fertige präbuildliste der Dateien, die in das Paket eingeschlossen werden sollen.

> [!NOTE]
> Die Namen der zusätzlichen Protokolldateien entsprechen in der Regel WPP-Zielen. Sie können diese Ziele überprüfen, indem Sie die Datei " *Microsoft. Web. Publishing. targets* " im Ordner "% Program Files (x86)% \ msbuild\microsoft\visualstudio\v10.0\web" überprüfen.

Wenn die Inhalte Ihres Webpakets nicht Ihren Erwartungen entsprechen, können Sie diese Dateien überprüfen, um zu ermitteln, an welcher Stelle im Prozess ein Fehler aufgetreten ist.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie Sie die **enablepackageprocessloggingandassert** -Eigenschaft in MSBuild verwenden können, um Probleme mit dem Verpackungsprozess zu beheben. Es wurden die verschiedenen Methoden erläutert, mit denen Sie den Eigenschafts Wert für den Buildprozess bereitstellen können. Außerdem wurden die zusätzlichen Informationen beschrieben, die aufgezeichnet werden, wenn Sie die-Eigenschaft auf **true**festlegen.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zur Verwendung von benutzerdefinierten MSBuild-Projektdateien zum Steuern des Bereitstellungs Prozesses finden Sie Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen zu WPP und zur Verwaltung des Verpackungsprozesses finden Sie unter [Building and Packaging Web Application Projects (entwickeln und Verpacken von Webanwendungs Projekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)). Anleitungen zum Ausschließen bestimmter Dateien und Ordner aus Webbereitstellungs Paketen finden Sie unter [Ausschließen von Dateien und Ordnern aus der Bereitstellung](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Previous](running-windows-powershell-scripts-from-msbuild-project-files.md)
