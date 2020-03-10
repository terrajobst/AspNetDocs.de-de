---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Bereitstellen von Mitgliedschafts Datenbanken in Unternehmensumgebungen | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema werden die wichtigsten Überlegungen und Probleme erläutert, die Sie bei der Bereitstellung von ASP.NET-Datenbanken (Common...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 50f49af502b75aa5ad52756a76a5e7340aca53f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422157"
---
# <a name="deploying-membership-databases-to-enterprise-environments"></a>Bereitstellen von Datenbankrollenmitgliedschaften in Unternehmensumgebungen

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema werden die wichtigsten Überlegungen und Probleme erläutert, die Sie bei der Bereitstellung von ASP.NET-Anwendungs Dienst Datenbanken (häufig als Mitgliedschafts Datenbanken bezeichnet) in Test-, Staging-oder Produktionsumgebungen überwinden müssen. Außerdem werden die Ansätze beschrieben, die Sie verwenden können, um diese Herausforderungen zu erfüllen.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Welche Probleme bestehen beim Bereitstellen einer Mitgliedschafts Datenbank?

In den meisten Fällen müssen Sie, wenn Sie eine Bereitstellungs Strategie für eine Datenbank entwickeln, die Daten, die Sie bereitstellen möchten, als erstes in Erwägung gezogen werden. In einer Entwicklungs-oder Testumgebung können Sie Benutzerkontodaten bereitstellen, um schnelle und einfache Tests zu ermöglichen. In einer Staging-oder Produktionsumgebung ist es sehr unwahrscheinlich, dass Sie Benutzerkontodaten bereitstellen möchten.

Leider stellen ASP.net-Mitgliedschafts Datenbanken einige besondere Herausforderungen dar, die diese Entscheidung wesentlich komplexer machen:

- Bei einer reinen Schema Bereitstellung wird die Mitgliedschafts Datenbank in einem nicht funktionsfähigen Status belassen. Dies liegt daran, dass die Mitgliedschafts Datenbank einige Konfigurationsdaten (in der Tabelle **ASPNET\_schemaversions** ) enthält, die die Datenbank benötigt, damit Sie funktioniert. Wenn Sie also eine reine Schema Bereitstellung der Mitgliedschafts Datenbank durchführen, um Benutzerkontodaten auszuschließen, müssen Sie ein Skript nach der Bereitstellung ausführen, um die wichtigen Konfigurationsdaten hinzuzufügen.
- Abhängig davon, wie die Mitgliedschafts Datenbank konfiguriert ist, kann der Mitgliedschafts Anbieter den Computer Schlüssel verwenden, um Kenn Wörter zu verschlüsseln und in der Datenbank zu speichern. In diesem Fall können alle Benutzerkontodaten, die Sie mit der Datenbank bereitstellen, auf dem Zielserver unbrauchbar werden. Aus diesem Grund wird das Bereitstellen von Benutzerkontodaten nicht unterstützt.

## <a name="choosing-a-membership-database-strategy"></a>Auswählen einer Strategie für die Mitgliedschafts Datenbank

Verwenden Sie diese Richtlinien, wenn Sie auswählen, wie eine Mitgliedschafts Datenbank in einer Unternehmensserver Umgebung bereitgestellt werden soll:

- Stellen Sie, sofern möglich, keine Mitgliedschafts Datenbanken bereit. Erstellen Sie stattdessen die Mitgliedschafts Datenbank manuell auf dem Ziel Datenbankserver. Wenn Sie Ihr Mitgliedschafts Datenbankschema nicht angepasst haben, können Sie einfach eine neue Datei am Ziel mit dem [ASP.NET SQL Server Registration-Tool (ASPNET\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)erstellen.
- Wenn Sie über keine Option verfügen, aber zum Beispiel eine&#x2014;Mitgliedschafts Datenbank bereitstellen möchten, sollten Sie, wenn Sie Umfang&#x2014;reiche Änderungen am Datenbankschema vorgenommen haben, eine reine Schema Bereitstellung der Mitgliedschafts Datenbank durchführen, um Benutzerkontodaten auszuschließen und dann ein Skript nach der Bereitstellung auszuführen, um alle erforderlichen Konfigurationsdaten hinzuzufügen. Eine umfassende Anleitung zu diesen Vorgehensweisen finden Sie unter Gewusst [wie: Bereitstellen der ASP.net-Mitgliedschafts Datenbank ohne Angabe von Benutzerkonten](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Beachten Sie, dass *das Schema Ihrer Mitgliedschafts Datenbank wahrscheinlich relativ statisch ist*. Auch wenn Sie die Mitgliedschafts Datenbank angepasst haben, ist es unwahrscheinlich, dass Sie das Schema regelmäßig aktualisieren müssen, da&#x2014;es nicht mit der gleichen Häufigkeit wie der Code in einer Webanwendung oder einem Datenbankprojekt geändert wird. Daher sollten Sie die Mitgliedschafts Datenbank nicht in automatisierte oder einstufige Bereitstellungs Prozesse einschließen.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Verwenden von VSDBCMD zum Aktualisieren eines Mitgliedschafts Datenbankschemas

Wenn Sie die Struktur der Mitgliedschafts Datenbank nach der ersten Bereitstellung ändern, empfiehlt es sich, das Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) zum erneuten Bereitstellen der Datenbank zu verwenden. Die Funktion zur Daten Bank Bereitstellung in Web deploy umfasst nicht die Möglichkeit, stattdessen differenzielle Aktualisierungen&#x2014;an einer Zieldatenbank vorzunehmen, Web deploy muss die Datenbank löschen und neu erstellen. Dies bedeutet, dass Sie alle vorhandenen Benutzerkontodaten verlieren, was in Staging-oder Produktionsumgebungen normalerweise unerwünscht ist.

Alternativ können Sie das VSDBCmd-Hilfsprogramm verwenden, um das Schema der Zieldatenbank zu aktualisieren. VSDBCMD umfasst zwei wichtige Funktionen. Zuerst kann das Schema einer vorhandenen Datenbank in eine dbschema-Datei importiert werden. Zweitens kann es eine dbschema-Datei in einer vorhandenen Datenbank als differenzielles Update bereitstellen. Dies bedeutet, dass nur die Änderungen vorgenommen werden, die erforderlich sind, um die Zieldatenbank auf den neuesten Stand zu bringen, und dass keine Daten verloren gehen.

Zum Aktualisieren eines Mitgliedschafts Datenbankschemas können Sie die folgenden Schritte ausführen:

1. Verwenden Sie die **Import** Aktion von VSDBCMD, um eine dbschema-Datei für die Quell Mitgliedschafts Datenbank zu generieren. Dieses Verfahren wird unter Gewusst [wie: Importieren eines Schemas aus einer Eingabeaufforderung](https://msdn.microsoft.com/library/dd172135.aspx)beschrieben.
2. Verwenden Sie die Bereitstellungs **Aktion "** VSDBCmd", um die dbschema-Datei in Ihrer Ziel Mitgliedschafts Datenbank bereitzustellen. Dieses Verfahren wird in der [Befehlszeilen Referenz für VSDBCMD beschrieben. EXE (Bereitstellung und Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wurden einige der Herausforderungen beschrieben, die möglicherweise auftreten, wenn Sie ASP.net-Mitgliedschafts Datenbanken in verschiedenen Ziel Umgebungen bereitstellen müssen. Insbesondere wurde erläutert, warum bei reinen Schema Bereitstellungen die Mitgliedschafts Datenbank nicht in Betrieb ist und die Bereitstellung von Benutzerkontodaten nicht unterstützt wird. Das Thema stellt auch Anleitungen zum Bereitstellen, bereitstellen und Aktualisieren von Mitgliedschafts Datenbanken in verschiedenen Szenarien bereit.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Anleitungen und Beispiele zur Verwendung von VSDBCMD finden Sie unter [Befehlszeilen Referenz für VSDBCMD. EXE (Bereitstellung und Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx) und Gewusst [wie: Importieren eines Schemas von einer Eingabeaufforderung aus](https://msdn.microsoft.com/library/dd172135.aspx). Weitere Informationen zur Verwendung von ASPNET\_RegSql. exe zum Erstellen von Mitgliedschafts Datenbanken finden Sie unter [ASP.NET SQL Server Registration Tool (ASPNET\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Allgemeine Anleitungen zum Bereitstellen von Mitgliedschafts Datenbanken finden [Sie unter Gewusst wie: Bereitstellen der ASP.net-Mitgliedschafts Datenbank ohne einschließen von Benutzerkonten](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Zurück](deploying-database-role-memberships-to-test-environments.md)
> [Weiter](excluding-files-and-folders-from-deployment.md)
