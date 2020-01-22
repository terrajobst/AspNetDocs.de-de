---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Transformationen der Datei "Web. config"-3 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9e7902bcf8a16c154aee1a982824bfaedeea7d9d
ms.sourcegitcommit: 7b1e1784213dd4c301635f9e181764f3e2f94162
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76309235"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Transformationen der Datei "Web. config"-3 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht über

In diesem Tutorial wird gezeigt, wie Sie den Prozess der Änderung der Datei " *Web. config* " automatisieren, wenn Sie Sie in verschiedenen Ziel Umgebungen bereitstellen. Die meisten Anwendungen verfügen über Einstellungen in der Datei " *Web. config* ", die sich bei der Bereitstellung der Anwendung unterscheiden müssen. Wenn Sie den Prozess der Durchführung dieser Änderungen automatisieren, müssen Sie Sie nicht jedes Mal manuell ausführen, wenn Sie bereitstellen, was mühsam und fehleranfällig wäre.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web. config-Transformationen im Vergleich zu Web deploy-Parametern

Es gibt zwei Möglichkeiten, den Prozess der Änderung von *Web. config* -Datei Einstellungen zu automatisieren: [Web. config-Transformationen](https://msdn.microsoft.com/library/dd465326.aspx) und [Web deploy Parameter](https://msdn.microsoft.com/library/ff398068.aspx). Eine *Web. config* -Transformations Datei enthält XML-Markup, das angibt, wie die Datei " *Web. config* " bei der Bereitstellung geändert wird. Sie können unterschiedliche Änderungen für bestimmte Buildkonfigurationen und für bestimmte Veröffentlichungs profile angeben. Die Standardbuildkonfigurationen sind Debug und Release, und Sie können benutzerdefinierte Buildkonfigurationen erstellen. Ein Veröffentlichungs Profil entspricht in der Regel einer Zielumgebung. (Weitere Informationen zu Veröffentlichungs Profilen finden Sie im Tutorial bereitstellen [in IIS als Test Umgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .)

Web deploy Parameter können verwendet werden, um viele verschiedene Arten von Einstellungen anzugeben, die während der Bereitstellung konfiguriert werden müssen, einschließlich der in *Web. config* -Dateien gefundenen Einstellungen. Wenn Sie zum Angeben von Änderungen an *Web. config* -Dateien verwendet werden, sind Web deploy Parameter komplexer einzurichten, Sie sind jedoch nützlich, wenn Sie den festzulegenden Wert erst nach der Bereitstellung kennen. In einer Unternehmensumgebung können Sie z. b. ein *Bereitstellungs Paket* erstellen und an eine Person in der IT-Abteilung übergeben, um Sie in der Produktionsumgebung zu installieren, und diese Person muss in der Lage sein, Verbindungs Zeichenfolgen oder Kenn Wörter einzugeben, die Sie nicht kennen.

In dem Szenario, das in diesem Tutorial behandelt wird, wissen Sie alles, was für die Datei " *Web. config* " erforderlich ist, sodass Sie keine Web deploy Parameter verwenden müssen. Sie konfigurieren einige Transformationen, die sich je nach verwendeter Buildkonfiguration unterscheiden, und einige unterschiedliche Transformationen, je nachdem, welches Veröffentlichungs Profil verwendet wird.

## <a name="creating-transformation-files-for-publish-profiles"></a>Erstellen von Transformations Dateien für Veröffentlichungs profile

Erweitern Sie in **Projektmappen-Explorer**den Eintrag *Web. config* , um die Transformations Dateien *Web. Debug. config* und *Web. Release. config* anzuzeigen, die standardmäßig für die beiden Standardbuildkonfigurationen erstellt werden.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Sie können Transformations Dateien für benutzerdefinierte Buildkonfigurationen erstellen, indem Sie mit der rechten Maustaste auf die Datei Web. config klicken und **Konfigurations Transformationen hinzufügen** aus dem Kontextmenü auswählen, aber für dieses Tutorial müssen Sie dies nicht tun.

Sie benötigen zwei weitere Transformations Dateien zum Konfigurieren von Änderungen, die sich auf das Bereitstellungs Ziel beziehen, anstatt auf die Buildkonfiguration. Ein typisches Beispiel für diese Art von Einstellung ist ein WCF-Endpunkt, der für Tests und Produktion anders ist. In späteren Tutorials erstellen Sie Veröffentlichungs Profile mit dem Namen "Test" und "Produktion", sodass Sie eine *Web. Test. config* -Datei und eine *Web. Production. config* -Datei benötigen.

An Veröffentlichungs profile gebundene Transformations Dateien müssen manuell erstellt werden. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt contosouniversity, und wählen Sie **Ordner in Windows-Explorer öffnen aus**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

Wählen Sie in **Windows-Explorer**die Datei *Web. Release. config* aus, kopieren Sie die Datei, und fügen Sie dann zwei Kopien der Datei ein. Benennen Sie diese Datei in " *Web. Production. config* " und " *Web. Test. config*" um, und schließen Sie **Windows Explorer**.

Klicken Sie in **Projektmappen-Explorer**auf **Aktualisieren** , um die neuen Dateien anzuzeigen.

Wählen Sie die neuen Dateien aus, klicken Sie mit der rechten Maustaste, und klicken Sie dann im Kontextmenü auf **in Projekt einschließen** .

![Einschließen von Test-und Produktions Konfigurationsdateien in das Projekt](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Um zu verhindern, dass diese Dateien bereitgestellt werden, wählen Sie diese in **Projektmappen-Explorer**aus, und ändern Sie dann im **Eigenschaften** Fenster **die Eigenschaft** Buildvorgang von **Inhalt** in **keine**. (Die Bereitstellung der Transformations Dateien, die auf Buildkonfigurationen basieren, wird automatisch verhindert.)

Sie können jetzt *Web. config* -Transformationen in die *Web. config* -Transformations Dateien eingeben.

## <a name="limiting-error-log-access-to-administrators"></a>Beschränken des Zugriffs auf das Fehlerprotokoll auf Administratoren

Wenn ein Fehler auftritt, während die Anwendung ausgeführt wird, zeigt die Anwendung anstelle der vom System generierten Fehlerseite eine generische Fehlerseite an, und für die Fehler Protokollierung und Berichterstellung wird das [ELMAH-nuget-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) verwendet. Das `customErrors`-Element in der Datei " *Web. config* " gibt die Fehlerseite an:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Um die Fehlerseite anzuzeigen, ändern Sie vorübergehend das `mode`-Attribut des `customErrors`-Elements von "RemoteOnly" in "on", und führen Sie die Anwendung von Visual Studio aus. Verursacht einen Fehler, indem eine ungültige URL angefordert wird, z. b. " *studentsxxx. aspx*". Anstelle einer von IIS generierten Fehlerseite "Seite nicht gefunden" wird die Seite " *GenericErrorPage. aspx* " angezeigt.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Um das Fehlerprotokoll anzuzeigen, ersetzen Sie alles in der URL nach der Portnummer mit *ELMAH. axd* (für das Beispiel im Screenshot, `http://localhost:51130/elmah.axd`), und drücken Sie die EINGABETASTE:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Vergessen Sie nicht, das `customErrors`-Element wieder auf den Modus "RemoteOnly" festzulegen, wenn Sie fertig sind.

Auf dem Entwicklungs Computer ist es praktisch, den kostenlosen Zugriff auf die Fehlerprotokoll Seite zu ermöglichen, aber in der Produktionsumgebung, die ein Sicherheitsrisiko darstellen würde. Für den Produktionsstandort können Sie eine Autorisierungs Regel hinzufügen, die den Zugriff auf den Fehlerprotokoll nur auf Administratoren einschränkt, indem Sie eine Transformation in der Datei " *Web. Production. config* " konfigurieren.

Öffnen Sie *Web. Production. config* , und fügen Sie ein neues `location`-Element direkt nach dem öffnenden `configuration`-Tag hinzu, wie hier gezeigt. (Stellen Sie sicher, dass Sie nur das `location`-Element und nicht das umgebende Markup hinzufügen, das hier nur zur Bereitstellung eines Kontexts angezeigt wird.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

Der `Transform`-Attribut Wert "Insert" bewirkt, dass dieses `location` Element allen vorhandenen `location` Elementen in der Datei " *Web. config* " als gleich geordnetes Element hinzugefügt wird. (Es gibt bereits ein `location` Element, das Autorisierungs Regeln für die Seite " **Update Guthaben** " angibt.) Wenn Sie den Produktionsstandort nach der Bereitstellung testen, testen Sie, ob diese Autorisierungs Regel gültig ist.

Sie müssen den Zugriff auf den Fehlerprotokoll in der Testumgebung nicht einschränken, daher müssen Sie diesen Code nicht der Datei " *Web. Test. config* " hinzufügen.

> [!NOTE] 
> 
> **Sicherheitshinweis** Zeigen Sie in einer Produktionsanwendung niemals Fehlerdetails der Öffentlichkeit an, oder speichern Sie diese Informationen an einem öffentlichen Speicherort. Angreifer können Fehlerinformationen verwenden, um Sicherheitsrisiken an einem Standort zu ermitteln. Wenn Sie ELMAH in ihrer eigenen Anwendung verwenden, sollten Sie untersuchen, wie ELMAH konfiguriert werden kann, um Sicherheitsrisiken zu minimieren. Das ELMAH-Beispiel in diesem Tutorial sollte nicht als empfohlene Konfiguration betrachtet werden. Es ist ein Beispiel, das ausgewählt wurde, um zu veranschaulichen, wie ein Ordner behandelt werden kann, in dem die Anwendung Dateien erstellen muss.

## <a name="setting-an-environment-indicator"></a>Festlegen eines Umgebungs Indikators

Ein häufiges Szenario besteht darin, dass die Einstellungen der Datei " *Web. config* " in jeder Umgebung, in der Sie bereitgestellt werden, unterschiedlich sein müssen. Beispielsweise benötigt eine Anwendung, die einen WCF-Dienst aufruft, möglicherweise einen anderen Endpunkt in Test-und Produktionsumgebungen. Die Anwendung "" der Anwendung "" der Anwendung "" ist ebenfalls auf diese Art festgelegt. Mit dieser Einstellung wird ein sichtbarer Indikator auf den Seiten einer Site gesteuert, die Aufschluss darüber gibt, in welcher Umgebung Sie sich befinden (z. b. Entwicklung, Test oder Produktion). Der Einstellungs Wert bestimmt, ob die Anwendung "(dev)" oder "(Test)" an die Hauptüberschrift auf der Seite " *Site. Master* Master" anfügt:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Der Umgebungs Indikator wird weggelassen, wenn die Anwendung in der Produktionsumgebung ausgeführt wird.

Die Web Pages der conum so University haben einen Wert gelesen, der in `appSettings` in der Datei " *Web. config* " festgelegt ist, um zu bestimmen, in welcher Umgebung die Anwendung ausgeführt wird:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Der Wert sollte in der Testumgebung "Test" und in der Produktionsumgebung "Prod" lauten.

Öffnen Sie *Web. Production. config* , und fügen Sie unmittelbar vor dem öffnenden Tag des `location` Elements, das Sie zuvor hinzugefügt haben, ein `appSettings` Element hinzu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

Der `xdt:Transform`-Attribut Wert "", gibt an, dass der Zweck dieser Transformation darin besteht, Attributwerte eines vorhandenen Elements in der Datei " *Web. config* " zu ändern. Der `xdt:Locator`-Attribut Wert "Match (Key)" gibt an, dass das zu ändernde Element das Element ist, dessen `key` Attribut mit dem hier angegebenen `key`-Attribut übereinstimmt. Das einzige andere Attribut des `add`-Elements ist `value`, und das wird in der bereitgestellten *Web. config* -Datei geändert. Dieser Code bewirkt, dass das `value`-Attribut des `Environment` `appSettings`-Elements in der *Web. config* -Datei, die in der Produktionsumgebung bereitgestellt wird, auf "Prod" festgelegt wird.

Wenden Sie als nächstes die gleiche Änderung auf die Datei " *Web. Test. config* " an, und legen Sie die `value` auf "Test" anstelle von "Prod" fest. Wenn Sie den Vorgang abgeschlossen haben, sieht der Abschnitt "`appSettings`" in der *Datei "Web. Test. config* " wie im folgenden Beispiel aus:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Debugmodus wird deaktiviert

Bei einem Releasebuild soll das Debuggen unabhängig von der Umgebung, in der Sie bereitgestellt werden, nicht aktiviert werden. Standardmäßig wird die Transformations Datei *Web. Release. config* automatisch mit Code erstellt, mit dem das `debug`-Attribut aus dem `compilation`-Element entfernt wird:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

Das Attribut `Transform` bewirkt, dass das `debug` Attribut aus der bereitgestellten *Web. config* -Datei weggelassen wird, wenn Sie einen Releasebuild bereitstellen.

Dieselbe Transformation befindet sich in Test-und Produktions Transformations Dateien, da Sie Sie durch Kopieren der releasetransformations-Datei erstellt haben. Sie benötigen Sie nicht duplizieren, öffnen Sie also jede dieser Dateien, entfernen Sie das **Kompilierungs** Element, und speichern und schließen Sie jede Datei.

## <a name="setting-connection-strings"></a>Verbindungs Zeichenfolgen festlegen

In den meisten Fällen müssen Sie keine Verbindungs Zeichenfolgen-Transformationen einrichten, da Sie Verbindungs Zeichenfolgen im Veröffentlichungs Profil angeben können. Beim Bereitstellen einer SQL Server Compact Datenbank gibt es jedoch eine Ausnahme, und Sie verwenden Entity Framework Code First-Migrationen, um die Datenbank auf dem Ziel Server zu aktualisieren. In diesem Szenario müssen Sie eine zusätzliche Verbindungs Zeichenfolge angeben, die auf dem Server zum Aktualisieren des Datenbankschemas verwendet wird. Um diese Transformation einzurichten, fügen Sie ein **&lt;connectionStrings&gt;** -Element direkt nach dem öffnenden **&lt;Configuration&gt;** -Tag in den Transformations Dateien *Web. Test. config* und *Web. Production. config* hinzu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

Das `Transform`-Attribut gibt an, dass diese Verbindungs Zeichenfolge dem *connectionStrings* -Element in der bereitgestellten *Web. config* -Datei hinzugefügt wird. (Der Veröffentlichungsprozess erstellt diese zusätzliche Verbindungs Zeichenfolge automatisch für Sie, wenn Sie nicht vorhanden ist, aber standardmäßig wird das **providerName** -Attribut auf `System.Data.SqlClient`festgelegt, was für SQL Server Compact nicht funktioniert. Durch manuelles Hinzufügen der Verbindungs Zeichenfolge wird verhindert, dass der Bereitstellungs Prozess ein Verbindungs Zeichen folgen Element mit dem falschen Anbieter Namen erstellt.)

Sie haben jetzt alle *Web. config* -Transformationen angegeben, die Sie zum Bereitstellen der Conto-University-Anwendung für die Test-und Produktionsumgebung benötigen. Im folgenden Tutorial werden Bereitstellungs Einrichtungs Aufgaben behandelt, die das Festlegen von Projekteigenschaften erfordern.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie im Web. config-Transformations Szenario in [ASP.net Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
