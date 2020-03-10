---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Offline schalten von Webanwendungen mit Web deploy | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie eine Webanwendung für die Dauer einer automatisierten Bereitstellung mithilfe der Internetinformationsdienste (IIS)-Webbereitstellung offline geschaltet wird...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: ba60664a0c3daa0650cd7e7cfc4ab9da08df3440
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422211"
---
# <a name="taking-web-applications-offline-with-web-deploy"></a>Offlineschalten von Webanwendungen mit Web Deploy

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie eine Webanwendung für die Dauer der automatisierten Bereitstellung mit dem Webbereitstellungs Web deploy Tool Internetinformationsdienste (IIS) offline geschaltet wird. Benutzer, die zur Webanwendung navigieren, werden bis zum Abschluss der Bereitstellung zu einer *App\_Datei "offline. htm* " umgeleitet.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="task-overview"></a>Aufgaben Übersicht

In vielen Szenarien empfiehlt es sich, eine Webanwendung offline zu schalten, während Sie Änderungen an verknüpften Komponenten vornehmen, wie z. b. Datenbanken oder Webdienste. In IIS und ASP.net erreichen Sie dies in der Regel, indem Sie im Stamm Ordner der IIS-Website oder-Webanwendung eine Datei mit dem Namen *App\_offline. htm* platzieren. Bei der *App\_offline. htm* -Datei handelt es sich um eine Standard-HTML-Datei, die in der Regel eine einfache Meldung enthält, die den Benutzer darüber informiert, dass die Site aufgrund der Wartung vorübergehend nicht Während die *App\_offline. htm* -Datei im Stamm Ordner der Website vorhanden ist, leitet IIS automatisch sämtliche Anforderungen an die Datei weiter. Wenn Sie mit der Aktualisierung der APP fertig sind, entfernen Sie die *App\_Datei "offline. htm* ", und die Website setzt Anforderungen wie gewohnt fort.

Wenn Sie Web deploy verwenden, um automatisierte oder einstufige bereit Stellungen in einer Zielumgebung durchzuführen, können Sie das Hinzufügen und Entfernen der *App\_offline. htm* -Datei in ihren Bereitstellungs Prozess integrieren. Zu diesem Zweck müssen Sie die folgenden Aufgaben auf hoher Ebene ausführen:

- Erstellen Sie in der Microsoft-Build-Engine (MSBuild)-Projektdatei, die Sie zum Steuern des Bereitstellungs Prozesses verwenden, ein MSBuild-Ziel, das eine *App\_offline. htm* -Datei auf den Zielserver kopiert, bevor alle Bereitstellungs Aufgaben beginnen.
- Fügen Sie ein weiteres MSBuild-Ziel hinzu, das die *App\_Datei "offline. htm* " vom Zielserver entfernt, wenn alle Bereitstellungs Aufgaben vollständig sind.
- Erstellen Sie im Webanwendungs Projekt eine *WPP. targets* -Datei, mit der sichergestellt wird, dass dem Bereitstellungs Paket, wenn Web deploy aufgerufen wird, eine *App\_offline. htm* -Datei hinzugefügt wird.

In diesem Thema wird gezeigt, wie Sie diese Prozeduren ausführen. Bei den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits eine Projekt Mappe erstellt haben, die mindestens ein Webanwendungs Projekt enthält, und Sie können eine benutzerdefinierte Projektdatei verwenden, um den Bereitstellungs Prozess zu steuern, wie unter [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) Alternativ können Sie die Beispiellösung [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) verwenden, um die Beispiele im Thema zu befolgen.

## <a name="adding-an-app_offline-file-to-a-web-application-project"></a>Hinzufügen einer APP\_Offline Datei zu einem Webanwendungs Projekt

Die erste Aufgabe, die Sie ausführen müssen, ist das Hinzufügen einer *App\_Offline* Datei zum Webanwendungs Projekt:

- Um zu verhindern, dass die Datei den Entwicklungsprozess beeinträchtigt (Sie möchten nicht, dass Ihre Anwendung dauerhaft offline geschaltet wird), sollten Sie Sie als *App-\_"offline. htm*" bezeichnen. Beispielsweise können Sie die Datei *App\_Offline-Template. htm*benennen.
- Um zu verhindern, dass die Datei unverändert bereitgestellt wird, sollten Sie die Buildaktion auf **keine**festlegen.

**So fügen Sie einem Webanwendungs Projekt eine APP\_-Offline Datei hinzu**

1. Öffnen Sie die Projekt Mappe in Visual Studio 2010.
2. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf das Webanwendungs Projekt, zeigen Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**.
3. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **HTML-Seite**aus.
4. Geben Sie im Feld **Name den Namen** **App\_Offline-Template. htm**ein, und klicken Sie dann auf **Hinzufügen**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Fügen Sie einigen einfachen HTML-Code hinzu, um Benutzer darüber zu informieren, dass die Anwendung nicht verfügbar ist, und speichern Sie die Datei Nehmen Sie keine serverseitigen Tags auf (z. b. alle Tags mit dem Präfix "ASP:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf die neue Datei, und klicken Sie dann auf **Eigenschaften**.
7. Wählen Sie im Fenster **Eigenschaften** in der **Zeile** Buildvorgang die Option **keine**aus.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-app_offline-file"></a>Bereitstellen und Löschen einer APP\_Offline Datei

Der nächste Schritt besteht darin, die Bereitstellungs Logik so zu ändern, dass die Datei zu Beginn des Bereitstellungs Prozesses auf den Zielserver kopiert und am Ende entfernt wird.

> [!NOTE]
> Im nächsten Verfahren wird davon ausgegangen, dass Sie eine benutzerdefinierte MSBuild-Projektdatei verwenden, um den Bereitstellungs Prozess zu steuern, wie in Grundlegendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschrieben. Wenn Sie direkt aus Visual Studio bereitstellen, müssen Sie einen anderen Ansatz verwenden. Sayed Ibrahim Hashimi beschreibt einen solchen Ansatz, in dem [Sie Ihre Web-App während der Veröffentlichung Offline](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)schalten können.

Zum Bereitstellen einer *App\_Offline* Datei auf einer IIS-Zielwebsite müssen Sie "msbereitstellungs. exe" mithilfe des [Web deploy **contentPath** -Anbieters](https://technet.microsoft.com/library/dd569034(WS.10).aspx)aufrufen. Der **contentPath** -Anbieter unterstützt sowohl physische Verzeichnispfade als auch IIS-Website-oder-Anwendungs Pfade, sodass er die ideale Wahl für die Synchronisierung einer Datei zwischen einem Visual Studio-Projektordner und einer IIS-Webanwendung ist. Zum Bereitstellen der Datei sollte der msbereitstellungs-Befehl wie folgt aussehen:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]

Wenn Sie die Datei am Ende des Bereitstellungs Prozesses vom Ziel Standort entfernen möchten, sollte der msdeployment-Befehl wie folgt aussehen:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]

Um diese Befehle im Rahmen eines Build-und Bereitstellungs Prozesses zu automatisieren, müssen Sie Sie in Ihre benutzerdefinierte MSBuild-Projektdatei integrieren. Im nächsten Verfahren wird die Vorgehensweise beschrieben.

**So können Sie eine APP\_Offline Datei bereitstellen und löschen**

1. Öffnen Sie in Visual Studio 2010 die MSBuild-Projektdatei, mit der der Bereitstellungs Prozess gesteuert wird. In der Beispiellösung [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ist dies die Datei *Publish. proj* .
2. Erstellen Sie im root **Project** -Element ein neues **PropertyGroup** -Element, um die Variablen für die *App\_die Offline* Bereitstellung zu speichern:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. Die **SourceRoot** -Eigenschaft wird an anderer Stelle in der Datei " *Publish. proj* " definiert. Gibt den Speicherort des Stamm Ordners für den Quell Inhalt relativ zum aktuellen Pfad&#x2014;relativ zum Speicherort der Datei " *Publish. proj* " an.
4. Der **contentPath** -Anbieter akzeptiert keine relativen Dateipfade, sodass Sie einen absoluten Pfad zu Ihrer Quelldatei erhalten müssen, bevor Sie Sie bereitstellen können. Hierfür können Sie die [convertdeabsolutepath](https://msdn.microsoft.com/library/bb882668.aspx) -Aufgabe verwenden.
5. Fügen Sie ein neues **Ziel** Element mit dem Namen **getappofflineabsolutepath**hinzu. Verwenden Sie in diesem Ziel die **convertdeabsolutepath** -Aufgabe, um einen absoluten Pfad zur *App\_Offline-Vorlagen* Datei in Ihrem Projektordner abzurufen.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Dieses Ziel übernimmt den relativen Pfad zur *App\_Offline-Vorlagen* Datei in Ihrem Projektordner und speichert Sie als absoluten Dateipfad in einer neuen Eigenschaft. Das **BeforeTargets** -Attribut gibt an, dass dieses Ziel vor dem **deployappoffline** -Ziel ausgeführt werden soll, das Sie im nächsten Schritt erstellen.
7. Fügen Sie ein neues Ziel mit dem Namen **deployappoffline**hinzu. Rufen Sie in diesem Ziel den Befehl msbereitstellungs. exe auf, mit dem die *App\_Offline* Datei auf dem Zielweb Server bereitgestellt wird.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. In diesem Beispiel wird die **contactmanageriispath** -Eigenschaft an anderer Stelle in der Projektdatei definiert. Dabei handelt es sich einfach um einen IIS-Anwendungspfad im Format *[IIS-Website Name]/[Anwendungsname]* . Wenn Sie eine Bedingung in das Ziel einschließen, können Benutzer die *App\_Offline* Bereitstellung ein-oder ausschalten, indem Sie einen Eigenschafts Wert ändern oder einen Befehlszeilenparameter bereitstellen.
9. Fügen Sie ein neues Ziel mit dem Namen **deleteappoffline**hinzu. Rufen Sie in diesem Ziel den Befehl msbereitstellungs. exe auf, mit dem die *App\_Offline* Datei vom Zielweb Server entfernt wird.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Die letzte Aufgabe besteht darin, diese neuen Ziele an den entsprechenden Punkten während der Ausführung der Projektdatei aufzurufen. Dies kann auf unterschiedliche Weise erfolgen. Beispielsweise gibt die **fullpublishdependson** -Eigenschaft in der Datei *Publish. proj* eine Liste von Zielen an, die ausgeführt werden müssen, wenn das **fullpublish** -Standardziel aufgerufen wird.
11. Ändern Sie die MSBuild-Projektdatei, um die Ziele **deployappoffline** und **deleteappoffline** an den entsprechenden Stellen im Veröffentlichungsprozess aufzurufen.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Wenn Sie die benutzerdefinierte MSBuild-Projektdatei ausführen, wird die *App\_Offline* Datei auf dem Server sofort nach einem erfolgreichen Build bereitgestellt. Sie wird dann vom Server gelöscht, sobald alle Bereitstellungs Aufgaben vollständig ausgeführt wurden.

## <a name="adding-an-app_offline-file-to-deployment-packages"></a>Hinzufügen einer APP\_Offline Datei zu Bereitstellungs Paketen

Je nachdem, wie Sie die Bereitstellung konfigurieren, werden vorhandene Inhalte in der IIS-&#x2014;Ziel Webanwendung, wie die *App\_offline. htm* -Datei&#x2014;, möglicherweise automatisch gelöscht, wenn Sie ein Webpaket für das Ziel bereitstellen. Um sicherzustellen, dass die *App\_offline. htm* -Datei für die Dauer der Bereitstellung weiterhin vorhanden ist, müssen Sie die Datei zusätzlich zur Bereitstellung der Datei direkt zu Beginn des Bereitstellungs Prozesses in das Webbereitstellungs Paket einbinden.

- Wenn Sie die vorherigen Aufgaben in diesem Thema befolgt haben, haben Sie dem Webanwendungs Projekt unter einem anderen Dateinamen (wir haben *App\_Offline-Template. htm*verwendet) die APP-\_Datei " *Offline. htm* " hinzugefügt, und Sie haben die Buildaktion auf " **None**" festgelegt. Diese Änderungen sind erforderlich, um zu verhindern, dass die Datei die Entwicklung und das Debuggen beeinträchtigt. Daher müssen Sie den Verpackungsprozess anpassen, um sicherzustellen, dass die *App\_offline. htm* -Datei im Webbereitstellungs Paket enthalten ist.

Die Webpublishing Pipeline (WPP) verwendet eine Elementliste mit dem Namen " **filesforpackagingfromproject** ", um eine Liste von Dateien zu erstellen, die im Webbereitstellungs Paket enthalten sein sollen. Sie können den Inhalt Ihrer Webpakete anpassen, indem Sie eigene Elemente zu dieser Liste hinzufügen. Hierzu müssen Sie die folgenden Schritte ausführen:

1. Erstellen Sie eine benutzerdefinierte Projektdatei mit dem Namen *[Project Name]. WPP. targets* in demselben Ordner wie die Projektdatei.

    > [!NOTE]
    > Die *WPP. targets* -Datei muss sich im selben Ordner wie die Projektdatei&#x2014;der Webanwendung befinden. Beispiel: *ContactManager. MVC. csproj*&#x2014;und nicht im selben Ordner wie benutzerdefinierte Projektdateien, mit denen Sie den Build-und Bereitstellungs Prozess steuern.
2. Erstellen Sie in der Datei *. WPP. targets* ein neues MSBuild-Ziel, das *vor* dem **copyallfilestosinglefolderforpackage** -Ziel ausgeführt wird. Dies ist das WPP-Ziel, das eine Liste von Elementen erstellt, die in das Paket eingeschlossen werden sollen.
3. Erstellen Sie im neuen Ziel ein **ItemGroup** -Element.
4. Fügen Sie im **ItemGroup** -Element ein " **filesforpackagingfromproject** "-Element hinzu, und geben Sie die *App\_offline. htm* -Datei an.

Die *WPP. targets* -Datei sollte in etwa wie folgt aussehen:

[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]

Dies sind die wichtigsten Punkte in diesem Beispiel:

- Das **BeforeTargets** -Attribut fügt dieses Ziel in das WPP ein, indem es angibt, dass es unmittelbar vor dem **copyallfilestosinglefolderforpackage** -Ziel ausgeführt werden soll.
- Das **filesforpackagingfromproject** -Element verwendet den **destinationrelativepath** -Metadatenwert zum Umbenennen der Datei von *App-\_Offline-Template. htm* in *App\_offline. htm* , wenn Sie der Liste hinzugefügt wird.

Im nächsten Verfahren wird gezeigt, wie Sie diese *WPP. targets* -Datei einem Webanwendungs Projekt hinzufügen.

**So fügen Sie einem Webbereitstellungs Paket eine WPP. targets-Datei hinzu**

1. Öffnen Sie die Projekt Mappe in Visual Studio 2010.
2. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf den Webanwendungs Projekt Knoten (z. **b. ContactManager. MVC**), zeigen Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**.
3. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **XML-Datei** aus.
4. Geben Sie im Feld **Name den Namen** *[Project Name] * * *. WPP. targets** (z **. b. ContactManager. MVC. WPP. targets**) ein, und klicken Sie dann auf **Hinzufügen**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Wenn Sie dem Stamm Knoten eines Projekts ein neues Element hinzufügen, wird die Datei im selben Ordner wie die Projektdatei erstellt. Sie können dies überprüfen, indem Sie den Ordner in Windows-Explorer öffnen.
5. Fügen Sie in der Datei das zuvor beschriebene MSBuild-Markup hinzu.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Speichern und schließen Sie die Datei *[Project Name]. WPP. targets* .

Wenn Sie das nächste Mal das Webanwendungs Projekt erstellen und packen, wird die WPP *. targets* -Datei automatisch von WPP erkannt. Die *App\_Offline-Template. htm* -Datei wird in das resultierende Webbereitstellungs Paket als *App\_offline. htm*eingeschlossen.

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler auftritt, bleibt die Datei " *App\_offline. htm* " erhalten, und die Anwendung bleibt offline. Dies ist in der Regel das gewünschte Verhalten. Wenn Sie Ihre Anwendung wieder online schalten möchten, können Sie die *App\_Datei "offline. htm* " von Ihrem Webserver löschen. Wenn Sie Fehler beheben und eine erfolgreiche Bereitstellung ausführen, wird die *App\_offline. htm* -Datei entfernt.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie eine Webanwendung für die Dauer einer Bereitstellung offline geschaltet wird, indem eine *App\_offline. htm* -Datei auf dem Zielserver zu Beginn des Bereitstellungs Prozesses veröffentlicht und am Ende entfernt wird. Außerdem wird beschrieben, wie Sie eine *App\_offline. htm* -Datei in ein Webbereitstellungs Paket einbinden.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zum Verpackungs-und Bereitstellungs Prozess finden Sie unter [Erstellung und Verpacken von Webanwendungs Projekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurieren von Parametern für die Webpaket Bereitstellung und bereit](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)stellen von [Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Wenn Sie Ihre Webanwendungen direkt aus Visual Studio heraus veröffentlichen, anstatt den in diesen Tutorials beschriebenen benutzerdefinierten MSBuild-Projektdatei Ansatz zu verwenden, müssen Sie einen etwas anderen Ansatz verwenden, um die Anwendung während der Veröffentlichung offline zu schalten. ESS.

> [!div class="step-by-step"]
> [Zurück](excluding-files-and-folders-from-deployment.md)
> [Weiter](running-windows-powershell-scripts-from-msbuild-project-files.md)
