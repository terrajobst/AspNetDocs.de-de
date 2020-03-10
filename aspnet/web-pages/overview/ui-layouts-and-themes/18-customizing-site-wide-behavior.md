---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Anpassen des Standort weiten Verhaltens für ASP.net Web Pages (Razor) Websites | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Sie die Einstellungen für Ihre gesamte Website oder einen vollständigen Ordner anstatt nur für eine Seite erstellen.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: f05e05f725d9209bce283ce18659ae5efe4de2ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515247"
---
# <a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Anpassen des Standort weiten Verhaltens für ASP.net Web Pages-Websites (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie Standort seitige Einstellungen für Seiten auf einer ASP.net Web Pages-Website (Razor) erstellen.
> 
> Sie lernen Folgendes:
> 
> - Ausführen von Code, mit dem Sie Werte für alle Seiten einer Site festlegen können (globale Werte oder Hilfseinstellungen).
> - Ausführen von Code, mit dem Sie Werte für alle Seiten in einem Ordner festlegen können.
> - Ausführen von Code vor und nach dem Laden einer Seite.
> - Vorgehensweise beim Senden von Fehlern an eine zentrale Fehlerseite
> - Vorgehensweise beim Hinzufügen von Authentifizierung zu allen Seiten in einem Ordner.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - Webmatrix 3
> - ASP.net-webhilfsprogramme-Bibliothek (nuget-Paket)
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 3 und Visual Studio 2013 (oder Visual Studio Express 2013 für Web), mit der Ausnahme, dass Sie nicht die ASP.net-webhilfsprogramme-Bibliothek verwenden können.

<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Startcode für Website wird hinzugefügt für ASP.net Web Pages

Für einen Großteil des Codes, den Sie in ASP.net Web Pages schreiben, kann eine einzelne Seite den gesamten Code enthalten, der für diese Seite erforderlich ist. Wenn eine Seite z. b. eine e-Mail-Nachricht sendet, können Sie den gesamten Code für diesen Vorgang auf einer einzelnen Seite ablegen. Dies kann den Code zum Initialisieren der Einstellungen für das Senden von e-Mails (d. h. für den SMTP-Server) und das Senden der e-Mail-Nachricht einschließen.

In einigen Fällen möchten Sie jedoch vielleicht Code ausführen, bevor eine Seite auf der Website ausgeführt wird. Dies ist nützlich, um Werte festzulegen, die an beliebiger Stelle auf der Website verwendet werden können (als *globale Werte*bezeichnet). Beispielsweise erfordern einige Hilfsprogramme, dass Sie Werte wie e-Mail-Einstellungen oder Kontoschlüssel angeben. Es kann nützlich sein, diese Einstellungen in globalen Werten zu speichern.

Erstellen Sie hierzu im Stammverzeichnis der Website eine Seite mit dem Namen *\_AppStart. cshtml* . Wenn diese Seite vorhanden ist, wird Sie ausgeführt, wenn eine Seite der Website zum ersten Mal angefordert wird. Daher ist es ein guter Ausgangspunkt, Code zum Festlegen globaler Werte auszuführen. (Da *\_AppStart. cshtml* ein Unterstrich Präfix enthält, sendet ASP.net die Seite auch dann nicht an einen Browser, wenn Sie von den Benutzern direkt angefordert werden.)

Das folgende Diagramm zeigt, wie die *\_AppStart. cshtml* -Seite funktioniert. Wenn eine Anforderung für eine Seite angezeigt wird, und wenn dies die erste Anforderung für eine beliebige Seite in der Site ist, überprüft ASP.net zuerst, ob eine *\_AppStart. cshtml* -Seite vorhanden ist. Wenn dies der Fall ist, wird der Code auf der *\_AppStart. cshtml* -Seite ausgeführt, und dann wird die angeforderte Seite ausgeführt.

![Klang](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Festlegen globaler Werte für Ihre Website

1. Erstellen Sie im Stamm Ordner einer webmatrix-Website eine Datei mit dem Namen *\_AppStart. cshtml*. Die Datei muss sich im Stammverzeichnis der Website befinden.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    In diesem Code wird ein Wert im `AppState` Wörterbuch gespeichert, das für alle Seiten der Site automatisch verfügbar ist. Beachten Sie, dass die *\_AppStart. cshtml* -Datei kein Markup enthält. Die Seite führt den Code aus und leitet dann zu der ursprünglich angeforderten Seite um.

    > [!NOTE]
    > Gehen Sie vorsichtig vor, wenn Sie Code in der *\_AppStart. cshtml* -Datei ablegen. Wenn Fehler im Code in der *\_AppStart. cshtml* -Datei auftreten, wird die Website nicht gestartet.
3. Erstellen Sie im Stamm Ordner eine neue Seite mit dem Namen *appname. cshtml*.
4. Ersetzen Sie das Standard Markup und den Code durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Mit diesem Code wird der Wert aus dem `AppState`-Objekt extrahiert, das Sie auf der Seite *\_AppStart. cshtml* festgelegt haben.
5. Führen Sie die Seite *appname. cshtml* in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Auf der Seite wird der globale Wert angezeigt. 

    ![Klang](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Festlegen von Werten für Hilfsprogramme

Eine gute Verwendung für die *\_AppStart. cshtml* -Datei ist die Festlegung von Werten für Hilfsprogramme, die Sie an Ihrer Website verwenden und die initialisiert werden müssen. Typische Beispiele hierfür sind e-Mail-Einstellungen für die `WebMail`-Hilfsprogramm und die privaten und öffentlichen Schlüssel für das `ReCaptcha`-Hilfsprogramm. In solchen Fällen können Sie die Werte einmal in der *\_AppStart. cshtml* festlegen und sind dann für alle Seiten auf Ihrer Website bereits festgelegt.

In diesem Verfahren wird gezeigt, wie Sie `WebMail` Einstellungen global festlegen. (Weitere Informationen zur Verwendung des `WebMail`-Hilfsprogramms finden Sie unter [Hinzufügen von e-Mail zu einer ASP.net Web Pages Website](../getting-started/11-adding-email-to-your-web-site.md).)

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, wenn Sie Sie noch nicht hinzugefügt haben.
2. Wenn Sie nicht bereits über eine *\_AppStart. cshtml* -Datei verfügen, erstellen Sie im Stamm Ordner einer Website eine Datei mit dem Namen *\_AppStart. cshtml*.
3. Fügen Sie der *\_AppStart. cshtml* -Datei die folgenden `WebMail` Einstellungen hinzu: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Ändern Sie die folgenden e-Mail-bezogenen Einstellungen im Code:

   - Legen Sie `your-SMTP-host` auf den Namen des SMTP-Servers fest, auf den Sie Zugriff haben.
   - Legen Sie `your-user-name-here` auf den Benutzernamen für Ihr SMTP-Server Konto fest.
   - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server Konto fest.
   - Legen Sie `your-email-address-here` auf Ihre e-Mail-Adresse fest. Dies ist die e-Mail-Adresse, von der die Nachricht gesendet wird. (Bei einigen e-Mail-Anbietern ist es nicht möglich, eine andere `From` Adresse anzugeben, und der Benutzername wird als `From` Adresse verwendet.)

     Weitere Informationen zu SMTP-Einstellungen finden Sie [unter Konfigurieren von e](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) -Mail-Einstellungen im Artikel [Senden von e-Mail von einer ASP.net Web Pages-Website (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) und [Problemen beim Senden von e-Mails](https://go.microsoft.com/fwlink/?LinkId=253001#email) im Handbuch zur Problembehandlung bei [ASP.net Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Speichern Sie die Datei *\_AppStart. cshtml* , und schließen Sie Sie.
5. Erstellen Sie im Stamm Ordner einer Website eine neue Seite mit dem Namen *Testemail. cshtml*.
6. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Führen Sie die Seite " *Testemail. cshtml* " in einem Browser aus.
8. Füllen Sie die Felder aus, um sich selbst eine e-Mail zu senden, und klicken Sie dann auf **senden**.
9. Überprüfen Sie Ihre e-Mail, um sicherzustellen, dass die Nachricht abgerufen wurde.

Der wichtige Teil dieses Beispiels ist, dass die Einstellungen, die Sie in der Regel nicht ändern – wie der Name des SMTP-Servers und Ihre e-Mail-Anmelde Informationen – in der *\_AppStart. cshtml* -Datei festgelegt werden. Auf diese Weise müssen Sie Sie nicht erneut auf jeder Seite festlegen, an die Sie eine e-Mail senden. (Obwohl Sie aus irgendeinem Grund diese Einstellungen ändern müssen, können Sie sie einzeln auf einer Seite festlegen.) Auf der Seite legen Sie nur die Werte fest, die normalerweise jedes Mal geändert werden, z. b. den Empfänger und den Text der e-Mail-Nachricht.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Ausführen von Code vor und nach Dateien in einem Ordner

Ebenso wie Sie *\_AppStart. cshtml* zum Schreiben von Code verwenden können, bevor Seiten auf der Website ausgeführt werden, können Sie Code schreiben, der vor (und nach) einer beliebigen Seite in einem bestimmten Ordner ausgeführt wird. Dies ist hilfreich, wenn die gleiche Layoutseite für alle Seiten in einem Ordner festgelegt wird, oder um zu überprüfen, ob ein Benutzer angemeldet ist, bevor eine Seite im Ordner ausgeführt wird.

Für Seiten in bestimmten Ordnern können Sie Code in einer Datei mit dem Namen *\_pagestart. cshtml*erstellen. Das folgende Diagramm zeigt, wie die Seite *\_pagestart. cshtml* funktioniert. Wenn eine Anforderung für eine Seite eingehen soll, sucht ASP.net zuerst nach einer *\_AppStart. cshtml* -Seite und führt diese aus. Dann überprüft ASP.net, ob eine *\_Page Start. cshtml* -Seite vorhanden ist. ist dies der Fall, wird diese ausgeführt. Anschließend wird die angeforderte Seite ausgeführt.

Auf der Seite *\_pagestart. cshtml* können Sie angeben, wohin die angeforderte Seite während der Verarbeitung durch Einschließen einer `RunPage` Methode ausgeführt werden soll. Auf diese Weise können Sie Code ausführen, bevor die angeforderte Seite ausgeführt wird, und dann erneut darauf klicken. Wenn Sie `RunPage`nicht einschließen, wird der gesamte Code in *\_pagestart. cshtml* ausgeführt, und anschließend wird die angeforderte Seite automatisch ausgeführt.

![Klang](18-customizing-site-wide-behavior/_static/image3.jpg)

Mit ASP.net können Sie eine Hierarchie von *\_pagestart. cshtml* -Dateien erstellen. Sie können eine *\_pagestart. cshtml* -Datei im Stammverzeichnis der Website und in einem beliebigen Unterordner platzieren. Wenn eine Seite angefordert wird, wird die *\_pagestart. cshtml* -Datei auf der obersten Ebene (am nächsten zum Stammverzeichnis der Website) ausgeführt, gefolgt von der *\_pagestart. cshtml* -Datei im nächsten Unterordner usw., bis die Anforderung den Ordner erreicht, der die angeforderte Seite enthält. Nachdem alle anwendbaren *\_pagestart. cshtml* -Dateien ausgeführt wurden, wird die angeforderte Seite ausgeführt.

Beispielsweise können Sie die folgende Kombination aus *\_Page Start. cshtml* -Dateien und der Datei " *default. cshtml* " aufweisen:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Wenn Sie */MyFolder/default.cshtml*ausführen, wird Folgendes angezeigt:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Ausführen des Initialisierungs Codes für alle Seiten in einem Ordner

Eine gute Verwendung für *\_pagestart. cshtml* -Dateien ist die Initialisierung derselben Layoutseite für alle Dateien in einem einzelnen Ordner.

1. Erstellen Sie im Stamm Ordner einen neuen Ordner mit dem Namen *initpages*.
2. Erstellen Sie im Ordner *initpages* Ihrer Website eine Datei mit dem Namen *\_pagestart. cshtml* , und ersetzen Sie das Standard Markup und den Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Erstellen Sie im Stammverzeichnis der Website einen Ordner mit dem Namen *Shared*.
4. Erstellen Sie im frei *gegebenen* Ordner eine Datei mit dem Namen *\_layout1. cshtml* , und ersetzen Sie das Standard Markup und den Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. Erstellen Sie im Ordner *initpages* eine Datei mit dem Namen *Content1. cshtml* , und ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. Erstellen Sie im Ordner *initpages* eine weitere Datei mit dem Namen *Content2. cshtml* , und ersetzen Sie das Standard Markup durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Führen Sie *Content1. cshtml* in einem Browser aus. 

    ![Klang](18-customizing-site-wide-behavior/_static/image4.jpg)

    Wenn die *Content1. cshtml* -Seite ausgeführt wird, legt die *\_pagestart. cshtml* -Datei `Layout` fest und legt `PageData["MyBackground"]` auf eine Farbe fest. In *Content1. cshtml*werden Layout und Farbe angewendet.
8. Anzeigen von *Content2. cshtml* in einem Browser. 

    Das Layout ist identisch, da beide Seiten die gleiche Layoutseite und-Farbe wie in *\_pagestart. cshtml*initialisiert verwenden.

## <a name="using-_pagestartcshtml-to-handle-errors"></a>Verwenden von \_pagestart. cshtml zum Behandeln von Fehlern

Ein weiterer guter Verwendungs Vorteil für die Datei *\_pagestart. cshtml* besteht darin, eine Möglichkeit zum Verarbeiten von Programmierfehlern (Ausnahmen) zu erstellen, die auf einer beliebigen *cshtml* -Seite in einem Ordner auftreten können. Dieses Beispiel zeigt eine Möglichkeit, dies zu tun.

1. Erstellen Sie im Stamm Ordner einen Ordner mit dem Namen *initcatch*.
2. Erstellen Sie im Ordner *initcatch* Ihrer Website eine Datei mit dem Namen *\_pagestart. cshtml* , und ersetzen Sie das vorhandene Markup und den Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    In diesem Code versuchen Sie, die angeforderte Seite explizit durch Aufrufen der `RunPage`-Methode in einem `try`-Block auszuführen. Wenn auf der angeforderten Seite Programmierfehler auftreten, wird der Code im `catch`-Block ausgeführt. In diesem Fall wird der Code an eine Seite (*Error. cshtml*) umgeleitet und übergibt den Namen der Datei, in der der Fehler aufgetreten ist, als Teil der URL. (Sie erstellen die Seite in Kürze.)
3. Erstellen Sie im Ordner *initcatch* Ihrer Website eine Datei mit dem Namen *Exception. cshtml* , und ersetzen Sie das vorhandene Markup und den Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Im Rahmen dieses Beispiels wird in diesem Beispiel bewusst ein Fehler erstellt, wenn Sie versuchen, eine Datenbankdatei zu öffnen, die nicht vorhanden ist.
4. Erstellen Sie im Stamm Ordner eine Datei namens *Error. cshtml* , und ersetzen Sie das vorhandene Markup und den Code durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Auf dieser Seite ruft der Ausdruck `@Request["source"]` den Wert aus der URL ab und zeigt ihn an.
5. Klicken Sie in der Symbolleiste auf **Speichern**.
6. Führen Sie *Exception. cshtml* in einem Browser aus. 

    ![Klang](18-customizing-site-wide-behavior/_static/image5.jpg)

    Da in *Exception. cshtml*ein Fehler auftritt, wird die *\_pagestart. cshtml* -Seite an die Datei *Error. cshtml* umgeleitet, in der die Meldung angezeigt wird.

    Weitere Informationen zu Ausnahmen finden Sie unter [Einführung in ASP.net Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-_pagestartcshtml-to-restrict-folder-access"></a>Verwenden von \_pagestart. cshtml zum Einschränken des Ordner Zugriffs

Sie können auch die Datei *\_pagestart. cshtml* verwenden, um den Zugriff auf alle Dateien in einem Ordner einzuschränken.

1. Erstellen Sie in webmatrix mithilfe der Option **Site from Template** eine neue Website.
2. Wählen Sie in den verfügbaren Vorlagen die Option **Starter Site**aus.
3. Erstellen Sie im Stamm Ordner einen Ordner mit dem Namen *authentieredcontent*.
4. Erstellen Sie im Ordner *Authenti-Content* eine Datei mit dem Namen *\_pagestart. cshtml* , und ersetzen Sie das vorhandene Markup und den Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Der Code wird gestartet, indem verhindert wird, dass alle Dateien im Ordner zwischengespeichert werden. (Dies ist für Szenarien wie z. b. öffentliche Computer erforderlich, in denen Sie nicht möchten, dass die zwischengespeicherten Seiten eines Benutzers für den nächsten Benutzer verfügbar sind.) Als Nächstes bestimmt der Code, ob der Benutzer sich bei der Website angemeldet hat, bevor er eine der Seiten im Ordner anzeigen kann. Wenn der Benutzer nicht angemeldet ist, wird der Code an die Anmeldeseite umgeleitet. Auf der Anmeldeseite kann der Benutzer auf die ursprünglich angeforderte Seite zurückgegeben werden, wenn Sie einen Abfrage Zeichenfolgen-Wert mit dem Namen `ReturnUrl`einschließen.
5. Erstellen Sie im Ordner *Authenti-Content* eine neue Seite mit dem Namen *Page. cshtml*.
6. Ersetzen Sie das Standard Markup durch Folgendes:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Führen Sie *Page. cshtml* in einem Browser aus. Der Code leitet Sie zu einer Anmeldeseite um. Sie müssen sich registrieren, bevor Sie sich anmelden. Nachdem Sie registriert und angemeldet sind, können Sie zu der Seite navigieren und ihren Inhalt anzeigen.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Einführung in ASP.net Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587)
