---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Grundlegendes zur Projektdatei | Microsoft-Dokumentation
author: jrjlee
description: Microsoft-Build-Engine (MSBuild)-Projektdateien bilden das Herzstück des Build-und Bereitstellungs Prozesses. Dieses Thema beginnt mit einer konzeptionellen Übersicht über MSBuild...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 419fe51aaf65bddcc2c50380f099f842a8d9439c
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445690"
---
# <a name="understanding-the-project-file"></a>Grundlegendes zur Projektdatei

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Microsoft-Build-Engine (MSBuild)-Projektdateien bilden das Herzstück des Build-und Bereitstellungs Prozesses. Dieses Thema beginnt mit einer konzeptionellen Übersicht über MSBuild und die Projektdatei. Es beschreibt die wichtigsten Komponenten, die Sie bei der Arbeit mit Projektdateien finden werden, und zeigt anhand eines Beispiels, wie Sie Projektdateien verwenden können, um reale Anwendungen bereitzustellen.
> 
> Lernen Sie Folgendes:
> 
> - Die Verwendung von MSBuild-Projektdateien durch MSBuild zum Erstellen von Projekten.
> - Die Integration von MSBuild in Bereitstellungs Technologien, wie z. b. das Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy).
> - Erläutert, wie die Hauptkomponenten einer Projektdatei verstanden werden.
> - So können Sie mithilfe von Projektdateien komplexe Anwendungen erstellen und bereitstellen.

## <a name="msbuild-and-the-project-file"></a>MSBuild und die Projektdatei

Wenn Sie Projektmappen in Visual Studio erstellen und erstellen, verwendet Visual Studio MSBuild, um jedes Projekt in der Projekt Mappe zu erstellen. Jedes Visual Studio-Projekt enthält eine MSBuild-Projektdatei mit einer Dateierweiterung, die den Projekttyp&#x2014;, z C# . b. ein Projekt (. csproj), ein Visual Basic.net-Projekt (. vbproj) oder ein Datenbankprojekt (. dbproj), angibt. Um ein Projekt zu erstellen, muss MSBuild die Projektdatei verarbeiten, die dem Projekt zugeordnet ist. Die Projektdatei ist ein XML-Dokument, das alle Informationen und Anweisungen enthält, die MSBuild benötigt, um Ihr Projekt zu erstellen, wie z. b. den einzuschließende Inhalt, die Platt Form Anforderungen, Versionsinformationen, Webserver-oder Datenbankserver Einstellungen und die Tasks, die ausgeführt werden müssen.

MSBuild-Projektdateien basieren auf dem [XML-Schema von MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference). Folglich ist der Buildprozess vollständig offen und transparent. Außerdem müssen Sie Visual Studio nicht installieren, um die MSBuild-Engine&#x2014;zu verwenden. die ausführbare Datei "MSBuild. exe" ist Teil der .NET Framework, und Sie können Sie über eine Eingabeaufforderung ausführen. Als Entwickler können Sie mithilfe des MSBuild-XML-Schemas eigene MSBuild-Projektdateien erstellen, um eine ausgereifte und präzisere Kontrolle über die Erstellung und Bereitstellung Ihrer Projekte zu erzwingen. Diese benutzerdefinierten Projektdateien funktionieren genauso wie die Projektdateien, die von Visual Studio automatisch generiert werden.

> [!NOTE]
> Sie können auch MSBuild-Projektdateien mit dem Team Build-Dienst in Team Foundation Server (TFS) verwenden. Beispielsweise können Sie Projektdateien in Continuous Integration-Szenarios (CI) verwenden, um die Bereitstellung in einer Testumgebung zu automatisieren, wenn neuer Code eingeglichen wird. Weitere Informationen finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).

### <a name="project-file-naming-conventions"></a>Benennungs Konventionen für Projektdateien

Wenn Sie Ihre eigenen Projektdateien erstellen, können Sie eine beliebige Dateierweiterung verwenden. Damit Ihre Projektmappen für andere Benutzer leichter zu verstehen sind, sollten Sie jedoch die folgenden allgemeinen Konventionen verwenden:

- Verwenden Sie die Erweiterung ". proj", wenn Sie eine Projektdatei erstellen, mit der Projekte erstellt werden.
- Verwenden Sie die Erweiterung. targets, wenn Sie eine wiederverwendbare Projektdatei erstellen, um Sie in andere Projektdateien zu importieren. Dateien mit der Erweiterung. targets bauen in der Regel nichts selbst auf, da Sie lediglich Anweisungen enthalten, die Sie in Ihre proj-Dateien importieren können.

### <a name="integration-with-deployment-technologies"></a>Integration in Bereitstellungs Technologien

Wenn Sie mit Webanwendungs Projekten in Visual Studio 2010 gearbeitet haben, wie ASP.NET Webanwendungen und ASP.NET MVC-Webanwendungen, wissen Sie, dass diese Projekte integrierte Unterstützung für das Verpacken und Bereitstellen der Webanwendung in einer Zielumgebung enthalten. Die **Eigenschaften** Seiten für diese Projekte umfassen die Registerkarten " **Paket-/veröffentlichungsweb** " und " **Verpacken/veröffentlichen** ", mit denen Sie konfigurieren können, wie die Komponenten der Anwendung gepackt und bereitgestellt werden. Dadurch wird die Registerkarte " **Web packen/veröffentlichen** " angezeigt:

![](understanding-the-project-file/_static/image1.png)

Die zugrunde liegende Technologie hinter diesen Funktionen wird als Web Publishing Pipeline (WPP) bezeichnet. In WPP werden MSBuild und [Web deploy](https://go.microsoft.com/?linkid=9805122) im wesentlichen miteinander verknüpft, um einen kompletten Build-, Paket-und Bereitstellungs Prozess für Ihre Webanwendungen bereitzustellen.

Die gute Nachricht ist, dass Sie die Integrationspunkte nutzen können, die die WPP beim Erstellen benutzerdefinierter Projektdateien für Webprojekte bereitstellt. Sie können Bereitstellungs Anweisungen in die Projektdatei einschließen, mit der Sie Ihre Projekte erstellen, Webbereitstellungs Pakete erstellen und diese Pakete auf Remote Servern über eine einzelne Projektdatei und einen einzelnen MSBuild-Client installieren können. Sie können auch beliebige andere ausführbare Dateien als Teil des Buildprozesses aufzurufen. Beispielsweise können Sie das Befehlszeilen Tool "VSDBCMD. exe" ausführen, um eine Datenbank aus einer Schema Datei bereitzustellen. Im Rahmen dieses Themas erfahren Sie, wie Sie diese Funktionen nutzen können, um die Anforderungen Ihrer Szenarien für die Unternehmens Bereitstellung zu erfüllen.

> [!NOTE]
> Weitere Informationen zur Funktionsweise des Bereitstellungs Prozesses für die Webanwendung finden Sie unter [Übersicht über die Projekt Bereitstellung ASP.NET Webanwendungen](https://msdn.microsoft.com/library/dd394698.aspx).

## <a name="the-anatomy-of-a-project-file"></a>Die Anatomie einer Projektdatei

Bevor Sie den Buildprozess ausführlicher betrachten, ist es sinnvoll, sich mit der grundlegenden Struktur einer MSBuild-Projektdatei vertraut zu machen. Dieser Abschnitt enthält eine Übersicht über die gängigeren Elemente, die beim Überprüfen, bearbeiten oder Erstellen einer Projektdatei auftreten. Insbesondere lernen Sie Folgendes:

- Verwenden von *Eigenschaften* zum Verwalten von Variablen für den Buildprozess.
- Gewusst wie: Verwenden von *Elementen* zum Identifizieren der Eingaben für den Buildprozess, wie z. b. Code Dateien.
- Verwenden von *Zielen* und *Aufgaben* zum Bereitstellen von Ausführungs Anweisungen für MSBuild mithilfe von *Eigenschaften* und *Elementen* , die an anderer Stelle in der Projektdatei definiert sind.

Dadurch wird die Beziehung zwischen den Schlüsselelementen in einer MSBuild-Projektdatei angezeigt:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Das Project-Element

Das [Project](https://msdn.microsoft.com/library/bcxfsh87.aspx) -Element ist das Stamm Element jeder Projektdatei. Zusätzlich zur Identifizierung des XML-Schemas für die Projektdatei kann das **Project** -Element Attribute enthalten, mit denen die Einstiegspunkte für den Buildprozess angegeben werden. Beispielsweise gibt die Datei *Publish. proj* in der [Beispiellösung Contact Manager](the-contact-manager-solution.md)an, dass der Build beginnen soll, indem das Ziel mit dem Namen **fullpublish**aufgerufen wird.

[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]

### <a name="properties-and-conditions"></a>Eigenschaften und Bedingungen

Eine Projektdatei muss in der Regel viele verschiedene Informationen bereitstellen, um Ihre Projekte erfolgreich erstellen und bereitstellen zu können. Diese Informationen können Servernamen, Verbindungs Zeichenfolgen, Anmelde Informationen, Buildkonfigurationen, Quell-und Ziel Dateipfade sowie weitere Informationen enthalten, die Sie zur Unterstützung der Anpassung einschließen möchten. In einer Projektdatei müssen Eigenschaften innerhalb eines [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) -Elements definiert werden. MSBuild-Eigenschaften bestehen aus Schlüssel-Wert-Paaren. Innerhalb des **PropertyGroup** -Elements definiert der Elementname den Eigenschafts Schlüssel, und der Inhalt des Elements definiert den Eigenschafts Wert. Beispielsweise können Sie Eigenschaften mit dem Namen " **Server** Name" und " **ConnectionString** " definieren, um einen statischen Servernamen und eine Verbindungs Zeichenfolge zu speichern.

[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]

Um einen Eigenschafts Wert abzurufen, verwenden Sie das Format *$ (PropertyName)* . Wenn Sie z. b. den Wert der **Servername** -Eigenschaft abrufen möchten, geben Sie Folgendes ein:

[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]

> [!NOTE]
> Beispiele für die Verwendung von Eigenschafts Werten finden Sie weiter unten in diesem Thema.

Das Einbetten von Informationen als statische Eigenschaften in einer Projektdatei ist nicht immer die ideale Methode zum Verwalten des Buildprozesses. In vielen Szenarien sollten Sie die Informationen aus anderen Quellen abrufen oder den Benutzer in die Lage versetzen, die Informationen über die Eingabeaufforderung bereitzustellen. Mit MSBuild können Sie einen beliebigen Eigenschafts Wert als Befehlszeilenparameter angeben. Der Benutzer könnte z. b. einen Wert für **Servername** angeben, wenn er MSBuild. exe über die Befehlszeile ausführt.

[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]

> [!NOTE]
> Weitere Informationen zu den Argumenten und Schaltern, die Sie mit MSBuild. exe verwenden können, finden Sie unter [MSBuild-Befehlszeilen Referenz](https://msdn.microsoft.com/library/ms164311.aspx).

Sie können dieselbe Eigenschaften Syntax verwenden, um die Werte von Umgebungsvariablen und integrierten Projekteigenschaften zu erhalten. Viele häufig verwendete Eigenschaften werden für Sie definiert, und Sie können Sie in ihren Projektdateien verwenden, indem Sie den entsprechenden Parameternamen einschließen. Wenn Sie z. b. die aktuelle Projekt&#x2014;Plattform (z. b. **x86** oder **anycpu**&#x2014;) abrufen möchten, können Sie den **$ (Platform)** -Eigenschafts Verweis in Ihre Projektdatei einschließen. Weitere Informationen finden Sie unter [Makros für Buildbefehle und-Eigenschaften](https://msdn.microsoft.com/library/c02as0cs.aspx), [allgemeine MSBuild-Projekteigenschaften](https://msdn.microsoft.com/library/bb629394.aspx)und [reservierte Eigenschaften](https://msdn.microsoft.com/library/ms164309.aspx).

Eigenschaften werden häufig in Verbindung mit *Bedingungen*verwendet. Die meisten MSBuild-Elemente unterstützen das **Condition** -Attribut, mit dem Sie die Kriterien angeben können, nach denen MSBuild das Element auswerten soll. Beachten Sie z. b. die folgende Eigenschafts Definition:

[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]

Wenn MSBuild diese Eigenschafts Definition verarbeitet, prüft Sie zunächst, ob ein **$ (outputroot)** -Eigenschafts Wert verfügbar ist. Wenn der-Eigenschafts Wert&#x2014;nicht leer ist, hat der Benutzer keinen Wert für diese Eigenschaft&#x2014;angegeben, die Bedingung ergibt **true** , und der-Eigenschafts Wert **ist auf festgelegt. \Publish\out**. Wenn der Benutzer einen Wert für diese Eigenschaft angegeben hat, wird die Bedingung als **false** ausgewertet, und der Wert der statischen Eigenschaft wird nicht verwendet.

Weitere Informationen zu den verschiedenen Methoden, mit denen Sie Bedingungen angeben können, finden Sie unter [MSBuild-Bedingungen](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elemente und Elementgruppen

Eine der wichtigsten Rollen der Projektdatei ist das Definieren der Eingaben für den Buildprozess. In der Regel sind diese Eingaben&#x2014;Dateien Code Dateien, Konfigurationsdateien, Befehls Dateien und andere Dateien, die Sie als Teil des Buildprozesses verarbeiten oder kopieren müssen. Im MSBuild-Projekt Schema werden diese Eingaben durch [Element](https://msdn.microsoft.com/library/ms164283.aspx) Elemente dargestellt. In einer Projektdatei müssen Elemente innerhalb eines [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) -Elements definiert werden. Ebenso wie **Eigenschafts** Elemente können **Sie ein Element** für Sie beliebig benennen. Sie müssen jedoch ein **include** -Attribut angeben, um die Datei oder den Platzhalter zu identifizieren, die das Element darstellt.

[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]

Wenn Sie mehrere **Element** Elemente mit dem gleichen Namen angeben, erstellen Sie effektiv eine benannte Liste von Ressourcen. Eine gute Möglichkeit, dies in Aktion zu sehen, besteht darin, eine der Projektdateien zu betrachten, die Visual Studio erstellt. Beispielsweise enthält die Datei *ContactManager. MVC. csproj* in der Beispiellösung viele Elementgruppen, von denen jede über mehrere identisch benannte **Element** Elemente verfügt.

[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]

Auf diese Weise weist die Projektdatei MSBuild an, Listen von Dateien zu erstellen, die auf die gleiche Weise verarbeitet werden müssen,&#x2014;wie die **Verweis** Liste Assemblys enthält, die für einen erfolgreichen Build vorhanden sein müssen, die **Kompilierungs** Liste enthält Code Dateien, die kompiliert werden müssen, und die **Inhalts** Liste enthält Ressourcen, die unverändert kopiert werden müssen. Wir sehen uns an, wie der Buildprozess später in diesem Thema auf diese Elemente verweist und diese verwendet.

Element Elemente können auch untergeordnete [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) -Elemente enthalten. Dabei handelt es sich um benutzerdefinierte Schlüssel-Wert-Paare, die im wesentlichen Eigenschaften darstellen, die für dieses Element spezifisch sind. Beispielsweise enthalten viele Elemente der **Kompilierungs** Elemente in der Projektdatei **abhängige abhängige** Elemente.

[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]

> [!NOTE]
> Zusätzlich zu den Metadaten, die vom Benutzer erstellt wurden, werden allen Elementen bei der Erstellung verschiedene allgemeine Metadaten zugewiesen. Weitere Informationen finden Sie unter [Well-known Item Metadata (Bekannte Elementmetadaten)](https://msdn.microsoft.com/library/ms164313.aspx).

Sie können **ItemGroup** -Elemente innerhalb des **Projekt** Elements auf der Stamm Ebene oder innerhalb bestimmter **Ziel** Elemente erstellen. **ItemGroup** -Elemente unter **stützen auch** Bedingungs Attribute, mit denen Sie die Eingaben für den Buildprozess entsprechend den Bedingungen wie der Projekt Konfiguration oder der Plattform anpassen können.

### <a name="targets-and-tasks"></a>Ziele und Aufgaben

Im MSBuild-Schema stellt ein [Task](https://msdn.microsoft.com/library/77f2hx1s.aspx) -Element eine einzelne buildanweisung (oder Aufgabe) dar. MSBuild umfasst eine Vielzahl von vordefinierten Tasks. Beispiel:

- Der **Kopier** Task kopiert Dateien an einen neuen Speicherort.
- Die **csc** -Aufgabe ruft den C# visuellen Compiler auf.
- Der **vbc** -Task ruft den Visual Basic-Compiler auf.
- Der **exec** -Task führt ein angegebenes Programm aus.
- Der **Message** -Task schreibt eine Meldung in eine Protokollierung.

> [!NOTE]
> Ausführliche Informationen zu den Aufgaben, die standardmäßig verfügbar sind, finden Sie unter [MSBuild](https://msdn.microsoft.com/library/7z253716.aspx)-Aufgaben Referenz. Weitere Informationen zu Aufgaben, einschließlich der Erstellung eigener benutzerdefinierter Tasks, finden Sie unter [MSBuild-Aufgaben](https://msdn.microsoft.com/library/ms171466.aspx).

Aufgaben müssen immer in [Ziel](https://msdn.microsoft.com/library/t50z2hka.aspx) Elementen enthalten sein. Ein **Ziel** Element ist ein Satz von einer oder mehreren Aufgaben, die sequenziell ausgeführt werden, und eine Projektdatei kann mehrere Ziele enthalten. Wenn Sie einen Task oder eine Reihe von Tasks ausführen möchten, rufen Sie das Ziel auf, das Sie enthält. Nehmen Sie beispielsweise an, Sie verfügen über eine einfache Projektdatei, die eine Meldung protokolliert.

[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]

Sie können das Ziel über die Befehlszeile aufrufen, indem Sie den **/t** -Schalter verwenden, um das Ziel anzugeben.

[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]

Alternativ können Sie dem **Project** -Element ein **DefaultTargets** -Attribut hinzufügen, um die Ziele anzugeben, die Sie aufrufen möchten.

[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]

In diesem Fall müssen Sie das Ziel nicht in der Befehlszeile angeben. Sie können einfach die Projektdatei angeben, und MSBuild Ruft das **fullpublish** -Ziel für Sie auf.

[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]

Beide Ziele und Aufgaben können Bedingungs **Attribute einschließen** . Daher können Sie auswählen, ob Sie vollständige Ziele oder einzelne Aufgaben weglassen, wenn bestimmte Bedingungen erfüllt sind.

Im Allgemeinen müssen Sie, wenn Sie nützliche Tasks und Ziele erstellen, auf die Eigenschaften und Elemente verweisen, die Sie an anderer Stelle in der Projektdatei definiert haben:

- Um einen Eigenschafts Wert zu verwenden, geben Sie **$ (***propertyName***)** ein, wobei *propertyName* der Name des **Property** -Elements oder der Name des Parameters ist.
- Wenn Sie ein Element verwenden möchten, geben Sie **@ (***ItemName***)** ein, wobei *ItemName* der Name des **Item** -Elements ist.

> [!NOTE]
> Beachten Sie, dass Sie eine Liste erstellen, wenn Sie mehrere Elemente mit demselben Namen erstellen. Wenn Sie dagegen mehrere Eigenschaften mit demselben Namen erstellen, überschreibt der letzte von Ihnen bereitgestellte Eigenschafts Wert alle vorherigen Eigenschaften mit demselben Namen&#x2014;. eine Eigenschaft darf nur einen einzelnen Wert enthalten.

Betrachten Sie beispielsweise in der Datei " *Publish. proj* " in der Beispiellösung das Ziel " **buildprojects** ".

[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]

In diesem Beispiel können Sie diese wichtigen Punkte beachten:

- Wenn der **buildinginteambuild** -Parameter angegeben wird und den Wert **true**aufweist, wird keine der Tasks innerhalb dieses Ziels ausgeführt.
- Das Ziel enthält eine einzelne Instanz der [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) -Aufgabe. Mit diesem Task können Sie andere MSBuild-Projekte erstellen.
- Das Element **projectstobuild** wird an die Aufgabe übermittelt. Dieses Element kann eine Liste von Projekt-oder Projektmappendateien darstellen, die alle durch **projectstobuild** -Element Elemente innerhalb einer Element Gruppe definiert sind. In diesem Fall verweist das **projectstobuild** -Element auf eine einzelne Projektmappendatei.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Die an die **MSBuild** -Aufgabe übergebenen Eigenschaftswerte enthalten Parameter namens " **outputroot** " und " **Configuration**". Diese werden auf Parameterwerte festgelegt, wenn Sie bereitgestellt werden, oder auf statische Eigenschaftswerte, wenn Sie nicht vorhanden sind.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Sie können auch sehen, dass die **MSBuild** -Aufgabe ein Ziel namens " **Build**" aufruft. Dies ist eines von mehreren integrierten Zielen, die in Visual Studio-Projektdateien häufig verwendet werden und Ihnen in Ihren benutzerdefinierten Projektdateien verfügbar **sind, wie**z. b. Build, **Bereinigung**, **Neuerstellung**und **Veröffentlichung**. Weiter unten in diesem Thema erfahren Sie mehr über die Verwendung von Zielen und Aufgaben zum Steuern des Buildprozesses und über die **MSBuild** -Aufgabe.

> [!NOTE]
> Weitere Informationen zu Zielen finden Sie unter [MSBuild-Ziele](https://msdn.microsoft.com/library/ms171462.aspx).

## <a name="splitting-project-files-to-support-multiple-environments"></a>Aufteilen von Projektdateien zur Unterstützung mehrerer Umgebungen

Angenommen, Sie möchten eine Lösung in mehreren Umgebungen bereitstellen können, wie z. b. Testserver, stagingplattformen und Produktionsumgebungen. Die Konfiguration kann sich zwischen diesen Umgebungen&#x2014;nicht nur im Hinblick auf Servernamen, Verbindungs Zeichenfolgen usw. unterscheiden, sondern auch in Bezug auf Anmelde Informationen, Sicherheitseinstellungen und viele andere Faktoren. Wenn Sie dies regelmäßig durchführen müssen, ist es nicht sinnvoll, mehrere Eigenschaften in der Projektdatei jedes Mal zu bearbeiten, wenn Sie die Zielumgebung wechseln. Es ist auch keine ideale Lösung, eine unendliche Liste von Eigenschafts Werten für den Buildprozess zu benötigen.

Glücklicherweise gibt es eine Alternative. Mit MSBuild können Sie die Buildkonfiguration auf mehrere Projektdateien aufteilen. Um zu sehen, wie dies funktioniert, beachten Sie in der Beispiellösung, dass zwei benutzerdefinierte Projektdateien vorhanden sind:

- *Publish. proj*, das Eigenschaften, Elemente und Ziele enthält, die für alle Umgebungen gelten.
- *Env-dev. proj*, das Eigenschaften enthält, die spezifisch für eine Entwicklerumgebung sind.

Beachten Sie nun, dass die *Publish. proj* -Datei ein [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) -Element direkt unterhalb des öffnenden **projekttags** enthält.

[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]

Das **Import** -Element wird verwendet, um den Inhalt einer anderen MSBuild-Projektdatei in die aktuelle MSBuild-Projektdatei zu importieren. In diesem Fall gibt der **targetenvpropsfile** -Parameter den Dateinamen der Projektdatei an, die Sie importieren möchten. Sie können einen Wert für diesen Parameter angeben, wenn Sie MSBuild ausführen.

[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]

Dadurch wird der Inhalt der beiden Dateien effektiv in eine einzelne Projektdatei zusammengeführt. Mit diesem Ansatz können Sie eine Projektdatei erstellen, die ihre universelle Buildkonfiguration und mehrere ergänzende Projektdateien enthält, die Umgebungs spezifische Eigenschaften enthalten. Daher können Sie durch einfaches Ausführen eines Befehls mit einem anderen Parameterwert Ihre Lösung in einer anderen Umgebung bereitstellen.

![](understanding-the-project-file/_static/image3.png)

Wenn Sie Ihre Projektdateien auf diese Weise aufteilen, empfiehlt es sich, zu folgen. Dadurch können Entwickler in mehreren Umgebungen bereitstellen, indem Sie einen einzelnen Befehl ausführen und gleichzeitig die Duplizierung universeller Buildeigenschaften über mehrere Projektdateien hinweg vermeiden.

> [!NOTE]
> Anleitungen zum Anpassen der Umgebungs spezifischen Projektdateien für Ihre eigenen Serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungs Eigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="conclusion"></a>Schlussfolgerung

Dieses Thema bietet eine allgemeine Einführung in MSBuild-Projektdateien und erläutert, wie Sie eigene benutzerdefinierte Projektdateien erstellen können, um den Buildprozess zu steuern. Außerdem wurde das Konzept der Aufteilung von Projektdateien in universelle Buildanweisungen und Umgebungs spezifische Buildeigenschaften eingeführt, um das Erstellen und Bereitstellen von Projekten für mehrere Ziele zu vereinfachen.

Das nächste Thema, [das Verständnis des Buildprozesses](understanding-the-build-process.md), bietet einen besseren Einblick in die Verwendung von Projektdateien zum Steuern von Build und Bereitstellung, indem Sie die Bereitstellung einer Lösung mit einem realistischen Maß an Komplexität durchlaufen.

## <a name="further-reading"></a>Weiterführende Themen

Eine ausführlichere Einführung in Projektdateien und WPP finden Sie unter [in der Microsoft-Build-Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) von Sayed Ibrahim Hashimi und William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](setting-up-the-contact-manager-solution.md)
> [Weiter](understanding-the-build-process.md)
