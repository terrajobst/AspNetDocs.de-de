---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen zusätzlicher Dateien | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: eaa3141c22980f0c816e2f33b5597ac9fe69c23c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594904"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen zusätzlicher Dateien

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht über

In diesem Tutorial wird gezeigt, wie die Visual Studio-Webveröffentlichungs Pipeline erweitert wird, um während der Bereitstellung eine zusätzliche Aufgabe auszuführen. Der Task besteht darin, zusätzliche Dateien, die sich nicht im Projektordner befinden, auf die Zielwebsite zu kopieren.

In diesem Tutorial kopieren Sie eine zusätzliche Datei: *robots. txt*. Sie möchten diese Datei in der Stagingumgebung, aber nicht in der Produktionsumgebung bereitstellen. Im Tutorial Bereitstellung in der [Produktion](deploying-to-production.md) haben Sie diese Datei dem Projekt hinzugefügt und das Veröffentlichungs Profil für die Produktion so konfiguriert, dass Sie ausgeschlossen wird. In diesem Tutorial wird eine alternative Methode für diese Situation angezeigt, die für alle Dateien nützlich ist, die Sie bereitstellen möchten, aber nicht in das Projekt aufgenommen werden sollen.

## <a name="move-the-robotstxt-file"></a>Verschieben Sie die Datei "robots. txt".

Wenn Sie sich auf eine andere Methode zur Behandlung von " *robots. txt*" vorbereiten möchten, verschieben Sie die Datei in diesem Abschnitt des Tutorials in einen Ordner, der nicht im Projekt enthalten ist, und löschen Sie " *robots. txt* " aus der Stagingumgebung. Sie müssen die Datei aus der Staging-Datei löschen, damit Sie überprüfen können, ob die neue Methode zur Bereitstellung der Datei in dieser Umgebung ordnungsgemäß funktioniert.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Datei *robots. txt* , und klicken Sie dann auf **aus Projekt ausschließen**.
2. Erstellen Sie mit dem Windows-Datei-Explorer einen neuen Ordner im Projektmappenordner, und nennen Sie ihn *ExtraFiles*.
3. Verschieben Sie die Datei " *robots. txt* " aus dem Projektordner *condesouniversity* in den Ordner *ExtraFiles* .

    ![Ordner "ExtraFiles"](deploying-extra-files/_static/image1.png)
4. Löschen Sie die Datei " *robots. txt* " mithilfe Ihres FTP-Tools von der Stagingwebsite.

    Alternativ können Sie auf der Registerkarte Einstellungen des stagingprofils für die Veröffentlichung auf der Registerkarte **Einstellungen** die **Option** **Weitere Dateien am Ziel entfernen** auswählen und in Staging veröffentlichen.

## <a name="update-the-publish-profile-file"></a>Aktualisieren der Veröffentlichungs Profil Datei

Sie benötigen " *robots. txt* " nur in der Stagingumgebung, sodass das einzige Veröffentlichungs Profil, das Sie aktualisieren müssen, um es bereitzustellen, bereitgestellt wird.

1. Öffnen Sie in Visual Studio die Datei *Staging. pubxml*.
2. Fügen Sie am Ende der Datei vor dem schließenden `</Project>`-Tag das folgende Markup hinzu:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Dieser Code erstellt ein neues *Ziel* , das zusätzliche bereit zustellende Dateien sammelt. Ein Ziel besteht aus einem oder mehreren Tasks, die von MSBuild basierend auf den von Ihnen angegebenen Bedingungen ausgeführt werden.

    Das `Include`-Attribut gibt an, dass der Ordner, in dem die Dateien gesucht werden sollen, *ExtraFiles*ist, der sich auf derselben Ebene befindet wie der Projektordner. MSBuild sammelt alle Dateien aus diesem Ordner und rekursiv aus beliebigen Unterordnern (das doppelte Sternchen gibt rekursive Unterordner an). Mit diesem Code können Sie mehrere Dateien und Dateien in Unterordnern im Ordner " *ExtraFiles* " ablegen, und alle werden bereitgestellt.

    Das `DestinationRelativePath`-Element gibt an, dass die Ordner und Dateien in den Stamm Ordner der Zielwebsite in derselben Datei-und Ordnerstruktur kopiert werden sollen, wie Sie im Ordner " *ExtraFiles* " gefunden werden. Wenn Sie den Ordner *ExtraFiles* selbst kopieren möchten, ist der `DestinationRelativePath` Wert *ExtraFiles\%(RecursiveDir)% (Dateiname)% (Extension)* .
3. Fügen Sie am Ende der Datei vor dem schließenden `</Project>`-Tag das folgende Markup hinzu, das angibt, wann das neue Ziel ausgeführt werden soll.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Dieser Code bewirkt, dass das neue `CustomCollectFiles` Ziel immer dann ausgeführt wird, wenn das Ziel, das Dateien in den Zielordner kopiert, ausgeführt wird. Es gibt ein separates Ziel für das Veröffentlichen im Vergleich zur Erstellung des Bereitstellungs Pakets. das neue Ziel wird in beide Ziele eingefügt, wenn Sie sich für die Bereitstellung mithilfe eines Bereitstellungs Pakets anstelle der Veröffentlichung entscheiden.

    Die *pubxml* -Datei sieht nun wie im folgenden Beispiel aus:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Speichern und schließen Sie die Datei *Staging. pubxml* .

## <a name="publish-to-staging"></a>In Staging veröffentlichen

Veröffentlichen Sie die Anwendung mithilfe der One-Click-Veröffentlichung oder der Befehlszeile, indem Sie das Stagingprofil verwenden.

Wenn Sie die One-Click-Veröffentlichung verwenden, können Sie im **Vorschaufenster** überprüfen, ob " *robots. txt* " kopiert wird. Verwenden Sie andernfalls das FTP-Tool, um zu überprüfen, ob sich die Datei " *robots. txt* " im Stamm Ordner der Website nach der Bereitstellung befindet.

## <a name="summary"></a>Summary

Dies schließt diese Reihe von Tutorials zum Bereitstellen einer ASP.NET-Webanwendung für einen Drittanbieter-Hostinganbieter ab. Weitere Informationen zu den Themen, die in diesen Tutorials behandelt werden, finden Sie in der [ASP.net-Bereitstellungs Inhalts](https://go.microsoft.com/fwlink/p/?LinkId=282413)Zuordnung.

## <a name="more-information"></a>Weitere Informationen

Wenn Sie wissen, wie Sie mit MSBuild-Dateien arbeiten, können Sie viele andere Bereitstellungs Aufgaben automatisieren, indem Sie Code in *pubxml* -Dateien (für Profil spezifische Aufgaben) oder die Datei "Project *. WPP. targets* " (für Aufgaben, die für alle Profile gelten) schreiben. Weitere Informationen zu *. pubxml* -und *WPP. targets* -Dateien finden Sie unter Gewusst [wie: Bearbeiten von Bereitstellungs Einstellungen in Veröffentlichungs Profil Dateien (. pubxml) und in der Datei ". WPP. targets" in Visual Studio-Webprojekten](https://msdn.microsoft.com/library/ff398069). Eine grundlegende Einführung in MSBuild-Code finden Sie **unter Anatomie einer Projektdatei in der** [Enterprise-Bereitstellungs Reihe: Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Informationen zum Arbeiten mit MSBuild-Dateien zum Ausführen von Aufgaben für Ihre eigenen Szenarien finden Sie in diesem Buch: [innerhalb des Microsoft-Build-Engine: Verwenden von MSBuild und Team Foundation Build](http://msbuildbook.com) von Sayed Ibraham Hashimi und William Bartholomew.

## <a name="acknowledgements"></a>Danksagungen

Ich möchte den folgenden Personen danken, die bedeutende Beiträge zum Inhalt dieser tutorialreihe gemacht haben:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, Data Platform Development MVP, USA
- Harte Verpflichtung, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (Twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Papst, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Treffen Sie Srivastava, Microsoft
- [Raffaele rialdi, Italien](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(Twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (Twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (Twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbien](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (Twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Zurück](command-line-deployment.md)
> [Weiter](troubleshooting.md)
