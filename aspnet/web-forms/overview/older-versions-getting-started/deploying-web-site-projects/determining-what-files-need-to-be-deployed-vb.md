---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: Bestimmen, welche Dateien bereitgestellt werden müssen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Welche Dateien aus der Entwicklungsumgebung in der Produktionsumgebung bereitgestellt werden müssen, hängt teilweise davon ab, ob die ASP.NET-Anwendung erstellt wurde...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: a11dadfda8b6a189acedd7ac723d85f8b2084324
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569927"
---
# <a name="determining-what-files-need-to-be-deployed-vb"></a>Festlegen der bereitzustellenden Dateien (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> Welche Dateien aus der Entwicklungsumgebung in der Produktionsumgebung bereitgestellt werden müssen, hängt davon ab, ob die ASP.NET-Anwendung mit dem Website Modell oder dem Webanwendungs Modell erstellt wurde. Erfahren Sie mehr über diese beiden Projekt Modelle und wie sich das Projekt Modell auf die Bereitstellung auswirkt.

## <a name="introduction"></a>Einführung

Beim Bereitstellen einer ASP.NET-Webanwendung müssen die ASP.NET-bezogenen Dateien aus der Entwicklungsumgebung in die Produktionsumgebung kopiert werden. Zu den ASP.NET bezogenen Dateien gehören ASP.NET-Webseiten Markup und Code sowie Client-und serverseitige Unterstützungs Dateien. Bei Client seitigen Unterstützungs Dateien handelt es sich um Dateien, auf die von ihren Webseiten verwiesen und die direkt an die Browser Bilder, CSS-Dateien und JavaScript-Dateien gesendet werden. Serverseitige Unterstützungs Dateien enthalten diejenigen, die zum Verarbeiten einer serverseitigen Anforderung verwendet werden. Dies schließt u. a. Konfigurationsdateien, Webdienste, Klassendateien, typisierte Datasets und LINQ to SQL Dateien ein.

Im Allgemeinen sollten alle Client seitigen Unterstützungs Dateien aus der Entwicklungsumgebung in die Produktionsumgebung kopiert werden, aber welche serverseitigen Unterstützungs Dateien kopiert werden, hängt davon ab, ob Sie den serverseitigen Code explizit in eine Assembly (eine `.dll` Datei) kompilieren oder ob diese Assemblys automatisch generiert werden. In diesem Tutorial wird hervorgehoben, welche Dateien bereitgestellt werden müssen, wenn der Code explizit in eine Assembly kompiliert wird und dieser Kompilierungs Schritt automatisch erfolgt.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Explizite Kompilierung und automatische Kompilierung

ASP.net Web Pages werden in deklaratives Markup und Quellcode aufgeteilt. Der deklarative Markup Teil umfasst HTML, websteuer Elemente und Datenbindung-Syntax. der Codeteil enthält Ereignishandler, die in Visual Basic oder C# Code geschrieben wurden. Die Markup-und Code Teile sind in der Regel in verschiedene Dateien unterteilt: `WebPage.aspx` enthält das deklarative Markup, während `WebPage.aspx.vb` den Code enthält.

Angenommen, eine ASP.NET-Seite mit dem Namen `Clock.aspx`, die ein Label-Steuerelement enthält, dessen Text-Eigenschaft auf das aktuelle Datum und die Uhrzeit der Seiten Auslastung festgelegt ist Der deklarative Markup Teil (in `Clock.aspx`) enthält das Markup für ein Bezeichnungs-websteuer Element `<asp:Label runat="server" id="TimeLabel" />`, während der Codeteil (in `Clock.aspx.vb`) einen `Page_Load` Ereignishandler mit folgendem Code enthält:

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

Damit die ASP.net-Engine eine Anforderung für diese Seite bedienen kann, muss der Codeteil der Seite (die *`WebPage`* `.aspx.vb` Datei) zuerst kompiliert werden. Diese Kompilierung kann explizit oder automatisch ausgeführt werden.

Wenn die Kompilierung explizit erfolgt, wird der Quellcode der gesamten Anwendung in eine oder mehrere Assemblys (`.dll` Dateien) kompiliert, die sich im `Bin` Verzeichnis der Anwendung befinden. Wenn die Kompilierung automatisch erfolgt, wird die resultierende automatisch generierte Assembly standardmäßig in den Ordner "`Temporary ASP.NET Files`" eingefügt, der in `%WINDOWS%\Microsoft.NET\Framework\<version>`gefunden wird, obwohl dieser Speicherort über das&lt;- [Kompilierungs&gt; Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in `Web.config`konfiguriert werden kann. Bei der expliziten Kompilierung müssen Sie Maßnahmen ergreifen, um den Code der ASP.NET-Anwendung in eine Assembly zu kompilieren, und dieser Schritt tritt vor der Bereitstellung auf. Bei der automatischen Kompilierung erfolgt der Kompilierungs Vorgang auf dem Webserver, wenn der Zugriff auf die Ressource zum ersten Mal erfolgt.

Unabhängig davon, welches Kompilierungs Modell Sie verwenden, muss der Markup Teil aller ASP.NET Seiten (die `WebPage.aspx` Dateien) in die Produktionsumgebung kopiert werden. Bei der expliziten Kompilierung müssen Sie die Assemblys im Ordner "`Bin`" kopieren. Sie müssen jedoch nicht die Code Teile der ASP.NET Seiten kopieren (die `WebPage.aspx.vb` Dateien). Bei der automatischen Kompilierung müssen Sie die Codeteil Dateien kopieren, damit der Code vorhanden ist und beim Besuch der Seite automatisch kompiliert werden kann. Der Markup Teil jeder ASP.NET-Webseite enthält eine `@Page`-Direktive mit Attributen, die angeben, ob der zugeordnete Code der Seite bereits explizit kompiliert wurde oder ob er automatisch kompiliert werden muss. Folglich kann die Produktionsumgebung mit einem der beiden Kompilierungs Modelle nahtlos arbeiten, und Sie müssen keine speziellen Konfigurationseinstellungen anwenden, um anzugeben, dass eine explizite oder automatische Kompilierung verwendet wird.

Tabelle 1 fasst die verschiedenen Dateien zusammen, die bei Verwendung der expliziten Kompilierung und der automatischen Kompilierung bereitgestellt Beachten Sie, dass Sie unabhängig vom verwendeten Kompilierungs Modell immer die Assemblys im Ordner `Bin` bereitstellen sollten, wenn dieser Ordner vorhanden ist. Der `Bin` Ordner enthält die für die Webanwendung spezifischen Assemblys, die den kompilierten Quellcode enthalten, wenn das explizite Kompilierungs Modell verwendet wird. Das `Bin` Verzeichnis enthält auch Assemblys aus anderen Projekten und alle Open Source-Assemblys oder Assemblys von Drittanbietern, die Sie verwenden können. diese müssen auf dem Produktionsserver vorhanden sein. Daher sollten Sie als allgemeine Faustregel den `Bin` Ordner bei der Bereitstellung in die Produktionsumgebung kopieren. (Wenn Sie das Modell für die automatische Kompilierung verwenden und keine externen Assemblys verwenden, haben Sie kein `Bin` Verzeichnis, das ist OK!)

| **Kompilierungs Modell** | **Markup Teil Datei bereitstellen?** | **Quell Code Datei bereitstellen?** | **Assemblys in `Bin` Verzeichnis bereitstellen?** |
| --- | --- | --- | --- |
| Explizite Kompilierung | Ja | Nein | Ja |
| Automatische Kompilierung | Ja | Ja | Ja (sofern vorhanden) |

**Tabelle 1: welche Dateien Sie bereitstellen, hängt vom verwendeten Kompilierungs Modell ab.**

## <a name="taking-a-trip-down-memory-lane"></a>Übernehmen der Arbeitsspeicher Spur

Welche Kompilierungs Methode verwendet wird, hängt teilweise davon ab, wie die ASP.NET-Anwendung in Visual Studio verwaltet wird. Zumal. Die Inbetriebnahme des Netzwerks im Jahr 2000 gab es vier verschiedene Versionen von Visual Studio: Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 und Visual Studio 2008. Von Visual Studio .NET 2002 und 2003 verwaltete ASP.NET-Anwendungen, die das *Webanwendungs Projekt* Modell verwenden. Die wichtigsten Features des Webanwendungs Projekt Modells sind:

- Die Dateien, die das Projekt zusammenstellen, werden in einer einzelnen Projektdatei definiert. Alle Dateien, die nicht in der Projektdatei definiert sind, werden von Visual Studio nicht als Teil der Webanwendung behandelt.
- Verwendet die explizite Kompilierung. Beim Erstellen des Projekts werden die Code Dateien innerhalb des Projekts in eine einzelne Assembly kompiliert, die im Ordner "`Bin`" platziert wird.

Als Microsoft Visual Studio 2005 veröffentlichte, wurde die Unterstützung für das Webanwendungs Projekt Modell gelöscht und durch das Website Projekt Modell ersetzt. Das Website Projekt Modell unterscheidet sich wie folgt vom *Webanwendungs Projekt* Modell:

- Anstatt eine einzelne Projektdatei zu verwenden, die die Dateien des Projekts auslöst, wird stattdessen das Dateisystem verwendet. Kurz gesagt, alle Dateien im Webanwendungs Ordner (oder Unterordner) werden als Teil des Projekts betrachtet.
- Wenn Sie ein Projekt in Visual Studio erstellen, wird keine Assembly im `Bin` Verzeichnis erstellt. Stattdessen werden bei der Erstellung eines Website Projekts alle Kompilierzeitfehler gemeldet.
- Unterstützung für die automatische Kompilierung. Website Projekte werden in der Regel bereitgestellt, indem das Markup und der Quellcode in die Produktionsumgebung kopiert werden, obwohl der Code vorkompiliert werden kann (explizite Kompilierung).

Microsoft hat das Webanwendungs Projekt Modell bei der Veröffentlichung von Visual Studio 2005 Service Pack 1 wiederbelebt. Visual Web Developer unterstützt jedoch weiterhin nur das Website Projekt Modell. Die gute Nachricht ist, dass diese Einschränkung mit Visual Web Developer 2008 Service Pack 1 gelöscht wurde. Heute können Sie ASP.NET-Anwendungen in Visual Studio (und Visual Web Developer) mit dem Webanwendungs Projekt Modell oder dem Website Projekt Modell erstellen. Beide Modelle haben ihre vor-und Nachteile. Einen Vergleich der beiden Modelle finden Sie unter [Einführung in Webanwendungs Projekte: Vergleichen von Website Projekten und Webanwendungs Projekten](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) und Unterstützung bei der Entscheidung, welches Projekt Modell für Ihre Situation am besten geeignet ist.

## <a name="exploring-the-sample-web-application"></a>Untersuchen der beispielweb Anwendung

Der Download für dieses Tutorial umfasst eine ASP.NET-Anwendung mit dem Namen "Buch Reviews". Die Website imitiert eine Hobby Website, die jemand erstellen könnte, um seine Buchbesprechungen mit der Online Community auszutauschen. Diese ASP.NET-Webanwendung ist sehr einfach und besteht aus den folgenden Ressourcen:

- `Web.config`die Konfigurationsdatei der Anwendung.
- Eine Master Seite (`Site.master`).
- Sieben verschiedene ASP.NET Seiten:

    - ~/`Default.aspx`-die Homepage des Standorts.
    - ~/`About.aspx`-eine Seite "Informationen zur Site".
    - ~/`Fiction/Default.aspx`-eine Seite mit den überprüften Büchern.

        - ~/`Fiction/Blaze.aspx`-a Review of the Richard Bachman Roman *Blaze*.
    - ~/`Tech/Default.aspx`-eine Seite, auf der die von Ihnen überprüften Technologie Bücher aufgelistet sind.

        - ~/`Tech/CYOW.aspx`-eine Prüfung der *Erstellung einer eigenen Website*.
        - ~/`Tech/TYASP35.aspx`-eine Überprüfung *selbst ASP.NET 3,5 in 24 Stunden*.
- Drei verschiedene CSS-Dateien im Ordner "`Styles`".
- Vier Bilddateien: ein ASP.NET-Logo und Bilder der Cover-Dateien der drei geprüften Bücher, die sich im `Images` Ordner befinden.
- Eine `Web.sitemap`-Datei, die die Site Übersicht definiert und zum Anzeigen von Menüs auf den `Default.aspx` Seiten im Stammverzeichnis und `Fiction`-und `Tech` Ordner verwendet wird.
- Eine Klassendatei mit dem Namen `BasePage.vb`, die eine Basis `Page` Klasse definiert. Diese Klasse erweitert die Funktionalität der `Page`-Klasse, indem die `Title`-Eigenschaft basierend auf der Position der Seite in der Site Übersicht automatisch festgelegt wird. Kurz gesagt, jede ASP.NET-Code-Behind-Klasse, die `BasePage` (statt `System.Web.UI.Page`) erweitert, wird der Titel abhängig von seiner Position in der Site Übersicht auf einen Wert festgelegt. Wenn Sie z. b. die Seite ~/`Tech/CYOW.aspx` anzeigen, wird der Titel auf "Home: Technology: Create Your Own Website" festgelegt.

Abbildung 1 zeigt einen Screenshot der Website "Book Reviews", wenn Sie in einem Browser angezeigt wird. Hier sehen Sie die Seite ~/Tech/TYASP35.aspx, mit der das Buch *3,5 selbst in 24 Stunden*unter "" angezeigt wird. Der Breadcrumb-Wert, der den oberen Rand der Seite und das Menü in der linken Spalte umfasst, basiert auf der Struktur der Site Struktur, die in `Web.sitemap`definiert ist. Das Bild in der rechten oberen Ecke ist eines der Buch deckbilder, das sich im Ordner "`Images`" befindet. Das Aussehen und Verhalten der Website wird über Cascading Stylesheet-Regeln definiert, die von den CSS-Dateien im Ordner "`Styles`" geschrieben werden, während das übergeordnete Seitenlayout auf der Master Seite `Site.master`definiert ist.

[![die Book Reviews-Website Überprüfungen auf eine Reihe von Titeln bietet](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**Abbildung 1**: die Book Reviews-Website bietet Reviews zu einer Reihe von Titeln ([Klicken Sie, um das Bild in voller Größe anzuzeigen](determining-what-files-need-to-be-deployed-vb/_static/image3.png))

Diese Anwendung verwendet keine Datenbank. jede Überprüfung wird als separate Webseite in der Anwendung implementiert. In diesem Tutorial (und den nächsten vielen Tutorials) wird die Bereitstellung einer Webanwendung erläutert, die keine-Datenbank enthält. In einem zukünftigen Tutorial wird diese Anwendung jedoch erweitert, um Reviews, Leserkommentare und andere Informationen in einer Datenbank zu speichern, und es wird erläutert, welche Schritte ausgeführt werden müssen, um eine datengesteuerte Webanwendung ordnungsgemäß bereitzustellen.

> [!NOTE]
> Diese Tutorials konzentrieren sich auf das Hosten von ASP.NET-Anwendungen mit einem Webhostinganbieter und untersuchen keine ergänzenden Themen wie ASP. Das NET-Site Übersichts System oder die Verwendung einer Basis Seiten Klasse. Weitere Informationen zu diesen Technologien und weitere Hintergrundinformationen zu anderen Themen, die im gesamten Tutorial behandelt werden, finden Sie im Abschnitt "Weitere Informationen" am Ende jedes Tutorials.

Der Download dieses Tutorials umfasst zwei Kopien der Webanwendung, die jeweils als ein anderer Visual Studio-Projekttyp implementiert werden: bookreviewswap, ein Webanwendungs Projekt und bookreviewswsp, ein Website Projekt. Beide Projekte wurden mit Visual Web Developer 2008 SP1 erstellt und verwenden ASP.NET 3,5 SP1. Zum Arbeiten mit diesen Projekten beginnen Sie, indem Sie den Inhalt auf Ihren Desktop entzippen. Navigieren Sie zum Öffnen des Webanwendungs Projekts (bookreviewswap) zum `BookReviewsWAP` Ordner, und doppelklicken Sie auf die Projektmappendatei, `BookReviewsWAP.sln`. Starten Sie Visual Studio, und klicken Sie dann im Menü Datei auf die Option Website öffnen, navigieren Sie zum Ordner `BookReviewsWSP` auf Ihrem Desktop, und klicken Sie auf OK, um das Website Projekt (bookreviewtausp) zu öffnen.

Die verbleibenden zwei Abschnitte in diesem Tutorial untersuchen, welche Dateien beim Bereitstellen der Anwendung in die Produktionsumgebung kopiert werden müssen. In den nächsten beiden Tutorials wird beschrieben, wie Sie [*Ihre Website mithilfe von FTP*](deploying-your-site-using-an-ftp-client-vb.md) bereitstellen und [*Ihre Website mithilfe von Visual Studio*](deploying-your-site-using-visual-studio-vb.md) bereitstellen: zeigen Sie verschiedene Möglichkeiten zum Kopieren dieser Dateien in einen Webhost Anbieter

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Festlegen der bereit zustellenden Dateien für das Webanwendungs Projekt

Das Webanwendungs Projekt Modell verwendet eine explizite Kompilierung. der Quellcode des Projekts wird jedes Mal, wenn Sie die Anwendung erstellen, in eine einzelne Assembly kompiliert. Diese Kompilierung umfasst die Code Behind-Dateien der ASP.net Pages (~/`Default.aspx.vb`, ~/`About.aspx.vb`usw.) sowie die `BasePage.vb`-Klasse. Die resultierende Assembly wird `BookReviewsWAP.dll` benannt und befindet sich im `Bin` Verzeichnis der Anwendung.

Abbildung 2 zeigt die Dateien, aus denen das Webanwendungs Projekt "Book Reviews" besteht.

[![der Projektmappen-Explorer listet die Dateien auf, aus denen das Webanwendungs Projekt besteht.](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**Abbildung 2**: die Projektmappen-Explorer listet die Dateien auf, aus denen das Webanwendungs Projekt besteht.

> [!NOTE]
> Wie in Abbildung 2 gezeigt, werden die Code-Behind-Dateien der ASP.net Pages nicht im Projektmappen-Explorer für ein Visual Basic Webanwendungs Projekt angezeigt. Um die Code Behind-Klasse für eine Seite anzuzeigen, klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Seite, und wählen Sie Code anzeigen aus.

Zum Bereitstellen einer ASP.NET-Anwendung, die mithilfe des Webanwendungs Projekt Modells entwickelt wurde, beginnen Sie, indem Sie die Anwendung so erstellen, dass der neueste Quellcode explizit in eine Assembly kompiliert wird. Kopieren Sie als nächstes die folgenden Dateien in die Produktionsumgebung:

- Die Dateien, die das deklarative Markup für jede ASP.NET Seite enthalten, z. b. ~/`Default.aspx`, ~/`About.aspx`usw. Kopieren Sie außerdem das deklarative Markup für alle Masterseiten und Benutzer Steuerelemente.
- Die Assemblys (`.dll` Dateien) im Ordner `Bin`. Sie müssen die Programm Datenbankdateien (`.pdb`) oder XML-Dateien, die Sie möglicherweise im `Bin` Verzeichnis finden, nicht kopieren.

Sie müssen die Quell Code Dateien der ASP.net Pages nicht in die Produktionsumgebung kopieren, und Sie müssen nicht die `BasePage.vb` Klassendatei kopieren.

> [!NOTE]
> Wie in Abbildung 2 gezeigt, wird die `BasePage`-Klasse als Klassendatei in das Projekt implementiert, das in den Ordner "`HelperClasses`" eingefügt wird. Wenn das Projekt kompiliert wird, wird der Code in der `BasePage.vb`-Datei zusammen mit den Code Behind-Klassen der ASP.net pages in der einzelnen Assembly, `BookReviewsWAP.dll`, kompiliert. ASP.net verfügt über einen speziellen Ordner mit dem Namen `App_Code`, der Klassendateien für Website Projekte enthalten soll. Der Code im Ordner "`App_Code`" wird automatisch kompiliert und sollte daher nicht mit Webanwendungs Projekten verwendet werden. Stattdessen sollten Sie die Klassendateien der Anwendung in einem normalen Ordner namens `HelperClasses`oder `Classes`oder etwas ähnliches platzieren. Alternativ können Sie Klassendateien in einem separaten Klassen Bibliotheksprojekt platzieren.

Zusätzlich zum Kopieren der ASP.NET-bezogenen Markup Dateien und der Assembly in den `Bin` Ordner müssen Sie auch die Client seitigen Unterstützungs Dateien kopieren: die Bilder und CSS-Dateien sowie die anderen serverseitigen Unterstützungs Dateien `Web.config` und `Web.sitemap`. Diese Client-und serverseitigen Unterstützungs Dateien müssen in die Produktionsumgebung kopiert werden, unabhängig davon, ob Sie die explizite oder die automatische Kompilierung verwenden.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Festlegen der bereit zustellenden Dateien für die Website-Projektdateien

Das Website Projekt Modell unterstützt die automatische Kompilierung, ein Feature, das bei Verwendung des Webanwendungs Projekt Modells nicht verfügbar ist. Bei der expliziten Kompilierung müssen Sie den Quellcode des Projekts in eine Assembly kompilieren und diese Assembly in die Produktionsumgebung kopieren. Bei der automatischen Kompilierung kopieren Sie jedoch einfach den Quellcode in die Produktionsumgebung und werden bei Bedarf von der Runtime bei Bedarf kompiliert.

Die Menüoption Build in Visual Studio ist sowohl in Webanwendungs Projekten als auch in Website Projekten vorhanden. Beim Erstellen eines Webanwendungs Projekts wird der Quellcode des Projekts in eine einzelne Assembly, die sich im `Bin` Verzeichnis befindet, kompiliert. beim Erstellen eines Website Projekts werden Kompilierzeitfehler überprüft, aber es werden keine Assemblys erstellt. Um eine ASP.NET-Anwendung bereitzustellen, die mit dem Website Projekt Modell entwickelt wurde, müssen Sie lediglich die entsprechenden Dateien in die Produktionsumgebung kopieren, aber ich empfehle Ihnen, zuerst das Projekt zu erstellen, um sicherzustellen, dass keine Kompilierzeitfehler auftreten.

Abbildung 3 zeigt die Dateien, aus denen das Website Projekt der Buch Überprüfungen besteht.

[![der Projektmappen-Explorer listet die Dateien auf, aus denen das Website Projekt besteht.](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**Abbildung 3**: die Projektmappen-Explorer listet die Dateien auf, aus denen das Website Projekt besteht.

Das Bereitstellen eines Website Projekts umfasst das Kopieren aller ASP.NET-bezogenen Dateien in die Produktionsumgebung, die die Markup Seiten für ASP.NET-Seiten, Masterseiten und Benutzer Steuerelemente zusammen mit den zugehörigen Code Dateien enthält. Sie müssen auch alle Klassendateien kopieren, z. b. `BasePage.vb`. Beachten Sie, dass sich die `BasePage.vb` Datei im Ordner `App_Code` befindet. Dies ist ein spezieller ASP.NET-Ordner, der in Website Projekten für Klassendateien verwendet wird. Der spezielle Ordner muss auch in der Produktion erstellt werden, da die Klassendateien im Ordner `App_Code` in der Entwicklungsumgebung in den Ordner `App_Code` in der Produktionsumgebung kopiert werden müssen.

Zusätzlich zum Kopieren der ASP.NET-Markup-und Quell Code Dateien müssen Sie auch die Client seitigen Unterstützungs Dateien kopieren: die Bilder und die CSS-Dateien sowie die anderen serverseitigen Unterstützungs Dateien, `Web.config` und `Web.sitemap`.

> [!NOTE]
> Website Projekte können auch eine explizite Kompilierung verwenden. In einem zukünftigen Tutorial wird erläutert, wie ein Website Projekt explizit kompiliert wird.

## <a name="summary"></a>Summary

Das Bereitstellen einer ASP.NET-Anwendung umfasst das Kopieren der erforderlichen Dateien aus der Entwicklungsumgebung in die Produktionsumgebung. Der genaue Satz von Dateien, die synchronisiert werden müssen, hängt davon ab, ob der Code der ASP.NET-Anwendung explizit oder automatisch kompiliert wird. Welche Kompilierungs Strategie angestellt ist, hängt davon ab, ob Visual Studio für die Verwaltung der ASP.NET-Anwendung mit dem Webanwendungs Projekt Modell oder dem Website Projekt Modell konfiguriert ist.

Das Webanwendungs Projekt Modell verwendet eine explizite Kompilierung und kompiliert den Projekt Code in eine einzelne Assembly im Ordner "`Bin`". Beim Bereitstellen der Anwendung müssen der Markup Teil der ASP.NET-Seiten und der Inhalt des `Bin` Ordners in die Produktionsumgebung verschoben werden. der Quellcode in der Anwendung, z. b. die Code Dateien und Code-Behind-Klassen, muss nicht in die Produktionsumgebung kopiert werden.

Das Website Projekt Modell verwendet standardmäßig die automatische Kompilierung, obwohl es möglich ist, ein Website Projekt explizit zu kompilieren, wie wir in zukünftigen Tutorials sehen werden. Das Bereitstellen einer ASP.NET-Anwendung, die die automatische Kompilierung verwendet, erfordert, dass der Markup Teil *und* der Quellcode in die Produktionsumgebung kopiert werden müssen. Der Code wird automatisch in der Produktionsumgebung kompiliert, wenn er zum ersten Mal angefordert wird.

Nachdem wir nun geprüft haben, welche Dateien zwischen den Entwicklungs-und Produktionsumgebungen synchronisiert werden müssen, sind wir bereit, die Anwendung "Book Reviews" für einen Webhost Anbieter bereitzustellen.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.net Übersicht](https://msdn.microsoft.com/library/ms178466.aspx)
- [ASP.NET-Benutzer Steuerelemente](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Untersuchen von ASP. NET-Site Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Einführung in Webanwendungs Projekte](https://msdn.microsoft.com/library/aa730880.aspx)
- [Lernprogramme für Master Seiten](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Freigeben von Code zwischen Seiten](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Verwenden einer benutzerdefinierten Basisklasse für Code Behind-Klassen Ihrer ASP.net Pages](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005-Website Projekt System: Worum handelt es sich dabei, und warum haben wir es getan?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Exemplarische Vorgehensweise: umstellen eines Website Projekts in ein Webanwendungs Projekt in Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Zurück](asp-net-hosting-options-vb.md)
> [Weiter](deploying-your-site-using-an-ftp-client-vb.md)
