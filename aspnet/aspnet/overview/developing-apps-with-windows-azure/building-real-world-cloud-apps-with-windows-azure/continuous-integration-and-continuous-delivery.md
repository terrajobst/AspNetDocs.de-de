---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Continuous Integration und Continuous Delivery (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Das e-Book zur Entwicklung realer Cloud-apps mit Azure basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Vorgehensweisen erläutert, für die er...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 52c710053feca7872aa6fcc93c99bce90359f8fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585880"
---
# <a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Continuous Integration und Continuous Delivery (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Verfahren erläutert, die Ihnen bei der Entwicklung von Web-Apps für die Cloud helfen können. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Bei den ersten beiden empfohlenen Entwicklungsprozess Mustern wurden alles und die [Quell](source-control.md)Code Verwaltung [automatisiert](automate-everything.md) , und das dritte Prozess Muster kombiniert Sie. Continuous Integration (CI) bedeutet, dass ein Build automatisch ausgelöst wird, wenn ein Entwickler Code in das Quellrepository eincheckt. Continuous Delivery (CD) führt dies einen Schritt weiter aus: nach erfolgreicher Erstellung eines Builds und automatisierter Komponententests stellen Sie die Anwendung automatisch in einer Umgebung bereit, in der Sie ausführlichere Tests durchführen können.

Mit der Cloud können Sie die Kosten für die Wartung einer Testumgebung minimieren, da Sie nur für die Umgebungs Ressourcen bezahlen, sofern Sie Sie verwenden. Der CD-Prozess kann die Testumgebung bei Bedarf einrichten, und Sie können die Umgebung herunterskalieren, wenn Sie die Tests abgeschlossen haben.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Continuous Integration-und Continuous Delivery-Workflow

Im Allgemeinen wird empfohlen, dass Sie Continuous Delivery zu ihren Entwicklungs-und Stagingumgebungen verwenden. Die meisten Teams, auch bei Microsoft, benötigen einen manuellen Überprüfungs-und Genehmigungsprozess für die Produktions Bereitstellung. Bei einer Produktions Bereitstellung sollten Sie sicherstellen, dass Sie ausgeführt wird, wenn wichtige Personen im Entwicklungsteam zur Unterstützung oder während der Zeiten mit geringem Datenverkehr verfügbar sind. Es ist jedoch nichts zu verhindern, dass Sie Ihre Entwicklungs-und Testumgebungen vollständig automatisieren, damit alle Entwickler eine Änderung einchecken und eine Umgebung für Akzeptanz Tests eingerichtet ist.

Im folgenden Diagramm [eines Microsoft Patterns and Practices-e-Books über Continuous Delivery](https://aka.ms/ReleasePipeline) wird ein typischer Workflow veranschaulicht. Klicken Sie auf das Bild, um die vollständige Größe im ursprünglichen Kontext anzuzeigen.

[Continuous Delivery-Workflow ![](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Wie die Cloud kostengünstige CI und CD ermöglicht

Die Automatisierung dieser Prozesse in Azure ist einfach. Da Sie alles in der Cloud ausführen, müssen Sie keine Server für Ihre Builds oder Ihre Testumgebungen erwerben und verwalten. Und Sie müssen nicht warten, bis ein Server verfügbar ist, auf dem Sie die Tests durchführen können. Mit jedem Build, den Sie ausführen, können Sie eine Testumgebung in Azure mithilfe Ihres Automation-Skripts einrichten, Akzeptanz Tests oder ausführlichere Tests dafür ausführen und dann, wenn Sie fertig sind, diese einfach abfangen. Und wenn Sie diesen Server nur für 2 Stunden oder 8 Stunden oder täglich ausführen, ist das Geld, das Sie hierfür bezahlen müssen, minimal, da Sie nur für die Zeit bezahlen, in der ein Computer tatsächlich ausgeführt wird. Beispielsweise kostet die Umgebung, die für die Behebung der IT-Anwendung erforderlich ist, im Grunde etwa 1 Cent pro Stunde, wenn Sie eine Ebene auf der Free-Ebene nach oben wechseln. Wenn Sie im Laufe eines Monats die Umgebung nur einmal pro Stunde ausgeführt haben, würde die Testumgebung wahrscheinlich weniger als ein Latte Macchiato kosten, den Sie bei Starbucks kaufen.

## <a name="azure-devops-services"></a>Azure DevOps Services 

Azure DevOps Services bietet eine Reihe von Features, die Sie bei der Anwendungsentwicklung von der Planung bis zur Bereitstellung unterstützen.

- Sie unterstützt sowohl die git-(verteilte) als auch die tfvc-Quell Code Verwaltung (zentral).
- Es bietet einen elastischen Builddienst, d. h., er erstellt dynamisch Buildserver, wenn Sie benötigt werden, und führt Sie aus, wenn Sie fertig sind. Sie können einen Buildvorgang automatisch starten, wenn jemand Änderungen am Quell Code eincheckt, und Sie müssen nicht für Ihre eigenen Buildserver bezahlen, die sich in den meisten Fällen im Leerlauf befinden. Der Builddienst ist kostenlos, solange Sie eine bestimmte Anzahl von Builds nicht überschreiten. Wenn Sie eine große Menge an Builds durchführen möchten, können Sie für reservierte Buildserver etwas zusätzliches bezahlen.
- Sie unterstützt Continuous Delivery in Azure.
- Es unterstützt automatisierte Auslastungs Tests. Auslastungs Tests sind für eine Cloud-App wichtig, werden jedoch oft vernachlässigt, bis Sie zu spät ist. Auslastungs Tests simulieren die Nutzung einer APP durch Tausende von Benutzern, sodass Sie Engpässe finden und den Durchsatz verbessern können – bevor Sie die app in der Produktion veröffentlichen.
- Es unterstützt die Zusammenarbeit im Team Raum, was die Echtzeitkommunikation und Zusammenarbeit für kleine Agile-Teams ermöglicht.
- Sie unterstützt das Agile-Projektmanagement.

Weitere Informationen zu den Continuous Integration-und Übermittlungs Features von Azure DevOps Services finden Sie in [der Azure devops-Dokumentation](/azure/devops/index).

Wenn Sie nach einer Lösung für das Projektmanagement, die Team Zusammenarbeit und die Quell Code Verwaltung suchen, sehen Sie sich Azure DevOps Services an. Melden Sie sich bei [Azure DevOps Services](https://dev.azure.com/)an.

## <a name="summary"></a>Summary

In den ersten drei cloudentwicklungstmustern wurde erläutert, wie Sie einen wiederholbaren, zuverlässigen, vorhersagbaren Entwicklungsprozess mit geringer Zeit Umbauzeit implementieren. Im [nächsten Kapitel](web-development-best-practices.md) beginnen wir mit der Betrachtung von Architektur-und Codierungs Mustern.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie unter Bereitstellen [einer Web-App in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Weitere Informationen finden Sie auch in den folgenden Ressourcen:

- [Erstellung einer releasepipeline mit Team Foundation Server 2012](https://aka.ms/ReleasePipeline). E-Book, praktische Übungseinheiten und Beispielcode von Microsoft Patterns and Practices bietet eine ausführliche Einführung in die Continuous Delivery. Deckt die Verwendung von Visual Studio Lab Management und Visual Studio Release Management ab.
- [Alm Rangers devops-Tools und Anleitungen](https://aka.ms/vsarsolutions/). Die Alm Ranger haben die Beispiel-Begleit Lösung für devops Workbench vorgestellt und praktische Anleitungen für die Zusammenarbeit mit den Mustern &amp; Practices-Buch *Erstellung einer releasepipeline mit TFS 2012*, als hervorragend für das Erlernen der Konzepte von devops &amp; Release Management für TFS 2012 und zum Starten der Reifen. In der Anleitung wird gezeigt, wie ein Mal erstellt und in mehreren Umgebungen bereitgestellt wird.
- [Tests für Continuous Delivery mit Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). E-Book von Microsoft Patterns and Practices, erläutert, wie Sie automatisierte Tests mit Continuous Delivery integrieren.
- [Windowsazuredeploymenttracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Quellcode für ein Tool, das zum Erfassen eines Builds aus TFS (basierend auf einer Bezeichnung) konzipiert ist, ihn erstellen, Verpacken, einem Benutzer in der devops-Rolle ermöglicht, bestimmte Aspekte dieses zu konfigurieren und ihn per Push in Azure zu übernehmen. Das Tool verfolgt den Bereitstellungs Prozess, um Vorgänge für ein Rollback auf eine zuvor bereitgestellte Version zu aktivieren. Das Tool verfügt über keine externen Abhängigkeiten und kann eigenständig mithilfe von TFS-APIs und dem Azure SDK funktionsfähig sein.
- [Continuous Delivery: zuverlässige Software Releases durch Build-, Test-und Bereitstellungs Automatisierung](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Buch von Jez bescheiden.
- [Release! entwerfen und Bereitstellen von Produktions bereiter Software](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Buch von Michael T. Nygard.

> [!div class="step-by-step"]
> [Zurück](source-control.md)
> [Weiter](web-development-best-practices.md)
