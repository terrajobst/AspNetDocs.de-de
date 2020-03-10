---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Transformationen der Datei "Web. config" | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513711"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Transformationen der Datei "Web. config"

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht

In diesem Tutorial wird gezeigt, wie Sie den Prozess der Änderung der Datei " *Web. config* " automatisieren, wenn Sie Sie in verschiedenen Ziel Umgebungen bereitstellen. Die meisten Anwendungen verfügen über Einstellungen in der Datei " *Web. config* ", die sich bei der Bereitstellung der Anwendung unterscheiden müssen. Wenn Sie den Prozess der Durchführung dieser Änderungen automatisieren, müssen Sie Sie nicht jedes Mal manuell ausführen, wenn Sie bereitstellen, was mühsam und fehleranfällig wäre.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](troubleshooting.md)Behandlung überprüfen.

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web. config-Transformationen im Vergleich zu Web deploy-Parametern

Es gibt zwei Möglichkeiten, den Prozess der Änderung von *Web. config* -Datei Einstellungen zu automatisieren: [Web. config-Transformationen](https://msdn.microsoft.com/library/dd465326.aspx) und [Web deploy Parameter](https://msdn.microsoft.com/library/ff398068.aspx). Eine *Web. config* -Transformations Datei enthält XML-Markup, das angibt, wie die Datei " *Web. config* " bei der Bereitstellung geändert wird. Sie können unterschiedliche Änderungen für bestimmte Buildkonfigurationen und für bestimmte Veröffentlichungs profile angeben. Die Standardbuildkonfigurationen sind Debug und Release, und Sie können benutzerdefinierte Buildkonfigurationen erstellen. Ein Veröffentlichungs Profil entspricht in der Regel einer Zielumgebung. (Weitere Informationen zu Veröffentlichungs Profilen finden Sie im Tutorial bereitstellen [in IIS als Test Umgebung](deploying-to-iis.md) .)

Web deploy Parameter können verwendet werden, um viele verschiedene Arten von Einstellungen anzugeben, die während der Bereitstellung konfiguriert werden müssen, einschließlich der in *Web. config* -Dateien gefundenen Einstellungen. Wenn Sie zum Angeben von Änderungen an *Web. config* -Dateien verwendet werden, sind Web deploy Parameter komplexer einzurichten, Sie sind jedoch nützlich, wenn Sie den festzulegenden Wert erst nach der Bereitstellung kennen. In einer Unternehmensumgebung können Sie z. b. ein *Bereitstellungs Paket* erstellen und an eine Person in der IT-Abteilung übergeben, um Sie in der Produktionsumgebung zu installieren, und diese Person muss in der Lage sein, Verbindungs Zeichenfolgen oder Kenn Wörter einzugeben, die Sie nicht kennen.

In dem Szenario, das in dieser tutorialreihe behandelt wird, wissen Sie im Voraus alles, was Sie mit der Datei " *Web. config* " tun müssen, sodass Sie keine Web deploy Parameter verwenden müssen. Sie konfigurieren einige Transformationen, die sich je nach verwendeter Buildkonfiguration unterscheiden, und einige unterschiedliche Transformationen, je nachdem, welches Veröffentlichungs Profil verwendet wird.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Angeben von Web. config-Einstellungen in Azure

Wenn sich die Einstellungen für die *Web. config* -Datei, die Sie ändern möchten, in der `<connectionStrings>` oder im `<appSettings>` Element befinden und Sie die Bereitstellung für Web-Apps in Azure App Service ausführen, haben Sie eine weitere Option zum Automatisieren von Änderungen während der Bereitstellung. Sie können die Einstellungen, die in Azure wirksam werden sollen, auf der Registerkarte **Konfigurieren** der Verwaltungs Portalseite für Ihre Web-App eingeben (Scrollen Sie nach unten zu den Abschnitten **App-Einstellungen** und **Verbindungs** Zeichenfolgen). Wenn Sie das Projekt bereitstellen, wendet Azure die Änderungen automatisch an. Weitere Informationen finden Sie unter [Windows Azure-Websites: Funktionsweise von Anwendungs-und Verbindungs](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)Zeichenfolgen.

## <a name="default-transformation-files"></a>Standard Transformations Dateien

Erweitern Sie in **Projektmappen-Explorer**den Eintrag *Web. config* , um die Transformations Dateien *Web. Debug. config* und *Web. Release. config* anzuzeigen, die standardmäßig für die beiden Standardbuildkonfigurationen erstellt werden.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Sie können Transformations Dateien für benutzerdefinierte Buildkonfigurationen erstellen, indem Sie mit der rechten Maustaste auf die Datei Web. config klicken und **Konfigurations Transformationen hinzufügen** aus dem Kontextmenü auswählen. Für dieses Tutorial müssen Sie dies nicht tun, und die Menüoption ist deaktiviert, da Sie keine benutzerdefinierten Buildkonfigurationen erstellt haben.

Später erstellen Sie drei weitere Transformations Dateien, jeweils eine für die Veröffentlichungs profile "Test", "Staging" und "Produktion". Ein typisches Beispiel für eine Einstellung, die Sie in einer Transformations Datei für Veröffentlichungs profile behandeln würden, weil Sie von der Zielumgebung abhängt, ist ein WCF-Endpunkt, der für Tests und Produktion anders ist. Sie erstellen die Transformations Dateien für Veröffentlichungs Profile in späteren Tutorials, nachdem Sie die Veröffentlichungs Profile erstellt haben, mit denen Sie fortfahren.

## <a name="disable-debug-mode"></a>Debugmodus deaktivieren

Ein Beispiel für eine Einstellung, die von der Buildkonfiguration und nicht von der Zielumgebung abhängt, ist das `debug`-Attribut. Bei einem Releasebuild sollte das Debuggen unabhängig von der Umgebung, in der Sie bereitgestellt werden, deaktiviert werden. Daher erstellen die Visual Studio-Projektvorlagen standardmäßig *Web. Release. config* -Transformations Dateien mit Code, der das `debug`-Attribut aus dem `compilation`-Element entfernt. Im folgenden finden Sie die Standarddatei *Web. Release. config*: Zusätzlich zu einigen Beispielcode für die Transformation, der auskommentiert wird, enthält er Code im `compilation`-Element, das das `debug` Attribut entfernt:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

Das `xdt:Transform="RemoveAttributes(debug)"`-Attribut gibt an, dass das `debug` Attribut aus dem `system.web/compilation`-Element in der bereitgestellten *Web. config* -Datei entfernt werden soll. Dies erfolgt jedes Mal, wenn Sie einen Releasebuild bereitstellen.

## <a name="limit-error-log-access-to-administrators"></a>Beschränken des Zugriffs auf das Fehlerprotokoll auf Administratoren

Wenn ein Fehler auftritt, während die Anwendung ausgeführt wird, zeigt die Anwendung anstelle der vom System generierten Fehlerseite eine generische Fehlerseite an, und das [ELMAH-nuget-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) wird für die Fehler Protokollierung und Berichterstellung verwendet. Das `customErrors`-Element in der Datei " *Web. config* " der Anwendung gibt die Fehlerseite an:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Um die Fehlerseite anzuzeigen, ändern Sie vorübergehend das `mode`-Attribut des `customErrors`-Elements von "RemoteOnly" in "on", und führen Sie die Anwendung von Visual Studio aus. Verursacht einen Fehler, indem eine ungültige URL angefordert wird, z. b. " *studentsxxx. aspx*". Anstelle einer von IIS generierten Fehlerseite "die Ressource wurde nicht gefunden" wird die Seite " *GenericErrorPage. aspx* " angezeigt.

![Fehlerseite](web-config-transformations/_static/image2.png)

Um das Fehlerprotokoll anzuzeigen, ersetzen Sie alles in der URL nach der Portnummer mit *ELMAH. axd* (z. b. `http://localhost:51130/elmah.axd`), und drücken Sie die EINGABETASTE:

![ELMAH-Seite](web-config-transformations/_static/image3.png)

Vergessen Sie nicht, das `customErrors`-Element wieder auf den Modus "RemoteOnly" festzulegen, wenn Sie fertig sind.

Auf dem Entwicklungs Computer ist es praktisch, den kostenlosen Zugriff auf die Fehlerprotokoll Seite zu ermöglichen, aber in der Produktionsumgebung, die ein Sicherheitsrisiko darstellen würde. Für den Produktionsstandort möchten Sie eine Autorisierungs Regel hinzufügen, mit der der Zugriff auf das Fehlerprotokoll auf Administratoren beschränkt wird, und um sicherzustellen, dass die Einschränkung funktioniert, auch wenn Sie Sie in Test-und Stagingumgebungen benötigen. Dies ist eine weitere Änderung, die Sie bei jeder Bereitstellung eines Releasebuilds implementieren möchten. Daher gehört Sie zu der Datei " *Web. Release. config* ".

Öffnen Sie *Web. Release. config* , und fügen Sie unmittelbar vor dem schließenden `configuration`-Tag ein neues `location`-Element hinzu, wie hier gezeigt.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

Der `Transform`-Attribut Wert "Insert" bewirkt, dass dieses `location` Element allen vorhandenen `location` Elementen in der Datei " *Web. config* " als gleich geordnetes Element hinzugefügt wird. (Es gibt bereits ein `location` Element, das Autorisierungs Regeln für die Seite " **Update Guthaben** " angibt.)

Nun können Sie eine Vorschau der Transformation anzeigen, um sicherzustellen, dass Sie Sie richtig codiert haben.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *Web. Release. config* , und klicken Sie auf **Vorschau transformieren**.

![Menü "Vorschau transformieren"](web-config-transformations/_static/image4.png)

Daraufhin wird eine Seite geöffnet, auf der die *Web. config* -Entwicklungs Datei auf der linken Seite angezeigt wird, und die bereitgestellte *Web. config* -Datei wird auf der rechten Seite angezeigt, und die Änderungen werden hervorgehoben.

![Vorschau der Debug-Transformation](web-config-transformations/_static/image5.png)

![Vorschau der Location-Transformation](web-config-transformations/_static/image6.png)

(In der Vorschau stellen Sie möglicherweise einige zusätzliche Änderungen fest, für die Sie keine Transformationen geschrieben haben: Diese enthalten in der Regel das Entfernen von Leerraum, der die Funktionalität nicht beeinträchtigt.)

Wenn Sie den Standort nach der Bereitstellung testen, testen Sie auch, ob die Autorisierungs Regel gültig ist.

> [!NOTE] 
> 
> **Sicherheitshinweis** Zeigen Sie in einer Produktionsanwendung niemals Fehlerdetails der Öffentlichkeit an, oder speichern Sie diese Informationen an einem öffentlichen Speicherort. Angreifer können Fehlerinformationen verwenden, um Sicherheitsrisiken an einem Standort zu ermitteln. Wenn Sie ELMAH in ihrer eigenen Anwendung verwenden, konfigurieren Sie ELMAH, um Sicherheitsrisiken zu minimieren. Das ELMAH-Beispiel in diesem Tutorial sollte nicht als empfohlene Konfiguration betrachtet werden. Es ist ein Beispiel, das ausgewählt wurde, um zu veranschaulichen, wie ein Ordner behandelt werden kann, in dem die Anwendung Dateien erstellen muss. Weitere Informationen finden Sie unter [Sichern des ELMAH-Endpunkts](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Eine Einstellung, die Sie in Transformations Dateien für das Veröffentlichen von Profilen behandeln

Ein häufiges Szenario besteht darin, dass die Einstellungen der Datei " *Web. config* " in jeder Umgebung, in der Sie bereitgestellt werden, unterschiedlich sein müssen. Beispielsweise benötigt eine Anwendung, die einen WCF-Dienst aufruft, möglicherweise einen anderen Endpunkt in Test-und Produktionsumgebungen. Die Anwendung "" der Anwendung "" der Anwendung "" ist ebenfalls auf diese Art festgelegt. Mit dieser Einstellung wird ein sichtbarer Indikator auf den Seiten einer Site gesteuert, die Aufschluss darüber gibt, in welcher Umgebung Sie sich befinden (z. b. Entwicklung, Test oder Produktion). Der Einstellungs Wert bestimmt, ob die Anwendung "(dev)" oder "(Test)" an die Hauptüberschrift auf der Seite " *Site. Master* Master" anfügt:

![Umgebungs Indikator](web-config-transformations/_static/image7.png)

Der Umgebungs Indikator wird weggelassen, wenn die Anwendung in einer Staging-oder Produktionsumgebung ausgeführt wird.

Die Web Pages der conum so University haben einen Wert gelesen, der in `appSettings` in der Datei " *Web. config* " festgelegt ist, um zu bestimmen, in welcher Umgebung die Anwendung ausgeführt wird:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Der Wert muss "Test" in der Test Umgebung und "Prod" für Staging und Produktion lauten.

Mit dem folgenden Code in einer Transformations Datei wird diese Transformation implementiert:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Der `xdt:Transform`-Attribut Wert "", gibt an, dass der Zweck dieser Transformation darin besteht, Attributwerte eines vorhandenen Elements in der Datei " *Web. config* " zu ändern. Der `xdt:Locator`-Attribut Wert "Match (Key)" gibt an, dass das zu ändernde Element das Element ist, dessen `key` Attribut mit dem hier angegebenen `key`-Attribut übereinstimmt. Das einzige andere Attribut des `add`-Elements ist `value`, und das wird in der bereitgestellten *Web. config* -Datei geändert. Der hier gezeigte Code bewirkt, dass das `value`-Attribut des `Environment` `appSettings`-Elements in der bereitgestellten *Web. config* -Datei auf "Test" festgelegt wird.

Diese Transformation gehört zu den Transformations Dateien für Veröffentlichungs Profile, die Sie noch nicht erstellt haben. Sie erstellen und aktualisieren die Transformations Dateien, die diese Änderung implementieren, wenn Sie die Veröffentlichungs Profile für die Test-, Staging-und Produktionsumgebungen erstellen. Dies wird in den Tutorials bereitstellen [für IIS](deploying-to-iis.md) und bereitstellen [in der Produktion](deploying-to-production.md) durchführen.

> [!NOTE]
> Da diese Einstellung im `<appSettings>`-Element enthalten ist, haben Sie eine weitere Alternative zum Angeben der Transformation bei der Bereitstellung für Web-Apps in Azure App Service siehe [Angeben von Web. config-Einstellungen in Azure](#watransforms) weiter oben in diesem Thema.

## <a name="setting-connection-strings"></a>Festlegen von Verbindungszeichenfolgen

Obwohl die Standard Transformations Datei ein Beispiel enthält, das zeigt, wie eine Verbindungs Zeichenfolge aktualisiert wird, müssen Sie in den meisten Fällen keine Verbindungs Zeichenfolgen-Transformationen einrichten, da Sie Verbindungs Zeichenfolgen im Veröffentlichungs Profil angeben können. Dies wird in den Tutorials bereitstellen [für IIS](deploying-to-iis.md) und bereitstellen [in der Produktion](deploying-to-production.md) durchführen.

## <a name="summary"></a>Zusammenfassung

Mit Web. config-Transformationen haben Sie nun so viele Schritte ausgeführt, wie Sie es mit *Web. config* -Transformationen durchführen können, bevor Sie die Veröffentlichungs Profile erstellen, und Sie haben eine Vorschau der bereitgestellten Web. config-Datei gesehen.

![Vorschau der Location-Transformation](web-config-transformations/_static/image8.png)

Im folgenden Tutorial werden Bereitstellungs Einrichtungs Aufgaben behandelt, die das Festlegen von Projekteigenschaften erfordern.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden [Sie unter Verwenden von Web. config-Transformationen zum Ändern der Einstellungen in der Web. config-Zieldatei oder der app. config-Datei während der Bereitstellung](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) in der Webbereitstellungs-Inhalts Zuordnung für Visual Studio und ASP.net.

> [!div class="step-by-step"]
> [Zurück](preparing-databases.md)
> [Weiter](project-properties.md)
