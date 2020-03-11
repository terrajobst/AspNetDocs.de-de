---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Erstellen einer Builddefinition, die Bereitstellung unterstützt | Microsoft-Dokumentation
author: jrjlee
description: Wenn Sie eine beliebige Art von Build in Team Foundation Server (TFS) 2010 ausführen möchten, müssen Sie eine Builddefinition innerhalb des Team Projekts erstellen. In diesem Thema wird...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e11c91a824446572aaf0b3bc6954b9b8ffb4eaff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78489009"
---
# <a name="creating-a-build-definition-that-supports-deployment"></a>Erstellen einer Builddefinition mit Unterstützung für Bereitstellungen

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie eine beliebige Art von Build in Team Foundation Server (TFS) 2010 ausführen möchten, müssen Sie eine Builddefinition innerhalb des Team Projekts erstellen. In diesem Thema wird beschrieben, wie Sie eine neue Builddefinition in TFS erstellen und wie Sie die Webbereitstellung als Teil des Buildprozesses in Team Build steuern können.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz zum Aufteilen von Projektdateien, bei dem der Build-und Bereitstellungs Prozess von zwei Projekt&#x2014;Dateien gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="task-overview"></a>Aufgaben Übersicht

Eine Builddefinition ist der Mechanismus, mit dem gesteuert wird, wie und wann Builds für Team Projekte in TFS auftreten. Jede Builddefinition gibt Folgendes an:

- Die Dinge, die Sie erstellen möchten, z. b. Visual Studio-Projektmappendateien oder Projektdateien für benutzerdefinierte Microsoft-Build-Engine (MSBuild)
- Die Kriterien, die bestimmen, wann ein Build stattfinden soll, wie z. b. manuelle Trigger, Continuous Integration (CI) oder Gated-Check-ins.
- Der Speicherort, an den der TeamBuild Buildausgaben senden soll, einschließlich Bereitstellungs Artefakte wie Webpaketen und Daten Bank Skripts.
- Die Zeitspanne, für die jeder Build beibehalten werden soll.
- Verschiedene andere Parameter des Buildprozesses.

> [!NOTE]
> Weitere Informationen zu Builddefinitionen finden [Sie unter Definieren des Buildprozesses](https://msdn.microsoft.com/library/ms181715.aspx).

In diesem Thema erfahren Sie, wie Sie eine Builddefinition erstellen, die CI verwendet, sodass ein Build ausgelöst wird, wenn ein Entwickler neuen Inhalt prüft. Wenn der Build erfolgreich ist, führt der Builddienst eine benutzerdefinierte Projektdatei aus, um die Lösung in einer Testumgebung bereitzustellen.

Wenn Sie einen Build auslöst, müssen diese Aktionen ausgeführt werden:

- Zuerst sollte Team Build die Projekt Mappe erstellen. Im Rahmen dieses Vorgangs wird die Web Publishing Pipeline (WPP) von Team Build aufgerufen, um Webbereitstellungs Pakete für jedes Webanwendungs Projekt in der Projekt Mappe zu generieren. Team Build führt außerdem alle Komponententests aus, die der Projekt Mappe zugeordnet sind.
- Wenn der Projektmappenbuild fehlschlägt, sollte Team Build keine weiteren Maßnahmen ergreifen. Komponenten Test Fehler sollten als Buildfehler behandelt werden.
- Wenn der Projektmappenbuild erfolgreich erstellt wurde, sollte Team Build die benutzerdefinierte Projektdatei ausführen, mit der die Bereitstellung der Lösung gesteuert wird. Im Rahmen dieses Prozesses ruft Team Build das Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) auf, um die gepackten Webanwendungen auf den Zielweb Servern zu installieren, und ruft das Hilfsprogramm "VSDBCMD. exe" auf, um die Daten Bank Erstellung auszuführen. Skripts auf den Ziel Datenbankservern.

Dies veranschaulicht den Prozess:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

Die Beispiellösung [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) enthält eine benutzerdefinierte MSBuild-Projektdatei ( *Publish. proj*), die Sie von MSBuild oder Team Build ausführen können. Wie in Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md)beschrieben, definiert diese Projektdatei die Logik, mit der Ihre Webpakete und Datenbanken in einer Zielumgebung bereitgestellt werden. Die Datei enthält Logik, bei der der Erstellungs-und Paket Erstellungs Prozess ausgelassen wird, wenn er im TeamBuild ausgeführt wird, sodass nur die Bereitstellungs Aufgaben ausgeführt werden. Dies liegt daran, dass Sie bei der Automatisierung der Bereitstellung auf diese Weise sicherstellen möchten, dass die Lösung erfolgreich erstellt wird und alle Komponententests durchgeführt werden, bevor der Bereitstellungs Prozess startet.

Im nächsten Abschnitt wird erläutert, wie dieser Prozess durch Erstellen einer neuen Builddefinition implementiert wird.

> [!NOTE]
> Dieses Verfahren&#x2014;, bei dem ein einzelner automatisierter Prozess eine Lösung&#x2014;erstellt, testet und bereitstellt, ist wahrscheinlich am besten für die Bereitstellung in Testumgebungen geeignet. Für Staging-und Produktionsumgebungen ist es viel wahrscheinlicher, dass Sie Inhalte aus einem vorherigen Build bereitstellen möchten, den Sie bereits in einer Testumgebung überprüft und überprüft haben. Diese Vorgehensweise wird im nächsten Thema, dem bereitstellen [eines bestimmten Builds](deploying-a-specific-build.md), beschrieben.

### <a name="who-performs-this-procedure"></a>Wer führt dieses Verfahren aus?

In der Regel führt ein TFS-Administrator dieses Verfahren aus. In einigen Fällen kann ein Entwicklerteam Leiter die Verantwortung für die Teamprojekt Sammlung in TFS übernehmen. Um eine neue Builddefinition zu erstellen, müssen Sie Mitglied der Gruppe Projekt Auflistungs- **buildadministratoren** für die Teamprojekt Sammlung sein, in der die Projekt Mappe enthalten ist.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Erstellen einer Builddefinition für CI und Bereitstellung

Im nächsten Verfahren wird beschrieben, wie Sie eine Builddefinition erstellen, die von CI ausgelöst wird. Wenn der Build erfolgreich ist, wird die Lösung mithilfe der Logik in einer benutzerdefinierten MSBuild-Projektdatei bereitgestellt.

**So erstellen Sie eine Builddefinition für CI und die Bereitstellung**

1. Erweitern Sie in Visual Studio 2010 im Fenster **Team Explorer** den Knoten Team Projekt, klicken Sie mit der rechten Maustaste auf **Builds**, und klicken Sie dann auf **neue Builddefinition**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Geben Sie auf der Registerkarte **Allgemein** einen Namen für die Builddefinition (z. b. **deploydetest**) und eine optionale Beschreibung ein.
3. Wählen Sie auf der Registerkarte "Registerkarte **" die Kriterien** aus, auf denen ein neuer Build auslöst werden soll. Wenn Sie z. b. die Projekt Mappe erstellen und in der Testumgebung bereitstellen möchten, wenn ein Entwickler neuen Code eincheckt, wählen Sie **Continuous Integration**aus.
4. Geben Sie auf der Registerkarte **Build-Standardwerte** im Feld Buildausgabe in **den folgenden Ablage Ordner kopieren** den Universal Naming Convention (UNC)-Pfad des Ablage Ordners ein (z. **b.\\tfsbuild\drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > In diesem Ablage Speicherort werden mehrere Builds gespeichert, abhängig von der konfigurierten Aufbewahrungs Richtlinie. Wenn Sie Bereitstellungs Artefakte aus einem bestimmten Build in einer Staging-oder Produktionsumgebung veröffentlichen möchten, finden Sie diese hier.
5. Belassen Sie auf der Registerkarte **Prozess** in der Dropdown Liste **buildprozessdatei** die Option **DefaultTemplate. XAML** . Dies ist eine der Standard-buildprozessvorlagen, die allen neuen Team Projekten hinzugefügt werden.
6. Klicken Sie in der Tabelle **buildprozessparameter** **auf die Zeile** **Elemente zum Erstellen** , und klicken Sie dann auf die Schaltfläche mit den Auslassungs Zeichen

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. Klicken Sie im Dialogfeld **zu Buildelemente** auf **Hinzufügen**.
8. Navigieren Sie zum Speicherort Ihrer Projektmappendatei, und klicken Sie dann auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. Klicken Sie im Dialogfeld **zu Buildelemente** auf **Hinzufügen**.
10. Wählen Sie in der Dropdown Liste **Elemente des Typs** die Option **MSBuild-Projektdateien**aus.
11. Navigieren Sie zum Speicherort der benutzerdefinierten Projektdatei, mit der Sie den Bereitstellungs Prozess steuern, wählen Sie die Datei aus, und klicken Sie dann auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. Im Dialogfeld **zu Buildelemente** sollten jetzt zwei Elemente angezeigt werden. Klicken Sie auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Erweitern Sie auf der Registerkarte **Prozess** in der Tabelle **buildprozessparameter** **den Abschnitt erweitert** .
14. Fügen Sie in der Zeile **MSBuild-Argumente** alle MSBuild-Befehlszeilenargumente hinzu *, die für die Erstellung* eines der Elemente erforderlich sind. Im Szenario der Kontakt-Manager-Lösung sind diese Argumente erforderlich:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. Für dieses Beispiel gilt Folgendes:

    1. Die Argumente **deployonbuild = true** und **deploytarget = Package** sind erforderlich, wenn Sie die Contact Manager-Lösung erstellen. Dies weist MSBuild an, nach dem Erstellen der einzelnen Webanwendungs Projekte Webbereitstellungs Pakete zu erstellen, wie unter [Erstellen und Verpacken von Webanwendungs Projekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)beschrieben.
    2. Das **targetenvpropsfile** -Argument ist erforderlich, wenn Sie die *Publish. proj* -Datei erstellen. Diese Eigenschaft gibt den Speicherort der Umgebungs spezifischen Konfigurationsdatei an, wie Untergrund Legendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md)beschrieben.
16. Konfigurieren Sie auf der Registerkarte **Beibehaltungs Richtlinie** , wie viele Builds jedes Typs bei Bedarf beibehalten werden sollen.
17. Klicken Sie auf **Speichern**.

## <a name="queue-a-build"></a>Hinzufügen eines Builds zur Warteschlange

An diesem Punkt haben Sie mindestens eine neue Builddefinition erstellt. Der von Ihnen definierte Buildprozess wird nun entsprechend den in der Builddefinition angegebenen Triggern ausgeführt.

Wenn Sie die Builddefinition für die Verwendung von CI konfiguriert haben, können Sie die Builddefinition auf zweierlei Weise testen:

- Checken Sie einen Inhalt in das Teamprojekt ein, um einen automatischen Buildvorgang zu initiieren.
- Manuell einen Build in die Warteschlange stellen

**So erstellen Sie einen Build manuell**

1. Klicken Sie im **Team Explorer** Fenster mit der rechten Maustaste auf die Builddefinition, und klicken Sie dann auf **neuen Build in Warteschlange**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. Überprüfen Sie im Dialogfeld **Warteschlangen Erstellung** die Buildeigenschaften, und klicken Sie dann auf **Warteschlange**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Um den Fortschritt und das Ergebnis eines Builds&#x2014;unabhängig davon, ob er manuell ausgelöst wurde, zu&#x2014;überprüfen, oder Doppelklicken Sie im **Team Explorer** Fenster automatisch auf die Builddefinition. Dadurch wird eine **Build-Explorer** -Registerkarte geöffnet.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Von hier aus können Sie Fehler bei Builds beheben. Wenn Sie auf einen einzelnen Build doppelklicken, können Sie zusammenfassende Informationen anzeigen und zu detaillierten Protokolldateien klicken.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Sie können diese Informationen verwenden, um Fehler bei Builds zu beheben und Probleme zu beheben, bevor Sie einen anderen Build versuchen.

> [!NOTE]
> Builds, die die Bereitstellungs Logik ausführen, schlagen wahrscheinlich fehl, bis Sie dem Buildserver alle Berechtigungen erteilt haben, die in der Zielumgebung erforderlich sind. Weitere Informationen finden Sie unter [Konfigurieren von Berechtigungen für die Team Build-Bereitstellung](configuring-permissions-for-team-build-deployment.md).

## <a name="monitor-the-build-process"></a>Überwachen des Buildprozesses

TFS bietet eine Vielzahl von Funktionen, mit denen Sie den Buildprozess überwachen können. Beispielsweise kann TFS Ihnen eine e-Mail senden oder Warnungen im Infobereich der Taskleiste anzeigen, wenn ein Build abgeschlossen wurde. Weitere Informationen finden Sie unter [ausführen und Überwachen von Builds](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema wird beschrieben, wie eine Builddefinition in TFS erstellt wird. Die Builddefinition ist für CI konfiguriert, sodass der Buildprozess immer dann ausgeführt wird, wenn ein Entwickler Inhalt in das Teamprojekt eincheckt. Die Builddefinition führt eine benutzerdefinierte MSBuild-Projektdatei aus, um Webpakete und Daten Bank Skripts in einer Zielserver Umgebung bereitzustellen.

Damit eine automatisierte Bereitstellung im Rahmen eines Buildprozesses erfolgreich ausgeführt werden kann, müssen Sie dem Builddienstkonto auf den Zielweb Servern und dem Ziel Datenbankserver entsprechende Berechtigungen erteilen. Im letzten Thema dieses Tutorials, [Konfigurieren von Berechtigungen für die Team Build-Bereitstellung](configuring-permissions-for-team-build-deployment.md), wird beschrieben, wie Sie die Berechtigungen identifizieren und konfigurieren, die für die automatisierte Bereitstellung von einem Team Build Server erforderlich sind.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Erstellen von Builddefinitionen finden Sie unter [Erstellen einer grundlegenden Builddefinition](https://msdn.microsoft.com/library/ms181716.aspx) und definieren des Buildprozesses. [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx) Weitere Informationen zur Warteschlange von Builds finden Sie unter [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Zurück](configuring-a-tfs-build-server-for-web-deployment.md)
> [Weiter](deploying-a-specific-build.md)
