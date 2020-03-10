---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Grundlegendes zum Buildprozess | Microsoft-Dokumentation
author: jrjlee
description: Dieses Thema enthält eine exemplarische Vorgehensweise für einen Build-und Bereitstellungs Prozess auf Unternehmens Niveau. Der in diesem Thema beschriebene Ansatz verwendet benutzerdefinierte Microsoft Build Engin...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 802d93f7ca987d018967275bae68b8c56d883a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520833"
---
# <a name="understanding-the-build-process"></a>Grundlegendes zum Buildprozess

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema enthält eine exemplarische Vorgehensweise für einen Build-und Bereitstellungs Prozess auf Unternehmens Niveau. Die in diesem Thema beschriebene Vorgehensweise verwendet benutzerdefinierte Microsoft-Build-Engine (MSBuild)-Projektdateien, um eine präzise Kontrolle über alle Aspekte des Prozesses zu bieten. Innerhalb der Projektdateien werden benutzerdefinierte MSBuild-Ziele zum Ausführen von Bereitstellungs Dienstprogrammen verwendet, wie z. b. das Webbereitstellungs Tool Internetinformationsdienste (IIS) und das Hilfsprogramm VSDBCMD. exe für die Daten Bank Bereitstellung.
> 
> > [!NOTE]
> > Im vorherigen Thema, Grundlegendes [zur Projektdatei](understanding-the-project-file.md), wurden die Hauptkomponenten einer MSBuild-Projektdatei beschrieben und das Konzept der geteilten Projektdateien eingeführt, um die Bereitstellung in mehreren Ziel Umgebungen zu unterstützen. Wenn Sie mit diesen Konzepten noch nicht vertraut sind, sollten Sie sich über [das Verständnis der Projektdatei](understanding-the-project-file.md) informieren, bevor Sie dieses Thema durcharbeiten.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="build-and-deployment-overview"></a>Übersicht über Build & Deployment

In der [Contact Manager-Lösung](the-contact-manager-solution.md)steuern drei Dateien den Build-und Bereitstellungs Prozess:

- Eine *universelle Projektdatei* (*Publish. proj*). Diese enthält Anweisungen zum Erstellen und bereitstellen, die sich nicht Zwischenziel Umgebungen ändern.
- Eine *Umgebungs spezifische Projektdatei* ("*en v-dev. proj*"). Dies umfasst Build-und Bereitstellungs Einstellungen, die spezifisch für eine bestimmte Zielumgebung sind. Beispielsweise können Sie die Datei " *env-dev. proj* " verwenden, um Einstellungen für einen Entwickler oder eine Testumgebung bereitzustellen und eine Alternative Datei mit dem Namen " *env-Stage. proj* " zu erstellen, um Einstellungen für eine Stagingumgebung bereitzustellen.
- Eine *Befehlsdatei* (*Publish-dev. cmd*). Diese enthält einen MSBuild. exe-Befehl, der angibt, welche Projektdateien Sie ausführen möchten. Sie können eine Befehlsdatei für jede Zielumgebung erstellen, in der jede Datei einen MSBuild. exe-Befehl enthält, der eine andere Umgebungs spezifische Projektdatei angibt. Dadurch kann der Entwickler die Bereitstellung in verschiedenen Umgebungen durchführen, indem er einfach die entsprechende Befehlsdatei ausführen kann.

In der Beispiellösung finden Sie diese drei Dateien im Ordner Projekt Mappe veröffentlichen.

![](understanding-the-build-process/_static/image1.png)

Bevor Sie sich diese Dateien ausführlicher ansehen, werfen wir einen Blick darauf, wie der allgemeine Buildprozess bei der Verwendung dieses Ansatzes funktioniert. Auf hoher Ebene sieht der Build-und Bereitstellungs Prozess wie folgt aus:

![](understanding-the-build-process/_static/image2.png)

Der erste Schritt besteht darin, dass die beiden Projektdateien&#x2014;, die eine universelle Build-und Bereitstellungs Anleitung enthalten, und eine&#x2014;mit Umgebungs spezifischen Einstellungen in eine einzelne Projektdatei zusammengeführt werden. MSBuild arbeitet dann mit den Anweisungen in der Projektdatei. Alle Projekte in der Projekt Mappe werden mithilfe der Projektdatei für jedes Projekt erstellt. Anschließend werden andere Tools wie Web deploy (msbereitstellungs. exe) und das Hilfsprogramm VSDBCmd aufgerufen, um Ihre Webinhalte und Datenbanken in der Zielumgebung bereitzustellen.

Von Anfang bis Ende führt der Build-und Bereitstellungs Prozess die folgenden Aufgaben aus:

1. Der Inhalt des Ausgabeverzeichnisses wird in Vorbereitung auf einen neuen Build gelöscht.
2. Jedes Projekt in der Projekt Mappe wird erstellt:

    1. Bei Webprojekten&#x2014;wird in diesem Fall eine ASP.NET MVC-Webanwendung und ein WCF&#x2014;-Webdienst durch den Buildprozess ein Webbereitstellungs Paket für jedes Projekt erstellt.
    2. Bei Datenbankprojekten wird vom Buildprozess ein Bereitstellungs Manifest (DeployManifest-Datei) für jedes Projekt erstellt.
3. Mithilfe des Hilfsprogramms "VSDBCMD. exe" wird jedes Datenbankprojekt in der Projekt Mappe bereitgestellt. dabei werden&#x2014;verschiedene Eigenschaften aus den Projektdateien verwendet,&#x2014;um eine Ziel Verbindungs Zeichenfolge und einen Datenbanknamen sowie die DeployManifest-Datei zu verwenden.
4. Mithilfe des Hilfsprogramms "msdeployment. exe" wird jedes Webprojekt in der Projekt Mappe bereitgestellt. dabei werden verschiedene Eigenschaften aus den Projektdateien verwendet, um den Bereitstellungs Prozess zu steuern.

Sie können die Beispiellösung verwenden, um diesen Prozess ausführlicher zu verfolgen.

> [!NOTE]
> Anleitungen zum Anpassen der Umgebungs spezifischen Projektdateien für Ihre eigenen Serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungs Eigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="invoking-the-build-and-deployment-process"></a>Aufrufen des Build & Deployment Prozesses

Zum Bereitstellen der Contact Manager-Lösung in einer Entwickler Testumgebung führt der Entwickler die Befehlsdatei *Publish-dev. cmd* aus. Dadurch wird "MSBuild. exe" aufgerufen, wobei " *Publish. proj* " als Projektdatei zum Ausführen und " *enumv-dev. proj* " als Parameterwert angegeben wird.

[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]

> [!NOTE]
> Der Schalter **/FL** (Short für **/FileLogger**) protokolliert die Buildausgabe in einer Datei namens *MSBuild. log* im aktuellen Verzeichnis. Weitere Informationen finden Sie in der [MSBuild-Befehlszeilen Referenz](https://msdn.microsoft.com/library/ms164311.aspx).

An diesem Punkt wird die Ausführung von MSBuild gestartet, die Datei " *Publish. proj* " geladen und mit der Verarbeitung der darin enthaltenen Anweisungen begonnen. Die erste Anweisung weist MSBuild an, die Projektdatei zu importieren, die vom **targetenvpropsfile** -Parameter angegeben wird.

[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]

Der **targetenvpropsfile** -Parameter gibt die Datei " *dev-dev. proj* " an, sodass MSBuild den Inhalt der Datei " *Rev-dev. proj* " in der Datei " *Publish. proj* " zusammenfasst.

Die nächsten Elemente, auf die MSBuild in der zusammengeführten Projektdatei stößt, sind Eigenschafts Gruppen. Eigenschaften werden in der Reihenfolge verarbeitet, in der Sie in der Datei angezeigt werden. MSBuild erstellt ein Schlüssel-Wert-Paar für jede Eigenschaft, sodass bestimmte Bedingungen erfüllt sind. Eigenschaften, die später in der Datei definiert werden, überschreiben alle Eigenschaften mit dem gleichen Namen, die zuvor in der Datei definiert wurden. Beachten Sie z. b. die **outputroot** -Eigenschaften.

[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]

Wenn MSBuild das erste **outputroot** -Element verarbeitet und ein ähnlich benannter Parameter nicht bereitgestellt wird, wird der Wert der **outputroot** -Eigenschaft auf. festgelegt.  **\Publish\out**. Wenn das zweite **outputroot** -Element gefunden wird, wird der Wert der **outputroot** -Eigenschaft mit dem Wert des **OutDir** -Parameters überschrieben, wenn die Bedingung als **true**ausgewertet wird.

Das nächste Element, das von MSBuild erkannt wird, ist eine einzelne Element Gruppe, die ein Element mit dem Namen **projectstobuild**enthält.

[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]

MSBuild verarbeitet diese Anweisung, indem eine Elementliste mit dem Namen **projectstobuild erstellt**wird. In diesem Fall enthält die Elementliste einen einzelnen Wert&#x2014;für den Pfad und den Dateinamen der Projektmappendatei.

An diesem Punkt sind die restlichen Elemente Ziele. Ziele werden von Eigenschaften und Elementen&#x2014;anders verarbeitet. Ziele werden nur dann verarbeitet, wenn Sie entweder explizit vom Benutzer angegeben oder von einem anderen Konstrukt in der Projektdatei aufgerufen werden. Beachten Sie, dass das öffnende **projekttags ein DefaultTargets** -Attribut enthält.

[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]

Dadurch wird MSBuild angewiesen, das **fullpublish** -Ziel aufzurufen, wenn keine Ziele angegeben werden, wenn "MSBuild. exe" aufgerufen wird. Das **fullpublish** -Ziel enthält keine Tasks. Stattdessen wird einfach eine Liste von Abhängigkeiten angegeben.

[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]

Diese Abhängigkeit teilt MSBuild mit, dass die Liste der Ziele in der angegebenen Reihenfolge aufgerufen werden muss, um das **fullpublish** -Ziel auszuführen:

1. Das **Clean** -Ziel muss aufgerufen werden.
2. Das **buildprojects** -Ziel muss aufgerufen werden.
3. Er muss das **gatherpackagesforpublishing** -Ziel aufrufen.
4. Er muss das **publishdbpackages** -Ziel aufrufen.
5. Er muss das **publishwebpackages** -Ziel aufrufen.

### <a name="the-clean-target"></a>Das Clean-Ziel

Das **Clean** -Ziel löscht im Grunde das Ausgabeverzeichnis und seinen gesamten Inhalt, als Vorbereitung für einen neuen Build.

[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]

Beachten Sie, dass das Ziel ein **ItemGroup** -Element enthält. Wenn Sie Eigenschaften oder Elemente innerhalb eines **target** -Elements definieren, erstellen Sie *dynamische* Eigenschaften und Elemente. Mit anderen Worten, die Eigenschaften oder Elemente werden erst verarbeitet, wenn das Ziel ausgeführt wird. Das Ausgabeverzeichnis ist möglicherweise nicht vorhanden oder enthält bis zum Beginn des Buildprozesses Dateien, sodass Sie die **\_filestodelete** -Liste nicht als statisches Element erstellen können. Sie müssen warten, bis die Ausführung ausgeführt wird. Daher erstellen Sie die Liste als dynamisches Element innerhalb des Ziels.

> [!NOTE]
> In diesem Fall ist es nicht erforderlich, eine dynamische Element Gruppe zu verwenden, da das **Clean** -Ziel das erste auszuführen ist. Es empfiehlt sich jedoch, dynamische Eigenschaften und Elemente in dieser Art von Szenario zu verwenden, da Sie Ziele möglicherweise zu einem bestimmten Zeitpunkt in einer anderen Reihenfolge ausführen möchten.  
> Sie sollten auch vermeiden, Elemente zu deklarieren, die nie verwendet werden. Wenn Sie über Elemente verfügen, die nur von einem bestimmten Ziel verwendet werden, erwägen Sie, diese innerhalb des Ziels zu platzieren, um unnötige Verwaltungsaufwand für den Buildprozess zu entfernen.

Dynamische Elemente: das **Clean** -Ziel ist recht unkompliziert und nutzt die integrierten **Message**-, **Delete**-und **RemoveDir** -Aufgaben für folgende Aufgaben:

1. Sendet eine Nachricht an die Protokollierung.
2. Erstellen Sie eine Liste der zu löschenden Dateien.
3. Löschen Sie die Dateien.
4. Entfernen Sie das Ausgabeverzeichnis.

### <a name="the-buildprojects-target"></a>Das buildprojects-Ziel

Das **buildprojects** -Ziel erstellt im Grunde alle Projekte in der Beispiel Projekt Mappe.

[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]

Dieses Ziel wurde im vorherigen Thema, Grundlegendes [zur Projektdatei](understanding-the-project-file.md), ausführlich beschrieben, um zu veranschaulichen, wie Tasks und Ziele auf Eigenschaften und Elemente verweisen. An diesem Punkt sind Sie hauptsächlich an der **MSBuild** -Aufgabe interessiert. Sie können diesen Task verwenden, um mehrere Projekte zu erstellen. Der Task erstellt keine neue Instanz von "MSBuild. exe". Es verwendet die aktuell aktive Instanz, um die einzelnen Projekte zu erstellen. Die wichtigsten wichtigen Punkte in diesem Beispiel sind die Bereitstellungs Eigenschaften:

- Die **deployonbuild** -Eigenschaft weist MSBuild an, Bereitstellungs Anweisungen in den Projekteinstellungen auszuführen, wenn der Build der einzelnen Projekte fertiggestellt ist.
- Die **deploytarget** -Eigenschaft identifiziert das Ziel, das Sie nach dem Erstellen des Projekts aufrufen möchten. In diesem Fall erstellt das **Paketziel** die Projekt Ausgabe in ein bereitstell bares Webpaket.

> [!NOTE]
> Das **Paketziel** Ruft die Webpublishing Pipeline (WPP) auf, die die Integration zwischen MSBuild und Web deploy ermöglicht. Wenn Sie sich die integrierten Ziele ansehen möchten, die die WPP bietet, überprüfen Sie die Datei " *Microsoft. Web. Publishing. targets* " im Ordner "% Program Files (x86)% \ msbuild\microsoft\visualstudio\v10.0\web".

### <a name="the-gatherpackagesforpublishing-target"></a>Das gatherpackagesforpublishing-Ziel

Wenn Sie das **gatherpackagesforpublishing** -Ziel untersuchen, werden Sie feststellen, dass es keine Aufgaben enthält. Stattdessen enthält Sie eine einzelne Element Gruppe, die drei dynamische Elemente definiert.

[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]

Diese Elemente beziehen sich auf die Bereitstellungs Pakete, die beim Ausführen des **buildprojects** -Ziels erstellt wurden. Diese Elemente konnten nicht statisch in der Projektdatei definiert werden, da die Dateien, auf die die Elemente verweisen, erst vorhanden sind, wenn das **buildprojects** -Ziel ausgeführt wird. Stattdessen müssen die Elemente dynamisch innerhalb eines Ziels definiert werden, das erst aufgerufen wird, nachdem das **buildprojects** -Ziel ausgeführt wurde.

Die Elemente werden in diesem Ziel&#x2014;nicht verwendet. dieses Ziel erstellt lediglich die Elemente und die Metadaten, die den einzelnen Element Werten zugeordnet sind. Nachdem diese Elemente verarbeitet wurden, enthält das **publishpackages** -Element zwei Werte, den Pfad zur Datei " *ContactManager. MVC.* Bereitstellungs Datei" und den Pfad zur Datei " *ContactManager. Service. Bereitstellungs. cmd* ". Web deploy diese Dateien als Teil des Webpakets für die einzelnen Projekte erstellt, und dies sind die Dateien, die Sie auf dem Zielserver aufrufen müssen, um die Pakete bereitzustellen. Wenn Sie eine dieser Dateien öffnen, sehen Sie im Grunde einen msbereitstellungs. exe-Befehl mit verschiedenen buildspezifischen Parameterwerten.

Das Element " **dbpublishpackages** " enthält einen einzelnen Wert, den Pfad zur Datei " *ContactManager. Database. DeployManifest* ".

> [!NOTE]
> Eine DeployManifest-Datei wird generiert, wenn Sie ein Datenbankprojekt erstellen, und es wird das gleiche Schema wie eine MSBuild-Projektdatei verwendet. Sie enthält alle Informationen, die zum Bereitstellen einer Datenbank erforderlich sind, einschließlich des Speicher Orts des Datenbankschemas (. dbschema) und Details zu allen Skripts vor und nach der Bereitstellung. Weitere Informationen finden Sie unter [Übersicht über die Daten Bank Build & Deployment](https://msdn.microsoft.com/library/aa833165.aspx).

Erfahren Sie mehr darüber, wie Bereitstellungs-und datenbankbereitstellungsmanifeste erstellt und zum [Erstellen und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md) und Bereitstellen von [Datenbankprojekten](deploying-database-projects.md)verwendet werden

### <a name="the-publishdbpackages-target"></a>Das publishdbpackages-Ziel

Kurz gesagt: das Ziel " **publishdbpackages** " Ruft das Hilfsprogramm "VSDBCmd" auf, um die Datenbank " **ContactManager** " in einer Zielumgebung bereitzustellen. Das Konfigurieren der Daten Bank Bereitstellung umfasst viele Entscheidungen und Nuancen. Weitere Informationen hierzu finden Sie unter Bereitstellen von [Datenbankprojekten](deploying-database-projects.md) und [Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). In diesem Thema konzentrieren wir uns auf die Funktionsweise dieses Ziels.

Beachten Sie zunächst, dass das öffnende Tag ein **Outputs** -Attribut enthält.

[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]

Dies ist ein Beispiel für die Batch Verarbeitung von *Zielen*. In MSBuild-Projektdateien ist die Batch Verarbeitung eine Technik zum Durchlaufen von Auflistungen. Der Wert des **Outputs** -Attributs **"% (dbpublishpackages. Identity)"** verweist auf die **identitätsmetadateneigenschaft** der **dbpublishpackages** -Elementliste. Diese Notation, **Outputs =% * * * (itemList. ItemMetaDataName)* , wird übersetzt als:

- Teilen Sie die Elemente in **dbpublishpackages** in Batches von Elementen auf, die denselben **Identitäts** Metadatenwert enthalten.
- Führen Sie das Ziel einmal pro Batch aus.

> [!NOTE]
> **Identity** ist einer der [integrierten Metadatenwerte](https://msdn.microsoft.com/library/ms164313.aspx) , der jedem Element bei der Erstellung zugewiesen wird. Er verweist auf den Wert des **include** -Attributs im **Item** -&#x2014;Element, d. h. auf den Pfad und den Dateinamen des Elements.

Da in diesem Fall nie mehr als ein Element mit demselben Pfad und Dateinamen vorhanden sein sollte, arbeiten wir im Wesentlichen mit den Batch Größen von einem. Das Ziel wird für jedes Daten Bank Paket einmal ausgeführt.

Sie können eine ähnliche Notation in der **\_cmd** -Eigenschaft sehen, die einen VSDBCmd-Befehl mit den entsprechenden Schaltern erstellt.

[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]

In diesem Fall verweisen **% (dbpublishpackages. databaseconnectionstring)** , **% (dbpublishpackages. TargetDatabase)** und **% (dbpublishpackages. FullPath)** alle auf Metadatenwerte der **dbpublishpackages** -Element Auflistung. Die **\_cmd** -Eigenschaft wird von der **exec** -Aufgabe verwendet, die den Befehl aufruft.

[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]

Aufgrund dieser Notation erstellt der **exec** -Task Batches auf der Grundlage eindeutiger Kombinationen aus den Metadatenwerten **databaseconnectionstring**, **TargetDatabase**und **FullPath** , und die Aufgabe wird für jeden Batch einmal ausgeführt. Dies ist ein Beispiel für die Batch Verarbeitung von *Aufgaben*. Da die Batch Verarbeitung auf Ziel Ebene jedoch die Element Auflistung bereits in Einzelelement Batches aufgeteilt hat, wird der **exec** -Task nur einmal für jede Iterations Iterationen ausgeführt. Mit anderen Worten: diese Aufgabe ruft das Hilfsprogramm "VSDBCmd" für jedes Daten Bank Paket in der Lösung einmal auf.

> [!NOTE]
> Weitere Informationen zur Batch Verarbeitung von Ziel-und Aufgaben finden Sie unter MSBuild- [Batch](https://msdn.microsoft.com/library/ms171473.aspx)Verarbeitung, [Element Metadaten in Ziel Batch](https://msdn.microsoft.com/library/ms228229.aspx)Verarbeitung und [Element Metadaten in der Task Batch](https://msdn.microsoft.com/library/ms171474.aspx)Verarbeitung.

### <a name="the-publishwebpackages-target"></a>Das publishwebpackages-Ziel

An diesem Punkt haben Sie das Ziel " **buildprojects** " aufgerufen, das für jedes Projekt in der Beispiellösung ein Webbereitstellungs Paket generiert. Das zugehörige Paket ist eine Datei "bereitstellen *. cmd* ", die die für die Bereitstellung des Pakets in der Zielumgebung erforderlichen msset. exe-Befehle sowie die Datei " *SetParameters. XML* " enthält, in der die erforderlichen Details der Zielumgebung angegeben werden. Sie haben auch das Ziel " **gatherpackagesforpublishing** " aufgerufen, das eine Element Sammlung mit den für Sie interessanten *cmd* -Dateien für die Bereitstellung generiert. Im wesentlichen führt das **publishwebpackages** -Ziel die folgenden Funktionen aus:

- Mithilfe der **XmlPoke** -Aufgabe wird die Datei " *SetParameters. XML* " für jedes Paket so manipuliert, dass die richtigen Details für die Zielumgebung enthalten sind.
- Die Datei "bereitstellen *. cmd* " wird für jedes Paket mit den entsprechenden Switches aufgerufen.

Ebenso wie das **publishdbpackages** -Ziel verwendet das **publishwebpackages** -Ziel die Ziel-Batch Verarbeitung, um sicherzustellen, dass das Ziel für jedes Webpaket einmal ausgeführt wird.

[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]

Innerhalb des Ziels wird die **exec** -Aufgabe verwendet, um die Datei "bereitstellen *. cmd* " für jedes Webpaket auszuführen.

[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]

Weitere Informationen zum Konfigurieren der Bereitstellung von Webpaketen finden Sie im Thema zum entwickeln [und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Zusammenfassung

Dieses Thema bietet eine exemplarische Vorgehensweise für die Verwendung von Split-Projektdateien zum Steuern des Build-und Bereitstellungs Prozesses für die Contact Manager-Beispiellösung von Anfang bis Ende. Mithilfe dieses Ansatzes können Sie komplexe bereit Stellungen auf Unternehmensebene in einem einzelnen, wiederholbaren Schritt ausführen, indem Sie einfach eine Umgebungs spezifische Befehlsdatei ausführen.

## <a name="further-reading"></a>Weitere nützliche Informationen

Eine ausführlichere Einführung in Projektdateien und WPP finden Sie unter [in der Microsoft-Build-Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) von Sayed Ibrahim Hashimi und William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](understanding-the-project-file.md)
> [Weiter](building-and-packaging-web-application-projects.md)
