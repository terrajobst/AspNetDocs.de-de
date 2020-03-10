---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Vorkompilieren Ihrer Website (C#) | Microsoft-Dokumentation
author: rick-anderson
description: 'Visual Studio bietet ASP.NET Entwicklern zwei Arten von Projekten: Webanwendungs Projekte (WAPs) und Website Projekte (wsps). Einer der wichtigsten Unterschiede...'
ms.author: riande
ms.date: 06/09/2009
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: cb42398f44f3cd16ab83a9976e7b34f132384456
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78462873"
---
# <a name="precompiling-your-website-c"></a>Vorkompilieren Ihrer Website (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio bietet ASP.NET Entwicklern zwei Arten von Projekten: Webanwendungs Projekte (WAPs) und Website Projekte (wsps). Einer der Hauptunterschiede zwischen den beiden Projekttypen besteht darin, dass WAPs den Code vor der Bereitstellung explizit kompiliert haben muss, während der Code in einem WSP automatisch auf dem Webserver kompiliert werden kann. Es ist jedoch möglich, vor der Bereitstellung ein WSP vorzukompilieren. In diesem Tutorial werden die Vorteile der Vorkompilierung erläutert, und es wird gezeigt, wie Sie eine Website in Visual Studio und über die Befehlszeile vorkompilieren.

## <a name="introduction"></a>Einführung

Visual Studio bietet ASP.NET Entwicklern zwei verschiedene Projekttypen: Webanwendungs Projekte (WAP) und Website Projekte (WSP). Einer der Hauptunterschiede zwischen diesen Projekttypen besteht darin, dass WAPs eine *explizite Kompilierung* erfordert, während wsps standardmäßig die *automatische Kompilierung*verwendet. Mit WAPs kompilieren Sie den Code der Webanwendung in eine einzelne Assembly, die im `Bin` Ordner der Website erstellt wird. Die Bereitstellung umfasst das Kopieren des Markup Inhalts (der `.aspx.ascx`-und `.master` Dateien) im Projekt zusammen mit der Assembly im `Bin` Ordner. die Code-Behind-Klassendateien selbst müssen nicht bereitgestellt werden. Auf der anderen Seite stellen Sie wsps bereit, indem Sie sowohl die Markup Seiten als auch die zugehörigen Code-Behind-Klassen in die Produktionsumgebung kopieren. Die Code-Behind-Klassen werden Bedarfs gesteuert auf dem Webserver kompiliert.

> [!NOTE]
> Weitere Hintergrundinformationen zu den Unterschieden zwischen den Projekt Modellen, der expliziten und der automatischen Kompilierung und der Auswirkungen des Kompilierungs Modells auf die Bereitstellung finden Sie im Abschnitt "explizite Kompilierung im [ Vergleich zur automatischen](determining-what-files-need-to-be-deployed-cs.md) Kompilierung".

Die Option für die automatische Kompilierung ist einfach zu verwenden. Es gibt keinen expliziten Kompilierungs Schritt, und nur die Dateien, die geändert wurden, müssen bereitgestellt werden, während die explizite Kompilierung die Bereitstellung der geänderten Markup Seiten und der soeben kompilierten Assembly erfordert. Die automatische Bereitstellung hat jedoch zwei mögliche Nachteile:

- Da die Seiten beim ersten Besuch automatisch kompiliert werden müssen, kann es eine kurze, aber spürbare Verzögerung geben, wenn eine ASP.NET-Seite zum ersten Mal nach der Bereitstellung angefordert wird.
- Die automatische Kompilierung erfordert, dass sowohl das deklarative Markup als auch der Quellcode auf dem Webserver vorhanden sind. Dies kann eine unattraktive Option sein, wenn Sie planen, die Webanwendung für Kunden zu verkaufen, die Sie auf ihren Webservern installieren.

Wenn einer der beiden oben genannten Mängel die Trennungs Option ist, können Sie entweder zum WAP-Modell wechseln oder den WSP vor der Bereitstellung *Vorkompilieren* . In diesem Tutorial werden die vorkompilierungs Optionen erläutert, die für eine gehostete Website am besten geeignet sind, und die vorkompilierungs Prozesse und die Bereitstellung einer vorkompilierten Website werden

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Eine Übersicht über die ASP.NET-Code Generierung und-Kompilierung

Bevor wir uns mit den verfügbaren vorkompilierungs Optionen befassen, sprechen wir zuerst über die Codegenerierung und-Kompilierung, die auftritt, wenn eine ASP.NET Seite zum ersten Mal angefordert wird, nachdem Sie erstellt oder zuletzt aktualisiert wurde. Wie Sie wissen, bestehen ASP.NET Seiten aus zwei Teilen: deklaratives Markup in der `.aspx` Datei; und einen Quell Code Teil, in der Regel in einer separaten Code-Behind-Klassendatei (`.aspx.cs`). Welche Schritte von der Laufzeit ausgeführt werden, wenn eine ASP.NET-Seite angefordert wird, hängt vom Kompilierungs Modell der Anwendung ab.

Bei WAPs muss der Quellcode der Seiten vor der Bereitstellung explizit in eine einzelne Assembly kompiliert werden. Während der Bereitstellung werden diese Assembly und die verschiedenen Markup Seiten in die Produktionsumgebung kopiert. Wenn eine Anforderung für eine ASP.NET-Seite an den Webserver gesendet wird, erstellt die Laufzeit eine Instanz der Code-Behind-Klasse der Seite und ruft die `ProcessRequest`-Methode auf, die den Seiten Lebenszyklus startet und letztendlich den Seiten Inhalt generiert, der an den Anforderer zurückgegeben wird. Die Laufzeit kann mit der Code Behind-Klasse der ASP.NET-Seite arbeiten, da die Code-Behind-Klasse bereits vor der Bereitstellung in eine Assembly kompiliert wurde.

Bei wsps und der automatischen Kompilierung ist vor der Bereitstellung kein expliziter Kompilierungs Schritt vorhanden. Stattdessen wird durch die Bereitstellung sowohl der deklarative als auch der Quell Code Inhalt in die Produktionsumgebung kopiert. Wenn eine Anforderung zum ersten Mal beim Webserver für eine ASP.NET-Seite eingeht, nachdem die Seite erstellt oder zuletzt aktualisiert wurde, muss die Runtime zuerst die Code-Behind-Klasse in eine Assembly kompilieren. Diese kompilierte Assembly wird im Ordner `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`gespeichert, obwohl der Speicherort dieses Ordners über das `<compilation tempDirectory="" />`-Element von `<system.web>`angepasst werden kann, in der Regel in `Web.config`. Da die Assembly auf einem Datenträger gespeichert wird, muss Sie bei nachfolgenden Anforderungen auf derselben Seite nicht neu kompiliert werden.

> [!NOTE]
> Erwartungsgemäß kommt es zu einer geringfügigen Verzögerung, wenn eine Seite zum ersten Mal (oder zum ersten Mal seit ihrer Änderung) an einer Site mit automatischer Kompilierung angefordert wird, da es einen Moment dauert, bis der Server den Code der Seite kompiliert und die resultierende Assembly in Diskette.

Kurz gesagt, bei der expliziten Kompilierung müssen Sie den Quellcode der Website vor der Bereitstellung kompilieren, sodass die Laufzeit diesen Schritt nicht mehr ausführen muss. Bei der automatischen Kompilierung übernimmt die Laufzeit die Kompilierung des Quellcodes der Seiten, jedoch mit einem geringen Initialisierungs Aufwand für den ersten Besuch der Seite seit der Erstellung oder letzten Aktualisierung.

Aber wie sieht es mit dem deklarativen Teil der ASP.NET Seiten (der `.aspx` Datei) aus? Es ist offensichtlich, dass es eine Beziehung zwischen den `.aspx` Dateien und dem Code in Ihren Code-Behind-Klassen gibt, da auf die im deklarativen Markup definierten websteuer Elemente im Code zugegriffen werden kann. Es ist auch offensichtlich, dass sich der Inhalt in den `.aspx` Dateien stark auf das von der Seite generierte gerenderte Markup auswirkt. Wie funktioniert die Laufzeit also mit der in der `.aspx`-Datei definierten Syntax für Text, HTML und websteuer Elemente, um den gerenderten Inhalt der angeforderten Seite zu generieren?

Die Details der Implementierung auf niedriger Ebene, die sich zwischen WAPs und wsps unterscheiden, sollen nicht zu sidetet werden, aber kurz gesagt generiert die Common Language Runtime automatisch eine Klassendatei, die die verschiedenen websteuer Elemente als geschützte Member und Methoden enthält. Diese generierte Datei wird als *partielle Klasse* für die zugehörige Code Behind-Klasse implementiert. ([Partielle Klassen](http://www.dotnet-guide.com/partialclasses.html) ermöglichen, dass der Inhalt einer einzelnen Klasse auf mehrere Dateien verteilt wird.) Daher wird die Code-Behind-Klasse an zwei Stellen definiert: in der von Ihnen erstellten `.aspx.cs` Datei und in dieser automatisch generierten Klasse, die von der Laufzeit erstellt wurde. Diese automatisch generierte Klasse wird im Ordner `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` gespeichert.

Wichtig ist hierbei, dass für eine ASP.NET-Seite, die von der Laufzeit gerendert wird, sowohl der deklarative als auch der Quell Code Abschnitt in eine Assembly kompiliert werden müssen. Bei WAPs wird der Quellcode vor der Bereitstellung explizit in eine Assembly kompiliert, das deklarative Markup muss jedoch immer noch in den Code konvertiert und von der Laufzeit auf dem Webserver kompiliert werden. Bei Verwendung von wsps mithilfe der automatischen Kompilierung müssen sowohl der Quellcode als auch das deklarative Markup vom Webserver kompiliert werden.

Es ist möglich, die explizite Kompilierung mit dem WSP-Modell zu verwenden. Sie können den Quellcode-Teil wie im WAP-Modell explizit kompilieren. Darüber hinaus können Sie das deklarative Markup kompilieren.

## <a name="precompilation-options"></a>Vorkompilierungs Optionen

Der .NET Framework wird mit einem [ASP.net-Kompilierungs Tool (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) geliefert, das es Ihnen ermöglicht, den Quellcode (und sogar den Inhalt) einer ASP.NET-Anwendung zu kompilieren, die mit dem WSP-Modell erstellt wurde. Dieses Tool wurde mit der .NET Framework Version 2,0 veröffentlicht und befindet sich im Ordner `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Sie kann über die Befehlszeile oder über die Option Website veröffentlichen von Visual Studio aus in Visual Studio gestartet werden.

Das Kompilierungs Tool bietet zwei allgemeine Formen der Kompilierung: direkte Vorkompilierung und Vorkompilierung für die Bereitstellung. Durch die direkte Vorkompilierung führen Sie das `aspnet_compiler.exe` Tool über die Befehlszeile aus und geben den Pfad zum virtuellen Verzeichnis oder zum physischen Pfad einer Website an, die sich auf Ihrem Computer befindet. Das Kompilierungs Tool kompiliert dann jede ASP.NET-Seite im Projekt, wobei die kompilierte Version im `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` Ordner gespeichert wird, so als ob die Seiten zum ersten Mal über einen Browser aufgerufen wurden. Durch die direkte Vorkompilierung kann die erste Anforderung an neu bereitgestellten ASP.NET-Seiten auf Ihrer Website beschleunigt werden, da die Laufzeit nicht mehr benötigt, um diesen Schritt auszuführen. Die direkte Vorkompilierung ist jedoch für die meisten gehosteten Websites nicht nützlich, da es erforderlich ist, dass Sie Programme über die Befehlszeile des Webservers ausführen können. In freigegebenen Hostingumgebungen ist diese Zugriffsebene nicht zulässig.

> [!NOTE]
> Weitere Informationen zur direkten Vorkompilierung finden Sie unter Gewusst [wie: Vorkompilieren von ASP.net](https://msdn.microsoft.com/library/ms227972.aspx) -Websites und [Vorkompilierung in ASP.NET 2,0](http://www.odetocode.com/Articles/417.aspx).

Anstatt die Seiten in der Website in den `Temporary ASP.NET Files` Ordner zu kompilieren, kompiliert die Vorkompilierung für die Bereitstellung die Seiten in ein Verzeichnis Ihrer Wahl und in einem Format, das in der Produktionsumgebung bereitgestellt werden kann.

Es gibt zwei Arten der Vorkompilierung für die Bereitstellung, die wir in diesem Tutorial untersuchen: Vorkompilierung mit einer aktualisierbaren Benutzeroberfläche und Vorkompilierung mit einer nicht aktualisierbaren Benutzeroberfläche. Bei der Vorkompilierung mit einer aktualisierbaren Benutzeroberfläche bleibt das deklarative Markup in den `.aspx`-, `.ascx`-und `.master` Dateien erhalten. so kann ein Entwickler das deklarative Markup auf dem Produktionsserver anzeigen und ggf. ändern. Bei der Vorkompilierung mit einer nicht aktualisierbaren Benutzeroberfläche werden `.aspx` Seiten generiert, die aus beliebigen Inhalten bestehen, und `.ascx`-und `.master` Dateien werden entfernt. Dadurch wird das deklarative Markup ausgeblendet, und ein Entwickler kann es nicht mehr von der Produktionsumgebung aus ändern.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Vorkompilieren für die Bereitstellung mit einer aktualisierbaren Benutzeroberfläche

Die beste Möglichkeit, die Vorkompilierung für die Bereitstellung zu verstehen, ist das Anzeigen eines Beispiels in Aktion. Lassen Sie uns die WSP-Überprüfung für die Bereitstellung über eine aktualisierbare Benutzeroberfläche vorkompilieren. Das ASP.net-Kompilierungs Tool kann über das Build-Menü von Visual Studio oder über die Befehlszeile aufgerufen werden. In diesem Abschnitt wird die Verwendung des Tools in Visual Studio untersucht. der Abschnitt "Vorkompilieren über die Befehlszeile" untersucht das Ausführen des Compilertools über die Befehlszeile.

Öffnen Sie das Buch überprüfen Sie die wsp-Datei in Visual Studio, wechseln Sie zum Menü erstellen, und wählen Sie die Menüoption Website veröffentlichen aus. Dadurch wird das Dialogfeld Website veröffentlichen geöffnet (siehe **Abbildung 1**). hier können Sie den Ziel Speicherort angeben, unabhängig davon, ob die Benutzeroberfläche der vorkompilierten Website aktualisierbar ist, und andere compilertooloptionen. Beim Ziel Speicherort kann es sich um einen Remoteweb Server oder einen FTP-Server handeln, aber wählen Sie jetzt einen Ordner auf der Festplatte Ihres Computers aus. Da die Website mit einer aktualisierbaren Benutzeroberfläche vorkompiliert werden soll, lassen Sie das Kontrollkästchen "diese vorkompilierte Website kann nicht aktualisiert werden" aktiviert, und klicken Sie auf OK.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Abbildung 1**: das ASP.net-Kompilierungs Tool führt eine Vorkompilierung der Website am angegebenen Zielort aus.  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> Die Option Website veröffentlichen im Menü Build ist in Visual Web Developer nicht verfügbar. Wenn Sie Visual Web Developer verwenden, müssen Sie die Befehlszeilenversion des ASP.net-Kompilierungs Tools verwenden, die im Abschnitt "Vorkompilierung über die Befehlszeile" beschrieben wird.

Navigieren Sie nach der Vorkompilierung der Website zu dem Ziel Speicherort, den Sie im Dialogfeld Website veröffentlichen eingegeben haben. Nehmen Sie sich einen Moment Zeit, um den Inhalt dieses Ordners mit dem Inhalt Ihrer Website zu vergleichen. **Abbildung 2** zeigt den Ordner Reviews-Website Ordner. Beachten Sie, dass Sie sowohl `.aspx`-als auch `.aspx.cs`-Dateien enthält. Beachten Sie außerdem, dass das `Bin` Verzeichnis nur eine Datei enthält, `Elmah.dll`, die wir im [vorherigen Tutorial](logging-error-details-with-elmah-cs.md) hinzugefügt haben.

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Abbildung 2**: das Projektverzeichnis enthält `.aspx`-und `.aspx.cs` Dateien; der Ordner "`Bin`" enthält nur `Elmah.dll`  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](precompiling-your-website-cs/_static/image6.png))

**Abbildung 3** zeigt den Ziel Speicherort Ordner, dessen Inhalt vom ASP.net-Kompilierungs Tool erstellt wurde. Dieser Ordner enthält keine Code Behind-Dateien. Darüber hinaus enthält das `Bin` Verzeichnis dieses Ordners neben der `Elmah.dll` Assembly mehrere Assemblys und zwei `.compiled` Dateien.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Abbildung 3**: der Ziel Speicherort Ordner enthält die Dateien für die Bereitstellung.  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](precompiling-your-website-cs/_static/image9.png))

Anders als bei der expliziten Kompilierung in WAPs erstellt die Vorkompilierung für den Bereitstellungs Prozess nicht eine Assembly für den gesamten Standort. Stattdessen werden mehrere Seiten in jede Assembly gebündelt. Außerdem kompiliert Sie die `Global.asax` Datei (falls vorhanden) in Ihre eigene Assembly sowie alle Klassen im Ordner "`App_Code`". Die Dateien, die das deklarative Markup für ASP.NET Webseiten, Benutzer Steuerelemente und Masterseiten enthalten (`.aspx`, `.ascx`und `.master` Dateien), werden unverändert in das Ziel Speicherort Verzeichnis kopiert. Entsprechend werden die `Web.config`-Datei zusammen mit allen statischen Dateien, z. b. Bildern, CSS-Klassen und PDF-Dateien, direkt kopiert. Eine allgemeinere Beschreibung, wie das Kompilierungs Tool verschiedene Dateitypen verarbeitet, finden Sie unter [Datei Behandlung während der ASP.NET-Vorkompilierung](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Sie können das Kompilierungs Tool anweisen, eine Assembly pro ASP.NET-Seite, Benutzer Steuerelement oder Master Seite zu erstellen, indem Sie im Dialogfeld Website veröffentlichen das Kontrollkästchen "verwendete Benennungen und Assemblys mit einer einzelnen Seite" aktivieren. Wenn jede ASP.NET-Seite in eine eigene Assembly kompiliert wird, ist eine präzisere Steuerung der Bereitstellung möglich. Wenn Sie z. b. eine einzelne ASP.NET-Webseite aktualisiert haben und diese Änderung bereitstellen müssen, müssen Sie nur die `.aspx` Datei der Seite und die zugehörige Assembly in der Produktionsumgebung bereitstellen. Weitere Informationen finden Sie unter Gewusst [wie: generieren fester Namen mit dem ASP.net-Kompilierungs Tool](https://msdn.microsoft.com/library/ms228040.aspx) .

Das Verzeichnis Ziel Speicherort enthält auch eine Datei, die nicht Teil des vorkompilierten Webprojekts war, nämlich `PrecompiledApp.config`. Mit dieser Datei wird die ASP.NET-Laufzeit informiert, dass die Anwendung vorkompiliert wurde und ob Sie mit einer aktualisierbaren oder aktualisierbaren Benutzeroberfläche vorkompiliert wurde.

Nehmen Sie schließlich einen Moment Zeit, um eine der `.aspx` Dateien am Ziel Speicherort zu öffnen, indem Sie Visual Studio oder den Text-Editor Ihrer Wahl verwenden. Beim Vorkompilieren für die Bereitstellung mit einer aktualisierbaren Benutzeroberfläche enthalten die ASP.NET-Seiten im Ziel Speicherort Verzeichnis genau dasselbe Markup wie die entsprechenden Dateien auf der Website.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Vorkompilieren für die Bereitstellung mit einer nicht aktualisierbaren Benutzeroberfläche

Das ASP.NET-Compilertool kann auch zum Vorkompilieren einer Website für die Bereitstellung mit einer nicht aktualisierbaren Benutzeroberfläche verwendet werden. Die Vorkompilierung der Website mit einer nicht aktualisierbaren Benutzeroberfläche funktioniert ähnlich wie die Vorkompilierung mit einer aktualisierbaren Benutzeroberfläche. der Hauptunterschied besteht darin, dass die ASP.NET-Seiten, Benutzer Steuerelemente und Masterseiten im Zielverzeichnis von Ihrem Markup entfernt werden. Wenn Sie eine Website für die Bereitstellung mit einer nicht aktualisierbaren Benutzeroberfläche Vorkompilieren möchten, wählen Sie im Menü Erstellen die Option Website veröffentlichen aus, deaktivieren Sie die Option "diese vorkompilierte Site kann nicht aktualisiert werden" (siehe **Abbildung 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Abbildung 4**: Deaktivieren Sie die Option "diese vorkompilierte Site kann nicht aktualisiert werden", um eine Vorkompilierung mit einer nicht aktualisierbaren Benutzeroberfläche auszuführen.  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](precompiling-your-website-cs/_static/image12.png))

**Abbildung 5** zeigt den Ordner "Ziel Speicherort" nach der Vorkompilierung mit einer nicht aktualisierbaren Benutzeroberfläche.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Abbildung 5**: der Ziel Speicherort Ordner für die Bereitstellung mit einer nicht aktualisierbaren Benutzeroberfläche  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](precompiling-your-website-cs/_static/image15.png))

Vergleichen Sie **Abbildung 3** mit **Abbildung 5**. Obwohl die beiden Ordner identisch aussehen können, beachten Sie, dass der nicht aktualisierbare Benutzeroberflächen Ordner nicht die Master Seite `Site.master`. Und obwohl **Abbildung 5** die verschiedenen ASP.NET Seiten enthält, werden Sie, wenn Sie den Inhalt dieser Dateien anzeigen, feststellen, dass Sie Ihr deklaratives Markup entfernt und durch den Platzhalter Text ersetzt haben: "Dies ist eine vom vorkompilierungs Tool generierte Markerdatei und sollte nicht gelöscht werden!"

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Abbildung 5**: das deklarative Markup wurde aus den ASP.NET-Seiten entfernt.

Die `Bin` Ordner in **Abbildung 3** und **5** unterscheiden sich erheblich. Zusätzlich zu den Assemblys enthält der `Bin` Ordner in **Abbildung 5** eine `.compiled` Datei für jede ASP.NET Seite, jedes Benutzer Steuerelement und jede Master Seite.

Das Vorkompilieren einer Website mit einer nicht aktualisierbaren Benutzeroberfläche ist in Situationen nützlich, in denen Sie nicht möchten, dass die Inhalte der ASP.NET Seiten von der Person oder dem Unternehmen geändert werden, die die Website in der Produktionsumgebung installiert oder verwaltet. Wenn Sie eine ASP.NET-Webanwendung erstellen, die Sie an Kunden verkaufen, um Sie auf Ihren eigenen Webservern zu installieren, können Sie sicherstellen, dass Sie das Erscheinungsbild Ihrer Website nicht ändern, indem Sie die `.aspx` Seiten, die Sie bereitstellen, direkt bearbeiten. Wenn Sie Ihre Website mit einer nicht aktualisierbaren Benutzeroberfläche vorkompilieren, versenden Sie den Platzhalter `.aspx` Seiten als Teil der Installation, sodass Ihre Kunden ihre Inhalte nicht mehr untersuchen oder ändern können.

### <a name="precompiling-from-the-command-line"></a>Vorkompilieren über die Befehlszeile

Im Hintergrund wird im Dialogfeld Website veröffentlichen von Visual Studio das ASP.net-Kompilierungs Tool (`aspnet_compiler.exe`) aufgerufen, um die Website vorzukompilieren. Alternativ können Sie dieses Tool über die Befehlszeile aufrufen. Wenn Sie Visual Web Developer verwenden, müssen Sie das Compilertool von der Befehlszeile aus ausführen, da das Menü Build von Visual Web Developer nicht die Option Website veröffentlichen enthält.

Wenn Sie das Compilertool über die Befehlszeile verwenden möchten, löschen Sie zunächst die Befehlszeile, und navigieren Sie zum Frameworkverzeichnis, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Geben Sie als nächstes die folgende Anweisung in die Befehlszeile ein:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Der obige Befehl öffnet das ASP.NET-Compilertool (`aspnet_compiler.exe`) und weist es über den `-p`-Schalter an, die Website zu vorkompilieren, die auf dem *physischen\_Pfad\_in\_App*verankert ist. Dieser Wert wird wie `C:\MySites\BookReviews`lauten und sollte durch Anführungszeichen getrennt werden.

Der `-v`-Schalter gibt das virtuelle Verzeichnis der Site an. Wenn Ihre Website als Standard Website in der IIS-Metabase registriert ist, können Sie den `-p`-Schalter weglassen und nur das virtuelle Verzeichnis der Anwendung angeben. Wenn Sie den `-p`-Schalter verwenden, gibt der Wert, der den `-v` Schalter fortfährt, den Stamm der Website an und wird zum Auflösen von Anwendungs Stamm verweisen verwendet. Wenn Sie beispielsweise den Wert `-v /MySite` angeben, werden Verweise in der Anwendung auf `~/path/file` als `~/MySite/path/file`aufgelöst. Da sich die Website zur Buch Überprüfung im Stammverzeichnis unter meinem Webhostingunternehmen befindet, habe ich den Switch `-v /`verwendet.

Der `-f`-Switch weist, falls vorhanden, das Kompilierungs Tool an, den *Ziel\_Speicherort\_Ordner* Verzeichnis zu überschreiben, wenn es bereits vorhanden ist. Wenn Sie den `-f`-Schalter weglassen und der Ziel Speicherort Ordner bereits vorhanden ist, wird das Kompilierungs Tool mit dem folgenden Fehler beendet: "Fehler aspruntime: das Zielverzeichnis ist nicht leer. Löschen Sie Sie manuell, oder wählen Sie ein anderes Ziel aus. "

Der `-u`-Schalter, falls vorhanden, informiert das Tool darüber, eine aktualisierbare Benutzeroberfläche zu erstellen. Lassen Sie diesen Schalter aus, um die Website mit einer nicht aktualisierbaren Benutzeroberfläche vorzukompilieren.

Schließlich ist der *Ziel\_Speicherort\_Ordners* der physische Pfad zum Ziel Speicherort Verzeichnis. Dieser Wert wird wie `C:\MySites\Output\BookReviews`lauten und sollte durch Anführungszeichen getrennt werden.

## <a name="deploying-the-precompiled-website"></a>Bereitstellen der vorkompilierten Website

An dieser Stelle haben wir gesehen, wie das ASP.net-Kompilierungs Tool verwendet wird, um eine Website mithilfe der aktualisierbaren und nicht aktualisierbaren Benutzeroberflächen Optionen Vorkompilieren zu können. Allerdings haben unsere Beispiele bisher die Website in einem lokalen Ordner und nicht in der Produktionsumgebung vorkompiliert. Die gute Nachricht ist, dass die Bereitstellung der vorkompilierten Website ein Kinderspiel ist und über Visual Studio oder über einen anderen Datei Kopiermechanismus (z. b. über einen eigenständigen FTP-Client) erfolgen kann.

Das Dialogfeld Website veröffentlichen (zuerst in **Abbildung 1**dargestellt) verfügt über eine Option zum Ziel Speicherort, die angibt, wo die vorkompilierten Website Dateien kopiert werden. Hierbei kann es sich um einen Remoteweb Server oder einen FTP-Server handeln. Wenn Sie einen Remote Server in dieses Textfeld eingeben, wird die Website in einem einzigen Schritt vorkompiliert und auf dem angegebenen Server bereitgestellt. Alternativ können Sie die Website in einem lokalen Ordner vorkompilieren und dann den Inhalt dieses Ordners manuell in die Produktionsumgebung über FTP oder einen anderen Ansatz kopieren.

Wenn die vorkompilierte Website automatisch über das Dialogfeld Website veröffentlichen von Visual Studio bereitgestellt wird, eignet sich für einfache Websites, bei denen es keine Konfigurations Unterschiede zwischen den Entwicklungs-und Produktionsumgebungen gibt. Wie im [Tutorial *allgemeine Konfigurations Unterschiede zwischen Entwicklungs-und Produktions* ](common-configuration-differences-between-development-and-production-cs.md) Umgebungen erwähnt, ist es jedoch nicht ungewöhnlich, dass derartige Unterschiede vorhanden sind. Die Webanwendung Book Reviews verwendet beispielsweise eine andere Datenbank in der Produktionsumgebung als in der Entwicklungsumgebung. Wenn Visual Studio die Website auf einem Remote Server veröffentlicht, werden die Konfigurationsdatei Informationen Blind in der Entwicklungsumgebung kopiert.

Bei Standorten mit Konfigurations unterschieden zwischen den Entwicklungs-und Produktionsumgebungen ist es möglicherweise am besten, die Website in einem lokalen Verzeichnis zu kompilieren, die produktionsspezifischen Konfigurationsdateien zu kopieren und dann den Inhalt der vorkompilierten Ausgabe in zu kopieren. Produktion.

Ein Aktualisierungs Programm zum Kopieren von Dateien aus der Entwicklungsumgebung in die Produktionsumgebung finden Sie unter Bereitstellen [*Ihrer Website mithilfe eines FTP-Clients*](deploying-your-site-using-an-ftp-client-cs.md) und bereitstellen [*Ihrer Website mithilfe von Visual Studio*](determining-what-files-need-to-be-deployed-cs.md) -Tutorials.

## <a name="summary"></a>Zusammenfassung

ASP.NET unterstützt zwei Arten der Kompilierung: automatisch und explizit. Wie in den vorherigen Tutorials erläutert, verwenden Webanwendungs Projekte (WAPs) die explizite Kompilierung, während für Website Projekte (wsps) standardmäßig die automatische Kompilierung verwendet wird. Es ist jedoch möglich, mit dem ASP.net-Kompilierungs Tool eine wsp-Datei vor der Bereitstellung explizit zu kompilieren.

Dieses Tutorial konzentriert sich auf die Vorkompilierung des Kompilierungs Tools für die Bereitstellungs Unterstützung. Beim Vorkompilieren für die Bereitstellung erstellt das Kompilierungs Tool einen Ziel Speicherort Ordner, kompiliert den Quellcode der angegebenen Webanwendung und kopiert die kompilierten Assemblys und die Inhalts Dateien in den Ziel Speicherort Ordner. Das Kompilierungs Tool kann so konfiguriert werden, dass eine aktualisierbare oder nicht aktualisierbare Benutzeroberfläche erstellt wird. Beim Vorkompilieren mit einer nicht aktualisierbaren Benutzerschnittstellen Option wird das deklarative Markup in den Inhalts Dateien entfernt. Kurz gesagt: mithilfe der Vorkompilierung können Sie Ihre Website Projekt basierte Anwendung bereitstellen, ohne Quell Code Dateien einschließlich Quell Code Dateien und ggf. mit dem deklarativem Markup entfernen zu müssen.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.NET-Vorkompilierung der Website](https://msdn.microsoft.com/library/ms228015.aspx)
- [Code Behind und Kompilierung in ASP.NET 2,0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Vorkompilierung in ASP.net](http://www.odetocode.com/Articles/417.aspx)
- [Optionen für vorkompilierte Standorte in ASP.net](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Zurück](logging-error-details-with-elmah-cs.md)
> [Weiter](users-and-roles-on-the-production-website-cs.md)
