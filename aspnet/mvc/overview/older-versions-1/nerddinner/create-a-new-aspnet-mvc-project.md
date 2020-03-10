---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Erstellen eines neuen ASP.NET MVC-Projekts | Microsoft-Dokumentation
author: microsoft
description: In Schritt 1 wird veranschaulicht, wie die grundlegende nerddinner-Anwendungs Struktur eingerichtet wird.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469239"
---
# <a name="create-a-new-aspnet-mvc-project"></a>Erstellen eines neuen ASP.NET MVC-Projekts

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 1 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> In Schritt 1 wird veranschaulicht, wie die grundlegende nerddinner-Anwendungs Struktur eingerichtet wird.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-1-file-gtnew-project"></a>Nerddinner Step 1: Datei&gt;neues Projekt

Wir beginnen mit unserer "nerddinner"-Anwendung, indem wir das Menü Element " **File-&gt;New Project** " in Visual Studio 2008 oder im kostenlosen Visual Web Developer 2008 Express auswählen.

Dadurch wird das Dialogfeld "Neues Projekt" angezeigt. Um eine neue ASP.NET MVC-Anwendung zu erstellen, wählen wir auf der linken Seite des Dialog Felds den Knoten "Web" und dann auf der rechten Seite die Projektvorlage "ASP.NET MVC-Webanwendung" aus:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Wichtig: Stellen Sie sicher, dass Sie ASP.NET MVC heruntergeladen und installiert haben. andernfalls wird es nicht im Dialogfeld "Neues Projekt" angezeigt. Sie können v2 des [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) verwenden, wenn Sie es noch nicht installiert haben (ASP.NET MVC ist im Abschnitt "Webplattform-&gt;Frameworks und Laufzeiten" verfügbar).*

Wir nennen das neue Projekt, das wir erstellen, und klicken dann auf die Schaltfläche "OK", um es zu erstellen.

Wenn wir auf "OK" klicken, öffnet Visual Studio ein zusätzliches Dialogfeld, in dem Sie aufgefordert werden, optional auch ein Komponenten Testprojekt für die neue Anwendung zu erstellen. Mit diesem Komponenten Testprojekt können wir automatisierte Tests erstellen, mit denen die Funktionalität und das Verhalten der Anwendung überprüft werden (was wir später in diesem Tutorial behandeln werden).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Die Dropdown Liste "Test Framework" im obigen Dialogfeld enthält alle verfügbaren ASP.NET MVC-Komponenten Test-Projektvorlagen, die auf dem Computer installiert sind. Versionen können für nunit, MbUnit und xUnit heruntergeladen werden. Das integrierte Visual Studio-Komponenten Test-Framework wird ebenfalls unterstützt.

*Hinweis: das Visual Studio-Komponenten Test-Framework ist nur in Visual Studio 2008 Professional und höheren Versionen verfügbar. Wenn Sie vs 2008 Standard Edition oder Visual Web Developer 2008 Express verwenden, müssen Sie die nunit-, MbUnit-oder xUnit-Erweiterungen für ASP.NET MVC herunterladen und installieren, damit dieses Dialogfeld angezeigt wird. Wenn keine Test-Frameworks installiert sind, wird das Dialogfeld nicht angezeigt.*

Wir verwenden den Standardnamen "nerddinner. Tests" für das Testprojekt, das wir erstellen, und verwenden die Framework-Option "Visual Studio-Komponenten Test". Wenn wir auf die Schaltfläche "OK" klicken, erstellt Visual Studio eine Projekt Mappe mit zwei Projekten, eine für die Webanwendung und eine für die Komponententests:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Untersuchen der nerddinner Directory-Struktur

Wenn Sie eine neue ASP.NET MVC-Anwendung mit Visual Studio erstellen, werden dem Projekt automatisch eine Reihe von Dateien und Verzeichnissen hinzugefügt:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

ASP.NET MVC-Projekte verfügen standardmäßig über sechs Verzeichnisse der obersten Ebene:

| **Verzeichnis** | **Zweck** |
| --- | --- |
| **/Controllers** | Platzieren von Controller Klassen, die URL-Anforderungen verarbeiten |
| **/Models** | Wo Sie Klassen platzieren, die Daten darstellen und bearbeiten |
| **/Views** | Wo Sie Benutzeroberflächen-Vorlagen Dateien platzieren, die für das Rendern der Ausgabe verantwortlich sind |
| **"/Scripts"** | Wo Sie JavaScript-Bibliotheksdateien und-Skripts (. js) einfügen |
| **/Content** | Wo Sie CSS-und Bilddateien und andere nicht dynamische/nicht-JavaScript-Inhalte einfügen |
| **/App\_Daten** | Wo Sie Datendateien speichern, die Sie lesen/schreiben möchten. |

ASP.NET MVC erfordert diese Struktur nicht. Entwickler, die an großen Anwendungen arbeiten, partitionieren die Anwendung in der Regel über mehrere Projekte hinweg, um Sie besser verwalten zu können (z. b. werden Datenmodell Klassen häufig in einem separaten Klassen Bibliotheksprojekt von der Webanwendung verwendet). Die Standard Projektstruktur bietet allerdings eine gute Standardverzeichnis Konvention, die wir verwenden können, um zu verhindern, dass die Anwendungsprobleme bereinigt werden.

Wenn wir das Verzeichnis "/Controllers" erweitern, stellen wir fest, dass Visual Studio dem Projekt standardmäßig zwei Controller Klassen – HomeController und AccountController – hinzugefügt hat:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Wenn das Verzeichnis/views erweitert wird, werden drei Unterverzeichnisse –/Home,/Account und/Shared – sowie mehrere darin enthaltenen Vorlagen Dateien werden standardmäßig dem Projekt hinzugefügt:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Wenn Sie die Verzeichnisse/Content und "/Scripts" erweitern, finden Sie eine Site. CSS-Datei, mit der alle HTML-Dateien auf der Website formatiert werden, sowie JavaScript-Bibliotheken, die die Unterstützung von ASP.NET AJAX und jQuery innerhalb der Anwendung ermöglichen:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Wenn wir das Projekt "nerddinner. Tests" erweitern, finden Sie zwei Klassen, die Komponenten Tests für unsere Controller Klassen enthalten:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Diese Standard Dateien, die von Visual Studio hinzugefügt werden, bieten uns eine grundlegende Struktur für eine funktionierende Anwendung: vervollständigen mit Homepage, Info Seite, Kontoanmeldung/Abmelde-/Registrierungsseiten und eine nicht behandelte Fehlerseite (alle zusammen und sind standardmäßig Funktions bereit).

### <a name="running-the-nerddinner-application"></a>Ausführen der "nerddinner"-Anwendung

Sie können das Projekt ausführen, indem Sie entweder die Menü Elemente **Debuggen&gt;Debuggen starten** oder **Debuggen&gt;starten ohne Debugging** auswählen:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Dadurch wird der integrierte ASP.net Web-Server gestartet, der in Visual Studio verfügbar ist, und die Anwendung wird ausgeführt:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Im folgenden finden Sie die Startseite für das neue Projekt (URL: "/"), wenn es ausgeführt wird:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Wenn Sie auf die Registerkarte "Info" klicken, wird eine Info-Seite angezeigt (URL: "/Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Wenn Sie oben rechts auf den Link "Anmelden" klicken, gelangen Sie zu einer Anmeldeseite (URL: "/Account/LOGON").

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Wenn kein Anmelde Konto vorhanden ist, können Sie auf den Link registrieren (URL: "/Account/Register") klicken, um einen Link zu erstellen:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Der Code zum Implementieren der oben genannten Funktionen "Home", "about" und "Logout/Register" wurde beim Erstellen des neuen Projekts standardmäßig hinzugefügt. Wir verwenden diese als Ausgangspunkt unserer Anwendung.

### <a name="testing-the-nerddinner-application"></a>Testen der "nerddinner"-Anwendung

Wenn wir die Professional Edition oder eine höhere Version von Visual Studio 2008 verwenden, können wir die integrierte IDE-Unterstützung für Komponententests in Visual Studio verwenden, um das Projekt zu testen:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Wenn Sie eine der oben genannten Optionen auswählen, wird der Bereich "Testergebnisse" in der IDE geöffnet, und wir erhalten den Status "erfolgreich" und "Fehler" für die 27 Komponententests, die in unserem neuen Projekt enthalten sind und die integrierten Funktionen abdecken:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Später in diesem Tutorial erfahren Sie mehr über automatisierte Tests und zusätzliche Komponententests, die die von uns implementierten Anwendungs Funktionalität abdecken.

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt eine grundlegende Anwendungs Struktur eingerichtet. Nun erstellen wir [eine Datenbank, in der die Anwendungsdaten gespeichert](create-a-database.md)werden.

> [!div class="step-by-step"]
> [Zurück](introducing-the-nerddinner-tutorial.md)
> [Weiter](create-a-database.md)
