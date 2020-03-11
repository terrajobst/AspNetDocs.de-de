---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Bereitstellen eines bestimmten Builds | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Webpakete und Daten Bank Skripts aus einem bestimmten vorherigen Build an einem neuen Ziel bereitgestellt werden, wie z. b. eine Staging-oder Produktionsumgebung...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6bede6b36c24ade928ab052e14daec1e017bd0b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519573"
---
# <a name="deploying-a-specific-build"></a>Bereitstellen eines bestimmten Builds

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie Webpakete und Daten Bank Skripts aus einem bestimmten vorherigen Build in einem neuen Ziel wie einer Staging-oder Produktionsumgebung bereitstellen.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz zum Aufteilen von Projektdateien, bei dem der Build-und Bereitstellungs Prozess von zwei Projekt&#x2014;Dateien gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="task-overview"></a>Aufgaben Übersicht

Bisher haben sich die Themen in diesem Lernprogramm Satz auf das Erstellen, Verpacken und Bereitstellen von Webanwendungen und Datenbanken im Rahmen eines einstufigen oder automatisierten Prozesses konzentriert. In einigen gängigen Szenarien sollten Sie jedoch die Ressourcen auswählen, die Sie aus einer Liste von Builds in einem Ablage Ordner bereitstellen. Mit anderen Worten: der neueste Build ist möglicherweise nicht der Build, den Sie bereitstellen möchten.

Beachten Sie das im vorherigen Thema beschriebene Continuous Integration Szenario (CI), [das Erstellen einer Builddefinition, die die Bereitstellung unterstützt](creating-a-build-definition-that-supports-deployment.md). Sie haben eine Builddefinition in Team Foundation Server (TFS) 2010 erstellt. Jedes Mal, wenn ein Entwickler Code in TFS prüft, erstellt Team Build ihren Code, erstellt im Rahmen des Buildprozesses Webpakete und Daten Bank Skripts, führt alle Komponententests aus und stellt ihre Ressourcen in einer Testumgebung bereit. Abhängig von der Beibehaltungs Richtlinie, die Sie beim Erstellen der Builddefinition konfiguriert haben, behält TFS eine bestimmte Anzahl vorheriger Builds bei.

![](deploying-a-specific-build/_static/image1.png)

Angenommen, Sie haben Überprüfungs-und Validierungstests für einen dieser Builds in der Testumgebung durchgeführt, und Sie können Ihre Anwendung in einer Stagingumgebung bereitstellen. In der Zwischenzeit haben Entwickler möglicherweise neuen Code eingeglichen. Sie möchten die Projekt Mappe nicht neu erstellen und in der Stagingumgebung bereitstellen, und Sie möchten den neuesten Build nicht in der Stagingumgebung bereitstellen. Stattdessen möchten Sie den spezifischen Build bereitstellen, den Sie auf den Testservern überprüft und überprüft haben.

Um dies zu erreichen, müssen Sie dem Microsoft-Build-Engine (MSBuild) mitteilen, wo die von einem bestimmten Build generierten Webpakete und Daten Bank Skripts zu finden sind.

## <a name="overriding-the-outputroot-property"></a>Überschreiben der outputroot-Eigenschaft

In der [Beispiellösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)deklariert die Datei *Publish. proj* eine Eigenschaft mit dem Namen **outputroot**. Wie der Name bereits vermuten lässt, ist dies der Stamm Ordner, der alle Elemente enthält, die vom Buildprozess generiert werden. In der Datei *Publish. proj* sehen Sie, dass sich die **outputroot** -Eigenschaft auf den Stamm Speicherort für alle Bereitstellungs Ressourcen bezieht.

> [!NOTE]
> **Outputroot** ist ein häufig verwendeter Eigenschaftsname. Visual C# -und Visual Basic-Projektdateien deklarieren diese Eigenschaft auch, um den Stamm Speicherort für alle Buildausgaben zu speichern.

[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]

Wenn Sie möchten, dass die Projektdatei Webpakete und Daten Bank Skripts von einem&#x2014;anderen Speicherort wie die Ausgaben eines vorherigen TFS-Builds&#x2014;bereitstellt, müssen Sie lediglich die **outputroot** -Eigenschaft überschreiben. Sie sollten den Eigenschafts Wert auf den entsprechenden Buildordner auf dem Team Build Server festlegen. Wenn Sie MSBuild über die Befehlszeile ausgeführt haben, können Sie einen Wert für **outputroot** als Befehlszeilenargument angeben:

[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]

In der Praxis möchten Sie jedoch auch das **Build** -Ziel&#x2014;überspringen, wenn Sie nicht beabsichtigen, die Buildausgaben zu verwenden. Hierzu können Sie die Ziele angeben, die Sie von der Befehlszeile aus ausführen möchten:

[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]

In den meisten Fällen möchten Sie jedoch die Bereitstellungs Logik in eine TFS-Builddefinition erstellen. Dies ermöglicht Benutzern mit der **Warteschlangen** Erstellung die Berechtigung, die Bereitstellung aus einer beliebigen Visual Studio-Installation mit einer Verbindung mit dem TFS-Server zu initiieren.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Erstellen einer Builddefinition zum Bereitstellen bestimmter Builds

Im nächsten Verfahren wird beschrieben, wie Sie eine Builddefinition erstellen, die es Benutzern ermöglicht, bereit Stellungen in einer Stagingumgebung mit einem einzigen Befehl zu initiieren.

In diesem Fall möchten Sie nicht, dass die Builddefinition tatsächlich alles&#x2014;erstellt, was Sie nur zum Ausführen der Bereitstellungs Logik in der benutzerdefinierten Projektdatei benötigen. Die Datei *Publish. proj* enthält bedingte Logik, die das Buildziel **über** springt, wenn die Datei in Team Build ausgeführt wird. Dies erfolgt durch Auswerten der integrierten **buildinginteambuild** -Eigenschaft, die automatisch auf **true** festgelegt wird, wenn Sie die Projektdatei in Team Build ausführen. Daher können Sie den Buildprozess überspringen und einfach die Projektdatei ausführen, um einen vorhandenen Build bereitzustellen.

**So erstellen Sie eine Builddefinition für die manuelle Triggerbereitstellung**

1. Erweitern Sie in Visual Studio 2010 im Fenster **Team Explorer** den Knoten Team Projekt, klicken Sie mit der rechten Maustaste auf **Builds**, und klicken Sie dann auf **neue Builddefinition**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Geben Sie auf der Registerkarte **Allgemein** einen Namen für die Builddefinition (z. b. **deploydestaging**) und eine optionale Beschreibung ein.
3. Wählen Sie **auf der Register** Karte "Registerkarte" die Option **manuell – Check-ins keinen neuen Build**aus.
4. Geben Sie auf der Registerkarte **Build-Standardwerte** im Feld Buildausgabe in **den folgenden Ablage Ordner kopieren** den Universal Naming Convention (UNC)-Pfad des Ablage Ordners ein (z. **b.\\tfsbuild\drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Belassen Sie auf der Registerkarte **Prozess** in der Dropdown Liste **buildprozessdatei** die Option **DefaultTemplate. XAML** . Dies ist eine der Standard-buildprozessvorlagen, die allen neuen Team Projekten hinzugefügt werden.
6. Klicken Sie in der Tabelle **buildprozessparameter** **auf die Zeile** **Elemente zum Erstellen** , und klicken Sie dann auf die Schaltfläche mit den Auslassungs Zeichen

    ![](deploying-a-specific-build/_static/image4.png)
7. Klicken Sie im Dialogfeld **zu Buildelemente** auf **Hinzufügen**.
8. Wählen Sie in der Dropdown Liste **Elemente des Typs** die Option **MSBuild-Projektdateien**aus.
9. Navigieren Sie zum Speicherort der benutzerdefinierten Projektdatei, mit der Sie den Bereitstellungs Prozess steuern, wählen Sie die Datei aus, und klicken Sie dann auf **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. Klicken Sie im Dialogfeld **zu Buildelemente** auf **OK**.
11. Erweitern Sie in der Tabelle **buildprozessparameter** den Abschnitt **erweitert** .
12. Geben Sie in der Zeile **MSBuild-Argumente** den Speicherort der Umgebungs spezifischen Projektdatei an, und fügen Sie einen Platzhalter für den Speicherort des buildordners hinzu:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Sie müssen den **outputroot** -Wert jedes Mal überschreiben, wenn Sie einen Build in die Warteschlange stellen. Dies wird im nächsten Verfahren behandelt.
13. Klicken Sie auf **Speichern**.

Wenn Sie einen Build auslöst, müssen Sie die **outputroot** -Eigenschaft aktualisieren, um auf den Build zu verweisen, den Sie bereitstellen möchten.

**So stellen Sie einen bestimmten Build aus einer Builddefinition bereit**

1. Klicken Sie im **Team Explorer** Fenster mit der rechten Maustaste auf die Builddefinition, und klicken Sie dann auf **neuen Build in Warteschlange**.

    ![](deploying-a-specific-build/_static/image7.png)
2. Erweitern Sie im Dialogfeld **Warteschlangen Erstellung** auf der Registerkarte **Parameter** den Abschnitt **erweitert** .
3. Ersetzen Sie in der Zeile **MSBuild-Argumente** den Wert der **outputroot** -Eigenschaft durch den Speicherort des buildordners. Zum Beispiel:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Stellen Sie sicher, dass Sie am Ende des Pfads zum Buildordner einen nachgestellten Schrägstrich einschließen.
4. Klicken Sie auf **Queue**.

Wenn Sie den Build in die Warteschlange stellen, stellt die Projektdatei die Daten Bank Skripts und Webpakete aus dem Build Drop-Ordner bereit, den Sie in der **outputroot** -Eigenschaft angegeben haben

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema wird beschrieben, wie Bereitstellungs Ressourcen wie Webpakete und Daten Bank Skripts aus einem bestimmten vorherigen Build mit dem Projekt Bereitstellungs Modell "Split Project file" veröffentlicht werden. Es wurde erläutert, wie Sie die **outputroot** -Eigenschaft überschreiben und die Bereitstellungs Logik in eine TFS-Builddefinition integrieren.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Erstellen von Builddefinitionen finden Sie unter [Erstellen einer grundlegenden Builddefinition](https://msdn.microsoft.com/library/ms181716.aspx) und definieren des Buildprozesses. [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx) Weitere Informationen zur Warteschlange von Builds finden Sie unter [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Zurück](creating-a-build-definition-that-supports-deployment.md)
> [Weiter](configuring-permissions-for-team-build-deployment.md)
