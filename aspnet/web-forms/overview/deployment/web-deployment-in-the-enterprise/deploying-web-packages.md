---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Bereitstellen von Webpaketen | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie Webbereitstellungs Pakete mithilfe des Webbereitstellungs Tools für Internetinformationsdienste (IIS) auf einem Remote Server veröffentlichen können.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 91b99e6e250342851aea6860164b6f6af54818d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464967"
---
# <a name="deploying-web-packages"></a>Bereitstellen von Webpaketen

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie Webbereitstellungs Pakete mithilfe des Internetinformationsdienste (IIS)-Webbereitstellungs Tools (Web deploy) 2,0 auf einem Remote Server veröffentlichen können.
> 
> Es gibt zwei Hauptmethoden, mit denen Sie ein Webpaket auf einem Remote Server bereitstellen können:
> 
> - Sie können das Befehlszeilen-Hilfsprogramm "msbereitstellungs. exe" direkt verwenden.
> - Sie können die Datei *[Project Name].* Bereitstellungs Datei ausführen, die der Buildprozess generiert.
> 
> Das Endergebnis ist unabhängig vom verwendeten Ansatz identisch. Im Wesentlichen besteht die gesamte Bereitstellung. *cmd* -Datei darin, MS". exe" mit einigen vordefinierten Werten auszuführen, sodass Sie nicht so viele Informationen bereitstellen müssen, um das Paket bereitzustellen. Dies vereinfacht den Bereitstellungs Prozess. Auf der anderen Seite bietet die Verwendung von "msbereitstellungs. exe" eine viel höhere Flexibilität bei der Bereitstellung des Pakets.
> 
> Welchen Ansatz Sie verwenden, hängt von einer Vielzahl von Faktoren ab, einschließlich der erforderlichen Kontrolle über den Bereitstellungs Prozess und der Angabe, ob Sie den Web deploy Remote-Agent-Dienst oder den Web deploy-Handler als Ziel verwenden. In diesem Thema wird erläutert, wie Sie die einzelnen Ansätze verwenden und ermitteln, wann die einzelnen Ansätze geeignet sind.
> 
> In den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird Folgendes vorausgesetzt:
> 
> - Sie haben Ihre Webanwendung erstellt und gepackt, wie unter [Erstellen und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md)beschrieben.
> - Sie haben die Datei " *SetParameters. XML* " so geändert, dass Sie die richtigen Parameterwerte für Ihre Zielumgebung bereitstellt, wie unter [Konfigurieren von Parametern für die Webpaket Bereitstellung](configuring-parameters-for-web-package-deployment.md)beschrieben.

Das Ausführen der Datei [*Project Name*]. bereitstellen *. cmd* ist die einfachste Möglichkeit, ein Webpaket bereitzustellen. Insbesondere bietet die Verwendung der Datei ". Bereitstellungs *. cmd* " diese Vorteile gegenüber der direkten Verwendung von "msbereitstellungs. exe":

- Sie müssen nicht den Speicherort des Webbereitstellungs Pakets&#x2014;angeben. die Datei ". Deployment *. cmd* " weiß bereits, wo Sie sich befindet.
- Sie müssen nicht den Speicherort der Datei&#x2014;" *SetParameters. XML* " angeben. die Bereitstellung *. cmd* -Datei weiß bereits, wo Sie sich befindet.
- Sie müssen keine Quell-und Ziel-msprovideranbieter&#x2014;angeben. die Bereitstellung *. cmd* -Datei weiß bereits, welche Werte verwendet werden sollen.
- Sie müssen keine msbereitstellungsvorgangeinstellungen&#x2014;angeben. die Bereitstellung. *cmd* -Datei fügt die häufig erforderlichen Werte automatisch dem Befehl "msbereitstellungs. exe" hinzu.

Bevor Sie ein Webpaket mithilfe der Datei. bereitstellen *. cmd* bereitstellen, sollten Sie Folgendes sicherstellen:

- Die. Bereitstellungs-. *cmd* -Datei, [*Projektname*]. Die Datei " *SetParameters. XML* " und das Webpaket ([*Project Name*]. *ZIP*) befinden sich im selben Ordner.
- Auf dem Computer, auf dem die Datei " *. cmd* " ausgeführt wird, ist Web deploy (msbereitstellungs. exe) installiert.

Die Datei ". Bereitstellungs *. cmd* " unterstützt verschiedene Befehlszeilenoptionen. Wenn Sie die Datei an einer Eingabeaufforderung ausführen, handelt es sich hierbei um die grundlegende Syntax:

[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]

Sie müssen entweder ein **/T** -Flag oder ein **/Y** -Flag angeben, um anzugeben, ob Sie einen Testlauf oder eine Live Bereitstellung ausführen möchten (verwenden Sie nicht beide Flags im gleichen Befehl). In dieser Tabelle wird der Zweck der einzelnen Flags erläutert.

| Flag | Beschreibung |
| --- | --- |
| **/T** | Ruft msbereitstellungs. exe mit dem **– WhatIf** -Flag auf, das einen Testlauf angibt. Anstatt das Paket bereitzustellen, erstellt es einen Bericht darüber, was passieren würde, wenn Sie das Paket bereitstellen. |
| **/Y** | Ruft msbereit. exe ohne das Flag **– WhatIf** auf. Dadurch wird das Paket auf dem lokalen Computer oder dem angegebenen Zielserver bereitgestellt. |
| **/M** | Gibt den Namen des Zielservers oder der Dienst-URL an. Weitere Informationen zu den Werten, die Sie hier bereitstellen können, finden Sie im Abschnitt **Überlegungen zum Endpunkt** in diesem Thema. Wenn Sie das **/M** -Flag weglassen, wird das Paket auf dem lokalen Computer bereitgestellt. |
| **/A** | Gibt den Authentifizierungstyp an, der von msdeployment. exe zum Ausführen der Bereitstellung verwendet werden soll. Mögliche Werte sind **NTLM** und **Basic**. Wenn Sie das **/A** -Flag weglassen, ist der Authentifizierungstyp standardmäßig **NTLM** für die Bereitstellung für den Web deploy Remote-Agent-Dienst und **Basic** für die Bereitstellung für den Web deploy Handler. |
| **/U** | Gibt den Benutzernamen an. Dies gilt nur, wenn Sie die Standard Authentifizierung verwenden. |
| **/P** | Gibt das Kennwort an. Dies gilt nur, wenn Sie die Standard Authentifizierung verwenden. |
| **/L** | Gibt an, dass das Paket auf der lokalen IIS Express Instanz bereitgestellt werden soll. |
| **/G** | Gibt an, dass das Paket mithilfe der [tempAgent-Anbieter Einstellung](https://technet.microsoft.com/library/ee517345(WS.10).aspx)bereitgestellt wird. Wenn Sie das **/G** -Flag weglassen, wird der Wert standardmäßig auf **false**gesetzt. |

> [!NOTE]
> Jedes Mal, wenn der Buildprozess ein Webpaket erstellt, wird auch eine Datei mit dem Namen *[Project Name]. deploy-Readme. txt* erstellt, in der diese Bereitstellungs Optionen erläutert werden.

Zusätzlich zu diesen Flags können Sie Web deploy Vorgangs Einstellungen als zusätzliche *. cmd* -Parameter angeben. Alle zusätzlichen Einstellungen, die Sie angeben, werden einfach an den zugrunde liegenden Befehl "msbereitstellungs. exe" weitergegeben. Weitere Informationen zu diesen Einstellungen finden Sie unter [Web deploy Vorgangs Einstellungen](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Angenommen, Sie möchten das Projekt "ContactManager. MVC-Webanwendung" in einer Testumgebung bereitstellen, indem Sie die Datei " *. cmd* " ausführen. Die Testumgebung ist so konfiguriert, dass Sie den Web deploy Remote-Agent-Dienst verwendet, wie unter [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)beschrieben. Zum Bereitstellen der-Webanwendung müssen Sie die nächsten Schritte ausführen.

**So stellen Sie eine Webanwendung mithilfe der. cmd-Datei bereit**

1. Erstellen und Verpacken Sie das Webanwendungs Projekt, wie unter [Erstellen und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md)beschrieben.
2. Ändern Sie die Datei " *ContactManager. MVC. SetParameters. XML* " so, dass Sie die richtigen Parameterwerte für die Testumgebung enthält, wie unter [Konfigurieren von Parametern für die Webpaket Bereitstellung](configuring-parameters-for-web-package-deployment.md)beschrieben.
3. Öffnen Sie ein Eingabe Aufforderungs Fenster, und navigieren Sie zum Speicherort der Datei " *ContactManager. MVC. Bereitstellungs. cmd* ".
4. Geben Sie diesen Befehl ein, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

In diesem Beispiel:

- Das Flag **/Y** gibt an, dass Sie das Paket tatsächlich bereitstellen möchten, anstatt einen Testlauf auszuführen.
- Das Flag **/M** gibt an, dass Sie das Paket auf dem Server mit dem Namen TESTWEB1 bereitstellen möchten. Von diesem Wert wird von "msbereitstellungs. exe" versucht, das Paket auf http://TESTWEB1/MSDeployAgentServiceauf dem Web deploy Remote-Agent-Dienst bereitzustellen.
- Das Flag **/A** gibt an, dass Sie die NTLM-Authentifizierung verwenden möchten. Daher müssen Sie keinen Benutzernamen und kein Kennwort angeben.

Um zu veranschaulichen, wie der Bereitstellungs Prozess mithilfe der Datei " *. Deployment. cmd* " vereinfacht wird, sehen Sie sich den Befehl "msdeployment. exe" an, der generiert und ausgeführt wird, wenn Sie " *ContactManager. MVC. Deployment. cmd* " mithilfe der oben gezeigten Optionen ausführen.

[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]

Weitere Informationen zur Verwendung der Datei " *. Deployment. cmd* " für die Bereitstellung eines Webpakets finden Sie unter Gewusst [wie: Installieren eines Bereitstellungs Pakets mit der Datei "Deployment. cmd](https://msdn.microsoft.com/library/ff356104.aspx)".

## <a name="using-msdeployexe"></a>Verwenden von msbereitstellungs. exe

Obwohl die Bereitstellung der Datei " *. Deployment. cmd* " den Bereitstellungs Prozess in der Regel vereinfacht, gibt es einige Situationen, in denen die Verwendung von "msdeployment. exe" vorzuziehen ist. Beispiel:

- Wenn Sie die Bereitstellung für den Web deploy Handler als Benutzer ohne Administratorrechte ausführen möchten, können Sie die Datei. bereitstellen *. cmd* nicht verwenden. Dies ist auf einen Fehler in Web deploy 2,0 zurückzuführen, wie unter **Endpunkt Überlegungen**beschrieben.
- Wenn Sie einen manuellen Wechsel zwischen verschiedenen *SetParameters. XML* -Dateien an unterschiedlichen Speicherorten durchführen möchten, empfiehlt es sich, MS-Bereitstellung. exe direkt zu verwenden.
- Wenn Sie mehrere Befehlszeilenargumente von "msbereitstellungs. exe" überschreiben möchten, empfiehlt es sich, "msbereitstellungs. exe" direkt zu verwenden.

Wenn Sie msbereitstellungs. exe verwenden, müssen Sie drei wichtige Informationen bereitstellen:

- Ein **– Source** -Parameter, der angibt, woher die Daten stammen.
- Ein **– dest** -Parameter, der angibt, wohin die Daten gehen.
- Ein **– Verb** -Parameter, der den [Vorgang](https://technet.microsoft.com/library/dd568989(WS.10).aspx) angibt, den Sie ausführen möchten.

Die Verarbeitung von Quell-und Zieldaten durch msbereitstellungs. exe basiert auf [Web deploy Anbietern](https://technet.microsoft.com/library/dd569040(WS.10).aspx) . Web deploy umfasst zahlreiche Anbieter, die den Bereich der Anwendungen und Datenquellen darstellen, mit&#x2014;denen Sie z. b. arbeiten kann. es gibt Anbieter für SQL Server Datenbanken, IIS-Webserver, Zertifikate, GAC-Assemblys (Global Assembly Cache), verschiedene Konfigurationsdateien und viele andere Arten von Daten. Sowohl der **– Source** -Parameter als auch der **– dest** -Parameter müssen einen Anbieter im Format **– Source**: [*providerName*] = [*Location*] angeben. Wenn Sie ein Webpaket auf einer IIS-Website bereitstellen, sollten Sie die folgenden Werte verwenden:

- Der **– Source** -Anbieter ist immer [Package](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Beispiel:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Der **– dest** -Anbieter ist immer [automatisch](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Zum Beispiel:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- Das **–-Verb** ist immer **Sync**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Außerdem müssen Sie verschiedene andere [anbieterspezifische Einstellungen](https://technet.microsoft.com/library/dd569001(WS.10).aspx) und allgemeine [Vorgangs Einstellungen](https://technet.microsoft.com/library/dd569089(WS.10).aspx)angeben. Angenommen, Sie möchten die "ContactManager. MVC"-Webanwendung in einer Stagingumgebung bereitstellen. Die Bereitstellung wird auf den Web deploy-Handler abzielen und muss die Standard Authentifizierung verwenden. Zum Bereitstellen der-Webanwendung müssen Sie die nächsten Schritte ausführen.

**So stellen Sie eine Webanwendung mit msbereitstellungs. exe bereit**

1. Erstellen und Verpacken Sie das Webanwendungs Projekt, wie unter [Erstellen und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md)beschrieben.
2. Ändern Sie die Datei " *ContactManager. MVC. SetParameters. XML* " so, dass Sie die richtigen Parameterwerte für Ihre Stagingumgebung enthält, wie unter [Konfigurieren von Parametern für die Webpaket Bereitstellung](configuring-parameters-for-web-package-deployment.md)beschrieben.
3. Öffnen Sie ein Eingabe Aufforderungs Fenster, und navigieren Sie zum Speicherort von "msbereitstellungs. exe". Dies befindet sich in der Regel unter%ProgramFiles%\iis\microsoft Web deploy V2\msdeploy.exe.
4. Geben Sie diesen Befehl ein, und drücken Sie dann die EINGABETASTE (ignorieren Sie die Zeilenumbrüche):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

In diesem Beispiel:

- Der **– Source** -Parameter gibt den **Paket** Anbieter an und gibt den Speicherort des Webpakets an.
- Der **– dest** -Parameter gibt den **Auto** -Anbieter an. Die Einstellung **Computername** stellt die Dienst-URL des Web deploy Handlers auf dem Zielserver bereit. Die Einstellung **authType** gibt an, dass Sie die Standard Authentifizierung verwenden möchten. Daher müssen Sie einen **Benutzernamen** und ein **Kennwort**angeben. Schließlich gibt die Einstellung **"includeacls =" false "** an, dass Sie die Zugriffs Steuerungs Listen (ACLs) der Dateien in der Quellweb Anwendung nicht auf den Zielserver kopieren möchten.
- Das **– Verb: Sync** -Argument gibt an, dass Sie den Quell Inhalt auf dem Zielserver replizieren möchten.
- Die **– disableLink** -Argumente geben an, dass Sie keine Anwendungs Pools, die virtuelle Verzeichnis Konfiguration oder Secure Sockets Layer (SSL)-Zertifikate auf dem Zielserver replizieren möchten. Weitere Informationen finden Sie unter [Web deploy Link Erweiterungen](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Der **– setparamfile** -Parameter gibt den Speicherort der Datei " *SetParameters. XML* " an.
- Der Schalter **– zuwuntrusted** gibt an, dass Web deploy SSL-Zertifikate akzeptieren soll, die nicht von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurden. Wenn Sie für den Web deploy Handler bereitstellen und ein selbst signiertes Zertifikat zum Sichern der Dienst-URL verwendet haben, müssen Sie diesen Switch einschließen.

## <a name="automating-web-package-deployment"></a>Automatisieren der Webpaket Bereitstellung

In vielen Unternehmens Szenarien sollten Sie Ihre Webpakete als Teil einer größeren oder automatisierten Bereitstellung bereitstellen. Unabhängig davon, ob Sie Ihre Webpakete bereitstellen möchten, indem Sie die *. cmd* -Datei ausführen oder msbereitstellungs. exe direkt verwenden, können Sie die Befehle parametrisieren und Sie von einem Ziel in einer Microsoft-Build-Engine (MSBuild)-Projektdatei aus abrufen.

Sehen Sie sich in der Beispiellösung "Contact Manager" das **publishwebpackages** -Ziel in der Datei " *Publish. proj* " an. Dieses Ziel wird für jede bereitgestellte *. cmd* -Datei, die durch eine Elementliste mit dem Namen **publishpackages**identifiziert wird, einmal ausgeführt. Das Ziel verwendet Eigenschaften und Element Metadaten, um einen vollständigen Satz von Argument Werten für jede *. cmd* -Datei zu erstellen, und verwendet dann die **exec** -Aufgabe zum Ausführen des Befehls.

[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]

> [!NOTE]
> Einen umfassenderen Überblick über das Projektdatei Modell in der Beispiellösung und eine Einführung in benutzerdefinierte Projektdateien im Allgemeinen finden Sie Untergrund Legendes zur [Projektdatei](understanding-the-project-file.md) und Grundlegendes [zum Buildprozess](understanding-the-build-process.md).

## <a name="endpoint-considerations"></a>Endpunkt Überlegungen

Unabhängig davon, ob Sie das Webpaket bereitstellen, indem Sie die Datei " *. Deployment. cmd* " ausführen oder "msdeployment. exe" direkt verwenden, müssen Sie einen Computernamen oder einen Dienst Endpunkt für Ihre Bereitstellung angeben.

Wenn der Zielweb Server mithilfe des Web deploy Remote-Agent-Diensts für die Bereitstellung konfiguriert ist, geben Sie die Ziel Dienst-URL als Ziel an.

[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]

Alternativ dazu können Sie den Servernamen allein als Ziel angeben und Web deploy die URL des Remote-Agent-Dienstanbieter ableiten.

[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]

Wenn der Zielweb Server mithilfe des Web deploy Handlers für die Bereitstellung konfiguriert ist, müssen Sie die Endpunkt Adresse des IIS-Webverwaltungsdiensts (WmSvc) als Ziel angeben. Standardmäßig hat dies folgende Form:

[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]

Sie können einen beliebigen Endpunkt als Ziel verwenden, indem Sie entweder die *. cmd* -Datei oder die Datei "msbereitstellungs. exe" direkt verwenden. Wenn Sie jedoch die Bereitstellung für den Web deploy Handler als Benutzer ohne Administratorrechte durchzuführen möchten, wie unter [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Web deploy Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)beschrieben, müssen Sie der dienstend Punkt Adresse eine Abfrage Zeichenfolge hinzufügen.

[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]

Dies liegt daran, dass der Benutzer ohne Administratorrechte keinen Zugriff auf die IIS-Serverebene hat. Er hat nur Zugriff auf eine bestimmte IIS-Website. Zum Zeitpunkt des Schreibens konnten Sie aufgrund eines Fehlers in der Web Publishing Pipeline (WPP) die Datei ". Bereitstellungs- *. cmd* " nicht mit einer Endpunkt Adresse ausführen, die eine Abfrage Zeichenfolge enthält. In diesem Szenario müssen Sie das Webpaket direkt mithilfe von "msbereitstellungs. exe" bereitstellen.

> [!NOTE]
> Weitere Informationen zum Web deploy Remote-Agent-Dienst und zum Web deploy Handler finden [Sie unter Auswählen des richtigen Ansatzes für die Webbereitstellung](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Anleitungen zum Konfigurieren der Umgebungs spezifischen Projektdateien für die Bereitstellung auf diesen Endpunkten finden Sie unter [Konfigurieren von Bereitstellungs Eigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="authentication-considerations"></a>Überlegungen zur Authentifizierung

Unabhängig davon, ob Sie das Webpaket bereitstellen, indem Sie die *. cmd* -Datei oder msbereitstellungs. exe direkt verwenden, müssen Sie einen Authentifizierungstyp angeben. Web deploy akzeptiert zwei mögliche Werte: **NTLM** oder **Basic**. Wenn Sie die Standard Authentifizierung angeben, müssen Sie auch einen Benutzernamen und ein Kennwort angeben. Es gibt verschiedene Faktoren, die Sie beachten müssen, wenn Sie einen Authentifizierungstyp auswählen:

- Wenn Sie für den Web deploy Remote-Agent-Dienst bereitstellen, müssen Sie die NTLM-Authentifizierung verwenden. Der Remote-Agent-Dienst akzeptiert keine Standard Anmelde Informationen für die Authentifizierung.
- Wenn Sie die Bereitstellung für den Web deploy Handler ausführen, können Sie entweder NTLM oder Standard Authentifizierung verwenden. Die Standardeinstellung ist Standard Authentifizierung. Obwohl die Standard Authentifizierung von Benutzernamen und Kenn Wörtern abhängig ist, die im Klartext übertragen werden, werden Ihre Anmelde Informationen geschützt, da der Web deploy Handler immer SSL-Verschlüsselung verwendet.
- Wenn das Webpaket eine Datenbank enthält und es sich bei dem Webserver und dem Datenbankserver um separate Computer handelt, können Sie die Datenbank aufgrund der [NTLM-Einschränkung "Double-Hop"](https://go.microsoft.com/?linkid=9805120)nicht mithilfe der NTLM-Authentifizierung bereitstellen. Sie müssen entweder SQL Server Anmelde Informationen in der Verbindungs Zeichenfolge der Bereitstellung verwenden oder Anmelde Informationen für die Standard Authentifizierung angeben, um Web Deploy. Dieses Problem wird unter Bereitstellen [von Mitgliedschafts Datenbanken für Unternehmensumgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)ausführlicher beschrieben.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie Sie ein Webpaket bereitstellen können, indem Sie entweder die Datei " *. cmd* " oder "msbereitstellungs. exe" direkt verwenden. Es wird erläutert, wann jeder Ansatz geeignet ist, und es wurde beschrieben, wie Sie einen Bereitstellungs Befehl im Rahmen eines größeren einstufigen oder automatisierten Buildprozesses parametrisieren und ausführen können.

## <a name="further-reading"></a>Weitere nützliche Informationen

Anleitungen zum Erstellen und Parametrisieren eines Webbereitstellungs Pakets finden Sie unter Erstellen [und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md) und [Konfigurieren von Parametern für die Webpaket Bereitstellung](configuring-parameters-for-web-package-deployment.md). Anleitungen zum Erstellen und Bereitstellen von Webpaketen aus einer Team Foundation Server-Instanz (TFS) finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Informationen zum Anpassen und Beheben von Problemen beim Bereitstellungs Prozess finden Sie unter [Ausschließen von Dateien und Ordnern aus der Bereitstellung](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Zurück](configuring-parameters-for-web-package-deployment.md)
> [Weiter](deploying-database-projects.md)
