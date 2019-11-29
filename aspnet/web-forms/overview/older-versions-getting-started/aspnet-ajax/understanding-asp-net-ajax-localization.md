---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Grundlegendes zur ASP.NET AJAX-Lokalisierung | Microsoft-Dokumentation
author: scottcate
description: Lokalisierung ist der Prozess des Entwurfs und der Integration der Unterstützung für eine bestimmte Sprache und Kultur in eine Anwendung oder eine Anwendungs Komponente. Die MIC...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 003e7939accd7a68dab97441b3d999bca835b85a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600865"
---
# <a name="understanding-aspnet-ajax-localization"></a>Grundlegendes zur Lokalisierung in ASP.NET AJAX

von [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Lokalisierung ist der Prozess des Entwurfs und der Integration der Unterstützung für eine bestimmte Sprache und Kultur in eine Anwendung oder eine Anwendungs Komponente. Die Microsoft ASP.NET Plattform bietet umfassende Unterstützung für die Lokalisierung für standardmäßige ASP.NET-Anwendungen durch Integration des standardmäßigen .net-Lokalisierungs Modells. das Microsoft AJAX-Framework nutzt das integrierte Modell, um die verschiedenen Szenarien zu unterstützen, in denen die Lokalisierung durchgeführt werden kann.

## <a name="introduction"></a>Einführung

Die ASP.NET-Technologie von Microsoft sorgt für ein objektorientiertes und Ereignis gesteuertes Programmiermodell und vereinigt es mit den Vorteilen von kompiliertem Code. Das serverseitige Verarbeitungsmodell weist jedoch mehrere Nachteile in der Technologie auf, von denen viele durch die neuen Features im System. Web. Extensions-Namespace adressiert werden können, die die Microsoft AJAX-Dienste im .NET Framework 3,5. Diese Erweiterungen ermöglichen viele Rich Client-Features, die zuvor als Teil der ASP.NET 2,0 AJAX-Erweiterungen verfügbar waren, aber jetzt Teil der Framework-Basisklassen Bibliothek sind. Zu den Steuerelementen und Features in diesem Namespace gehört das partielle Rendering von Seiten, ohne dass eine vollständige Seiten Aktualisierung erforderlich ist, die Möglichkeit des Zugriffs auf Webdienste über ein Client Skript (einschließlich der ASP.net-Profilerstellungs-API) und eine umfangreiche Client seitige API für die Spiegelung vieler die Steuerelement Schemas, die in der serverseitigen ASP.NET-Steuerelement Gruppe angezeigt werden.

In diesem Whitepaper werden die Lokalisierungs Features von Microsoft AJAX Framework und der Microsoft AJAX-Skript Bibliothek im Kontext der geschäftlichen Notwendigkeit für Lokalisierungsunterstützung und die Überprüfung bereits integrierter Unterstützung für die Lokalisierung im Web untersucht. Anwendungen, die vom .NET Framework bereitgestellt werden. Die Microsoft AJAX-Skript Bibliothek verwendet das RESX-Dateiformat, das bereits von .NET-Anwendungen verwendet wird, das integrierte IDE-Unterstützung und einen Share baren Ressourcentyp bereitstellt.

Dieses Whitepaper basiert auf der Beta 2-Version von Microsoft Visual Studio 2008. In diesem Whitepaper wird auch davon ausgegangen, dass Sie mit Visual Studio 2008, nicht mit Visual Web Developer Express arbeiten und Exemplarische Vorgehensweisen entsprechend der Benutzeroberfläche von Visual Studio bereitstellen. In einigen Codebeispielen werden Projektvorlagen verwendet, die in Visual Web Developer Express möglicherweise nicht verfügbar sind.

## <a name="the-need-for-localization"></a>*Der Bedarf an Lokalisierung*

Insbesondere für Entwickler von Unternehmensanwendungen und Komponenten Entwickler ist die Möglichkeit, Tools zu erstellen, die die Unterschiede zwischen Kulturen und Sprachen erkennen können, zunehmend notwendig. Das Entwerfen von Komponenten mit der Möglichkeit zur Anpassung an das Gebiets Schema des Clients steigert die Produktivität von Entwicklern und reduziert den Arbeitsaufwand, der erforderlich ist, damit eine Komponente Global funktioniert.

Lokalisierung ist der Prozess des Entwurfs und der Integration der Unterstützung für eine bestimmte Sprache und Kultur in eine Anwendung oder eine Anwendungs Komponente. Die Microsoft ASP.NET Plattform bietet umfassende Unterstützung für die Lokalisierung für standardmäßige ASP.NET-Anwendungen durch Integration des standardmäßigen .net-Lokalisierungs Modells. das Microsoft AJAX-Framework nutzt das integrierte Modell, um die verschiedenen Szenarien zu unterstützen, in denen die Lokalisierung durchgeführt werden kann. Mit dem Microsoft AJAX-Framework können Skripts entweder durch Bereitstellung in Satellitenassemblys oder durch Verwendung einer statischen Dateisystem Struktur lokalisiert werden.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Einbetten von Skripts mit Satelliten*

In Übereinstimmung mit der standardmäßigen .NET Framework Lokalisierungsstrategie können Ressourcen in Satellitenassemblys eingeschlossen werden. Satellitenassemblys bieten im Vergleich zur herkömmlichen Ressourcen Einbindung in Binärdateien mehrere Vorteile. eine beliebige Lokalisierung kann aktualisiert werden, ohne das größere Image zu aktualisieren. Weitere Lokalisierungen können einfach durch Installieren von Satellitenassemblys bereitgestellt werden. der Projektordner und Satellitenassemblys können ohne erneutes Laden der Hauptprojektassembly bereitgestellt werden. Besonders in ASP.NET-Projekten ist dies von Vorteil, da es die Menge der Systemressourcen, die von inkrementellen Updates verwendet werden, erheblich reduzieren und die Nutzung der Produktions Website minimal stören kann.

Skripts werden in Assemblys eingebettet, indem Sie in verwaltete RESX-Dateien (oder kompilierte resources-Dateien) eingefügt werden, die zur Kompilierzeit in der Assembly enthalten sind. Ihre Ressourcen werden dann mithilfe von AJAX-Lauf Zeit generierten Code über Attribute auf Assemblyebene der Skript Anwendung zur Verfügung gestellt.

*Benennungs Konventionen für eingebettete Skriptdateien*

Die Skript Verwaltung von Microsoft AJAX Framework unterstützt eine Vielzahl von Optionen zur Verwendung bei der Bereitstellung und zum Testen von Skripts, und Richtlinien werden bereitgestellt, um diese Optionen zu vereinfachen.

*So vereinfachen Sie das Debugging:*

Releaseskripts sollten den `.debug`-Qualifizierer nicht in den Dateinamen einschließen. Skripts, die für das Debuggen entworfen wurden, `.debug` sollten im Dateinamen

*So vereinfachen Sie die Lokalisierung:*

Neutrale Kultur Skripts sollten keinen Kultur Bezeichner in den Namen der Datei einschließen. Bei Skripts, die lokalisierte Ressourcen enthalten, sollte der ISO-Sprachcode im Dateinamen angegeben werden. `es-CO` steht beispielsweise für Spanisch, Columbia.

In der folgenden Tabelle werden die Benennungs Konventionen für Dateien zusammengefasst. Beispiele:

| Filename | Bedeutung |
| --- | --- |
| Skript. js | Ein Kultur neutrales Skript für die Releaseversion. |
| Skript. Debug. js | Ein Kultur neutrales Skript für die Debugversion. |
| Skript. en-US. js | Eine Version in englischer Sprache, USA Skript. |
| Script.Debug.es-Co. js | Ein "Debug-Version Spanish"-Skript (Columbia). |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Exemplarische Vorgehensweise: Erstellen eines lokalisierten, eingebetteten Skripts

*Beachten Sie: Diese exemplarische Vorgehensweise erfordert die Verwendung von Visual Studio 2008, da Visual Web Developer Express keine Projektvorlage für Klassen Bibliotheks Projekte enthält.*

1. Erstellen Sie ein neues Website Projekt mit integrierter ASP.NET-AJAX-Erweiterung. Erstellen Sie ein weiteres Projekt, ein Klassen Bibliotheksprojekt, in der Projekt Mappe mit dem Namen LocalizingResources.
2. Fügen Sie dem Projekt LocalizingResources eine JScript-Datei mit dem Namen "verifydelete. js" sowie resx-Ressourcen Dateien namens "deletionresources. resx" und "deletionresources. es. resx" hinzu. Der erste enthält Kultur neutrale Ressourcen. Letztere enthält spanischsprachige Ressourcen.
3. Fügen Sie den folgenden Code zu "verifylösch. js" hinzu:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Für Personen, die mit der JavaScript Regex-Syntax nicht vertraut sind, ist Text in einzelnen Schrägstrichen (im vorherigen Beispiel/filename/ein Beispiel) ein RegExp-Objekt. Die MSDN Library enthält eine umfassende JavaScript-Referenz, und Ressourcen für Native JavaScript-Objekte finden Sie online.

1. Fügen Sie die folgenden Ressourcen Zeichenfolgen zu deletionresources. resx hinzu: 

    **Verifydelete**: möchten Sie den Dateinamen wirklich löschen?

    **Gelöscht**: der Dateiname wurde gelöscht.

1. Fügen Sie deletionresources. es. resx die folgenden Ressourcen Zeichenfolgen hinzu: 

    **Verifydelete**: EST seguro que (quitar-Dateiname) wird nicht angezeigt?

    **Gelöscht**: Dateiname se ha quitado.
2. Fügen Sie der AssemblyInfo-Datei die folgenden Codezeilen hinzu:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Fügen Sie dem Projekt LocalizingResources Verweise auf System. Web und System. Web. Extensions hinzu.
2. Fügen Sie einen Verweis auf das Projekt LocalizingResources aus dem Website Projekt hinzu.
3. Aktualisieren Sie in der Datei default. aspx unter dem Website Projekt das ScriptManager-Steuerelement mit dem folgenden zusätzlichen Markup:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Fügen Sie in "default. aspx" an einer beliebigen Stelle auf der Seite dieses Markup ein:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Drücken Sie F5. Wenn Sie dazu aufgefordert werden, aktivieren Sie das Debugging Wenn die Seite geladen ist, klicken Sie auf die Schaltfläche Löschen. Beachten Sie, dass Sie in englischer Sprache aufgefordert werden (es sei denn, Ihr Computer ist so eingestellt, dass spanischsprachige Ressourcen standardmäßig bevorzugt werden).
2. Schließen Sie das Browserfenster, und kehren Sie zu "default. aspx" zurück. Ersetzen Sie in der @Page-Header Direktive Auto for Culture und UICulture durch es-es. Drücken Sie erneut F5, um die Webanwendung erneut im Browser zu starten. Beachten Sie diesmal, dass Sie aufgefordert werden, die Datei auf Spanisch zu löschen:

[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-localization/_static/image3.png))

[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-localization/_static/image6.png))

Beachten Sie, dass für diese exemplarische Vorgehensweise mehrere Variationen vorhanden sind. Beispielsweise können Skripts beim Laden von Seiten Programm gesteuert beim ScriptManager-Steuerelement registriert werden.

## <a name="including-a-static-script-file-structure"></a>*Einschließen einer statischen Skriptdatei Struktur*

Wenn Sie statische Skriptdateien für die Bereitstellung verwenden, verlieren Sie einige der Vorteile der Verwendung des inhärenten .net-Lokalisierungs Schemas. Hauptsächlich sichtbar ist, dass Sie den automatischen Typ verlieren, der aus dem einschließen von Skript Ressourcen Dateien generiert wurde. in der obigen exemplarischen Vorgehensweise wurden beispielsweise Ressourcen von einem automatisch generierten Typ mit dem Namen "Message" aus dem ScriptManager-Steuerelement verfügbar gemacht.

Es gibt jedoch einige Vorteile bei der Verwendung einer statischen Skriptdatei Struktur. Updates können ohne erneutes Kompilieren und erneutes Bereitstellen von Satellitenassemblys durchgeführt werden, und die Verwendung einer statischen Dateistruktur kann auch zum Überschreiben des eingebetteten Skripts erfolgen, um ein kleines Element zu integrieren, das möglicherweise nicht mit einer Komponente ausgeliefert wurde.

Microsoft empfiehlt, Probleme mit der Versionskontrolle zu vermeiden, indem die Skript Ressourcen während der Projekt Kompilierung automatisch erstellt werden. Beim Verwalten einer umfassenden Skript Codebasis kann es zunehmend schwierig werden, sicherzustellen, dass Codeänderungen in jedem lokalisierten Skript widergespiegelt werden. Als Alternative können Sie einfach ein logisches Skript und mehrere Lokalisierungs Skripts verwalten und die Dateien beim Erstellen des Projekts zusammenführen.

Da keine Ressourcen zur deklarativen Einbeziehung von vorhanden sind, muss auf statische Skriptdateien verwiesen werden, indem `<asp:ScriptElement>` Elemente als untergeordnetes Element des `<Scripts>`-Tags des ScriptManager-Steuer Elements hinzugefügt werden oder indem der `Scripts`-Eigenschaft des `ScriptManager`-Steuer Elements auf der Seite zur Laufzeit Programm gesteuert hinzu `ScriptReference` gefügt wird.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager und seine Rolle in der Lokalisierung*

Der ScriptManager ermöglicht mehrere automatische Verhalten für lokalisierte Anwendungen:

- Skriptdateien werden automatisch auf Grundlage von Einstellungen und Benennungs Konventionen lokalisiert. Beispielsweise werden debugaktivierte Skripts im Debugmodus geladen und lokalisierte Skripts basierend auf der Benutzeroberflächen Auswahl des Browsers geladen.
- Sie ermöglicht die Definition von Kulturen, einschließlich benutzerdefinierter Kulturen.
- Dadurch wird die Komprimierung von Skriptdateien über HTTP ermöglicht.
- Skripts werden zwischengespeichert, um viele Anforderungen effizient zu verwalten.
- Sie fügt Skripts eine Dereferenzierungsschicht hinzu, indem Sie Sie durch eine verschlüsselte URL weiterleiten.

Skript Verweise können dem ScriptManager-Steuerelement entweder Programm gesteuert oder durch deklaratives Markup hinzugefügt werden. Deklaratives Markup ist besonders nützlich beim Arbeiten mit Skripts, die in anderen Assemblys als dem Website Projekt selbst eingebettet sind, da der Name des Skripts wahrscheinlich nicht geändert wird, wenn Revisionen durchlaufen werden.

## <a name="summary"></a>Summary

Wenn Webanwendungen größer werden, um eine größere Zielgruppe zu erreichen, ist es erforderlich, umfassendere Kulturen und Communitys zu erreichen, die zu einem Kern für ein Geschäftsmodell werden. e-Commerce-Webanwendungen müssen in der Lage sein, mit fremden Währungen umzugehen, Inhalts Verwaltungssysteme müssen nicht nur ihren Inhalt, sondern auch Ihre Navigationshinweise und Formularfelder in anderen Sprachen präsentieren können, und Unternehmen müssen wissen, dass dies erforderlich ist. zugegriffen.

Der .NET Framework unterstützt intrinsisch ein umfassendes Lokalisierungs Framework und nutzt Satellitenassemblys und XML-Ressourcen Dateien (RESX-Dateien), um Ressourcen Zeichenfolgen und Images zu suchen. Die ASP.NET-AJAX-Erweiterungen, einschließlich des Microsoft AJAX-Frameworks und der Microsoft AJAX-Skript Bibliothek, bieten Unterstützung für dieses Programmiermodell im Client seitigen Code und ermöglichen einfache Ressourcen Zeichenfolgen-Suchvorgänge. Satellitenassemblys unterstützen die automatische Einbindung von Skript Ressourcen (tatsächliche JS-Dateien) über "ScriptResource. axd", sofern die Dateinamen einem bestimmten Benennungs Schema folgen. Mit dieser Unterstützung vereinfachen die ASP.NET AJAX-Erweiterungen die Lokalisierung von Skripts und die Globalisierung von Anwendungen.

## <a name="bio"></a>*Basierten*

Scott Cate arbeitet seit 1997 mit Microsoft-Webtechnologien und ist der Präsident von myKB.com ([www.myKB.com](http://www.myKB.com)), wo er sich darauf spezialisiert hat, ASP.NET basierte Anwendungen zu schreiben, die sich auf die Software Lösungen der Wissensdatenbank konzentrieren. Scott kann über [scott.cate@myKB.com](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com) per e-Mail kontaktiert werden.

> [!div class="step-by-step"]
> [Zurück](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Weiter](understanding-asp-net-ajax-web-services.md)
