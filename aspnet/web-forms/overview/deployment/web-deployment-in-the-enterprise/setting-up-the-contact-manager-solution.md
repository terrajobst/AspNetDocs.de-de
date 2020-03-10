---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Einrichten der Contact Manager-Lösung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie die Contact Manager-Lösung herunterladen und konfigurieren, um lokal auf einer Entwickler Arbeitsstation auszuführen.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511161"
---
# <a name="setting-up-the-contact-manager-solution"></a>Einrichten der Contact Manager-Lösung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie die Contact Manager-Lösung herunterladen und konfigurieren, um lokal auf einer Entwickler Arbeitsstation auszuführen.

## <a name="system-requirements"></a>Systemanforderungen

Wenn Sie die Contact Manager-Lösung lokal ausführen und die anderen in diesem Tutorial beschriebenen Aufgaben ausführen möchten, müssen Sie diese Software auf Ihrer Entwickler Arbeitsstation installieren:

- Visual Studio 2010 Service Pack 1, Premium oder Ultimate Edition
- Internetinformationsdienste (IIS) 7,5 Express
- SQL Server Express 2008 R2
- IIS-Webbereitstellungs Tool (Web deploy) 2,1 oder höher
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Mit Ausnahme von Visual Studio 2010 können Sie die neuesten Versionen aller dieser Produkte und Komponenten über den [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118)herunterladen und installieren.

## <a name="download-and-extract-the-solution"></a>Herunterladen und Extrahieren der Lösung

Sie können die Beispielanwendung Contact Manager aus der MSDN Code Gallery [hier](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0)herunterladen.

## <a name="configure-and-run-the-solution"></a>Konfigurieren und Ausführen der Lösung

Zum Konfigurieren und Ausführen der Contact Manager-Lösung auf dem lokalen Computer müssen Sie die folgenden Grundschritte ausführen:

1. Wenn Sie noch nicht über eins verfügen, erstellen Sie eine lokale ASP.NET-Anwendungs Dienst Datenbank mit aktivierten Mitgliedschafts-und Rollen Verwaltungsfunktionen.
2. Bearbeiten Sie Verbindungs Zeichenfolgen in den *Web. config* -Dateien so, dass Sie auf Ihre lokale SQL Server Express Instanz verweisen.
3. Führen Sie die Projekt Mappe aus Visual Studio 2010 aus.

Im restlichen Teil dieses Abschnitts finden Sie weitere Anleitungen zum Ausführen dieser Aufgaben.

**So erstellen Sie die Anwendungs Dienst Datenbank**

1. Öffnen Sie eine Visual Studio 2010-Eingabeaufforderung. Zeigen Sie dazu im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, klicken Sie auf **Visual Studio-Tools**, und klicken Sie dann auf **Visual Studio-Eingabeaufforderung (2010)** .
2. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Verwenden Sie den Schalter **– C** , um die Verbindungs Zeichenfolge für den Datenbankserver anzugeben.
    2. Verwenden Sie den Schalter **– A** , um die Anwendungs Dienst Features anzugeben, die Sie der Datenbank hinzufügen möchten. In diesem Fall gibt **m** an, dass Sie Unterstützung für den Mitgliedschafts Anbieter hinzufügen möchten, und **r** gibt an, dass Sie Unterstützung für den Rollen-Manager hinzufügen möchten.
    3. Verwenden Sie den Schalter **– d** , um einen Namen für die Anwendungs Dienst Datenbank anzugeben. Wenn Sie diesen Schalter weglassen, erstellt das Hilfsprogramm eine Datenbank mit dem Standardnamen **aspnetdb**.
3. Wenn die Datenbank erfolgreich erstellt wurde, wird in der Eingabeaufforderung eine Bestätigung angezeigt.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Weitere Informationen zum ASPNET\_RegSql-Hilfsprogramm finden Sie unter [ASP.NET SQL Server Registration Tool (ASPNET\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).

Im nächsten Schritt müssen Sie sicherstellen, dass die Verbindungs Zeichenfolgen in der Contact Manager-Lösung auf Ihre lokale Instanz von SQL Server Express verweisen.

**So aktualisieren Sie die Verbindungs Zeichenfolgen**

1. Öffnen Sie die Contact Manager-Lösung in Visual Studio 2010.
2. Erweitern Sie im Fenster **Projektmappen-Explorer** das Projekt **ContactManager. MVC** , und doppelklicken Sie dann auf den Knoten **Web. config** .

    > [!NOTE]
    > Das ContactManager. MVC-Projekt enthält zwei *Web. config* -Dateien. Sie müssen die Datei auf Projektebene bearbeiten.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. Überprüfen Sie im **connectionStrings** -Element, ob die Verbindungs Zeichenfolge mit dem Namen **ApplicationServices** auf Ihre lokale ASP.NET-Anwendungs Dienst Datenbank verweist.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. Erweitern Sie im Fenster **Projektmappen-Explorer** das Projekt **ContactManager. Service** , und doppelklicken Sie dann auf den Knoten **Web. config** .

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. Überprüfen Sie im **connectionStrings** -Element in der Verbindungs Zeichenfolge mit dem Namen **contactmanagercontext**, ob die **Datenquellen** Eigenschaft auf Ihre lokale Instanz von SQL Server Express festgelegt ist. Sie müssen nichts anderes in der Verbindungs Zeichenfolge ändern.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Speichern Sie alle geöffneten Dateien.

Jetzt sollten Sie bereit sein, die Contact Manager-Lösung auf dem lokalen Computer auszuführen.

> [!NOTE]
> Wenn Sie diese Schritte ausführen, ohne zuerst eine Anwendungs Dienst-Datenbank zu erstellen, erstellt ASP.net die Datenbank, wenn Sie zum ersten Mal versuchen, einen Benutzer zu erstellen. Wenn Sie die Datenbank manuell erstellen, haben Sie jedoch viel mehr Kontrolle über die Anwendungs Dienst-Funktionsgruppe, die Sie unterstützen möchten.

**So führen Sie die Contact Manager-Lösung aus**

1. Drücken Sie in Visual Studio 2010 die Taste F5.
2. Internet Explorer wird gestartet und fordert die URL der Anwendung Contact Manager ASP.NET MVC 3 an. Standardmäßig wird von der Anwendung die Seite **alle Kontakte** angezeigt.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Fügen Sie einige Kontakte hinzu, und überprüfen Sie dann, ob die Anwendung erwartungsgemäß funktioniert.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Navigieren Sie zu `http://localhost:50114/Account/Register` (passen Sie die URL an, wenn Sie die Anwendung auf einem anderen Port verwenden). Fügen Sie einen Benutzernamen, eine e-Mail-Adresse und ein Kennwort hinzu, und überprüfen Sie, ob ein Konto erfolgreich registriert werden kann.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Navigieren Sie zu `http://localhost:50114/Account/LogOn` (passen Sie die URL an, wenn Sie die Anwendung auf einem anderen Port verwenden). Vergewissern Sie sich, dass Sie sich mit dem soeben erstellten Konto anmelden können.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Schließen Sie Internet Explorer, um das Debugging zu beenden.

## <a name="conclusion"></a>Zusammenfassung

An diesem Punkt sollte die Contact Manager-Lösung vollständig so konfiguriert sein, dass Sie auf dem lokalen Computer ausgeführt wird. Sie können die Lösung als Referenz verwenden, wenn Sie die anderen Themen in diesem Tutorial durcharbeiten.

Im nächsten Thema, Grundlegendes [zur Projektdatei](understanding-the-project-file.md), wird erläutert, wie Sie die Projektdateien für benutzerdefinierte Microsoft-Build-Engine (MSBuild) in der Contact Manager-Lösung verwenden können, um den Bereitstellungs Prozess zu steuern.

> [!div class="step-by-step"]
> [Zurück](the-contact-manager-solution.md)
> [Weiter](understanding-the-project-file.md)
