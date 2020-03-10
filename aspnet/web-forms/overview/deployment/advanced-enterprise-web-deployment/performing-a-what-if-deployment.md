---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Ausführen einer What if Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie mit dem Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) und V...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 73a0e038cc0d4ebae0ffc8ed3fd2de4c9dad673c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510333"
---
# <a name="performing-a-what-if-deployment"></a>Durchführen einer simulierten Bereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie mit dem Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) und VSDBCmd "Was-wäre-wenn-bereit Stellungen" (oder simulierte) bereit Stellungen ausführen. Auf diese Weise können Sie die Auswirkungen der Bereitstellungs Logik auf eine bestimmte Zielumgebung ermitteln, bevor Sie Ihre Anwendung tatsächlich bereitstellen.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz zum Aufteilen von Projektdateien, bei dem der Build-und Bereitstellungs Prozess von zwei Projekt&#x2014;Dateien gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Ausführen einer "What if"-Bereitstellung für Webpakete

Web deploy enthält Funktionen, mit denen Sie bereit Stellungen im "Was-wäre-wenn"-Modus (oder im Test Modus) ausführen können. Wenn Sie Artefakte im "Was-wäre-wenn"-Modus bereitstellen, generiert Web deploy eine Protokolldatei, als ob Sie die Bereitstellung durchgeführt hätten, aber auf dem Zielserver ändert sich nichts. Wenn Sie die Protokolldatei überprüfen, können Sie besser verstehen, welche Auswirkung Ihre Bereitstellung auf dem Zielserver hat. Dies gilt insbesondere für:

- Das, was hinzugefügt wird.
- Was wird aktualisiert...
- Was wird gelöscht?

Da eine "Was-wäre-wenn"-Bereitstellung nichts auf dem Zielserver ändert, kann nicht immer vorhergesagt werden, ob eine Bereitstellung erfolgreich ist.

Wie unter Bereitstellen von [Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md)beschrieben, können Sie Webpakete mithilfe von Web deploy auf&#x2014;zwei Arten bereitstellen, indem Sie das Befehlszeilen-Hilfsprogramm "msbereitstellungs. exe" direkt verwenden oder indem Sie die vom Buildprozess generierte Datei " *.* Bereitstellungs Datei" ausführen.

Wenn Sie msdeployment. exe direkt verwenden, können Sie eine "Was-wäre-wenn"-Bereitstellung ausführen, indem Sie dem Befehl das Flag **– WhatIf** hinzufügen. Wenn Sie beispielsweise auswerten möchten, was geschieht, wenn Sie das Paket "ContactManager. MVC. zip" in einer Stagingumgebung bereitgestellt haben, sollte der msbereitstellungs-Befehl wie folgt aussehen:

[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]

Wenn Sie mit den Ergebnissen der "Was-wäre-wenn"-Bereitstellung zufrieden sind, können Sie das Flag " **– WhatIf** " entfernen, um eine Live Bereitstellung auszuführen.

> [!NOTE]
> Weitere Informationen zu Befehlszeilenoptionen für "msbereitstellungs. exe" finden Sie unter [Web deploy Vorgangs Einstellungen](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Wenn Sie die Datei " *. Deployment. cmd* " verwenden, können Sie eine "Was-wäre-wenn"-Bereitstellung durchführen, indem Sie das Flag " **/t** Flag (Testmodus)" anstelle des **/y** -Flags ("yes" oder "Update") in den Befehl einschließen. Wenn Sie beispielsweise auswerten möchten, was geschieht, wenn Sie das Paket "ContactManager. MVC. zip" durch Ausführen der Datei ". Bereitstellungs *. cmd* " bereitgestellt haben, sollte der Befehl wie folgt aussehen:

[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]

Wenn Sie mit den Ergebnissen der "Test Modus"-Bereitstellung zufrieden sind, können Sie das **/t** -Flag durch ein **/y** -Flag ersetzen, um eine Live Bereitstellung auszuführen:

[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]

> [!NOTE]
> Weitere Informationen zu Befehlszeilenoptionen für. Bereitstellen von *cmd* -Dateien finden Sie unter Gewusst [wie: Installieren eines Bereitstellungs Pakets mit der Datei "Deployment. cmd](https://msdn.microsoft.com/library/ff356104.aspx)". Wenn Sie die *. cmd* -Datei ohne Angabe von Flags ausführen, wird in der Eingabeaufforderung eine Liste der verfügbaren Flags angezeigt.

## <a name="performing-a-what-if-deployment-for-databases"></a>Ausführen einer "What if"-Bereitstellung für Datenbanken

In diesem Abschnitt wird davon ausgegangen, dass Sie mit dem Hilfsprogramm VSDBCmd eine inkrementelle, Schema basierte Daten Bank Bereitstellung ausführen. Diese Vorgehensweise wird in Bereitstellen von [Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md)ausführlicher beschrieben. Wir empfehlen Ihnen, sich mit diesem Thema vertraut zu machen, bevor Sie die hier beschriebenen Konzepte anwenden.

Wenn Sie VSDBCmd im **Bereitstellungs Modus** verwenden, können Sie das Flag **/DD** (oder **/DeployToDatabase**) verwenden, um zu steuern, ob die Datenbank tatsächlich von VSDBCMD bereitgestellt wird oder nur ein Bereitstellungs Skript generiert wird. Wenn Sie eine dbschema-Datei bereitstellen, ist dies das folgende Verhalten:

- Wenn Sie **/DD +** oder **/DD**angeben, generiert VSDBCmd ein Bereitstellungs Skript und stellt die Datenbank bereit.
- Wenn Sie **/DD-** angeben oder den Schalter weglassen, wird von VSDBCMD nur ein Bereitstellungs Skript generiert.

> [!NOTE]
> Wenn Sie eine DeployManifest-Datei anstelle einer dbschema-Datei bereitstellen, ist das Verhalten des **/DD** -Schalters wesentlich komplizierter. Im Wesentlichen ignoriert VSDBCmd den Wert des **/DD** -Schalters, wenn die DeployManifest-Datei ein **deploydedatabase** -Element mit dem Wert " **true**" enthält. Beim Bereitstellen von [Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md) wird dieses Verhalten vollständig beschrieben.

Um z. b. ein Bereitstellungs Skript für die **ContactManager** -Datenbank zu generieren, ohne die Datenbank tatsächlich bereitzustellen, sollte der VSDBCmd-Befehl wie folgt aussehen:

[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]

VSDBCMD ist ein differenziertes Tool für die Daten Bank Bereitstellung. Daher wird das Bereitstellungs Skript dynamisch generiert, um alle SQL-Befehle zu enthalten, die erforderlich sind, um die aktuelle Datenbank, sofern vorhanden, in das angegebene Schema zu aktualisieren. Das Überprüfen des Bereitstellungs Skripts ist eine nützliche Möglichkeit, um zu bestimmen, welche Auswirkung die Bereitstellung auf die aktuelle Datenbank und die darin enthaltenen Daten hat. Beispielsweise können Sie Folgendes bestimmen:

- Ob vorhandene Tabellen entfernt werden und ob dies zu Datenverlusten führt.
- Ob die Reihenfolge der Vorgänge das Risiko eines Daten Verlusts birgt, z. b. Wenn Sie Tabellen aufteilen oder zusammenführen.

Wenn Sie mit dem Bereitstellungs Skript zufrieden sind, können Sie VSDBCmd mit einem **/DD +** -Flag wiederholen, um die Änderungen vorzunehmen. Alternativ können Sie das Bereitstellungs Skript bearbeiten, um Ihre Anforderungen zu erfüllen, und es dann manuell auf dem Datenbankserver ausführen.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrieren von "What if"-Funktionen in benutzerdefinierte Projektdateien

In komplexeren Bereitstellungs Szenarien empfiehlt es sich, eine benutzerdefinierte Microsoft-Build-Engine (MSBuild)-Projektdatei zu verwenden, um Ihre Build-und Bereitstellungs Logik zu kapseln, wie in Grundlegendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschrieben. Beispielsweise wird in der Beispiellösung " [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) " die Datei " *Publish. proj* " angezeigt:

- Erstellt die Projekt Mappe.
- Verwendet Web deploy, um die Anwendung ContactManager. MVC zu verpacken und bereitzustellen.
- Verwendet Web deploy, um die Anwendung ContactManager. Service zu verpacken und bereitzustellen.
- Stellt die **ContactManager** -Datenbank bereit.

Wenn Sie die Bereitstellung mehrerer Webpakete und/oder Datenbanken auf diese Weise in einen schrittweisen Prozess integrieren, können Sie auch die Option für die gesamte Bereitstellung im "Was-wäre-wenn"-Modus ausführen.

Die Datei " *Publish. proj* " veranschaulicht, wie Sie dies tun können. Zunächst müssen Sie eine Eigenschaft erstellen, um den "Was-wäre-wenn"-Wert zu speichern:

[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]

In diesem Fall haben Sie eine Eigenschaft mit dem Namen **WhatIf** mit dem Standardwert **false**erstellt. Benutzer können diesen Wert überschreiben, indem Sie in einem Befehlszeilenparameter die-Eigenschaft auf " **true** " festlegen, wie Sie in Kürze sehen werden.

Die nächste Phase besteht darin, alle Web deploy-und VSDBCmd-Befehle zu parametrisieren, damit die Flags den **WhatIf** -Eigenschafts Wert widerspiegeln. Beispielsweise wird durch das nächste Ziel (aus der Datei " *Publish. proj* " entnommen und vereinfacht) die Datei ". Bereitstellungs *. cmd* " ausgeführt, um ein Webpaket bereitzustellen. Standardmäßig enthält der Befehl einen **/Y** -Schalter ("yes" oder Update Mode). Wenn **WhatIf** auf **true**festgelegt ist, wird dies durch einen **/T** -Schalter (Testversion oder "Was-wäre-wenn-Modus") ersetzt.

[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]

Entsprechend verwendet das nächste Ziel das Hilfsprogramm "VSDBCmd", um eine Datenbank bereitzustellen. Standardmäßig ist kein **/DD** -Schalter enthalten. Dies bedeutet, dass VSDBCmd ein Bereitstellungs Skript generiert, aber die Datenbank&#x2014;nicht mit anderen Worten, einem "Was-wäre-wenn"-Szenario, bereitstellt. Wenn die **WhatIf** -Eigenschaft nicht auf **true**festgelegt ist, wird ein **/DD** -Switch hinzugefügt, und die Datenbank wird von VSDBCMD bereitgestellt.

[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]

Sie können denselben Ansatz verwenden, um alle relevanten Befehle in der Projektdatei zu parametrisieren. Wenn Sie eine "Was-wäre-wenn"-Bereitstellung ausführen möchten, können Sie einfach einen **WhatIf** -Eigenschafts Wert über die Befehlszeile angeben:

[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]

Auf diese Weise können Sie eine "Was-wäre-wenn"-Bereitstellung für alle Projektkomponenten in einem einzigen Schritt ausführen.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie "Was-wäre-wenn"-bereit Stellungen mit Web deploy, VSDBCmd und MSBuild ausgeführt werden. Mit einer "Was-wäre-wenn"-Bereitstellung können Sie die Auswirkungen einer vorgeschlagenen Bereitstellung auswerten, bevor Sie tatsächlich Änderungen an der Zielumgebung vornehmen.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu Web deploy Befehlszeilen Syntax finden Sie unter [Web deploy Vorgangs Einstellungen](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Anleitungen zu Befehlszeilenoptionen bei Verwendung der Datei " *. Deployment. cmd* " finden Sie unter Gewusst [wie: Installieren eines Bereitstellungs Pakets mit der Datei "Deployment. cmd](https://msdn.microsoft.com/library/ff356104.aspx)". Anleitungen zur Befehlszeilen Syntax von VSDBCMD finden Sie unter [Befehlszeilen Referenz für VSDBCMD. EXE (Bereitstellung und Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Zurück](advanced-enterprise-web-deployment.md)
> [Weiter](customizing-database-deployments-for-multiple-environments.md)
