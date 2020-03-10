---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Bereitstellen von Daten bankrollen Mitgliedschaften in Test Umgebungen | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie Benutzerkonten zu Daten bankrollen als Teil einer Lösungs Bereitstellung in einer Testumgebung hinzufügen. Beim Bereitstellen einer Projekt Mappe, die...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a15f5bf5f659d151e91ef9e53c5ad55bcd8e2b01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474945"
---
# <a name="deploying-database-role-memberships-to-test-environments"></a>Bereitstellen von Datenbankrollenmitgliedschaften in Testumgebungen

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie Benutzerkonten zu Daten bankrollen als Teil einer Lösungs Bereitstellung in einer Testumgebung hinzufügen.
> 
> Wenn Sie eine Lösung mit einem Datenbankprojekt in einer Staging-oder Produktionsumgebung bereitstellen, möchten Sie in der Regel nicht, dass Entwickler das Hinzufügen von Benutzerkonten zu Daten bankrollen automatisieren. In den meisten Fällen weiß der Entwickler nicht, welche Benutzerkonten zu welchen Daten bankrollen hinzugefügt werden müssen, und diese Anforderungen können sich jederzeit ändern. Wenn Sie jedoch eine Lösung bereitstellen, die ein Datenbankprojekt in einer Entwicklungs-oder Testumgebung enthält, ist die Situation in der Regel eher anders:
> 
> - Der Entwickler stellt die Lösung normalerweise regelmäßig mehrmals täglich wieder her.
> - Die Datenbank wird in der Regel bei jeder Bereitstellung neu erstellt. Dies bedeutet, dass Datenbankbenutzer erstellt und nach jeder Bereitstellung Rollen hinzugefügt werden müssen.
> - Der Entwickler verfügt in der Regel über vollständige Kontrolle über die zielentwicklungs-oder Testumgebung.
> 
> In diesem Szenario ist es häufig vorteilhaft, Datenbankbenutzer automatisch zu erstellen und im Rahmen des Bereitstellungs Prozesses Daten bankrollen Mitgliedschaften zuzuweisen.
> 
> Der Hauptfaktor ist, dass dieser Vorgang abhängig von der Zielumgebung bedingt sein muss. Wenn Sie in einer Staging-oder Produktionsumgebung bereitstellen, möchten Sie den Vorgang überspringen. Wenn Sie in einer Entwicklungs-oder Testumgebung bereitstellen, möchten Sie die Rollen Mitgliedschaften ohne weiteren Eingriff bereitstellen. In diesem Thema wird ein Ansatz beschrieben, mit dem Sie diese Herausforderung meistern können.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="task-overview"></a>Aufgaben Übersicht

In diesem Thema wird Folgendes vorausgesetzt:

- Sie verwenden den Ansatz zum Aufteilen von Projektdateien für die Lösungs Bereitstellung, wie in Grundlegendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschrieben.
- Sie können VSDBCmd aus der Projektdatei abrufen, um das Datenbankprojekt bereitzustellen, wie Untergrund Legendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md)beschrieben.

Wenn Sie Datenbankbenutzer erstellen und Rollen Mitgliedschaften zuweisen möchten, wenn Sie ein Datenbankprojekt in einer Testumgebung bereitstellen, müssen Sie folgende Schritte ausführen:

- Erstellen Sie ein Transact-strukturierte Abfragesprache-Skript (Transact-SQL), das die erforderlichen Daten Bank Änderungen vornimmt.
- Erstellen Sie ein Microsoft-Build-Engine-Ziel (MSBuild), das das sqlcmd. exe-Hilfsprogramm zum Ausführen des SQL-Skripts verwendet.
- Konfigurieren Sie die Projektdateien, um das Ziel beim Bereitstellen der Projekt Mappe in einer Testumgebung aufzurufen.

In diesem Thema wird gezeigt, wie Sie die einzelnen Prozeduren ausführen.

## <a name="scripting-the-database-role-memberships"></a>Skripterstellung für die Daten bankrollen Mitgliedschaften

Sie können ein Transact-SQL-Skript auf viele verschiedene Arten erstellen und an jedem beliebigen Ort auswählen. Der einfachste Ansatz besteht darin, das Skript in der Projekt Mappe in Visual Studio 2010 zu erstellen.

**So erstellen Sie ein SQL-Skript**

1. Erweitern Sie im Fenster **Projektmappen-Explorer** den Knoten Datenbankprojekt.
2. Klicken Sie mit der rechten Maustaste auf den Ordner **Scripts** , zeigen Sie auf **Hinzufügen**, und klicken Sie dann auf **neuer Ordner**
3. Geben Sie **Test** als Ordnernamen ein, und drücken Sie dann die EINGABETASTE.
4. Klicken Sie mit der rechten Maustaste auf den Ordner **Test** , zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Skript**.
5. Geben Sie im Dialogfeld **Neues Element hinzufügen** einen aussagekräftigen Namen (z **. b. addrolemembership. SQL**), und klicken Sie dann auf **Hinzufügen**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. Fügen Sie in der Datei *addrolemembership. SQL* folgende Transact-SQL-Anweisungen hinzu:

    1. Erstellen Sie einen Datenbankbenutzer für den SQL Server Anmelde Namen, der auf Ihre Datenbank zugreift.
    2. Fügen Sie den Datenbankbenutzer allen erforderlichen Daten bankrollen hinzu.
7. Die Datei sollte in etwa wie folgt aussehen:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Speichern Sie die Datei .

## <a name="executing-the-script-on-the-target-database"></a>Ausführen des Skripts in der Zieldatenbank

Im Idealfall würden Sie alle erforderlichen Transact-SQL-Skripts als Teil eines Skripts nach der Bereitstellung ausführen, wenn Sie das Datenbankprojekt bereitstellen. Skripts nach der Bereitstellung ermöglichen es Ihnen jedoch nicht, Logik bedingt auf der Grundlage von Projektmappenkonfigurationen oder Buildeigenschaften auszuführen. Alternativ können Sie die SQL-Skripts direkt aus der MSBuild-Projektdatei ausführen, indem Sie ein **target** -Element erstellen, das einen sqlcmd. exe-Befehl ausführt. Sie können diesen Befehl verwenden, um das Skript in der Zieldatenbank auszuführen:

[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]

> [!NOTE]
> Weitere Informationen zu sqlcmd-Befehlszeilenoptionen finden Sie unter [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).

Bevor Sie diesen Befehl in ein MSBuild-Ziel einbetten, müssen Sie berücksichtigen, unter welchen Bedingungen das Skript ausgeführt werden soll:

- Die Zieldatenbank muss vorhanden sein, bevor Sie die Rollen Mitgliedschaften ändern. Daher müssen Sie dieses Skript *nach* der Daten Bank Bereitstellung ausführen.
- Sie müssen eine Bedingung einschließen, damit das Skript nur für Testumgebungen ausgeführt wird.
- Wenn Sie eine "Was-wäre-wenn"&#x2014;-Bereitstellung mit anderen Worten ausführen, sollten Sie das SQL-Skript nicht&#x2014;ausführen, wenn Sie Bereitstellungs Skripts erstellen, aber nicht ausführen.

Wenn Sie den in Grundlegendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz zum Aufteilen von Projektdateien verwenden, können Sie die Buildanweisungen für das SQL-Skript wie folgt aufteilen:

- Alle erforderlichen Umgebungs spezifischen Eigenschaften und die-Eigenschaft, die bestimmt, ob Berechtigungen bereitgestellt werden sollen, sollten in die Umgebungs spezifische Projektdatei (z. *b. env-dev. proj*) gelangen.
- Das MSBuild-Ziel selbst und alle Eigenschaften, die sich nicht Zwischenziel Umgebungen ändern, sollten in die universelle Projektdatei (z. b. *Publish. proj*) gelangen.

In der Umgebungs spezifischen Projektdatei müssen Sie den Namen des Datenbankservers, den Namen der Zieldatenbank und eine boolesche Eigenschaft definieren, mit der der Benutzer angeben kann, ob Rollen Mitgliedschaften bereitgestellt werden sollen.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]

In der universellen Projektdatei müssen Sie den Speicherort der ausführbaren sqlcmd-Datei und den Speicherort des SQL-Skripts angeben, das Sie ausführen möchten. Diese Eigenschaften bleiben unabhängig von der Zielumgebung unverändert. Sie müssen auch ein MSBuild-Ziel erstellen, um den SQLCMD-Befehl auszuführen.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]

Beachten Sie, dass Sie den Speicherort der ausführbaren Datei sqlcmd als statische Eigenschaft hinzufügen, da dies für andere Ziele nützlich sein könnte. Im Gegensatz dazu definieren Sie den Speicherort des SQL-Skripts und die Syntax des sqlcmd-Befehls als dynamische Eigenschaften innerhalb des Ziels, da Sie vor der Ausführung des Ziels nicht benötigt werden. In diesem Fall wird das Bereitstellungs Ziel " **deploytestdbberechtigungs** " nur ausgeführt, wenn die folgenden Bedingungen erfüllt sind:

- Die **deploytestdbrolemembership** -Eigenschaft ist auf **true**festgelegt.
- Der Benutzer hat kein **WhatIf = true** -Flag angegeben.

Vergessen Sie schließlich nicht, das Ziel aufzurufen. In der Datei " *Publish. proj* " können Sie dies tun, indem Sie das Ziel der Abhängigkeits Liste für das standardmäßige **fullpublish** -Ziel hinzufügen. Sie müssen sicherstellen, dass das Ziel " **deploytestdbberechtigungs** " nicht ausgeführt wird, bis das Ziel " **publishdbpackages** " ausgeführt wurde.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wurde eine Möglichkeit beschrieben, wie Sie Datenbankbenutzer und Rollen Mitgliedschaften als Aktion nach der Bereitstellung hinzufügen können, wenn Sie ein Datenbankprojekt bereitstellen. Dies ist in der Regel hilfreich, wenn Sie eine Datenbank regelmäßig in einer Testumgebung neu erstellen, aber Sie sollte in der Regel vermieden werden, wenn Sie Datenbanken in Staging-oder Produktionsumgebungen bereitstellen. Daher sollten Sie sicherstellen, dass Sie die erforderliche bedingte Logik verwenden, damit Datenbankbenutzer und Rollen Mitgliedschaften nur erstellt werden, wenn dies angemessen ist.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zur Verwendung von VSDBCMD zum Bereitstellen von Datenbankprojekten finden Sie unter Bereitstellen von [Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md). Anleitungen zum Anpassen von Daten Bank Bereitstellungen für verschiedene Ziel Umgebungen finden Sie unter [Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen](customizing-database-deployments-for-multiple-environments.md). Weitere Informationen zur Verwendung von benutzerdefinierten MSBuild-Projektdateien zum Steuern des Bereitstellungs Prozesses finden Sie Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen zu sqlcmd-Befehlszeilenoptionen finden Sie unter [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Zurück](customizing-database-deployments-for-multiple-environments.md)
> [Weiter](deploying-membership-databases-to-enterprise-environments.md)
