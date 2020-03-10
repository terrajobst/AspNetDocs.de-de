---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Erstellen und Ausführen einer Befehlsdatei für die Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie eine Befehlsdatei erstellen, mit der Sie eine Bereitstellung mit Microsoft-Build-Engine-Projektdateien (MSBuild) als Einzelschritt ausführen können.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514977"
---
# <a name="creating-and-running-a-deployment-command-file"></a>Erstellen und Ausführen einer Befehlsdatei für die Bereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie eine Befehlsdatei erstellen können, mit der Sie eine Bereitstellung mithilfe Microsoft-Build-Engine (MSBuild)-Projektdateien als wiederholbaren Prozess mit einem Schritt ausführen können.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der&#x2014; [Contact Manager](the-contact-manager-solution.md) -Lösung verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Mittelpunkt dieser Tutorials basiert auf dem Untergrund Legendes [zum Buildprozess](understanding-the-build-process.md)beschriebenen Ansatz der Split Project-Datei, in dem der Buildprozess von zwei&#x2014;Projektdateien gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="process-overview"></a>Prozessübersicht

In diesem Thema erfahren Sie, wie Sie eine Befehlsdatei erstellen und ausführen, die diese Projektdateien verwendet, um eine wiederholbare Bereitstellung in Ihrer Zielumgebung auszuführen. Im Wesentlichen muss die Befehlsdatei lediglich einen MSBuild-Befehl enthalten, der Folgendes enthält:

- Weist MSBuild an, die Umgebungs agnostische Datei *Publish. proj* auszuführen.
- Teilt der *Publish. proj* -Datei mit, welche Datei die Umgebungs spezifischen Projekteinstellungen enthält und wo Sie gefunden werden soll.

## <a name="create-an-msbuild-command"></a>Erstellen eines MSBuild-Befehls

Wie in Grund [Legendes zum Buildprozess](understanding-the-build-process.md)beschrieben, ist die Umgebungs spezifische&#x2014;Projektdatei z. b. " *ESV-dev. proj*&#x2014;" so konzipiert, dass Sie zur Buildzeit in die Umgebungs unabhängige Datei " *Publish. proj* " importiert wird. Diese beiden Dateien enthalten gemeinsame Anweisungen zum Erstellen und Bereitstellen Ihrer Lösung in MSBuild.

Die Datei *Publish. proj* verwendet ein **Import** -Element, um die Umgebungs spezifische Projektdatei zu importieren.

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

Wenn Sie MSBuild. exe zum Erstellen und Bereitstellen der Contact Manager-Lösung verwenden, müssen Sie die folgenden Schritte ausführen:

- Führen Sie MSBuild. exe für die Datei *Publish. proj* aus.
- Geben Sie den Speicherort der Umgebungs spezifischen Projektdatei an, indem Sie einen Befehlszeilenparameter mit dem Namen " **targetenvpropsfile**" bereitstellen.

Zu diesem Zweck sollte der MSBuild-Befehl wie folgt aussehen:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

Von hier aus ist es ein einfacher Schritt, zu einer wiederholbaren, einstufigen Bereitstellung zu wechseln. Sie müssen lediglich ihren MSBuild-Befehl zu einer. cmd-Datei hinzufügen. In der Lösung "Contact Manager" enthält der Ordner "Publish" eine Datei mit dem Namen " *Publish-dev. cmd* ", die genau dies tut.

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> Der **/FL** -Schalter weist MSBuild an, eine Protokolldatei mit dem Namen " *MSBuild. log* " im Arbeitsverzeichnis zu erstellen, in dem "MSBuild. exe" aufgerufen wurde.

Zum Bereitstellen oder erneuten Bereitstellen der Contact Manager-Lösung müssen Sie lediglich die Datei *Publish-dev. cmd* ausführen. Wenn Sie die Datei ausführen, führt MSBuild Folgendes aus:

- Erstellen Sie alle Projekte in der Projekt Mappe.
- Generieren von bereitstell baren Webpaketen für die Webanwendungs Projekte.
- Generieren Sie dbschema-und DeployManifest-Dateien für die Datenbankprojekte.
- Stellen Sie die Webpakete auf dem Webserver bereit.
- Stellen Sie die Datenbank auf dem Datenbankserver bereit.

## <a name="run-the-deployment"></a>Ausführen der Bereitstellung

Wenn Sie eine Befehlsdatei für die Zielumgebung erstellt haben, sollten Sie die gesamte Bereitstellung ausführen können, indem Sie einfach die Datei ausführen.

**So stellen Sie die Contact Manager-Lösung für die Testumgebung bereit**

1. Öffnen Sie Windows-Explorer auf Ihrer Entwickler Arbeitsstation, und navigieren Sie dann zum Speicherort der Datei " *Publish-dev. cmd* ".
2. Doppelklicken Sie auf die Datei, um Sie auszuführen.
3. Wenn das Dialogfeld **Datei öffnen – Sicherheitswarnung** angezeigt wird, klicken Sie auf **Ausführen**.
4. Wenn Ihre Konfigurationseinstellungen und Testserver ordnungsgemäß eingerichtet sind, wird im Eingabe Aufforderungs Fenster eine Meldung zum erfolgreichen **Erstellen** angezeigt, wenn die Verarbeitung der Projektdateien durch MSBuild abgeschlossen ist.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Wenn Sie die Lösung zum ersten Mal in dieser Umgebung bereitgestellt haben, müssen Sie das Computer Konto des Testweb Servers der **DB-\_-DataWriter** -und DB- **\_DataReader** -Rollen in der Datenbank **ContactManager** hinzufügen. Dieses Verfahren wird unter [Konfigurieren eines Datenbankservers für die Web deploy Veröffentlichung](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)beschrieben.

    > [!NOTE]
    > Sie müssen diese Berechtigungen nur zuweisen, wenn Sie die Datenbank erstellen. Standardmäßig wird die Datenbank nicht bei jeder Bereitstellung&#x2014;vom Buildprozess neu erstellt. stattdessen wird die vorhandene Datenbank mit dem aktuellen Schema verglichen, und es werden nur die erforderlichen Änderungen vorgenommen. Daher sollten Sie diese Daten bankrollen nur bei der erstmaligen Bereitstellung der Lösung zuordnen.
6. Öffnen Sie Internet Explorer, und navigieren Sie zur URL der Kontakt-Manager-Anwendung (z. b. `http://testweb1:85/ContactManager/`).
7. Überprüfen Sie, ob die Anwendung erwartungsgemäß funktioniert, und Sie können Kontakte hinzufügen.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Zusammenfassung

Das Erstellen einer Befehlsdatei, die ihre MSBuild-Anweisungen enthält, bietet eine schnelle und einfache Möglichkeit zum Erstellen und Bereitstellen einer Lösung mit mehreren Projekten in einer bestimmten Zielumgebung. Wenn Sie die Lösung wiederholt in mehreren Ziel Umgebungen bereitstellen müssen, können Sie mehrere Befehls Dateien erstellen. In jeder Befehlsdatei wird mit dem MSBuild-Befehl dieselbe universelle Projektdatei erstellt, aber es wird eine andere Umgebungs spezifische Projektdatei angegeben. Beispielsweise kann eine Befehlsdatei zum Veröffentlichen in einer Entwickler-oder Testumgebung diesen MSBuild-Befehl enthalten:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

Eine Befehlsdatei zum Veröffentlichen in einer Stagingumgebung enthält möglicherweise diesen MSBuild-Befehl:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> Anleitungen zum Anpassen der Umgebungs spezifischen Projektdateien für Ihre eigenen Serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungs Eigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Sie können den Buildprozess für jede Umgebung auch anpassen, indem Sie die Eigenschaften überschreiben oder verschiedene andere Switches im MSBuild-Befehl festlegen. Weitere Informationen finden Sie unter [MSBuild-Befehlszeilen Referenz](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Zurück](deploying-database-projects.md)
> [Weiter](manually-installing-web-packages.md)
