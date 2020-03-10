---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Befehlszeilen Bereitstellung | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512073"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Befehlszeilen Bereitstellung

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht

In diesem Tutorial wird gezeigt, wie Sie die Visual Studio-Webveröffentlichungs Pipeline von der Befehlszeile aus aufrufen. Dies ist nützlich für Szenarien, in denen Sie [den Bereitstellungs Prozess automatisieren](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) möchten, anstatt ihn manuell in Visual Studio zu verwenden, in der Regel mit einem [Quell Code Versionskontrollsystem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Vornehmen einer Änderung an der Bereitstellung

Die Seite "Info" zeigt derzeit den Vorlagen Code an.

![Info Seite mit Vorlagen Code](command-line-deployment/_static/image1.png)

Ersetzen Sie dies durch Code, der eine Zusammenfassung der Schüler-/Studentenregistrierung anzeigt.

Öffnen Sie die Seite *about. aspx* , löschen Sie das gesamte Markup innerhalb des `MainContent` `Content`-Elements, und fügen Sie das folgende Markup an der Stelle ein:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Führen Sie das Projekt aus, und wählen Sie **die Seite Info** aus.

![Infoseite](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Bereitstellen für Tests über die Befehlszeile

Sie werden keine andere Daten Bank Änderung bereitstellen, daher deaktivieren Sie die dbdacfx-Daten Bank Bereitstellung für die ASPNET-contosouniversity-Datenbank. Öffnen Sie den Assistenten **Web veröffentlichen** , und deaktivieren Sie in jedem der drei Veröffentlichungs Profile das Kontrollkästchen **Datenbank aktualisieren** auf der Registerkarte **Einstellungen** .

Suchen Sie auf der Start Seite von Windows 8 nach **Developer-Eingabeaufforderung für VS2012**.

Klicken Sie mit der rechten Maustaste auf das Symbol für **Developer-Eingabeaufforderung für VS2012** , und klicken Sie auf **als Administrator ausführen**.

Geben Sie den folgenden Befehl an der Eingabeaufforderung ein, und ersetzen Sie dabei den Pfad zur Projektmappendatei durch den Pfad zu ihrer Projektmappendatei:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild erstellt die Lösung und stellt Sie in der Testumgebung bereit.

![Befehlszeilenausgabe](command-line-deployment/_static/image3.png)

Öffnen Sie einen Browser, navigieren Sie zu `http://localhost/ContosoUniversity`, und klicken Sie dann **auf die Seite Info, um sicher** zustellen, dass die Bereitstellung erfolgreich war

Wenn Sie im Test keine Studenten erstellt haben, wird unter der Überschrift " **Student Body Statistics** " eine leere Seite angezeigt. Wechseln Sie **zur Seite "** **Studenten** ", klicken Sie auf " **Student hinzufügen**", und fügen Sie einige Schüler und Studenten hinzu.

![Info Seite in Test Umgebung](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Wichtige Befehlszeilenoptionen

Der von Ihnen eingegebene Befehl hat den projektmappendateipfad und zwei Eigenschaften an MSBuild übermittelt:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Bereitstellen der Lösung im Vergleich zur Bereitstellung einzelner Projekte

Das Angeben der Projektmappendatei bewirkt, dass alle Projekte in der Projekt Mappe erstellt werden. Wenn Sie mehrere Webprojekte in der Projekt Mappe haben, gilt das folgende MSBuild-Verhalten:

- Die Eigenschaften, die Sie in der Befehlszeile angeben, werden an jedes Projekt weitergeleitet. Daher muss jedes Webprojekt über ein Veröffentlichungs Profil mit dem von Ihnen angegebenen Namen verfügen. Wenn Sie `/p:PublishProfile=Test`angeben, muss jedes Webprojekt über ein Veröffentlichungs Profil mit dem Namen " *Test*" verfügen.
- Sie können ein Projekt erfolgreich veröffentlichen, wenn ein anderes Projekt nicht gerade erstellt wird. Weitere Informationen finden Sie unter Fehler bei MSBuild für den StackOverflow-Thread [mit zwei Paketen](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Wenn Sie anstelle einer Projekt Mappe ein einzelnes Projekt angeben, müssen Sie einen Parameter hinzufügen, der die Visual Studio-Version angibt. Wenn Sie Visual Studio 2012 verwenden, sieht die Befehlszeile in etwa wie im folgenden Beispiel aus:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Die Versionsnummer für Visual Studio 2010 ist 10,0. Weitere Informationen finden Sie unter [Visual Studio-Projekt Kompatibilität und visualstudioversion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) für den Sayed Hashimi-Blog.

### <a name="specifying-the-publish-profile"></a>Angeben des Veröffentlichungs Profils

Sie können das Veröffentlichungs Profil anhand des Namens oder des vollständigen Pfads der *pubxml* -Datei angeben, wie im folgenden Beispiel gezeigt:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Webveröffentlichungs Methoden, die für die Befehlszeilen Veröffentlichung unterstützt werden

Drei Veröffentlichungs Methoden werden für die Veröffentlichung von Befehlszeilen unterstützt:

- `MSDeploy` mit Web deploy veröffentlichen.
- `Package`-Veröffentlichung durch Erstellen eines Web deploy Pakets. Sie müssen das Paket getrennt vom MSBuild-Befehl installieren, von dem es erstellt wird.
- `FileSystem`-Veröffentlichung durch Kopieren von Dateien in einen angegebenen Ordner.

### <a name="specifying-the-build-configuration-and-platform"></a>Angeben der Buildkonfiguration und-Plattform

Die Buildkonfiguration und-Plattform müssen in Visual Studio oder in der Befehlszeile festgelegt werden. Die Veröffentlichungs Profile enthalten Eigenschaften mit dem Namen `LastUsedBuildConfiguration` und `LastUsedPlatform`. Sie können diese Eigenschaften jedoch nicht festlegen, um zu bestimmen, wie das Projekt erstellt wird. Weitere Informationen finden Sie unter [MSBuild: So legen Sie die Konfigurations Eigenschaft für den](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Blog von Sayed Hashimi fest.

## <a name="deploy-to-staging"></a>Bereitstellen in der Stagingumgebung

Zum Bereitstellen in Azure müssen Sie das Kennwort der Befehlszeile hinzufügen. Wenn Sie das Kennwort im Veröffentlichungs Profil in Visual Studio gespeichert haben, wurde es in verschlüsselter Form in der *pubxml. User* -Datei gespeichert. Der Zugriff auf diese Datei erfolgt nicht durch MSBuild, wenn Sie eine Befehlszeilen Bereitstellung ausführen, sodass Sie das Kennwort in einem Befehlszeilenparameter übergeben müssen.

1. Kopieren Sie das benötigte Kennwort aus der *publishsettings* -Datei, die Sie zuvor für die Stagingwebsite heruntergeladen haben. Das Kennwort ist der Wert des `userPWD`-Attributs für das Web deploy `publishProfile`-Element.

    ![Web deploy Kennwort](command-line-deployment/_static/image5.png)
2. Suchen Sie auf der Start Seite von Windows 8 nach **Developer-Eingabeaufforderung für VS2012**, und klicken Sie auf das Symbol, um die Eingabeaufforderung zu öffnen. (Sie müssen diese Zeit nicht als Administrator öffnen, weil Sie nicht auf IIS auf dem lokalen Computer bereitstellen.)
3. Geben Sie den folgenden Befehl an der Eingabeaufforderung ein, und ersetzen Sie dabei den Pfad zur Projektmappendatei durch den Pfad zu ihrer Projektmappendatei und das Kennwort durch Ihr Kennwort:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Beachten Sie, dass diese Befehlszeile einen zusätzlichen Parameter enthält: `/p:AllowUntrustedCertificate=true`. Wenn dieses Lernprogramm geschrieben wird, muss die `AllowUntrustedCertificate`-Eigenschaft festgelegt werden, wenn Sie über die Befehlszeile in Azure veröffentlichen. Wenn die Behebung für diesen fehlerfrei gegeben wird, benötigen Sie diesen Parameter nicht.
4. Öffnen Sie einen Browser, navigieren Sie zur URL Ihrer Stagingwebsite, und klicken Sie dann auf die Seite Info, um **zu überprüfen** , ob die Bereitstellung erfolgreich war.

    Wie Sie bereits gesehen haben, müssen Sie möglicherweise einige Studenten erstellen **, um Statistiken auf der Seite** "Info" anzuzeigen.

## <a name="deploy-to-production"></a>Bereitstellen für die Produktion

Der Prozess für die Bereitstellung in der Produktion ähnelt dem Stagingprozess.

1. Kopieren Sie das benötigte Kennwort aus der Datei " *. publishsettings* ", die Sie zuvor für die Produktions Website heruntergeladen haben.
2. Öffnen Sie **Developer-Eingabeaufforderung für VS2012**.
3. Geben Sie den folgenden Befehl an der Eingabeaufforderung ein, und ersetzen Sie dabei den Pfad zur Projektmappendatei durch den Pfad zu ihrer Projektmappendatei und das Kennwort durch Ihr Kennwort:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Bei einer realen Produktions Site kopieren Sie in der Regel vor der Bereitstellung die APP\_Datei " *Offline. htm* " auf die Website, und löschen Sie Sie nach erfolgreicher Bereitstellung.
4. Öffnen Sie einen Browser, navigieren Sie zur URL Ihrer Stagingwebsite, und klicken Sie dann auf die Seite Info, um **zu überprüfen** , ob die Bereitstellung erfolgreich war.

## <a name="summary"></a>Zusammenfassung

Sie haben nun ein Anwendungs Update über die Befehlszeile bereitgestellt.

![Info Seite in Test Umgebung](command-line-deployment/_static/image6.png)

Im nächsten Tutorial sehen Sie ein Beispiel zum Erweitern der Webveröffentlichungs Pipeline. Im Beispiel wird gezeigt, wie Sie Dateien bereitstellen, die nicht im Projekt enthalten sind.

> [!div class="step-by-step"]
> [Zurück](deploying-a-database-update.md)
> [Weiter](deploying-extra-files.md)
