---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Konfigurieren von Parametern für die Webpaket Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Parameterwerte wie Internetinformationsdienste (IIS)-Webanwendungs Namen, Verbindungs Zeichenfolgen und Dienst Endpunkte festgelegt werden,...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: f04ace98d81a33053b10cab7e40dbd75a6c0992c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438399"
---
# <a name="configuring-parameters-for-web-package-deployment"></a>Konfigurieren von Parametern für die Bereitstellung von Webpaketen

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Parameterwerte, wie Internetinformationsdienste (IIS)-Webanwendungs Namen, Verbindungs Zeichenfolgen und Dienst Endpunkte, beim Bereitstellen eines Webpakets auf einem IIS-Remote Webserver festgelegt werden.

Wenn Sie ein Webanwendungs Projekt erstellen, generiert der Build-und Verpackungsprozess drei Schlüsseldateien:

- Eine *[Projektname] zip* -Datei. Dies ist das Webbereitstellungs Paket für das Webanwendungs Projekt. Dieses Paket enthält alle Assemblys, Dateien, Daten Bank Skripts und Ressourcen, die zum erneuten Erstellen der Webanwendung auf einem IIS-Remote Webserver erforderlich sind.
- Eine *[Projektname]. cmd* -Datei. Diese enthält einen Satz parametrisierter Web deploy (msdeployment. exe)-Befehle, mit denen das Webbereitstellungs Paket auf einem IIS-Remote Webserver veröffentlicht wird.
- Ein *[Projektname]. Datei "SetParameters. XML* ". Dadurch wird ein Satz von Parameterwerten für den Befehl "msbereitstellungs. exe" bereitgestellt. Sie können die Werte in dieser Datei aktualisieren und an Web deploy als Befehlszeilenparameter übergeben, wenn Sie das Webpaket bereitstellen.

> [!NOTE]
> Weitere Informationen zum Erstellungs-und Verpackungsprozess finden Sie unter [Erstellen und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md).

Die Datei " *SetParameters. XML* " wird dynamisch aus der Webanwendungs Projektdatei und allen Konfigurationsdateien in Ihrem Projekt generiert. Wenn Sie Ihr Projekt erstellen und packen, erkennt die Webpublishing Pipeline (WPP) automatisch viele der Variablen, die sich wahrscheinlich zwischen Bereitstellungs Umgebungen ändern, wie z. b. die IIS-Ziel Webanwendung und beliebige Daten bankverbindungs Zeichenfolgen. Diese Werte werden automatisch im Webbereitstellungs Paket parametrisiert und der Datei " *SetParameters. XML* " hinzugefügt. Wenn Sie z. b. der Datei " *Web. config* " in Ihrem Webanwendungs Projekt eine Verbindungs Zeichenfolge hinzufügen, wird diese Änderung vom Buildprozess erkannt, und der Datei " *SetParameters. XML* " wird ein Eintrag entsprechend hinzugefügt.

In vielen Fällen ist diese automatische Parametrisierung ausreichend. Wenn die Benutzer jedoch andere Einstellungen zwischen Bereitstellungs Umgebungen, z. b. Anwendungseinstellungen oder Dienst Endpunkt-URLs, verändern müssen, müssen Sie die WPP anweisen, diese Werte im Bereitstellungs Paket zu parametrisieren und der Datei " *SetParameters. XML* " entsprechende Einträge hinzuzufügen. In den folgenden Abschnitten wird erläutert, wie dies zu tun ist.

### <a name="automatic-parameterization"></a>Automatische Parametrisierung

Wenn Sie eine Webanwendung erstellen und packen, werden diese vom WPP automatisch parametrisiert:

- Der Ziel-IIS-Webanwendungs Pfad und-Name.
- Alle Verbindungs Zeichenfolgen in der Datei " *Web. config* ".
- Verbindungs Zeichenfolgen für Datenbanken, die Sie auf den Eigenschaften Seiten des Projekts der Registerkarte " **Paket/SQL veröffentlichen** " hinzufügen.

Wenn Sie z. b. die Beispiellösung [Contact Manager](the-contact-manager-solution.md) erstellen und Verpacken, ohne den Parametrisierung-Prozess zu berühren, generiert WPP diese Datei *ContactManager. MVC. SetParameters. XML* :

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]

In diesem Fall:

- Der Name-Parameter der **IIS-Webanwendung** ist der IIS-Pfad, in dem Sie die Webanwendung bereitstellen möchten. Der Standardwert wird auf den Eigenschaften Seiten des Projekts auf der **Webseite zum Packen/veröffentlichen** übernommen.
- Der **ApplicationServices-Web. config-Verbindungs Zeichenfolgen-** Parameter wurde aus einem **connectionStrings/Add** -Element in der Datei " *Web. config* " generiert. Sie stellt die Verbindungs Zeichenfolge dar, die von der Anwendung zum Kontaktieren der Mitgliedschafts Datenbank verwendet werden soll. Der Wert, den Sie hier angeben, wird in die bereitgestellte *Web. config* -Datei ersetzt. Der Standardwert wird aus der *Web. config* -Datei der vorab Bereitstellung entnommen.

Die WPP parametrisiert auch diese Eigenschaften im Bereitstellungs Paket, das Sie generiert. Sie können Werte für diese Eigenschaften angeben, wenn Sie das Bereitstellungs Paket installieren. Wenn Sie das Paket manuell über den IIS-Manager installieren (wie unter [Manuelles Installieren von Webpaketen](manually-installing-web-packages.md)beschrieben), werden Sie vom Installations-Assistenten aufgefordert, Werte für Parameter anzugeben. Wenn Sie das Paket mithilfe der Datei *.* Bereitstellungs Datei Remote installieren, wie unter Bereitstellen von [Webpaketen](deploying-web-packages.md)beschrieben, werden Web deploy diese Datei " *SetParameters. XML* " suchen, um die Parameterwerte bereitzustellen. Sie können die Werte in der Datei " *SetParameters. XML* " manuell bearbeiten, oder Sie können die Datei im Rahmen eines automatisierten Build-und Bereitstellungs Prozesses anpassen. Dieser Vorgang wird weiter unten in diesem Thema ausführlicher beschrieben.

### <a name="custom-parameterization"></a>Benutzerdefinierte Parametrisierung

In komplexeren Bereitstellungs Szenarien sollten Sie häufig zusätzliche Eigenschaften parametrisieren, bevor Sie das Projekt bereitstellen. Im Allgemeinen sollten Sie alle Eigenschaften und Einstellungen parametrisieren, die sich Zwischenziel Umgebungen unterscheiden. Folgende Aktionen sind möglich:

- Dienst Endpunkte in der Datei " *Web. config* ".
- Anwendungseinstellungen in der Datei " *Web. config* ".
- Alle anderen deklarativen Eigenschaften, die Benutzer zur Angabe auffordern möchten.

Die einfachste Möglichkeit, diese Eigenschaften zu parametrisieren, besteht darin, dem Stamm Ordner des Webanwendungs Projekts die Datei " *Parameters. XML* " hinzuzufügen. Beispielsweise enthält das ContactManager. MVC-Projekt in der Contact Manager-Lösung eine " *Parameters. XML* "-Datei im Stamm Ordner.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Wenn Sie diese Datei öffnen, werden Sie feststellen, dass Sie einen einzelnen **Parameter** Eintrag enthält. Der Eintrag verwendet eine XPath-Abfrage (XML Path Language), um die Endpunkt-URL des contactservice-Windows Communication Foundation (WCF)-Diensts in der Datei " *Web. config* " zu suchen und zu parametrisieren.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]

Zusätzlich zum parametrisierten der Endpunkt-URL im Bereitstellungs Paket fügt WPP auch einen entsprechenden Eintrag in die Datei " *SetParameters. XML* " ein, die zusammen mit dem Bereitstellungs Paket generiert wird.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]

Wenn Sie das Bereitstellungs Paket manuell installieren, werden Sie von IIS-Manager zur Eingabe der dienstend Punkt Adresse neben den Eigenschaften aufgefordert, die automatisch parametrisiert wurden. Wenn Sie das Bereitstellungs Paket durch Ausführen der Datei " *. Deployment. cmd* " installieren, können Sie die Datei " *SetParameters. XML* " Bearbeiten, um einen Wert für die Adresse des Dienst Endpunkts sowie Werte für die Eigenschaften anzugeben, die automatisch parametrisiert wurden.

Ausführliche Informationen zum Erstellen der Datei " *Parameters. XML* " finden Sie unter Gewusst [wie: Verwenden von Parametern zum Konfigurieren von Bereitstellungs Einstellungen bei der Installation eines Pakets](https://msdn.microsoft.com/library/ff398068.aspx). Die Vorgehensweise **zum Verwenden der Bereitstellungs Parameter für die Einstellungen der Datei "Web. config** " enthält Schritt-für-Schritt-Anleitungen.

## <a name="modifying-the-setparametersxml-file"></a>Ändern der Datei "SetParameters. xml"

&#x2014;Wenn Sie das Webanwendungs Paket manuell bereitstellen möchten, indem Sie entweder die Datei ". Deployment *. cmd* " ausführen oder "msdeployment.&#x2014;exe" über die Befehlszeile ausführen, müssen Sie die Datei " *SetParameters. XML* " vor der Bereitstellung nicht manuell bearbeiten. Wenn Sie jedoch an einer Unternehmenslösung arbeiten, müssen Sie möglicherweise ein Webanwendungs Paket als Teil eines größeren, automatisierten Build-und Bereitstellungs Prozesses bereitstellen. In diesem Szenario benötigen Sie den Microsoft-Build-Engine (MSBuild), um die Datei " *SetParameters. XML* " für Sie zu ändern. Hierfür können Sie die **XmlPoke** -Aufgabe von MSBuild verwenden.

Dieses Verfahren wird in der [Beispiellösung Contact Manager](the-contact-manager-solution.md) veranschaulicht. Die folgenden Codebeispiele wurden bearbeitet, um nur die für dieses Beispiel relevanten Details anzuzeigen.

> [!NOTE]
> Einen umfassenderen Überblick über das Projektdatei Modell in der Beispiellösung und eine Einführung in benutzerdefinierte Projektdateien im Allgemeinen finden Sie Untergrund Legendes zur [Projektdatei](understanding-the-project-file.md) und Grundlegendes [zum Buildprozess](understanding-the-build-process.md).

Zuerst werden die zu berücksichtigenden Parameterwerte als Eigenschaften in der Umgebungs spezifischen Projektdatei (z *. b. env-dev. proj*) definiert.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]

> [!NOTE]
> Anleitungen zum Anpassen der Umgebungs spezifischen Projektdateien für Ihre eigenen Serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungs Eigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Als nächstes importiert die Datei *Publish. proj* diese Eigenschaften. Da jede Datei " *SetParameters. XML* " einer *. cmd* -Datei zugeordnet ist und wir letztendlich möchten, dass die Projektdatei jede. Bereitstellung. *cmd* -Datei aufruft, erstellt die Projektdatei ein MSBuild- *Element* für jede *.* cmd-Datei und definiert die Eigenschaften, die von Interesse sind, als *Element Metadaten*.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]

In diesem Fall:

- Der **parametersxml** -Metadatenwert gibt den Speicherort der Datei " *SetParameters. XML* " an.
- Der Wert **iiswebappname** ist der IIS-Pfad, in dem Sie die Webanwendung bereitstellen möchten.
- Der Wert für " **Membership shipdbconnectionstring** " ist die Verbindungs Zeichenfolge für die Mitgliedschafts Datenbank, und der Wert für " **Membership shipdbconnectionname** " ist das **namens** Attribut des entsprechenden Parameters in der Datei " *SetParameters. XML* ".
- Der **serviceendpointvalue** -Wert ist die Endpunkt Adresse für den WCF-Dienst auf dem Zielserver, und der **serviceendpointparamname** -Wert ist das Name-Attribut des entsprechenden Parameters in der Datei " *SetParameters. XML* ".

Schließlich verwendet das **publishwebpackages** -Ziel in der Datei *Publish. proj* die **XmlPoke** -Aufgabe, um diese Werte in der Datei " *SetParameters. XML* " zu ändern.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]

Sie werden feststellen, dass jede **XmlPoke** -Aufgabe vier Attributwerte angibt:

- Das **XmlInputPath** -Attribut teilt der Aufgabe mit, wo sich die Datei befindet, die Sie ändern möchten.
- Das **Query** -Attribut ist eine XPath-Abfrage, die den XML-Knoten identifiziert, den Sie ändern möchten.
- Das **value** -Attribut ist der neue Wert, den Sie in den ausgewählten XML-Knoten einfügen möchten.
- Das **Condition** -Attribut ist die Kriterien, auf denen die Aufgabe ausgeführt oder nicht ausgeführt werden soll. In diesen Fällen stellt die Bedingung sicher, dass Sie nicht versuchen, einen NULL-Wert oder einen leeren Wert in die Datei " *SetParameters. XML* " einzufügen.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wurde die Rolle der Datei " *SetParameters. XML* " beschrieben und erläutert, wie Sie beim Erstellen eines Webanwendungs Projekts generiert wird. Es wurde erläutert, wie Sie zusätzliche Einstellungen parametrisieren können, indem Sie dem Projekt die Datei " *Parameters. XML* " hinzufügen. Außerdem wird beschrieben, wie Sie die Datei " *SetParameters. XML* " im Rahmen eines größeren, automatisierten Buildprozesses ändern können, indem Sie die **XmlPoke** -Aufgabe in den Projektdateien verwenden.

Im nächsten Thema, Bereitstellen von [Webpaketen](deploying-web-packages.md), wird beschrieben, wie Sie ein Webpaket bereitstellen können, indem Sie entweder die Datei " *. cmd* " oder die Befehle "msbereitstellungs. exe" direkt verwenden. In beiden Fällen können Sie die Datei " *SetParameters. XML* " als Bereitstellungs Parameter angeben.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zum Erstellen von Webpaketen finden Sie unter Erstellen [und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md). Eine Anleitung zur eigentlichen Bereitstellung eines Webpakets finden Sie unter Bereitstellen von [Webpaketen](deploying-web-packages.md). Eine Schritt-für-Schritt-Anleitung zum Erstellen der Datei " *Parameters. XML* " finden Sie unter Gewusst [wie: Verwenden von Parametern zum Konfigurieren von Bereitstellungs Einstellungen bei der Installation eines Pakets](https://msdn.microsoft.com/library/ff398068.aspx).

Weitere allgemeine Informationen zur Parametrisierung in Web deploy finden Sie unter [Web deploy Parametrisierung in Action](https://go.microsoft.com/?linkid=9805119) (Blogbeitrag).

> [!div class="step-by-step"]
> [Zurück](building-and-packaging-web-application-projects.md)
> [Weiter](deploying-web-packages.md)
