---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Hinzufügen von Inhalt zur Quell Code Verwaltung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird erläutert, wie Sie der Quell Code Verwaltung Inhalt in Team Foundation Server (TFS) 2010 hinzufügen. Es wird beschrieben, wie Projektmappen und Projekten einem Teamprofil hinzugefügt werden...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515109"
---
# <a name="adding-content-to-source-control"></a>Hinzufügen von Inhalten zur Quellcodeverwaltung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird erläutert, wie Sie der Quell Code Verwaltung Inhalt in Team Foundation Server (TFS) 2010 hinzufügen. Es wird beschrieben, wie Sie Projektmappen und Projekte zu einem Teamprojekt in TFS hinzufügen. Außerdem wird erläutert, wie Sie der Quell Code Verwaltung externe Abhängigkeiten wie Frameworks oder Assemblys hinzufügen.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

## <a name="task-overview"></a>Aufgaben Übersicht

In den meisten Fällen sollte jedes Mitglied des Entwicklerteams in der Lage sein, der Quell Code Verwaltung Inhalt hinzuzufügen. Zum Hinzufügen einer Projekt Mappe zur Quell Code Verwaltung in TFS müssen Sie diese Grundschritte ausführen:

- Stellen Sie eine Verbindung mit einem Teamprojekt her.
- Ordnen Sie die Teamprojekt-Ordnerstruktur auf dem-Server einer Ordnerstruktur auf dem lokalen Computer zu.
- Fügen Sie die Projekt Mappe und ihren Inhalt der Quell Code Verwaltung hinzu.
- Fügen Sie externe Abhängigkeiten zur Quell Code Verwaltung hinzu.

In diesem Thema wird gezeigt, wie Sie diese Prozeduren ausführen.

In den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits ein neues TFS-Teamprojekt erstellt haben, um Ihre Inhalte zu verwalten. Weitere Informationen zum Erstellen eines neuen Teamprojekts finden Sie unter [Erstellen eines Teamprojekts in TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Wer führt diese Prozeduren aus?

In den meisten Fällen sollte jedes Mitglied des Entwicklerteams in der Lage sein, Inhalte innerhalb bestimmter Team Projekte hinzuzufügen und zu ändern.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Herstellen einer Verbindung mit einem Team Projekt und Erstellen einer Ordner Zuordnung

Bevor Sie der Quell Code Verwaltung Inhalte hinzufügen, müssen Sie eine Verbindung mit einem Teamprojekt herstellen und eine Zuordnung zwischen der Ordnerstruktur auf dem Server und dem Dateisystem auf dem lokalen Computer erstellen.

**So stellen Sie eine Verbindung mit einem Teamprojekt her und ordnen einen lokalen Pfad zu**

1. Öffnen Sie Visual Studio 2010 auf Ihrer Entwickler Arbeitsstation.
2. Klicken Sie in Visual Studio im Menü **Team** auf **Verbindung mit Team Foundation Server herstellen**.

    > [!NOTE]
    > Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 3-6 weglassen.
3. Klicken Sie im Dialogfeld **Verbindung mit Team Projekt** auf **Server**.
4. Klicken Sie im Dialogfeld **Team Foundation Server hinzufügen/entfernen** auf **Hinzufügen**.
5. Geben Sie im Dialogfeld **Team Foundation Server hinzufügen** die Details der TFS-Instanz an, und klicken Sie dann auf **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. Klicken Sie im Dialogfeld **Team Foundation Server hinzufügen/entfernen** auf **Schließen**.
7. Wählen Sie im Dialogfeld **mit Team Projekt verbinden** die TFS-Instanz aus, mit der Sie eine Verbindung herstellen möchten, wählen Sie die Teamprojekt Sammlung aus, wählen Sie das Teamprojekt aus, das Sie hinzufügen möchten, und klicken Sie dann auf **verbinden**.

    ![](adding-content-to-source-control/_static/image2.png)
8. Erweitern Sie im Fenster **Team Explorer** das Team Projekt, und doppelklicken Sie dann auf **Quell**Code Verwaltung.

    ![](adding-content-to-source-control/_static/image3.png)
9. Klicken Sie auf der Registerkarte **Quellcodeverwaltungs-Explorer** auf **nicht zugeordnet**.

    ![](adding-content-to-source-control/_static/image4.png)
10. Navigieren Sie **im Dialogfeld** Zuordnung im Feld **lokaler Ordner** zu einem lokalen Ordner, der als Stamm Ordner für das Teamprojekt fungieren soll, **und klicken Sie**dann auf zuordnen.

    ![](adding-content-to-source-control/_static/image5.png)
11. Wenn Sie zum Herunterladen der Quelldateien aufgefordert werden, klicken Sie auf **Ja**.

    ![](adding-content-to-source-control/_static/image6.png)

An diesem Punkt haben Sie den serverseitigen Ordner für das Teamprojekt einem lokalen Ordner auf Ihrer Entwickler Arbeitsstation zugeordnet. Sie haben auch vorhandene Inhalte aus dem Teamprojekt in Ihre lokale Ordnerstruktur heruntergeladen. Sie können jetzt mit dem Hinzufügen Ihres eigenen Inhalts zur Quell Code Verwaltung beginnen.

## <a name="add-projects-and-solutions-to-source-control"></a>Hinzufügen von Projekten und Projektmappen zur Quell Code Verwaltung

Wenn Sie Projekte und Projektmappen zur Quell Code Verwaltung hinzufügen möchten, müssen Sie Sie zuerst in den zugeordneten Ordner für das Teamprojekt auf dem lokalen Computer verschieben. Sie können dann den Inhalt einchecken, um die Ergänzungen mit dem Server zu synchronisieren.

**So fügen Sie Projekte zur Quell Code Verwaltung hinzu**

1. Verschieben Sie Ihre Projekte und Projektmappen auf der Arbeitsstation des Entwicklers an eine geeignete Position in der zugeordneten Ordnerstruktur für das Teamprojekt.

    > [!NOTE]
    > Viele Organisationen haben einen bevorzugten Ansatz, um zu erfahren, wie Projekte und Projektmappen in der Quell Code Verwaltung organisiert werden sollten. Anleitungen zum Strukturieren von Ordnern finden Sie unter Gewusst [wie: Strukturieren der Quell Code Verwaltungs Ordner in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Öffnen Sie die Projekt Mappe in Visual Studio 2010.
3. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf die Projekt Mappe, und klicken Sie dann auf Projekt Mappe **zur Quell Code Verwaltung hinzufügen**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > In einigen Fällen müssen Sie, je nachdem, wie Ihre Organisation Inhalte in TFS strukturiert, möglicherweise Projekte zur Quell Code Verwaltung einzeln hinzufügen, um eine präzisere Kontrolle über die Organisation des Quellcodes zu bieten.
4. Vergewissern Sie sich, dass die Registerkarte **Quellcodeverwaltungs-Explorer** den Inhalt anzeigt, den Sie in der Server Ordnerstruktur für das Teamprojekt hinzugefügt haben.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > Auf der Registerkarte **Quellcodeverwaltungs-Explorer** werden Ihre Inhalte ohne weitere Aufforderung angezeigt, weil Sie die Projekt Mappe einem zugeordneten Ordner im lokalen Dateisystem hinzugefügt haben. Wenn sich Ihre Lösung an einem nicht zugeordneten Speicherort befand, werden Sie aufgefordert, Ordner Speicherorte sowohl in TFS als auch in Ihrem lokalen Dateisystem anzugeben.
5. Klicken Sie auf der Registerkarte **Quellcodeverwaltungs-Explorer** im **Ordner** Fenster mit der rechten Maustaste auf das Teamprojekt (z. **b. ContactManager**), und klicken Sie dann auf **Ausstehende Änderungen einchecken**.
6. Geben Sie im Dialogfeld **Einchecken – Quelldateien** einen Kommentar ein, und klicken Sie dann auf **Einchecken**.

    ![](adding-content-to-source-control/_static/image9.png)

An diesem Punkt haben Sie die Projekt Mappe der Quell Code Verwaltung in TFS hinzugefügt.

## <a name="add-external-dependencies-to-source-control"></a>Externe Abhängigkeiten zur Quell Code Verwaltung hinzufügen

Wenn Sie ein Projekt oder eine Projekt Mappe zur Quell Code Verwaltung hinzufügen, werden alle Dateien und Ordner in Ihrem Projekt oder ihrer Projekt Mappe ebenfalls hinzugefügt. In vielen Fällen basieren Projekte und Projektmappen aber auch auf externen Abhängigkeiten, wie z. b. lokalen Assemblys, um ordnungsgemäß zu funktionieren. Sie müssen diese Ressourcen der Quell Code Verwaltung hinzufügen, damit sowohl der Team Build als auch andere Mitglieder des Entwicklerteams den Code erfolgreich erstellen können.

Die Ordnerstruktur für die Beispiel Projekt Mappe Contact Manager enthält z. b. einen Ordner mit dem Namen Packages. Diese enthält die Assembly und verschiedene unterstützende Ressourcen für den ADO.NET-Entity Framework 4,1. Der Ordner "Packages" ist nicht Teil der Contact Manager-Lösung, aber die Lösung wird ohne diese nicht erfolgreich erstellt. Um Team Build zum Erstellen der Projekt Mappe zu aktivieren, müssen Sie den Paket Ordner der Quell Code Verwaltung hinzufügen.

> [!NOTE]
> Die Einbindung eines Paket Ordners ist typisch für das, was geschieht, wenn Sie der Projekt Mappe mit der nuget-Erweiterung für Visual Studio 2010 die Entity Framework oder ähnliche Ressourcen hinzufügen.

**So fügen Sie der Quell Code Verwaltung nicht-Projektinhalte hinzu**

1. Stellen Sie sicher, dass sich die Elemente, die Sie hinzufügen möchten (z. b. der Ordner Pakete), an einem geeigneten Speicherort innerhalb eines zugeordneten Ordners auf dem lokalen Dateisystem befinden
2. Erweitern Sie in Visual Studio 2010 im Fenster **Team Explorer** das Team Projekt, und doppelklicken Sie dann auf **Quell**Code Verwaltung.

    ![](adding-content-to-source-control/_static/image10.png)
3. Wählen Sie auf der Registerkarte **Quellcodeverwaltungs-Explorer** im **Ordner Ordner** den Ordner aus, der die Elemente enthält, die Sie hinzufügen möchten.
4. Klicken Sie auf die Schaltfläche **Elemente zu Ordner hinzufügen** .

    ![](adding-content-to-source-control/_static/image11.png)
5. Wählen Sie im Dialogfeld **zur Quell Code Verwaltung hinzufügen** den Ordner oder die Elemente aus, die Sie hinzufügen möchten, und klicken Sie dann auf **weiter**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Wählen Sie auf der Registerkarte **ausgeschlossene Elemente** alle erforderlichen Elemente aus, die automatisch ausgeschlossen wurden (z. b. Assemblys), und klicken Sie dann auf **Element (e) einschließen**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Vergewissern Sie sich, dass auf der Registerkarte hinzu zufügende **Elemente** alle Dateien aufgelistet sind, die Sie einschließen möchten, und klicken Sie dann auf **Fertig**stellen.

    ![](adding-content-to-source-control/_static/image14.png)
8. Klicken Sie im **Quellcodeverwaltungs-Explorer** Fenster auf die Schaltfläche **Einchecken** .

    ![](adding-content-to-source-control/_static/image15.png)
9. Geben Sie im Dialogfeld **Einchecken – Quelldateien** einen Kommentar ein, und klicken Sie dann auf **Einchecken**.

An diesem Punkt haben Sie die externen Abhängigkeiten für die Projekt Mappe zur Quell Code Verwaltung hinzugefügt.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie Sie eine Verbindung mit einem Teamprojekt herstellen, eine Ordnerstruktur zuordnen und der Quell Code Verwaltung Inhalt hinzufügen. Weitere Informationen zum Arbeiten mit Elementen unter Quell Code Verwaltung finden [Sie unter Verwenden der Versionskontrolle](https://msdn.microsoft.com/library/ms181368.aspx).

Im nächsten Thema [Konfigurieren eines TFS-Buildservers für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md)wird beschrieben, wie Sie einen TFS Team Build-Server zum Erstellen und Bereitstellen Ihrer Lösung vorbereiten.

## <a name="further-reading"></a>Weitere nützliche Informationen

Ausführlichere Informationen zum Arbeiten mit der Quell Code Verwaltung in TFS finden [Sie unter Verwenden der Versionskontrolle](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Zurück](creating-a-team-project-in-tfs.md)
> [Weiter](configuring-a-tfs-build-server-for-web-deployment.md)
