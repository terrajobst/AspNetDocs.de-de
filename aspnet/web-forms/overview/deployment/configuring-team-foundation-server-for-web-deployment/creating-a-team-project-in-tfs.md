---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Erstellen eines Team Projekts in TFS | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein neues Teamprojekt in Team Foundation Server (TFS) 2010 erstellt wird.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519561"
---
# <a name="creating-a-team-project-in-tfs"></a>Erstellen eines Teamprojekts in TFS

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie ein neues Teamprojekt in Team Foundation Server (TFS) 2010 erstellt wird.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

## <a name="task-overview"></a>Aufgaben Übersicht

Zum Bereitstellen und Verwenden eines neuen Teamprojekts in TFS müssen Sie die folgenden Schritte ausführen:

- Erteilen Sie dem Benutzer, der das neue Teamprojekt erstellt, Berechtigungen.
- Erstellen Sie das Teamprojekt.
- Erteilen von Berechtigungen für Teammitglieder, die an dem Projekt arbeiten werden.
- Überprüfen Sie einige Inhalte.

In diesem Thema wird erläutert, wie Sie diese Prozeduren ausführen, und es werden die Benutzer und Auftrags Rollen identifiziert, die wahrscheinlich für die einzelnen Prozeduren verantwortlich sind. Beachten Sie, dass in Abhängigkeit von der Struktur Ihrer Organisation jede dieser Aufgaben die Verantwortung einer anderen Person haben kann.

Bei den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie TFS installiert und konfiguriert haben und dass Sie im Rahmen des Konfigurations Vorgangs eine Teamprojekt Sammlung erstellt haben. Weitere Informationen zu diesen Annahmen und allgemeine Hintergrundinformationen zu diesem Szenario finden Sie unter [Konfigurieren eines TFS-Buildservers für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Erteilen von Berechtigungen für den Team Projekt Ersteller

Um ein neues Teamprojekt zu erstellen, benötigen Sie diese Berechtigungen:

- Sie müssen über die Berechtigung " **neue Projekte erstellen** " für die TFS-Anwendungsebene verfügen. Diese Berechtigung erteilen Sie in der Regel, indem Sie der TFS-Gruppe der **Projekt Sammlungs Administratoren** Benutzer hinzufügen. Die globale Gruppe **Team Foundation-Administratoren** enthält auch diese Berechtigung.
- Sie müssen über die Berechtigung zum Erstellen neuer Team Sites innerhalb der SharePoint-Website Sammlung verfügen, die der TFS-Teamprojekt Sammlung entspricht. Sie erteilen diese Berechtigung in der Regel, indem Sie den Benutzer einer SharePoint-Gruppe mit **voll** Zugriffsrechten für die SharePoint-Website Sammlung hinzufügen.
- Wenn Sie SQL Server Reporting Services Features verwenden, müssen Sie Mitglied der Rolle " **Team Foundation-Inhalts-Manager** " in Reporting Services sein.

### <a name="who-performs-these-procedures"></a>Wer führt diese Prozeduren aus?

In der Regel führt die Person oder Gruppe, die die TFS-Bereitstellung verwaltet, auch diese Verfahren aus.

Da es sich hierbei um einen Berechtigungs Satz mit hohen Berechtigungen handelt, werden neue Team Projekte in der Regel von einer kleinen Teilmenge von Benutzern erstellt, die für die Verwaltung einer TFS-Bereitstellung zuständig sind. Entwicklern werden normalerweise nicht die Berechtigungen erteilt, die zum Erstellen neuer Team Projekte erforderlich sind.

### <a name="grant-permissions-in-tfs"></a>Erteilen von Berechtigungen in TFS

Wenn Sie einem Benutzer das Erstellen neuer Team Projekte ermöglichen möchten, besteht die erste allgemeine Aufgabe darin, den Benutzer der Gruppe Projekt Auflistungs **Administratoren** für die Teamprojekt Sammlung hinzuzufügen.

**So fügen Sie der Gruppe "Projekt Auflistungs Administratoren" einen Benutzer hinzu**

1. Zeigen Sie auf dem TFS-Server im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft Team Foundation Server 2010**, und klicken Sie dann auf **Team Foundation-Verwaltungskonsole**.
2. Erweitern Sie in der Navigationsstruktur Ansicht den Knoten **Anwendungsebene**, und klicken Sie dann auf **Team Projekt Sammlungen**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. Wählen Sie im Bereich **Teamprojekt Sammlungen** die Teamprojekt Sammlung aus, die Sie verwalten möchten.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Klicken Sie auf der Registerkarte **Allgemein** auf **Gruppenmitgliedschaft**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. Wählen Sie im Dialogfeld **globale Gruppen** die Gruppe **Projekt** Auflistungs Administratoren aus, und klicken Sie dann auf **Eigenschaften**.
6. Wählen Sie im Dialogfeld **Eigenschaften von Team Foundation Server Gruppe** die Option **Windows-Benutzer oder-Gruppe**aus, und klicken Sie dann auf **Hinzufügen**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. Geben Sie im Dialogfeld **Benutzer, Computer oder Gruppen auswählen** den Benutzernamen des Benutzers ein, in dem Sie neue Team Projekte erstellen möchten, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. Klicken Sie im Dialogfeld **Eigenschaften von Team Foundation Server Gruppe** auf **OK**.
9. Klicken Sie im Dialogfeld **globale Gruppen** auf **Schließen**.

### <a name="grant-permissions-in-sharepoint-services"></a>Erteilen von Berechtigungen in SharePoint Services

Als nächstes müssen Sie dem Benutzer die Berechtigung zum Erstellen neuer Team Sites in der SharePoint-Website Sammlung erteilen, die der TFS-Teamprojekt Sammlung entspricht.

**So erteilen Sie voll Zugriffsberechtigungen für die SharePoint-Website Sammlung**

1. Wählen Sie in der Team Foundation Server-Verwaltungskonsole auf der Seite **Teamprojekt Sammlungen** die Teamprojekt Sammlung aus, die Sie verwalten möchten.
2. Notieren Sie sich auf der Registerkarte **SharePoint-Website** den Wert der **aktuellen Standard Website** -URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Öffnen Sie Internet Explorer, und navigieren Sie dann zu der URL, die Sie in Schritt 2 notiert haben.

    > [!NOTE]
    > Wenn Sie als Benutzer, der die Teamprojekt Sammlung erstellt hat, nicht bei Windows angemeldet sind, müssen Sie sich bei SharePoint als dieser Benutzer anmelden, um den Vorgang fortzusetzen.
4. Klicken Sie im Menü **Websiteaktionen** auf **Siteeinstellungen**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Klicken Sie auf der Seite **Site Einstellungen** unter **Benutzer und Berechtigungen**auf **Personen und Gruppen**.
6. Klicken Sie im linken Navigationsbereich auf **Gruppen**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Klicken Sie auf der Seite **Personen und Gruppen: alle Gruppen** auf **Gruppen für diesen Standort einrichten**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Aufgrund eines doppelten http-Codierungs Fehlers erhalten Sie möglicherweise die Fehlermeldung " <strong>HTTP 404 nicht gefunden</strong> ". Ersetzen Sie in diesem Fall die URL durch Folgendes:   
   > Beispiel: `[site_collection_URL]/_layouts/permsetup.aspx`  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Fügen Sie auf der Seite **Gruppen für diese Site einrichten** den Benutzer, der Team Projekte erstellt, der Gruppe **Besitzer** hinzu, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Weitere Informationen zum Aktivieren von Benutzern für das Erstellen neuer Team Projekte in einer Teamprojekt Auflistung finden Sie unter [Festlegen von Administrator Berechtigungen für Teamprojekt Sammlungen](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Erstellen eines neuen Team Projekts und Hinzufügen von Benutzern

Sobald Sie über die erforderlichen Berechtigungen verfügen, können Sie das **Team Explorer** Fenster in Visual Studio 2010 verwenden, um ein neues Team Projekt zu erstellen. Dieser Ansatz stellt einen Assistenten bereit, der alle erforderlichen Informationen sammelt und die erforderlichen Aufgaben in TFS, SharePoint und SQL Server Reporting Services ausführt. Außerdem müssen Sie den Mitgliedern des Entwicklerteams Berechtigungen für das neue Teamprojekt erteilen, damit Sie Inhalte hinzufügen und ändern können.

### <a name="who-performs-these-procedures"></a>Wer führt diese Prozeduren aus?

In der Regel führt entweder ein TFS-Administrator oder ein Entwicklerteam Leiter diese Verfahren aus.

### <a name="create-a-new-team-project"></a>Neues Team Projekt erstellen

Im nächsten Verfahren wird das Erstellen eines neuen Teamprojekts in TFS 2010 beschrieben.

**So erstellen Sie ein neues Teamprojekt**

1. Zeigen Sie im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, klicken Sie mit der rechten Maustaste auf **Microsoft Visual Studio 2010**, und klicken Sie dann auf **als Administrator ausführen**.

    > [!NOTE]
    > Wenn Sie Visual Studio 2010 nicht als Administrator ausführen, schlägt der Assistent für neue Team Projekte im letzten Schritt fehl.
2. Wenn das Dialogfeld **Benutzerkontensteuerung** angezeigt wird, klicken Sie auf **Ja**.
3. Klicken Sie in Visual Studio im Menü **Team** auf **Verbindung mit Team Foundation Server herstellen**.

    > [!NOTE]
    > Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 4-7 weglassen.
4. Klicken Sie im Dialogfeld **Verbindung mit Team Projekt** auf **Server**.
5. Klicken Sie im Dialogfeld **Team Foundation Server hinzufügen/entfernen** auf **Hinzufügen**.
6. Geben Sie im Dialogfeld **Team Foundation Server hinzufügen** die Details der TFS-Instanz an, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. Klicken Sie im Dialogfeld **Team Foundation Server hinzufügen/entfernen** auf **Schließen**.
8. Wählen Sie im Dialogfeld **mit Team Projekt verbinden** die TFS-Instanz aus, mit der Sie eine Verbindung herstellen möchten, wählen Sie die Team Projekt Sammlung aus, die Sie hinzufügen möchten, und klicken Sie dann auf **verbinden**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. Klicken Sie im **Team Explorer** Fenster mit der rechten Maustaste auf die Teamprojekt Sammlung, und klicken Sie dann auf **Neues Team Projekt**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. Geben Sie im Dialogfeld **Neues Team Projekt** einen Namen und eine Beschreibung für das Teamprojekt ein, und klicken Sie dann auf **weiter**.

    > [!NOTE]
    > Wenn das Teamprojekt Leerzeichen enthält, treten möglicherweise Probleme auf, wenn Sie das Webbereitstellungs Tool (Web deploy) Internetinformationsdienste (IIS) verwenden, um Pakete über den Ausgabepfad bereitzustellen. Leerzeichen im Pfad können das Ausführen Web deploy Befehle viel schwieriger machen.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Wählen Sie auf der Seite **Prozess Vorlage auswählen** die Prozess Vorlage aus, die Sie zum Verwalten des Entwicklungsprozesses verwenden möchten, und klicken Sie dann auf **weiter**.

    > [!NOTE]
    > Weitere Informationen zu Prozessvorlagen für TFS finden Sie unter [Prozessvorlagen und-Tools](https://msdn.microsoft.com/vstudio/aa718795).
12. Überlassen Sie auf der Seite " **Team Site Einstellungen** " die Standardeinstellungen unverändert, und klicken Sie dann auf **weiter**.
13. Mit dieser Einstellung wird eine SharePoint-Team Website erstellt oder identifiziert, die dem TFS-Teamprojekt zugeordnet ist. Das Entwicklungsteam kann diese Website zum Verwalten der Dokumentation, zur Teilnahme an Diskussions Threads, zum Erstellen von Wiki-Seiten und zum Ausführen verschiedener anderer Aufgaben verwenden, die nicht mit Code verknüpft sind. Weitere Informationen finden Sie unter [Interaktionen zwischen SharePoint-Produkten und Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Überlassen Sie auf der Seite Quell Code Verwaltungs **Einstellungen angeben** die Standardeinstellungen unverändert, und klicken Sie dann auf **weiter**.
15. Mit dieser Einstellung wird der Speicherort in der TFS-Ordnerhierarchie identifiziert oder erstellt, der als Stamm Ordner für Ihre Inhalte fungiert.
16. Klicken Sie auf der Seite **Team Projekteinstellungen bestätigen** auf **Fertig**stellen.
17. Wenn das neue Teamprojekt erfolgreich erstellt wurde, klicken Sie auf der Seite **Teamprojekt erstellt** auf **Schließen**.

### <a name="add-users-to-a-team-project"></a>Hinzufügen von Benutzern zu einem Team Projekt

Nachdem Sie das neue Teamprojekt erstellt haben, können Sie Benutzern Berechtigungen erteilen, damit Sie mit dem Hinzufügen und zusammenarbeiten von Inhalten beginnen können.

**So fügen Sie einem Teamprojekt Benutzer hinzu**

1. Klicken Sie in Visual Studio 2010 im Fenster **Team Explorer** mit der rechten Maustaste auf das Teamprojekt, zeigen Sie auf **Teamprojekt Einstellungen**, und klicken Sie dann auf **Gruppenmitgliedschaft**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Um es Benutzern zu ermöglichen, Code unter der Quell Code Verwaltung hinzuzufügen, zu ändern und zu entfernen, müssen Sie Sie der Gruppe **mitwirk** Ende hinzufügen.
3. Wählen Sie im Dialogfeld **Projektgruppen** die Gruppe **mitwirk** Ende aus, und klicken Sie dann auf **Eigenschaften**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. Wählen Sie im Dialogfeld **Eigenschaften von Team Foundation Server Gruppe** die Option **Windows-Benutzer oder-Gruppe**aus, und klicken Sie dann auf **Hinzufügen**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. Geben Sie im Dialogfeld **Benutzer, Computer oder Gruppen auswählen** den Benutzernamen des Benutzers ein, den Sie dem Teamprojekt hinzufügen möchten, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. Klicken Sie im Dialogfeld **Eigenschaften von Team Foundation Server Gruppe** auf **OK**.
7. Klicken Sie im Dialogfeld **Projektgruppen** auf **Schließen**.

## <a name="conclusion"></a>Zusammenfassung

An diesem Punkt ist Ihr neues Teamprojekt einsatzbereit, und Ihr Entwicklerteam kann mit dem Hinzufügen von Inhalten und der Zusammenarbeit am Entwicklungsprozess beginnen.

Im nächsten Thema, [Hinzufügen von Inhalt zur Quell](adding-content-to-source-control.md)Code Verwaltung, wird beschrieben, wie Inhalt zur Quell Code Verwaltung hinzugefügt wird.

## <a name="further-reading"></a>Weitere nützliche Informationen

Eine umfassendere Anleitung zum Erstellen von Team Projekten in TFS finden Sie unter [Erstellen eines Teamprojekts](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Weitere Informationen zum Aktivieren von Benutzern für das Erstellen neuer Team Projekte in einer Teamprojekt Auflistung finden Sie unter [Festlegen von Administrator Berechtigungen für Teamprojekt Sammlungen](https://msdn.microsoft.com/library/dd547204.aspx). Weitere Informationen zum Hinzufügen von Benutzern zu Team Projekten finden [Sie unter Hinzufügen von Benutzern zu Team Projekten](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Zurück](configuring-team-foundation-server-for-web-deployment.md)
> [Weiter](adding-content-to-source-control.md)
