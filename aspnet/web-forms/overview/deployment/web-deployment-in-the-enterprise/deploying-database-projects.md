---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Bereitstellen von Datenbankprojekten | Microsoft-Dokumentation
author: jrjlee
description: 'Hinweis: in vielen Szenarien für die Unternehmens Bereitstellung ist es erforderlich, inkrementelle Updates für eine bereitgestellte Datenbank zu veröffentlichen. Die Alternative besteht darin, neu zu erstellen...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 221808758492aedb8e8329364e511df28fd11105
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514941"
---
# <a name="deploying-database-projects"></a>Bereitstellen von Datenbankprojekten

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> In vielen Szenarien für die Unternehmens Bereitstellung benötigen Sie die Möglichkeit, inkrementelle Updates für eine bereitgestellte Datenbank zu veröffentlichen. Die Alternative besteht darin, die Datenbank bei jeder Bereitstellung neu zu erstellen. das bedeutet, dass alle Daten in der vorhandenen Datenbank verloren gehen. Wenn Sie mit Visual Studio 2010 arbeiten, ist die Verwendung von VSDBCMD der empfohlene Ansatz für die inkrementelle Daten Bank Veröffentlichung. Die nächste Version von Visual Studio und die Web Publishing Pipeline (WPP) enthalten jedoch Tools, die die inkrementelle Veröffentlichung direkt unterstützen.

Wenn Sie die Beispiellösung Contact Manager in Visual Studio 2010 öffnen, sehen Sie, dass das Datenbankprojekt einen Ordner properties enthält, der vier Dateien enthält.

![](deploying-database-projects/_static/image1.png)

Mit der Projektdatei (in diesem Fall "*ContactManager. Database. dbproj* ") können diese Dateien verschiedene Aspekte des Build-und Bereitstellungs Prozesses steuern:

- Die Datei *Database. sqlcmdvars* enthält Werte für alle sqlcmd-Variablen, die Sie verwenden, wenn Sie das Projekt bereitstellen. Jede Projektmappenkonfiguration (z. b. Debug und Release) kann eine andere sqlcmdvars-Datei angeben.
- Die Datei *Database. sqldeployment* stellt Bereitstellungs spezifische Einstellungen bereit, z. b. ob die im Projekt definierte Sortierung oder die Sortierung des Zielservers verwendet werden soll, ob die Zieldatenbank jedes Mal neu erstellt oder einfach die vorhandene Datenbank geändert werden soll, um Sie auf den neuesten Stand zu bringen usw. In jeder Projektmappenkonfiguration kann eine andere sqldeployment-Datei angegeben werden.
- Die *Datenbank. sqlberechtigungs* Datei ist ein XML-Dokument, das Sie verwenden können, um alle Berechtigungen zu definieren, die Sie der Zieldatenbank hinzufügen möchten. Alle Projektmappenkonfigurationen verwenden die gleiche. sqlberechtigungs Datei.
- Die *Datenbank. sqlsettings* -Datei gibt die Eigenschaften auf Datenbankebene an, die beim Erstellen der Datenbank verwendet werden sollen, wie z. b. die zu verwendende Sortierung, das Verhalten von Vergleichs Operatoren usw. Alle Projektmappenkonfigurationen verwenden dieselbe sqlsettings-Datei.

Es lohnt sich, diese Dateien in Visual Studio zu öffnen und sich mit den Inhalten vertraut zu machen.

Beim Erstellen eines Datenbankprojekts werden vom Buildprozess zwei Dateien erstellt:

- Ein *Datenbankschema* (dbschema-Datei). Dies beschreibt das Schema der Datenbank, die Sie im XML-Format erstellen möchten.
- Ein *Bereitstellungs Manifest* (. DeployManifest-Datei). Diese enthält alle Informationen, die zum Erstellen und Bereitstellen der Datenbank erforderlich sind. Er verweist auf die dbschema-Datei zusammen mit anderen Ressourcen, wie z. b. die Bereitstellungs Anweisungen (sqldeployment-Datei) und alle SQL-Skripts vor oder nach der Bereitstellung.

Dadurch wird die Beziehung zwischen diesen Ressourcen angezeigt:

![](deploying-database-projects/_static/image2.png)

Wie Sie sehen können, sind die sqlsettings-Datei und die sqlberechtigungs-Datei Eingaben für den Buildprozess. Diese Dateien werden zusammen mit der Datenbankprojekt Datei zum Erstellen der Datenbankschema Datei verwendet. Die sqldeployment-Datei und die sqlcmdvars-Datei durchlaufen den Buildprozess unverändert. Das Bereitstellungs Manifest gibt den Speicherort des Datenbankschemas, die sqldeployment-Datei, die sqlcmdvars-Datei und alle SQL-Skripts vor oder nach der Bereitstellung an.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Gründe für die Bereitstellung eines Datenbankprojekts mit VSDBCmd

Es gibt verschiedene Ansätze zum Bereitstellen von Datenbankprojekten. Allerdings sind nicht alle für die Bereitstellung eines Datenbankprojekts auf Remote Servern in einer Unternehmensumgebung geeignet. Beachten Sie, was Sie in einer Datenbankprojekt Bereitstellung wünschen. In Szenarien für die Unternehmens Bereitstellung benötigen Sie wahrscheinlich Folgendes:

- Die Möglichkeit, das Datenbankprojekt von einem Remote Standort aus bereitzustellen.
- Die Möglichkeit, inkrementelle Aktualisierungen für eine vorhandene Datenbank vorzunehmen.
- Die Möglichkeit, Skripts vor der Bereitstellung oder Skripts nach der Bereitstellung einzubeziehen.
- Die Möglichkeit, die Bereitstellung auf mehrere Ziel Umgebungen abzustimmen.
- Die Möglichkeit zur Bereitstellung des Datenbankprojekts als Teil einer größeren, in der Regel bereitgestellten Lösung für die Bereitstellung von Einzelschritten.

Es gibt drei Hauptansätze, die Sie verwenden können, um ein Datenbankprojekt bereitzustellen:

- Sie können die Bereitstellungs Funktion mit dem Daten Bank Projekttyp in Visual Studio 2010 verwenden. Wenn Sie ein Datenbankprojekt in Visual Studio 2010 erstellen und bereitstellen, verwendet der Bereitstellungs Prozess das Bereitstellungs Manifest, um eine für die Buildkonfiguration spezifische SQL-basierte Bereitstellungs Datei zu generieren. Dadurch wird die Datenbank erstellt, wenn Sie noch nicht vorhanden ist, oder es werden erforderliche Änderungen an der Datenbank vorgenommen, wenn Sie bereits vorhanden ist. Sie können "sqlcmd. exe" verwenden, um diese Datei auf dem Zielserver auszuführen, oder Sie können festlegen, dass Visual Studio die Datei erstellt und ausgeführt wird. Der Nachteil dieses Ansatzes besteht darin, dass Sie nur eingeschränkte Kontrolle über die Bereitstellungs Einstellungen haben. Möglicherweise müssen Sie auch die SQL-Bereitstellungs Datei ändern, um Umgebungs spezifische Variablen Werte bereitzustellen. Sie können diesen Ansatz nur auf einem Computer verwenden, auf dem Visual Studio 2010 installiert ist, und der Entwickler muss die Verbindungs Zeichenfolgen und Anmelde Informationen für alle Ziel Umgebungen kennen.
- Sie können das Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) verwenden, um [eine Datenbank als Teil eines Webanwendungs Projekts](https://msdn.microsoft.com/library/dd465343.aspx)bereitzustellen. Dieser Ansatz ist jedoch viel komplexer, wenn Sie ein Datenbankprojekt bereitstellen möchten, anstatt einfach eine vorhandene lokale Datenbank auf einem Zielserver zu replizieren. Sie können Web deploy konfigurieren, um das SQL-Bereitstellungs Skript auszuführen, das vom Datenbankprojekt generiert wird. hierfür müssen Sie jedoch eine benutzerdefinierte WPP-Zieldatei für das Webanwendungs Projekt erstellen. Dadurch wird der Bereitstellungs Prozess erheblich komplexer. Darüber hinaus unterstützt Web deploy inkrementelle Updates für vorhandene Datenbanken nicht direkt. Weitere Informationen zu diesem Ansatz finden Sie unter [Erweitern der Webpublishing Pipeline auf die Paketdaten Bank Projekt](https://go.microsoft.com/?linkid=9805121)bereitgestellte SQL-Datei.
- Sie können das Hilfsprogramm VSDBCmd verwenden, um die Datenbank mithilfe des Datenbankschemas oder des Bereitstellungs Manifests bereitzustellen. Sie können "VSDBCMD. exe" von einem MSBuild-Ziel aus anrufen, mit dem Sie Datenbanken im Rahmen eines größeren, Skript gesteuerten Bereitstellungs Prozesses veröffentlichen können. Sie können die Variablen in der sqlcmdvars-Datei und viele andere Daten Bank Eigenschaften aus einem VSDBCmd-Befehl überschreiben, sodass Sie die Bereitstellung für verschiedene Umgebungen anpassen können, ohne mehrere Buildkonfigurationen zu erstellen. VSDBCMD bietet Differenzierungs Funktionen. Dies bedeutet, dass nur die erforderlichen Änderungen vorgenommen werden müssen, um eine Zieldatenbank mit dem Datenbankschema auszurichten. VSDBCMD bietet auch eine große Bandbreite an Befehlszeilenoptionen, mit denen Sie den Bereitstellungs Prozess präzise steuern können.

In dieser Übersicht können Sie sehen, dass die Verwendung von VSDBCMD mit MSBuild der Ansatz ist, der sich am besten für ein typisches Szenario für die Unternehmens Bereitstellung eignet:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Unterstützt die Remote Bereitstellung? | Ja | Ja | Ja |
| Unterstützt inkrementelle Updates | Ja | Nein | Ja |
| Unterstützt Skripts vor und nach der Bereitstellung? | Ja | Ja | Ja |
| Unterstützt die Bereitstellung in mehreren Umgebungen? | Eingeschränkt | Eingeschränkt | Ja |
| Unterstützt die Skript gesteuerte Bereitstellung? | Eingeschränkt | Ja | Ja |

Im restlichen Teil dieses Themas wird die Verwendung von VSDBCMD mit MSBuild zum Bereitstellen von Datenbankprojekten beschrieben.

## <a name="understanding-the-deployment-process"></a>Grundlegendes zum Bereitstellungs Prozess

Mit dem VSDBCmd-Hilfsprogramm können Sie eine Datenbank entweder mit dem Datenbankschema (dbschema-Datei) oder dem Bereitstellungs Manifest (DeployManifest-Datei) bereitstellen. In der Praxis verwenden Sie fast immer das Bereitstellungs Manifest, da Sie mit dem Bereitstellungs Manifest Standardwerte für verschiedene Bereitstellungs Eigenschaften bereitstellen und alle SQL-Skripts vor oder nach der Bereitstellung identifizieren können, die Sie ausführen möchten. Beispielsweise wird dieser VSDBCmd-Befehl verwendet, um die **ContactManager** -Datenbank auf einem Datenbankserver in einer Testumgebung bereitzustellen:

[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]

In diesem Fall:

- Der Schalter **/a** (oder **/Action**) gibt an, was VSDBCmd tun soll. Sie können diese Einstellung **importieren** **oder bereit**stellen. Die **Import** Option wird verwendet, um eine dbschema-Datei aus einer vorhandenen Datenbank zu generieren, **und die** Bereitstellungs Option wird verwendet, um eine dbschema-Datei in einer Zieldatenbank bereitzustellen.
- Der Schalter **/Manifest** (oder **/MANIFESTFILE**) identifiziert die. DeployManifest-Datei, die Sie bereitstellen möchten. Wenn Sie stattdessen die dbschema-Datei verwenden möchten, verwenden Sie den Schalter **/Model** (oder **/ModelFile**).
- Der Schalter **/CS** (oder **'/ConnectionString '** ) stellt die Verbindungs Zeichenfolge für den Ziel Datenbankserver bereit. Beachten Sie, dass dies nicht den Namen der Datenbank&#x2014;enthält, mit der VSDBCmd eine Verbindung mit dem Server herstellen muss, um die Datenbank zu erstellen. Es muss keine Verbindung mit einer einzelnen Datenbank hergestellt werden. Wenn Ihre DeployManifest-Datei eine Verbindungs Zeichenfolge enthält, können Sie diesen Schalter weglassen. Wenn Sie den Switch trotzdem verwenden, wird der Wert von ". DeployManifest" durch den Schalter Wert überschrieben.
- Die <strong>/p: TargetDatabase</strong> -Eigenschaft gibt den Namen an, der der Zieldatenbank bei der Erstellung zugewiesen werden soll. Dies überschreibt den Wert der <strong>TargetDatabase</strong> -Eigenschaft in der DeployManifest-Datei. Sie können die Syntax <strong>/p:</strong> <em>[Property Name]</em>verwenden, um eine Vielzahl von Bereitstellungs Eigenschaften festzulegen und alle sqlcmd-Variablen zu überschreiben, die in der sqlcmdvars-Datei deklariert sind.
- Der Schalter **/DD +** (oder **/DeployToDatabase +** ) gibt an, dass Sie eine Bereitstellung erstellen und in der Zielumgebung bereitstellen möchten. Wenn Sie **/DD-** angeben oder den Schalter weglassen, wird von VSDBCMD ein Bereitstellungs Skript generiert, aber nicht in der Zielumgebung bereitgestellt. Dieser Schalter ist häufig die Ursache für Verwirrung und wird im nächsten Abschnitt ausführlicher erläutert.
- Der Schalter **/Script** (oder **/DeploymentScriptFile**) gibt an, wo Sie das Bereitstellungs Skript generieren möchten. Dieser Wert hat keine Auswirkung auf den Bereitstellungs Prozess.

Weitere Informationen zu VSDBCmd finden Sie unter [Befehlszeilen Referenz für "VSDBCmd". EXE (Bereitstellung und Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx) und Vorgehens [Weise: Vorbereiten einer Datenbank für die Bereitstellung über eine Eingabeaufforderung mithilfe von VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Ein Beispiel für die Verwendung von VSDBCMD aus einer MSBuild-Projektdatei finden Sie Untergrund Legendes [zum Buildprozess](understanding-the-build-process.md). Beispiele zum Konfigurieren von Einstellungen für die Daten Bank Bereitstellung für mehrere Umgebungen finden Sie unter [Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Grundlegendes zum "deploydedatabase"-Schalter

Das Verhalten des **/DD** -oder **/DeployToDatabase** -Schalters hängt davon ab, ob Sie VSDBCmd mit einer dbschema-Datei oder einer DeployManifest-Datei verwenden. Wenn Sie eine dbschema-Datei verwenden, ist das Verhalten recht einfach:

- Wenn Sie **/DD +** oder **/DD**angeben, generiert VSDBCmd ein Bereitstellungs Skript und stellt die Datenbank bereit.
- Wenn Sie **/DD-** angeben oder den Schalter weglassen, wird von VSDBCMD nur ein Bereitstellungs Skript generiert.

Wenn Sie eine DeployManifest-Datei verwenden, ist das Verhalten viel komplizierter. Dies liegt daran, dass die DeployManifest-Datei einen Eigenschaften Namen **deploydedatabase** enthält, der auch bestimmt, ob die Datenbank bereitgestellt wird.

[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]

Der Wert dieser Eigenschaft wird entsprechend den Eigenschaften des Datenbankprojekts festgelegt. Wenn Sie die **Aktion** bereitstellen so festlegen, dass **ein Bereitstellungs Skript (. SQL) erstellt**wird, lautet der Wert **false**. Wenn Sie die **Aktion** bereitstellen zum **Erstellen eines Bereitstellungs Skripts (. SQL) und zum Bereitstellen in der Datenbank**festlegen, ist der Wert **true**.

> [!NOTE]
> Diese Einstellungen sind einer bestimmten Buildkonfiguration und Plattform zugeordnet. Wenn Sie z. b. Einstellungen für die **Debugkonfiguration** konfigurieren und dann mithilfe der **Releasekonfiguration** veröffentlichen, werden Ihre Einstellungen nicht verwendet.

![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> In diesem Szenario sollte die Bereitstellungs **Aktion** immer so festgelegt werden, dass Sie **ein Bereitstellungs Skript (. SQL) erstellt**, da Sie nicht möchten, dass Visual Studio 2010 Ihre Datenbank bereitstellt. Anders ausgedrückt: die **deploydedatabase** -Eigenschaft sollte immer **false**sein.

Wenn eine **deploydedatabase** -Eigenschaft angegeben wird, überschreibt der **/DD** -Schalter nur die-Eigenschaft, wenn der-Eigenschafts Wert " **false**" lautet:

- Wenn die Eigenschaft **deploydedatabase** den Wert **false**hat und Sie **/DD +** oder **/DD**angeben, überschreibt VSDBCmd die Eigenschaft **deploydedatabase** und stellt die Datenbank bereit.
- Wenn die **deploydedatabase** -Eigenschaft auf **false**festgelegt ist und Sie **/DD-** angeben oder den Schalter weglassen, wird die Datenbank von VSDBCMD nicht bereitgestellt.
- Wenn die **deploydedatabase** -Eigenschaft den Wert **true**hat, ignoriert VSDBCmd den Switch und stellt die Datenbank bereit.
- In jedem Fall wird ein Bereitstellungs Skript generiert, unabhängig davon, ob Sie die Datenbank ebenfalls bereitstellen.

## <a name="conclusion"></a>Zusammenfassung

Dieses Thema bietet einen Überblick über den Build-und Bereitstellungs Prozess für Datenbankprojekte in Visual Studio 2010. Außerdem wird beschrieben, wie Sie VSDBCMD. exe mit MSBuild verwenden können, um die Daten Bank Bereitstellung auf Unternehmens Niveau zu unterstützen.

Weitere Informationen zur Vorgehensweise in der Praxis finden Sie unter [Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Weitere nützliche Informationen

Informationen zum Anpassen von Daten Bank Bereitstellungen durch Erstellen einer separaten Bereitstellungs Konfigurationsdatei für jede Umgebung finden Sie unter [Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Eine Anleitung zum Konfigurieren von Daten bankrollen Mitgliedschaften durch Ausführen eines Skripts nach der Bereitstellung finden Sie unter Bereitstellen von [Daten bankrollen Mitgliedschaften in Test Umgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Anleitungen zum Verwalten einiger der besonderen Herausforderungen, die von Mitgliedschafts Datenbanken auferlegt werden, finden Sie unter Bereitstellen von [Mitgliedschafts Datenbanken in Unternehmensumgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Diese Themen auf MSDN bieten umfassendere Anleitungen und Hintergrundinformationen zu Visual Studio-Datenbankprojekten und zum Daten Bank Bereitstellungs Prozess:

- [Visual Studio 2010 SQL Server-Datenbankprojekte](https://msdn.microsoft.com/library/ff678491.aspx)
- [Verwalten von Daten Bank Änderungen](https://msdn.microsoft.com/library/aa833404.aspx)
- [Vorgehensweise: Vorbereiten einer Datenbank für die Bereitstellung über eine Eingabeaufforderung mithilfe von VSDBCMD. Speichert](https://msdn.microsoft.com/library/dd193258.aspx)
- [Eine Übersicht über die Daten Bank Build & Deployment](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-web-packages.md)
> [Weiter](creating-and-running-a-deployment-command-file.md)
